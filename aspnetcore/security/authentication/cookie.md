---
title: "С помощью файла Cookie проверки подлинности без удостоверения ASP.NET Core"
author: rick-anderson
description: "Описание использования файлов cookie проверки подлинности без ASP.NET Core Identity"
keywords: "ASP.NET Core, файлы cookie"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: af3ffe418521d5d97f5d14ca9c904c21b4d4ff89
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>С помощью файла Cookie проверки подлинности без удостоверения ASP.NET Core

<a name="security-authentication-cookie-middleware"></a>

ASP.NET Core 1.x указывает куки-файл [по промежуточного слоя](../../fundamentals/middleware.md#fundamentals-middleware) которого сериализует участника-пользователя в зашифрованном файле cookie, а затем, при последующих запросах проводится проверка куки-файл, повторно создает основной и присваивает его `HttpContext.User` свойство . Если вы хотите предоставить экраны входа и пользовательские базы данных, можно использовать файл cookie по промежуточного слоя изолированно.

Значительное изменение в ASP.NET Core 2.x — что по промежуточного слоя куки-файл не существует. Вместо этого `UseAuthentication` вызов метода в `Configure` метод *файла Startup.cs* добавляет AuthenticationMiddleware, который задает `HttpContext.User` свойства.

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a>Добавление и настройка

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Выполните следующие действия:

- Если не используется `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), установите версию 2.0 + `Microsoft.AspNetCore.Authentication.Cookies` пакета NuGet в проекте.

- Вызвать `UseAuthentication` метод в `Configure` метод *файла Startup.cs* файла:

    ```csharp
    app.UseAuthentication();
    ```

- Вызвать `AddAuthentication` и `AddCookie` методы в `ConfigureServices` метод *файла Startup.cs* файла:

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie("MyCookieAuthenticationScheme", options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Выполните следующие действия:

- Установить `Microsoft.AspNetCore.Authentication.Cookies` пакет NuGet в проекте. Этот пакет содержит файл cookie по промежуточного слоя.

- Добавьте следующие строки для `Configure` метод в вашей *файла Startup.cs* файла перед `app.UseMvc()` инструкции:

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

Фрагменты кода выше настроить некоторые или все из следующих вариантов:

* `AccessDeniedPath`— Это относительный путь, к которому перенаправления запросов, если пользователь пытается получить доступ к ресурсу, но не передает [политики авторизации](xref:security/authorization/policies#security-authorization-policies-based) для этого ресурса.

* `AuthenticationScheme`-Это значение, по которой происходит обращение схему определенного файла cookie проверки подлинности. Это полезно при наличии нескольких экземпляров файла cookie проверки подлинности, и вы хотите [применять проверку подлинности только один экземпляр](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).

* `AutomaticAuthenticate`-Этот флаг действителен только для ASP.NET Core 1.x. Указывает на то файла cookie проверки подлинности следует запустить при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана.

* `AutomaticChallenge`-Этот флаг действителен только для ASP.NET Core 1.x. Указывает, что файл cookie проверки подлинности 1.x следует перенаправить браузер `LoginPath` или `AccessDeniedPath` при сбое авторизации.

* `LoginPath`— Это относительный путь, по которому перенаправить запросы, когда пользователь пытается получить доступ к ресурсу, но не прошел проверку подлинности.

[Другие параметры](xref:security/authentication/cookie#security-authentication-cookie-options) включает возможность устанавливать издатель все утверждения, создает файл cookie проверки подлинности, имя файла cookie проверки подлинности удаляет домен для файла cookie и различные свойства безопасности в файле cookie. По умолчанию файл cookie проверки подлинности использует соответствующие параметры защиты для все файлы cookie, который создается, такие как:
- Установка флажка HttpOnly, чтобы предотвратить доступ к файлам cookie в JavaScript на стороне клиента
- Ограничение куки-файл для HTTPS, если запрос прошел по протоколу HTTPS

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a>Создание файла cookie удостоверений

Чтобы создать файл cookie, содержащий сведения о пользователе, следует создать [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) с информацией, нужно сериализовать в файл cookie. При наличии подходящего `ClaimsPrincipal` , следует вызвать следующий внутри метода контроллера:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

Это создает зашифрованный файл cookie и добавляет его в текущий ответ. `AuthenticationScheme` Указано при [конфигурации](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) должен использоваться при вызове `SignInAsync`.

На самом деле шифрования, используемый — ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) системы. Если имеются на нескольких компьютерах, нагрузки балансировки или с помощью веб-фермы, то необходимо [настроить защиту данных](xref:security/data-protection/configuration/overview#data-protection-configuring) использовать же ключей и идентификатор приложения.

## <a name="signing-out"></a>Выход

Чтобы выйти из системы текущего пользователя и удалить их куки-файл, следует вызвать следующий в свой контроллер:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a>Отклик на изменения в серверной части

>[!WARNING]
> После создания основного файла cookie становится единственным источником удостоверения. Даже при отключении пользователя в серверной части системы проверки подлинности файла cookie не имеет сведений о это и пользователь остается вошедший в систему, при условии, что их файл cookie является допустимым.

Файл cookie проверки подлинности предоставляет ряд событий в классе параметра. `ValidateAsync()` Событий можно использовать для перехвата и переопределение проверки подлинности файла cookie.

Рассмотрим базу данных пользователя серверной части, возможно, столбец «LastChanged». Чтобы сделать недействительными файла cookie при изменениях базы данных, следует сначала, если [Создание куки-файл](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), добавить утверждение «LastChanged», содержащую текущее значение. При изменении базы данных, значение «LastChanged» должен быть обновлен.

Чтобы реализовать переопределение для `ValidateAsync()` событий, необходимо написать метод со следующей сигнатурой:

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

ASP.NET Core Identity реализует эту проверку как часть его `SecurityStampValidator`. Пример выглядит следующим образом:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

Это может затем быть реализовано во время регистрации службы куки-файл в `ConfigureServices` метод *файла Startup.cs*:

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

Это может затем быть реализовано во время настройки файла cookie проверки подлинности в `Configure` метод *файла Startup.cs*:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

Рассмотрим пример, в котором был обновлен их имя &mdash; принятия решений, которая не влияет на безопасность системы никаким образом. Если вы хотите безболезненно обновить участника-пользователя, можно вызвать `context.ReplacePrincipal()` и задайте `context.ShouldRenew` свойства `true`.

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a>Управление параметрами куки-файл

[CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) класс включает различные параметры конфигурации для точной настройки создаваемого файлы cookie.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x объединяет API, используемые для настройки файлов cookie. 1.x, API-интерфейсы были помечены как устаревшие, а новые `Cookie` свойство типа `CookieBuilder` была введена в `CookieAuthenticationOptions` класса. Рекомендуется выполнить миграцию 2.x API-интерфейсы.

* `ClaimsIssuer`является издателем для [издателя](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) свойство все утверждения, созданные с помощью проверки подлинности файла cookie.

* `CookieBuilder.Domain`— Это имя домена, на который обслуживается куки-файл. По умолчанию это имя узла, которому отправляется запрос. Браузер служит только файла cookie для сопоставления имени узла. Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене. Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`и т. д.

* `CookieBuilder.HttpOnly`флаг, указывающий, куки-файл должен быть доступен только на серверы. По умолчанию используется `true`. Изменение этого значения может открыть приложение для кражи cookie приложения должны иметь ошибки межсайтовых сценариев.

* `CookieBuilder.Path`можно использовать для изоляции приложений, выполняющихся на то же имя узла. Если у вас есть приложение, работающее в `/app1` ограничить файлы cookie, выданный для только что отправлен на это приложение, затем следует установить `CookiePath` свойства `/app1`. Таким образом, куки-файл доступен только для запросов к `/app1` или каком-либо под ним.

* `CookieBuilder.SameSite`Указывает, является ли браузер должен позволять куки-файл для подключения к веб-сайте или межсайтовых запросов. По умолчанию используется `SameSiteMode.Lax`.

* `CookieBuilder.SecurePolicy`флаг, указывающий, если созданный файл cookie был ограничен HTTPS, HTTP или HTTPS или тот же протокол, что и при запросе. По умолчанию используется `SameAsRequest`.

* `ExpireTimeSpan`— `TimeSpan` после которого истекает срок действия cookie. Он добавляется в текущую дату и время создания срока действия файла cookie.

* `SlidingExpiration`флаг, указывающий, сбрасывается ли дата окончания срока действия файла cookie при более половины является `ExpireTimeSpan` истечет интервал. Новую дату окончания срока действия перемещается вперед для текущей даты, а также `ExpireTimespan`. [Абсолютный срок действия](xref:security/authentication/cookie#security-authentication-absolute-expiry) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`. Абсолютного срока действия может повысить безопасность ваших приложений, ограничивая количество времени, для которого действителен cookie проверки подлинности.

Пример использования `CookieAuthenticationOptions` в `ConfigureServices` метод *файла Startup.cs* ниже:

```csharp
services.AddAuthentication()
        .AddCookie(options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `ClaimsIssuer`является издателем для [издателя](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) свойство все утверждения, созданные по промежуточного слоя.

* `CookieDomain`— Это имя домена, на который обслуживается куки-файл. По умолчанию это имя узла, которому отправляется запрос. Браузер служит только файла cookie для сопоставления имени узла. Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене. Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`и т. д.

* `CookieHttpOnly`флаг, указывающий, куки-файл должен быть доступен только на серверы. По умолчанию используется `true`. Изменение этого значения может открыть приложение для кражи cookie приложения должны иметь ошибки межсайтовых сценариев.

* `CookiePath`можно использовать для изоляции приложений, выполняющихся на то же имя узла. Если у вас есть приложение, работающее в `/app1` ограничить файлы cookie, выданный для только что отправлен на это приложение, затем следует установить `CookiePath` свойства `/app1`. Таким образом, куки-файл доступен только для запросов к `/app1` или каком-либо под ним.

* `CookieSecure`флаг, указывающий, если созданный файл cookie был ограничен HTTPS, HTTP или HTTPS или тот же протокол, что и при запросе. По умолчанию используется `SameAsRequest`.

* `ExpireTimeSpan`— `TimeSpan` после которого истекает срок действия cookie. Он добавляется в текущую дату и время создания срока действия файла cookie.

* `SlidingExpiration`флаг, указывающий, сбрасывается ли дата окончания срока действия файла cookie при более половины является `ExpireTimeSpan` истечет интервал. Новую дату окончания срока действия перемещается вперед для текущей даты, а также `ExpireTimespan`. [Абсолютный срок действия](xref:security/authentication/cookie#security-authentication-absolute-expiry) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`. Абсолютного срока действия может повысить безопасность ваших приложений, ограничивая количество времени, для которого действителен cookie проверки подлинности.

Пример использования `CookieAuthenticationOptions` в `Configure` метод *файла Startup.cs* ниже:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a>Сохраняемые файлы cookie и время абсолютного срока действия

Вы можете истечения срока действия файла cookie, и сохраняются между сеансами браузера абсолютного срока действия для удостоверения и передает его куки-файл. Это сохраняемости должны включаться только с согласия пользователя, через флажка «Запомнить мои» на имя входа или аналогичного механизма. Это можно сделать с помощью `AuthenticationProperties` параметр на `SignInAsync` метод вызывается, когда [подписи в удостоверении и создание файла cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie). Пример:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

`AuthenticationProperties` Класс, используемый в предыдущем фрагменте кода находится в `Microsoft.AspNetCore.Authentication` пространства имен.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

`AuthenticationProperties` Класс, используемый в предыдущем фрагменте кода находится в `Microsoft.AspNetCore.Http.Authentication` пространства имен.

---

В предыдущем фрагменте кода создает удостоверение и соответствующий файл cookie, который сохраняется до закрытия браузера. Параметры скользящего срока действия ранее настроены с помощью [параметры cookie](xref:security/authentication/cookie#security-authentication-cookie-options) по-прежнему соблюдаются. После истечения срока действия файла cookie, хотя браузер будет закрыт, браузер удаляет его, после его перезапуска.

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

В предыдущем фрагменте кода создает удостоверение и соответствующий файл cookie, который сохраняется в течение 20 минут. Это не учитывает параметры скользящего срока действия ранее настроены с помощью [параметры cookie](xref:security/authentication/cookie#security-authentication-cookie-options).

`ExpiresUtc` И `IsPersistent` свойства являются взаимоисключающими.
