---
title: Модели размещения Blazor ASP.NET Core
author: guardrex
description: Общие сведения о моделях размещения Blazor и Blazor Server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
- blazor.webassembly.js
uid: blazor/hosting-models
ms.openlocfilehash: c9521acf40317c90d1197660bfa516710263cfc9
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160045"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="c93c1-103">Модели размещения Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c93c1-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="c93c1-104">По [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c93c1-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="c93c1-105"> — это веб-платформа, предназначенная для запуска на стороне клиента в браузере в среде выполнения .NET на основе [сборки](https://webassembly.org/)(Blazor веб- *сборки*) или на стороне сервера в ASP.NET Core ( *Blazor Server*).</span><span class="sxs-lookup"><span data-stu-id="c93c1-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="c93c1-106">Независимо от модели размещения, модели приложения и компонента *одинаковы*.</span><span class="sxs-lookup"><span data-stu-id="c93c1-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="c93c1-107">Сведения о создании проекта для моделей размещения, описанных в этой статье, см. в разделе <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="c93c1-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="c93c1-108"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="c93c1-108"> WebAssembly</span></span>

<span data-ttu-id="c93c1-109">Основная модель размещения для Blazor работает на стороне клиента в браузере на веб-сборке.</span><span class="sxs-lookup"><span data-stu-id="c93c1-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="c93c1-110">Приложение Blazor, его зависимости и среда выполнения .NET скачиваются в браузере.</span><span class="sxs-lookup"><span data-stu-id="c93c1-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="c93c1-111">Приложение выполняется непосредственно в потоке пользовательского интерфейса браузера.</span><span class="sxs-lookup"><span data-stu-id="c93c1-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="c93c1-112">Обновления пользовательского интерфейса и обработка событий выполняются в рамках одного процесса.</span><span class="sxs-lookup"><span data-stu-id="c93c1-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="c93c1-113">Ресурсы приложения развертываются как статические файлы на веб-сервере или в службе, способной обслуживать статические содержимое клиентам.</span><span class="sxs-lookup"><span data-stu-id="c93c1-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[! Операцион. NO-LOC (Блазор)]<span data-ttu-id="c93c1-114">. сборка: [! Операцион. Приложение NO-LOC (Блазор)] выполняется в потоке пользовательского интерфейса в браузере.</span><span class="sxs-lookup"><span data-stu-id="c93c1-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="c93c1-115">Чтобы создать Blazor приложение с помощью клиентской модели размещения, используйте шаблон **приложенияBlazor сборки** ([DotNet New блазорвасм](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="c93c1-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="c93c1-116">ВыбравBlazor шаблон **приложения-сборки** , вы можете настроить приложение для использования ASP.NET Core серверной части, установив флажок **ASP.NET Core Hosted** ([DotNet New блазорвасм — Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="c93c1-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="c93c1-117">ASP.NET Core приложение обслуживает Blazor приложение клиентам.</span><span class="sxs-lookup"><span data-stu-id="c93c1-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="c93c1-118">Blazor приложение веб-сборки может взаимодействовать с сервером по сети с помощью вызовов или [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="c93c1-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="c93c1-119">Шаблоны включают `blazor.webassembly.js` сценарий, который обрабатывает:</span><span class="sxs-lookup"><span data-stu-id="c93c1-119">The templates include the `blazor.webassembly.js` script that handles:</span></span>

* <span data-ttu-id="c93c1-120">Загрузка среды выполнения .NET, приложения и зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="c93c1-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="c93c1-121">Инициализация среды выполнения для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="c93c1-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="c93c1-122">Модель размещения Blazorных сборок предоставляет несколько преимуществ.</span><span class="sxs-lookup"><span data-stu-id="c93c1-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="c93c1-123">Зависимость на стороне сервера .NET отсутствует.</span><span class="sxs-lookup"><span data-stu-id="c93c1-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="c93c1-124">Приложение полностью работает после загрузки на клиент.</span><span class="sxs-lookup"><span data-stu-id="c93c1-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="c93c1-125">Ресурсы и возможности клиента полностью используются.</span><span class="sxs-lookup"><span data-stu-id="c93c1-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="c93c1-126">Работа разгружается с сервера на клиент.</span><span class="sxs-lookup"><span data-stu-id="c93c1-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="c93c1-127">Веб-сервер ASP.NET Core не требуется для размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="c93c1-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="c93c1-128">Возможны сценарии бессерверного развертывания (например, обслуживание приложения из CDN).</span><span class="sxs-lookup"><span data-stu-id="c93c1-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="c93c1-129">Существует недостаток для Blazor размещения сборки:</span><span class="sxs-lookup"><span data-stu-id="c93c1-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="c93c1-130">Приложение ограничено возможностями браузера.</span><span class="sxs-lookup"><span data-stu-id="c93c1-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="c93c1-131">Требуется аппаратное и программное обеспечение, поддерживающее клиент (например, поддержка сборок).</span><span class="sxs-lookup"><span data-stu-id="c93c1-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="c93c1-132">Размер скачивания больше, и приложения загружаются дольше.</span><span class="sxs-lookup"><span data-stu-id="c93c1-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="c93c1-133">Поддержка среды выполнения .NET и инструментария менее зрела.</span><span class="sxs-lookup"><span data-stu-id="c93c1-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="c93c1-134">Например, существуют ограничения на поддержку и отладку [.NET Standard](/dotnet/standard/net-standard) .</span><span class="sxs-lookup"><span data-stu-id="c93c1-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a>Blazor<span data-ttu-id="c93c1-135"> Server</span><span class="sxs-lookup"><span data-stu-id="c93c1-135"> Server</span></span>

<span data-ttu-id="c93c1-136">При использовании модели размещения сервера Blazor приложение выполняется на сервере из приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c93c1-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="c93c1-137">Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="c93c1-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Браузер взаимодействует с приложением (размещенным в приложении ASP.NET Core) на сервере через [! Операцион. НЕТ-LOC (SignalR)] подключение.](hosting-models/_static/blazor-server.png)

<span data-ttu-id="c93c1-139">Чтобы создать Blazor приложение с помощью модели размещения сервера Blazor, используйте шаблон **серверного приложения ASP.NET CoreBlazor** ([DotNet New блазорсервер](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="c93c1-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="c93c1-140">Приложение ASP.NET Core размещает приложение Blazor Server и создает конечную точку SignalR, в которую подключаются клиенты.</span><span class="sxs-lookup"><span data-stu-id="c93c1-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="c93c1-141">ASP.NET Core приложение ссылается на класс `Startup` приложения для добавления:</span><span class="sxs-lookup"><span data-stu-id="c93c1-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="c93c1-142">Службы на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="c93c1-142">Server-side services.</span></span>
* <span data-ttu-id="c93c1-143">Приложение в конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="c93c1-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="c93c1-144">Сценарий `blazor.server.js`&dagger; устанавливает соединение с клиентом.</span><span class="sxs-lookup"><span data-stu-id="c93c1-144">The `blazor.server.js` script&dagger; establishes the client connection.</span></span> <span data-ttu-id="c93c1-145">Ответственность за сохранение и восстановление состояния приложения в случае необходимости (например, в случае потери сетевого подключения) несет приложение.</span><span class="sxs-lookup"><span data-stu-id="c93c1-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="c93c1-146">Модель размещения серверов Blazor предлагает несколько преимуществ.</span><span class="sxs-lookup"><span data-stu-id="c93c1-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="c93c1-147">Размер загружаемого файла значительно меньше, чем Blazor приложение сборки, и приложение загружается гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="c93c1-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="c93c1-148">Приложение обладает всеми преимуществами серверных возможностей, включая использование любых API-интерфейсов, совместимых с .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c93c1-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="c93c1-149">.NET Core на сервере используется для запуска приложения, поэтому существующие инструменты .NET, такие как отладка, работают должным образом.</span><span class="sxs-lookup"><span data-stu-id="c93c1-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="c93c1-150">Поддерживаются тонкие клиенты.</span><span class="sxs-lookup"><span data-stu-id="c93c1-150">Thin clients are supported.</span></span> <span data-ttu-id="c93c1-151">Например, Blazor серверные приложения работают с браузерами, которые не поддерживают сборку и устройства с ограниченными ресурсами.</span><span class="sxs-lookup"><span data-stu-id="c93c1-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="c93c1-152">База .NET (C# код) приложения, включая код компонента приложения, не обслуживаются для клиентов.</span><span class="sxs-lookup"><span data-stu-id="c93c1-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="c93c1-153">Существуют недостатки Blazor размещения сервера:</span><span class="sxs-lookup"><span data-stu-id="c93c1-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="c93c1-154">Обычно существует большая задержка.</span><span class="sxs-lookup"><span data-stu-id="c93c1-154">Higher latency usually exists.</span></span> <span data-ttu-id="c93c1-155">Каждое взаимодействие с пользователем включает сетевой прыжок.</span><span class="sxs-lookup"><span data-stu-id="c93c1-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="c93c1-156">Автономная поддержка отсутствует.</span><span class="sxs-lookup"><span data-stu-id="c93c1-156">There's no offline support.</span></span> <span data-ttu-id="c93c1-157">Если подключение клиента завершается сбоем, приложение перестает работать.</span><span class="sxs-lookup"><span data-stu-id="c93c1-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="c93c1-158">Масштабируемость — это сложная задача для приложений с большим количеством пользователей.</span><span class="sxs-lookup"><span data-stu-id="c93c1-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="c93c1-159">Сервер должен управлять несколькими клиентскими подключениями и обрабатывать состояние клиента.</span><span class="sxs-lookup"><span data-stu-id="c93c1-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="c93c1-160">Для обслуживания приложения требуется сервер ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c93c1-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="c93c1-161">Сценарии бессерверного развертывания невозможны (например, обслуживание приложения из CDN).</span><span class="sxs-lookup"><span data-stu-id="c93c1-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="c93c1-162">&dagger;сценарий `blazor.server.js` обслуживается из внедренного ресурса в ASP.NET Core общей платформе.</span><span class="sxs-lookup"><span data-stu-id="c93c1-162">&dagger;The `blazor.server.js` script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="c93c1-163">Сравнение с ИНТЕРФЕЙСом, отображаемым сервером</span><span class="sxs-lookup"><span data-stu-id="c93c1-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="c93c1-164">Одним из способов понимания Blazor серверных приложений является понимание того, как оно отличается от традиционных моделей для отрисовки пользовательского интерфейса в ASP.NET Core приложениях с помощью представлений Razor или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c93c1-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="c93c1-165">В обеих моделях используется язык Razor для описания содержимого HTML, но они значительно отличаются от отрисовки разметки.</span><span class="sxs-lookup"><span data-stu-id="c93c1-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="c93c1-166">При отрисовке страницы Razor или представления каждая строка кода Razor выдает HTML в виде текста.</span><span class="sxs-lookup"><span data-stu-id="c93c1-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="c93c1-167">После отрисовки сервер удаляет экземпляр страницы или представления, включая любое созданное состояние.</span><span class="sxs-lookup"><span data-stu-id="c93c1-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="c93c1-168">Когда происходит другой запрос к странице, например при сбое проверки сервера и отображении сводки проверки:</span><span class="sxs-lookup"><span data-stu-id="c93c1-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="c93c1-169">Вся страница снова преобразуется в HTML-текст.</span><span class="sxs-lookup"><span data-stu-id="c93c1-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="c93c1-170">Страница отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="c93c1-170">The page is sent to the client.</span></span>

<span data-ttu-id="c93c1-171">Blazor приложение состоит из многократно используемых элементов пользовательского интерфейса, называемых *компонентами*.</span><span class="sxs-lookup"><span data-stu-id="c93c1-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="c93c1-172">Компонент содержит C# код, разметку и другие компоненты.</span><span class="sxs-lookup"><span data-stu-id="c93c1-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="c93c1-173">При подготовке компонента к просмотру Blazor создает граф включенных компонентов, аналогичный HTML-или модель DOM XML (DOM).</span><span class="sxs-lookup"><span data-stu-id="c93c1-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="c93c1-174">Эта диаграмма включает в себя состояние компонента, содержащееся в свойствах и полях.</span><span class="sxs-lookup"><span data-stu-id="c93c1-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="c93c1-175"> оценивает граф компонентов для создания двоичного представления разметки.</span><span class="sxs-lookup"><span data-stu-id="c93c1-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="c93c1-176">Двоичный формат может быть следующим:</span><span class="sxs-lookup"><span data-stu-id="c93c1-176">The binary format can be:</span></span>

* <span data-ttu-id="c93c1-177">Преобразуется в текст HTML (во время предварительной отрисовки).</span><span class="sxs-lookup"><span data-stu-id="c93c1-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="c93c1-178">Используется для эффективного обновления разметки во время обычной отрисовки.</span><span class="sxs-lookup"><span data-stu-id="c93c1-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="c93c1-179">Обновление пользовательского интерфейса в Blazor активируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c93c1-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="c93c1-180">Взаимодействие с пользователем, например выбор кнопки.</span><span class="sxs-lookup"><span data-stu-id="c93c1-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="c93c1-181">Триггеры приложений, например таймер.</span><span class="sxs-lookup"><span data-stu-id="c93c1-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="c93c1-182">Граф перерисовывается, и вычисляется *различие в пользовательском* интерфейсе (разница).</span><span class="sxs-lookup"><span data-stu-id="c93c1-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="c93c1-183">Это различие является наименьшим набором изменений DOM, необходимых для обновления пользовательского интерфейса на клиенте.</span><span class="sxs-lookup"><span data-stu-id="c93c1-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="c93c1-184">Diff отправляется клиенту в двоичном формате и применяется в браузере.</span><span class="sxs-lookup"><span data-stu-id="c93c1-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="c93c1-185">Компонент уничтожается после того, как пользователь выходит из него с клиента.</span><span class="sxs-lookup"><span data-stu-id="c93c1-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="c93c1-186">Несмотря на то, что пользователь взаимодействует с компонентом, состояние компонента (службы, ресурсы) должно храниться в памяти сервера.</span><span class="sxs-lookup"><span data-stu-id="c93c1-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="c93c1-187">Так как состояние множества компонентов может поддерживаться сервером одновременно, нехватка памяти является проблемой, которую необходимо решить.</span><span class="sxs-lookup"><span data-stu-id="c93c1-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="c93c1-188">Инструкции по созданию приложения Blazor Server для обеспечения оптимального использования памяти сервера см. в разделе <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="c93c1-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="c93c1-189">Интеграция компонентов Razor в приложения Razor Pages и MVC</span><span class="sxs-lookup"><span data-stu-id="c93c1-189">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="c93c1-190">Использование компонентов в страницах и представлениях</span><span class="sxs-lookup"><span data-stu-id="c93c1-190">Use components in pages and views</span></span>

<span data-ttu-id="c93c1-191">Существующее приложение Razor Pages или MVC может интегрировать компоненты Razor в страницы и представления:</span><span class="sxs-lookup"><span data-stu-id="c93c1-191">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="c93c1-192">В файле макета приложения ( *_layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="c93c1-192">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="c93c1-193">Добавьте следующий тег `<base>` в элемент `<head>`:</span><span class="sxs-lookup"><span data-stu-id="c93c1-193">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="c93c1-194">Значение `href` ( *базовый путь приложения*) в предыдущем примере предполагает, что приложение находится по КОРНЕВОМУ URL-пути (`/`).</span><span class="sxs-lookup"><span data-stu-id="c93c1-194">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="c93c1-195">Если приложение является вложенным приложением, следуйте указаниям в разделе " *базовый путь приложения* " статьи <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="c93c1-195">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="c93c1-196">Файл *_layout. cshtml* находится в папке *pages/shared* в приложении Razor Pages или в папке *views/Shared* приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="c93c1-196">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="c93c1-197">Добавьте тег `<script>` для скрипта *блазор. Server. js* внутри закрывающего тега `</body>`:</span><span class="sxs-lookup"><span data-stu-id="c93c1-197">Add a `<script>` tag for the *blazor.server.js* script inside of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="c93c1-198">Платформа добавляет в приложение скрипт *блазор. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="c93c1-198">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="c93c1-199">Нет необходимости вручную добавлять скрипт в приложение.</span><span class="sxs-lookup"><span data-stu-id="c93c1-199">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="c93c1-200">Добавьте файл *_Imports. Razor* в корневую папку проекта со следующим содержимым (измените Последнее пространство имен, `MyAppNamespace`, в пространство имен приложения):</span><span class="sxs-lookup"><span data-stu-id="c93c1-200">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="c93c1-201">В `Startup.ConfigureServices`добавьте службу сервера Blazor.</span><span class="sxs-lookup"><span data-stu-id="c93c1-201">In `Startup.ConfigureServices`, add the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="c93c1-202">В `Startup.Configure`Добавьте конечную точку концентратора Blazor для `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="c93c1-202">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="c93c1-203">Интегрируйте компоненты в любую страницу или представление.</span><span class="sxs-lookup"><span data-stu-id="c93c1-203">Integrate components into any page or view.</span></span> <span data-ttu-id="c93c1-204">Дополнительные сведения см. в разделе *Интеграция компонентов в Razor Pages и приложения MVC* статьи <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="c93c1-204">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="c93c1-205">Использование маршрутизируемых компонентов в приложении Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c93c1-205">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="c93c1-206">Для поддержки маршрутизируемых компонентов Razor в Razor Pages приложениях:</span><span class="sxs-lookup"><span data-stu-id="c93c1-206">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="c93c1-207">Следуйте указаниям в разделе [Использование компонентов в страницах и представлениях](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="c93c1-207">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="c93c1-208">Добавьте файл *app. Razor* в корневую папку проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="c93c1-208">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="c93c1-209">Добавьте файл *_Host. cshtml* в папку *pages* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="c93c1-209">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="c93c1-210">Компоненты используют общий файл *_layout. cshtml* для их макета.</span><span class="sxs-lookup"><span data-stu-id="c93c1-210">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="c93c1-211">Добавьте маршрут с низким приоритетом для страницы *_Host. cshtml* в конфигурацию конечной точки в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c93c1-211">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="c93c1-212">Добавьте маршрутизируемые компоненты в приложение.</span><span class="sxs-lookup"><span data-stu-id="c93c1-212">Add routable components to the app.</span></span> <span data-ttu-id="c93c1-213">Например:</span><span class="sxs-lookup"><span data-stu-id="c93c1-213">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="c93c1-214">При использовании пользовательской папки для хранения компонентов приложения добавьте пространство имен, представляющее папку, в файл *pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="c93c1-214">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="c93c1-215">Для получения дополнительной информации см. <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="c93c1-215">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="c93c1-216">Использование маршрутизируемых компонентов в приложении MVC</span><span class="sxs-lookup"><span data-stu-id="c93c1-216">Use routable components in an MVC app</span></span>

<span data-ttu-id="c93c1-217">Для поддержки маршрутизации компонентов Razor в приложениях MVC:</span><span class="sxs-lookup"><span data-stu-id="c93c1-217">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="c93c1-218">Следуйте указаниям в разделе [Использование компонентов в страницах и представлениях](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="c93c1-218">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="c93c1-219">Добавьте файл *app. Razor* в корневую папку проекта со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="c93c1-219">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="c93c1-220">Добавьте файл *_Host. cshtml* в папку *Views/Home* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="c93c1-220">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="c93c1-221">Компоненты используют общий файл *_layout. cshtml* для их макета.</span><span class="sxs-lookup"><span data-stu-id="c93c1-221">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="c93c1-222">Добавьте действие в контроллер Home:</span><span class="sxs-lookup"><span data-stu-id="c93c1-222">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="c93c1-223">Добавьте маршрут с низким приоритетом для действия контроллера, которое возвращает представление *_Host. cshtml* в конфигурацию конечной точки в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c93c1-223">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="c93c1-224">Создайте папку *pages* и добавьте в приложение маршрутизируемые компоненты.</span><span class="sxs-lookup"><span data-stu-id="c93c1-224">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="c93c1-225">Например:</span><span class="sxs-lookup"><span data-stu-id="c93c1-225">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="c93c1-226">При использовании пользовательской папки для хранения компонентов приложения добавьте пространство имен, представляющее папку, в файл *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="c93c1-226">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="c93c1-227">Для получения дополнительной информации см. <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="c93c1-227">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="circuits"></a><span data-ttu-id="c93c1-228">Защит</span><span class="sxs-lookup"><span data-stu-id="c93c1-228">Circuits</span></span>

<span data-ttu-id="c93c1-229">Серверное приложение Blazor построено на основе [ASP.NET Core SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="c93c1-229">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="c93c1-230">Каждый клиент взаимодействует с сервером через одно или несколько SignalR соединений, называемых *каналом*.</span><span class="sxs-lookup"><span data-stu-id="c93c1-230">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="c93c1-231">Цепь — это Blazorабстракции для SignalR подключений, которые могут допускать временные перерывы в сети.</span><span class="sxs-lookup"><span data-stu-id="c93c1-231">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="c93c1-232">Когда клиент Blazor видит, что SignalRное подключение отключено, оно пытается повторно подключиться к серверу с помощью нового соединения SignalR.</span><span class="sxs-lookup"><span data-stu-id="c93c1-232">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="c93c1-233">Каждый экран браузера (вкладка браузера или IFRAME), подключенный к Blazor серверному приложению, использует SignalRное соединение.</span><span class="sxs-lookup"><span data-stu-id="c93c1-233">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="c93c1-234">Это еще одно важное различие по сравнению с обычными серверно-визуализированными приложениями.</span><span class="sxs-lookup"><span data-stu-id="c93c1-234">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="c93c1-235">В приложении, готовом для просмотра, открытие одного и того же приложения на нескольких экранах браузера обычно не приводит к дополнительным требованиям к ресурсам на сервере.</span><span class="sxs-lookup"><span data-stu-id="c93c1-235">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="c93c1-236">В приложении Blazor Server каждый экран браузера требует отдельного канала и отдельных экземпляров состояния компонента для управления сервером.</span><span class="sxs-lookup"><span data-stu-id="c93c1-236">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="c93c1-237"> рассматривает закрытие вкладки браузера или переход по внешнему URL-адресу *корректное* завершение.</span><span class="sxs-lookup"><span data-stu-id="c93c1-237"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="c93c1-238">В случае корректного завершения работы канал и связанные ресурсы немедленно освобождаются.</span><span class="sxs-lookup"><span data-stu-id="c93c1-238">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="c93c1-239">Клиент может также отключиться от некорректного, например, из-за прерывания работы сети.</span><span class="sxs-lookup"><span data-stu-id="c93c1-239">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> Blazor<span data-ttu-id="c93c1-240"> Server хранит отключенные цепи в течение настраиваемого интервала, чтобы разрешить клиенту повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="c93c1-240"> Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="c93c1-241">Дополнительные сведения см. в разделе повторное [Подключение к тому же серверу](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="c93c1-241">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="c93c1-242">Задержка пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="c93c1-242">UI Latency</span></span>

<span data-ttu-id="c93c1-243">Задержка пользовательского интерфейса — это время, которое занимает от инициированного действия до момента обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="c93c1-243">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="c93c1-244">Меньшее значение задержки пользовательского интерфейса является обязательным, чтобы приложение было легко реагировать на пользователя.</span><span class="sxs-lookup"><span data-stu-id="c93c1-244">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="c93c1-245">В приложении Blazor Server каждое действие отправляется на сервер, обрабатывается, и разница пользовательского интерфейса отсылается обратно.</span><span class="sxs-lookup"><span data-stu-id="c93c1-245">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="c93c1-246">Следовательно, задержка пользовательского интерфейса — это сумма задержки в сети и задержки сервера при обработке действия.</span><span class="sxs-lookup"><span data-stu-id="c93c1-246">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="c93c1-247">Для бизнес-приложения, которое ограничено частной корпоративной сетью, воздействие на восприятие пользователей задержки из-за задержки в сети обычно незаметно.</span><span class="sxs-lookup"><span data-stu-id="c93c1-247">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="c93c1-248">Для приложения, развернутого через Интернет, задержка может стать заметным для пользователей, особенно в том случае, если пользователи широко распределены географически.</span><span class="sxs-lookup"><span data-stu-id="c93c1-248">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="c93c1-249">Использование памяти может также повлиять на задержку приложения.</span><span class="sxs-lookup"><span data-stu-id="c93c1-249">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="c93c1-250">Увеличение использования памяти приводит к частым сборкам мусора или выделению памяти на диск, что снижает производительность приложений и, следовательно, увеличивает задержку пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="c93c1-250">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="c93c1-251">Для получения дополнительной информации см. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="c93c1-251">For more information, see <xref:security/blazor/server>.</span></span>

Blazor<span data-ttu-id="c93c1-252"> серверные приложения должны быть оптимизированы для минимизации задержки пользовательского интерфейса за счет уменьшения задержки сети и использования памяти.</span><span class="sxs-lookup"><span data-stu-id="c93c1-252"> Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="c93c1-253">Способ измерения задержки в сети см. в разделе <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="c93c1-253">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="c93c1-254">Дополнительные сведения о SignalR и Blazorсм. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="c93c1-254">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="c93c1-255">Соединение с сервером</span><span class="sxs-lookup"><span data-stu-id="c93c1-255">Connection to the server</span></span>

<span data-ttu-id="c93c1-256">для Blazor серверных приложений требуется активное подключение SignalR к серверу.</span><span class="sxs-lookup"><span data-stu-id="c93c1-256">Blazor Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="c93c1-257">Если соединение потеряно, приложение попытается повторно подключиться к серверу.</span><span class="sxs-lookup"><span data-stu-id="c93c1-257">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="c93c1-258">Пока состояние клиента по-прежнему находится в памяти, сеанс клиента возобновляется без потери состояния.</span><span class="sxs-lookup"><span data-stu-id="c93c1-258">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="c93c1-259">Повторное подключение к тому же серверу</span><span class="sxs-lookup"><span data-stu-id="c93c1-259">Reconnection to the same server</span></span>

<span data-ttu-id="c93c1-260">Приложение Blazor Server предварительно отображается в ответ на первый клиентский запрос, который настраивает состояние пользовательского интерфейса на сервере.</span><span class="sxs-lookup"><span data-stu-id="c93c1-260">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="c93c1-261">Когда клиент пытается создать SignalR подключение, клиент должен повторно подключиться к тому же серверу.</span><span class="sxs-lookup"><span data-stu-id="c93c1-261">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> Blazor<span data-ttu-id="c93c1-262"> серверных приложений, использующих более одного внутреннего сервера, должны реализовывать *закрепленные сеансы* для SignalRных соединений.</span><span class="sxs-lookup"><span data-stu-id="c93c1-262"> Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="c93c1-263">Мы рекомендуем использовать для серверных приложений Blazor[службу Azure SignalR](/azure/azure-signalr).</span><span class="sxs-lookup"><span data-stu-id="c93c1-263">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="c93c1-264">Она позволяет вертикально масштабировать серверные приложения Blazor для одновременного использования большого числа подключений SignalR.</span><span class="sxs-lookup"><span data-stu-id="c93c1-264">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="c93c1-265">Прикрепленные сеансы включены для службы SignalR Azure, задав для параметра `ServerStickyMode` или значения конфигурации службы значение `Required`.</span><span class="sxs-lookup"><span data-stu-id="c93c1-265">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="c93c1-266">Для получения дополнительной информации см. <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c93c1-266">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="c93c1-267">При использовании служб IIS прикрепленные сеансы включаются с помощью маршрутизации запросов приложений.</span><span class="sxs-lookup"><span data-stu-id="c93c1-267">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="c93c1-268">Дополнительные сведения см. в статье [Балансировка нагрузки HTTP с помощью маршрутизации запросов приложений](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="c93c1-268">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="c93c1-269">Отражение состояния соединения в пользовательском интерфейсе</span><span class="sxs-lookup"><span data-stu-id="c93c1-269">Reflect the connection state in the UI</span></span>

<span data-ttu-id="c93c1-270">Когда клиент обнаруживает, что соединение потеряно, пользователь по умолчанию отображает пользовательский интерфейс, когда клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="c93c1-270">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="c93c1-271">Если происходит сбой повторного подключения, пользователь предоставляет возможность повторить попытку.</span><span class="sxs-lookup"><span data-stu-id="c93c1-271">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="c93c1-272">Чтобы настроить пользовательский интерфейс, определите элемент с `id` `components-reconnect-modal` в `<body>` страницы Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="c93c1-272">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="c93c1-273">В следующей таблице описаны классы CSS, применяемые к элементу `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="c93c1-273">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="c93c1-274">Класс CSS</span><span class="sxs-lookup"><span data-stu-id="c93c1-274">CSS class</span></span>                       | <span data-ttu-id="c93c1-275">Указывает,&hellip;</span><span class="sxs-lookup"><span data-stu-id="c93c1-275">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="c93c1-276">Утерянное подключение.</span><span class="sxs-lookup"><span data-stu-id="c93c1-276">A lost connection.</span></span> <span data-ttu-id="c93c1-277">Клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="c93c1-277">The client is attempting to reconnect.</span></span> <span data-ttu-id="c93c1-278">Отображение модального окна.</span><span class="sxs-lookup"><span data-stu-id="c93c1-278">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="c93c1-279">Активное подключение будет восстановлено на сервере.</span><span class="sxs-lookup"><span data-stu-id="c93c1-279">An active connection is re-established to the server.</span></span> <span data-ttu-id="c93c1-280">Скрыть модальное окно.</span><span class="sxs-lookup"><span data-stu-id="c93c1-280">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="c93c1-281">Сбой повторного подключения, возможно, из-за сбоя сети.</span><span class="sxs-lookup"><span data-stu-id="c93c1-281">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="c93c1-282">Чтобы повторить попытку подключения, вызовите `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="c93c1-282">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="c93c1-283">Повторное подключение отклонено.</span><span class="sxs-lookup"><span data-stu-id="c93c1-283">Reconnection rejected.</span></span> <span data-ttu-id="c93c1-284">Сервер был достигнут, но отклонил подключение, и состояние пользователя на сервере потеряно.</span><span class="sxs-lookup"><span data-stu-id="c93c1-284">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="c93c1-285">Чтобы перезагрузить приложение, вызовите `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="c93c1-285">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="c93c1-286">Это состояние соединения может появиться в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="c93c1-286">This connection state may result when:</span></span><ul><li><span data-ttu-id="c93c1-287">Происходит сбой в цепи на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="c93c1-287">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="c93c1-288">Клиент отключается достаточно долго, чтобы сервер отключил состояние пользователя.</span><span class="sxs-lookup"><span data-stu-id="c93c1-288">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="c93c1-289">Удаляются экземпляры компонентов, с которыми взаимодействует пользователь.</span><span class="sxs-lookup"><span data-stu-id="c93c1-289">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="c93c1-290">Сервер перезагружается, или рабочий процесс приложения перезапускается.</span><span class="sxs-lookup"><span data-stu-id="c93c1-290">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="c93c1-291">Повторное подключение с отслеживанием состояния после предварительной подготовки к просмотру</span><span class="sxs-lookup"><span data-stu-id="c93c1-291">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="c93c1-292">Серверные приложения Blazor настроены по умолчанию для выprerender пользовательский интерфейс на сервере, прежде чем будет установлено клиентское соединение с сервером.</span><span class="sxs-lookup"><span data-stu-id="c93c1-292">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="c93c1-293">Это настраивается на странице Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="c93c1-293">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="c93c1-294">`RenderMode` настраивает, является ли компонент:</span><span class="sxs-lookup"><span data-stu-id="c93c1-294">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="c93c1-295">Предварительно отображается на странице.</span><span class="sxs-lookup"><span data-stu-id="c93c1-295">Is prerendered into the page.</span></span>
* <span data-ttu-id="c93c1-296">Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки Blazor приложения из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="c93c1-296">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="c93c1-297">Описание</span><span class="sxs-lookup"><span data-stu-id="c93c1-297">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="c93c1-298">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="c93c1-298">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="c93c1-299">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="c93c1-299">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="c93c1-300">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="c93c1-300">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="c93c1-301">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="c93c1-301">Output from the component isn't included.</span></span> <span data-ttu-id="c93c1-302">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="c93c1-302">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="c93c1-303">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="c93c1-303">Renders the component into static HTML.</span></span> |

<span data-ttu-id="c93c1-304">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="c93c1-304">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="c93c1-305">Если `RenderMode` `ServerPrerendered`, компонент изначально отрисовывается статически как часть страницы.</span><span class="sxs-lookup"><span data-stu-id="c93c1-305">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="c93c1-306">После того, как браузер установит соединение с сервером, компонент *снова*готовится к просмотру, и теперь компонент является интерактивным.</span><span class="sxs-lookup"><span data-stu-id="c93c1-306">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="c93c1-307">Если имеется метод жизненно инициализированного метода жизненный цикл [{Async}](xref:blazor/lifecycle#component-initialization-methods) для инициализации компонента, метод выполняется *дважды*:</span><span class="sxs-lookup"><span data-stu-id="c93c1-307">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="c93c1-308">Когда компонент предварительно отображается статически.</span><span class="sxs-lookup"><span data-stu-id="c93c1-308">When the component is prerendered statically.</span></span>
* <span data-ttu-id="c93c1-309">После установки соединения с сервером.</span><span class="sxs-lookup"><span data-stu-id="c93c1-309">After the server connection has been established.</span></span>

<span data-ttu-id="c93c1-310">Это может привести к заметному изменению данных, отображаемых в пользовательском интерфейсе, когда компонент в итоге готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="c93c1-310">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="c93c1-311">Чтобы избежать сценария двойной визуализации в приложении Blazor Server, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="c93c1-311">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="c93c1-312">Передайте идентификатор, который можно использовать для кэширования состояния во время подготовки к просмотру, и для получения состояния после перезапуска приложения.</span><span class="sxs-lookup"><span data-stu-id="c93c1-312">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="c93c1-313">Используйте идентификатор во время подготовки к просмотру для сохранения состояния компонента.</span><span class="sxs-lookup"><span data-stu-id="c93c1-313">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="c93c1-314">Используйте идентификатор после предварительной подготовки для получения кэшированного состояния.</span><span class="sxs-lookup"><span data-stu-id="c93c1-314">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="c93c1-315">В следующем коде показано обновленное `WeatherForecastService` в серверном приложении Blazor на основе шаблонов, которое позволяет избежать двойной отрисовки:</span><span class="sxs-lookup"><span data-stu-id="c93c1-315">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="c93c1-316">Отображение интерактивных компонентов с отслеживанием состояния из страниц и представлений Razor</span><span class="sxs-lookup"><span data-stu-id="c93c1-316">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="c93c1-317">Интерактивные компоненты с отслеживанием состояния могут быть добавлены на страницу Razor или в представление.</span><span class="sxs-lookup"><span data-stu-id="c93c1-317">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="c93c1-318">При визуализации страницы или представления:</span><span class="sxs-lookup"><span data-stu-id="c93c1-318">When the page or view renders:</span></span>

* <span data-ttu-id="c93c1-319">Компонент предварительно отображается страницей или представлением.</span><span class="sxs-lookup"><span data-stu-id="c93c1-319">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="c93c1-320">Исходное состояние компонента, используемое для предварительной визуализации, потеряно.</span><span class="sxs-lookup"><span data-stu-id="c93c1-320">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="c93c1-321">Новое состояние компонента создается при установке подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="c93c1-321">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="c93c1-322">Следующая страница Razor визуализирует компонент `Counter`:</span><span class="sxs-lookup"><span data-stu-id="c93c1-322">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="c93c1-323">Прорисовка неинтерактивных компонентов на страницах и представлениях Razor</span><span class="sxs-lookup"><span data-stu-id="c93c1-323">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="c93c1-324">На следующей странице Razor компонент `Counter` статически подготавливается к просмотру с начальным значением, указанным с помощью формы:</span><span class="sxs-lookup"><span data-stu-id="c93c1-324">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="c93c1-325">Поскольку `MyComponent` является статически отображаемым, компонент не может быть интерактивным.</span><span class="sxs-lookup"><span data-stu-id="c93c1-325">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="c93c1-326">Обнаруживать время подготовки приложения к просмотру</span><span class="sxs-lookup"><span data-stu-id="c93c1-326">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="c93c1-327">Настройка клиента SignalR для приложений Blazor Server</span><span class="sxs-lookup"><span data-stu-id="c93c1-327">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="c93c1-328">Иногда необходимо настроить клиент SignalR, используемый приложениями Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="c93c1-328">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="c93c1-329">Например, может потребоваться настроить ведение журнала на SignalRном клиенте для диагностики проблемы с подключением.</span><span class="sxs-lookup"><span data-stu-id="c93c1-329">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="c93c1-330">Настройка клиента SignalR в файле *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="c93c1-330">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="c93c1-331">Добавьте атрибут `autostart="false"` в тег `<script>` для скрипта `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="c93c1-331">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="c93c1-332">Вызовите `Blazor.start` и передайте объект конфигурации, указывающий построитель SignalR.</span><span class="sxs-lookup"><span data-stu-id="c93c1-332">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="c93c1-333">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c93c1-333">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
