---
title: "Настроить проверку подлинности в ASP.NET Core"
author: ardalis
description: "Как настроить проверку подлинности в ASP.NET Core"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: aa401f956d74680efd3964203af3e8866b129887
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="5eb0c-104">Настроить проверку подлинности в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5eb0c-104">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="5eb0c-105">По [Стив Смит](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="5eb0c-105">By [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="5eb0c-106">Проверка подлинности Windows можно настроить для приложений ASP.NET Core, размещенной в IIS или WebListener.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS or WebListener.</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="5eb0c-107">Что такое проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="5eb0c-107">What is Windows authentication</span></span>

<span data-ttu-id="5eb0c-108">Проверка подлинности Windows зависит от операционной системы, для проверки подлинности пользователей приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="5eb0c-109">При запуске сервера в корпоративной сети с помощью удостоверения домена Active Directory или других учетных записей Windows для идентификации пользователей, можно использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="5eb0c-110">Проверка подлинности Windows является наиболее безопасный способ проверки подлинности наиболее подходящих для среды интрасети, которой принадлежит пользователей, клиентские приложения и веб-серверов в том же домене Windows.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-110">Windows authentication is a secure form of authentication best suited to intranet environments where users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="5eb0c-111">[Дополнительные сведения о проверке подлинности Windows и его установки для служб IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="5eb0c-111">[Learn more about Windows Authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a><span data-ttu-id="5eb0c-112">Включение проверки подлинности Windows в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5eb0c-112">Enabling Windows authentication in an ASP.NET Core application</span></span>

<span data-ttu-id="5eb0c-113">Шаблон веб-приложения Visual Studio можно настроить для поддержки проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="using-the-windows-authentication-app-template"></a><span data-ttu-id="5eb0c-114">С помощью шаблона приложения проверки подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="5eb0c-114">Using the Windows authentication app template</span></span>

<span data-ttu-id="5eb0c-115">В Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="5eb0c-115">In Visual Studio:</span></span>
* <span data-ttu-id="5eb0c-116">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-116">Create a new ASP.NET Core Web Application.</span></span> 
* <span data-ttu-id="5eb0c-117">В списке шаблонов выберите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-117">Select Web Application from the list of templates.</span></span>
* <span data-ttu-id="5eb0c-118">Нажмите кнопку "Изменить проверку подлинности" и выберите **проверки подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-118">Select the Change Authentication button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="5eb0c-119">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-119">Run the app.</span></span> <span data-ttu-id="5eb0c-120">Имя пользователя отображается в верхней правой части приложения.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-120">The username appears in the top right of the app.</span></span>

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="5eb0c-122">Для разработки с помощью IIS Express шаблон содержит все настройки, необходимой для использования проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="5eb0c-123">В следующем разделе показано, как настроить приложение ASP.NET Core вручную для проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-123">The next section shows how to configure an ASP.NET Core app manually for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="5eb0c-124">Параметры Visual Studio для Windows и анонимная проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="5eb0c-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="5eb0c-125">Страницы свойств Visual Studio вкладку «Отладка» предоставляет флажки для проверки подлинности Windows и анонимную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-125">The Visual Studio properties page, debug tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="5eb0c-127">Можно также настроить эти свойства в `launchSettings.json` файла:</span><span class="sxs-lookup"><span data-stu-id="5eb0c-127">You can also configure these properties in the `launchSettings.json` file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a><span data-ttu-id="5eb0c-128">Включение проверки подлинности Windows в службах IIS</span><span class="sxs-lookup"><span data-stu-id="5eb0c-128">Enabling Windows Authentication with IIS</span></span>

<span data-ttu-id="5eb0c-129">Службы IIS используют [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) (ANCM) для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="5eb0c-130">ANCM потоки проверки подлинности Windows для служб IIS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="5eb0c-131">Настройка проверки подлинности Windows осуществляется в службах IIS, не проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="5eb0c-132">Следующие разделы показывают, как использовать диспетчер служб IIS для настройки приложения ASP.NET Core, чтобы использовать проверку подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="5eb0c-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication:</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="5eb0c-133">Создать новый сайт IIS</span><span class="sxs-lookup"><span data-stu-id="5eb0c-133">Create a new IIS site</span></span>

<span data-ttu-id="5eb0c-134">Укажите имя и папка и разрешить его, чтобы создать новый пул приложений.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="5eb0c-135">Настройки проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="5eb0c-135">Customize Authentication</span></span>

<span data-ttu-id="5eb0c-136">Откройте меню проверки подлинности для сайта.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-136">Open the Authentication menu for the site.</span></span>

![Меню проверки подлинности IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="5eb0c-138">Отключите анонимную проверку подлинности и включить проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Параметры проверки подлинности IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="5eb0c-140">Опубликовать проект в папку узла IIS</span><span class="sxs-lookup"><span data-stu-id="5eb0c-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="5eb0c-141">С помощью Visual Studio или .NET Core (CLI), *публикации* приложения в папку назначения.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-141">Using Visual Studio or the .NET Core CLI, *publish* your app to the destination folder.</span></span>

![Диалоговое окно публикации Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="5eb0c-143">Дополнительные сведения о [публикация в службах IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="5eb0c-143">Learn more about [publishing to IIS](xref:publishing/iis).</span></span>

<span data-ttu-id="5eb0c-144">Запустите приложение, чтобы убедиться, что проверка подлинности Windows работает.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enabling-windows-authentication-with-weblistener"></a><span data-ttu-id="5eb0c-145">Включение проверки подлинности Windows с WebListener</span><span class="sxs-lookup"><span data-stu-id="5eb0c-145">Enabling Windows authentication with WebListener</span></span>

<span data-ttu-id="5eb0c-146">Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [WebListener](xref:fundamentals/servers/weblistener) для поддержки сценариев резидентных в Windows.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-146">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="5eb0c-147">В следующем примере настраивается узел веб-приложения для WebListener с проверкой подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="5eb0c-147">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a><span data-ttu-id="5eb0c-148">Работа с проверкой подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="5eb0c-148">Working with Windows authentication</span></span>

<span data-ttu-id="5eb0c-149">Если ваше приложение использует проверку подлинности Windows и анонимный доступ, можно использовать ``[Authorize]`` и ``[AllowAnonymous]`` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-149">If your app uses Windows authentication and anonymous access, you can use the ``[Authorize]`` and ``[AllowAnonymous]`` attributes.</span></span> <span data-ttu-id="5eb0c-150">Приложения, имеющие не требует анонимного включена ``[Authorize]``; приложение рассматривается как требование проверки подлинности, анонимные запросы будут отвергнуты.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-150">Apps that do not have anonymous enabled do not require ``[Authorize]``; the  app is treated as requiring authentication, anonymous requests are rejected.</span></span> <span data-ttu-id="5eb0c-151">Обратите внимание, если узел IIS настроен **не** разрешает анонимный доступ ``[AllowAnonymous]`` атрибут не **не** Разрешить анонимные запросы.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-151">Note, if the IIS site is configured **not** to allow anonymous access, the ``[AllowAnonymous]`` attribute does **not** allow anonymous requests.</span></span> <span data-ttu-id="5eb0c-152">``[AllowAnonymous]`` Переопределения атрибутов ``[Authorize]`` атрибут для использования в приложениях, которые разрешить анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-152">The ``[AllowAnonymous]`` attribute overrides ``[Authorize]`` attribute usage within apps that allow anonymous access.</span></span>

### <a name="impersonation"></a><span data-ttu-id="5eb0c-153">Олицетворение</span><span class="sxs-lookup"><span data-stu-id="5eb0c-153">Impersonation</span></span>

<span data-ttu-id="5eb0c-154">Олицетворение ASP.NET Core не реализован.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-154">ASP.NET Core does not implement impersonation.</span></span> <span data-ttu-id="5eb0c-155">Приложения работают с удостоверением приложения для всех запросов, используя удостоверение пула или процесс приложения.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-155">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="5eb0c-156">Если вам нужно явно выполнять действия от имени пользователя, используйте ``WindowsIdentity.RunImpersonated``.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-156">If you need to explicitly perform an action on behalf of a user, use ``WindowsIdentity.RunImpersonated``.</span></span> <span data-ttu-id="5eb0c-157">Запустить одно действие в этом контексте, а затем закройте контекст.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-157">Run a single action in this context and then close the context.</span></span> <span data-ttu-id="5eb0c-158">Обратите внимание, что ``RunImpersonated`` не поддерживает асинхронные и не должны использоваться для сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-158">Note that ``RunImpersonated`` does not support async and should not be used for complex scenarios.</span></span> <span data-ttu-id="5eb0c-159">Например упаковки все запросы или цепочки по промежуточного слоя не поддерживается или рекомендуемые.</span><span class="sxs-lookup"><span data-stu-id="5eb0c-159">For example, wrapping entire requests or middleware chains is not supported or recommended.</span></span>
