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
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a>Интеграция ASP.NET Core компонентов Razor в Razor Pages и приложения MVC

[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)

Компоненты Razor можно интегрировать в Razor Pages и приложения MVC. При отображении страницы или представления компоненты можно предварительно отобразить в одно и то же время.

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a>Подготовка приложения к использованию компонентов в страницах и представлениях

Существующее приложение Razor Pages или MVC может интегрировать компоненты Razor в страницы и представления:

1. В файле макета приложения ( *_layout. cshtml*):

   * Добавьте следующий тег `<base>` в элемент `<head>`:

     ```html
     <base href="~/" />
     ```

     Значение `href` ( *базовый путь приложения*) в предыдущем примере предполагает, что приложение находится по КОРНЕВОМУ URL-пути (`/`). Если приложение является вложенным приложением, следуйте указаниям в разделе " *базовый путь приложения* " статьи <xref:host-and-deploy/blazor/index#app-base-path>.

     Файл *_layout. cshtml* находится в папке *pages/shared* в приложении Razor Pages или в папке *views/Shared* приложения MVC.

   * Добавьте тег `<script>` для скрипта *блазор. Server. js* непосредственно перед закрывающим тегом `</body>`:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     Платформа добавляет в приложение скрипт *блазор. Server. js* . Нет необходимости вручную добавлять скрипт в приложение.

1. Добавьте файл *_Imports. Razor* в корневую папку проекта со следующим содержимым (измените Последнее пространство имен, `MyAppNamespace`, в пространство имен приложения):

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

1. В `Startup.ConfigureServices`Зарегистрируйте службу сервера Блазор:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. В `Startup.Configure`Добавьте конечную точку концентратора Блазор в `app.UseEndpoints`:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Интегрируйте компоненты в любую страницу или представление. Дополнительные сведения см. в разделе [подготовка компонентов с помощью страницы или представления](#render-components-from-a-page-or-view) .

## <a name="use-routable-components-in-a-razor-pages-app"></a>Использование маршрутизируемых компонентов в приложении Razor Pages

*Этот раздел относится к добавлению компонентов, напрямую направляемых из запросов пользователей.*

Для поддержки маршрутизируемых компонентов Razor в Razor Pages приложениях:

1. Следуйте указаниям в разделе [Подготовка приложения к использованию компонентов в страницах и представлениях](#prepare-the-app-to-use-components-in-pages-and-views) .

1. Добавьте файл *app. Razor* в корень проекта со следующим содержимым:

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

1. Добавьте файл *_Host. cshtml* в папку *pages* со следующим содержимым:

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Компоненты используют общий файл *_layout. cshtml* для их макета.

1. Добавьте маршрут с низким приоритетом для страницы *_Host. cshtml* в конфигурацию конечной точки в `Startup.Configure`:

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

   Дополнительные сведения о пространствах имен см. в разделе [пространства имен компонентов](#component-namespaces) .

## <a name="use-routable-components-in-an-mvc-app"></a>Использование маршрутизируемых компонентов в приложении MVC

*Этот раздел относится к добавлению компонентов, напрямую направляемых из запросов пользователей.*

Для поддержки маршрутизации компонентов Razor в приложениях MVC:

1. Следуйте указаниям в разделе [Подготовка приложения к использованию компонентов в страницах и представлениях](#prepare-the-app-to-use-components-in-pages-and-views) .

1. Добавьте файл *app. Razor* в корневую папку проекта со следующим содержимым:

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

1. Добавьте файл *_Host. cshtml* в папку *Views/Home* со следующим содержимым:

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Компоненты используют общий файл *_layout. cshtml* для их макета.

1. Добавьте действие в контроллер Home:

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. Добавьте маршрут с низким приоритетом для действия контроллера, которое возвращает представление *_Host. cshtml* в конфигурацию конечной точки в `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. Создайте папку *pages* и добавьте в приложение маршрутизируемые компоненты. Пример:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Дополнительные сведения о пространствах имен см. в разделе [пространства имен компонентов](#component-namespaces) .

## <a name="component-namespaces"></a>Пространства имен компонентов

При использовании пользовательской папки для хранения компонентов приложения добавьте пространство имен, представляющее папку, в страницу или представление либо в файл *_ViewImports. cshtml* . Рассмотрим следующий пример:

* Замените `MyAppNamespace` пространством имен приложения.
* Если папка с именем *Components* не используется для хранения компонентов, измените `Components` в папку, в которой находятся компоненты.

```cshtml
@using MyAppNamespace.Components
```

Файл *_ViewImports. cshtml* находится в папке *pages* приложения Razor Pages или в папке *views* приложения MVC.

Дополнительные сведения см. в разделе <xref:blazor/components#import-components>.

## <a name="render-components-from-a-page-or-view"></a>Отрисовка компонентов из страницы или представления

*Этот раздел относится к добавлению компонентов к страницам или представлениям, в которых компоненты не маршрутизируются напрямую из запросов пользователей.*

Чтобы отобразить компонент из страницы или представления, используйте вспомогательную функцию тега `Component`:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

Тип параметра должен быть сериализуемым JSON, что обычно означает, что тип должен иметь конструктор по умолчанию и устанавливаемые свойства. Например, можно указать значение для `IncrementAmount`, так как тип `IncrementAmount` является `int`, который является типом-примитивом, поддерживаемым сериализатором JSON.

`RenderMode` настраивает, является ли компонент:

* Предварительно отображается на странице.
* Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Блазор из агента пользователя.

| `RenderMode`        | Description |
| ------------------- | ----------- |
| `ServerPrerendered` | Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения. При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения. |
| `Server`            | Отображает маркер для серверного приложения Blazor. Выходные данные компонента не включаются. При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения. |
| `Static`            | Преобразует компонент в статический HTML. |

Хотя страницы и представления могут использовать компоненты, наоборот это не так. Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы. Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.

Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.

Дополнительные сведения о подготовке компонентов к просмотру, состоянии компонентов и вспомогательной функции тега `Component` см. в следующих статьях:

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
