---
title: Конфигурация модели размещения Blazor ASP.NET Core
author: guardrex
description: Сведения о конфигурации модели размещения Blazor, в том числе о том, как интегрировать компоненты Razor в Razor Pages и приложения MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: bd44643877e45c5b48b0972bcc2f637fbc5d98f2
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447039"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="73015-103">Конфигурация модели размещения Блазор ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73015-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="73015-104">По [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="73015-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="73015-105">В этой статье рассматривается Конфигурация модели размещения.</span><span class="sxs-lookup"><span data-stu-id="73015-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="73015-106">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="73015-106">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="73015-107">Отражение состояния соединения в пользовательском интерфейсе</span><span class="sxs-lookup"><span data-stu-id="73015-107">Reflect the connection state in the UI</span></span>

<span data-ttu-id="73015-108">Когда клиент обнаруживает, что соединение потеряно, пользователь по умолчанию отображает пользовательский интерфейс, когда клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="73015-108">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="73015-109">Если происходит сбой повторного подключения, пользователь предоставляет возможность повторить попытку.</span><span class="sxs-lookup"><span data-stu-id="73015-109">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="73015-110">Чтобы настроить пользовательский интерфейс, определите элемент с `id` `components-reconnect-modal` в `<body>` страницы Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="73015-110">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="73015-111">В следующей таблице описаны классы CSS, применяемые к элементу `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="73015-111">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="73015-112">Класс CSS</span><span class="sxs-lookup"><span data-stu-id="73015-112">CSS class</span></span>                       | <span data-ttu-id="73015-113">Указывает,&hellip;</span><span class="sxs-lookup"><span data-stu-id="73015-113">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="73015-114">Утерянное подключение.</span><span class="sxs-lookup"><span data-stu-id="73015-114">A lost connection.</span></span> <span data-ttu-id="73015-115">Клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="73015-115">The client is attempting to reconnect.</span></span> <span data-ttu-id="73015-116">Отображение модального окна.</span><span class="sxs-lookup"><span data-stu-id="73015-116">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="73015-117">Активное подключение будет восстановлено на сервере.</span><span class="sxs-lookup"><span data-stu-id="73015-117">An active connection is re-established to the server.</span></span> <span data-ttu-id="73015-118">Скрыть модальное окно.</span><span class="sxs-lookup"><span data-stu-id="73015-118">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="73015-119">Сбой повторного подключения, возможно, из-за сбоя сети.</span><span class="sxs-lookup"><span data-stu-id="73015-119">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="73015-120">Чтобы повторить попытку подключения, вызовите `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="73015-120">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="73015-121">Повторное подключение отклонено.</span><span class="sxs-lookup"><span data-stu-id="73015-121">Reconnection rejected.</span></span> <span data-ttu-id="73015-122">Сервер был достигнут, но отклонил подключение, и состояние пользователя на сервере потеряно.</span><span class="sxs-lookup"><span data-stu-id="73015-122">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="73015-123">Чтобы перезагрузить приложение, вызовите `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="73015-123">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="73015-124">Это состояние соединения может появиться в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="73015-124">This connection state may result when:</span></span><ul><li><span data-ttu-id="73015-125">Происходит сбой в цепи на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="73015-125">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="73015-126">Клиент отключается достаточно долго, чтобы сервер отключил состояние пользователя.</span><span class="sxs-lookup"><span data-stu-id="73015-126">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="73015-127">Удаляются экземпляры компонентов, с которыми взаимодействует пользователь.</span><span class="sxs-lookup"><span data-stu-id="73015-127">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="73015-128">Сервер перезагружается, или рабочий процесс приложения перезапускается.</span><span class="sxs-lookup"><span data-stu-id="73015-128">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="73015-129">Режим обработки</span><span class="sxs-lookup"><span data-stu-id="73015-129">Render mode</span></span>

<span data-ttu-id="73015-130">Серверные приложения блазор по умолчанию настроены на выстройку пользовательского интерфейса на сервере, прежде чем будет установлено клиентское соединение с сервером.</span><span class="sxs-lookup"><span data-stu-id="73015-130">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="73015-131">Это настраивается на странице Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="73015-131">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="73015-132">`RenderMode` настраивает, является ли компонент:</span><span class="sxs-lookup"><span data-stu-id="73015-132">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="73015-133">Предварительно отображается на странице.</span><span class="sxs-lookup"><span data-stu-id="73015-133">Is prerendered into the page.</span></span>
* <span data-ttu-id="73015-134">Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Блазор из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="73015-134">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="73015-135">Описание</span><span class="sxs-lookup"><span data-stu-id="73015-135">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="73015-136">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="73015-136">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="73015-137">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="73015-137">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="73015-138">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="73015-138">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="73015-139">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="73015-139">Output from the component isn't included.</span></span> <span data-ttu-id="73015-140">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="73015-140">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="73015-141">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="73015-141">Renders the component into static HTML.</span></span> |

<span data-ttu-id="73015-142">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="73015-142">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="73015-143">Отображение интерактивных компонентов с отслеживанием состояния из страниц и представлений Razor</span><span class="sxs-lookup"><span data-stu-id="73015-143">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="73015-144">Интерактивные компоненты с отслеживанием состояния могут быть добавлены на страницу Razor или в представление.</span><span class="sxs-lookup"><span data-stu-id="73015-144">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="73015-145">При визуализации страницы или представления:</span><span class="sxs-lookup"><span data-stu-id="73015-145">When the page or view renders:</span></span>

* <span data-ttu-id="73015-146">Компонент предварительно отображается страницей или представлением.</span><span class="sxs-lookup"><span data-stu-id="73015-146">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="73015-147">Исходное состояние компонента, используемое для предварительной визуализации, потеряно.</span><span class="sxs-lookup"><span data-stu-id="73015-147">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="73015-148">Новое состояние компонента создается при установке подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="73015-148">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="73015-149">Следующая страница Razor визуализирует компонент `Counter`:</span><span class="sxs-lookup"><span data-stu-id="73015-149">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="73015-150">Прорисовка неинтерактивных компонентов на страницах и представлениях Razor</span><span class="sxs-lookup"><span data-stu-id="73015-150">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="73015-151">На следующей странице Razor компонент `Counter` статически подготавливается к просмотру с начальным значением, указанным с помощью формы:</span><span class="sxs-lookup"><span data-stu-id="73015-151">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="73015-152">Поскольку `MyComponent` является статически отображаемым, компонент не может быть интерактивным.</span><span class="sxs-lookup"><span data-stu-id="73015-152">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="73015-153">Настройка клиента SignalR для приложений Blazor Server</span><span class="sxs-lookup"><span data-stu-id="73015-153">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="73015-154">Иногда необходимо настроить клиент SignalR, используемый приложениями Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="73015-154">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="73015-155">Например, может потребоваться настроить ведение журнала на SignalRном клиенте для диагностики проблемы с подключением.</span><span class="sxs-lookup"><span data-stu-id="73015-155">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="73015-156">Настройка клиента SignalR в файле *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="73015-156">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="73015-157">Добавьте атрибут `autostart="false"` в тег `<script>` для скрипта `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="73015-157">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="73015-158">Вызовите `Blazor.start` и передайте объект конфигурации, указывающий построитель SignalR.</span><span class="sxs-lookup"><span data-stu-id="73015-158">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
