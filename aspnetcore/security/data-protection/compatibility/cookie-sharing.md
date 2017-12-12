---
title: "Совместное использование файлов cookie между приложениями"
author: rick-anderson
description: "В этом документе объясняется, как стек защиты данных поддерживает совместное использование файлов cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core."
keywords: "Совместное использование Core,ASP.NET,cookies,Interop,cookie ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e92cc81f9362787b7b4bfb44ba26b82182826a59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="7a7de-104">Совместное использование файлов cookie между приложениями</span><span class="sxs-lookup"><span data-stu-id="7a7de-104">Sharing cookies between applications</span></span>

<span data-ttu-id="7a7de-105">Веб-сайтов обычно состоят из многих отдельных веб-приложениях, все работает вместе harmoniously.</span><span class="sxs-lookup"><span data-stu-id="7a7de-105">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="7a7de-106">Если разработчику приложения для обеспечения эффективной работы единый вход, они часто понадобятся все различные веб-приложения на узле для совместного использования билетов проверки подлинности между друг с другом.</span><span class="sxs-lookup"><span data-stu-id="7a7de-106">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="7a7de-107">Для поддержки этого сценария, стека защиты данных позволяет совместное использование проверки подлинности файла cookie Katana и ASP.NET Core билетов проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="7a7de-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="7a7de-108">Совместное использование файлов cookie проверки подлинности между приложениями</span><span class="sxs-lookup"><span data-stu-id="7a7de-108">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="7a7de-109">Для совместного использования файлов cookie проверки подлинности между двумя приложениями ASP.NET Core, настройка каждого приложения, которое следует совместно использовать файлы cookie следующим образом.</span><span class="sxs-lookup"><span data-stu-id="7a7de-109">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="7a7de-110">В вашей настройки метода, используйте CookieAuthenticationOptions, чтобы настроить службу защиты данных файлов cookie и AuthenticationScheme для сопоставления ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="7a7de-110">In your configure method, use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.x.</span></span>

<span data-ttu-id="7a7de-111">Если вы используете удостоверений:</span><span class="sxs-lookup"><span data-stu-id="7a7de-111">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

<span data-ttu-id="7a7de-112">Если вы используете файлы cookie напрямую:</span><span class="sxs-lookup"><span data-stu-id="7a7de-112">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
<span data-ttu-id="7a7de-113">`DataProtectionProvider` Требует `Microsoft.AspNetCore.DataProtection.Extensions` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="7a7de-113">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="7a7de-114">При использовании таким образом DirectoryInfo должны указывать на расположение хранилища ключей, специально предназначенные для файлов cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="7a7de-114">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="7a7de-115">Файл cookie проверки подлинности по промежуточного слоя будет использовать явно предоставленных реализации DataProtectionProvider, который теперь является изолированной из системы защиты данных, используемые в других частях приложения.</span><span class="sxs-lookup"><span data-stu-id="7a7de-115">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="7a7de-116">Имя приложения учитывается (намеренно так, как вы пытаетесь получить несколько приложений, для совместного использования полезных данных).</span><span class="sxs-lookup"><span data-stu-id="7a7de-116">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="7a7de-117">Следует подумать о настройке DataProtectionProvider таким образом, что они шифруются при хранении, как в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="7a7de-117">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="7a7de-118">Совместное использование файлов cookie проверки подлинности между ASP.NET 4.x и приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a7de-118">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="7a7de-119">Для создания файлов cookie проверки подлинности, которые совместимы с по промежуточного слоя ASP.NET Core файла cookie проверки подлинности можно настроить приложения ASP.NET 4.x, которые используют Katana промежуточного по проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="7a7de-119">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="7a7de-120">Это позволяет поэтапное обновление отдельных приложений большой узел одновременно обеспечивая smooth единого входа на взаимодействие на сайте.</span><span class="sxs-lookup"><span data-stu-id="7a7de-120">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="7a7de-121">Чтобы узнать, использует ли текущее приложение Katana промежуточного по проверки подлинности файла cookie, существование вызов UseCookieAuthentication в Startup.Auth.cs проекта.</span><span class="sxs-lookup"><span data-stu-id="7a7de-121">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="7a7de-122">Проекты веб-приложений ASP.NET 4.x создан в Visual Studio 2013 и более поздней версии, по умолчанию используют промежуточного по проверки подлинности файла cookie Katana.</span><span class="sxs-lookup"><span data-stu-id="7a7de-122">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="7a7de-123">Приложение ASP.NET 4.x необходимо использовать .NET Framework 4.5.1 или более поздней версии, в противном случае необходимые пакеты NuGet не удастся установить.</span><span class="sxs-lookup"><span data-stu-id="7a7de-123">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="7a7de-124">Для совместного использования файлов cookie проверки подлинности между 4.x приложений ASP.NET и приложений ASP.NET Core, настроить приложение ASP.NET Core, как упоминалось выше, а затем настройте приложений ASP.NET 4.x, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="7a7de-124">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="7a7de-125">Установка пакета Microsoft.Owin.Security.Interop в каждое приложение ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="7a7de-125">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="7a7de-126">В Startup.Auth.cs найдите вызов UseCookieAuthentication, который обычно будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="7a7de-126">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="7a7de-127">Измените вызов UseCookieAuthentication следующим образом, изменения CookieName должно соответствовать имени, используемого ASP.NET Core файла cookie проверки подлинности по промежуточного слоя, предоставления экземпляра DataProtectionProvider, который был инициализирован в расположение хранилища ключей и значение CookieManager взаимодействия диспетчер chunkingcookiemanager, который, поэтому совместим неверный формат.</span><span class="sxs-lookup"><span data-stu-id="7a7de-127">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    <span data-ttu-id="7a7de-128">Чтобы она указывала на том же месте хранения, указывает приложению ASP.NET Core и должны быть настроены с одинаковыми параметрами имеет DirectoryInfo.</span><span class="sxs-lookup"><span data-stu-id="7a7de-128">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="7a7de-129">ASP.NET 4.x и приложения ASP.NET Core теперь настроены для совместного использования файлов cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="7a7de-129">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="7a7de-130">Вам потребуется, чтобы убедиться в том, что система идентификаторов для каждого приложения указывает на одной и той же базе данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="7a7de-130">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="7a7de-131">В противном случае система идентификации вызовет сбои во время выполнения при попытке соответствует сведениям в файл cookie проверки подлинности со сведениями в своей базе данных.</span><span class="sxs-lookup"><span data-stu-id="7a7de-131">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>
