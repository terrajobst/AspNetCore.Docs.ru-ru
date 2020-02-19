---
title: Интеграция ASP.NET Core компонентов Razor в Razor Pages и приложения MVC
author: guardrex
description: Сведения о сценариях привязки данных для компонентов и элементов DOM в Blazor приложениях.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: de1a37ffd9456c956e3d84fcc69431ecb794513c
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453214"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="714c0-103">Интеграция ASP.NET Core компонентов Razor в Razor Pages и приложения MVC</span><span class="sxs-lookup"><span data-stu-id="714c0-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="714c0-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="714c0-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="714c0-105">Компоненты Razor можно интегрировать в Razor Pages и приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="714c0-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="714c0-106">При отображении страницы или представления компоненты можно предварительно отобразить в одно и то же время.</span><span class="sxs-lookup"><span data-stu-id="714c0-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="714c0-107">Подготовка приложения к использованию компонентов в страницах и представлениях</span><span class="sxs-lookup"><span data-stu-id="714c0-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="714c0-108">Существующее приложение Razor Pages или MVC может интегрировать компоненты Razor в страницы и представления:</span><span class="sxs-lookup"><span data-stu-id="714c0-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="714c0-109">В файле макета приложения ( *_layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="714c0-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="714c0-110">Добавьте следующий тег `<base>` в элемент `<head>`:</span><span class="sxs-lookup"><span data-stu-id="714c0-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="714c0-111">Значение `href` ( *базовый путь приложения*) в предыдущем примере предполагает, что приложение находится по КОРНЕВОМУ URL-пути (`/`).</span><span class="sxs-lookup"><span data-stu-id="714c0-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="714c0-112">Если приложение является вложенным приложением, следуйте указаниям в разделе " *базовый путь приложения* " статьи <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="714c0-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="714c0-113">Файл *_layout. cshtml* находится в папке *pages/shared* в приложении Razor Pages или в папке *views/Shared* приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="714c0-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="714c0-114">Добавьте тег `<script>` для скрипта *блазор. Server. js* непосредственно перед закрывающим тегом `</body>`:</span><span class="sxs-lookup"><span data-stu-id="714c0-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="714c0-115">Платформа добавляет в приложение скрипт *блазор. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="714c0-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="714c0-116">Нет необходимости вручную добавлять скрипт в приложение.</span><span class="sxs-lookup"><span data-stu-id="714c0-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="714c0-117">Добавьте файл *_Imports. Razor* в корневую папку проекта со следующим содержимым (измените Последнее пространство имен, `MyAppNamespace`, в пространство имен приложения):</span><span class="sxs-lookup"><span data-stu-id="714c0-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="714c0-118">В `Startup.ConfigureServices`Зарегистрируйте службу сервера Блазор:</span><span class="sxs-lookup"><span data-stu-id="714c0-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="714c0-119">В `Startup.Configure`Добавьте конечную точку концентратора Блазор в `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="714c0-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="714c0-120">Интегрируйте компоненты в любую страницу или представление.</span><span class="sxs-lookup"><span data-stu-id="714c0-120">Integrate components into any page or view.</span></span> <span data-ttu-id="714c0-121">Дополнительные сведения см. в разделе [подготовка компонентов с помощью страницы или представления](#render-components-from-a-page-or-view) .</span><span class="sxs-lookup"><span data-stu-id="714c0-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="714c0-122">Использование маршрутизируемых компонентов в приложении Razor Pages</span><span class="sxs-lookup"><span data-stu-id="714c0-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="714c0-123">*Этот раздел относится к добавлению компонентов, напрямую направляемых из запросов пользователей.*</span><span class="sxs-lookup"><span data-stu-id="714c0-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="714c0-124">Для поддержки маршрутизируемых компонентов Razor в Razor Pages приложениях:</span><span class="sxs-lookup"><span data-stu-id="714c0-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="714c0-125">Следуйте указаниям в разделе [Подготовка приложения к использованию компонентов в страницах и представлениях](#prepare-the-app-to-use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="714c0-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="714c0-126">Добавьте файл *app. Razor* в корень проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="714c0-126">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="714c0-127">Добавьте файл *_Host. cshtml* в папку *pages* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="714c0-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="714c0-128">Компоненты используют общий файл *_layout. cshtml* для их макета.</span><span class="sxs-lookup"><span data-stu-id="714c0-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="714c0-129">Добавьте маршрут с низким приоритетом для страницы *_Host. cshtml* в конфигурацию конечной точки в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="714c0-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="714c0-130">Добавьте маршрутизируемые компоненты в приложение.</span><span class="sxs-lookup"><span data-stu-id="714c0-130">Add routable components to the app.</span></span> <span data-ttu-id="714c0-131">Пример:</span><span class="sxs-lookup"><span data-stu-id="714c0-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="714c0-132">Дополнительные сведения о пространствах имен см. в разделе [пространства имен компонентов](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="714c0-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="714c0-133">Использование маршрутизируемых компонентов в приложении MVC</span><span class="sxs-lookup"><span data-stu-id="714c0-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="714c0-134">*Этот раздел относится к добавлению компонентов, напрямую направляемых из запросов пользователей.*</span><span class="sxs-lookup"><span data-stu-id="714c0-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="714c0-135">Для поддержки маршрутизации компонентов Razor в приложениях MVC:</span><span class="sxs-lookup"><span data-stu-id="714c0-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="714c0-136">Следуйте указаниям в разделе [Подготовка приложения к использованию компонентов в страницах и представлениях](#prepare-the-app-to-use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="714c0-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="714c0-137">Добавьте файл *app. Razor* в корневую папку проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="714c0-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="714c0-138">Добавьте файл *_Host. cshtml* в папку *Views/Home* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="714c0-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="714c0-139">Компоненты используют общий файл *_layout. cshtml* для их макета.</span><span class="sxs-lookup"><span data-stu-id="714c0-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="714c0-140">Добавьте действие в контроллер Home:</span><span class="sxs-lookup"><span data-stu-id="714c0-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="714c0-141">Добавьте маршрут с низким приоритетом для действия контроллера, которое возвращает представление *_Host. cshtml* в конфигурацию конечной точки в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="714c0-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="714c0-142">Создайте папку *pages* и добавьте в приложение маршрутизируемые компоненты.</span><span class="sxs-lookup"><span data-stu-id="714c0-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="714c0-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="714c0-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="714c0-144">Дополнительные сведения о пространствах имен см. в разделе [пространства имен компонентов](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="714c0-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="714c0-145">Пространства имен компонентов</span><span class="sxs-lookup"><span data-stu-id="714c0-145">Component namespaces</span></span>

<span data-ttu-id="714c0-146">При использовании пользовательской папки для хранения компонентов приложения добавьте пространство имен, представляющее папку, в страницу или представление либо в файл *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="714c0-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="714c0-147">Рассмотрим следующий пример:</span><span class="sxs-lookup"><span data-stu-id="714c0-147">In the following example:</span></span>

* <span data-ttu-id="714c0-148">Замените `MyAppNamespace` пространством имен приложения.</span><span class="sxs-lookup"><span data-stu-id="714c0-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="714c0-149">Если папка с именем *Components* не используется для хранения компонентов, измените `Components` в папку, в которой находятся компоненты.</span><span class="sxs-lookup"><span data-stu-id="714c0-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="714c0-150">Файл *_ViewImports. cshtml* находится в папке *pages* приложения Razor Pages или в папке *views* приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="714c0-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="714c0-151">Дополнительные сведения см. в разделе <xref:blazor/components#import-components>.</span><span class="sxs-lookup"><span data-stu-id="714c0-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="714c0-152">Отрисовка компонентов из страницы или представления</span><span class="sxs-lookup"><span data-stu-id="714c0-152">Render components from a page or view</span></span>

<span data-ttu-id="714c0-153">*Этот раздел относится к добавлению компонентов к страницам или представлениям, в которых компоненты не маршрутизируются напрямую из запросов пользователей.*</span><span class="sxs-lookup"><span data-stu-id="714c0-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="714c0-154">Чтобы отобразить компонент из страницы или представления, используйте вспомогательную функцию тега `Component`:</span><span class="sxs-lookup"><span data-stu-id="714c0-154">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="714c0-155">Тип параметра должен быть сериализуемым JSON, что обычно означает, что тип должен иметь конструктор по умолчанию и устанавливаемые свойства.</span><span class="sxs-lookup"><span data-stu-id="714c0-155">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="714c0-156">Например, можно указать значение для `IncrementAmount`, так как тип `IncrementAmount` является `int`, который является типом-примитивом, поддерживаемым сериализатором JSON.</span><span class="sxs-lookup"><span data-stu-id="714c0-156">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="714c0-157">`RenderMode` настраивает, является ли компонент:</span><span class="sxs-lookup"><span data-stu-id="714c0-157">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="714c0-158">Предварительно отображается на странице.</span><span class="sxs-lookup"><span data-stu-id="714c0-158">Is prerendered into the page.</span></span>
* <span data-ttu-id="714c0-159">Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Блазор из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="714c0-159">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="714c0-160">Description</span><span class="sxs-lookup"><span data-stu-id="714c0-160">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="714c0-161">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="714c0-161">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="714c0-162">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="714c0-162">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="714c0-163">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="714c0-163">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="714c0-164">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="714c0-164">Output from the component isn't included.</span></span> <span data-ttu-id="714c0-165">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="714c0-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="714c0-166">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="714c0-166">Renders the component into static HTML.</span></span> |

<span data-ttu-id="714c0-167">Хотя страницы и представления могут использовать компоненты, наоборот это не так.</span><span class="sxs-lookup"><span data-stu-id="714c0-167">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="714c0-168">Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="714c0-168">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="714c0-169">Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="714c0-169">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="714c0-170">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="714c0-170">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="714c0-171">Дополнительные сведения о подготовке компонентов к просмотру, состоянии компонентов и вспомогательной функции тега `Component` см. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="714c0-171">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
