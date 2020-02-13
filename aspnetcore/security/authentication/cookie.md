---
title: Использовать проверку подлинности файлов cookie без удостоверения ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать проверку подлинности файлов cookie без удостоверения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: 62a3d247dade6c83156a8378407d5e3891713fd1
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172119"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Использовать проверку подлинности файлов cookie без удостоверения ASP.NET Core

[Рик Андерсон (](https://twitter.com/RickAndMSFT) и [Люк ЛаСаМ](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core удостоверение — это полнофункциональный поставщик проверки подлинности для создания и обслуживания имен входа. Однако можно использовать поставщик проверки подлинности на основе файлов cookie без ASP.NET Core удостоверения. Дополнительные сведения см. в разделе <xref:security/authentication/identity>.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))

В демонстрационных целях в примере приложения учетная запись пользователя для гипотетического пользователя, Мария Rodriguez, жестко закодирована в приложении. Используйте адрес **электронной почты** `maria.rodriguez@contoso.com` и любой пароль для входа пользователя. Пользователь прошел проверку подлинности в методе `AuthenticateUser` в файле *pages/Account/Login. cshtml. CS* . В реальном примере пользователь будет проходить проверку подлинности в базе данных.

## <a name="configuration"></a>Конфигурация

Если приложение не использует [метапакет Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для пакета [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .

В методе `Startup.ConfigureServices` создайте службы промежуточного слоя проверки подлинности с помощью методов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> и <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>, передаваемые в `AddAuthentication`, задает схему проверки подлинности по умолчанию для приложения. `AuthenticationScheme` полезна при наличии нескольких экземпляров проверки подлинности файлов cookie и необходимости [авторизации с определенной схемой](xref:security/authorization/limitingidentitybyscheme). Установка `AuthenticationScheme` [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) предоставляет значение "cookies" для схемы. Можно указать любое строковое значение, которое различает схему.

Схема проверки подлинности приложения отличается от схемы проверки подлинности файлов cookie приложения. Если для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>не указана схема проверки подлинности файлов cookie, она использует `CookieAuthenticationDefaults.AuthenticationScheme` ("cookies").

Для свойства <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> файла cookie проверки подлинности по умолчанию установлено значение `true`. Файлы cookie проверки подлинности разрешены, когда посетитель сайта не был передан в сбор данных. Дополнительные сведения см. в разделе <xref:security/gdpr#essential-cookies>.

В `Startup.Configure`вызовите метод `UseAuthentication` и `UseAuthorization`, чтобы задать свойство `HttpContext.User` и запустить по промежуточного слоя авторизации для запросов. Перед вызовом `UseEndpoints`вызовите методы `UseAuthentication` и `UseAuthorization`:

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

Класс <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> используется для настройки параметров поставщика проверки подлинности.

Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в методе `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>По промежуточного слоя политики файлов cookie

По [промежуточного слоя политики файлов cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) включает возможности политики файлов cookie. Добавление по промежуточного слоя к конвейеру обработки приложений выполняется с учетом порядка&mdash;он влияет только на нисходящие компоненты, зарегистрированные в конвейере.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Используйте <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>, предоставляемые по промежуточного слоя политики cookie для управления глобальными характеристиками обработки файлов cookie и подключения к обработчикам обработки файлов cookie при добавлении или удалении файлов cookie.

Значение <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> по умолчанию — `SameSiteMode.Lax`, чтобы обеспечить проверку подлинности OAuth2. Чтобы строго применить политику того же сайта `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`. Хотя этот параметр нарушает OAuth2 и другие схемы проверки подлинности в разных источниках, он повышает уровень безопасности файлов cookie для других типов приложений, которые не полагаются на обработку запросов между источниками.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Параметр по промежуточного слоя политики cookie для `MinimumSameSitePolicy` может влиять на настройку `Cookie.SameSite` в параметрах `CookieAuthenticationOptions` в соответствии с приведенной ниже таблицей.

| минимумсамеситеполици | Файл cookie. SameSite | Результирующий параметр cookie. SameSite |
| --------------------- | --------------- | --------------------------------- |
| Самеситемоде. None     | Самеситемоде. None<br>Самеситемоде. нестрогий<br>Самеситемоде. | Самеситемоде. None<br>Самеситемоде. нестрогий<br>Самеситемоде. |
| Самеситемоде. нестрогий      | Самеситемоде. None<br>Самеситемоде. нестрогий<br>Самеситемоде. | Самеситемоде. нестрогий<br>Самеситемоде. нестрогий<br>Самеситемоде. |
| Самеситемоде.   | Самеситемоде. None<br>Самеситемоде. нестрогий<br>Самеситемоде. | Самеситемоде.<br>Самеситемоде.<br>Самеситемоде. |

## <a name="create-an-authentication-cookie"></a>Создание файла cookie для проверки подлинности

Чтобы создать файл cookie, содержащий сведения о пользователе, создайте <xref:System.Security.Claims.ClaimsPrincipal>. Сведения о пользователе сериализуются и хранятся в файле cookie. 

Создайте <xref:System.Security.Claims.ClaimsIdentity> с любыми необходимыми <xref:System.Security.Claims.Claim>s и вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> для входа пользователя:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ. Если `AuthenticationScheme` не указан, используется схема по умолчанию.

Для шифрования используется система [защиты данных](xref:security/data-protection/using-data-protection) ASP.NET Core. Для приложения, размещенного на нескольких компьютерах, балансировки нагрузки между приложениями или с помощью веб-фермы, [Настройте защиту данных](xref:security/data-protection/configuration/overview) для использования одного и того же типа звонка и идентификатора приложения.

## <a name="sign-out"></a>Выход

Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Если `CookieAuthenticationDefaults.AuthenticationScheme` (или "cookie") не используется в качестве схемы (например, "Контосокукие"), укажите схему, используемую при настройке поставщика проверки подлинности. В противном случае используется схема по умолчанию.

## <a name="react-to-back-end-changes"></a>Реагирование на изменения серверной части

После создания файла cookie файл cookie является единственным источником удостоверения. Если учетная запись пользователя отключена в серверных системах:

* Система проверки подлинности файлов cookie приложения продолжит обрабатывать запросы на основе файла cookie проверки подлинности.
* Пользователь остается в приложении, если файл cookie проверки подлинности является допустимым.

Событие <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> может использоваться для перехвата и переопределения проверки удостоверения файла cookie. Проверка файла cookie при каждом запросе снижает риск отзыва пользователей, обращающихся к приложению.

Один из подходов к проверке файлов cookie основан на отслеживании времени изменения пользовательской базы данных. Если база данных не была изменена с момента выдачи файла cookie пользователя, нет необходимости повторно пройти проверку подлинности пользователя, если его файл cookie остается действительным. В примере приложения база данных реализуется в `IUserRepository` и сохраняет значение `LastChanged`. Когда пользователь обновляется в базе данных, `LastChanged` значение устанавливается в текущее время.

Чтобы сделать файл cookie недействительным при изменении базы данных на основе значения `LastChanged`, создайте файл cookie с утверждением `LastChanged`, содержащим текущее значение `LastChanged` из базы данных:

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

Чтобы реализовать переопределение для события `ValidatePrincipal`, напишите метод со следующей сигнатурой в классе, производном от <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Ниже приведен пример реализации `CookieAuthenticationEvents`.

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

Зарегистрируйте экземпляр событий во время регистрации службы файлов cookie в методе `Startup.ConfigureServices`. Укажите регистрацию службы с заданной [областью](xref:fundamentals/dependency-injection#service-lifetimes) для класса `CustomCookieAuthenticationEvents`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Рассмотрим ситуацию, в которой имя пользователя обновляется&mdash;принятия решения, которое не влияет на безопасность каким-либо образом. Если вы хотите без разрушения обновить субъект-пользователя, вызовите `context.ReplacePrincipal` и задайте для свойства `context.ShouldRenew` значение `true`.

> [!WARNING]
> Описанный здесь подход срабатывает при каждом запросе. Проверка файлов cookie проверки подлинности для всех пользователей при каждом запросе может привести к значительному снижению производительности приложения.

## <a name="persistent-cookies"></a>Постоянные файлы cookie

Возможно, вы хотите, чтобы файл cookie сохранялся в сеансах браузера. Это сохраняемость следует включать только при явном согласии пользователей с флажком "Запомнить меня" при входе или аналогичном механизме. 

В следующем фрагменте кода создается удостоверение и соответствующий файл cookie, который остается недействительным через замыкания браузера. Учитываются все настроенные ранее параметры срока действия. Если срок действия файла cookie истекает при закрытии браузера, браузер очищает файл cookie после его перезапуска.

Задайте для <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> значение `true` в <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

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

## <a name="absolute-cookie-expiration"></a>Абсолютный срок действия файла cookie

Абсолютный срок действия можно задать с помощью <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Чтобы создать постоянный файл cookie, необходимо также задать `IsPersistent`. В противном случае файл cookie создается с временем существования на основе сеанса и может истечь до или после полученного билета проверки подлинности. Если параметр `ExpiresUtc` установлен, он переопределяет значение параметра <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> в <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, если оно задано.

В следующем фрагменте кода создается удостоверение и соответствующий файл cookie, который длится 20 минут. При этом игнорируются все параметры скользящего срока действия, настроенные ранее.

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core удостоверение — это полнофункциональный поставщик проверки подлинности для создания и обслуживания имен входа. Однако можно использовать поставщик проверки подлинности на основе файлов cookie без ASP.NET Core удостоверения. Дополнительные сведения см. в разделе <xref:security/authentication/identity>.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))

В демонстрационных целях в примере приложения учетная запись пользователя для гипотетического пользователя, Мария Rodriguez, жестко закодирована в приложении. Используйте адрес **электронной почты** `maria.rodriguez@contoso.com` и любой пароль для входа пользователя. Пользователь прошел проверку подлинности в методе `AuthenticateUser` в файле *pages/Account/Login. cshtml. CS* . В реальном примере пользователь будет проходить проверку подлинности в базе данных.

## <a name="configuration"></a>Конфигурация

Если приложение не использует [метапакет Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для пакета [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .

В методе `Startup.ConfigureServices` Создайте службу промежуточного слоя проверки подлинности с помощью методов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> и <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>, передаваемые в `AddAuthentication`, задает схему проверки подлинности по умолчанию для приложения. `AuthenticationScheme` полезна при наличии нескольких экземпляров проверки подлинности файлов cookie и необходимости [авторизации с определенной схемой](xref:security/authorization/limitingidentitybyscheme). Установка `AuthenticationScheme` [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) предоставляет значение "cookies" для схемы. Можно указать любое строковое значение, которое различает схему.

Схема проверки подлинности приложения отличается от схемы проверки подлинности файлов cookie приложения. Если для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>не указана схема проверки подлинности файлов cookie, она использует `CookieAuthenticationDefaults.AuthenticationScheme` ("cookies").

Для свойства <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> файла cookie проверки подлинности по умолчанию установлено значение `true`. Файлы cookie проверки подлинности разрешены, когда посетитель сайта не был передан в сбор данных. Дополнительные сведения см. в разделе <xref:security/gdpr#essential-cookies>.

В методе `Startup.Configure` вызовите метод `UseAuthentication` для вызова по промежуточного слоя проверки подлинности, устанавливающего свойство `HttpContext.User`. Вызовите метод `UseAuthentication` перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

Класс <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> используется для настройки параметров поставщика проверки подлинности.

Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в методе `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>По промежуточного слоя политики файлов cookie

По [промежуточного слоя политики файлов cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) включает возможности политики файлов cookie. Добавление по промежуточного слоя к конвейеру обработки приложений выполняется с учетом порядка&mdash;он влияет только на нисходящие компоненты, зарегистрированные в конвейере.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Используйте <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>, предоставляемые по промежуточного слоя политики cookie для управления глобальными характеристиками обработки файлов cookie и подключения к обработчикам обработки файлов cookie при добавлении или удалении файлов cookie.

Значение <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> по умолчанию — `SameSiteMode.Lax`, чтобы обеспечить проверку подлинности OAuth2. Чтобы строго применить политику того же сайта `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`. Хотя этот параметр нарушает OAuth2 и другие схемы проверки подлинности в разных источниках, он повышает уровень безопасности файлов cookie для других типов приложений, которые не полагаются на обработку запросов между источниками.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Параметр по промежуточного слоя политики cookie для `MinimumSameSitePolicy` может влиять на настройку `Cookie.SameSite` в параметрах `CookieAuthenticationOptions` в соответствии с приведенной ниже таблицей.

| минимумсамеситеполици | Файл cookie. SameSite | Результирующий параметр cookie. SameSite |
| --------------------- | --------------- | --------------------------------- |
| Самеситемоде. None     | Самеситемоде. None<br>Самеситемоде. нестрогий<br>Самеситемоде. | Самеситемоде. None<br>Самеситемоде. нестрогий<br>Самеситемоде. |
| Самеситемоде. нестрогий      | Самеситемоде. None<br>Самеситемоде. нестрогий<br>Самеситемоде. | Самеситемоде. нестрогий<br>Самеситемоде. нестрогий<br>Самеситемоде. |
| Самеситемоде.   | Самеситемоде. None<br>Самеситемоде. нестрогий<br>Самеситемоде. | Самеситемоде.<br>Самеситемоде.<br>Самеситемоде. |

## <a name="create-an-authentication-cookie"></a>Создание файла cookie для проверки подлинности

Чтобы создать файл cookie, содержащий сведения о пользователе, создайте <xref:System.Security.Claims.ClaimsPrincipal>. Сведения о пользователе сериализуются и хранятся в файле cookie. 

Создайте <xref:System.Security.Claims.ClaimsIdentity> с любыми необходимыми <xref:System.Security.Claims.Claim>s и вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> для входа пользователя:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ. Если `AuthenticationScheme` не указан, используется схема по умолчанию.

Для шифрования используется система [защиты данных](xref:security/data-protection/using-data-protection) ASP.NET Core. Для приложения, размещенного на нескольких компьютерах, балансировки нагрузки между приложениями или с помощью веб-фермы, [Настройте защиту данных](xref:security/data-protection/configuration/overview) для использования одного и того же типа звонка и идентификатора приложения.

## <a name="sign-out"></a>Выход

Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Если `CookieAuthenticationDefaults.AuthenticationScheme` (или "cookie") не используется в качестве схемы (например, "Контосокукие"), укажите схему, используемую при настройке поставщика проверки подлинности. В противном случае используется схема по умолчанию.

## <a name="react-to-back-end-changes"></a>Реагирование на изменения серверной части

После создания файла cookie файл cookie является единственным источником удостоверения. Если учетная запись пользователя отключена в серверных системах:

* Система проверки подлинности файлов cookie приложения продолжит обрабатывать запросы на основе файла cookie проверки подлинности.
* Пользователь остается в приложении, если файл cookie проверки подлинности является допустимым.

Событие <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> может использоваться для перехвата и переопределения проверки удостоверения файла cookie. Проверка файла cookie при каждом запросе снижает риск отзыва пользователей, обращающихся к приложению.

Один из подходов к проверке файлов cookie основан на отслеживании времени изменения пользовательской базы данных. Если база данных не была изменена с момента выдачи файла cookie пользователя, нет необходимости повторно пройти проверку подлинности пользователя, если его файл cookie остается действительным. В примере приложения база данных реализуется в `IUserRepository` и сохраняет значение `LastChanged`. Когда пользователь обновляется в базе данных, `LastChanged` значение устанавливается в текущее время.

Чтобы сделать файл cookie недействительным при изменении базы данных на основе значения `LastChanged`, создайте файл cookie с утверждением `LastChanged`, содержащим текущее значение `LastChanged` из базы данных:

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

Чтобы реализовать переопределение для события `ValidatePrincipal`, напишите метод со следующей сигнатурой в классе, производном от <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Ниже приведен пример реализации `CookieAuthenticationEvents`.

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

Зарегистрируйте экземпляр событий во время регистрации службы файлов cookie в методе `Startup.ConfigureServices`. Укажите регистрацию службы с заданной [областью](xref:fundamentals/dependency-injection#service-lifetimes) для класса `CustomCookieAuthenticationEvents`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Рассмотрим ситуацию, в которой имя пользователя обновляется&mdash;принятия решения, которое не влияет на безопасность каким-либо образом. Если вы хотите без разрушения обновить субъект-пользователя, вызовите `context.ReplacePrincipal` и задайте для свойства `context.ShouldRenew` значение `true`.

> [!WARNING]
> Описанный здесь подход срабатывает при каждом запросе. Проверка файлов cookie проверки подлинности для всех пользователей при каждом запросе может привести к значительному снижению производительности приложения.

## <a name="persistent-cookies"></a>Постоянные файлы cookie

Возможно, вы хотите, чтобы файл cookie сохранялся в сеансах браузера. Это сохраняемость следует включать только при явном согласии пользователей с флажком "Запомнить меня" при входе или аналогичном механизме. 

В следующем фрагменте кода создается удостоверение и соответствующий файл cookie, который остается недействительным через замыкания браузера. Учитываются все настроенные ранее параметры срока действия. Если срок действия файла cookie истекает при закрытии браузера, браузер очищает файл cookie после его перезапуска.

Задайте для <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> значение `true` в <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

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

## <a name="absolute-cookie-expiration"></a>Абсолютный срок действия файла cookie

Абсолютный срок действия можно задать с помощью <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Чтобы создать постоянный файл cookie, необходимо также задать `IsPersistent`. В противном случае файл cookie создается с временем существования на основе сеанса и может истечь до или после полученного билета проверки подлинности. Если параметр `ExpiresUtc` установлен, он переопределяет значение параметра <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> в <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, если оно задано.

В следующем фрагменте кода создается удостоверение и соответствующий файл cookie, который длится 20 минут. При этом игнорируются все параметры скользящего срока действия, настроенные ранее.

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

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Проверки ролей на основе политик](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
