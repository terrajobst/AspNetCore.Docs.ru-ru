---
title: "Совместное использование файлов cookie между приложениями"
author: rick-anderson
description: "Дополнительные сведения о совместном использовании файлов cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: 034c080c3459cd3a9e6b412492d1b19d9e6348a4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2018
---
# <a name="sharing-cookies-among-apps"></a>Совместное использование файлов cookie между приложениями

По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Latham Люк](https://github.com/guardrex)

Веб-сайтов часто состоят из отдельных веб-приложений, работающих вместе. Для обеспечения единого входа (SSO), веб-приложений в пределах сайта должны совместно использовать файлы cookie проверки подлинности. Для поддержки этого сценария, стека защиты данных позволяет совместное использование проверки подлинности файла cookie Katana и ASP.NET Core билетов проверки подлинности файла cookie.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

В образце показано совместное использование три приложения, использующие файл cookie проверки подлинности файла cookie:

* Приложения ASP.NET Core страниц Razor 2.0 без использования [ASP.NET Core Identity](xref:security/authentication/identity)
* Приложение MVC ASP.NET Core 2.0 с удостоверением ASP.NET Core
* Приложение MVC ASP.NET Framework 4.6.1 с ASP.NET Identity

В следующем примере:

* Общее значение для имени файла cookie проверки подлинности имеет значение `.AspNet.SharedCookie`.
* `AuthenticationType` Равно `Identity.Application` явно или по умолчанию.
* [CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) используется в качестве схемы проверки подлинности. Константа разрешается в значение `Cookies`.
* Файл cookie проверки подлинности по промежуточного слоя использует реализацию [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider`предоставляет службы защиты данных для шифрования и расшифровки данных полезные данные файлов cookie проверки подлинности. `DataProtectionProvider` Экземпляр изолирован от системы защиты данных, используемые в других частях приложения.
  * Обычное [ключа защиты данных](xref:security/data-protection/implementation/key-management) используется расположение хранилища. В примере приложения используется папка с именем *KeyRing* в корень решения для хранения ключей защиты данных.
  * [DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) принимает [DirectoryInfo](/dotnet/api/system.io.directoryinfo) для использования с файлы cookie проверки подлинности. Пример приложения предоставляет путь *KeyRing* папки `DirectoryInfo`.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) требует [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет NuGet. Чтобы получить этот пакет для основных ASP.NET 2.0 и более поздних версий приложений, ссылаются на [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. При разработке для .NET Framework, добавьте ссылку на пакет `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Совместно использовать файлы cookie проверки подлинности между приложениями ASP.NET Core

При использовании ASP.NET Core Identity:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

В `ConfigureServices` используйте [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) метод расширения, чтобы настроить службу защиты данных файлов cookie.

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

В разделе *CookieAuthWithIdentity.Core* проекта в [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

В `Configure` используйте [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) для настройки:

* Служба защиты данных файлов cookie.
* `AuthenticationScheme` Для сопоставления ASP.NET 4.x.

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

При использовании файлов cookie напрямую:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

В разделе *CookieAuth.Core* проекта в [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a>Шифрование ключей защиты данных на rest

В производственной среде, настройте `DataProtectionProvider` для шифрования ключей при хранении с помощью DPAPI или X509Certificate. В разделе [ключ шифрования на Rest](xref:security/data-protection/implementation/key-encryption-at-rest) для получения дополнительной информации.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Совместное использование файлов cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core

Для создания файлов cookie проверки подлинности, которые совместимы с по промежуточного слоя ASP.NET Core файла cookie проверки подлинности можно настроить приложения ASP.NET 4.x, которые используют Katana промежуточного по проверки подлинности файла cookie. Это позволяет поэтапное обновление большого узла отдельными приложениями то же время предоставляя smooth службы Единого входа в пределах сайта.

> [!TIP]
> Если приложение использует Katana промежуточного по проверки подлинности файла cookie, он вызывает `UseCookieAuthentication` в проекте *Startup.Auth.cs* файла. Приложения веб-проектов ASP.NET 4.x создан в Visual Studio 2013 и более поздней версии, по умолчанию используют промежуточного по проверки подлинности файла cookie Katana.

> [!NOTE]
> Приложение ASP.NET 4.x должны предназначаться для платформы .NET Framework 4.5.1 или более поздней версии. В противном случае необходимые пакеты NuGet не удалось установить.

Для совместного использования файлов cookie проверки подлинности между ASP.NET 4.x приложений и приложений ASP.NET Core, настроить приложение ASP.NET Core, как упоминалось выше, а затем настройте приложения ASP.NET 4.x, выполнив указанные ниже действия.

1. Установите пакет [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) в каждое приложение ASP.NET 4.x.

2. В *Startup.Auth.cs*, найдите вызов `UseCookieAuthentication` и внесите следующие изменения. Измените имя файла cookie, должно соответствовать имени, используемые по промежуточного слоя ASP.NET Core файла cookie проверки подлинности. Предоставляет экземпляр `DataProtectionProvider` инициализирован в общую папку хранилища ключей защиты данных.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

В разделе *CookieAuthWithIdentity.NETFramework* проекта в [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).

При создании удостоверения пользователя, типа проверки подлинности должен соответствовать типу, определенные в `AuthenticationType` с `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Задать `CookieManager` с взаимодействием `ChunkingCookieManager` , совместима неверный формат.

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a>Использование общей базы данных пользователя

Убедитесь, что система идентификаторов для каждого приложения указывает на одной и той же базе данных пользователя. В противном случае система идентификации выводятся ошибки во время выполнения при попытке соответствует сведениям в файл cookie проверки подлинности со сведениями в своей базе данных.
