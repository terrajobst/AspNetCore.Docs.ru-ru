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
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="2535c-103">Совместное использование приложениями файлов cookie с помощью ASP.NET и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2535c-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="2535c-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2535c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2535c-105">Веб-сайтов часто состоят из отдельных веб-приложений, работающих вместе.</span><span class="sxs-lookup"><span data-stu-id="2535c-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="2535c-106">Для работы единого входа (SSO), веб-приложений в пределах сайта должны совместно использовать файлы cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2535c-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="2535c-107">Для поддержки этого сценария, в стеке защиты данных позволяет совместное использование проверки подлинности файла cookie Katana и билеты проверки подлинности файла cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2535c-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="2535c-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2535c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2535c-109">В примере показано совместное использование трех приложениях, использующих файл cookie проверки подлинности файла cookie:</span><span class="sxs-lookup"><span data-stu-id="2535c-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="2535c-110">Приложения ASP.NET Core 2.0 Razor Pages без использования [удостоверения ASP.NET Core](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="2535c-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="2535c-111">Приложение ASP.NET Core 2.0 MVC с помощью ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="2535c-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="2535c-112">Приложения ASP.NET Framework 4.6.1 MVC в ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="2535c-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="2535c-113">В приведенных ниже примерах:</span><span class="sxs-lookup"><span data-stu-id="2535c-113">In the examples that follow:</span></span>

* <span data-ttu-id="2535c-114">Имя файла cookie проверки подлинности имеет значение к общему значению `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="2535c-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="2535c-115">`AuthenticationType` Присваивается `Identity.Application` явно или по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2535c-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="2535c-116">Общее имя приложения используется для включения система защиты данных для совместного использования ключей защиты данных (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="2535c-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="2535c-117">`Identity.Application` используется как схему проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2535c-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="2535c-118">Используется независимо от схемы, он должен использоваться согласованно *внутри и между* общего файла cookie приложения, как схема по умолчанию или установив его явным образом.</span><span class="sxs-lookup"><span data-stu-id="2535c-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="2535c-119">Схема используется при шифровании и расшифровки куки-файлов, поэтому необходимо использовать согласованную схему в различных приложениях.</span><span class="sxs-lookup"><span data-stu-id="2535c-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="2535c-120">Общий [ключа защиты данных](xref:security/data-protection/implementation/key-management) используется место хранения.</span><span class="sxs-lookup"><span data-stu-id="2535c-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="2535c-121">Пример приложения использует папку с именем *Брелок* в корне решения для хранения ключи защиты данных.</span><span class="sxs-lookup"><span data-stu-id="2535c-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="2535c-122">В приложениях ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) используется для указания расположения хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="2535c-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="2535c-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) используется для настройки общего имени общего приложения.</span><span class="sxs-lookup"><span data-stu-id="2535c-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="2535c-124">В приложении .NET Framework, по промежуточного слоя проверки подлинности использует реализацию [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="2535c-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="2535c-125">`DataProtectionProvider` предоставляет службы защиты данных для шифрования и расшифровки данных полезные данные файлов cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2535c-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="2535c-126">`DataProtectionProvider` Экземпляр изолирован от система защиты данных, используемых в других частях приложения.</span><span class="sxs-lookup"><span data-stu-id="2535c-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="2535c-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, действие\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) принимает [DirectoryInfo](/dotnet/api/system.io.directoryinfo) для указания расположения для хранения ключей защиты данных.</span><span class="sxs-lookup"><span data-stu-id="2535c-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="2535c-128">В примере приложения приведен путь к *Брелок* папку `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="2535c-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="2535c-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) задает общее имя приложения.</span><span class="sxs-lookup"><span data-stu-id="2535c-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="2535c-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) требует [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="2535c-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="2535c-131">Чтобы получить этот пакет для ASP.NET Core 2.1 и более поздних версий приложений, ссылаются на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2535c-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2535c-132">При нацеливании на .NET Framework, добавьте ссылки на пакет `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="2535c-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="2535c-133">Совместное использование файлов cookie проверки подлинности между приложениями ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2535c-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="2535c-134">При использовании удостоверения ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2535c-134">When using ASP.NET Core Identity:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2535c-135">В `ConfigureServices` используйте [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) метод расширения, чтобы настроить службу защиты данных для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="2535c-135">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2535c-136">Ключи защиты данных и имя приложения должно совместно использоваться приложениями.</span><span class="sxs-lookup"><span data-stu-id="2535c-136">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2535c-137">В образце приложения `GetKeyRingDirInfo` возвращает общие расположение хранилища ключей для [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) метод.</span><span class="sxs-lookup"><span data-stu-id="2535c-137">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2535c-138">Используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) Настройка общего имени общего приложения (`SharedCookieApp` в образце).</span><span class="sxs-lookup"><span data-stu-id="2535c-138">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2535c-139">Дополнительные сведения см. в разделе [Настройка защиты данных](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2535c-139">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="2535c-140">При размещении приложений, которые совместно использовать файлы cookie в поддомены, укажите общий домен в [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) свойство.</span><span class="sxs-lookup"><span data-stu-id="2535c-140">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="2535c-141">Для совместного использования файлов cookie в приложениях в `contoso.com`, такие как `first_subdomain.contoso.com` и `second_subdomain.contoso.com`, укажите `Cookie.Domain` как `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="2535c-141">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="2535c-142">См. в разделе *CookieAuthWithIdentity.Core* в проекте [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([загрузке](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2535c-142">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2535c-143">В `Configure` используйте [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) для настройки:</span><span class="sxs-lookup"><span data-stu-id="2535c-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="2535c-144">Служба защиты данных для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="2535c-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="2535c-145">`AuthenticationScheme` В соответствии с ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="2535c-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="2535c-146">При использовании файлов cookie напрямую:</span><span class="sxs-lookup"><span data-stu-id="2535c-146">When using cookies directly:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2535c-147">Ключи защиты данных и имя приложения должно совместно использоваться приложениями.</span><span class="sxs-lookup"><span data-stu-id="2535c-147">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2535c-148">В образце приложения `GetKeyRingDirInfo` возвращает общие расположение хранилища ключей для [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) метод.</span><span class="sxs-lookup"><span data-stu-id="2535c-148">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2535c-149">Используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) Настройка общего имени общего приложения (`SharedCookieApp` в образце).</span><span class="sxs-lookup"><span data-stu-id="2535c-149">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2535c-150">Дополнительные сведения см. в разделе [Настройка защиты данных](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2535c-150">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="2535c-151">При размещении приложений, которые совместно использовать файлы cookie в поддомены, укажите общий домен в [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) свойство.</span><span class="sxs-lookup"><span data-stu-id="2535c-151">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="2535c-152">Для совместного использования файлов cookie в приложениях в `contoso.com`, такие как `first_subdomain.contoso.com` и `second_subdomain.contoso.com`, укажите `Cookie.Domain` как `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="2535c-152">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="2535c-153">См. в разделе *CookieAuth.Core* в проекте [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([загрузке](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2535c-153">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

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

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="2535c-154">Шифрование неактивных ключей защиты данных</span><span class="sxs-lookup"><span data-stu-id="2535c-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="2535c-155">Для развертывания в рабочей среде, настройте `DataProtectionProvider` для шифрования ключей при хранении с помощью DPAPI или X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="2535c-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="2535c-156">См. в разделе [ключ шифрования неактивных](xref:security/data-protection/implementation/key-encryption-at-rest) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="2535c-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="2535c-157">Совместное использование файлов cookie проверки подлинности ASP.NET 4.x и приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2535c-157">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="2535c-158">Приложения ASP.NET 4.x, которые используют Katana файл cookie проверки подлинности, по промежуточного слоя можно настроить для создания файлов cookie проверки подлинности, которые совместимы с по промежуточного слоя ASP.NET Core файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2535c-158">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2535c-159">Благодаря этому поэтапное обновление отдельных приложений некий обширный узел то же время предоставляя беспроблемную работу единого входа в рамках всего сайта.</span><span class="sxs-lookup"><span data-stu-id="2535c-159">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="2535c-160">Когда приложение использует Katana промежуточного по проверки подлинности файла cookie, он вызывает `UseCookieAuthentication` в проекте *Startup.Auth.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="2535c-160">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="2535c-161">Приложения веб-проектов ASP.NET 4.x созданные в Visual Studio 2013 и более поздней версии используйте по промежуточного слоя Katana файл cookie проверки подлинности по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2535c-161">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="2535c-162">Несмотря на то что `UseCookieAuthentication` устарел и не поддерживается для приложений ASP.NET Core, вызвав `UseCookieAuthentication` в приложении ASP.NET 4.x, которое использует Katana промежуточного по проверки подлинности файла cookie является допустимым.</span><span class="sxs-lookup"><span data-stu-id="2535c-162">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="2535c-163">Приложение ASP.NET 4.x необходимо предназначено для .NET Framework 4.5.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="2535c-163">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="2535c-164">В противном случае необходимые пакеты NuGet не удается установить.</span><span class="sxs-lookup"><span data-stu-id="2535c-164">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="2535c-165">Для совместного использования файлов cookie проверки подлинности между приложения ASP.NET 4.x и приложения ASP.NET Core, настройте приложение ASP.NET Core, как указано выше, а затем настройте приложение ASP.NET 4.x, сделав следующее:</span><span class="sxs-lookup"><span data-stu-id="2535c-165">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="2535c-166">Установите пакет [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) в каждое приложение ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="2535c-166">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="2535c-167">В *Startup.Auth.cs*, найдите вызов `UseCookieAuthentication` и измените его следующим образом.</span><span class="sxs-lookup"><span data-stu-id="2535c-167">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="2535c-168">Измените имя файла cookie, чтобы оно соответствовало имени, используемые по промежуточного слоя ASP.NET Core файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2535c-168">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2535c-169">Предоставляет экземпляр `DataProtectionProvider` инициализирован в общую папку хранилища ключей защиты данных.</span><span class="sxs-lookup"><span data-stu-id="2535c-169">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="2535c-170">Убедитесь, что общее имя приложения, используемое для всех приложений, которые совместно используют файлы cookie, присваивается имя приложения `SharedCookieApp` в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="2535c-170">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="2535c-171">См. в разделе *CookieAuthWithIdentity.NETFramework* в проекте [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([загрузке](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2535c-171">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="2535c-172">При создании удостоверения пользователя, тип проверки подлинности должен соответствовать типу, определенные в `AuthenticationType` набор с `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="2535c-172">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="2535c-173">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="2535c-173">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="2535c-174">Использование общей базы данных пользователя</span><span class="sxs-lookup"><span data-stu-id="2535c-174">Use a common user database</span></span>

<span data-ttu-id="2535c-175">Убедитесь, что система идентификации для каждого приложения указывает на одну и ту же базу данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="2535c-175">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="2535c-176">В противном случае система идентификации дает сбои во время выполнения, если предпринимается попытка сопоставить сведения в файл cookie проверки подлинности с информацией в своей базе данных.</span><span class="sxs-lookup"><span data-stu-id="2535c-176">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2535c-177">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2535c-177">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
