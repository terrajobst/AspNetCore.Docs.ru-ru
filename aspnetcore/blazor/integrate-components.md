---
title: Интеграция компонентов Razor ASP.NET Core в приложения MVC и Razor Pages
author: guardrex
description: Сведения о сценариях привязки к данным в компонентах и элементах модели DOM в приложениях Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: cf6056e0985d5433bddecac8dd183ca3f4c2af5b
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218938"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a>Интеграция компонентов Razor ASP.NET Core в приложения MVC и Razor Pages

Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Дэниэл Рот](https://github.com/danroth27) (Daniel Roth)

Компоненты Razor можно интегрировать в приложения MVC и Razor Pages. Одновременно с отрисовкой страницы или представления можно выполнять предварительную обработку компонентов.

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a>Подготовка приложения к использованию компонентов на страницах и в представлениях

Существующее приложение MVC или Razor Pages может интегрировать компоненты Razor в страницы и представления:

1. В файле макета приложения ( *_Layout.cshtml*) сделайте следующее:

   * Добавьте следующий тег `<base>` в элемент `<head>`:

     ```html
     <base href="~/" />
     ```

     Значение `href` (*базовый путь к приложению* ) в предыдущем примере предполагает, что приложение находится по корневому URL-пути (`/`). Если приложение является подчиненным, следуйте инструкциям в разделе *Базовый путь к приложению* статьи <xref:host-and-deploy/blazor/index#app-base-path>.

     Файл *_Layout.cshtml* находится в папке *Pages/Shared* приложения Razor Pages или папке *Views/Shared* приложения MVC.

   * Добавьте тег `<script>` для скрипта *blazor.server.js* непосредственно перед закрывающим тегом `</body>`:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     Платформа добавляет скрипт *blazor.server.js* в приложение. Добавлять скрипт в приложение вручную не требуется.

1. Добавьте файл *_Imports.razor* в корневую папку проекта со следующим содержимым (измените последнее пространство имен `MyAppNamespace` на пространство имен приложения):

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

1. В `Startup.ConfigureServices` зарегистрируйте службу Blazor Server.

   ```csharp
   services.AddServerSideBlazor();
   ```

1. В `Startup.Configure` добавьте конечную точку Blazor Hub в `app.UseEndpoints`.

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Интегрируйте компоненты в какую-либо страницу или какое-либо представление. Дополнительные сведения см. в разделе [Отрисовка компонентов со страницы или представления](#render-components-from-a-page-or-view).

## <a name="use-routable-components-in-a-razor-pages-app"></a>Использование маршрутизируемых компонентов в приложении Razor Pages

*Этот раздел описывает добавление компонентов, напрямую маршрутизируемых из запросов пользователей.*

Для поддержки маршрутизируемых компонентов Razor в приложениях Razor Pages сделайте следующее:

1. Следуйте указаниям в разделе [Подготовка приложения к использованию компонентов на страницах и в представлениях](#prepare-the-app-to-use-components-in-pages-and-views).

1. Добавьте файл *App.razor* в корневой каталог проекта со следующим содержимым:

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

1. Добавьте файл *_Host.cshtml* в папку *Pages* со следующим содержимым:

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Для макета компоненты используют общий файл *_Layout.cshtml*.

1. Добавьте маршрут с низким приоритетом для страницы *_Host.cshtml* в конфигурацию конечной точки в `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. Добавьте маршрутизируемые компоненты в приложение. Пример:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Дополнительные сведения о пространствах имен см. в разделе [Пространства имен компонентов](#component-namespaces).

## <a name="use-routable-components-in-an-mvc-app"></a>Использование маршрутизируемых компонентов в приложении MVC

*Этот раздел описывает добавление компонентов, напрямую маршрутизируемых из запросов пользователей.*

Для поддержки маршрутизируемых компонентов Razor в приложениях MVC сделайте следующее:

1. Следуйте указаниям в разделе [Подготовка приложения к использованию компонентов на страницах и в представлениях](#prepare-the-app-to-use-components-in-pages-and-views).

1. Добавьте файл *App.razor* в корневой каталог проекта со следующим содержимым:

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

1. Добавьте файл *_Host.cshtml* в папку *Views/Home* со следующим содержимым:

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Для макета компоненты используют общий файл *_Layout.cshtml*.

1. Добавьте действие в контроллер Home:

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. Добавьте маршрут с низким приоритетом для действия контроллера, которое возвращает представление *_Host.cshtml*, в конфигурацию конечной точки в `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. Создайте папку *Pages* и добавьте маршрутизируемые компоненты в приложение. Пример:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Дополнительные сведения о пространствах имен см. в разделе [Пространства имен компонентов](#component-namespaces).

## <a name="component-namespaces"></a>Пространства имен компонентов

При использовании настраиваемой папки для хранения компонентов приложения добавьте пространство имен, представляющее эту папку, на страницу или в представление либо в файл *_ViewImports.cshtml*. Рассмотрим следующий пример:

* Измените `MyAppNamespace` на пространство имен приложения.
* Если папка с именем *Components* не используется для хранения компонентов, измените `Components` на папку, где находятся компоненты.

```cshtml
@using MyAppNamespace.Components
```

Файл *_ViewImports.cshtml* находится в папке *Pages* приложения Razor Pages или папке *Views* приложения MVC.

Дополнительные сведения см. в разделе <xref:blazor/components#import-components>.

## <a name="render-components-from-a-page-or-view"></a>Отрисовка компонентов со страницы или представления

*Этот раздел описывает добавление на страницы или в представления компонентов, не являющихся напрямую маршрутизируемыми из запросов пользователей.*

Чтобы отрисовать компонент из страницы или представления, используйте [вспомогательную функцию тега компонента](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).

Дополнительные сведения об отрисовке компонентов, состоянии компонентов и вспомогательной функции тегов `Component` см. в следующих статьях:

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
* <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>
