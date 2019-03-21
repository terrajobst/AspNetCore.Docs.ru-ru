---
title: Совместное использование приложениями файлов cookie с помощью ASP.NET и ASP.NET Core
author: rick-anderson
description: Узнайте, как совместно использовать файлы cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: 504d3369c908aabddeaa2fc3d1ef16a42b5cb771
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208519"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>Совместное использование приложениями файлов cookie с помощью ASP.NET и ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)

Веб-сайтов часто состоят из отдельных веб-приложений, работающих вместе. Для работы единого входа (SSO), веб-приложений в пределах сайта должны совместно использовать файлы cookie проверки подлинности. Для поддержки этого сценария, в стеке защиты данных позволяет совместное использование проверки подлинности файла cookie Katana и билеты проверки подлинности файла cookie ASP.NET Core.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([как скачивать](xref:index#how-to-download-a-sample))

В примере показано совместное использование трех приложениях, использующих файл cookie проверки подлинности файла cookie:

* Приложения ASP.NET Core 2.0 Razor Pages без использования [удостоверения ASP.NET Core](xref:security/authentication/identity)
* Приложение ASP.NET Core 2.0 MVC с помощью ASP.NET Core Identity
* Приложения ASP.NET Framework 4.6.1 MVC в ASP.NET Identity

В приведенных ниже примерах:

* Имя файла cookie проверки подлинности имеет значение к общему значению `.AspNet.SharedCookie`.
* `AuthenticationType` Присваивается `Identity.Application` явно или по умолчанию.
* Общее имя приложения используется для включения система защиты данных для совместного использования ключей защиты данных (`SharedCookieApp`).
* `Identity.Application` используется как схему проверки подлинности. Используется независимо от схемы, он должен использоваться согласованно *внутри и между* общего файла cookie приложения, как схема по умолчанию или установив его явным образом. Схема используется при шифровании и расшифровки куки-файлов, поэтому необходимо использовать согласованную схему в различных приложениях.
* Общий [ключа защиты данных](xref:security/data-protection/implementation/key-management) используется место хранения. Пример приложения использует папку с именем *Брелок* в корне решения для хранения ключи защиты данных.
* В приложениях ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) используется для указания расположения хранилища ключей. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) используется для настройки общего имени общего приложения.
* В приложении .NET Framework, по промежуточного слоя проверки подлинности использует реализацию [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` предоставляет службы защиты данных для шифрования и расшифровки данных полезные данные файлов cookie проверки подлинности. `DataProtectionProvider` Экземпляр изолирован от система защиты данных, используемых в других частях приложения.
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo, действие\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) принимает [DirectoryInfo](/dotnet/api/system.io.directoryinfo) для указания расположения для хранения ключей защиты данных. В примере приложения приведен путь к *Брелок* папку `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) задает общее имя приложения.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) требует [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет NuGet. Чтобы получить этот пакет для ASP.NET Core 2.1 и более поздних версий приложений, ссылаются на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). При нацеливании на .NET Framework, добавьте ссылки на пакет `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Совместное использование файлов cookie проверки подлинности между приложениями ASP.NET Core

При использовании удостоверения ASP.NET Core:

::: moniker range=">= aspnetcore-2.0"

В `ConfigureServices` используйте [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) метод расширения, чтобы настроить службу защиты данных для файлов cookie.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Ключи защиты данных и имя приложения должно совместно использоваться приложениями. В образце приложения `GetKeyRingDirInfo` возвращает общие расположение хранилища ключей для [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) метод. Используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) Настройка общего имени общего приложения (`SharedCookieApp` в образце). Дополнительные сведения см. в разделе [Настройка защиты данных](xref:security/data-protection/configuration/overview).

При размещении приложений, которые совместно использовать файлы cookie в поддомены, укажите общий домен в [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) свойство. Для совместного использования файлов cookie в приложениях в `contoso.com`, такие как `first_subdomain.contoso.com` и `second_subdomain.contoso.com`, укажите `Cookie.Domain` как `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

См. в разделе *CookieAuthWithIdentity.Core* в проекте [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([загрузке](xref:index#how-to-download-a-sample)).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

В `Configure` используйте [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) для настройки:

* Служба защиты данных для файлов cookie.
* `AuthenticationScheme` В соответствии с ASP.NET 4.x.

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

::: moniker-end

При использовании файлов cookie напрямую:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Ключи защиты данных и имя приложения должно совместно использоваться приложениями. В образце приложения `GetKeyRingDirInfo` возвращает общие расположение хранилища ключей для [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) метод. Используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) Настройка общего имени общего приложения (`SharedCookieApp` в образце). Дополнительные сведения см. в разделе [Настройка защиты данных](xref:security/data-protection/configuration/overview).

При размещении приложений, которые совместно использовать файлы cookie в поддомены, укажите общий домен в [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) свойство. Для совместного использования файлов cookie в приложениях в `contoso.com`, такие как `first_subdomain.contoso.com` и `second_subdomain.contoso.com`, укажите `Cookie.Domain` как `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

См. в разделе *CookieAuth.Core* в проекте [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([загрузке](xref:index#how-to-download-a-sample)).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

::: moniker-end

## <a name="encrypting-data-protection-keys-at-rest"></a>Шифрование неактивных ключей защиты данных

Для развертывания в рабочей среде, настройте `DataProtectionProvider` для шифрования ключей при хранении с помощью DPAPI или X509Certificate. См. в разделе [ключ шифрования неактивных](xref:security/data-protection/implementation/key-encryption-at-rest) Дополнительные сведения.

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Совместное использование файлов cookie проверки подлинности ASP.NET 4.x и приложений ASP.NET Core

Приложения ASP.NET 4.x, которые используют Katana файл cookie проверки подлинности, по промежуточного слоя можно настроить для создания файлов cookie проверки подлинности, которые совместимы с по промежуточного слоя ASP.NET Core файл cookie проверки подлинности. Благодаря этому поэтапное обновление отдельных приложений некий обширный узел то же время предоставляя беспроблемную работу единого входа в рамках всего сайта.

Когда приложение использует Katana промежуточного по проверки подлинности файла cookie, он вызывает `UseCookieAuthentication` в проекте *Startup.Auth.cs* файла. Приложения веб-проектов ASP.NET 4.x созданные в Visual Studio 2013 и более поздней версии используйте по промежуточного слоя Katana файл cookie проверки подлинности по умолчанию. Несмотря на то что `UseCookieAuthentication` устарел и не поддерживается для приложений ASP.NET Core, вызвав `UseCookieAuthentication` в приложении ASP.NET 4.x, которое использует Katana промежуточного по проверки подлинности файла cookie является допустимым.

Приложение ASP.NET 4.x необходимо предназначено для .NET Framework 4.5.1 или более поздней версии. В противном случае необходимые пакеты NuGet не удается установить.

Для совместного использования файлов cookie проверки подлинности между приложения ASP.NET 4.x и приложения ASP.NET Core, настройте приложение ASP.NET Core, как указано выше, а затем настройте приложение ASP.NET 4.x, сделав следующее:

1. Установите пакет [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) в каждое приложение ASP.NET 4.x.

2. В *Startup.Auth.cs*, найдите вызов `UseCookieAuthentication` и измените его следующим образом. Измените имя файла cookie, чтобы оно соответствовало имени, используемые по промежуточного слоя ASP.NET Core файл cookie проверки подлинности. Предоставляет экземпляр `DataProtectionProvider` инициализирован в общую папку хранилища ключей защиты данных. Убедитесь, что общее имя приложения, используемое для всех приложений, которые совместно используют файлы cookie, присваивается имя приложения `SharedCookieApp` в примере приложения.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

См. в разделе *CookieAuthWithIdentity.NETFramework* в проекте [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([загрузке](xref:index#how-to-download-a-sample)).

При создании удостоверения пользователя, тип проверки подлинности должен соответствовать типу, определенные в `AuthenticationType` набор с `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a>Использование общей базы данных пользователя

Убедитесь, что система идентификации для каждого приложения указывает на одну и ту же базу данных пользователя. В противном случае система идентификации дает сбои во время выполнения, если предпринимается попытка сопоставить сведения в файл cookie проверки подлинности с информацией в своей базе данных.

## <a name="additional-resources"></a>Дополнительные ресурсы

<xref:host-and-deploy/web-farm>
