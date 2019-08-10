---
title: ASP.NET Core моделей размещения Блазор
author: guardrex
description: Общие сведения о моделях размещения Блазор на стороне клиента и на стороне сервера.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 9dd96ff6e3539bf1c3e932b33776b16d0fbb2d34
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68948294"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="c8789-103">ASP.NET Core моделей размещения Блазор</span><span class="sxs-lookup"><span data-stu-id="c8789-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="c8789-104">По [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c8789-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="c8789-105">Блазор — это веб-платформа, предназначенная для запуска на стороне клиента в браузере на среде выполнения .NET на основе веб- [сборки](https://webassembly.org/)(*блазор на стороне клиента*) или на стороне сервера в ASP.NET Core (*блазор на стороне сервера*).</span><span class="sxs-lookup"><span data-stu-id="c8789-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="c8789-106">Независимо от модели размещения, модели приложения и компонента *одинаковы*.</span><span class="sxs-lookup"><span data-stu-id="c8789-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="c8789-107">Сведения о создании проекта для моделей размещения, описанных в этой статье, <xref:blazor/get-started>см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="c8789-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="c8789-108">Компоненты на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="c8789-108">Client-side</span></span>

<span data-ttu-id="c8789-109">Основная модель размещения для Блазор работает на стороне клиента в браузере на веб-сборке.</span><span class="sxs-lookup"><span data-stu-id="c8789-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="c8789-110">Приложение Blazor, его зависимости и среда выполнения .NET скачиваются в браузере.</span><span class="sxs-lookup"><span data-stu-id="c8789-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="c8789-111">Приложение выполняется непосредственно в потоке пользовательского интерфейса браузера.</span><span class="sxs-lookup"><span data-stu-id="c8789-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="c8789-112">Обновления пользовательского интерфейса и обработка событий выполняются в рамках одного процесса.</span><span class="sxs-lookup"><span data-stu-id="c8789-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="c8789-113">Ресурсы приложения развертываются как статические файлы на веб-сервере или в службе, способной обслуживать статические содержимое клиентам.</span><span class="sxs-lookup"><span data-stu-id="c8789-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Блазор на стороне клиента: Приложение Блазор выполняется в потоке пользовательского интерфейса в браузере.](hosting-models/_static/client-side.png)

<span data-ttu-id="c8789-115">Чтобы создать приложение Блазор с помощью клиентской модели размещения, используйте один из следующих шаблонов:</span><span class="sxs-lookup"><span data-stu-id="c8789-115">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="c8789-116">**Блазор (на стороне клиента)** ([DotNet New блазор](/dotnet/core/tools/dotnet-new)) &ndash; Развертывается как набор статических файлов.</span><span class="sxs-lookup"><span data-stu-id="c8789-116">**Blazor (client-side)** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="c8789-117">**Блазор (ASP.NET Core Hosted)** ([DotNet New блазорхостед](/dotnet/core/tools/dotnet-new)) &ndash; Размещается на сервере ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8789-117">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="c8789-118">ASP.NET Core приложение обслуживает приложения Блазор для клиентов.</span><span class="sxs-lookup"><span data-stu-id="c8789-118">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="c8789-119">Клиентское приложение Блазор может взаимодействовать с сервером по сети с помощью вызовов веб-API или [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="c8789-119">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="c8789-120">Шаблоны включают скрипт *блазор... js* , который обрабатывает:</span><span class="sxs-lookup"><span data-stu-id="c8789-120">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="c8789-121">Загрузка среды выполнения .NET, приложения и зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="c8789-121">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="c8789-122">Инициализация среды выполнения для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="c8789-122">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="c8789-123">Модель размещения на стороне клиента предлагает несколько преимуществ:</span><span class="sxs-lookup"><span data-stu-id="c8789-123">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="c8789-124">Зависимость на стороне сервера .NET отсутствует.</span><span class="sxs-lookup"><span data-stu-id="c8789-124">There's no .NET server-side dependency.</span></span> <span data-ttu-id="c8789-125">Приложение полностью работает после загрузки на клиент.</span><span class="sxs-lookup"><span data-stu-id="c8789-125">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="c8789-126">Ресурсы и возможности клиента полностью используются.</span><span class="sxs-lookup"><span data-stu-id="c8789-126">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="c8789-127">Работа разгружается с сервера на клиент.</span><span class="sxs-lookup"><span data-stu-id="c8789-127">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="c8789-128">Веб-сервер ASP.NET Core не требуется для размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="c8789-128">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="c8789-129">Возможны сценарии бессерверного развертывания (например, обслуживание приложения из CDN).</span><span class="sxs-lookup"><span data-stu-id="c8789-129">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="c8789-130">Существует недостаток для размещения на стороне клиента:</span><span class="sxs-lookup"><span data-stu-id="c8789-130">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="c8789-131">Приложение ограничено возможностями браузера.</span><span class="sxs-lookup"><span data-stu-id="c8789-131">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="c8789-132">Требуется аппаратное и программное обеспечение, поддерживающее клиент (например, поддержка сборок).</span><span class="sxs-lookup"><span data-stu-id="c8789-132">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="c8789-133">Размер скачивания больше, и приложения загружаются дольше.</span><span class="sxs-lookup"><span data-stu-id="c8789-133">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="c8789-134">Поддержка среды выполнения .NET и инструментария менее зрела.</span><span class="sxs-lookup"><span data-stu-id="c8789-134">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="c8789-135">Например, существуют ограничения на поддержку и отладку [.NET Standard](/dotnet/standard/net-standard) .</span><span class="sxs-lookup"><span data-stu-id="c8789-135">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="c8789-136">Компоненты на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="c8789-136">Server-side</span></span>

<span data-ttu-id="c8789-137">В модели размещения на стороне сервера приложение выполняется на сервере из приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8789-137">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="c8789-138">Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="c8789-138">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Браузер взаимодействует с приложением (размещенным в приложении ASP.NET Core) на сервере через подключение SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="c8789-140">Чтобы создать приложение Блазор с помощью модели размещения на стороне сервера, используйте шаблон серверное **приложение ASP.NET Core блазор** ([DotNet New блазорсерверсиде](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="c8789-140">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserverside](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="c8789-141">Приложение ASP.NET Core размещает приложение на стороне сервера и создает конечную точку SignalR, в которую подключаются клиенты.</span><span class="sxs-lookup"><span data-stu-id="c8789-141">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="c8789-142">ASP.NET Core приложение ссылается на `Startup` класс приложения для добавления:</span><span class="sxs-lookup"><span data-stu-id="c8789-142">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="c8789-143">Службы на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="c8789-143">Server-side services.</span></span>
* <span data-ttu-id="c8789-144">Приложение в конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="c8789-144">The app to the request handling pipeline.</span></span>

<span data-ttu-id="c8789-145">Сценарий&dagger; *блазор. Server. js* устанавливает клиентское соединение.</span><span class="sxs-lookup"><span data-stu-id="c8789-145">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="c8789-146">Ответственность за сохранение и восстановление состояния приложения в случае необходимости (например, в случае потери сетевого подключения) несет приложение.</span><span class="sxs-lookup"><span data-stu-id="c8789-146">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="c8789-147">Модель размещения на стороне сервера предлагает несколько преимуществ.</span><span class="sxs-lookup"><span data-stu-id="c8789-147">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="c8789-148">Размер скачивания значительно меньше клиентского приложения, и приложение загружается гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="c8789-148">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="c8789-149">Приложение обладает всеми преимуществами серверных возможностей, включая использование любых API-интерфейсов, совместимых с .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8789-149">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="c8789-150">.NET Core на сервере используется для запуска приложения, поэтому существующие инструменты .NET, такие как отладка, работают должным образом.</span><span class="sxs-lookup"><span data-stu-id="c8789-150">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="c8789-151">Поддерживаются тонкие клиенты.</span><span class="sxs-lookup"><span data-stu-id="c8789-151">Thin clients are supported.</span></span> <span data-ttu-id="c8789-152">Например, серверные приложения работают с браузерами, которые не поддерживают веб-сборку и устройства с ограниченными ресурсами.</span><span class="sxs-lookup"><span data-stu-id="c8789-152">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="c8789-153">База .NET (C# код) приложения, включая код компонента приложения, не обслуживаются для клиентов.</span><span class="sxs-lookup"><span data-stu-id="c8789-153">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="c8789-154">Существуют недостатки размещения на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="c8789-154">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="c8789-155">Обычно существует большая задержка.</span><span class="sxs-lookup"><span data-stu-id="c8789-155">Higher latency usually exists.</span></span> <span data-ttu-id="c8789-156">Каждое взаимодействие с пользователем включает сетевой прыжок.</span><span class="sxs-lookup"><span data-stu-id="c8789-156">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="c8789-157">Автономная поддержка отсутствует.</span><span class="sxs-lookup"><span data-stu-id="c8789-157">There's no offline support.</span></span> <span data-ttu-id="c8789-158">Если подключение клиента завершается сбоем, приложение перестает работать.</span><span class="sxs-lookup"><span data-stu-id="c8789-158">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="c8789-159">Масштабируемость — это сложная задача для приложений с большим количеством пользователей.</span><span class="sxs-lookup"><span data-stu-id="c8789-159">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="c8789-160">Сервер должен управлять несколькими клиентскими подключениями и обрабатывать состояние клиента.</span><span class="sxs-lookup"><span data-stu-id="c8789-160">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="c8789-161">Для обслуживания приложения требуется сервер ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8789-161">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="c8789-162">Сценарии бессерверного развертывания невозможны (например, обслуживание приложения из CDN).</span><span class="sxs-lookup"><span data-stu-id="c8789-162">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="c8789-163">&dagger;Сценарий *блазор. Server. js* обрабатывается из внедренного ресурса в ASP.NET Core общей платформе.</span><span class="sxs-lookup"><span data-stu-id="c8789-163">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="c8789-164">Повторное подключение к тому же серверу</span><span class="sxs-lookup"><span data-stu-id="c8789-164">Reconnection to the same server</span></span>

<span data-ttu-id="c8789-165">Для приложений на стороне сервера блазор требуется активное подключение SignalR к серверу.</span><span class="sxs-lookup"><span data-stu-id="c8789-165">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="c8789-166">Если соединение потеряно, приложение попытается повторно подключиться к серверу.</span><span class="sxs-lookup"><span data-stu-id="c8789-166">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="c8789-167">Пока состояние клиента по-прежнему находится в памяти, сеанс клиента возобновляется без потери состояния.</span><span class="sxs-lookup"><span data-stu-id="c8789-167">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="c8789-168">Когда клиент обнаруживает, что соединение потеряно, пользователь по умолчанию отображает пользовательский интерфейс, когда клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="c8789-168">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="c8789-169">Если происходит сбой повторного подключения, пользователь предоставляет возможность повторить попытку.</span><span class="sxs-lookup"><span data-stu-id="c8789-169">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="c8789-170">Чтобы настроить пользовательский интерфейс, определите элемент, указав `components-reconnect-modal` его `id` в качестве элемента на странице Razor *_Host. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="c8789-170">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="c8789-171">Клиент обновляет этот элемент одним из следующих классов CSS в зависимости от состояния соединения:</span><span class="sxs-lookup"><span data-stu-id="c8789-171">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="c8789-172">`components-reconnect-show`&ndash; Показать пользовательский интерфейс, указывающий, что соединение было потеряно, а клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="c8789-172">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="c8789-173">`components-reconnect-hide`&ndash; Клиент имеет активное соединение, скрыть пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="c8789-173">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="c8789-174">`components-reconnect-failed`&ndash; Сбой повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="c8789-174">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="c8789-175">Чтобы повторить попытку повторного подключения `window.Blazor.reconnect()`, вызовите.</span><span class="sxs-lookup"><span data-stu-id="c8789-175">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="c8789-176">Повторное подключение с отслеживанием состояния после предварительной подготовки к просмотру</span><span class="sxs-lookup"><span data-stu-id="c8789-176">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="c8789-177">Приложения блазор на стороне сервера настраиваются по умолчанию для создания интерфейса пользователя на сервере, прежде чем будет установлено клиентское соединение с сервером.</span><span class="sxs-lookup"><span data-stu-id="c8789-177">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="c8789-178">Это настраивается на странице Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="c8789-178">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="c8789-179">Клиент повторно подключится к серверу с тем же состоянием, которое использовалось для выprerender приложения.</span><span class="sxs-lookup"><span data-stu-id="c8789-179">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="c8789-180">Если состояние приложения по-прежнему находится в памяти, состояние компонента не будет повторно отображено после установления подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8789-180">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="c8789-181">Отображение интерактивных компонентов с отслеживанием состояния из страниц и представлений Razor</span><span class="sxs-lookup"><span data-stu-id="c8789-181">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="c8789-182">Интерактивные компоненты с отслеживанием состояния могут быть добавлены на страницу Razor или в представление.</span><span class="sxs-lookup"><span data-stu-id="c8789-182">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="c8789-183">При отрисовке страницы или представления компонент предварительно отображает его.</span><span class="sxs-lookup"><span data-stu-id="c8789-183">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="c8789-184">После установки клиентского подключения приложение снова подключится к состоянию компонента, пока состояние остается в памяти.</span><span class="sxs-lookup"><span data-stu-id="c8789-184">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="c8789-185">Например, Следующая страница Razor визуализирует `Counter` компонент с начальным счетчиком, указанным с помощью формы:</span><span class="sxs-lookup"><span data-stu-id="c8789-185">For example, the following Razor page renders a `Counter` component with an initial count that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialCount" />
    <button type="submit">Set initial count</button>
</form>
 
@(await Html.RenderComponentAsync<Counter>(new { InitialCount = InitialCount }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialCount { get; set; }
}
```

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="c8789-186">Обнаруживать время подготовки приложения к просмотру</span><span class="sxs-lookup"><span data-stu-id="c8789-186">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="c8789-187">Настройка клиента SignalR для приложений на стороне сервера Блазор</span><span class="sxs-lookup"><span data-stu-id="c8789-187">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="c8789-188">Иногда необходимо настроить клиент SignalR, используемый приложениями Блазор на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="c8789-188">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="c8789-189">Например, может потребоваться настроить ведение журнала на клиенте SignalR для диагностики проблемы с подключением.</span><span class="sxs-lookup"><span data-stu-id="c8789-189">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="c8789-190">Чтобы настроить клиент SignalR в файле Pages */_Host. cshtml* , сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="c8789-190">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="c8789-191">Добавьте атрибут в тег для скрипта *блазор. Server. js.* `<script>` `autostart="false"`</span><span class="sxs-lookup"><span data-stu-id="c8789-191">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="c8789-192">Вызовите `Blazor.start` и передайте объект конфигурации, указывающий построитель SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8789-192">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="c8789-193">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c8789-193">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
