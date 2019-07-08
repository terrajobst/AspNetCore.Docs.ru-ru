---
title: Использовать проверку подлинности файлов cookie без ASP.NET Core Identity
author: rick-anderson
description: Узнайте, как использовать проверку подлинности файлов cookie без ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622746"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Использовать проверку подлинности файлов cookie без ASP.NET Core Identity

По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)

Удостоверение ASP.NET Core — это поставщик проверки подлинности завершено, полнофункциональную для создания и обслуживания имена входа. Тем не менее можно использовать проверку подлинности поставщик проверки подлинности на основе файлов cookie без ASP.NET Core Identity. Дополнительные сведения см. в разделе <xref:security/authentication/identity>.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Для демонстрационных целей в примере приложения учетной записи пользователя для гипотетической пользователя Родригез Мария встроен в приложение. Используйте **электронной почты** имя пользователя `maria.rodriguez@contoso.com` и любой пароль для входа пользователя. Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла. В реальный пример пользователь может пройти проверку подлинности в базе данных.

## <a name="configuration"></a>Параметр Configuration

Если приложение не использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета.

В `Startup.ConfigureServices` метод, создайте службу проверки подлинности по промежуточного слоя с <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> и <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> методов:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> передаваемый `AddAuthentication` задает схему проверки подлинности по умолчанию для приложения. `AuthenticationScheme` полезно, если имеется несколько экземпляров файла cookie проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme). Установка `AuthenticationScheme` для [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) предоставляет значение файлов «cookie» для схемы. Вы можете указать любое строковое значение, являющийся отличительным признаком схемы.

Схема проверки подлинности приложения отличается от схемы проверки подлинности файла cookie для приложения. Когда схему проверки подлинности файла cookie не предоставляется для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, он использует `CookieAuthenticationDefaults.AuthenticationScheme` («файлы cookie»).

Файл cookie проверки подлинности <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> свойству `true` по умолчанию. Файлы cookie проверки подлинности разрешены, если посетитель не означает согласие на сбор данных. Дополнительные сведения см. в разделе <xref:security/gdpr#essential-cookies>.

В `Startup.Configure` мы вызываем метод `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, который задает `HttpContext.User` свойства. Вызовите `UseAuthentication` метод перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Класс используется для настройки параметров поставщика проверки подлинности.

Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в `Startup.ConfigureServices` метод:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>По промежуточного слоя для файлов cookie политики

[По промежуточного слоя для файлов cookie политики](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) включает возможности политики файлов cookie. Добавление по промежуточного слоя в конвейер обработки приложений является порядок конфиденциальных&mdash;оно влияет только на нижестоящим компонентам, зарегистрированному в конвейере.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Используйте <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> для промежуточного слоя политики для управления общих характеристик обработки файлов cookie и обработчик в обработчики обработки файлов cookie, если файлы cookie добавляется или удаляется.

Значение по умолчанию <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> значение `SameSiteMode.Lax` для разрешения проверки подлинности OAuth2. Для строго ограничивает политику веб-сайте `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`. Несмотря на то, что этот параметр приводит к сбою OAuth2 и других схем проверки подлинности независимо от источника, оно повышает уровень безопасности cookie для других типов приложений, которые не полагаются на обработку независимо от источника запроса.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Параметр политики промежуточного слоя для `MinimumSameSitePolicy` может повлиять на значение `Cookie.SameSite` в `CookieAuthenticationOptions` параметры в соответствии с в таблице ниже.

| MinimumSameSitePolicy | Cookie.SameSite | Результирующий параметр Cookie.SameSite |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Создайте файл cookie проверки подлинности

Чтобы создать файл cookie, содержащий сведения о пользователе, сконструировать <xref:System.Security.Claims.ClaimsPrincipal>. Сведения о пользователе сериализуется и сохраняется в файле cookie. 

Создание <xref:System.Security.Claims.ClaimsIdentity> с любыми обязательными <xref:System.Security.Claims.Claim>s и вызвать метод <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> для входа пользователя:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ. Если `AuthenticationScheme` не указано, используется схема по умолчанию.

ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection) системы используется для шифрования. Для приложения, размещенного на нескольких компьютерах, загрузить балансировку между приложениями, или с помощью веб-фермы, [настроить защиту данных](xref:security/data-protection/configuration/overview) использование одного набора ключей и идентификатор приложения.

## <a name="sign-out"></a>Выйти

Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Если `CookieAuthenticationDefaults.AuthenticationScheme` (или файлы «cookie») не используется как схемы (например, «ContosoCookie»), укажите схему, используемую при настройке поставщика проверки подлинности. В противном случае используется схема по умолчанию.

## <a name="react-to-back-end-changes"></a>Реагировать на изменения серверной части

Создав файл cookie, файл cookie является единственным источником удостоверения. Если учетная запись пользователя отключена в обслуживающих систем:

* В системе приложения-файла cookie проверки подлинности продолжает обрабатывать запросы на основании файла cookie проверки подлинности.
* Пользователь остается в приложение до тех пор, пока файл cookie проверки подлинности является допустимым.

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Событий можно использовать для перехвата и переопределение проверки подлинности файла cookie. Проверка файла cookie при каждом запросе уменьшает риск появления отозванных пользователей, обращающихся к приложению.

Один из способов проверки cookie основан на слежении за при изменениях базы данных пользователя. Если базы данных не был изменен с момента выпуска файла cookie пользователя, нет необходимости пройти повторную аутентификацию пользователя, если их куки-файл все еще действителен. В примере приложения базы данных была реализована в `IUserRepository` и сохраняет `LastChanged` значение. При обновлении пользователя в базе данных, `LastChanged` имеет значение текущего времени.

Чтобы сделать недействительным файл cookie, когда база данных изменяется в зависимости от `LastChanged` создайте файл cookie с `LastChanged` утверждение, содержащее текущий `LastChanged` значение из базы данных:

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

Для реализации переопределения для `ValidatePrincipal` события, написание метод со следующей сигнатурой в классе, который является производным от <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Ниже приведен пример реализации `CookieAuthenticationEvents`:

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Зарегистрируйте экземпляр события во время регистрации службы файл cookie в `Startup.ConfigureServices` метод. Укажите [областью действия регистрации службы](xref:fundamentals/dependency-injection#service-lifetimes) для вашей `CustomCookieAuthenticationEvents` класса:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Рассмотрим ситуацию, в котором имя пользователя будет обновлено&mdash;решение, которое не влияет на безопасность любым способом. Если вы хотите безболезненно обновить участника-пользователя, вызовите `context.ReplacePrincipal` и задайте `context.ShouldRenew` свойства `true`.

> [!WARNING]
> Подход, описанный здесь активируется при каждом запросе. Проверка файлов cookie проверки подлинности для всех пользователей при каждом запросе может привести к снижению производительности для приложения.

## <a name="persistent-cookies"></a>Постоянные файлы cookie

Требуется файл cookie для сохранения между сеансами браузера. Такое сохранение должны включаться только с согласия пользователя с помощью флажка «Запомнить мои» при входе или похожий механизм. 

Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выдерживает его через браузер замыкания. Учитываются параметры скользящего срока действия ранее была настроена. Если срок действия файла cookie, при закрытии браузера, браузер удаляет файл cookie после перезагрузки.

Задайте <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> для `true` в <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>Срок действия файла cookie абсолютный

Можно задать абсолютный срок действия <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Чтобы создать постоянный файл cookie, `IsPersistent` также должно быть задано. В противном случае файл cookie создается с временем жизни на основе сеансов и срок действия может истечь до или после билет проверки подлинности, который его содержит. Когда `ExpiresUtc` не установлен, этот параметр переопределяет значение <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> параметр <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, если задать.

Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выполняется в течение 20 минут. Это не учитывает параметры скользящего срока действия ранее была настроена.

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Проверок роли на основе политик](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
