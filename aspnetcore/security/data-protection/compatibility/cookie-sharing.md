---
title: "Совместное использование файлов cookie между приложениями"
author: rick-anderson
description: "Дополнительные сведения о совместном использовании файлов cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e87caa5ba78c6b4c365facc0dea07d747e7c9589
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="sharing-cookies-among-apps"></a><span data-ttu-id="70c57-103">Совместное использование файлов cookie между приложениями</span><span class="sxs-lookup"><span data-stu-id="70c57-103">Sharing cookies among apps</span></span>

<span data-ttu-id="70c57-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Latham Люк](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="70c57-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="70c57-105">Веб-сайтов часто состоят из отдельных веб-приложений, работающих вместе.</span><span class="sxs-lookup"><span data-stu-id="70c57-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="70c57-106">Для обеспечения единого входа (SSO), веб-приложений в пределах сайта должны совместно использовать файлы cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="70c57-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="70c57-107">Для поддержки этого сценария, стека защиты данных позволяет совместное использование проверки подлинности файла cookie Katana и ASP.NET Core билетов проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="70c57-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="70c57-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="70c57-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="70c57-109">В образце показано совместное использование три приложения, использующие файл cookie проверки подлинности файла cookie:</span><span class="sxs-lookup"><span data-stu-id="70c57-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="70c57-110">Приложения ASP.NET Core страниц Razor 2.0 без использования [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="70c57-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="70c57-111">Приложение MVC ASP.NET Core 2.0 с удостоверением ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70c57-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="70c57-112">Приложение MVC ASP.NET Framework 4.6.1 с ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="70c57-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="70c57-113">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="70c57-113">In the examples that follow:</span></span>

* <span data-ttu-id="70c57-114">Общее значение для имени файла cookie проверки подлинности имеет значение `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="70c57-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="70c57-115">`AuthenticationType` Равно `Identity.Application` явно или по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="70c57-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="70c57-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) используется в качестве схемы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="70c57-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) is used as the authentication scheme.</span></span> <span data-ttu-id="70c57-117">Константа разрешается в значение `Cookies`.</span><span class="sxs-lookup"><span data-stu-id="70c57-117">The constant resolves to a value of `Cookies`.</span></span>
* <span data-ttu-id="70c57-118">Файл cookie проверки подлинности по промежуточного слоя использует реализацию [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="70c57-118">The cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="70c57-119">`DataProtectionProvider`предоставляет службы защиты данных для шифрования и расшифровки данных полезные данные файлов cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="70c57-119">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="70c57-120">`DataProtectionProvider` Экземпляр изолирован от системы защиты данных, используемые в других частях приложения.</span><span class="sxs-lookup"><span data-stu-id="70c57-120">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="70c57-121">Обычное [ключа защиты данных](xref:security/data-protection/implementation/key-management) используется расположение хранилища.</span><span class="sxs-lookup"><span data-stu-id="70c57-121">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="70c57-122">В примере приложения используется папка с именем *KeyRing* в корень решения для хранения ключей защиты данных.</span><span class="sxs-lookup"><span data-stu-id="70c57-122">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
  * <span data-ttu-id="70c57-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) принимает [DirectoryInfo](/dotnet/api/system.io.directoryinfo) для использования с файлы cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="70c57-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) for use with authentication cookies.</span></span> <span data-ttu-id="70c57-124">Пример приложения предоставляет путь *KeyRing* папки `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="70c57-124">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span>
  * <span data-ttu-id="70c57-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) требует [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="70c57-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="70c57-126">Чтобы получить этот пакет для основных ASP.NET 2.0 и более поздних версий приложений, ссылаются на [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="70c57-126">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="70c57-127">При разработке для .NET Framework, добавьте ссылку на пакет `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="70c57-127">When targeting .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="70c57-128">Совместно использовать файлы cookie проверки подлинности между приложениями ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70c57-128">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="70c57-129">При использовании ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="70c57-129">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="70c57-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="70c57-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="70c57-131">В `ConfigureServices` используйте [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) метод расширения, чтобы настроить службу защиты данных файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="70c57-131">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="70c57-132">В разделе *CookieAuthWithIdentity.Core* проекта в [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="70c57-132">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="70c57-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="70c57-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="70c57-134">В `Configure` используйте [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) для настройки:</span><span class="sxs-lookup"><span data-stu-id="70c57-134">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="70c57-135">Служба защиты данных файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="70c57-135">The data protection service for cookies.</span></span>
* <span data-ttu-id="70c57-136">`AuthenticationScheme` Для сопоставления ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="70c57-136">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="70c57-137">При использовании файлов cookie напрямую:</span><span class="sxs-lookup"><span data-stu-id="70c57-137">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="70c57-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="70c57-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="70c57-139">В разделе *CookieAuth.Core* проекта в [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="70c57-139">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="70c57-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="70c57-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="70c57-141">Шифрование ключей защиты данных на rest</span><span class="sxs-lookup"><span data-stu-id="70c57-141">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="70c57-142">В производственной среде, настройте `DataProtectionProvider` для шифрования ключей при хранении с помощью DPAPI или X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="70c57-142">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="70c57-143">В разделе [ключ шифрования на Rest](xref:security/data-protection/implementation/key-encryption-at-rest) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="70c57-143">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="70c57-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="70c57-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="70c57-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="70c57-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="70c57-146">Совместное использование файлов cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70c57-146">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="70c57-147">Для создания файлов cookie проверки подлинности, которые совместимы с по промежуточного слоя ASP.NET Core файла cookie проверки подлинности можно настроить приложения ASP.NET 4.x, которые используют Katana промежуточного по проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="70c57-147">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="70c57-148">Это позволяет поэтапное обновление большого узла отдельными приложениями то же время предоставляя smooth службы Единого входа в пределах сайта.</span><span class="sxs-lookup"><span data-stu-id="70c57-148">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="70c57-149">Если приложение использует Katana промежуточного по проверки подлинности файла cookie, он вызывает `UseCookieAuthentication` в проекте *Startup.Auth.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="70c57-149">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="70c57-150">Приложения веб-проектов ASP.NET 4.x создан в Visual Studio 2013 и более поздней версии, по умолчанию используют промежуточного по проверки подлинности файла cookie Katana.</span><span class="sxs-lookup"><span data-stu-id="70c57-150">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="70c57-151">Приложение ASP.NET 4.x должны предназначаться для платформы .NET Framework 4.5.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="70c57-151">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="70c57-152">В противном случае необходимые пакеты NuGet не удалось установить.</span><span class="sxs-lookup"><span data-stu-id="70c57-152">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="70c57-153">Для совместного использования файлов cookie проверки подлинности между ASP.NET 4.x приложений и приложений ASP.NET Core, настроить приложение ASP.NET Core, как упоминалось выше, а затем настройте приложения ASP.NET 4.x, выполнив указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="70c57-153">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="70c57-154">Установите пакет [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) в каждое приложение ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="70c57-154">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="70c57-155">В *Startup.Auth.cs*, найдите вызов `UseCookieAuthentication` и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="70c57-155">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="70c57-156">Измените имя файла cookie, должно соответствовать имени, используемые по промежуточного слоя ASP.NET Core файла cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="70c57-156">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="70c57-157">Предоставляет экземпляр `DataProtectionProvider` инициализирован в общую папку хранилища ключей защиты данных.</span><span class="sxs-lookup"><span data-stu-id="70c57-157">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="70c57-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="70c57-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="70c57-159">В разделе *CookieAuthWithIdentity.NETFramework* проекта в [пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="70c57-159">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="70c57-160">При создании удостоверения пользователя, типа проверки подлинности должен соответствовать типу, определенные в `AuthenticationType` с `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="70c57-160">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="70c57-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="70c57-161">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="70c57-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="70c57-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="70c57-163">Задать `CookieManager` с взаимодействием `ChunkingCookieManager` , совместима неверный формат.</span><span class="sxs-lookup"><span data-stu-id="70c57-163">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="70c57-164">Использование общей базы данных пользователя</span><span class="sxs-lookup"><span data-stu-id="70c57-164">Use a common user database</span></span>

<span data-ttu-id="70c57-165">Убедитесь, что система идентификаторов для каждого приложения указывает на одной и той же базе данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="70c57-165">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="70c57-166">В противном случае система идентификации выводятся ошибки во время выполнения при попытке соответствует сведениям в файл cookie проверки подлинности со сведениями в своей базе данных.</span><span class="sxs-lookup"><span data-stu-id="70c57-166">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
