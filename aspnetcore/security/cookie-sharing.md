---
title: Совместно использовать файлы cookie для приложений с помощью ASP.NET и ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о совместном использовании файлов cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: 2636688fa50fb36a8cbd07549e8038474ffa30ca
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278470"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="45b4d-103">Совместно использовать файлы cookie для приложений с помощью ASP.NET и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45b4d-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="45b4d-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Latham Люк](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="45b4d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="45b4d-105">Веб-сайтов часто состоят из отдельных веб-приложений, работающих вместе.</span><span class="sxs-lookup"><span data-stu-id="45b4d-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="45b4d-106">Для обеспечения единого входа (SSO), веб-приложений в пределах сайта должны совместно использовать файлы cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="45b4d-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="45b4d-107">Для поддержки этого сценария, стека защиты данных позволяет совместное использование проверки подлинности файла cookie Katana и ASP.NET Core билетов проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="45b4d-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="45b4d-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="45b4d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="45b4d-109">В образце показано совместное использование три приложения, использующие файл cookie проверки подлинности файла cookie:</span><span class="sxs-lookup"><span data-stu-id="45b4d-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="45b4d-110">Приложения ASP.NET Core страниц Razor 2.0 без использования [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="45b4d-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="45b4d-111">Приложение MVC ASP.NET Core 2.0 с удостоверением ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45b4d-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="45b4d-112">Приложение MVC ASP.NET Framework 4.6.1 с ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="45b4d-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="45b4d-113">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="45b4d-113">In the examples that follow:</span></span>

* <span data-ttu-id="45b4d-114">Общее значение для имени файла cookie проверки подлинности имеет значение `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="45b4d-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="45b4d-115">`AuthenticationType` Равно `Identity.Application` явно или по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="45b4d-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="45b4d-116">Общее имя приложения используется для включения системы защиты данных для совместного использования ключей для защиты данных (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="45b4d-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="45b4d-117">`Identity.Application` используется схема проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="45b4d-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="45b4d-118">Независимо от схемы используется, он должен постоянно использоваться *внутри и между* общего файла cookie приложения, как схема по умолчанию или путем явного задания.</span><span class="sxs-lookup"><span data-stu-id="45b4d-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="45b4d-119">Схема используется при шифровании и расшифровки куки-файлов, поэтому необходимо использовать согласованную схему для приложений.</span><span class="sxs-lookup"><span data-stu-id="45b4d-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="45b4d-120">Обычное [ключа защиты данных](xref:security/data-protection/implementation/key-management) используется расположение хранилища.</span><span class="sxs-lookup"><span data-stu-id="45b4d-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="45b4d-121">В примере приложения используется папка с именем *KeyRing* в корень решения для хранения ключей защиты данных.</span><span class="sxs-lookup"><span data-stu-id="45b4d-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="45b4d-122">В приложениях ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) используется для задания расположения хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="45b4d-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="45b4d-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) используется для настройки общее имя общего приложения.</span><span class="sxs-lookup"><span data-stu-id="45b4d-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="45b4d-124">В приложении .NET Framework, файл cookie проверки подлинности по промежуточного слоя использует реализацию [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="45b4d-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="45b4d-125">`DataProtectionProvider` предоставляет службы защиты данных для шифрования и расшифровки данных полезные данные файлов cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="45b4d-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="45b4d-126">`DataProtectionProvider` Экземпляр изолирован от системы защиты данных, используемые в других частях приложения.</span><span class="sxs-lookup"><span data-stu-id="45b4d-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="45b4d-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, действие\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) принимает [DirectoryInfo](/dotnet/api/system.io.directoryinfo) для указания расположения для хранения ключей для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="45b4d-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="45b4d-128">Пример приложения предоставляет путь *KeyRing* папки `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="45b4d-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="45b4d-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) задает общее имя приложения.</span><span class="sxs-lookup"><span data-stu-id="45b4d-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="45b4d-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) требует [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="45b4d-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="45b4d-131">Чтобы получить этот пакет для ASP.NET Core 2.1 и более поздних версий приложений, ссылаются на [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="45b4d-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="45b4d-132">При разработке для .NET Framework, добавьте ссылку на пакет `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="45b4d-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="45b4d-133">Совместно использовать файлы cookie проверки подлинности между приложениями ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45b4d-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="45b4d-134">При использовании ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="45b4d-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="45b4d-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="45b4d-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="45b4d-136">В `ConfigureServices` используйте [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) метод расширения, чтобы настроить службу защиты данных файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="45b4d-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="45b4d-137">Ключи защиты данных и имя приложения должны быть распределены между приложениями.</span><span class="sxs-lookup"><span data-stu-id="45b4d-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="45b4d-138">В примерах приложений `GetKeyRingDirInfo` возвращает расположение общей хранилища ключей для [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) метод.</span><span class="sxs-lookup"><span data-stu-id="45b4d-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="45b4d-139">Используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) Настройка общее имя общего приложения (`SharedCookieApp` в образце).</span><span class="sxs-lookup"><span data-stu-id="45b4d-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="45b4d-140">Дополнительные сведения см. в разделе [настройке защиты данных](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="45b4d-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="45b4d-141">В разделе *CookieAuthWithIdentity.Core* проекта в [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="45b4d-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="45b4d-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="45b4d-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="45b4d-143">В `Configure` используйте [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) для настройки:</span><span class="sxs-lookup"><span data-stu-id="45b4d-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="45b4d-144">Служба защиты данных файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="45b4d-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="45b4d-145">`AuthenticationScheme` Для сопоставления ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="45b4d-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="45b4d-146">При использовании файлов cookie напрямую:</span><span class="sxs-lookup"><span data-stu-id="45b4d-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="45b4d-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="45b4d-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="45b4d-148">Ключи защиты данных и имя приложения должны быть распределены между приложениями.</span><span class="sxs-lookup"><span data-stu-id="45b4d-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="45b4d-149">В примерах приложений `GetKeyRingDirInfo` возвращает расположение общей хранилища ключей для [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) метод.</span><span class="sxs-lookup"><span data-stu-id="45b4d-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="45b4d-150">Используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) Настройка общее имя общего приложения (`SharedCookieApp` в образце).</span><span class="sxs-lookup"><span data-stu-id="45b4d-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="45b4d-151">Дополнительные сведения см. в разделе [настройке защиты данных](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="45b4d-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="45b4d-152">В разделе *CookieAuth.Core* проекта в [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="45b4d-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="45b4d-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="45b4d-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="45b4d-154">Шифрование ключей защиты данных на rest</span><span class="sxs-lookup"><span data-stu-id="45b4d-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="45b4d-155">В производственной среде, настройте `DataProtectionProvider` для шифрования ключей при хранении с помощью DPAPI или X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="45b4d-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="45b4d-156">В разделе [ключ шифрования на Rest](xref:security/data-protection/implementation/key-encryption-at-rest) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="45b4d-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="45b4d-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="45b4d-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="45b4d-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="45b4d-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="45b4d-159">Совместное использование файлов cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45b4d-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="45b4d-160">Для создания файлов cookie проверки подлинности, которые совместимы с по промежуточного слоя ASP.NET Core файла cookie проверки подлинности можно настроить приложения ASP.NET 4.x, которые используют Katana промежуточного по проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="45b4d-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="45b4d-161">Это позволяет поэтапное обновление большого узла отдельными приложениями то же время предоставляя smooth службы Единого входа в пределах сайта.</span><span class="sxs-lookup"><span data-stu-id="45b4d-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="45b4d-162">Если приложение использует Katana промежуточного по проверки подлинности файла cookie, он вызывает `UseCookieAuthentication` в проекте *Startup.Auth.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="45b4d-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="45b4d-163">Приложения веб-проектов ASP.NET 4.x создан в Visual Studio 2013 и более поздней версии, по умолчанию используют промежуточного по проверки подлинности файла cookie Katana.</span><span class="sxs-lookup"><span data-stu-id="45b4d-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="45b4d-164">Несмотря на то что `UseCookieAuthentication` устарел и не поддерживается для приложений ASP.NET Core, вызвав `UseCookieAuthentication` в приложении ASP.NET 4.x, использующий Katana промежуточного по проверки подлинности файла cookie является допустимым.</span><span class="sxs-lookup"><span data-stu-id="45b4d-164">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="45b4d-165">Приложение ASP.NET 4.x должны предназначаться для платформы .NET Framework 4.5.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="45b4d-165">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="45b4d-166">В противном случае необходимые пакеты NuGet не удалось установить.</span><span class="sxs-lookup"><span data-stu-id="45b4d-166">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="45b4d-167">Для совместного использования файлов cookie проверки подлинности между 4.x приложения ASP.NET и приложения ASP.NET Core, настроить приложения ASP.NET Core, как упоминалось выше, а затем настроить приложение ASP.NET 4.x, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="45b4d-167">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="45b4d-168">Установите пакет [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) в каждое приложение ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="45b4d-168">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="45b4d-169">В *Startup.Auth.cs*, найдите вызов `UseCookieAuthentication` и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="45b4d-169">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="45b4d-170">Измените имя файла cookie, должно соответствовать имени, используемые по промежуточного слоя ASP.NET Core файла cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="45b4d-170">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="45b4d-171">Предоставляет экземпляр `DataProtectionProvider` инициализирован в общую папку хранилища ключей защиты данных.</span><span class="sxs-lookup"><span data-stu-id="45b4d-171">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="45b4d-172">Убедитесь, что имя приложения присвоено общее имя приложения, используемые все приложения, которые совместно использовать файлы cookie, `SharedCookieApp` в пример приложения.</span><span class="sxs-lookup"><span data-stu-id="45b4d-172">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="45b4d-173">В разделе *CookieAuthWithIdentity.NETFramework* проекта в [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="45b4d-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="45b4d-174">При создании удостоверения пользователя, типа проверки подлинности должен соответствовать типу, определенные в `AuthenticationType` с `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="45b4d-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="45b4d-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="45b4d-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="45b4d-176">Использование общей базы данных пользователя</span><span class="sxs-lookup"><span data-stu-id="45b4d-176">Use a common user database</span></span>

<span data-ttu-id="45b4d-177">Убедитесь, что система идентификаторов для каждого приложения указывает на одной и той же базе данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="45b4d-177">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="45b4d-178">В противном случае система идентификации выводятся ошибки во время выполнения при попытке соответствует сведениям в файл cookie проверки подлинности со сведениями в своей базе данных.</span><span class="sxs-lookup"><span data-stu-id="45b4d-178">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
