---
title: Конфигурация модели размещения ASP.NET Core Blazor
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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78646792"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="b45c2-103">Конфигурация модели размещения ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="b45c2-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="b45c2-104">Автор: [Дэниэл Рот](https://github.com/danroth27) (Daniel Roth)</span><span class="sxs-lookup"><span data-stu-id="b45c2-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="b45c2-105">В этой статье рассматривается конфигурация модели размещения.</span><span class="sxs-lookup"><span data-stu-id="b45c2-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="b45c2-106">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="b45c2-106">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="b45c2-107">Отражение состояния соединения в пользовательском интерфейсе</span><span class="sxs-lookup"><span data-stu-id="b45c2-107">Reflect the connection state in the UI</span></span>

<span data-ttu-id="b45c2-108">Когда клиент обнаруживает, что соединение было потеряно, пользовательский интерфейс по умолчанию отображается пользователю, в то время как клиент пытается восстановить соединение.</span><span class="sxs-lookup"><span data-stu-id="b45c2-108">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="b45c2-109">Если происходит повторный сбой подключения, пользователю предоставляется возможность повторить попытку.</span><span class="sxs-lookup"><span data-stu-id="b45c2-109">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="b45c2-110">Чтобы настроить пользовательский интерфейс, определите элемент с `id` равным `components-reconnect-modal` в блоке `<body>` страницы Razor *__Host.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b45c2-110">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="b45c2-111">В следующей таблице описаны классы CSS, применяемые к элементу `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="b45c2-111">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="b45c2-112">Класс CSS</span><span class="sxs-lookup"><span data-stu-id="b45c2-112">CSS class</span></span>                       | <span data-ttu-id="b45c2-113">Означает&hellip;</span><span class="sxs-lookup"><span data-stu-id="b45c2-113">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="b45c2-114">Потеря соединения.</span><span class="sxs-lookup"><span data-stu-id="b45c2-114">A lost connection.</span></span> <span data-ttu-id="b45c2-115">Клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="b45c2-115">The client is attempting to reconnect.</span></span> <span data-ttu-id="b45c2-116">Отобразить модальное окно.</span><span class="sxs-lookup"><span data-stu-id="b45c2-116">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="b45c2-117">Активное подключение к серверу восстановлено.</span><span class="sxs-lookup"><span data-stu-id="b45c2-117">An active connection is re-established to the server.</span></span> <span data-ttu-id="b45c2-118">Скрыть модальное окно.</span><span class="sxs-lookup"><span data-stu-id="b45c2-118">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="b45c2-119">Сбой повторного подключения, возможно, из-за сбоя сети.</span><span class="sxs-lookup"><span data-stu-id="b45c2-119">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="b45c2-120">Чтобы повторить попытку подключения, вызовите метод `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="b45c2-120">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="b45c2-121">Повторное подключение отклонено.</span><span class="sxs-lookup"><span data-stu-id="b45c2-121">Reconnection rejected.</span></span> <span data-ttu-id="b45c2-122">Сервер был достигнут, но отклонил подключение, и состояние пользователя на сервере потеряно.</span><span class="sxs-lookup"><span data-stu-id="b45c2-122">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="b45c2-123">Чтобы перезагрузить приложение, вызовите метод `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="b45c2-123">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="b45c2-124">Это состояние соединения может появиться в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="b45c2-124">This connection state may result when:</span></span><ul><li><span data-ttu-id="b45c2-125">произошел сбой в цепи на стороне сервера;</span><span class="sxs-lookup"><span data-stu-id="b45c2-125">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="b45c2-126">клиент был отключен достаточно долго, чтобы сервер сбросил состояние пользователя.</span><span class="sxs-lookup"><span data-stu-id="b45c2-126">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="b45c2-127">Экземпляры компонентов, с которыми взаимодействовал пользователь, удаляются;</span><span class="sxs-lookup"><span data-stu-id="b45c2-127">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="b45c2-128">сервер был перезагружен или был выполнен перезапуск рабочего процесса приложения.</span><span class="sxs-lookup"><span data-stu-id="b45c2-128">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="b45c2-129">Режим обработки</span><span class="sxs-lookup"><span data-stu-id="b45c2-129">Render mode</span></span>

<span data-ttu-id="b45c2-130">Приложения Blazor Server по умолчанию настроены на предварительную отрисовку пользовательского интерфейса на сервере, прежде чем будет установлено клиентское соединение с сервером.</span><span class="sxs-lookup"><span data-stu-id="b45c2-130">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="b45c2-131">Это поведение настраивается на странице Razor *_Host.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b45c2-131">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="b45c2-132">Параметр `RenderMode` настраивает одно из следующих поведений компонента:</span><span class="sxs-lookup"><span data-stu-id="b45c2-132">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="b45c2-133">компонент предварительно преобразуется в страницу;</span><span class="sxs-lookup"><span data-stu-id="b45c2-133">Is prerendered into the page.</span></span>
* <span data-ttu-id="b45c2-134">компонент отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Blazor из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="b45c2-134">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="b45c2-135">Описание</span><span class="sxs-lookup"><span data-stu-id="b45c2-135">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="b45c2-136">Преобразует компонент в статический HTML и включает метку приложения Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="b45c2-136">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="b45c2-137">При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="b45c2-137">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="b45c2-138">Отображает метку приложения Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="b45c2-138">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="b45c2-139">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="b45c2-139">Output from the component isn't included.</span></span> <span data-ttu-id="b45c2-140">При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="b45c2-140">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="b45c2-141">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="b45c2-141">Renders the component into static HTML.</span></span> |

<span data-ttu-id="b45c2-142">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="b45c2-142">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="b45c2-143">Отображение интерактивных компонентов с отслеживанием состояния из страниц и представлений Razor</span><span class="sxs-lookup"><span data-stu-id="b45c2-143">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="b45c2-144">На страницу или в представление Razor можно добавить интерактивные компоненты с отслеживанием состояния.</span><span class="sxs-lookup"><span data-stu-id="b45c2-144">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="b45c2-145">При отображении страницы или представления:</span><span class="sxs-lookup"><span data-stu-id="b45c2-145">When the page or view renders:</span></span>

* <span data-ttu-id="b45c2-146">компонент предварительно отображается страницей или представлением;</span><span class="sxs-lookup"><span data-stu-id="b45c2-146">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="b45c2-147">исходное состояние компонента, используемое для предварительной визуализации, теряется;</span><span class="sxs-lookup"><span data-stu-id="b45c2-147">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="b45c2-148">новое состояние компонента создается при установке подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="b45c2-148">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="b45c2-149">Следующая страница Razor визуализирует компонент `Counter`.</span><span class="sxs-lookup"><span data-stu-id="b45c2-149">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="b45c2-150">Отображение неинтерактивных компонентов из страниц и представлений Razor</span><span class="sxs-lookup"><span data-stu-id="b45c2-150">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="b45c2-151">На следующей странице Razor компонент `Counter` статически подготавливается к просмотру с начальным значением, указанным с помощью формы.</span><span class="sxs-lookup"><span data-stu-id="b45c2-151">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="b45c2-152">Поскольку `MyComponent` отображается статически, компонент не может быть интерактивным.</span><span class="sxs-lookup"><span data-stu-id="b45c2-152">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="b45c2-153">Настройка клиента SignalR для приложений Blazor Server</span><span class="sxs-lookup"><span data-stu-id="b45c2-153">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="b45c2-154">Иногда необходимо настроить клиент SignalR, используемый приложениями Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="b45c2-154">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="b45c2-155">Например, может потребоваться настроить ведение журнала на клиенте SignalR для диагностики проблемы с подключением.</span><span class="sxs-lookup"><span data-stu-id="b45c2-155">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="b45c2-156">Чтобы настроить клиент SignalR, внесите следующие изменения в файле *Pages/_Host.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b45c2-156">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="b45c2-157">добавьте атрибут `autostart="false"` в тег `<script>` для сценария `blazor.server.js`;</span><span class="sxs-lookup"><span data-stu-id="b45c2-157">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="b45c2-158">вызовите `Blazor.start` и передайте объект конфигурации, указывающий построитель SignalR.</span><span class="sxs-lookup"><span data-stu-id="b45c2-158">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
