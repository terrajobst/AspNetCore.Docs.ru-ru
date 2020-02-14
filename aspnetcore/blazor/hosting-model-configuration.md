---
title: Конфигурация модели размещения Blazor ASP.NET Core
author: guardrex
description: Сведения о конфигурации модели размещения Blazor, в том числе о том, как интегрировать компоненты Razor в Razor Pages и приложения MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 91dd1aa32bb5165845eb4794377a50ccbd01adc3
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2020
ms.locfileid: "77214943"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="08477-103">Конфигурация модели размещения Блазор ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08477-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="08477-104">По [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="08477-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="08477-105">В этой статье рассматривается Конфигурация модели размещения.</span><span class="sxs-lookup"><span data-stu-id="08477-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="08477-106">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="08477-106">Blazor Server</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="08477-107">Интеграция компонентов Razor в приложения Razor Pages и MVC</span><span class="sxs-lookup"><span data-stu-id="08477-107">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="08477-108">Использование компонентов в страницах и представлениях</span><span class="sxs-lookup"><span data-stu-id="08477-108">Use components in pages and views</span></span>

<span data-ttu-id="08477-109">Существующее приложение Razor Pages или MVC может интегрировать компоненты Razor в страницы и представления:</span><span class="sxs-lookup"><span data-stu-id="08477-109">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="08477-110">В файле макета приложения ( *_layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="08477-110">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="08477-111">Добавьте следующий тег `<base>` в элемент `<head>`:</span><span class="sxs-lookup"><span data-stu-id="08477-111">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="08477-112">Значение `href` ( *базовый путь приложения*) в предыдущем примере предполагает, что приложение находится по КОРНЕВОМУ URL-пути (`/`).</span><span class="sxs-lookup"><span data-stu-id="08477-112">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="08477-113">Если приложение является вложенным приложением, следуйте указаниям в разделе " *базовый путь приложения* " статьи <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="08477-113">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="08477-114">Файл *_layout. cshtml* находится в папке *pages/shared* в приложении Razor Pages или в папке *views/Shared* приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="08477-114">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="08477-115">Добавьте тег `<script>` для скрипта *блазор. Server. js* непосредственно перед закрывающим тегом `</body>`:</span><span class="sxs-lookup"><span data-stu-id="08477-115">Add a `<script>` tag for the *blazor.server.js* script right before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="08477-116">Платформа добавляет в приложение скрипт *блазор. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="08477-116">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="08477-117">Нет необходимости вручную добавлять скрипт в приложение.</span><span class="sxs-lookup"><span data-stu-id="08477-117">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="08477-118">Добавьте файл *_Imports. Razor* в корневую папку проекта со следующим содержимым (измените Последнее пространство имен, `MyAppNamespace`, в пространство имен приложения):</span><span class="sxs-lookup"><span data-stu-id="08477-118">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="08477-119">В `Startup.ConfigureServices`Зарегистрируйте службу сервера Блазор:</span><span class="sxs-lookup"><span data-stu-id="08477-119">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="08477-120">В `Startup.Configure`Добавьте конечную точку концентратора Блазор в `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="08477-120">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="08477-121">Интегрируйте компоненты в любую страницу или представление.</span><span class="sxs-lookup"><span data-stu-id="08477-121">Integrate components into any page or view.</span></span> <span data-ttu-id="08477-122">Дополнительные сведения см. в разделе *Интеграция компонентов в Razor Pages и приложения MVC* статьи <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="08477-122">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="08477-123">Использование маршрутизируемых компонентов в приложении Razor Pages</span><span class="sxs-lookup"><span data-stu-id="08477-123">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="08477-124">Для поддержки маршрутизируемых компонентов Razor в Razor Pages приложениях:</span><span class="sxs-lookup"><span data-stu-id="08477-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="08477-125">Следуйте указаниям в разделе [Использование компонентов в страницах и представлениях](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="08477-125">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="08477-126">Добавьте файл *app. Razor* в корневую папку проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="08477-126">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="08477-127">Добавьте файл *_Host. cshtml* в папку *pages* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="08477-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="08477-128">Компоненты используют общий файл *_layout. cshtml* для их макета.</span><span class="sxs-lookup"><span data-stu-id="08477-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="08477-129">Добавьте маршрут с низким приоритетом для страницы *_Host. cshtml* в конфигурацию конечной точки в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="08477-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="08477-130">Добавьте маршрутизируемые компоненты в приложение.</span><span class="sxs-lookup"><span data-stu-id="08477-130">Add routable components to the app.</span></span> <span data-ttu-id="08477-131">Пример:</span><span class="sxs-lookup"><span data-stu-id="08477-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="08477-132">При использовании пользовательской папки для хранения компонентов приложения добавьте пространство имен, представляющее папку, в файл *pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="08477-132">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="08477-133">Дополнительные сведения см. в разделе <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="08477-133">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="08477-134">Использование маршрутизируемых компонентов в приложении MVC</span><span class="sxs-lookup"><span data-stu-id="08477-134">Use routable components in an MVC app</span></span>

<span data-ttu-id="08477-135">Для поддержки маршрутизации компонентов Razor в приложениях MVC:</span><span class="sxs-lookup"><span data-stu-id="08477-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="08477-136">Следуйте указаниям в разделе [Использование компонентов в страницах и представлениях](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="08477-136">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="08477-137">Добавьте файл *app. Razor* в корневую папку проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="08477-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="08477-138">Добавьте файл *_Host. cshtml* в папку *Views/Home* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="08477-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="08477-139">Компоненты используют общий файл *_layout. cshtml* для их макета.</span><span class="sxs-lookup"><span data-stu-id="08477-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="08477-140">Добавьте действие в контроллер Home:</span><span class="sxs-lookup"><span data-stu-id="08477-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="08477-141">Добавьте маршрут с низким приоритетом для действия контроллера, которое возвращает представление *_Host. cshtml* в конфигурацию конечной точки в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="08477-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="08477-142">Создайте папку *pages* и добавьте в приложение маршрутизируемые компоненты.</span><span class="sxs-lookup"><span data-stu-id="08477-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="08477-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="08477-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="08477-144">При использовании пользовательской папки для хранения компонентов приложения добавьте пространство имен, представляющее папку, в файл *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="08477-144">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="08477-145">Дополнительные сведения см. в разделе <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="08477-145">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="08477-146">Отражение состояния соединения в пользовательском интерфейсе</span><span class="sxs-lookup"><span data-stu-id="08477-146">Reflect the connection state in the UI</span></span>

<span data-ttu-id="08477-147">Когда клиент обнаруживает, что соединение потеряно, пользователь по умолчанию отображает пользовательский интерфейс, когда клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="08477-147">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="08477-148">Если происходит сбой повторного подключения, пользователь предоставляет возможность повторить попытку.</span><span class="sxs-lookup"><span data-stu-id="08477-148">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="08477-149">Чтобы настроить пользовательский интерфейс, определите элемент с `id` `components-reconnect-modal` в `<body>` страницы Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="08477-149">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="08477-150">В следующей таблице описаны классы CSS, применяемые к элементу `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="08477-150">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="08477-151">Класс CSS</span><span class="sxs-lookup"><span data-stu-id="08477-151">CSS class</span></span>                       | <span data-ttu-id="08477-152">Указывает,&hellip;</span><span class="sxs-lookup"><span data-stu-id="08477-152">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="08477-153">Утерянное подключение.</span><span class="sxs-lookup"><span data-stu-id="08477-153">A lost connection.</span></span> <span data-ttu-id="08477-154">Клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="08477-154">The client is attempting to reconnect.</span></span> <span data-ttu-id="08477-155">Отображение модального окна.</span><span class="sxs-lookup"><span data-stu-id="08477-155">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="08477-156">Активное подключение будет восстановлено на сервере.</span><span class="sxs-lookup"><span data-stu-id="08477-156">An active connection is re-established to the server.</span></span> <span data-ttu-id="08477-157">Скрыть модальное окно.</span><span class="sxs-lookup"><span data-stu-id="08477-157">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="08477-158">Сбой повторного подключения, возможно, из-за сбоя сети.</span><span class="sxs-lookup"><span data-stu-id="08477-158">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="08477-159">Чтобы повторить попытку подключения, вызовите `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="08477-159">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="08477-160">Повторное подключение отклонено.</span><span class="sxs-lookup"><span data-stu-id="08477-160">Reconnection rejected.</span></span> <span data-ttu-id="08477-161">Сервер был достигнут, но отклонил подключение, и состояние пользователя на сервере потеряно.</span><span class="sxs-lookup"><span data-stu-id="08477-161">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="08477-162">Чтобы перезагрузить приложение, вызовите `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="08477-162">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="08477-163">Это состояние соединения может появиться в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="08477-163">This connection state may result when:</span></span><ul><li><span data-ttu-id="08477-164">Происходит сбой в цепи на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="08477-164">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="08477-165">Клиент отключается достаточно долго, чтобы сервер отключил состояние пользователя.</span><span class="sxs-lookup"><span data-stu-id="08477-165">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="08477-166">Удаляются экземпляры компонентов, с которыми взаимодействует пользователь.</span><span class="sxs-lookup"><span data-stu-id="08477-166">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="08477-167">Сервер перезагружается, или рабочий процесс приложения перезапускается.</span><span class="sxs-lookup"><span data-stu-id="08477-167">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="08477-168">Режим обработки</span><span class="sxs-lookup"><span data-stu-id="08477-168">Render mode</span></span>

<span data-ttu-id="08477-169">Серверные приложения блазор по умолчанию настроены на выстройку пользовательского интерфейса на сервере, прежде чем будет установлено клиентское соединение с сервером.</span><span class="sxs-lookup"><span data-stu-id="08477-169">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="08477-170">Это настраивается на странице Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="08477-170">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="08477-171">`RenderMode` настраивает, является ли компонент:</span><span class="sxs-lookup"><span data-stu-id="08477-171">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="08477-172">Предварительно отображается на странице.</span><span class="sxs-lookup"><span data-stu-id="08477-172">Is prerendered into the page.</span></span>
* <span data-ttu-id="08477-173">Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Блазор из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="08477-173">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="08477-174">Description</span><span class="sxs-lookup"><span data-stu-id="08477-174">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="08477-175">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="08477-175">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="08477-176">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="08477-176">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="08477-177">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="08477-177">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="08477-178">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="08477-178">Output from the component isn't included.</span></span> <span data-ttu-id="08477-179">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="08477-179">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="08477-180">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="08477-180">Renders the component into static HTML.</span></span> |

<span data-ttu-id="08477-181">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="08477-181">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="08477-182">Отображение интерактивных компонентов с отслеживанием состояния из страниц и представлений Razor</span><span class="sxs-lookup"><span data-stu-id="08477-182">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="08477-183">Интерактивные компоненты с отслеживанием состояния могут быть добавлены на страницу Razor или в представление.</span><span class="sxs-lookup"><span data-stu-id="08477-183">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="08477-184">При визуализации страницы или представления:</span><span class="sxs-lookup"><span data-stu-id="08477-184">When the page or view renders:</span></span>

* <span data-ttu-id="08477-185">Компонент предварительно отображается страницей или представлением.</span><span class="sxs-lookup"><span data-stu-id="08477-185">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="08477-186">Исходное состояние компонента, используемое для предварительной визуализации, потеряно.</span><span class="sxs-lookup"><span data-stu-id="08477-186">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="08477-187">Новое состояние компонента создается при установке подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="08477-187">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="08477-188">Следующая страница Razor визуализирует компонент `Counter`:</span><span class="sxs-lookup"><span data-stu-id="08477-188">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="08477-189">Прорисовка неинтерактивных компонентов на страницах и представлениях Razor</span><span class="sxs-lookup"><span data-stu-id="08477-189">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="08477-190">На следующей странице Razor компонент `Counter` статически подготавливается к просмотру с начальным значением, указанным с помощью формы:</span><span class="sxs-lookup"><span data-stu-id="08477-190">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="08477-191">Поскольку `MyComponent` является статически отображаемым, компонент не может быть интерактивным.</span><span class="sxs-lookup"><span data-stu-id="08477-191">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="08477-192">Настройка клиента SignalR для приложений Blazor Server</span><span class="sxs-lookup"><span data-stu-id="08477-192">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="08477-193">Иногда необходимо настроить клиент SignalR, используемый приложениями Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="08477-193">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="08477-194">Например, может потребоваться настроить ведение журнала на SignalRном клиенте для диагностики проблемы с подключением.</span><span class="sxs-lookup"><span data-stu-id="08477-194">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="08477-195">Настройка клиента SignalR в файле *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="08477-195">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="08477-196">Добавьте атрибут `autostart="false"` в тег `<script>` для скрипта `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="08477-196">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="08477-197">Вызовите `Blazor.start` и передайте объект конфигурации, указывающий построитель SignalR.</span><span class="sxs-lookup"><span data-stu-id="08477-197">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```
