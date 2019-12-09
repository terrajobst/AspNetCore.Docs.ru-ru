---
title: Модели размещения Blazor ASP.NET Core
author: guardrex
description: Общие сведения о моделях размещения Blazor и Blazor Server.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 7676d16bddf146ea38619ed35c5e32c5bce731de
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943775"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="f3ec3-103">Модели размещения Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3ec3-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="f3ec3-104">По [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f3ec3-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="f3ec3-105"> — это веб-платформа, предназначенная для запуска на стороне клиента в браузере в среде выполнения .NET на основе [сборки](https://webassembly.org/)(Blazor веб- *сборки*) или на стороне сервера в ASP.NET Core ( *Blazor Server*).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="f3ec3-106">Независимо от модели размещения, модели приложения и компонента *одинаковы*.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="f3ec3-107">Сведения о создании проекта для моделей размещения, описанных в этой статье, см. в разделе <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="f3ec3-108"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="f3ec3-108"> WebAssembly</span></span>

<span data-ttu-id="f3ec3-109">Основная модель размещения для Blazor работает на стороне клиента в браузере на веб-сборке.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="f3ec3-110">Приложение Blazor, его зависимости и среда выполнения .NET скачиваются в браузере.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="f3ec3-111">Приложение выполняется непосредственно в потоке пользовательского интерфейса браузера.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="f3ec3-112">Обновления пользовательского интерфейса и обработка событий выполняются в рамках одного процесса.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="f3ec3-113">Ресурсы приложения развертываются как статические файлы на веб-сервере или в службе, способной обслуживать статические содержимое клиентам.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[! Операцион. NO-LOC (Блазор)]<span data-ttu-id="f3ec3-114">. сборка: [! Операцион. Приложение NO-LOC (Блазор)] выполняется в потоке пользовательского интерфейса в браузере.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="f3ec3-115">Чтобы создать Blazor приложение с помощью клиентской модели размещения, используйте шаблон **приложенияBlazor сборки** ([DotNet New блазорвасм](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="f3ec3-116">ВыбравBlazor шаблон **приложения-сборки** , вы можете настроить приложение для использования ASP.NET Core серверной части, установив флажок **ASP.NET Core Hosted** ([DotNet New блазорвасм — Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="f3ec3-117">ASP.NET Core приложение обслуживает Blazor приложение клиентам.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="f3ec3-118">Blazor приложение веб-сборки может взаимодействовать с сервером по сети с помощью вызовов или [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="f3ec3-119">Шаблоны включают скрипт *блазор... js* , который обрабатывает:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="f3ec3-120">Загрузка среды выполнения .NET, приложения и зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="f3ec3-121">Инициализация среды выполнения для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="f3ec3-122">Модель размещения Blazorных сборок предоставляет несколько преимуществ.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="f3ec3-123">Зависимость на стороне сервера .NET отсутствует.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="f3ec3-124">Приложение полностью работает после загрузки на клиент.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="f3ec3-125">Ресурсы и возможности клиента полностью используются.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="f3ec3-126">Работа разгружается с сервера на клиент.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="f3ec3-127">Веб-сервер ASP.NET Core не требуется для размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="f3ec3-128">Возможны сценарии бессерверного развертывания (например, обслуживание приложения из CDN).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="f3ec3-129">Существует недостаток для Blazor размещения сборки:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="f3ec3-130">Приложение ограничено возможностями браузера.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="f3ec3-131">Требуется аппаратное и программное обеспечение, поддерживающее клиент (например, поддержка сборок).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="f3ec3-132">Размер скачивания больше, и приложения загружаются дольше.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="f3ec3-133">Поддержка среды выполнения .NET и инструментария менее зрела.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="f3ec3-134">Например, существуют ограничения на поддержку и отладку [.NET Standard](/dotnet/standard/net-standard) .</span><span class="sxs-lookup"><span data-stu-id="f3ec3-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a><span data-ttu-id="f3ec3-135">Сервер Blazor</span><span class="sxs-lookup"><span data-stu-id="f3ec3-135">Blazor Server</span></span>

<span data-ttu-id="f3ec3-136">При использовании модели размещения сервера Blazor приложение выполняется на сервере из приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="f3ec3-137">Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Браузер взаимодействует с приложением (размещенным в приложении ASP.NET Core) на сервере через [! Операцион. НЕТ-LOC (SignalR)] подключение.](hosting-models/_static/blazor-server.png)

<span data-ttu-id="f3ec3-139">Чтобы создать Blazor приложение с помощью модели размещения сервера Blazor, используйте шаблон **серверного приложения ASP.NET CoreBlazor** ([DotNet New блазорсервер](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="f3ec3-140">Приложение ASP.NET Core размещает приложение Blazor Server и создает конечную точку SignalR, в которую подключаются клиенты.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="f3ec3-141">ASP.NET Core приложение ссылается на класс `Startup` приложения для добавления:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="f3ec3-142">Службы на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-142">Server-side services.</span></span>
* <span data-ttu-id="f3ec3-143">Приложение в конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="f3ec3-144">Скрипт *блазор. Server. js*&dagger; устанавливает клиентское соединение.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="f3ec3-145">Ответственность за сохранение и восстановление состояния приложения в случае необходимости (например, в случае потери сетевого подключения) несет приложение.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="f3ec3-146">Модель размещения серверов Blazor предлагает несколько преимуществ.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="f3ec3-147">Размер загружаемого файла значительно меньше, чем Blazor приложение сборки, и приложение загружается гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="f3ec3-148">Приложение обладает всеми преимуществами серверных возможностей, включая использование любых API-интерфейсов, совместимых с .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="f3ec3-149">.NET Core на сервере используется для запуска приложения, поэтому существующие инструменты .NET, такие как отладка, работают должным образом.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="f3ec3-150">Поддерживаются тонкие клиенты.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-150">Thin clients are supported.</span></span> <span data-ttu-id="f3ec3-151">Например, Blazor серверные приложения работают с браузерами, которые не поддерживают сборку и устройства с ограниченными ресурсами.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="f3ec3-152">База .NET (C# код) приложения, включая код компонента приложения, не обслуживаются для клиентов.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="f3ec3-153">Существуют недостатки Blazor размещения сервера:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="f3ec3-154">Обычно существует большая задержка.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-154">Higher latency usually exists.</span></span> <span data-ttu-id="f3ec3-155">Каждое взаимодействие с пользователем включает сетевой прыжок.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="f3ec3-156">Автономная поддержка отсутствует.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-156">There's no offline support.</span></span> <span data-ttu-id="f3ec3-157">Если подключение клиента завершается сбоем, приложение перестает работать.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="f3ec3-158">Масштабируемость — это сложная задача для приложений с большим количеством пользователей.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="f3ec3-159">Сервер должен управлять несколькими клиентскими подключениями и обрабатывать состояние клиента.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="f3ec3-160">Для обслуживания приложения требуется сервер ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="f3ec3-161">Сценарии бессерверного развертывания невозможны (например, обслуживание приложения из CDN).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="f3ec3-162">&dagger;сценарий *блазор. Server. js* обслуживается из внедренного ресурса в ASP.NET Core общей платформе.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="f3ec3-163">Сравнение с ИНТЕРФЕЙСом, отображаемым сервером</span><span class="sxs-lookup"><span data-stu-id="f3ec3-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="f3ec3-164">Одним из способов понимания Blazor серверных приложений является понимание того, как оно отличается от традиционных моделей для отрисовки пользовательского интерфейса в ASP.NET Core приложениях с помощью представлений Razor или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="f3ec3-165">В обеих моделях используется язык Razor для описания содержимого HTML, но они значительно отличаются от отрисовки разметки.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="f3ec3-166">При отрисовке страницы Razor или представления каждая строка кода Razor выдает HTML в виде текста.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="f3ec3-167">После отрисовки сервер удаляет экземпляр страницы или представления, включая любое созданное состояние.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="f3ec3-168">Когда происходит другой запрос к странице, например при сбое проверки сервера и отображении сводки проверки:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="f3ec3-169">Вся страница снова преобразуется в HTML-текст.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="f3ec3-170">Страница отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-170">The page is sent to the client.</span></span>

<span data-ttu-id="f3ec3-171">Blazor приложение состоит из многократно используемых элементов пользовательского интерфейса, называемых *компонентами*.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="f3ec3-172">Компонент содержит C# код, разметку и другие компоненты.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="f3ec3-173">При подготовке компонента к просмотру Blazor создает граф включенных компонентов, аналогичный HTML-или модель DOM XML (DOM).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="f3ec3-174">Эта диаграмма включает в себя состояние компонента, содержащееся в свойствах и полях.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="f3ec3-175"> оценивает граф компонентов для создания двоичного представления разметки.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="f3ec3-176">Двоичный формат может быть следующим:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-176">The binary format can be:</span></span>

* <span data-ttu-id="f3ec3-177">Преобразуется в текст HTML (во время предварительной отрисовки).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="f3ec3-178">Используется для эффективного обновления разметки во время обычной отрисовки.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="f3ec3-179">Обновление пользовательского интерфейса в Blazor активируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="f3ec3-180">Взаимодействие с пользователем, например выбор кнопки.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="f3ec3-181">Триггеры приложений, например таймер.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="f3ec3-182">Граф перерисовывается, и вычисляется *различие в пользовательском* интерфейсе (разница).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="f3ec3-183">Это различие является наименьшим набором изменений DOM, необходимых для обновления пользовательского интерфейса на клиенте.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="f3ec3-184">Diff отправляется клиенту в двоичном формате и применяется в браузере.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="f3ec3-185">Компонент уничтожается после того, как пользователь выходит из него с клиента.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="f3ec3-186">Несмотря на то, что пользователь взаимодействует с компонентом, состояние компонента (службы, ресурсы) должно храниться в памяти сервера.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="f3ec3-187">Так как состояние множества компонентов может поддерживаться сервером одновременно, нехватка памяти является проблемой, которую необходимо решить.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="f3ec3-188">Инструкции по созданию приложения Blazor Server для обеспечения оптимального использования памяти сервера см. в разделе <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="circuits"></a><span data-ttu-id="f3ec3-189">Защит</span><span class="sxs-lookup"><span data-stu-id="f3ec3-189">Circuits</span></span>

<span data-ttu-id="f3ec3-190">Серверное приложение Blazor построено на основе [ASP.NET Core SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-190">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="f3ec3-191">Каждый клиент взаимодействует с сервером через одно или несколько SignalR соединений, называемых *каналом*.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-191">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="f3ec3-192">Цепь — это Blazorабстракции для SignalR подключений, которые могут допускать временные перерывы в сети.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-192">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="f3ec3-193">Когда клиент Blazor видит, что SignalRное подключение отключено, оно пытается повторно подключиться к серверу с помощью нового соединения SignalR.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-193">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="f3ec3-194">Каждый экран браузера (вкладка браузера или IFRAME), подключенный к Blazor серверному приложению, использует SignalRное соединение.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-194">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="f3ec3-195">Это еще одно важное различие по сравнению с обычными серверно-визуализированными приложениями.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-195">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="f3ec3-196">В приложении, готовом для просмотра, открытие одного и того же приложения на нескольких экранах браузера обычно не приводит к дополнительным требованиям к ресурсам на сервере.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-196">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="f3ec3-197">В приложении Blazor Server каждый экран браузера требует отдельного канала и отдельных экземпляров состояния компонента для управления сервером.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-197">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="f3ec3-198"> рассматривает закрытие вкладки браузера или переход по внешнему URL-адресу *корректное* завершение.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-198"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="f3ec3-199">В случае корректного завершения работы канал и связанные ресурсы немедленно освобождаются.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-199">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="f3ec3-200">Клиент может также отключиться от некорректного, например, из-за прерывания работы сети.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-200">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> Blazor<span data-ttu-id="f3ec3-201"> Server хранит отключенные цепи в течение настраиваемого интервала, чтобы разрешить клиенту повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-201"> Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="f3ec3-202">Дополнительные сведения см. в разделе повторное [Подключение к тому же серверу](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="f3ec3-202">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="f3ec3-203">Задержка пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="f3ec3-203">UI Latency</span></span>

<span data-ttu-id="f3ec3-204">Задержка пользовательского интерфейса — это время, которое занимает от инициированного действия до момента обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-204">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="f3ec3-205">Меньшее значение задержки пользовательского интерфейса является обязательным, чтобы приложение было легко реагировать на пользователя.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-205">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="f3ec3-206">В приложении Blazor Server каждое действие отправляется на сервер, обрабатывается, и разница пользовательского интерфейса отсылается обратно.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-206">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="f3ec3-207">Следовательно, задержка пользовательского интерфейса — это сумма задержки в сети и задержки сервера при обработке действия.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-207">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="f3ec3-208">Для бизнес-приложения, которое ограничено частной корпоративной сетью, воздействие на восприятие пользователей задержки из-за задержки в сети обычно незаметно.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-208">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="f3ec3-209">Для приложения, развернутого через Интернет, задержка может стать заметным для пользователей, особенно в том случае, если пользователи широко распределены географически.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-209">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="f3ec3-210">Использование памяти может также повлиять на задержку приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-210">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="f3ec3-211">Увеличение использования памяти приводит к частым сборкам мусора или выделению памяти на диск, что снижает производительность приложений и, следовательно, увеличивает задержку пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-211">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="f3ec3-212">Для получения дополнительной информации см. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-212">For more information, see <xref:security/blazor/server>.</span></span>

Blazor<span data-ttu-id="f3ec3-213"> серверные приложения должны быть оптимизированы для минимизации задержки пользовательского интерфейса за счет уменьшения задержки сети и использования памяти.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-213"> Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="f3ec3-214">Способ измерения задержки в сети см. в разделе <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-214">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="f3ec3-215">Дополнительные сведения о SignalR и Blazorсм. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-215">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="f3ec3-216">Соединение с сервером</span><span class="sxs-lookup"><span data-stu-id="f3ec3-216">Connection to the server</span></span>

<span data-ttu-id="f3ec3-217">для Blazor серверных приложений требуется активное подключение SignalR к серверу.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-217">Blazor Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="f3ec3-218">Если соединение потеряно, приложение попытается повторно подключиться к серверу.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-218">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="f3ec3-219">Пока состояние клиента по-прежнему находится в памяти, сеанс клиента возобновляется без потери состояния.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-219">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="f3ec3-220">Повторное подключение к тому же серверу</span><span class="sxs-lookup"><span data-stu-id="f3ec3-220">Reconnection to the same server</span></span>

<span data-ttu-id="f3ec3-221">Приложение Blazor Server предварительно отображается в ответ на первый клиентский запрос, который настраивает состояние пользовательского интерфейса на сервере.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-221">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="f3ec3-222">Когда клиент пытается создать SignalR подключение, клиент должен повторно подключиться к тому же серверу.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-222">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> Blazor<span data-ttu-id="f3ec3-223"> серверных приложений, использующих более одного внутреннего сервера, должны реализовывать *закрепленные сеансы* для SignalRных соединений.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-223"> Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="f3ec3-224">Мы рекомендуем использовать для серверных приложений Blazor [службу Azure SignalR](/azure/azure-signalr).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-224">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="f3ec3-225">Она позволяет вертикально масштабировать серверные приложения Blazor для одновременного использования большого числа подключений SignalR.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-225">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="f3ec3-226">Прикрепленные сеансы включены для службы SignalR Azure, задав для параметра `ServerStickyMode` или значения конфигурации службы значение `Required`.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-226">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="f3ec3-227">Для получения дополнительной информации см. <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-227">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="f3ec3-228">При использовании IIS работа с прикрепленными сеансами включает маршрутизацию запросов приложений.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-228">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="f3ec3-229">Дополнительные сведения см. в разделе [Балансировка нагрузки HTTP с использованием маршрутизации запросов приложений](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="f3ec3-229">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="f3ec3-230">Отражение состояния соединения в пользовательском интерфейсе</span><span class="sxs-lookup"><span data-stu-id="f3ec3-230">Reflect the connection state in the UI</span></span>

<span data-ttu-id="f3ec3-231">Когда клиент обнаруживает, что соединение потеряно, пользователь по умолчанию отображает пользовательский интерфейс, когда клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-231">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="f3ec3-232">Если происходит сбой повторного подключения, пользователь предоставляет возможность повторить попытку.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-232">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="f3ec3-233">Чтобы настроить пользовательский интерфейс, определите элемент с `id` `components-reconnect-modal` в `<body>` страницы Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="f3ec3-233">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="f3ec3-234">В следующей таблице описаны классы CSS, применяемые к элементу `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-234">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="f3ec3-235">Класс CSS</span><span class="sxs-lookup"><span data-stu-id="f3ec3-235">CSS class</span></span>                       | <span data-ttu-id="f3ec3-236">Указывает,&hellip;</span><span class="sxs-lookup"><span data-stu-id="f3ec3-236">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="f3ec3-237">Утерянное подключение.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-237">A lost connection.</span></span> <span data-ttu-id="f3ec3-238">Клиент пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-238">The client is attempting to reconnect.</span></span> <span data-ttu-id="f3ec3-239">Отображение модального окна.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-239">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="f3ec3-240">Активное подключение будет восстановлено на сервере.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-240">An active connection is re-established to the server.</span></span> <span data-ttu-id="f3ec3-241">Скрыть модальное окно.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-241">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="f3ec3-242">Сбой повторного подключения, возможно, из-за сбоя сети.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-242">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="f3ec3-243">Чтобы повторить попытку подключения, вызовите `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-243">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="f3ec3-244">Повторное подключение отклонено.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-244">Reconnection rejected.</span></span> <span data-ttu-id="f3ec3-245">Сервер был достигнут, но отклонил подключение, и состояние пользователя на сервере потеряно.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-245">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="f3ec3-246">Чтобы перезагрузить приложение, вызовите `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-246">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="f3ec3-247">Это состояние соединения может появиться в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-247">This connection state may result when:</span></span><ul><li><span data-ttu-id="f3ec3-248">Происходит сбой в цепи на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-248">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="f3ec3-249">Клиент отключается достаточно долго, чтобы сервер отключил состояние пользователя.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-249">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="f3ec3-250">Удаляются экземпляры компонентов, с которыми взаимодействует пользователь.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-250">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="f3ec3-251">Сервер перезагружается, или рабочий процесс приложения перезапускается.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-251">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="f3ec3-252">Повторное подключение с отслеживанием состояния после предварительной подготовки к просмотру</span><span class="sxs-lookup"><span data-stu-id="f3ec3-252">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="f3ec3-253">Серверные приложения Blazor настроены по умолчанию для выprerender пользовательский интерфейс на сервере, прежде чем будет установлено клиентское соединение с сервером.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-253">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="f3ec3-254">Это настраивается на странице Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="f3ec3-254">This is set up in the *_Host.cshtml* Razor page:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

<span data-ttu-id="f3ec3-255">`RenderMode` настраивает, является ли компонент:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-255">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="f3ec3-256">Предварительно отображается на странице.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-256">Is prerendered into the page.</span></span>
* <span data-ttu-id="f3ec3-257">Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки Blazor приложения из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-257">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

::: moniker range=">= aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="f3ec3-258">Описание</span><span class="sxs-lookup"><span data-stu-id="f3ec3-258">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="f3ec3-259">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-259">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="f3ec3-260">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-260">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="f3ec3-261">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-261">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="f3ec3-262">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-262">Output from the component isn't included.</span></span> <span data-ttu-id="f3ec3-263">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-263">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="f3ec3-264">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-264">Renders the component into static HTML.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="f3ec3-265">Описание</span><span class="sxs-lookup"><span data-stu-id="f3ec3-265">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="f3ec3-266">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-266">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="f3ec3-267">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-267">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="f3ec3-268">Параметры не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-268">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="f3ec3-269">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-269">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="f3ec3-270">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-270">Output from the component isn't included.</span></span> <span data-ttu-id="f3ec3-271">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-271">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="f3ec3-272">Параметры не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-272">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="f3ec3-273">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-273">Renders the component into static HTML.</span></span> <span data-ttu-id="f3ec3-274">Поддерживаются параметры.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-274">Parameters are supported.</span></span> |

::: moniker-end

<span data-ttu-id="f3ec3-275">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-275">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="f3ec3-276">Если `RenderMode` `ServerPrerendered`, компонент изначально отрисовывается статически как часть страницы.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-276">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="f3ec3-277">После того, как браузер установит соединение с сервером, компонент *снова*готовится к просмотру, и теперь компонент является интерактивным.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-277">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="f3ec3-278">Если имеется метод жизненно инициализированного метода жизненный цикл [{Async}](xref:blazor/lifecycle#component-initialization-methods) для инициализации компонента, метод выполняется *дважды*:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-278">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="f3ec3-279">Когда компонент предварительно отображается статически.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-279">When the component is prerendered statically.</span></span>
* <span data-ttu-id="f3ec3-280">После установки соединения с сервером.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-280">After the server connection has been established.</span></span>

<span data-ttu-id="f3ec3-281">Это может привести к заметному изменению данных, отображаемых в пользовательском интерфейсе, когда компонент в итоге готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-281">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="f3ec3-282">Чтобы избежать сценария двойной визуализации в приложении Blazor Server, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-282">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="f3ec3-283">Передайте идентификатор, который можно использовать для кэширования состояния во время подготовки к просмотру, и для получения состояния после перезапуска приложения.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-283">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="f3ec3-284">Используйте идентификатор во время подготовки к просмотру для сохранения состояния компонента.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-284">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="f3ec3-285">Используйте идентификатор после предварительной подготовки для получения кэшированного состояния.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-285">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="f3ec3-286">В следующем коде показано обновленное `WeatherForecastService` в серверном приложении Blazor на основе шаблонов, которое позволяет избежать двойной отрисовки:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-286">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

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

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="f3ec3-287">Отображение интерактивных компонентов с отслеживанием состояния из страниц и представлений Razor</span><span class="sxs-lookup"><span data-stu-id="f3ec3-287">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="f3ec3-288">Интерактивные компоненты с отслеживанием состояния могут быть добавлены на страницу Razor или в представление.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-288">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="f3ec3-289">При визуализации страницы или представления:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-289">When the page or view renders:</span></span>

* <span data-ttu-id="f3ec3-290">Компонент предварительно отображается страницей или представлением.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-290">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="f3ec3-291">Исходное состояние компонента, используемое для предварительной визуализации, потеряно.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-291">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="f3ec3-292">Новое состояние компонента создается при установке подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-292">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="f3ec3-293">Следующая страница Razor визуализирует компонент `Counter`:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-293">The following Razor page renders a `Counter` component:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="f3ec3-294">Прорисовка неинтерактивных компонентов на страницах и представлениях Razor</span><span class="sxs-lookup"><span data-stu-id="f3ec3-294">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="f3ec3-295">На следующей странице Razor компонент `Counter` статически подготавливается к просмотру с начальным значением, указанным с помощью формы:</span><span class="sxs-lookup"><span data-stu-id="f3ec3-295">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

::: moniker range=">= aspnetcore-3.1"

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

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

@(await Html.RenderComponentAsync<Counter>(RenderMode.Static, 
    new { InitialValue = InitialValue }))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

<span data-ttu-id="f3ec3-296">Поскольку `MyComponent` является статически отображаемым, компонент не может быть интерактивным.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-296">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="f3ec3-297">Обнаруживать время подготовки приложения к просмотру</span><span class="sxs-lookup"><span data-stu-id="f3ec3-297">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="f3ec3-298">Настройка клиента SignalR для приложений Blazor Server</span><span class="sxs-lookup"><span data-stu-id="f3ec3-298">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="f3ec3-299">Иногда необходимо настроить клиент SignalR, используемый приложениями Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-299">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="f3ec3-300">Например, может потребоваться настроить ведение журнала на SignalRном клиенте для диагностики проблемы с подключением.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-300">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="f3ec3-301">Настройка клиента SignalR в файле *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="f3ec3-301">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="f3ec3-302">Добавьте атрибут `autostart="false"` в тег `<script>` для скрипта *блазор. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="f3ec3-302">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="f3ec3-303">Вызовите `Blazor.start` и передайте объект конфигурации, указывающий построитель SignalR.</span><span class="sxs-lookup"><span data-stu-id="f3ec3-303">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="f3ec3-304">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f3ec3-304">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
