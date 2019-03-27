---
title: Размещение и развертывание компонентов Razor
author: guardrex
description: 'Узнайте, как производится размещение и развертывание компонентов Razor и приложений Blazor с помощью ASP.NET Core, сетей доставки содержимого (CDN), файловых серверов и страниц GitHub.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2019
uid: host-and-deploy/razor-components/index
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="6ba1e-103">Размещение и развертывание компонентов Razor</span><span class="sxs-lookup"><span data-stu-id="6ba1e-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="6ba1e-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="6ba1e-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="6ba1e-105">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="6ba1e-105">Publish the app</span></span>

<span data-ttu-id="6ba1e-106">Приложения публикуются для развертывания в конфигурации выпуска при помощи команды [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="6ba1e-107">Интегрированная среда разработки может обрабатывать выполнение команды `dotnet publish` автоматически с помощью своих встроенных возможностей публикации, поэтому может быть необязательно вручную выполнять команду из командной строки в зависимости от используемых средств разработки.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-107">An IDE may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="6ba1e-108">`dotnet publish` активирует [восстановление](/dotnet/core/tools/dotnet-restore) зависимостей проекта и проводит [сборку](/dotnet/core/tools/dotnet-build) проекта, прежде чем создавать ресурсы для развертывания.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="6ba1e-109">В ходе процесса построения удаляются неиспользуемые методы и сборки, чтобы уменьшить размер скачиваемого приложения и время загрузки.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="6ba1e-110">Развертывание создается в папке */bin/Release/&lt;имя целевой платформы&gt;/publish*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-110">The deployment is created in the */bin/Release/&lt;target-framework&gt;/publish* folder.</span></span>

<span data-ttu-id="6ba1e-111">Ресурсы в папке *publish* развертываются на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="6ba1e-112">Развертывание может проводиться вручную или автоматизированно в зависимости от используемых средств разработки.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="6ba1e-113">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="6ba1e-113">Host configuration values</span></span>

<span data-ttu-id="6ba1e-114">Приложения на основе компонентов Razor, использующие [модель размещения на сервере](xref:razor-components/hosting-models#server-side-hosting-model), могут принимать [значения конфигурации веб-узла](xref:fundamentals/host/web-host#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-114">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Web Host configuration values](xref:fundamentals/host/web-host#host-configuration-values).</span></span>

<span data-ttu-id="6ba1e-115">Приложения Blazor, использующие [модель размещения на стороне клиента](xref:razor-components/hosting-models#client-side-hosting-model), могут принимать следующие значения конфигурации узла в качестве аргументов командной строки во время выполнения в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-115">Blazor apps that use the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="6ba1e-116">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="6ba1e-116">Content Root</span></span>

<span data-ttu-id="6ba1e-117">Аргумент `--contentroot` задает абсолютный путь к каталогу, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-117">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span>

* <span data-ttu-id="6ba1e-118">Передайте этот аргумент при локальном запуске приложения в командной строке.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-118">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="6ba1e-119">Перейдите в каталог приложения и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-119">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* <span data-ttu-id="6ba1e-120">Добавьте запись в файл приложения *launchSettings.json* в профиле **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-120">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="6ba1e-121">Этот параметр выбирается при выполнении приложения с помощью отладчика Visual Studio и при запуске приложения из командной строки с помощью `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-121">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* <span data-ttu-id="6ba1e-122">В Visual Studio укажите аргумент в меню **Свойства** > **Отладка** > **Аргументы приложения**.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-122">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="6ba1e-123">Задание аргумента на странице свойств Visual Studio добавляет его в файл *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-123">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a><span data-ttu-id="6ba1e-124">Базовый путь</span><span class="sxs-lookup"><span data-stu-id="6ba1e-124">Path base</span></span>

<span data-ttu-id="6ba1e-125">Аргумент `--pathbase` задает базовый путь приложения для локального выполнения с некорневым виртуальным путем (в `<base>` тег `href` задан с путем, отличным от `/` для промежуточной и рабочей сред).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-125">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="6ba1e-126">Дополнительные сведения см. в разделе [Базовый путь приложения](#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-126">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6ba1e-127">В отличие от пути, заданного через `href` в теге `<base>`, не ставьте косую черту (`/`) при передаче значения аргумента `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-127">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="6ba1e-128">Если основной путь приложения задан в теге `<base>` как `<base href="/CoolApp/" />` (включая косую черту в конце), передайте значение аргумента командной строки как `--pathbase=/CoolApp` (без косой черты в конце).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-128">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/" />` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="6ba1e-129">Передайте этот аргумент при локальном запуске приложения в командной строке.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-129">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="6ba1e-130">Перейдите в каталог приложения и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-130">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* <span data-ttu-id="6ba1e-131">Добавьте запись в файл приложения *launchSettings.json* в профиле **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-131">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="6ba1e-132">Этот параметр выбирается при выполнении приложения с помощью отладчика Visual Studio и при запуске приложения из командной строки с помощью `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-132">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* <span data-ttu-id="6ba1e-133">В Visual Studio укажите аргумент в меню **Свойства** > **Отладка** > **Аргументы приложения**.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-133">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="6ba1e-134">Задание аргумента на странице свойств Visual Studio добавляет его в файл *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-134">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a><span data-ttu-id="6ba1e-135">URL-адреса</span><span class="sxs-lookup"><span data-stu-id="6ba1e-135">URLs</span></span>

<span data-ttu-id="6ba1e-136">Аргумент `--urls` задает IP-адреса или адреса узлов с портами и протоколами, по которым следует ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-136">The `--urls` argument indicates the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="6ba1e-137">Передайте этот аргумент при локальном запуске приложения в командной строке.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-137">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="6ba1e-138">Перейдите в каталог приложения и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-138">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="6ba1e-139">Добавьте запись в файл приложения *launchSettings.json* в профиле **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-139">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="6ba1e-140">Этот параметр выбирается при выполнении приложения с помощью отладчика Visual Studio и при запуске приложения из командной строки с помощью `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-140">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="6ba1e-141">В Visual Studio укажите аргумент в меню **Свойства** > **Отладка** > **Аргументы приложения**.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-141">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="6ba1e-142">Задание аргумента на странице свойств Visual Studio добавляет его в файл *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-142">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a><span data-ttu-id="6ba1e-143">Развертывание приложения Blazor на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="6ba1e-143">Deploy a client-side Blazor app</span></span>

<span data-ttu-id="6ba1e-144">[Модель размещения на стороне клиента](xref:razor-components/hosting-models#client-side-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-144">With the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model):</span></span>

* <span data-ttu-id="6ba1e-145">Приложение Blazor, его зависимости и среда выполнения .NET скачиваются в браузере.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-145">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="6ba1e-146">Приложение выполняется непосредственно в потоке пользовательского интерфейса браузера.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-146">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="6ba1e-147">Используется одна из следующих стратегий:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-147">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="6ba1e-148">Приложение Blazor обслуживается приложением ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-148">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="6ba1e-149">См. раздел [Размещенное на стороне клиента развертывание Blazor с использованием ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-149">Covered in the [Client-side Blazor hosted deployment with ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="6ba1e-150">Приложение Blazor помещается на статическом веб-сервере или службе, где .NET не используется, чтобы обслуживать приложение Blazor.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-150">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="6ba1e-151">См. раздел [Развертывание изолированного приложения Blazor на стороне клиента](#client-side-blazor-standalone-deployment).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-151">Covered in the [Client-side Blazor standalone deployment](#client-side-blazor-standalone-deployment) section.</span></span>

### <a name="configure-the-linker"></a><span data-ttu-id="6ba1e-152">Настройка компоновщика</span><span class="sxs-lookup"><span data-stu-id="6ba1e-152">Configure the Linker</span></span>

<span data-ttu-id="6ba1e-153">Blazor выполняет компоновку для промежуточного языка (IL) в каждой сборке, чтобы удалить ненужный код из выходных сборок.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-153">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="6ba1e-154">Вы можете управлять компоновкой в ходе сборки.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-154">You can control assembly linking on build.</span></span> <span data-ttu-id="6ba1e-155">Для получения дополнительной информации см. <xref:host-and-deploy/razor-components/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-155">For more information, see <xref:host-and-deploy/razor-components/configure-linker>.</span></span>

### <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="6ba1e-156">Переопределение URL-адресов для маршрутизации</span><span class="sxs-lookup"><span data-stu-id="6ba1e-156">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="6ba1e-157">Маршрутизация запросов к компонентам страниц в приложении на стороне клиента не сложнее, чем маршрутизация запросов к приложению, размещенному на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-157">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="6ba1e-158">Рассмотрим случай клиентского приложения с двумя страницами:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-158">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="6ba1e-159">**_Main.cshtml_** &ndash; загружается в корне приложения и содержит ссылку на страницу About (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-159">**_Main.cshtml_** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="6ba1e-160">**_About.cshtml_** &ndash; — страница со сведениями.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-160">**_About.cshtml_** &ndash; About page.</span></span>

<span data-ttu-id="6ba1e-161">При запросе документа приложения по умолчанию в адресной строке браузера (например, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="6ba1e-161">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="6ba1e-162">Браузер отправляет запрос.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-162">The browser makes a request.</span></span>
1. <span data-ttu-id="6ba1e-163">Возвращается страница по умолчанию; обычно это *index.html*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-163">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="6ba1e-164">Страница *index.html* обеспечивает начальную загрузку приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-164">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="6ba1e-165">Загружается маршрутизатор Blazor; отображается страница Razor Main (*Main.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-165">Blazor's router loads and the Razor Main page (*Main.cshtml*) is displayed.</span></span>

<span data-ttu-id="6ba1e-166">На странице "Main" выбор ссылки на страницу About загружает страницу About.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-166">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="6ba1e-167">Выбор ссылки на страницу About работает на клиентском компьютере, так как маршрутизатор Blazor не дает браузеру сделать запрос в Интернете к `www.contoso.com` для `About` и сам передает страницу About.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-167">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="6ba1e-168">Все запросы внутренних страниц *в пределах клиентского приложения* будут работать так же: Запросы не активируют браузерные запросы к ресурсам, размещенным на сервере в Интернете.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-168">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="6ba1e-169">Маршрутизатор обрабатывает запросы внутренним образом.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-169">The router handles the requests internally.</span></span>

<span data-ttu-id="6ba1e-170">Если запрос `www.contoso.com/About` сделан с помощью адресной строки браузера, он завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-170">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="6ba1e-171">Такого ресурса не существует на интернет-узле приложения, поэтому возвращается ответ *404 не найдено*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-171">No such resource exists on the app's Internet host, so a *404 Not found* response is returned.</span></span>

<span data-ttu-id="6ba1e-172">Поскольку браузеры выполняют запросы к интернет-узлам для страниц на стороне клиента, веб-серверам и службам размещения необходимо переопределять все запросы к ресурсам, физически находящимся не на сервере, в случае страницы *index.html*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-172">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="6ba1e-173">Когда *index.html* возвращается, маршрутизатор клиентского приложения берет на себя управление и отвечает необходимым ресурсом.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-173">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

### <a name="app-base-path"></a><span data-ttu-id="6ba1e-174">Базовый путь приложения</span><span class="sxs-lookup"><span data-stu-id="6ba1e-174">App base path</span></span>

<span data-ttu-id="6ba1e-175">Базовый путь приложения — это корневой путь виртуального приложения на сервере.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-175">The app base path is the virtual app root path on the server.</span></span> <span data-ttu-id="6ba1e-176">Например, приложение, которое находится на сервере Contoso в виртуальной папке в `/CoolApp/`, достигается через `https://www.contoso.com/CoolApp` и имеет виртуальный базовый путь `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-176">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="6ba1e-177">Если установить базовый путь приложения как `CoolApp/`, приложению становится известно, где оно фактически находится на сервере.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-177">By setting the app base path to `CoolApp/`, the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="6ba1e-178">Приложение может использовать базовый путь приложения для создания URL-адресов относительно корневого каталога приложения из компонента, который не находится в корневом каталоге.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-178">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="6ba1e-179">Благодаря этому компоненты, которые существуют на разных уровнях структуры каталогов, могут создавать ссылки на другие ресурсы во всех местах приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-179">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="6ba1e-180">Основной путь приложения также используется для перехвата щелчка гиперссылок, когда `href` целевой объект ссылки находится в пределах URI-пространства базового пути приложения&mdash;; здесь маршрутизатор Blazor отвечает за внутреннюю навигацию.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-180">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="6ba1e-181">Во многих сценариях размещения виртуальный серверный путь к приложению является корнем приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-181">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="6ba1e-182">В таких случаях базовый путь приложения является косой чертой (`<base href="/" />`), что служит конфигурацией по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-182">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="6ba1e-183">В других сценариях размещения, таких как страницы GitHub и виртуальные каталоги и дочерние приложения IIS, базовому пути приложения должно быть присвоено значение виртуального серверного пути к приложению.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-183">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="6ba1e-184">Чтобы задать базовый путь приложения, нужно добавить или обновить тег `<base>` в *index.html* среди тегов элементов `<head>`.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-184">To set the app's base path, add or update the `<base>` tag in *index.html* found within the `<head>` tag elements.</span></span> <span data-ttu-id="6ba1e-185">Задайте значение атрибута `href` как `<virtual-path>/` (косая черта в конце обязательна), где `<virtual-path>/` — полный корневой путь виртуального приложения на сервере приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-185">Set the `href` attribute value to `<virtual-path>/` (the trailing slash is required), where `<virtual-path>/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="6ba1e-186">В приведенном выше примере виртуальный путь имеет значение `CoolApp/`: `<base href="CoolApp/" />`.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-186">In the preceding example, the virtual path is set to `CoolApp/`: `<base href="CoolApp/" />`.</span></span>

<span data-ttu-id="6ba1e-187">Приложение с некорневым виртуальным путем (например, `<base href="CoolApp/" />`) не сможет найти свои ресурсы *при локальном запуске*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-187">For an app with a non-root virtual path configured (for example, `<base href="CoolApp/" />`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="6ba1e-188">Для решения этой проблемы во время локальной разработки и тестирования можно предоставить аргумент *базового пути*, который соответствует значению `href` тега `<base>` во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-188">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="6ba1e-189">Для передачи аргумента базового пути с корневым путем (`/`) при локальном запуске приложения выполните следующую команду из каталога приложения:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-189">To pass the path base argument with the root path (`/`) when running the app locally, execute the following command from the app's directory:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="6ba1e-190">Приложение отвечает локально по `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-190">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="6ba1e-191">Дополнительные сведения см. в разделе [Значение конфигурации узла для базового пути](#path-base).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-191">For more information, see the [path base host configuration value](#path-base) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6ba1e-192">Если приложение использует [модель размещения на стороне клиента](xref:razor-components/hosting-models#client-side-hosting-model) (на основе шаблона проекта **Blazor**) и размещается как дочернее приложение IIS в приложении ASP.NET Core, очень важно отключить унаследованный обработчик модулей ASP.NET Core или убедиться, что раздел `<handlers>` файла *web.config* корневого (родительского) приложения не унаследован дочерним приложением.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-192">If an app uses the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) (based on the **Blazor** project template) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>
>
> <span data-ttu-id="6ba1e-193">Удалите обработчик в опубликованном файле *web.config* приложения, добавив раздел `<handlers>` в файл:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-193">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> <span data-ttu-id="6ba1e-194">Кроме того, отключите наследование раздела `<system.webServer>` от корневого (родительского) приложения, задав для элемента `<location>` значение `inheritInChildApplications` как `false`:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-194">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> <span data-ttu-id="6ba1e-195">Удаление обработчика или отключение наследования производится в дополнение к настройке базового пути приложения, как описано в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-195">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="6ba1e-196">Задайте базовый путь приложения в файле *index.html* приложения как псевдоним IIS, используемый при настройке дочернего приложения в IIS.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-196">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a><span data-ttu-id="6ba1e-197">Размещенное на стороне клиента развертывание Blazor с использованием ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ba1e-197">Client-side Blazor hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="6ba1e-198">*Размещенное развертывание*  обслуживает клиентское приложение Blazor для браузеров из [приложения ASP.NET Core](xref:index), выполняющегося на сервере.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-198">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="6ba1e-199">Приложение Blazor входит в состав приложения ASP.NET Core в публикуемых выходных данных; таким образом, два приложения развертываются вместе.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-199">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="6ba1e-200">Веб-сервер, позволяющий размещать приложения ASP.NET Core, является обязательным.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-200">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="6ba1e-201">Для размещенного развертывания Visual Studio включает шаблон проекта **Blazor (с размещением в ASP.NET Core)** (шаблон `blazorhosted` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-201">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="6ba1e-202">Дополнительные сведения о размещении приложения ASP.NET Core и его развертывании см. в разделе <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-202">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="6ba1e-203">Сведения о развертывании в службе приложений Azure см. в разделах:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-203">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>

<span data-ttu-id="6ba1e-204">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-204">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

### <a name="client-side-blazor-standalone-deployment"></a><span data-ttu-id="6ba1e-205">Развертывание изолированного приложения Blazor на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="6ba1e-205">Client-side Blazor standalone deployment</span></span>

<span data-ttu-id="6ba1e-206">*Автономное развертывание* представляет клиентское приложение Blazor как набор статических файлов, которые будут непосредственно запрошены клиентами.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-206">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="6ba1e-207">Веб-сервер не используется для обслуживания приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-207">A web server isn't used to serve the Blazor app.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a><span data-ttu-id="6ba1e-208">Развертывание изолированного приложения Blazor с помощью IIS</span><span class="sxs-lookup"><span data-stu-id="6ba1e-208">Client-side Blazor standalone hosting with IIS</span></span>

<span data-ttu-id="6ba1e-209">IIS можно использовать как полнофункциональный статический файловый сервер для приложений Blazor.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-209">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="6ba1e-210">Чтобы настроить IIS для размещения Blazor, см. раздел [Построение статического веб-сайта на IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-210">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="6ba1e-211">Опубликованные ресурсы создаются в папке *\\bin\\Release\\&lt;целевая-платформа&gt;\\publish*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-211">Published assets are created in the *\\bin\\Release\\&lt;target-framework&gt;\\publish* folder.</span></span> <span data-ttu-id="6ba1e-212">Разместить содержимое папки *publish* на веб-сервере или в службе размещения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-212">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

<span data-ttu-id="6ba1e-213">**web.config**</span><span class="sxs-lookup"><span data-stu-id="6ba1e-213">**web.config**</span></span>

<span data-ttu-id="6ba1e-214">При публикации проекта Blazor создается файл *web.config* со следующей конфигурацией IIS:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-214">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="6ba1e-215">Типы MIME заданы для следующих расширений файлов:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-215">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="6ba1e-216">*\*.dll*: `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="6ba1e-216">*\*.dll*: `application/octet-stream`</span></span>
  * <span data-ttu-id="6ba1e-217">*\*.json*: `application/json`</span><span class="sxs-lookup"><span data-stu-id="6ba1e-217">*\*.json*: `application/json`</span></span>
  * <span data-ttu-id="6ba1e-218">*\*.wasm*: `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="6ba1e-218">*\*.wasm*: `application/wasm`</span></span>
  * <span data-ttu-id="6ba1e-219">*\*.woff*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="6ba1e-219">*\*.woff*: `application/font-woff`</span></span>
  * <span data-ttu-id="6ba1e-220">*\*.woff2*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="6ba1e-220">*\*.woff2*: `application/font-woff`</span></span>
* <span data-ttu-id="6ba1e-221">Сжатие HTTP включено для следующих типов MIME:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-221">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="6ba1e-222">Задаются правила модуля переопределения URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-222">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="6ba1e-223">Обслуживать вложенный каталог, где находятся статические ресурсы приложения (*&lt;имя-сборки&gt;\\dist\\&lt;запрошенный-путь&gt;*).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-223">Serve the sub-directory where the app's static assets reside (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span></span>
  * <span data-ttu-id="6ba1e-224">Создать резервный маршрут одностраничного приложения таким образом, что запросы, не относящиеся к файлам ресурсов, перенаправляются на документ по умолчанию приложения в папке статических ресурсов (*&lt;имя-сборки&gt;\\dist\\index.html*).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-224">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*&lt;assembly_name&gt;\\dist\\index.html*).</span></span>

<span data-ttu-id="6ba1e-225">**Установка модуля переопределения URL-адресов**</span><span class="sxs-lookup"><span data-stu-id="6ba1e-225">**Install the URL Rewrite Module**</span></span>

<span data-ttu-id="6ba1e-226">[Модуль переопределения URL-адресов](https://www.iis.net/downloads/microsoft/url-rewrite) нужен, чтобы переопределять URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-226">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="6ba1e-227">Модуль не установлен по умолчанию и будет недоступен для установки как компонент службы роли веб-сервера (IIS).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-227">The module isn't installed by default, and it isn't available for install as an Web Server (IIS) role service feature.</span></span> <span data-ttu-id="6ba1e-228">Модуль необходимо скачать с веб-сайта IIS.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-228">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="6ba1e-229">Для его установки используйте установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-229">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="6ba1e-230">Локально перейдите на [страницу скачивания модуля переопределения URL-адресов](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-230">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="6ba1e-231">В англоязычной версии выберите **WebPI** для скачивания установщика веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-231">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="6ba1e-232">Для других языков выберите подходящую архитектуру сервера (x86 или x64) для скачивания установщика.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-232">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="6ba1e-233">Скопируйте установщик на сервер.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-233">Copy the installer to the server.</span></span> <span data-ttu-id="6ba1e-234">Запустите установщик.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-234">Run the installer.</span></span> <span data-ttu-id="6ba1e-235">Нажмите кнопку **Установить** и примите условия лицензионного соглашения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-235">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="6ba1e-236">Перезагрузка сервера после завершения установки не требуется.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-236">A server restart isn't required after the install completes.</span></span>

<span data-ttu-id="6ba1e-237">**Настройте веб-сайт**</span><span class="sxs-lookup"><span data-stu-id="6ba1e-237">**Configure the website**</span></span>

<span data-ttu-id="6ba1e-238">Установите для веб сайта **физический путь** к папке приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-238">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="6ba1e-239">Папка содержит:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-239">The folder contains:</span></span>

* <span data-ttu-id="6ba1e-240">Файл *web.config*, который используется службами IIS для настройки веб-сайта, включая необходимые правила перенаправления и типы содержимого файлов.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-240">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="6ba1e-241">Папку статических ресурсов приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-241">The app's static asset folder.</span></span>

<span data-ttu-id="6ba1e-242">**Устранение неполадок**</span><span class="sxs-lookup"><span data-stu-id="6ba1e-242">**Troubleshooting**</span></span>

<span data-ttu-id="6ba1e-243">Если получена *внутренняя ошибка сервера 500* и диспетчер служб IIS порождает ошибки при попытке получить доступ к конфигурации веб сайта, убедитесь, что установлен модуль переопределения URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-243">If a *500 Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="6ba1e-244">Если модуль не установлен, IIS не может проанализировать файл *web.config*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-244">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="6ba1e-245">Это предотвращает загрузку конфигурации веб сайта диспетчером служб IIS и передачу статических файлов Blazor c веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-245">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="6ba1e-246">Дополнительные сведения об устранении неполадок развертывания в службах IIS см. в разделе <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-246">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a><span data-ttu-id="6ba1e-247">Развертывание изолированного приложения Blazor с помощью Nginx</span><span class="sxs-lookup"><span data-stu-id="6ba1e-247">Client-side Blazor standalone hosting with Nginx</span></span>

<span data-ttu-id="6ba1e-248">Следующий файл *nginx.conf* был упрощен с целью демонстрации настройки Nginx для отправки файла *Index.html* каждый раз, когда не удается найти соответствующий файл на диске.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-248">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *Index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

<span data-ttu-id="6ba1e-249">Дополнительные сведения о конфигурации веб-сервера Nginx в рабочей среде см. в разделе [Создание файлов конфигурации NGINX Plus и NGINX](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-249">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a><span data-ttu-id="6ba1e-250">Развертывание изолированного приложения Blazor с помощью Docker</span><span class="sxs-lookup"><span data-stu-id="6ba1e-250">Client-side Blazor standalone hosting with Nginx in Docker</span></span>

<span data-ttu-id="6ba1e-251">Чтобы разместить Blazor в Docker с помощью Nginx, настройте в Dockerfile образ с Nginx на основе Alpine.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-251">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="6ba1e-252">Измените Dockerfile, скопировав файл *nginx.config* в контейнер.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-252">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="6ba1e-253">Добавьте одну строку в Dockerfile, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-253">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a><span data-ttu-id="6ba1e-254">Развертывание изолированного приложения Blazor с помощью страниц GitHub</span><span class="sxs-lookup"><span data-stu-id="6ba1e-254">Client-side Blazor standalone hosting with GitHub Pages</span></span>

<span data-ttu-id="6ba1e-255">Чтобы обеспечить переопределение URL-адресов, добавьте файл *404.html* со сценарием, который производит перенаправление на страницу *index.html*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-255">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="6ba1e-256">Пример реализации, предоставленный сообществом, см. в разделе [Одностраничные приложения для страниц GitHub](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages на сайте GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-256">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="6ba1e-257">Пример с использованием подхода сообщества можно увидеть в разделе [blazor-demo/blazor-demo.github.io на сайте GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([активный сайт](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-257">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="6ba1e-258">При использовании сайта проекта вместо сайта организации следует добавить или обновить тег `<base>` в файле *index.html*.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-258">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="6ba1e-259">Задайте значение атрибута `href` как `<repository-name>/`, где `<repository-name>/` — имя репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-259">Set the `href` attribute value to `<repository-name>/`, where `<repository-name>/` is the GitHub repository name.</span></span>

## <a name="deploy-a-server-side-razor-components-app"></a><span data-ttu-id="6ba1e-260">Развертывание приложения на основе компонентов Razor на сервере</span><span class="sxs-lookup"><span data-stu-id="6ba1e-260">Deploy a server-side Razor Components app</span></span>

<span data-ttu-id="6ba1e-261">При использовании [модели размещения на сервере](xref:razor-components/hosting-models#server-side-hosting-model), компоненты Razor работают на сервере в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-261">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="6ba1e-262">Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-262">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

<span data-ttu-id="6ba1e-263">Приложение входит в состав приложения ASP.NET Core в публикуемых выходных данных; таким образом, два приложения развертываются вместе.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-263">The app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="6ba1e-264">Веб-сервер, позволяющий размещать приложения ASP.NET Core, является обязательным.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-264">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="6ba1e-265">Для развертывания на стороне сервера Visual Studio включает шаблон проекта **Razor Components** (шаблон `razorcomponents` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="6ba1e-265">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="6ba1e-266">При публикации приложения ASP.NET Core приложение на основе компонентов Razor включается в опубликованные выходные данные таким образом, чтобы приложение ASP.NET Core и приложение на основе компонентов Razor могли развертываться вместе.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-266">When the ASP.NET Core app is published, the Razor Components app is included in the published output so that the ASP.NET Core app and the Razor Components app can be deployed together.</span></span> <span data-ttu-id="6ba1e-267">Дополнительные сведения о размещении приложения ASP.NET Core и его развертывании см. в разделе <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-267">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="6ba1e-268">Сведения о развертывании в службе приложений Azure см. в разделах:</span><span class="sxs-lookup"><span data-stu-id="6ba1e-268">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>

<span data-ttu-id="6ba1e-269">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ba1e-269">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>
