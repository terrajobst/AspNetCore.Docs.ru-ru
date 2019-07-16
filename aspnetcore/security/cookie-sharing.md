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
# <a name="share-authentication-cookies-among-aspnet-apps"></a><span data-ttu-id="f12e2-103">Совместное использование файлов cookie проверки подлинности между приложениями ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f12e2-103">Share authentication cookies among ASP.NET apps</span></span>

<span data-ttu-id="f12e2-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f12e2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f12e2-105">Веб-сайтов часто состоят из отдельных веб-приложений, работающих вместе.</span><span class="sxs-lookup"><span data-stu-id="f12e2-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="f12e2-106">Для работы единого входа (SSO), веб-приложений в пределах сайта должны совместно использовать файлы cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f12e2-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="f12e2-107">Для поддержки этого сценария, в стеке защиты данных позволяет совместное использование проверки подлинности файла cookie Katana и билеты проверки подлинности файла cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f12e2-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="f12e2-108">В приведенных ниже примерах:</span><span class="sxs-lookup"><span data-stu-id="f12e2-108">In the examples that follow:</span></span>

* <span data-ttu-id="f12e2-109">Имя файла cookie проверки подлинности имеет значение к общему значению `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="f12e2-109">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="f12e2-110">`AuthenticationType` Присваивается `Identity.Application` явно или по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f12e2-110">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="f12e2-111">Общее имя приложения используется для включения система защиты данных для совместного использования ключей защиты данных (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="f12e2-111">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="f12e2-112">`Identity.Application` используется как схему проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f12e2-112">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="f12e2-113">Используется независимо от схемы, он должен использоваться согласованно *внутри и между* общего файла cookie приложения, как схема по умолчанию или установив его явным образом.</span><span class="sxs-lookup"><span data-stu-id="f12e2-113">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="f12e2-114">Схема используется при шифровании и расшифровки куки-файлов, поэтому необходимо использовать согласованную схему в различных приложениях.</span><span class="sxs-lookup"><span data-stu-id="f12e2-114">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="f12e2-115">Общий [ключа защиты данных](xref:security/data-protection/implementation/key-management) используется место хранения.</span><span class="sxs-lookup"><span data-stu-id="f12e2-115">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span>
  * <span data-ttu-id="f12e2-116">В приложениях ASP.NET Core <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> используется для указания расположения хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="f12e2-116">In ASP.NET Core apps, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> is used to set the key storage location.</span></span>
  * <span data-ttu-id="f12e2-117">В приложениях .NET Framework, по промежуточного слоя для файлов Cookie проверки подлинности использует реализацию <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span><span class="sxs-lookup"><span data-stu-id="f12e2-117">In .NET Framework apps, Cookie Authentication Middleware uses an implementation of <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span></span> <span data-ttu-id="f12e2-118">`DataProtectionProvider` предоставляет службы защиты данных для шифрования и расшифровки данных полезные данные файлов cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f12e2-118">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="f12e2-119">`DataProtectionProvider` Экземпляр изолирован от система защиты данных, используемых в других частях приложения.</span><span class="sxs-lookup"><span data-stu-id="f12e2-119">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span> <span data-ttu-id="f12e2-120">[DataProtectionProvider.Create (System.IO.DirectoryInfo, действие\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) принимает <xref:System.IO.DirectoryInfo> для указания расположения для хранения ключей защиты данных.</span><span class="sxs-lookup"><span data-stu-id="f12e2-120">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepts a <xref:System.IO.DirectoryInfo> to specify the location for data protection key storage.</span></span>
* <span data-ttu-id="f12e2-121">`DataProtectionProvider` требуется [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="f12e2-121">`DataProtectionProvider` requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package:</span></span>
  * <span data-ttu-id="f12e2-122">В приложениях ASP.NET Core 2.x, ссылаются на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f12e2-122">In ASP.NET Core 2.x apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="f12e2-123">В приложениях .NET Framework, добавьте ссылки на пакет [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="f12e2-123">In .NET Framework apps, add a package reference to [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span></span>
* <span data-ttu-id="f12e2-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Задает общее имя приложения.</span><span class="sxs-lookup"><span data-stu-id="f12e2-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> sets the common app name.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="f12e2-125">Совместное использование файлов cookie проверки подлинности между приложениями ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f12e2-125">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="f12e2-126">При использовании удостоверения ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f12e2-126">When using ASP.NET Core Identity:</span></span>

* <span data-ttu-id="f12e2-127">Ключи защиты данных и имя приложения должно совместно использоваться приложениями.</span><span class="sxs-lookup"><span data-stu-id="f12e2-127">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="f12e2-128">Общую область хранения ключей предоставляется <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> метод в следующих примерах.</span><span class="sxs-lookup"><span data-stu-id="f12e2-128">A common key storage location is provided to the <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> method in the following examples.</span></span> <span data-ttu-id="f12e2-129">Используйте <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Настройка общего имени общего приложения (`SharedCookieApp` в следующих примерах).</span><span class="sxs-lookup"><span data-stu-id="f12e2-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> to configure a common shared app name (`SharedCookieApp` in the following examples).</span></span> <span data-ttu-id="f12e2-130">Дополнительные сведения см. в разделе <xref:security/data-protection/configuration/overview>.</span><span class="sxs-lookup"><span data-stu-id="f12e2-130">For more information, see <xref:security/data-protection/configuration/overview>.</span></span>
* <span data-ttu-id="f12e2-131">Используйте <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> метод расширения, чтобы настроить службу защиты данных для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="f12e2-131">Use the <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> extension method to set up the data protection service for cookies.</span></span>
* <span data-ttu-id="f12e2-132">В следующем примере присваивается тип проверки подлинности `Identity.Application` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f12e2-132">In the following example, the authentication type is set to `Identity.Application` by default.</span></span>

<span data-ttu-id="f12e2-133">В `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f12e2-133">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

<span data-ttu-id="f12e2-134">При использовании файлов cookie без ASP.NET Core Identity, настроить защиту данных и проверку подлинности в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f12e2-134">When using cookies directly without ASP.NET Core Identity, configure data protection and authentication in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f12e2-135">В следующем примере присваивается тип проверки подлинности `Identity.Application`:</span><span class="sxs-lookup"><span data-stu-id="f12e2-135">In the following example, the authentication type is set to `Identity.Application`:</span></span>

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

<span data-ttu-id="f12e2-136">При размещении приложений, которые совместно использовать файлы cookie в поддомены, укажите общий домен в [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) свойство.</span><span class="sxs-lookup"><span data-stu-id="f12e2-136">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) property.</span></span> <span data-ttu-id="f12e2-137">Для совместного использования файлов cookie в приложениях в `contoso.com`, такие как `first_subdomain.contoso.com` и `second_subdomain.contoso.com`, укажите `Cookie.Domain` как `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="f12e2-137">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a><span data-ttu-id="f12e2-138">Шифрование неактивных ключей защиты данных</span><span class="sxs-lookup"><span data-stu-id="f12e2-138">Encrypt data protection keys at rest</span></span>

<span data-ttu-id="f12e2-139">Для развертывания в рабочей среде, настройте `DataProtectionProvider` для шифрования ключей при хранении с помощью DPAPI или X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="f12e2-139">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="f12e2-140">Дополнительные сведения см. в разделе <xref:security/data-protection/implementation/key-encryption-at-rest>.</span><span class="sxs-lookup"><span data-stu-id="f12e2-140">For more information, see <xref:security/data-protection/implementation/key-encryption-at-rest>.</span></span> <span data-ttu-id="f12e2-141">В следующем примере предоставляется отпечаток сертификата <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span><span class="sxs-lookup"><span data-stu-id="f12e2-141">In the following example, a certificate thumbprint is provided to <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span></span>

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="f12e2-142">Совместное использование файлов cookie проверки подлинности ASP.NET 4.x и приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f12e2-142">Share authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="f12e2-143">Для создания файлов cookie проверки подлинности, которые совместимы с по промежуточного слоя ASP.NET Core файл Cookie проверки подлинности можно настроить приложений ASP.NET 4.x, использующих Katana промежуточного по проверки подлинности файла Cookie.</span><span class="sxs-lookup"><span data-stu-id="f12e2-143">ASP.NET 4.x apps that use Katana Cookie Authentication Middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core Cookie Authentication Middleware.</span></span> <span data-ttu-id="f12e2-144">Благодаря этому обновлении некий обширный узел отдельных приложений, за несколько шагов то же время предоставляя беспроблемную работу единого входа в рамках всего сайта.</span><span class="sxs-lookup"><span data-stu-id="f12e2-144">This allows upgrading a large site's individual apps in several steps while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="f12e2-145">Когда приложение использует Katana промежуточного по проверки подлинности файла Cookie, он вызывает `UseCookieAuthentication` в проекте *Startup.Auth.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="f12e2-145">When an app uses Katana Cookie Authentication Middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="f12e2-146">Приложения веб-проектов ASP.NET 4.x созданные в Visual Studio 2013 и более поздней версии используйте по промежуточного слоя Katana файл Cookie проверки подлинности по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f12e2-146">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana Cookie Authentication Middleware by default.</span></span> <span data-ttu-id="f12e2-147">Несмотря на то что `UseCookieAuthentication` устарел и не поддерживается для приложений ASP.NET Core, вызвав `UseCookieAuthentication` в ASP.NET 4.x приложение, использующее Katana промежуточного по проверки подлинности файла Cookie является допустимым.</span><span class="sxs-lookup"><span data-stu-id="f12e2-147">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana Cookie Authentication Middleware is valid.</span></span>

<span data-ttu-id="f12e2-148">Приложение ASP.NET 4.x необходимо предназначено для .NET Framework 4.5.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="f12e2-148">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="f12e2-149">В противном случае необходимые пакеты NuGet не удается установить.</span><span class="sxs-lookup"><span data-stu-id="f12e2-149">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="f12e2-150">Для совместного использования файлов cookie проверки подлинности между приложения ASP.NET 4.x и приложения ASP.NET Core, настройте приложение ASP.NET Core, как указано в [совместно использовать файлы cookie проверки подлинности между приложениями ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) раздела, а затем настройте приложение ASP.NET 4.x в качестве следующим образом.</span><span class="sxs-lookup"><span data-stu-id="f12e2-150">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated in the [Share authentication cookies among ASP.NET Core apps](#share-authentication-cookies-among-aspnet-core-apps) section, then configure the ASP.NET 4.x app as follows.</span></span>

<span data-ttu-id="f12e2-151">Убедитесь, что пакеты приложения, обновляются до последней версии.</span><span class="sxs-lookup"><span data-stu-id="f12e2-151">Confirm that the app's packages are updated to the latest releases.</span></span> <span data-ttu-id="f12e2-152">Установка [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) пакет в каждое приложение ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="f12e2-152">Install the [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package into each ASP.NET 4.x app.</span></span>

<span data-ttu-id="f12e2-153">Найдите и измените вызов `UseCookieAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="f12e2-153">Locate and modify the call to `UseCookieAuthentication`:</span></span>

* <span data-ttu-id="f12e2-154">Измените имя файла cookie, чтобы оно соответствовало имени, используемые по промежуточного слоя ASP.NET Core файл Cookie проверки подлинности (`.AspNet.SharedCookie` в примере).</span><span class="sxs-lookup"><span data-stu-id="f12e2-154">Change the cookie name to match the name used by the ASP.NET Core Cookie Authentication Middleware (`.AspNet.SharedCookie` in the example).</span></span>
* <span data-ttu-id="f12e2-155">В следующем примере присваивается тип проверки подлинности `Identity.Application`.</span><span class="sxs-lookup"><span data-stu-id="f12e2-155">In the following example, the authentication type is set to `Identity.Application`.</span></span>
* <span data-ttu-id="f12e2-156">Предоставляет экземпляр `DataProtectionProvider` инициализирован в общую папку хранилища ключей защиты данных.</span><span class="sxs-lookup"><span data-stu-id="f12e2-156">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>
* <span data-ttu-id="f12e2-157">Убедитесь, что имя приложения имеет значение с общим именем приложения, используемый для всех приложений, которые совместно используют файлы cookie проверки подлинности (`SharedCookieApp` в примере).</span><span class="sxs-lookup"><span data-stu-id="f12e2-157">Confirm that the app name is set to the common app name used by all apps that share authentication cookies (`SharedCookieApp` in the example).</span></span>

<span data-ttu-id="f12e2-158">Если не задан `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` и `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, задайте <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> на утверждение, являющийся отличительным признаком уникальных пользователей.</span><span class="sxs-lookup"><span data-stu-id="f12e2-158">If not setting `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` and `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, set <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> to a claim that distinguishes unique users.</span></span>

<span data-ttu-id="f12e2-159">*App_Start/Startup.AUTH.cs*:</span><span class="sxs-lookup"><span data-stu-id="f12e2-159">*App_Start/Startup.Auth.cs*:</span></span>

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

<span data-ttu-id="f12e2-160">При создании удостоверения пользователя, тип проверки подлинности (`Identity.Application`) должен совпадать с типом, определенные в `AuthenticationType` набор с `UseCookieAuthentication` в *App_Start/Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="f12e2-160">When generating a user identity, the authentication type (`Identity.Application`) must match the type defined in `AuthenticationType` set with `UseCookieAuthentication` in *App_Start/Startup.Auth.cs*.</span></span>

<span data-ttu-id="f12e2-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="f12e2-161">*Models/IdentityModels.cs*:</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="f12e2-162">Использование общей базы данных пользователя</span><span class="sxs-lookup"><span data-stu-id="f12e2-162">Use a common user database</span></span>

<span data-ttu-id="f12e2-163">Когда приложения использовали один идентификатор схемы (та же версия удостоверения), убедитесь, что система идентификации для каждого приложения указывает на одну и ту же базу данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="f12e2-163">When apps use the same Identity schema (same version of Identity), confirm that the Identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="f12e2-164">В противном случае система идентификации дает сбои во время выполнения, если предпринимается попытка сопоставить сведения в файл cookie проверки подлинности с информацией в своей базе данных.</span><span class="sxs-lookup"><span data-stu-id="f12e2-164">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

<span data-ttu-id="f12e2-165">При идентификации схема отличается между приложениями, обычно потому, что приложения, используют разные версии Identity, совместного использования общей базы данных на основе последней версии Identity невозможны без повторное сопоставление и добавления столбцов в схемах удостоверений других приложений.</span><span class="sxs-lookup"><span data-stu-id="f12e2-165">When the Identity schema is different among apps, usually because apps are using different Identity versions, sharing a common database based on the latest version of Identity isn't possible without remapping and adding columns in other app's Identity schemas.</span></span> <span data-ttu-id="f12e2-166">Часто бывает более эффективно для обновления в других приложениях, чтобы использовать последнюю версию удостоверений, таким образом, чтобы общей базы данных может совместно использоваться приложениями.</span><span class="sxs-lookup"><span data-stu-id="f12e2-166">It's often more efficient to upgrade the other apps to use the latest Identity version so that a common database can be shared by the apps.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f12e2-167">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f12e2-167">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
