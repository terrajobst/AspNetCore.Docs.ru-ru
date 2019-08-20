---
title: Размещение и развертывание приложений Blazor на стороне клиента с помощью ASP.NET Core
author: guardrex
description: Узнайте, как размещать и развертывать приложение Blazor с помощью ASP.NET Core, сетей доставки содержимого (CDN), файловых серверов и GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: e9a42bd4e8511d426761746047fed2d4f7dfc6dd
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994081"
---
# <a name="host-and-deploy-aspnet-core-blazor-client-side"></a><span data-ttu-id="21409-103">Размещение и развертывание приложений Blazor на стороне клиента с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21409-103">Host and deploy ASP.NET Core Blazor client-side</span></span>

<span data-ttu-id="21409-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="21409-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="21409-105">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="21409-105">Host configuration values</span></span>

<span data-ttu-id="21409-106">Приложения Blazor, использующие [модель размещения на стороне клиента](xref:blazor/hosting-models#client-side), могут принимать следующие значения конфигурации узла в качестве аргументов командной строки во время выполнения в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="21409-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="21409-107">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="21409-107">Content Root</span></span>

<span data-ttu-id="21409-108">Аргумент `--contentroot` задает абсолютный путь к каталогу, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="21409-109">В следующих примерах `/content-root-path` — это путь к корневому каталогу содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="21409-110">Передайте этот аргумент при локальном запуске приложения в командной строке.</span><span class="sxs-lookup"><span data-stu-id="21409-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="21409-111">Перейдите в каталог приложения и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="21409-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="21409-112">Добавьте запись в файл приложения *launchSettings.json* в профиле **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="21409-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="21409-113">Этот параметр используется при запуске приложения с помощью отладчика Visual Studio и из командной строки вместе с `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="21409-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="21409-114">В Visual Studio укажите аргумент в меню **Свойства** > **Отладка** > **Аргументы приложения**.</span><span class="sxs-lookup"><span data-stu-id="21409-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="21409-115">Задание аргумента на странице свойств Visual Studio добавляет его в файл *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="21409-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="21409-116">Базовый путь</span><span class="sxs-lookup"><span data-stu-id="21409-116">Path base</span></span>

<span data-ttu-id="21409-117">Аргумент `--pathbase` задает базовый путь приложения для локального выполнения с некорневым виртуальным путем (в `<base>` тег `href` задан с путем, отличным от `/` для промежуточной и рабочей сред).</span><span class="sxs-lookup"><span data-stu-id="21409-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="21409-118">В следующих примерах `/virtual-path` — это базовый путь приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="21409-119">Дополнительные сведения см. в разделе [Базовый путь приложения](#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="21409-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21409-120">В отличие от пути, заданного через `href` в теге `<base>`, не ставьте косую черту (`/`) при передаче значения аргумента `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="21409-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="21409-121">Если основной путь приложения задан в теге `<base>` как `<base href="/CoolApp/">` (включая косую черту в конце), передайте значение аргумента командной строки как `--pathbase=/CoolApp` (без косой черты в конце).</span><span class="sxs-lookup"><span data-stu-id="21409-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="21409-122">Передайте этот аргумент при локальном запуске приложения в командной строке.</span><span class="sxs-lookup"><span data-stu-id="21409-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="21409-123">Перейдите в каталог приложения и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="21409-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="21409-124">Добавьте запись в файл приложения *launchSettings.json* в профиле **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="21409-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="21409-125">Этот параметр используется при запуске приложения с помощью отладчика Visual Studio и из командной строки вместе с `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="21409-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="21409-126">В Visual Studio укажите аргумент в меню **Свойства** > **Отладка** > **Аргументы приложения**.</span><span class="sxs-lookup"><span data-stu-id="21409-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="21409-127">Задание аргумента на странице свойств Visual Studio добавляет его в файл *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="21409-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="21409-128">URL-адреса</span><span class="sxs-lookup"><span data-stu-id="21409-128">URLs</span></span>

<span data-ttu-id="21409-129">Аргумент `--urls` задает IP-адреса или адреса узлов с портами и протоколами, по которым ожидаются запросы.</span><span class="sxs-lookup"><span data-stu-id="21409-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="21409-130">Передайте этот аргумент при локальном запуске приложения в командной строке.</span><span class="sxs-lookup"><span data-stu-id="21409-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="21409-131">Перейдите в каталог приложения и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="21409-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="21409-132">Добавьте запись в файл приложения *launchSettings.json* в профиле **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="21409-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="21409-133">Этот параметр используется при запуске приложения с помощью отладчика Visual Studio и из командной строки вместе с `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="21409-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="21409-134">В Visual Studio укажите аргумент в меню **Свойства** > **Отладка** > **Аргументы приложения**.</span><span class="sxs-lookup"><span data-stu-id="21409-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="21409-135">Задание аргумента на странице свойств Visual Studio добавляет его в файл *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="21409-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="21409-136">Развертывание</span><span class="sxs-lookup"><span data-stu-id="21409-136">Deployment</span></span>

<span data-ttu-id="21409-137">[Модель размещения на стороне клиента](xref:blazor/hosting-models#client-side).</span><span class="sxs-lookup"><span data-stu-id="21409-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side):</span></span>

* <span data-ttu-id="21409-138">Приложение Blazor, его зависимости и среда выполнения .NET скачиваются в браузере.</span><span class="sxs-lookup"><span data-stu-id="21409-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="21409-139">Приложение выполняется непосредственно в потоке пользовательского интерфейса браузера.</span><span class="sxs-lookup"><span data-stu-id="21409-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="21409-140">Используется одна из следующих стратегий:</span><span class="sxs-lookup"><span data-stu-id="21409-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="21409-141">Приложение Blazor обслуживается приложением ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="21409-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="21409-142">Эта стратегия описана в разделе [Размещенное развертывание с использованием ASP.NET Core](#hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="21409-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="21409-143">Приложение Blazor помещается на статическом веб-сервере или службе, где .NET не используется, чтобы обслуживать приложение Blazor.</span><span class="sxs-lookup"><span data-stu-id="21409-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="21409-144">Эта стратегия описана в разделе [Автономное развертывание](#standalone-deployment).</span><span class="sxs-lookup"><span data-stu-id="21409-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="21409-145">Настройка компоновщика</span><span class="sxs-lookup"><span data-stu-id="21409-145">Configure the Linker</span></span>

<span data-ttu-id="21409-146">Blazor выполняет компоновку для промежуточного языка (IL) в каждой сборке, чтобы удалить ненужный код из выходных сборок.</span><span class="sxs-lookup"><span data-stu-id="21409-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="21409-147">Связыванием сборки можно управлять в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="21409-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="21409-148">Дополнительные сведения можно найти по адресу: <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="21409-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="21409-149">Переопределение URL-адресов для маршрутизации</span><span class="sxs-lookup"><span data-stu-id="21409-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="21409-150">Маршрутизация запросов к компонентам страниц в приложении на стороне клиента не сложнее, чем маршрутизация запросов к приложению, размещенному на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="21409-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="21409-151">Рассмотрим сценарий с клиентским приложением с двумя компонентами:</span><span class="sxs-lookup"><span data-stu-id="21409-151">Consider a client-side app with two components:</span></span>

* <span data-ttu-id="21409-152">*Main.razor* — загружается в корне приложения и содержит ссылку на компонент `About` (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="21409-152">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="21409-153">*About.razor* — компонент `About`.</span><span class="sxs-lookup"><span data-stu-id="21409-153">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="21409-154">При запросе документа приложения по умолчанию в адресной строке браузера (например, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="21409-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="21409-155">Браузер отправляет запрос.</span><span class="sxs-lookup"><span data-stu-id="21409-155">The browser makes a request.</span></span>
1. <span data-ttu-id="21409-156">Возвращается страница по умолчанию; обычно это *index.html*.</span><span class="sxs-lookup"><span data-stu-id="21409-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="21409-157">Страница *index.html* обеспечивает начальную загрузку приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="21409-158">Загружается маршрутизатор Blazor, и компонент `Main` Razor преобразовывается для просмотра.</span><span class="sxs-lookup"><span data-stu-id="21409-158">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="21409-159">На странице Main возможность выбора ссылки на компонент `About` доступна на клиентском компьютере, так как маршрутизатор Blazor не разрешает браузеру сделать запрос в Интернете к `www.contoso.com` для `About` и сам обрабатывает отображенный компонент `About`.</span><span class="sxs-lookup"><span data-stu-id="21409-159">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="21409-160">Все запросы внутренних конечных точек *в пределах клиентского приложения* будут работать так же: Запросы не активируют браузерные запросы к ресурсам, размещенным на сервере в Интернете.</span><span class="sxs-lookup"><span data-stu-id="21409-160">All of the requests for internal endpoints *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="21409-161">Маршрутизатор обрабатывает запросы внутренним образом.</span><span class="sxs-lookup"><span data-stu-id="21409-161">The router handles the requests internally.</span></span>

<span data-ttu-id="21409-162">Если запрос `www.contoso.com/About` сделан с помощью адресной строки браузера, он завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="21409-162">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="21409-163">Такой ресурс не существует на интернет-узле приложения, поэтому возвращается ответ *404 — Not Found* (не найдено).</span><span class="sxs-lookup"><span data-stu-id="21409-163">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="21409-164">Поскольку браузеры выполняют запросы к интернет-узлам для страниц на стороне клиента, веб-серверам и службам размещения необходимо переопределять все запросы к ресурсам, физически находящимся не на сервере, в случае страницы *index.html*.</span><span class="sxs-lookup"><span data-stu-id="21409-164">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="21409-165">Когда *index.html* возвращается, маршрутизатор клиентского приложения берет на себя управление и отвечает необходимым ресурсом.</span><span class="sxs-lookup"><span data-stu-id="21409-165">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="21409-166">Базовый путь приложения</span><span class="sxs-lookup"><span data-stu-id="21409-166">App base path</span></span>

<span data-ttu-id="21409-167">*Базовый путь приложения* — это корневой путь виртуального приложения на сервере.</span><span class="sxs-lookup"><span data-stu-id="21409-167">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="21409-168">Например, приложение, которое находится на сервере Contoso в виртуальной папке в `/CoolApp/`, достигается через `https://www.contoso.com/CoolApp` и имеет виртуальный базовый путь `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="21409-168">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="21409-169">Если установить в качестве базового пути приложения виртуальный путь (`<base href="/CoolApp/">`), то приложение узнает свое виртуальное размещение на сервере.</span><span class="sxs-lookup"><span data-stu-id="21409-169">By setting the app base path to the virtual path (`<base href="/CoolApp/">`), the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="21409-170">Приложение может использовать базовый путь приложения для создания URL-адресов относительно корневого каталога приложения из компонента, который не находится в корневом каталоге.</span><span class="sxs-lookup"><span data-stu-id="21409-170">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="21409-171">Благодаря этому компоненты, которые существуют на разных уровнях структуры каталогов, могут создавать ссылки на другие ресурсы во всех местах приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-171">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="21409-172">Основной путь приложения также используется для перехвата щелчка гиперссылок, когда `href` целевой объект ссылки находится в пределах URI-пространства базового пути приложения&mdash;; здесь маршрутизатор Blazor отвечает за внутреннюю навигацию.</span><span class="sxs-lookup"><span data-stu-id="21409-172">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="21409-173">Во многих сценариях размещения виртуальный серверный путь к приложению является корнем приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-173">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="21409-174">В таких случаях базовый путь приложения является косой чертой (`<base href="/" />`), что служит конфигурацией по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-174">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="21409-175">В других сценариях размещения, таких как страницы GitHub и виртуальные каталоги и дочерние приложения IIS, базовому пути приложения должно быть присвоено значение виртуального серверного пути к приложению.</span><span class="sxs-lookup"><span data-stu-id="21409-175">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="21409-176">Чтобы задать базовый путь приложения, измените тег `<base>` в элементах тега `<head>` файла *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="21409-176">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="21409-177">Задайте значение атрибута `href` как `/virtual-path/` (косая черта в конце обязательна), где `/virtual-path/` — полный корневой путь виртуального приложения на сервере приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-177">Set the `href` attribute value to `/virtual-path/` (the trailing slash is required), where `/virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="21409-178">В приведенном выше примере виртуальный путь имеет значение `/CoolApp/`: `<base href="/CoolApp/">`.</span><span class="sxs-lookup"><span data-stu-id="21409-178">In the preceding example, the virtual path is set to `/CoolApp/`: `<base href="/CoolApp/">`.</span></span>

<span data-ttu-id="21409-179">Приложение с некорневым виртуальным путем (например, `<base href="/CoolApp/">`) не сможет найти свои ресурсы *при локальном запуске*.</span><span class="sxs-lookup"><span data-stu-id="21409-179">For an app with a non-root virtual path configured (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="21409-180">Для решения этой проблемы во время локальной разработки и тестирования можно предоставить аргумент *базового пути*, который соответствует значению `href` тега `<base>` во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="21409-180">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="21409-181">Для передачи аргумента базового пути с корневым путем (`/`) при локальном запуске приложения выполните из каталога приложения команду `dotnet run` с параметром `--pathbase`:</span><span class="sxs-lookup"><span data-stu-id="21409-181">To pass the path base argument with the root path (`/`) when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

<span data-ttu-id="21409-182">Для приложений с виртуальным базовым путем `/CoolApp/` (`<base href="/CoolApp/">`) команда имеет следующий вид:</span><span class="sxs-lookup"><span data-stu-id="21409-182">For an app with a virtual base path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="21409-183">Приложение отвечает локально по `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="21409-183">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="21409-184">Дополнительные сведения см. в разделе [Значение конфигурации узла для базового пути](#path-base).</span><span class="sxs-lookup"><span data-stu-id="21409-184">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="21409-185">Приложение может использовать [модель размещения на стороне клиента](xref:blazor/hosting-models#client-side) (на основе шаблона проекта **Приложение Blazor WebAssembly**  — шаблона `blazorwasm` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)) и размещаться как дочернее приложение IIS в приложении ASP.NET Core. В таком случае важно отключить унаследованный обработчик модулей ASP.NET Core или убедиться в том, что раздел `<handlers>` файла *web.config* корневого (родительского) приложения не унаследован дочерним приложением.</span><span class="sxs-lookup"><span data-stu-id="21409-185">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side) (based on the **Blazor WebAssembly App** project template, the `blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) and is hosted as an IIS sub-app in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="21409-186">Удалите обработчик в опубликованном файле *web.config* приложения, добавив раздел `<handlers>` в файл:</span><span class="sxs-lookup"><span data-stu-id="21409-186">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="21409-187">Кроме того, отключите наследование раздела `<system.webServer>` от корневого (родительского) приложения, задав для элемента `<location>` значение `inheritInChildApplications` как `false`:</span><span class="sxs-lookup"><span data-stu-id="21409-187">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" ... />
      </handlers>
      <aspNetCore ... />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="21409-188">Удаление обработчика или отключение наследования производится в дополнение к настройке базового пути приложения, как описано в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="21409-188">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="21409-189">Задайте базовый путь приложения в файле *index.html* приложения как псевдоним IIS, используемый при настройке дочернего приложения в IIS.</span><span class="sxs-lookup"><span data-stu-id="21409-189">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="21409-190">Размещенное развертывание с использованием ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21409-190">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="21409-191">*Размещенное развертывание*  обслуживает клиентское приложение Blazor для браузеров из [приложения ASP.NET Core](xref:index), выполняющегося на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="21409-191">A *hosted deployment* serves the Blazor client-side app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="21409-192">Приложение Blazor входит в состав приложения ASP.NET Core в публикуемых выходных данных; таким образом, два приложения развертываются вместе.</span><span class="sxs-lookup"><span data-stu-id="21409-192">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="21409-193">Веб-сервер, позволяющий размещать приложения ASP.NET Core, является обязательным.</span><span class="sxs-lookup"><span data-stu-id="21409-193">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="21409-194">Для размещенного развертывания Visual Studio включает шаблон проекта **Приложение Blazor WebAssembly**  (шаблон `blazorwasm` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)) с выбранным параметром **Размещенный**.</span><span class="sxs-lookup"><span data-stu-id="21409-194">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected.</span></span>

<span data-ttu-id="21409-195">Дополнительные сведения о размещении приложения ASP.NET Core и его развертывании см. в разделе <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="21409-195">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="21409-196">Подробнее о развертывании в службе приложений Azure см. в <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="21409-196">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="21409-197">Автономное развертывание</span><span class="sxs-lookup"><span data-stu-id="21409-197">Standalone deployment</span></span>

<span data-ttu-id="21409-198">*Автономное развертывание* обрабатывает клиентское приложение Blazor как набор статических файлов, которые будут запрашиваться непосредственно клиентами.</span><span class="sxs-lookup"><span data-stu-id="21409-198">A *standalone deployment* serves the Blazor client-side app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="21409-199">Любой статический файловый сервер способен обслуживать приложение Blazor.</span><span class="sxs-lookup"><span data-stu-id="21409-199">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="21409-200">Изолированные ресурсы развертывания публикуются в папке *bin/Release/{целевая_платформа}publish/{имя_сборки}/dist*.</span><span class="sxs-lookup"><span data-stu-id="21409-200">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="21409-201">IIS</span><span class="sxs-lookup"><span data-stu-id="21409-201">IIS</span></span>

<span data-ttu-id="21409-202">IIS можно использовать как полнофункциональный статический файловый сервер для приложений Blazor.</span><span class="sxs-lookup"><span data-stu-id="21409-202">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="21409-203">Чтобы настроить IIS для размещения Blazor, см. раздел [Построение статического веб-сайта на IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="21409-203">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="21409-204">Опубликованные ресурсы создаются в папке */bin/Release/{целевая_платформа}/publish*.</span><span class="sxs-lookup"><span data-stu-id="21409-204">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="21409-205">Разместить содержимое папки *publish* на веб-сервере или в службе размещения.</span><span class="sxs-lookup"><span data-stu-id="21409-205">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="21409-206">web.config.</span><span class="sxs-lookup"><span data-stu-id="21409-206">web.config</span></span>

<span data-ttu-id="21409-207">При публикации проекта Blazor создается файл *web.config* со следующей конфигурацией IIS:</span><span class="sxs-lookup"><span data-stu-id="21409-207">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="21409-208">Типы MIME заданы для следующих расширений файлов:</span><span class="sxs-lookup"><span data-stu-id="21409-208">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="21409-209">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="21409-209">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="21409-210">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="21409-210">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="21409-211">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="21409-211">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="21409-212">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="21409-212">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="21409-213">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="21409-213">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="21409-214">Сжатие HTTP включено для следующих типов MIME:</span><span class="sxs-lookup"><span data-stu-id="21409-214">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="21409-215">Задаются правила модуля переопределения URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="21409-215">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="21409-216">Обслуживание вложенного каталога, в котором находятся статические ресурсы приложения ( *{имя_сборки}/dist/{запрошенный_путь}* ).</span><span class="sxs-lookup"><span data-stu-id="21409-216">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="21409-217">Создание резервного маршрута одностраничного приложения, чтобы запросы, не относящиеся к файлам ресурсов, перенаправлялись к документу приложения по умолчанию в папке статических ресурсов ( *{имя_сборки}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="21409-217">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="21409-218">Установка модуля переопределения URL-адресов</span><span class="sxs-lookup"><span data-stu-id="21409-218">Install the URL Rewrite Module</span></span>

<span data-ttu-id="21409-219">[Модуль переопределения URL-адресов](https://www.iis.net/downloads/microsoft/url-rewrite) нужен, чтобы переопределять URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="21409-219">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="21409-220">Модуль не устанавливается по умолчанию и недоступен для установки как компонент службы роли веб-сервера (IIS).</span><span class="sxs-lookup"><span data-stu-id="21409-220">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="21409-221">Модуль необходимо скачать с веб-сайта IIS.</span><span class="sxs-lookup"><span data-stu-id="21409-221">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="21409-222">Для его установки используйте установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="21409-222">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="21409-223">Локально перейдите на [страницу скачивания модуля переопределения URL-адресов](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="21409-223">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="21409-224">В англоязычной версии выберите **WebPI** для скачивания установщика веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="21409-224">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="21409-225">Для других языков выберите подходящую архитектуру сервера (x86 или x64) для скачивания установщика.</span><span class="sxs-lookup"><span data-stu-id="21409-225">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="21409-226">Скопируйте установщик на сервер.</span><span class="sxs-lookup"><span data-stu-id="21409-226">Copy the installer to the server.</span></span> <span data-ttu-id="21409-227">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="21409-227">Run the installer.</span></span> <span data-ttu-id="21409-228">Нажмите кнопку **Установить** и примите условия лицензионного соглашения.</span><span class="sxs-lookup"><span data-stu-id="21409-228">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="21409-229">Перезагрузка сервера после завершения установки не требуется.</span><span class="sxs-lookup"><span data-stu-id="21409-229">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="21409-230">Настройка веб-сайта</span><span class="sxs-lookup"><span data-stu-id="21409-230">Configure the website</span></span>

<span data-ttu-id="21409-231">Установите для веб сайта **физический путь** к папке приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-231">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="21409-232">Папка содержит:</span><span class="sxs-lookup"><span data-stu-id="21409-232">The folder contains:</span></span>

* <span data-ttu-id="21409-233">Файл *web.config*, который используется службами IIS для настройки веб-сайта, включая необходимые правила перенаправления и типы содержимого файлов.</span><span class="sxs-lookup"><span data-stu-id="21409-233">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="21409-234">Папку статических ресурсов приложения.</span><span class="sxs-lookup"><span data-stu-id="21409-234">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="21409-235">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="21409-235">Troubleshooting</span></span>

<span data-ttu-id="21409-236">Если получено сообщение *500 — Internal Server Error* (внутренняя ошибка сервера), а диспетчер служб IIS возвращает ошибки при попытке получить доступ к конфигурации веб сайта, убедитесь, что установлен модуль переопределения URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="21409-236">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="21409-237">Если модуль не установлен, IIS не может проанализировать файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="21409-237">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="21409-238">Это предотвращает загрузку конфигурации веб сайта диспетчером служб IIS и передачу статических файлов Blazor c веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="21409-238">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="21409-239">Дополнительные сведения об устранении неполадок развертывания в службах IIS см. в разделе <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="21409-239">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="21409-240">Хранилище Azure</span><span class="sxs-lookup"><span data-stu-id="21409-240">Azure Storage</span></span>

<span data-ttu-id="21409-241">Размещение статических файлов службы хранилища Azure предусматривает размещение бессерверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="21409-241">Azure Storage static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="21409-242">Поддерживаются пользовательские доменные имена, сеть доставки содержимого Azure (CDN) и HTTPS.</span><span class="sxs-lookup"><span data-stu-id="21409-242">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="21409-243">Если служба BLOB-объектов настроена для размещения статических веб-сайтов в учетной записи хранения:</span><span class="sxs-lookup"><span data-stu-id="21409-243">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="21409-244">Задайте для параметра **Имя документа индекса** значение `index.html`.</span><span class="sxs-lookup"><span data-stu-id="21409-244">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="21409-245">Задайте для параметра **Путь к документу с ошибкой** значение `index.html`.</span><span class="sxs-lookup"><span data-stu-id="21409-245">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="21409-246">Компоненты Razor и другие конечные точки, не являющиеся файлами, не имеют физических путей в статическом содержимом, хранящемся в службе больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="21409-246">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="21409-247">При получении запроса для одного из этих ресурсов, который должен обрабатываться маршрутизатором Blazor, ошибка *404 — Not Found* (не найдено), создаваемая службой BLOB-объектов, перенаправляет запрос, используя **путь к документу с ошибкой**.</span><span class="sxs-lookup"><span data-stu-id="21409-247">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="21409-248">При этом возвращается большой двоичный объект *index.html*, и маршрутизатор Blazor загружает и обрабатывает путь.</span><span class="sxs-lookup"><span data-stu-id="21409-248">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="21409-249">См. подробнее о [размещении статических веб-сайтов в службе хранилища Azure](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="21409-249">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="21409-250">Nginx</span><span class="sxs-lookup"><span data-stu-id="21409-250">Nginx</span></span>

<span data-ttu-id="21409-251">Следующий файл *nginx.conf* упрощен для демонстрации того, как настроить Nginx для отправки файла *Index.html*, когда не удается найти соответствующий файл на диске.</span><span class="sxs-lookup"><span data-stu-id="21409-251">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

<span data-ttu-id="21409-252">Дополнительные сведения о конфигурации веб-сервера Nginx в рабочей среде см. в разделе [Создание файлов конфигурации NGINX Plus и NGINX](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="21409-252">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="21409-253">Nginx в Docker</span><span class="sxs-lookup"><span data-stu-id="21409-253">Nginx in Docker</span></span>

<span data-ttu-id="21409-254">Чтобы разместить Blazor в Docker с помощью Nginx, настройте в Dockerfile образ с Nginx на основе Alpine.</span><span class="sxs-lookup"><span data-stu-id="21409-254">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="21409-255">Измените Dockerfile, скопировав файл *nginx.config* в контейнер.</span><span class="sxs-lookup"><span data-stu-id="21409-255">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="21409-256">Добавьте одну строку в Dockerfile, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="21409-256">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="21409-257">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="21409-257">GitHub Pages</span></span>

<span data-ttu-id="21409-258">Чтобы обеспечить переопределение URL-адресов, добавьте файл *404.html* со сценарием, который производит перенаправление на страницу *index.html*.</span><span class="sxs-lookup"><span data-stu-id="21409-258">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="21409-259">Пример реализации, предоставленный сообществом, см. в разделе [Одностраничные приложения для страниц GitHub](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages на сайте GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="21409-259">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="21409-260">Пример с использованием подхода сообщества можно увидеть в разделе [blazor-demo/blazor-demo.github.io на сайте GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([активный сайт](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="21409-260">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="21409-261">При использовании сайта проекта вместо сайта организации следует добавить или обновить тег `<base>` в файле *index.html*.</span><span class="sxs-lookup"><span data-stu-id="21409-261">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="21409-262">Задайте для имени репозитория GitHub значение атрибута `href` с косой чертой в конце (например, `my-repository/`).</span><span class="sxs-lookup"><span data-stu-id="21409-262">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>
