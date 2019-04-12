---
title: Компоненты Razor, размещения моделей
author: guardrex
description: Сведения о Blazor на стороне клиента и сервера ASP.NET Core Razor компоненты на стороне размещения моделей.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515648"
---
# <a name="razor-components-hosting-models"></a><span data-ttu-id="a7268-103">Компоненты Razor, размещения моделей</span><span class="sxs-lookup"><span data-stu-id="a7268-103">Razor Components hosting models</span></span>

<span data-ttu-id="a7268-104">По [Дэниэл рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a7268-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="a7268-105">Компоненты Razor — это веб-платформа, предназначен для запуска на стороне клиента в браузере на [WebAssembly](http://webassembly.org/)-на основе среды выполнения .NET (*Blazor*) или на сервере в ASP.NET Core (*ASP.NET Core Razor Компоненты*).</span><span class="sxs-lookup"><span data-stu-id="a7268-105">Razor Components is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="a7268-106">Независимо от модели размещения модель, приложения и компонента *остаются теми же*.</span><span class="sxs-lookup"><span data-stu-id="a7268-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="a7268-107">В этой статье рассматриваются доступные модели размещения.</span><span class="sxs-lookup"><span data-stu-id="a7268-107">This article discusses the available hosting models:</span></span>

* [<span data-ttu-id="a7268-108">Компоненты Blazor на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="a7268-108">Client-side Blazor</span></span>](#client-side-hosting-model)
* [<span data-ttu-id="a7268-109">Компоненты ASP.NET Core Razor на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="a7268-109">Server-side ASP.NET Core Razor Components</span></span>](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a><span data-ttu-id="a7268-110">Модель размещения на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="a7268-110">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="a7268-111">Основной модели размещения для Blazor — выполнение стороне клиента в браузере на WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="a7268-111">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="a7268-112">Приложение Blazor, его зависимости и среда выполнения .NET скачиваются в браузере.</span><span class="sxs-lookup"><span data-stu-id="a7268-112">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="a7268-113">Приложение выполняется непосредственно в потоке пользовательского интерфейса браузера.</span><span class="sxs-lookup"><span data-stu-id="a7268-113">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="a7268-114">Обновление элементов пользовательского интерфейса и обработка событий происходит в одном процессе.</span><span class="sxs-lookup"><span data-stu-id="a7268-114">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="a7268-115">Ресурсы приложения развертываются как статические файлы веб-сервере или службе, может обслуживать статическое содержимое клиентам.</span><span class="sxs-lookup"><span data-stu-id="a7268-115">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor со стороны клиента: Blazor приложение выполняется в потоке пользовательского интерфейса в браузере.](hosting-models/_static/client-side.png)

<span data-ttu-id="a7268-117">Чтобы создать приложение Blazor, используя модель размещения на стороне клиента, используйте одно из следующих шаблонов:</span><span class="sxs-lookup"><span data-stu-id="a7268-117">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="a7268-118">**Blazor** ([dotnet новый blazor](/dotnet/core/tools/dotnet-new)) &ndash; развернут как набор статических файлов.</span><span class="sxs-lookup"><span data-stu-id="a7268-118">**Blazor** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="a7268-119">**(ASP.NET Core с размещением) Blazor** ([dotnet новый blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; размещаемая сервером ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7268-119">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="a7268-120">Приложение ASP.NET Core обслуживает Blazor приложения клиентам.</span><span class="sxs-lookup"><span data-stu-id="a7268-120">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="a7268-121">Blazor клиентского приложения можно взаимодействовать с сервером по сети с помощью вызовов веб-API или [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="a7268-121">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="a7268-122">Шаблоны содержат *components.webassembly.js* сценария, обрабатывающего:</span><span class="sxs-lookup"><span data-stu-id="a7268-122">The templates include the *components.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="a7268-123">Загрузка среды выполнения .NET, приложения и зависимости приложения.</span><span class="sxs-lookup"><span data-stu-id="a7268-123">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="a7268-124">Инициализация среды выполнения для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="a7268-124">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="a7268-125">Модель размещения клиентского обладает рядом преимуществ.</span><span class="sxs-lookup"><span data-stu-id="a7268-125">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="a7268-126">Blazor на стороне клиента:</span><span class="sxs-lookup"><span data-stu-id="a7268-126">Client-side Blazor:</span></span>

* <span data-ttu-id="a7268-127">Имеет не зависят от .NET на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="a7268-127">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="a7268-128">Полностью использует ресурсы клиента и функции.</span><span class="sxs-lookup"><span data-stu-id="a7268-128">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="a7268-129">Разгрузка работать с сервера клиенту.</span><span class="sxs-lookup"><span data-stu-id="a7268-129">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="a7268-130">Поддерживает сценарии автономной работы.</span><span class="sxs-lookup"><span data-stu-id="a7268-130">Supports offline scenarios.</span></span>

<span data-ttu-id="a7268-131">Есть и недостатки, касающиеся размещения на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a7268-131">There are downsides to client-side hosting.</span></span> <span data-ttu-id="a7268-132">Blazor на стороне клиента:</span><span class="sxs-lookup"><span data-stu-id="a7268-132">Client-side Blazor:</span></span>

* <span data-ttu-id="a7268-133">Ограничивает его возможности браузера.</span><span class="sxs-lookup"><span data-stu-id="a7268-133">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="a7268-134">Требуется клиент с поддержкой оборудования и программного обеспечения (например, WebAssembly поддержка).</span><span class="sxs-lookup"><span data-stu-id="a7268-134">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="a7268-135">Имеет больший размер загрузки и больше приложений время загрузки.</span><span class="sxs-lookup"><span data-stu-id="a7268-135">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="a7268-136">В меньшей даже сложные среды выполнения .NET и поддержка средств (например, ограничения в [.NET Standard](/dotnet/standard/net-standard) поддержки и отладки).</span><span class="sxs-lookup"><span data-stu-id="a7268-136">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="a7268-137">Модель размещения на сервере</span><span class="sxs-lookup"><span data-stu-id="a7268-137">Server-side hosting model</span></span>

<span data-ttu-id="a7268-138">С помощью модели размещения ASP.NET Core Razor компонентов на сервере приложение выполняется на сервере в приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7268-138">With the ASP.NET Core Razor Components server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="a7268-139">Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через [SignalR](xref:signalr/introduction) подключения.</span><span class="sxs-lookup"><span data-stu-id="a7268-139">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![ASP.NET Core Razor компонентов на сервере: Браузер взаимодействует с приложением (размещенный внутри приложения ASP.NET Core) на сервере подключения SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="a7268-141">Чтобы создать приложение Razor компоненты, используя модель размещения на сервере, используйте ASP.NET Core **компоненты Razor** шаблона ([новый razorcomponents dotnet](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="a7268-141">To create a Razor Components app using the server-side hosting model, use the ASP.NET Core **Razor Components** template ([dotnet new razorcomponents](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="a7268-142">Приложение ASP.NET Core размещено приложение на сервере компонентов Razor и задает конечную точку SignalR, где клиенты подключаются.</span><span class="sxs-lookup"><span data-stu-id="a7268-142">The ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="a7268-143">Приложение ASP.NET Core ссылается на приложения `Startup` добавляемый класс:</span><span class="sxs-lookup"><span data-stu-id="a7268-143">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="a7268-144">Службы компонентов Razor на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="a7268-144">Server-side Razor Components services.</span></span>
* <span data-ttu-id="a7268-145">Приложение, чтобы конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="a7268-145">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="a7268-146">*Components.server.js* скрипт&dagger; устанавливает соединение с клиентом.</span><span class="sxs-lookup"><span data-stu-id="a7268-146">The *components.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="a7268-147">Это приложение отвечает за сохранение и восстановление состояние приложения при необходимости (например, в случае потери сетевого подключения).</span><span class="sxs-lookup"><span data-stu-id="a7268-147">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="a7268-148">Модель размещения на сервере обладает рядом преимуществ.</span><span class="sxs-lookup"><span data-stu-id="a7268-148">The server-side hosting model offers several benefits.</span></span> <span data-ttu-id="a7268-149">Компоненты Razor на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="a7268-149">Server-side Razor Components:</span></span>

* <span data-ttu-id="a7268-150">Имеют значительно меньший размер приложения, чем Blazor приложения на стороне клиента и загружаются гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="a7268-150">Have a significantly smaller app size than a client-side Blazor app and load much faster.</span></span>
* <span data-ttu-id="a7268-151">Можно использовать все возможности сервера, включая использование любого совместимых API .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7268-151">Can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="a7268-152">Выполняются в .NET Core на сервере, поэтому существующие .NET, оборудование, например, отладка, работает правильно.</span><span class="sxs-lookup"><span data-stu-id="a7268-152">Run on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="a7268-153">Работает с тонкие клиенты (например, браузеры, которые не поддерживают WebAssembly и ресурсов ограничение устройств).</span><span class="sxs-lookup"><span data-stu-id="a7268-153">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>
* <span data-ttu-id="a7268-154">.NET /C# базы кода, включая кода компонента приложения, не предоставлен клиентам.</span><span class="sxs-lookup"><span data-stu-id="a7268-154">.NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="a7268-155">Есть и недостатки, касающиеся размещения на сервере.</span><span class="sxs-lookup"><span data-stu-id="a7268-155">There are downsides to server-side hosting.</span></span> <span data-ttu-id="a7268-156">Компоненты Razor на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="a7268-156">Server-side Razor Components:</span></span>

* <span data-ttu-id="a7268-157">Имеют большее время задержки: Любое действие пользователя прыжок сети.</span><span class="sxs-lookup"><span data-stu-id="a7268-157">Have higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="a7268-158">Предложения не поддержку автономной работы: В случае сбоя подключения клиента, приложение перестает работать.</span><span class="sxs-lookup"><span data-stu-id="a7268-158">Offer no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="a7268-159">Снижена масштабируемости: Сервер управления подключениями нескольких клиентов и обработки состояния клиента.</span><span class="sxs-lookup"><span data-stu-id="a7268-159">Have reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="a7268-160">Требуется ASP.NET Core-сервер для обслуживания приложения.</span><span class="sxs-lookup"><span data-stu-id="a7268-160">Require an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="a7268-161">Развертывание без сервера (например, из сети доставки Содержимого) невозможно.</span><span class="sxs-lookup"><span data-stu-id="a7268-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="a7268-162">&dagger;*Components.server.js* скрипт публикуется по следующему пути: *bin / {Отладка | Выпуск} / {ЦЕЛЕВОЙ ПЛАТФОРМЫ} /publish/ {имя_приложения}. Приложения/dist/_платформа*.</span><span class="sxs-lookup"><span data-stu-id="a7268-162">&dagger;The *components.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
