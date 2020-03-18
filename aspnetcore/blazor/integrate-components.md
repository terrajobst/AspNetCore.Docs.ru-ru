---
title: Интеграция компонентов Razor ASP.NET Core в приложения MVC и Razor Pages
author: guardrex
description: Сведения о сценариях привязки к данным в компонентах и элементах модели DOM в приложениях Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: de1a37ffd9456c956e3d84fcc69431ecb794513c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78649084"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="0245f-103">Интеграция компонентов Razor ASP.NET Core в приложения MVC и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0245f-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="0245f-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Дэниэл Рот](https://github.com/danroth27) (Daniel Roth)</span><span class="sxs-lookup"><span data-stu-id="0245f-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="0245f-105">Компоненты Razor можно интегрировать в приложения MVC и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0245f-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="0245f-106">Одновременно с отрисовкой страницы или представления можно выполнять предварительную обработку компонентов.</span><span class="sxs-lookup"><span data-stu-id="0245f-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="0245f-107">Подготовка приложения к использованию компонентов на страницах и в представлениях</span><span class="sxs-lookup"><span data-stu-id="0245f-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="0245f-108">Существующее приложение MVC или Razor Pages может интегрировать компоненты Razor в страницы и представления:</span><span class="sxs-lookup"><span data-stu-id="0245f-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="0245f-109">В файле макета приложения ( *_Layout.cshtml*) сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="0245f-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="0245f-110">Добавьте следующий тег `<base>` в элемент `<head>`:</span><span class="sxs-lookup"><span data-stu-id="0245f-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="0245f-111">Значение `href` (*базовый путь к приложению* ) в предыдущем примере предполагает, что приложение находится по корневому URL-пути (`/`).</span><span class="sxs-lookup"><span data-stu-id="0245f-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="0245f-112">Если приложение является подчиненным, следуйте инструкциям в разделе *Базовый путь к приложению* статьи <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="0245f-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="0245f-113">Файл *_Layout.cshtml* находится в папке *Pages/Shared* приложения Razor Pages или папке *Views/Shared* приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="0245f-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="0245f-114">Добавьте тег `<script>` для скрипта *blazor.server.js* непосредственно перед закрывающим тегом `</body>`:</span><span class="sxs-lookup"><span data-stu-id="0245f-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="0245f-115">Платформа добавляет скрипт *blazor.server.js* в приложение.</span><span class="sxs-lookup"><span data-stu-id="0245f-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="0245f-116">Добавлять скрипт в приложение вручную не требуется.</span><span class="sxs-lookup"><span data-stu-id="0245f-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="0245f-117">Добавьте файл *_Imports.razor* в корневую папку проекта со следующим содержимым (измените последнее пространство имен `MyAppNamespace` на пространство имен приложения):</span><span class="sxs-lookup"><span data-stu-id="0245f-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="0245f-118">В `Startup.ConfigureServices` зарегистрируйте службу Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="0245f-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="0245f-119">В `Startup.Configure` добавьте конечную точку Blazor Hub в `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="0245f-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="0245f-120">Интегрируйте компоненты в какую-либо страницу или какое-либо представление.</span><span class="sxs-lookup"><span data-stu-id="0245f-120">Integrate components into any page or view.</span></span> <span data-ttu-id="0245f-121">Дополнительные сведения см. в разделе [Отрисовка компонентов со страницы или представления](#render-components-from-a-page-or-view).</span><span class="sxs-lookup"><span data-stu-id="0245f-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="0245f-122">Использование маршрутизируемых компонентов в приложении Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0245f-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="0245f-123">*Этот раздел описывает добавление компонентов, напрямую маршрутизируемых из запросов пользователей.*</span><span class="sxs-lookup"><span data-stu-id="0245f-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="0245f-124">Для поддержки маршрутизируемых компонентов Razor в приложениях Razor Pages сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="0245f-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="0245f-125">Следуйте указаниям в разделе [Подготовка приложения к использованию компонентов на страницах и в представлениях](#prepare-the-app-to-use-components-in-pages-and-views).</span><span class="sxs-lookup"><span data-stu-id="0245f-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="0245f-126">Добавьте файл *App.razor* в корневой каталог проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="0245f-126">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="0245f-127">Добавьте файл *_Host.cshtml* в папку *Pages* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="0245f-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="0245f-128">Для макета компоненты используют общий файл *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0245f-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="0245f-129">Добавьте маршрут с низким приоритетом для страницы *_Host.cshtml* в конфигурацию конечной точки в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0245f-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="0245f-130">Добавьте маршрутизируемые компоненты в приложение.</span><span class="sxs-lookup"><span data-stu-id="0245f-130">Add routable components to the app.</span></span> <span data-ttu-id="0245f-131">Пример:</span><span class="sxs-lookup"><span data-stu-id="0245f-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="0245f-132">Дополнительные сведения о пространствах имен см. в разделе [Пространства имен компонентов](#component-namespaces).</span><span class="sxs-lookup"><span data-stu-id="0245f-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="0245f-133">Использование маршрутизируемых компонентов в приложении MVC</span><span class="sxs-lookup"><span data-stu-id="0245f-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="0245f-134">*Этот раздел описывает добавление компонентов, напрямую маршрутизируемых из запросов пользователей.*</span><span class="sxs-lookup"><span data-stu-id="0245f-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="0245f-135">Для поддержки маршрутизируемых компонентов Razor в приложениях MVC сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="0245f-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="0245f-136">Следуйте указаниям в разделе [Подготовка приложения к использованию компонентов на страницах и в представлениях](#prepare-the-app-to-use-components-in-pages-and-views).</span><span class="sxs-lookup"><span data-stu-id="0245f-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="0245f-137">Добавьте файл *App.razor* в корневой каталог проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="0245f-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="0245f-138">Добавьте файл *_Host.cshtml* в папку *Views/Home* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="0245f-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="0245f-139">Для макета компоненты используют общий файл *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0245f-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="0245f-140">Добавьте действие в контроллер Home:</span><span class="sxs-lookup"><span data-stu-id="0245f-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="0245f-141">Добавьте маршрут с низким приоритетом для действия контроллера, которое возвращает представление *_Host.cshtml*, в конфигурацию конечной точки в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0245f-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="0245f-142">Создайте папку *Pages* и добавьте маршрутизируемые компоненты в приложение.</span><span class="sxs-lookup"><span data-stu-id="0245f-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="0245f-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="0245f-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="0245f-144">Дополнительные сведения о пространствах имен см. в разделе [Пространства имен компонентов](#component-namespaces).</span><span class="sxs-lookup"><span data-stu-id="0245f-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="0245f-145">Пространства имен компонентов</span><span class="sxs-lookup"><span data-stu-id="0245f-145">Component namespaces</span></span>

<span data-ttu-id="0245f-146">При использовании настраиваемой папки для хранения компонентов приложения добавьте пространство имен, представляющее эту папку, на страницу или в представление либо в файл *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0245f-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="0245f-147">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0245f-147">In the following example:</span></span>

* <span data-ttu-id="0245f-148">Измените `MyAppNamespace` на пространство имен приложения.</span><span class="sxs-lookup"><span data-stu-id="0245f-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="0245f-149">Если папка с именем *Components* не используется для хранения компонентов, измените `Components` на папку, где находятся компоненты.</span><span class="sxs-lookup"><span data-stu-id="0245f-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="0245f-150">Файл *_ViewImports.cshtml* находится в папке *Pages* приложения Razor Pages или папке *Views* приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="0245f-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="0245f-151">Для получения дополнительной информации см. <xref:blazor/components#import-components>.</span><span class="sxs-lookup"><span data-stu-id="0245f-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="0245f-152">Отрисовка компонентов со страницы или представления</span><span class="sxs-lookup"><span data-stu-id="0245f-152">Render components from a page or view</span></span>

<span data-ttu-id="0245f-153">*Этот раздел описывает добавление на страницы или в представления компонентов, не являющихся напрямую маршрутизируемыми из запросов пользователей.*</span><span class="sxs-lookup"><span data-stu-id="0245f-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="0245f-154">Чтобы отрисовать компонент из страницы или представления, используйте вспомогательную функцию тегов `Component`:</span><span class="sxs-lookup"><span data-stu-id="0245f-154">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="0245f-155">Тип параметра должен быть JSON-сериализуемым, что обычно означает, что тип должен иметь конструктор по умолчанию и устанавливаемые свойства.</span><span class="sxs-lookup"><span data-stu-id="0245f-155">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="0245f-156">Например, можно указать значение для `IncrementAmount`, так как тип `IncrementAmount` является `int`, то есть примитивным типом, поддерживаемым сериализатором JSON.</span><span class="sxs-lookup"><span data-stu-id="0245f-156">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="0245f-157">Параметр `RenderMode` настраивает одно из следующих поведений компонента:</span><span class="sxs-lookup"><span data-stu-id="0245f-157">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="0245f-158">компонент предварительно преобразуется в страницу;</span><span class="sxs-lookup"><span data-stu-id="0245f-158">Is prerendered into the page.</span></span>
* <span data-ttu-id="0245f-159">компонент отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Blazor из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="0245f-159">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="0245f-160">Описание</span><span class="sxs-lookup"><span data-stu-id="0245f-160">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="0245f-161">Преобразует компонент в статический HTML и включает метку приложения Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="0245f-161">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="0245f-162">При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="0245f-162">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="0245f-163">Отображает метку приложения Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="0245f-163">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="0245f-164">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="0245f-164">Output from the component isn't included.</span></span> <span data-ttu-id="0245f-165">При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="0245f-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="0245f-166">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="0245f-166">Renders the component into static HTML.</span></span> |

<span data-ttu-id="0245f-167">Хотя страницы и представления могут использовать компоненты, обратное неверно.</span><span class="sxs-lookup"><span data-stu-id="0245f-167">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="0245f-168">Компоненты не могут использовать сценарии для конкретных представлений и страниц, например частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="0245f-168">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="0245f-169">Чтобы использовать логику из частичного представления в компоненте, вынесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="0245f-169">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="0245f-170">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="0245f-170">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="0245f-171">Дополнительные сведения об отрисовке компонентов, состоянии компонентов и вспомогательной функции тегов `Component` см. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="0245f-171">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
