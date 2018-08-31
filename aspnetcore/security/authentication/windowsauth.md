---
title: Настройка проверки подлинности Windows в ASP.NET Core
author: ardalis
description: В этой статье описывается настройка проверки подлинности Windows в ASP.NET Core, используя IIS Express, службы IIS, HTTP.sys и WebListener.
ms.author: riande
ms.date: 08/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: a8066d248c0d4db1d1f61b2a14bdb4656a2f4265
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312416"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="70c88-103">Настройка проверки подлинности Windows в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70c88-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="70c88-104">Авторы: [Стив Смит](https://ardalis.com) (Steve Smith) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="70c88-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="70c88-105">Проверка подлинности Windows можно настроить для приложений ASP.NET Core, размещенных в IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), или [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="70c88-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="70c88-106">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="70c88-106">Windows Authentication</span></span>

<span data-ttu-id="70c88-107">Проверка подлинности Windows зависит от операционной системы для проверки подлинности пользователей, приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70c88-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="70c88-108">Можно использовать проверку подлинности Windows, когда сервер работает в корпоративной сети с помощью удостоверения домена Active Directory или других учетных записей Windows для идентификации пользователей.</span><span class="sxs-lookup"><span data-stu-id="70c88-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="70c88-109">Проверка подлинности Windows является наилучшим образом подходит для среды интрасети, в которых пользователи, клиентские приложения и веб-серверы принадлежат к тому же домену Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="70c88-110">[Дополнительные сведения о проверке подлинности Windows и его установки для служб IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="70c88-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="70c88-111">Включить проверку подлинности Windows в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70c88-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="70c88-112">Шаблон веб-приложения Visual Studio можно настроить для поддержки проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="70c88-113">Использовать шаблон приложения проверки подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="70c88-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="70c88-114">В Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="70c88-114">In Visual Studio:</span></span>

1. <span data-ttu-id="70c88-115">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70c88-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="70c88-116">Выберите веб-приложение из списка шаблонов.</span><span class="sxs-lookup"><span data-stu-id="70c88-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="70c88-117">Выберите **изменить способ проверки подлинности** и выберите **проверки подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="70c88-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="70c88-118">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="70c88-118">Run the app.</span></span> <span data-ttu-id="70c88-119">Имя пользователя отображается в правом верхнем углу приложения.</span><span class="sxs-lookup"><span data-stu-id="70c88-119">The username appears in the top right of the app.</span></span>

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="70c88-121">Для разработки с использованием IIS Express шаблон содержит все настройки, необходимой для использования проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="70c88-122">Ниже показано, как вручную настроить приложение ASP.NET Core для проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="70c88-123">Параметры Visual Studio для Windows и анонимная проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="70c88-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="70c88-124">В проект Visual Studio **свойства** страницы **Отладка** вкладка предоставляет флажки для проверки подлинности Windows и анонимную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="70c88-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="70c88-126">Кроме того, эти свойства можно задать в *launchSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="70c88-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="70c88-127">Включить проверку подлинности Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="70c88-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="70c88-128">Службы IIS используют [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70c88-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="70c88-129">В службах IIS, приложение не настроена проверка подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="70c88-130">Ниже показано, как использовать диспетчер служб IIS для настройки приложения ASP.NET Core для использования проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="70c88-131">Конфигурация IIS</span><span class="sxs-lookup"><span data-stu-id="70c88-131">IIS configuration</span></span>

<span data-ttu-id="70c88-132">Включение службы роли IIS для проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="70c88-133">Дополнительные сведения см. в разделе [включить проверку подлинности Windows в службы роли IIS (см. шаг 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="70c88-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="70c88-134">По умолчанию по промежуточного слоя для интеграции IIS настроен для автоматической проверки подлинности запросов.</span><span class="sxs-lookup"><span data-stu-id="70c88-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="70c88-135">Дополнительные сведения см. в разделе [размещение ASP.NET Core в Windows со службами IIS: параметры служб IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="70c88-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="70c88-136">Модуль ASP.NET Core настроен на пересылку токена проверки подлинности Windows в приложение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="70c88-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="70c88-137">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core: атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="70c88-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="70c88-138">Создание нового сайта IIS</span><span class="sxs-lookup"><span data-stu-id="70c88-138">Create a new IIS site</span></span>

<span data-ttu-id="70c88-139">Укажите имя и папку и разрешите ему создать новый пул приложений.</span><span class="sxs-lookup"><span data-stu-id="70c88-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="70c88-140">Настройки проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="70c88-140">Customize authentication</span></span>

<span data-ttu-id="70c88-141">Откройте средства проверки подлинности для сайта.</span><span class="sxs-lookup"><span data-stu-id="70c88-141">Open the Authentication features for the site.</span></span>

![Меню проверки подлинности IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="70c88-143">Отключить анонимную проверку подлинности и включить проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Параметры проверки подлинности IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="70c88-145">Опубликовать проект в папке сайта IIS</span><span class="sxs-lookup"><span data-stu-id="70c88-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="70c88-146">С помощью Visual Studio или .NET Core CLI, опубликуйте приложение в папку назначения.</span><span class="sxs-lookup"><span data-stu-id="70c88-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Диалоговое окно публикации Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="70c88-148">Дополнительные сведения о [публикация в службах IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="70c88-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="70c88-149">Запустите приложение, чтобы убедиться, что проверка подлинности Windows работает.</span><span class="sxs-lookup"><span data-stu-id="70c88-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="70c88-150">Включить проверку подлинности Windows с использованием HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="70c88-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="70c88-151">Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [HTTP.sys](xref:fundamentals/servers/httpsys) для поддержки сценариев резидентных в Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="70c88-152">В следующем примере настраивается узел веб-приложения для использования HTTP.sys с проверкой подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="70c88-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="70c88-153">HTTP.sys делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos.</span><span class="sxs-lookup"><span data-stu-id="70c88-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="70c88-154">Проверка подлинности в режиме пользователя не поддерживается с Kerberos и HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="70c88-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="70c88-155">Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="70c88-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="70c88-156">Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.</span><span class="sxs-lookup"><span data-stu-id="70c88-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="70c88-157">Включить проверку подлинности Windows с помощью WebListener</span><span class="sxs-lookup"><span data-stu-id="70c88-157">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="70c88-158">Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [WebListener](xref:fundamentals/servers/weblistener) для поддержки сценариев резидентных в Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-158">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="70c88-159">В следующем примере настраивается узел веб-приложения для использования WebListener с проверкой подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="70c88-159">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="70c88-160">WebListener делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos.</span><span class="sxs-lookup"><span data-stu-id="70c88-160">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="70c88-161">Проверка подлинности в режиме пользователя не поддерживается с Kerberos и WebListener.</span><span class="sxs-lookup"><span data-stu-id="70c88-161">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="70c88-162">Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="70c88-162">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="70c88-163">Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.</span><span class="sxs-lookup"><span data-stu-id="70c88-163">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="70c88-164">Работа с проверкой подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="70c88-164">Work with Windows Authentication</span></span>

<span data-ttu-id="70c88-165">Состояние конфигурации анонимный доступ определяет способ, которым `[Authorize]` и `[AllowAnonymous]` атрибуты используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="70c88-165">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="70c88-166">Следующих двух разделах объясняется, как обрабатывать запрещенные и разрешенные конфигурации состояния анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="70c88-166">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="70c88-167">Запретить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="70c88-167">Disallow anonymous access</span></span>

<span data-ttu-id="70c88-168">Если включена проверка подлинности Windows и анонимный доступ отключен, `[Authorize]` и `[AllowAnonymous]` атрибуты никак не влияют.</span><span class="sxs-lookup"><span data-stu-id="70c88-168">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="70c88-169">Если IIS сайта (или сервера HTTP.sys или WebListener) настроен на запрет анонимного доступа, никогда не достигнут приложения.</span><span class="sxs-lookup"><span data-stu-id="70c88-169">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="70c88-170">По этой причине `[AllowAnonymous]` атрибут не применяется.</span><span class="sxs-lookup"><span data-stu-id="70c88-170">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="70c88-171">Разрешить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="70c88-171">Allow anonymous access</span></span>

<span data-ttu-id="70c88-172">Если включены как проверка подлинности Windows, так и анонимный доступ, используйте `[Authorize]` и `[AllowAnonymous]` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="70c88-172">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="70c88-173">`[Authorize]` Атрибут позволяет защищать частей приложения, которые действительно требует проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-173">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="70c88-174">`[AllowAnonymous]` Атрибут переопределения `[Authorize]` атрибут использования в приложениях, в которых разрешен анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="70c88-174">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="70c88-175">См. в разделе [простой авторизации](xref:security/authorization/simple) сведения об использовании атрибута.</span><span class="sxs-lookup"><span data-stu-id="70c88-175">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="70c88-176">В ASP.NET Core 2.x, `[Authorize]` атрибута требуется дополнительная конфигурация *Startup.cs* бросить вызов анонимные запросы для проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="70c88-176">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="70c88-177">Рекомендуемая конфигурация зависит от немного используется веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="70c88-177">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="70c88-178">По умолчанию пользователи, у которых отсутствует разрешение на доступ к странице, откроется пустой ответ HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="70c88-178">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="70c88-179">[StatusCodePages по промежуточного слоя](xref:fundamentals/error-handling#configure-status-code-pages) можно настроить для предоставления пользователям более комфортные «Отказано в доступе».</span><span class="sxs-lookup"><span data-stu-id="70c88-179">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="70c88-180">IIS</span><span class="sxs-lookup"><span data-stu-id="70c88-180">IIS</span></span>

<span data-ttu-id="70c88-181">Если используется IIS, добавьте следующий код в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="70c88-181">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="70c88-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="70c88-182">HTTP.sys</span></span>

<span data-ttu-id="70c88-183">Если в файле HTTP.sys, добавьте следующий код в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="70c88-183">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="70c88-184">Олицетворение</span><span class="sxs-lookup"><span data-stu-id="70c88-184">Impersonation</span></span>

<span data-ttu-id="70c88-185">ASP.NET Core не реализует олицетворения.</span><span class="sxs-lookup"><span data-stu-id="70c88-185">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="70c88-186">Приложения запускаются с удостоверением приложения для всех запросов, с помощью удостоверения пула или процесс приложения.</span><span class="sxs-lookup"><span data-stu-id="70c88-186">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="70c88-187">Если вам нужно явно выполнять действия от имени пользователя, используйте `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="70c88-187">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="70c88-188">Запустить одно действие в этом контексте, а затем закройте контекста.</span><span class="sxs-lookup"><span data-stu-id="70c88-188">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="70c88-189">Обратите внимание, что `RunImpersonated` не поддерживает асинхронные операции и не должны использоваться для сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="70c88-189">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="70c88-190">Например упаковки всего запросов или по промежуточного слоя цепочек не поддерживается и не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="70c88-190">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
