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
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>Конфигурация модели размещения Блазор ASP.NET Core

По [Даниэль Roth)](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

В этой статье рассматривается Конфигурация модели размещения.

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a>Blazor Server

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a>Интеграция компонентов Razor в приложения Razor Pages и MVC

#### <a name="use-components-in-pages-and-views"></a>Использование компонентов в страницах и представлениях

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

1. В `Startup.ConfigureServices`Зарегистрируйте службу сервера Блазор:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. В `Startup.Configure`Добавьте конечную точку концентратора Блазор в `app.UseEndpoints`:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Интегрируйте компоненты в любую страницу или представление. Дополнительные сведения см. в разделе *Интеграция компонентов в Razor Pages и приложения MVC* статьи <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-a-razor-pages-app"></a>Использование маршрутизируемых компонентов в приложении Razor Pages

Для поддержки маршрутизируемых компонентов Razor в Razor Pages приложениях:

1. Следуйте указаниям в разделе [Использование компонентов в страницах и представлениях](#use-components-in-pages-and-views) .

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

   При использовании пользовательской папки для хранения компонентов приложения добавьте пространство имен, представляющее папку, в файл *pages/_ViewImports. cshtml* . Дополнительные сведения см. в разделе <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-an-mvc-app"></a>Использование маршрутизируемых компонентов в приложении MVC

Для поддержки маршрутизации компонентов Razor в приложениях MVC:

1. Следуйте указаниям в разделе [Использование компонентов в страницах и представлениях](#use-components-in-pages-and-views) .

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

   При использовании пользовательской папки для хранения компонентов приложения добавьте пространство имен, представляющее папку, в файл *views/_ViewImports. cshtml* . Дополнительные сведения см. в разделе <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

### <a name="reflect-the-connection-state-in-the-ui"></a>Отражение состояния соединения в пользовательском интерфейсе

Когда клиент обнаруживает, что соединение потеряно, пользователь по умолчанию отображает пользовательский интерфейс, когда клиент пытается повторно подключиться. Если происходит сбой повторного подключения, пользователь предоставляет возможность повторить попытку.

Чтобы настроить пользовательский интерфейс, определите элемент с `id` `components-reconnect-modal` в `<body>` страницы Razor *_Host. cshtml* :

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

В следующей таблице описаны классы CSS, применяемые к элементу `components-reconnect-modal`.

| Класс CSS                       | Указывает,&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | Утерянное подключение. Клиент пытается повторно подключиться. Отображение модального окна. |
| `components-reconnect-hide`     | Активное подключение будет восстановлено на сервере. Скрыть модальное окно. |
| `components-reconnect-failed`   | Сбой повторного подключения, возможно, из-за сбоя сети. Чтобы повторить попытку подключения, вызовите `window.Blazor.reconnect()`. |
| `components-reconnect-rejected` | Повторное подключение отклонено. Сервер был достигнут, но отклонил подключение, и состояние пользователя на сервере потеряно. Чтобы перезагрузить приложение, вызовите `location.reload()`. Это состояние соединения может появиться в следующих случаях:<ul><li>Происходит сбой в цепи на стороне сервера.</li><li>Клиент отключается достаточно долго, чтобы сервер отключил состояние пользователя. Удаляются экземпляры компонентов, с которыми взаимодействует пользователь.</li><li>Сервер перезагружается, или рабочий процесс приложения перезапускается.</li></ul> |

### <a name="render-mode"></a>Режим обработки

Серверные приложения блазор по умолчанию настроены на выстройку пользовательского интерфейса на сервере, прежде чем будет установлено клиентское соединение с сервером. Это настраивается на странице Razor *_Host. cshtml* :

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode` настраивает, является ли компонент:

* Предварительно отображается на странице.
* Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Блазор из агента пользователя.

| `RenderMode`        | Description |
| ------------------- | ----------- |
| `ServerPrerendered` | Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения. При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения. |
| `Server`            | Отображает маркер для серверного приложения Blazor. Выходные данные компонента не включаются. При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения. |
| `Static`            | Преобразует компонент в статический HTML. |

Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Отображение интерактивных компонентов с отслеживанием состояния из страниц и представлений Razor

Интерактивные компоненты с отслеживанием состояния могут быть добавлены на страницу Razor или в представление.

При визуализации страницы или представления:

* Компонент предварительно отображается страницей или представлением.
* Исходное состояние компонента, используемое для предварительной визуализации, потеряно.
* Новое состояние компонента создается при установке подключения SignalR.

Следующая страница Razor визуализирует компонент `Counter`:

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Прорисовка неинтерактивных компонентов на страницах и представлениях Razor

На следующей странице Razor компонент `Counter` статически подготавливается к просмотру с начальным значением, указанным с помощью формы:

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

Поскольку `MyComponent` является статически отображаемым, компонент не может быть интерактивным.

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>Настройка клиента SignalR для приложений Blazor Server

Иногда необходимо настроить клиент SignalR, используемый приложениями Blazor Server. Например, может потребоваться настроить ведение журнала на SignalRном клиенте для диагностики проблемы с подключением.

Настройка клиента SignalR в файле *pages/_Host. cshtml* :

* Добавьте атрибут `autostart="false"` в тег `<script>` для скрипта `blazor.server.js`.
* Вызовите `Blazor.start` и передайте объект конфигурации, указывающий построитель SignalR.

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
