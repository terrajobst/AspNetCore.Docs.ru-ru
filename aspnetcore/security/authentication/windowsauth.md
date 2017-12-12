---
title: "Настроить проверку подлинности в ASP.NET Core"
author: ardalis
description: "В этой статье описывается настройка проверки подлинности Windows в ASP.NET Core, используя IIS Express, службы IIS, HTTP.sys и WebListener."
keywords: "ASP.NET Core, проверка подлинности Windows, Authorize атрибут, атрибут AllowAnonymous"
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 703f924d049a267cb8bee22e63e6669b13099c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="f1c1f-104">Настройка проверки подлинности Windows в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1c1f-104">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="f1c1f-105">По [Стив Смит](https://ardalis.com) и [Скотт Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="f1c1f-105">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f1c1f-106">Проверка подлинности Windows можно настроить для приложения ASP.NET Core, размещенные в IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), или [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="f1c1f-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="f1c1f-107">Что такое проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="f1c1f-107">What is Windows authentication?</span></span>

<span data-ttu-id="f1c1f-108">Проверка подлинности Windows зависит от операционной системы, для проверки подлинности пользователей приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="f1c1f-109">При запуске сервера в корпоративной сети с помощью удостоверения домена Active Directory или других учетных записей Windows для идентификации пользователей, можно использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="f1c1f-110">Проверка подлинности Windows наилучшим образом подходит для среды интрасети, в которых пользователей, клиентские приложения и веб-серверы принадлежат к одному домену Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-110">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="f1c1f-111">[Дополнительные сведения о проверке подлинности Windows и его установки для служб IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="f1c1f-111">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="f1c1f-112">Включение проверки подлинности Windows в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1c1f-112">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="f1c1f-113">Шаблон веб-приложения Visual Studio можно настроить для поддержки проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="f1c1f-114">Используйте шаблон приложения проверки подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="f1c1f-114">Use the Windows authentication app template</span></span>

<span data-ttu-id="f1c1f-115">В Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f1c1f-115">In Visual Studio:</span></span>
1. <span data-ttu-id="f1c1f-116">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-116">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="f1c1f-117">В списке шаблонов выберите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-117">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="f1c1f-118">Выберите **изменить аутентификацию** и выберите пункт **проверки подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-118">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="f1c1f-119">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-119">Run the app.</span></span> <span data-ttu-id="f1c1f-120">Имя пользователя отображается в верхней правой части приложения.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-120">The username appears in the top right of the app.</span></span>

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="f1c1f-122">Для разработки с помощью IIS Express шаблон содержит все настройки, необходимой для использования проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="f1c1f-123">Ниже показано, как вручную настроить приложение ASP.NET Core для проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-123">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="f1c1f-124">Параметры Visual Studio для Windows и анонимная проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="f1c1f-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="f1c1f-125">Проект Visual Studio **свойства** страницы **отладки** вкладка предоставляет флажки для проверки подлинности Windows и анонимную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-125">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="f1c1f-127">Кроме того, можно настроить эти свойства в *launchSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="f1c1f-127">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="f1c1f-128">Включение проверки подлинности Windows в службах IIS</span><span class="sxs-lookup"><span data-stu-id="f1c1f-128">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="f1c1f-129">Службы IIS используют [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) (ANCM) для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="f1c1f-130">ANCM потоки проверки подлинности Windows для служб IIS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="f1c1f-131">Настройка проверки подлинности Windows осуществляется в службах IIS, не проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="f1c1f-132">Ниже показано, как использовать диспетчер служб IIS для настройки приложения ASP.NET Core, чтобы использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="f1c1f-133">Создать новый сайт IIS</span><span class="sxs-lookup"><span data-stu-id="f1c1f-133">Create a new IIS site</span></span>

<span data-ttu-id="f1c1f-134">Укажите имя и папка и разрешить его, чтобы создать новый пул приложений.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="f1c1f-135">Настройки проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="f1c1f-135">Customize authentication</span></span>

<span data-ttu-id="f1c1f-136">Откройте меню проверки подлинности для сайта.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-136">Open the Authentication menu for the site.</span></span>

![Меню проверки подлинности IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="f1c1f-138">Отключите анонимную проверку подлинности и включить проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Параметры проверки подлинности IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="f1c1f-140">Опубликовать проект в папку узла IIS</span><span class="sxs-lookup"><span data-stu-id="f1c1f-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="f1c1f-141">С помощью Visual Studio или .NET Core CLI, опубликуйте приложение в папку назначения.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-141">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Диалоговое окно публикации Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="f1c1f-143">Дополнительные сведения о [публикация в службах IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="f1c1f-143">Learn more about [publishing to IIS](xref:publishing/iis).</span></span>

<span data-ttu-id="f1c1f-144">Запустите приложение, чтобы убедиться, что проверка подлинности Windows работает.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="f1c1f-145">Включение проверки подлинности Windows в HTTP.sys или WebListener</span><span class="sxs-lookup"><span data-stu-id="f1c1f-145">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f1c1f-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f1c1f-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f1c1f-147">Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [HTTP.sys](xref:fundamentals/servers/httpsys) для поддержки сценариев резидентных в Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-147">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="f1c1f-148">В следующем примере настраивается узел веб-приложения для HTTP.sys с проверкой подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="f1c1f-148">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f1c1f-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f1c1f-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f1c1f-150">Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [WebListener](xref:fundamentals/servers/weblistener) для поддержки сценариев резидентных в Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-150">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="f1c1f-151">В следующем примере настраивается узел веб-приложения для WebListener с проверкой подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="f1c1f-151">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="f1c1f-152">Использование проверки подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="f1c1f-152">Work with Windows authentication</span></span>

<span data-ttu-id="f1c1f-153">Состояние конфигурации анонимный доступ определяет, каким образом `[Authorize]` и `[AllowAnonymous]` атрибуты используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-153">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="f1c1f-154">Следующих двух разделах объясняется, как обрабатывать конфигурации запрещенных и разрешенных состояний анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-154">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="f1c1f-155">Запретить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="f1c1f-155">Disallow anonymous access</span></span>

<span data-ttu-id="f1c1f-156">Если включена проверка подлинности Windows и анонимный доступ отключен, `[Authorize]` и `[AllowAnonymous]` атрибуты никак не влияют.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-156">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="f1c1f-157">Если сайт IIS (или HTTP.sys или WebListener сервера) настроен для запрещения анонимный доступ, запрос никогда не достигает приложения.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-157">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="f1c1f-158">По этой причине `[AllowAnonymous]` не применяется атрибут.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-158">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="f1c1f-159">Разрешить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="f1c1f-159">Allow anonymous access</span></span>

<span data-ttu-id="f1c1f-160">При включении проверки подлинности Windows и анонимный доступ, используйте `[Authorize]` и `[AllowAnonymous]` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-160">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="f1c1f-161">`[Authorize]` Атрибут позволяет защищать части приложения, которые действительно требуется проверка подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-161">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="f1c1f-162">`[AllowAnonymous]` Переопределения атрибутов `[Authorize]` атрибута для использования в приложениях, в которых разрешен анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-162">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="f1c1f-163">В разделе [простой авторизации](xref:security/authorization/simple) сведения об использовании атрибутов.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-163">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="f1c1f-164">В ASP.NET Core 2.x `[Authorize]` атрибут требует дополнительной настройки в *файла Startup.cs* проверку анонимные запросы на проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-164">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="f1c1f-165">Рекомендуемая конфигурация немного различается в зависимости от используемого веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-165">The recommended configuration varies slightly based on the web server being used.</span></span>

#### <a name="iis"></a><span data-ttu-id="f1c1f-166">IIS</span><span class="sxs-lookup"><span data-stu-id="f1c1f-166">IIS</span></span>

<span data-ttu-id="f1c1f-167">Если с помощью IIS, добавьте следующий код в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="f1c1f-167">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="f1c1f-168">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f1c1f-168">HTTP.sys</span></span>

<span data-ttu-id="f1c1f-169">Если с помощью файла HTTP.sys, добавьте следующий код в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="f1c1f-169">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="f1c1f-170">Олицетворение</span><span class="sxs-lookup"><span data-stu-id="f1c1f-170">Impersonation</span></span>

<span data-ttu-id="f1c1f-171">ASP.NET Core не реализует олицетворения.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-171">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="f1c1f-172">Приложения работают с удостоверением приложения для всех запросов, используя удостоверение пула или процесс приложения.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-172">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="f1c1f-173">Если вам нужно явно выполнять действия от имени пользователя, используйте `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-173">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="f1c1f-174">Запустить одно действие в этом контексте, а затем закройте контекст.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-174">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="f1c1f-175">Обратите внимание, что `RunImpersonated` не поддерживает асинхронные операции и не должны использоваться для сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-175">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="f1c1f-176">Например упаковки все запросы или цепочки по промежуточного слоя не поддерживается и не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="f1c1f-176">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
