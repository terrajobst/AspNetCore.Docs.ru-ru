---
title: Совместное использование файлов cookie проверки подлинности между приложениями ASP.NET
author: rick-anderson
description: Узнайте, как совместно использовать файлы cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2019
uid: security/cookie-sharing
ms.openlocfilehash: b2f906ac97fe79b2a66a5ab709bcbcb03ab8cc39
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223912"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>Совместное использование файлов cookie проверки подлинности между приложениями ASP.NET

По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)

Веб-сайтов часто состоят из отдельных веб-приложений, работающих вместе. Для работы единого входа (SSO), веб-приложений в пределах сайта должны совместно использовать файлы cookie проверки подлинности. Для поддержки этого сценария, в стеке защиты данных позволяет совместное использование проверки подлинности файла cookie Katana и билеты проверки подлинности файла cookie ASP.NET Core.

В приведенных ниже примерах:

* Имя файла cookie проверки подлинности имеет значение к общему значению `.AspNet.SharedCookie`.
* `AuthenticationType` Присваивается `Identity.Application` явно или по умолчанию.
* Общее имя приложения используется для включения система защиты данных для совместного использования ключей защиты данных (`SharedCookieApp`).
* `Identity.Application` используется как схему проверки подлинности. Используется независимо от схемы, он должен использоваться согласованно *внутри и между* общего файла cookie приложения, как схема по умолчанию или установив его явным образом. Схема используется при шифровании и расшифровки куки-файлов, поэтому необходимо использовать согласованную схему в различных приложениях.
* Общий [ключа защиты данных](xref:security/data-protection/implementation/key-management) используется место хранения.
  * В приложениях ASP.NET Core <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> используется для указания расположения хранилища ключей.
  * В приложениях .NET Framework, по промежуточного слоя для файлов Cookie проверки подлинности использует реализацию <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>. `DataProtectionProvider` предоставляет службы защиты данных для шифрования и расшифровки данных полезные данные файлов cookie проверки подлинности. `DataProtectionProvider` Экземпляр изолирован от система защиты данных, используемых в других частях приложения. [DataProtectionProvider.Create (System.IO.DirectoryInfo, действие\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) принимает <xref:System.IO.DirectoryInfo> для указания расположения для хранения ключей защиты данных.
* `DataProtectionProvider` требуется [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет NuGet:
  * В приложениях ASP.NET Core 2.x, ссылаются на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
  * В приложениях .NET Framework, добавьте ссылки на пакет [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Задает общее имя приложения.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Совместное использование файлов cookie проверки подлинности между приложениями ASP.NET Core

При использовании удостоверения ASP.NET Core:

* Ключи защиты данных и имя приложения должно совместно использоваться приложениями. Общую область хранения ключей предоставляется <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> метод в следующих примерах. Используйте <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Настройка общего имени общего приложения (`SharedCookieApp` в следующих примерах). Дополнительные сведения см. в разделе <xref:security/data-protection/configuration/overview>.
* Используйте <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> метод расширения, чтобы настроить службу защиты данных для файлов cookie.
* В следующем примере присваивается тип проверки подлинности `Identity.Application` по умолчанию.

В `Startup.ConfigureServices`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

При использовании файлов cookie без ASP.NET Core Identity, настроить защиту данных и проверку подлинности в `Startup.ConfigureServices`. В следующем примере присваивается тип проверки подлинности `Identity.Application`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

При размещении приложений, которые совместно использовать файлы cookie в поддомены, укажите общий домен в [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) свойство. Для совместного использования файлов cookie в приложениях в `contoso.com`, такие как `first_subdomain.contoso.com` и `second_subdomain.contoso.com`, укажите `Cookie.Domain` как `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Шифрование неактивных ключей защиты данных

Для развертывания в рабочей среде, настройте `DataProtectionProvider` для шифрования ключей при хранении с помощью DPAPI или X509Certificate. Дополнительные сведения см. в разделе <xref:security/data-protection/implementation/key-encryption-at-rest>. В следующем примере предоставляется отпечаток сертификата <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Совместное использование файлов cookie проверки подлинности ASP.NET 4.x и приложений ASP.NET Core

Для создания файлов cookie проверки подлинности, которые совместимы с по промежуточного слоя ASP.NET Core файл Cookie проверки подлинности можно настроить приложений ASP.NET 4.x, использующих Katana промежуточного по проверки подлинности файла Cookie. Благодаря этому обновлении некий обширный узел отдельных приложений, за несколько шагов то же время предоставляя беспроблемную работу единого входа в рамках всего сайта.

Когда приложение использует Katana промежуточного по проверки подлинности файла Cookie, он вызывает `UseCookieAuthentication` в проекте *Startup.Auth.cs* файла. Приложения веб-проектов ASP.NET 4.x созданные в Visual Studio 2013 и более поздней версии используйте по промежуточного слоя Katana файл Cookie проверки подлинности по умолчанию. Несмотря на то что `UseCookieAuthentication` устарел и не поддерживается для приложений ASP.NET Core, вызвав `UseCookieAuthentication` в ASP.NET 4.x приложение, использующее Katana промежуточного по проверки подлинности файла Cookie является допустимым.

Приложение ASP.NET 4.x необходимо предназначено для .NET Framework 4.5.1 или более поздней версии. В противном случае необходимые пакеты NuGet не удается установить.

Для совместного использования файлов cookie проверки подлинности между приложения ASP.NET 4.x и приложения ASP.NET Core, настройте приложение ASP.NET Core, как указано в [совместно использовать файлы cookie проверки подлинности между приложениями ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) раздела, а затем настройте приложение ASP.NET 4.x в качестве следующим образом.

Убедитесь, что пакеты приложения, обновляются до последней версии. Установка [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) пакет в каждое приложение ASP.NET 4.x.

Найдите и измените вызов `UseCookieAuthentication`:

* Измените имя файла cookie, чтобы оно соответствовало имени, используемые по промежуточного слоя ASP.NET Core файл Cookie проверки подлинности (`.AspNet.SharedCookie` в примере).
* В следующем примере присваивается тип проверки подлинности `Identity.Application`.
* Предоставляет экземпляр `DataProtectionProvider` инициализирован в общую папку хранилища ключей защиты данных.
* Убедитесь, что имя приложения имеет значение с общим именем приложения, используемый для всех приложений, которые совместно используют файлы cookie проверки подлинности (`SharedCookieApp` в примере).

Если не задан `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` и `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, задайте <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> на утверждение, являющийся отличительным признаком уникальных пользователей.

*App_Start/Startup.AUTH.cs*:

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
            DataProtectionProvider.Create({PATH TO COMMON KEY RING FOLDER},
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

При создании удостоверения пользователя, тип проверки подлинности (`Identity.Application`) должен совпадать с типом, определенные в `AuthenticationType` набор с `UseCookieAuthentication` в *App_Start/Startup.Auth.cs*.

*Models/IdentityModels.cs*:

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

## <a name="use-a-common-user-database"></a>Использование общей базы данных пользователя

Когда приложения использовали один идентификатор схемы (та же версия удостоверения), убедитесь, что система идентификации для каждого приложения указывает на одну и ту же базу данных пользователя. В противном случае система идентификации дает сбои во время выполнения, если предпринимается попытка сопоставить сведения в файл cookie проверки подлинности с информацией в своей базе данных.

При идентификации схема отличается между приложениями, обычно потому, что приложения, используют разные версии Identity, совместного использования общей базы данных на основе последней версии Identity невозможны без повторное сопоставление и добавления столбцов в схемах удостоверений других приложений. Часто бывает более эффективно для обновления в других приложениях, чтобы использовать последнюю версию удостоверений, таким образом, чтобы общей базы данных может совместно использоваться приложениями.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:host-and-deploy/web-farm>
