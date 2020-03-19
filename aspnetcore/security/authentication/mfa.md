---
title: Многофакторная идентификация в ASP.NET Core
author: damienbod
description: Узнайте, как настроить многофакторную проверку подлинности (MFA) в ASP.NET Core приложении.
monikerRange: '>= aspnetcore-3.1'
ms.author: rick-anderson
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Identity
uid: security/authentication/mfa
ms.openlocfilehash: 6220688d53f0718ca5be5f63dd5d9539d37e2391
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2020
ms.locfileid: "79520149"
---
# <a name="multi-factor-authentication-in-aspnet-core"></a>Многофакторная идентификация в ASP.NET Core

По [(Damien Бауден](https://github.com/damienbod)

Многофакторная идентификация (MFA) — это процесс, в ходе которого пользователь запрашивается во время события входа, чтобы получить дополнительные формы идентификации. Этот запрос может быть использован для ввода кода из целлфоне, использования ключа FIDO2 или для обеспечения сканирования отпечатков пальцев. Если требуется Вторая форма проверки подлинности, Безопасность улучшена. Злоумышленник не может легко получить или продублировать дополнительный фактор.

В этой статье рассматриваются следующие вопросы:

* Что такое MFA и какие потоки MFA рекомендуются
* Настройка MFA для страниц администрирования с помощью ASP.NET Core Identity
* Отправка требования к входу MFA в OpenID Connect Connect Server
* Принудительное ASP.NET Core OpenID Connect Connect Client для требования MFA

## <a name="mfa-2fa"></a>MFA, 2FA

MFA требует по крайней мере два типа цветопробы для удостоверения, такого как известное, что вы владеете или биометрической проверки подлинности пользователя.

Двухфакторная проверка подлинности (2FA) похожа на подмножество MFA, но в этом случае в MFA может потребоваться два или более фактора для подтверждения личности.

### <a name="mfa-totp-time-based-one-time-password-algorithm"></a>MFA TOTP (алгоритм одноразового пароля на основе времени)

MFA с использованием TOTP является поддерживаемой реализацией с помощью ASP.NET Core Identity. Это можно использовать вместе со всеми совместимыми приложениями проверки подлинности, в том числе:

* Приложение Microsoft Authenticator
* Приложение Google Authenticator

Сведения о реализации см. по следующей ссылке:

[Включение создания QR-кода для приложений TOTP Authenticator в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)

### <a name="mfa-fido2-or-passwordless"></a>FIDO2 MFA или безпарольный

FIDO2 в настоящее время:

* Самый безопасный способ достижения MFA.
* Единственный поток MFA, защищающий от фишинговых атак.

В настоящее время ASP.NET Core не поддерживает FIDO2 напрямую. FIDO2 можно использовать для MFA или беспарольных потоков.

Azure Active Directory обеспечивает поддержку для FIDO2 и безпарольных потоков. Дополнительные сведения см. в разделе [Параметры проверки подлинности](/azure/active-directory/authentication/concept-authentication-passwordless), не имеющие пароля, для Azure Active Directory.

### <a name="mfa-sms"></a>SMS ДЛЯ MFA

MFA с SMS значительно повышает безопасность с помощью проверки пароля (один фактор). Однако использовать SMS в качестве второго фактора больше не рекомендуется. Для этого типа реализации существует слишком много известных векторов атак.

[Рекомендации NIST](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="configure-mfa-for-administration-pages-using-aspnet-core-opno-locidentity"></a>Настройка MFA для страниц администрирования с помощью ASP.NET Core Identity

MFA можно принудительно заставить пользователей получать доступ к конфиденциальным страницам в приложении ASP.NET Core Identity. Это может быть полезно для приложений, где существуют разные уровни доступа для разных удостоверений. Например, пользователи могут просматривать данные профиля с помощью имени входа с паролем, но для доступа к административным страницам администратору потребуется использовать MFA.

### <a name="extend-the-login-with-an-mfa-claim"></a>Расширение имени входа с помощью утверждения MFA

Демонстрационный код — это программа установки, использующая ASP.NET Core с Identity и Razor Pages. Вместо `AddDefaultIdentity` одного `AddIdentity` используется метод, поэтому `IUserClaimsPrincipalFactory` можно использовать для добавления утверждений в удостоверение после успешного входа.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));
    
    services.AddIdentity<IdentityUser, IdentityRole>(
            options => options.SignIn.RequireConfirmedAccount = false)
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddSingleton<IEmailSender, EmailSender>();
    services.AddScoped<IUserClaimsPrincipalFactory<IdentityUser>, 
        AdditionalUserClaimsPrincipalFactory>();

    services.AddAuthorization(options =>
        options.AddPolicy("TwoFactorEnabled",
            x => x.RequireClaim("amr", "mfa")));

    services.AddRazorPages();
}
```

Класс `AdditionalUserClaimsPrincipalFactory` добавляет утверждение `amr` в утверждения пользователя только после успешного входа. Значение утверждения считывается из базы данных. Утверждение добавляется здесь, так как пользователь должен получить доступ к более защищенному представлению, если удостоверение было зарегистрировано с помощью MFA. Если представление базы данных считывается непосредственно из базы данных, а не с помощью утверждения, можно получить доступ к представлению без MFA непосредственно после активации MFA.

```csharp
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.Options;
using System.Collections.Generic;
using System.Security.Claims;
using System.Threading.Tasks;

namespace IdentityStandaloneMfa
{
    public class AdditionalUserClaimsPrincipalFactory : 
        UserClaimsPrincipalFactory<IdentityUser, IdentityRole>
    {
        public AdditionalUserClaimsPrincipalFactory( 
            UserManager<IdentityUser> userManager,
            RoleManager<IdentityRole> roleManager, 
            IOptions<IdentityOptions> optionsAccessor) 
            : base(userManager, roleManager, optionsAccessor)
        {
        }

        public async override Task<ClaimsPrincipal> CreateAsync(IdentityUser user)
        {
            var principal = await base.CreateAsync(user);
            var identity = (ClaimsIdentity)principal.Identity;

            var claims = new List<Claim>();

            if (user.TwoFactorEnabled)
            {
                claims.Add(new Claim("amr", "mfa"));
            }
            else
            {
                claims.Add(new Claim("amr", "pwd"));
            }

            identity.AddClaims(claims);
            return principal;
        }
    }
}
```

Поскольку установка службы Identity изменилась в классе `Startup`, необходимо обновить макеты Identity. Формирование шаблонов Identity страниц в приложении. Определите макет в файле *Identity/аккаунт/манаже/_layout. cshtml* .

```cshtml
@{
    Layout = "/Pages/Shared/_Layout.cshtml";
}
```

Также назначайте макет для всех страниц управления со страницы Identity:

```cshtml
@{
    Layout = "_Layout.cshtml";
}
```

### <a name="validate-the-mfa-requirement-in-the-administration-page"></a>Проверка требования MFA на странице "Администрирование"

На странице "Администрирование" Razor проверяется, что пользователь выполнил вход с помощью MFA. В методе `OnGet` удостоверение используется для доступа к утверждениям пользователя. Утверждение `amr` проверяется на `mfa`значения. Если в удостоверении отсутствует это утверждение или это `false`, страница переправляется на страницу включение MFA. Это возможно, так как пользователь уже вошел в систему, но без MFA.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace IdentityStandaloneMfa
{
    public class AdminModel : PageModel
    {
        public IActionResult OnGet()
        {
            var claimTwoFactorEnabled = 
                User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (claimTwoFactorEnabled != null && 
                "mfa".Equals(claimTwoFactorEnabled.Value))
            {
                // You logged in with MFA, do the administrative stuff
            }
            else
            {
                return Redirect(
                    "/Identity/Account/Manage/TwoFactorAuthentication");
            }

            return Page();
        }
    }
}
```

### <a name="ui-logic-to-toggle-user-login-information"></a>Логика пользовательского интерфейса для переключения сведений для входа пользователя

Политика авторизации была добавлена при запуске. Для политики требуется утверждение `amr` со значением `mfa`.

```csharp
services.AddAuthorization(options =>
    options.AddPolicy("TwoFactorEnabled",
        x => x.RequireClaim("amr", "mfa")));
```

Затем эту политику можно использовать в представлении `_Layout`, чтобы показать или скрыть меню **администрирования** с предупреждением:

```cshtml
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Identity
@inject SignInManager<IdentityUser> SignInManager
@inject UserManager<IdentityUser> UserManager
@inject IAuthorizationService AuthorizationService
```

Если удостоверение было зарегистрировано с помощью MFA, меню **Admin** отображается без предупреждения о подсказке. Когда пользователь вошел в систему без MFA, меню **Администратор (не включено)** отображается вместе с подсказкой, которая информирует пользователя (объяснение предупреждения).

```cshtml
@if (SignInManager.IsSignedIn(User))
{
    @if ((AuthorizationService.AuthorizeAsync(User, "TwoFactorEnabled")).Result.Succeeded)
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin">Admin</a>
        </li>
    }
    else
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin" 
               id="tooltip-demo"  
               data-toggle="tooltip" 
               data-placement="bottom" 
               title="MFA is NOT enabled. This is required for the Admin Page. If you have activated MFA, then logout, login again.">
                Admin (Not Enabled)
            </a>
        </li>
    }
}
```

Если пользователь входит в систему без MFA, отображается предупреждение:

![Проверка подлинности администратора MFA](mfa/_static/identitystandalonemfa_01.png)

Пользователь перенаправляется в представление включения MFA при щелчке ссылки **администратора** :

![Администратор активирует аутентификацию MFA](mfa/_static/identitystandalonemfa_02.png)

## <a name="send-mfa-sign-in-requirement-to-openid-connect-server"></a>Отправка требования к входу MFA в OpenID Connect Connect Server 

Параметр `acr_values` можно использовать для передачи `mfa` требуемого значения от клиента серверу в запросе проверки подлинности.

> [!NOTE]
> Чтобы это работало, необходимо обработать параметр `acr_values` на сервере Open ID Connect.

### <a name="openid-connect-aspnet-core-client"></a>OpenID Connect подключение клиента ASP.NET Core

Клиентское приложение ASP.NET Core Razor Pages Open ID Connect использует метод `AddOpenIdConnect` для входа на сервер Open ID Connect. Параметр `acr_values` задается со значением `mfa` и отправляется с запросом проверки подлинности. `OpenIdConnectEvents` используется для добавления этого.

Рекомендуемые значения параметров `acr_values` см. в разделе [Ссылочные значения метода проверки подлинности](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08).

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "<OpenID Connect server URL>";
        options.RequireHttpsMetadata = true;
        options.ClientId = "<OpenID Connect client ID>";
        options.ClientSecret = "<>";
        // Code with PKCE can also be used here
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
        options.Events = new OpenIdConnectEvents
        {
            OnRedirectToIdentityProvider = context =>
            {
                context.ProtocolMessage.SetParameter("acr_values", "mfa");
                return Task.FromResult(0);
            }
        };
    });
```

### <a name="example-openid-connect-identityserver-4-server-with-aspnet-core-opno-locidentity"></a>Пример OpenID Connect Connect IdentityServer 4 Server with ASP.NET Core Identity

На сервере OpenID Connect Connect, который реализуется с помощью ASP.NET Core Identity с представлениями MVC, создается новое представление с именем *ErrorEnable2FA. cshtml* . Представление:

* Отображается, если Identity поступает из приложения, которое требует MFA, но пользователь не активировал его в Identity.
* Информирует пользователя и добавляет ссылку для активации.

```cshtml
@{
    ViewData["Title"] = "ErrorEnable2FA";
}

<h1>The client application requires you to have MFA enabled. Enable this, try login again.</h1>

<br />

You can enable MFA to login here:

<br />

<a asp-controller="Manage" asp-action="TwoFactorAuthentication">Enable MFA</a>
```

В методе `Login` реализация интерфейса `IIdentityServerInteractionService` `_interaction` используется для доступа к параметрам запроса Connect Open ID. Доступ к параметру `acr_values` осуществляется с помощью свойства `AcrValues`. После того как клиент отправил это с помощью набора `mfa`, этот флажок можно проверить.

Если требуется MFA и для пользователя в ASP.NET Core Identity включена поддержка MFA, то имя входа продолжится. Если для пользователя не включено MFA, пользователь перенаправляется к пользовательскому представлению *ErrorEnable2FA. cshtml*. Затем ASP.NET Core Identity подписывает пользователя в.

```csharp
//
// POST: /Account/Login
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Login(LoginInputModel model)
{
    var returnUrl = model.ReturnUrl;
    var context = 
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa = 
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    var user = await _userManager.FindByNameAsync(model.Email);
    if (user != null && !user.TwoFactorEnabled && requires2Fa)
    {
        return RedirectToAction(nameof(ErrorEnable2FA));
    }

    // code omitted for brevity
```

Метод `ExternalLoginCallback` работает как имя входа локального Identity. Для значения `mfa` проверяется свойство `AcrValues`. Если указано значение `mfa`, MFA выполняется перед завершением имени входа (например, перенаправляется в представление `ErrorEnable2FA`).

```csharp
//
// GET: /Account/ExternalLoginCallback
[HttpGet]
[AllowAnonymous]
public async Task<IActionResult> ExternalLoginCallback(
    string returnUrl = null,
    string remoteError = null)
{
    var context =
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa =
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    if (remoteError != null)
    {
        ModelState.AddModelError(
            string.Empty,
            _sharedLocalizer["EXTERNAL_PROVIDER_ERROR", 
            remoteError]);
        return View(nameof(Login));
    }
    var info = await _signInManager.GetExternalLoginInfoAsync();

    if (info == null)
    {
        return RedirectToAction(nameof(Login));
    }

    var email = info.Principal.FindFirstValue(ClaimTypes.Email);

    if (!string.IsNullOrEmpty(email))
    {
        var user = await _userManager.FindByNameAsync(email);
        if (user != null && !user.TwoFactorEnabled && requires2Fa)
        {
            return RedirectToAction(nameof(ErrorEnable2FA));
        }
    }

    // Sign in the user with this external login provider if the user already has a login.
    var result = await _signInManager
        .ExternalLoginSignInAsync(
            info.LoginProvider, 
            info.ProviderKey, 
            isPersistent: 
            false);

    // code omitted for brevity
```

Если пользователь уже вошел в систему, клиентское приложение:

* По-прежнему проверяет утверждение `amr`.
* Можно настроить MFA со ссылкой на представление ASP.NET Core Identity.

![acr_values-1](mfa/_static/acr_values-1.png)

## <a name="force-aspnet-core-openid-connect-client-to-require-mfa"></a>Принудительное ASP.NET Core OpenID Connect Connect Client для требования MFA

В этом примере показано, как ASP.NET Core приложение страницы Razor, использующее OpenID Connect Connect для входа, может потребовать, чтобы пользователи прошли проверку подлинности с помощью MFA.

Для проверки требования MFA создается `IAuthorizationRequirement` требование. Это будет добавлено на страницы с помощью политики, требующей MFA.

```csharp
using Microsoft.AspNetCore.Authorization;
 
namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfa : IAuthorizationRequirement{}
}
```

Реализуется `AuthorizationHandler`, который будет использовать утверждение `amr` и проверить значение `mfa`. `amr` возвращается в `id_token` успешной проверки подлинности и может иметь множество различных значений, как определено в спецификации [ссылочных значений метода проверки подлинности](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) .

Возвращаемое значение зависит от того, как удостоверение прошло проверку подлинности и в реализации сервера Open ID Connect.

`AuthorizationHandler` использует `RequireMfa` требование и проверяет утверждение `amr`. Сервер OpenID Connect Connect можно реализовать с помощью IdentityServer4 с ASP.NET Core Identity. Когда пользователь входит в систему с помощью TOTP, возвращается утверждение `amr` с помощью значения MFA. При использовании другой реализации сервера OpenID Connect Connect или другого типа MFA утверждение `amr` будет иметь другое значение. Код необходимо расширить, чтобы принять его.

```csharp
using Microsoft.AspNetCore.Authorization;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfaHandler : AuthorizationHandler<RequireMfa>
    {
        protected override Task HandleRequirementAsync(
            AuthorizationHandlerContext context, 
            RequireMfa requirement)
        {
            if (context == null)
                throw new ArgumentNullException(nameof(context));
            if (requirement == null)
                throw new ArgumentNullException(nameof(requirement));

            var amrClaim =
                context.User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (amrClaim != null && amrClaim.Value == Amr.Mfa)
            {
                context.Succeed(requirement);
            }

            return Task.CompletedTask;
        }
    }
}
```

В методе `Startup.ConfigureServices` в качестве схемы запроса по умолчанию используется метод `AddOpenIdConnect`. Обработчик авторизации, который используется для проверки `amr` утверждения, добавляется к инверсию контейнера управления. После этого создается политика, которая добавляет требование `RequireMfa`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.ConfigureApplicationCookie(options =>
        options.Cookie.SecurePolicy =
            CookieSecurePolicy.Always);

    services.AddSingleton<IAuthorizationHandler, RequireMfaHandler>();

    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "https://localhost:44352";
        options.RequireHttpsMetadata = true;
        options.ClientId = "AspNetCoreRequireMfaOidc";
        options.ClientSecret = "AspNetCoreRequireMfaOidcSecret";
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
    });

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireMfa", policyIsAdminRequirement =>
        {
            policyIsAdminRequirement.Requirements.Add(new RequireMfa());
        });
    });

    services.AddRazorPages();
}
```

Эта политика затем используется на странице Razor по мере необходимости. Кроме того, политику можно добавить глобально для всего приложения.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Microsoft.Extensions.Logging;

namespace AspNetCoreRequireMfaOidc.Pages
{
    [Authorize(Policy= "RequireMfa")]
    public class IndexModel : PageModel
    {
        private readonly ILogger<IndexModel> _logger;

        public IndexModel(ILogger<IndexModel> logger)
        {
            _logger = logger;
        }

        public void OnGet()
        {
        }
    }
}
```

Если пользователь проходит проверку подлинности без MFA, то `amr` утверждение, вероятно, будет иметь `pwd` значение. Запрос не будет иметь разрешения на доступ к странице. При использовании значений по умолчанию пользователь будет перенаправлен на страницу *Account/AccessDenied* . Это поведение можно изменить, или можно реализовать собственную пользовательскую логику здесь. В этом примере добавляется ссылка, чтобы допустимый пользователь мог настроить MFA для своей учетной записи.

```cshtml
@page
@model AspNetCoreRequireMfaOidc.AccessDeniedModel
@{
    ViewData["Title"] = "AccessDenied";
    Layout = "~/Pages/Shared/_Layout.cshtml";
}

<h1>AccessDenied</h1>

You require MFA to login here

<a href="https://localhost:44352/Manage/TwoFactorAuthentication">Enable MFA</a>
```

Теперь доступ к странице или веб-сайту можно получить только пользователям, которые проходят проверку подлинности с помощью MFA. Если используются разные типы MFA или 2FA, то утверждение `amr` будет иметь разные значения и должно обрабатываться правильно. Другие серверы подключения с открытым кодом также возвращают разные значения для этого утверждения и могут не следовать спецификации [ссылочных значений метода проверки подлинности](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) .

При входе без MFA (например, при использовании только пароля):

* `amr` имеет `pwd` значение:

    ![require_mfa_oidc_02. png](mfa/_static/require_mfa_oidc_02.png)

* Отказано в доступе:

    ![require_mfa_oidc_03. png](mfa/_static/require_mfa_oidc_03.png)

Также можно войти в систему с помощью OTP с Identity:

![require_mfa_oidc_01. png](mfa/_static/require_mfa_oidc_01.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Включение создания QR-кода для приложений TOTP Authenticator в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)
* [Параметры проверки подлинности с паролем для Azure Active Directory](/azure/active-directory/authentication/concept-authentication-passwordless)
* [Библиотека FIDO2 .NET для FIDO2/WebAuthn аттестации и утверждения с помощью .NET](https://github.com/abergs/fido2-net-lib)
* [WebAuthn Awesome](https://github.com/herrjemand/awesome-webauthn)
