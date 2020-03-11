---
title: Совместное использование файлов cookie проверки подлинности между приложениями ASP.NET
author: rick-anderson
description: Узнайте, как совместно использовать файлы cookie проверки подлинности в приложениях ASP.NET 4. x и ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/cookie-sharing
ms.openlocfilehash: 7e29be22717f0b97fc115ac036cc54e333bed4e2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651610"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>Совместное использование файлов cookie проверки подлинности между приложениями ASP.NET

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

Веб-сайты часто состоят из отдельных веб-приложений, работающих вместе. Чтобы обеспечить единый вход, веб-приложения на сайте должны использовать файлы cookie проверки подлинности. Для поддержки этого сценария в стеке защиты данных разрешено совместное использование проверки подлинности Katana cookie и ASP.NET Core билетов проверки подлинности файлов cookie.

В следующих примерах:

* Для имени файла cookie проверки подлинности задано общее значение `.AspNet.SharedCookie`.
* Для `AuthenticationType` задано `Identity.Application` явно или по умолчанию.
* Общее имя приложения позволяет системе защиты данных совместно использовать ключи защиты данных (`SharedCookieApp`).
* в качестве схемы проверки подлинности используется `Identity.Application`. Независимо от используемой схемы, ее необходимо использовать последовательно *в и через* общие приложения cookie либо в качестве схемы по умолчанию, либо путем их явного задания. Схема используется при шифровании и расшифровке файлов cookie, поэтому согласованная схема должна использоваться в приложениях.
* Используется общее место хранения [ключей защиты данных](xref:security/data-protection/implementation/key-management) .
  * В ASP.NET Core приложениях <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> используется для задания места хранения ключей.
  * В .NET Framework приложениях по промежуточного слоя для проверки подлинности файлов использует реализацию <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>. `DataProtectionProvider` предоставляет службы защиты данных для шифрования и расшифровки полезных данных cookie проверки подлинности. `DataProtectionProvider` экземпляр изолирован от системы защиты данных, используемой другими частями приложения. [Датапротектионпровидер. Create (System. IO. DirectoryInfo, Action\<идатапротектионбуилдер >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) принимает <xref:System.IO.DirectoryInfo> для указания расположения хранилища ключей защиты данных.
* для `DataProtectionProvider` требуется пакет NuGet [Microsoft. AspNetCore. в отношении защиты. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) :
  * В ASP.NET Core приложения 2. x сослаться на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app).
  * В .NET Framework приложения добавьте ссылку на пакет в [Microsoft. AspNetCore. "Защита. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> задает общее имя приложения.

## <a name="share-authentication-cookies-with-aspnet-core-identity"></a>Совместное использование файлов cookie проверки подлинности с удостоверением ASP.NET Core

При использовании удостоверения ASP.NET Core:

* Ключи защиты данных и имя приложения должны быть общими для всех приложений. В следующих примерах для метода <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> предоставляется общее место хранения ключей. Используйте <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> для настройки общего имени общего приложения (`SharedCookieApp` в следующих примерах). Дополнительные сведения см. в разделе <xref:security/data-protection/configuration/overview>.
* Используйте метод расширения <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*>, чтобы настроить службу защиты данных для файлов cookie.
* Тип проверки подлинности по умолчанию — `Identity.Application`.

В `Startup.ConfigureServices`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

## <a name="share-authentication-cookies-without-aspnet-core-identity"></a>Совместное использование файлов cookie проверки подлинности без удостоверения ASP.NET Core

При использовании файлов cookie напрямую без ASP.NET Core удостоверения настройте защиту данных и проверку подлинности в `Startup.ConfigureServices`. В следующем примере тип проверки подлинности имеет значение `Identity.Application`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

## <a name="share-cookies-across-different-base-paths"></a>Совместное использование файлов cookie в разных базовых путях

Файл cookie для проверки подлинности использует [HttpRequest. пасбасе](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) в качестве [файла cookie. Path](xref:Microsoft.AspNetCore.Http.CookieBuilder.Path)по умолчанию. Если файл cookie приложения должен совместно использоваться в разных базовых путях, `Path` необходимо переопределить:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
    options.Cookie.Path = "/";
});
```

## <a name="share-cookies-across-subdomains"></a>Совместное использование файлов cookie в поддоменах

При размещении приложений, которые совместно используют файлы cookie в поддоменах, укажите общий домен в свойстве [cookie. domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) . Чтобы предоставить общий доступ к файлам cookie в разных приложениях на `contoso.com`, например `first_subdomain.contoso.com` и `second_subdomain.contoso.com`, укажите `Cookie.Domain` как `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Шифрование неактивных ключей защиты данных

Для рабочих развертываний настройте `DataProtectionProvider` для шифрования неактивных ключей с помощью DPAPI или X509. Дополнительные сведения см. в разделе <xref:security/data-protection/implementation/key-encryption-at-rest>. В следующем примере отпечаток сертификата предоставляется для <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Совместное использование файлов cookie проверки подлинности в приложениях ASP.NET 4. x и ASP.NET Core

Приложения ASP.NET 4. x, использующие по промежуточного слоя для проверки подлинности Katana cookie, можно настроить для создания файлов cookie проверки подлинности, совместимых с по промежуточного слоя ASP.NET Core cookie Authentication. Это позволяет обновлять отдельные приложения большого сайта за несколько этапов, предоставляя возможность беспрепятственного единого входа на сайте.

Когда приложение использует по промежуточного слоя проверки подлинности Katana cookie, оно вызывает `UseCookieAuthentication` в файле *Startup.auth.CS* проекта. Проекты веб-приложений ASP.NET 4. x, созданные с помощью Visual Studio 2013 и позднее, по умолчанию используют по промежуточного слоя проверки подлинности Katana cookie. Хотя `UseCookieAuthentication` устарела и не поддерживается для ASP.NET Core приложений, вызов `UseCookieAuthentication` в приложении ASP.NET 4. x, который использует по промежуточного слоя проверки подлинности Katana cookie, является допустимым.

Приложение ASP.NET 4. x должно быть предназначено для .NET Framework 4.5.1 или более поздней версии. В противном случае не удается установить необходимые пакеты NuGet.

Чтобы предоставить общий доступ к файлам cookie проверки подлинности между приложением ASP.NET 4. x и ASP.NET Core приложением, настройте ASP.NET Core приложение, как указано в разделе [общий доступ к файлам cookie проверки подлинности между приложениями ASP.NET Core](#share-authentication-cookies-with-aspnet-core-identity) , а затем настройте приложение ASP.NET 4. x следующим образом.

Убедитесь, что пакеты приложения обновлены до последних выпусков. Установите пакет [Microsoft. Owin. Security. Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) в каждое приложение ASP.NET 4. x.

Нахождение и изменение вызова `UseCookieAuthentication`:

* Измените имя файла cookie, чтобы оно совпадало с именем, используемым по промежуточного слоя ASP.NET Core cookie Authentication (`.AspNet.SharedCookie` в примере).
* В следующем примере тип проверки подлинности имеет значение `Identity.Application`.
* Укажите экземпляр `DataProtectionProvider`, инициализированного в общем расположении хранилища ключей для защиты данных.
* Убедитесь, что для имени приложения задано общее имя приложения, используемое всеми приложениями, которые совместно используют файлы cookie проверки подлинности (`SharedCookieApp` в примере).

Если `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` и `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`не заданы, задайте для <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> утверждение, которое различает уникальных пользователей.

*App_Start/стартуп.АУС.КС*:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = "Identity.Application",
    CookieName = ".AspNet.SharedCookie",
    LoginPath = new PathString("/Account/Login"),
    Provider = new CookieAuthenticationProvider
    {
        OnValidateIdentity =
            SecurityStampValidator
                .OnValidateIdentity<ApplicationUserManager, ApplicationUser>(
                    validateInterval: TimeSpan.FromMinutes(30),
                    regenerateIdentity: (manager, user) =>
                        user.GenerateUserIdentityAsync(manager))
    },
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create("{PATH TO COMMON KEY RING FOLDER}",
                (builder) => { builder.SetApplicationName("SharedCookieApp"); })
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies." +
                    "CookieAuthenticationMiddleware",
                "Identity.Application",
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});

System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier =
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name";
```

При создании удостоверения пользователя тип проверки подлинности (`Identity.Application`) должен соответствовать типу, определенному в `AuthenticationType` Set с `UseCookieAuthentication` в *App_Start/стартуп.АУС.КС*.

*Models/идентитимоделс. CS*:

```csharp
public class ApplicationUser : IdentityUser
{
    public async Task<ClaimsIdentity> GenerateUserIdentityAsync(
        UserManager<ApplicationUser> manager)
    {
        // The authenticationType must match the one defined in 
        // CookieAuthenticationOptions.AuthenticationType
        var userIdentity = 
            await manager.CreateIdentityAsync(this, "Identity.Application");

        // Add custom user claims here

        return userIdentity;
    }
}
```

## <a name="use-a-common-user-database"></a>Использование общей пользовательской базы данных

Если приложения используют одну и ту же схему удостоверений (идентичную версию Identity), убедитесь, что система идентификации для каждого приложения указывает на одну и ту же пользовательскую базу данных. В противном случае система идентификации выдает ошибки во время выполнения, когда пытается сопоставить информацию в файле cookie проверки подлинности с данными в своей базе данных.

Если схема удостоверений различается в приложениях, обычно так как приложения используют разные версии удостоверений, общий доступ к общей базе данных на основе последней версии удостоверения невозможен без повторного сопоставления и добавления столбцов в схемах удостоверений других приложений. Часто более эффективно обновлять другие приложения, чтобы использовать последнюю версию удостоверения, чтобы обеспечить общий доступ к общей базе данных для приложений.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:host-and-deploy/web-farm>
