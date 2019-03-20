---
title: Основы ASP.NET Core
author: rick-anderson
description: "Познакомьтесь с основными принципами создания приложений ASP.NET\_Core."
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/index
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="91fb4-103">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="91fb4-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="91fb4-104">В этой статье приведен обзор основных тем, связанных с разработкой приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="91fb4-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="91fb4-105">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="91fb4-105">The Startup class</span></span>

<span data-ttu-id="91fb4-106">В классе `Startup` делается следующее.</span><span class="sxs-lookup"><span data-stu-id="91fb4-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="91fb4-107">Настраиваются все службы, необходимые приложению.</span><span class="sxs-lookup"><span data-stu-id="91fb4-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="91fb4-108">Определяется конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="91fb4-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="91fb4-109">Код для настройки (или *регистрации*) служб добавляется в метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="91fb4-110">*Службы* — это компоненты, которые используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="91fb4-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="91fb4-111">Например, объект контекста Entity Framework Core является службой.</span><span class="sxs-lookup"><span data-stu-id="91fb4-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="91fb4-112">Код для настройки конвейера обработки запросов добавляется в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="91fb4-113">Конвейер состоит из ряда компонентов *ПО промежуточного слоя*.</span><span class="sxs-lookup"><span data-stu-id="91fb4-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="91fb4-114">Например, это ПО может обрабатывать запросы для статических файлов или перенаправлять HTTP-запросы в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91fb4-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="91fb4-115">Каждый компонент ПО промежуточного слоя выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="91fb4-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="91fb4-116">Пример класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="91fb4-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

::: moniker-end

<span data-ttu-id="91fb4-117">Дополнительные сведения: [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="91fb4-117">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="91fb4-118">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="91fb4-118">Dependency injection (services)</span></span>

<span data-ttu-id="91fb4-119">ASP.NET Core включает встроенную платформу внедрения зависимостей, позволяющую классам приложения обращаться к настроенным службам.</span><span class="sxs-lookup"><span data-stu-id="91fb4-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="91fb4-120">Одним из вариантов получения экземпляра службы в классе является создание конструктора с параметром требуемого типа.</span><span class="sxs-lookup"><span data-stu-id="91fb4-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="91fb4-121">Параметр может быть типом службы или интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="91fb4-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="91fb4-122">Система внедрения зависимостей предоставляет службу во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="91fb4-122">The DI system provides the service at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="91fb4-123">Ниже показан класс, который использует внедрение зависимостей для получения объекта контекста Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="91fb4-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="91fb4-124">Выделенная строка является примером внедрения через конструктор.</span><span class="sxs-lookup"><span data-stu-id="91fb4-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

::: moniker-end

<span data-ttu-id="91fb4-125">Несмотря на то, что система внедрения зависимостей является встроенной, она позволяет вам при желании подключать сторонний контейнер с инверсией управления (IoC).</span><span class="sxs-lookup"><span data-stu-id="91fb4-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="91fb4-126">Дополнительные сведения см. в разделе [Введение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="91fb4-126">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="91fb4-127">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="91fb4-127">Middleware</span></span>

<span data-ttu-id="91fb4-128">Конвейер обработки запросов состоит из ряда компонентов ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="91fb4-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="91fb4-129">Каждый компонент выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="91fb4-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="91fb4-130">По принятому соглашению компонент ПО промежуточного слоя добавляется в конвейер вызовом метода расширения `Use...` в методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="91fb4-131">Например, чтобы включить отрисовку статических файлов, вызовите `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="91fb4-132">Код настройки конвейера обработки запросов выделен в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="91fb4-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

::: moniker-end

<span data-ttu-id="91fb4-133">ASP.NET Core включает обширный набор встроенного ПО промежуточного слоя и позволяет вам писать свое собственное.</span><span class="sxs-lookup"><span data-stu-id="91fb4-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="91fb4-134">Дополнительные сведения: [ПО промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="91fb4-134">For more information, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="91fb4-135">Узел</span><span class="sxs-lookup"><span data-stu-id="91fb4-135">The host</span></span>

<span data-ttu-id="91fb4-136">Приложение ASP.NET Core создает *узел* при запуске.</span><span class="sxs-lookup"><span data-stu-id="91fb4-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="91fb4-137">Узел — это объект, который инкапсулирует все ресурсы приложения.</span><span class="sxs-lookup"><span data-stu-id="91fb4-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="91fb4-138">Реализация HTTP-сервера</span><span class="sxs-lookup"><span data-stu-id="91fb4-138">An HTTP server implementation</span></span>
* <span data-ttu-id="91fb4-139">Компоненты ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="91fb4-139">Middleware components</span></span>
* <span data-ttu-id="91fb4-140">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="91fb4-140">Logging</span></span>
* <span data-ttu-id="91fb4-141">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="91fb4-141">DI</span></span>
* <span data-ttu-id="91fb4-142">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="91fb4-142">Configuration</span></span>

<span data-ttu-id="91fb4-143">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="91fb4-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="91fb4-144">Код для создания узла находится в `Program.Main` и следует [шаблону построителя](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="91fb4-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="91fb4-145">Для настройки каждого ресурса, входящего в узел, вызываются соответствующие методы.</span><span class="sxs-lookup"><span data-stu-id="91fb4-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="91fb4-146">Чтобы свести все компоненты воедино и создать экземпляр объекта узла, вызывается метод построителя.</span><span class="sxs-lookup"><span data-stu-id="91fb4-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="91fb4-147">ASP.NET Core 2.x использует веб-узел (класс `WebHost`) для веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="91fb4-147">ASP.NET Core 2.x uses Web Host (the `WebHost` class) for web apps.</span></span> <span data-ttu-id="91fb4-148">Платформа предоставляет метод `CreateDefaultBuilder` для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="91fb4-148">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="91fb4-149">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="91fb4-149">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="91fb4-150">Загрузка конфигурации из файла *appsettings.json*, переменных среды, аргументов командной строки и других источников.</span><span class="sxs-lookup"><span data-stu-id="91fb4-150">Load configuration from *appsettings.json*, environment variables, command line arguments, and other sources.</span></span>
* <span data-ttu-id="91fb4-151">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="91fb4-151">Send logging output to the console and debug providers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="91fb4-152">Пример кода для создания узла:</span><span class="sxs-lookup"><span data-stu-id="91fb4-152">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="91fb4-153">Дополнительные сведения: [Веб-узел](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="91fb4-153">For more information, see [Web Host](xref:fundamentals/host/web-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="91fb4-154">В веб-приложении ASP.NET Core 3.0 можно использовать веб-узел (класс `WebHost`) или универсальный узел (класс `Host`).</span><span class="sxs-lookup"><span data-stu-id="91fb4-154">In ASP.NET Core 3.0, Web Host (`WebHost` class) or Generic Host (`Host` class) can be used in a web app.</span></span> <span data-ttu-id="91fb4-155">Универсальный узел является рекомендуемым вариантом, а веб-узел служит для обеспечения обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="91fb4-155">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="91fb4-156">Платформа предоставляет методы `CreateDefaultBuilder` и `ConfigureWebHostDefaults` для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="91fb4-156">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="91fb4-157">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="91fb4-157">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="91fb4-158">Загрузка конфигурации из файлов *appsettings.json* и *appsettings.[имя_среды].json*, переменных среды и аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="91fb4-158">Load configuration from *appsettings.json*, *appsettings.[EnvironmentName].json*, environment variables, and command line arguments.</span></span>
* <span data-ttu-id="91fb4-159">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="91fb4-159">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="91fb4-160">Ниже приведен пример кода для создания узла.</span><span class="sxs-lookup"><span data-stu-id="91fb4-160">Here's sample code that builds a host.</span></span> <span data-ttu-id="91fb4-161">В нем выделены методы для настройки узла с часто используемыми параметрами.</span><span class="sxs-lookup"><span data-stu-id="91fb4-161">The methods that set up the host with commonly used options are highlighted.</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="91fb4-162">Дополнительные сведения: [Универсальный узел](xref:fundamentals/host/generic-host), [Веб-узел](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="91fb4-162">For more information, see [Generic Host](xref:fundamentals/host/generic-host) and [Web Host](xref:fundamentals/host/web-host)</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="91fb4-163">Сложные сценарии использования узла</span><span class="sxs-lookup"><span data-stu-id="91fb4-163">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="91fb4-164">Веб-узел должен содержать реализацию HTTP-сервера, которая не требуется для других типов приложений .NET.</span><span class="sxs-lookup"><span data-stu-id="91fb4-164">Web Host is designed to include an HTTP server implementation, which isn't needed for other kinds of .NET apps.</span></span> <span data-ttu-id="91fb4-165">Начиная с версии 2.1 универсальный узел (класс `Host`) доступен для любых приложений .NET Core &mdash; не только для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="91fb4-165">Starting in 2.1, Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="91fb4-166">Он позволяет использовать перекрестные функции, такие как ведение журнала, внедрение зависимостей, конфигурации и управление жизненным циклом, в других типах приложений.</span><span class="sxs-lookup"><span data-stu-id="91fb4-166">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="91fb4-167">Дополнительные сведения: [Универсальный узел](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="91fb4-167">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="91fb4-168">Универсальный узел доступен для любых приложений .NET Core &mdash; не только для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="91fb4-168">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="91fb4-169">Он позволяет использовать перекрестные функции, такие как ведение журнала, внедрение зависимостей, конфигурации и управление жизненным циклом, в других типах приложений.</span><span class="sxs-lookup"><span data-stu-id="91fb4-169">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="91fb4-170">Дополнительные сведения: [Универсальный узел](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="91fb4-170">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

<span data-ttu-id="91fb4-171">Узел можно также использовать для выполнения фоновых задач.</span><span class="sxs-lookup"><span data-stu-id="91fb4-171">You can also use the host to run background tasks.</span></span> <span data-ttu-id="91fb4-172">Дополнительные сведения: [Фоновые задачи](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="91fb4-172">For more information, see [Background tasks](xref:fundamentals/host/hosted-services).</span></span>

## <a name="servers"></a><span data-ttu-id="91fb4-173">Серверы</span><span class="sxs-lookup"><span data-stu-id="91fb4-173">Servers</span></span>

<span data-ttu-id="91fb4-174">Приложение ASP.NET Core использует реализацию HTTP-сервера для приема HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="91fb4-174">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="91fb4-175">Сервер отправляет приложению запросы в виде набора [функций запросов](xref:fundamentals/request-features), объединенных в `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-175">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="91fb4-176">Windows</span><span class="sxs-lookup"><span data-stu-id="91fb4-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="91fb4-177">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="91fb4-177">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="91fb4-178">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="91fb4-178">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="91fb4-179">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="91fb4-179">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="91fb4-180">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="91fb4-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="91fb4-181">*HTTP-сервер IIS* — это сервер для Windows, который использует службы IIS.</span><span class="sxs-lookup"><span data-stu-id="91fb4-181">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="91fb4-182">Он позволяет запускать приложение ASP.NET Core и службы IIS в одном процессе.</span><span class="sxs-lookup"><span data-stu-id="91fb4-182">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="91fb4-183">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="91fb4-183">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="91fb4-184">macOS</span><span class="sxs-lookup"><span data-stu-id="91fb4-184">macOS</span></span>](#tab/macos)

<span data-ttu-id="91fb4-185">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="91fb4-185">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="91fb4-186">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="91fb4-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="91fb4-187">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="91fb4-187">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="91fb4-188">Linux</span><span class="sxs-lookup"><span data-stu-id="91fb4-188">Linux</span></span>](#tab/linux)

<span data-ttu-id="91fb4-189">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="91fb4-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="91fb4-190">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="91fb4-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="91fb4-191">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="91fb4-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="91fb4-192">Windows</span><span class="sxs-lookup"><span data-stu-id="91fb4-192">Windows</span></span>](#tab/windows)

<span data-ttu-id="91fb4-193">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="91fb4-193">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="91fb4-194">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="91fb4-194">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="91fb4-195">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="91fb4-195">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="91fb4-196">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="91fb4-196">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="91fb4-197">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="91fb4-197">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="91fb4-198">macOS</span><span class="sxs-lookup"><span data-stu-id="91fb4-198">macOS</span></span>](#tab/macos)

<span data-ttu-id="91fb4-199">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="91fb4-199">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="91fb4-200">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="91fb4-200">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="91fb4-201">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="91fb4-201">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="91fb4-202">Linux</span><span class="sxs-lookup"><span data-stu-id="91fb4-202">Linux</span></span>](#tab/linux)

<span data-ttu-id="91fb4-203">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="91fb4-203">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="91fb4-204">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="91fb4-204">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="91fb4-205">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="91fb4-205">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="91fb4-206">Дополнительные сведения: [Серверы](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="91fb4-206">For more information, see [Servers](xref:fundamentals/servers/index).</span></span>

## <a name="configuration"></a><span data-ttu-id="91fb4-207">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="91fb4-207">Configuration</span></span>

<span data-ttu-id="91fb4-208">ASP.NET Core предоставляет платформу конфигурации, которая получает параметры в виде пар "имя-значение" от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="91fb4-208">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="91fb4-209">Доступны встроенные поставщики конфигурации для различных источников, таких как файлы *JSON*, *XML*, переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="91fb4-209">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="91fb4-210">Кроме того, вы можете писать поставщики конфигурации сами.</span><span class="sxs-lookup"><span data-stu-id="91fb4-210">You can also write custom configuration providers.</span></span>

<span data-ttu-id="91fb4-211">Например, можно указать, что источником конфигурации являются файл *appsettings.json* и переменные среды.</span><span class="sxs-lookup"><span data-stu-id="91fb4-211">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="91fb4-212">В этом случае при запросе значения *ConnectionString* платформа сначала выполнит поиск в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="91fb4-212">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="91fb4-213">Если значение будет найдено не только в нем, но и в переменной среды, приоритет получит значение из переменной среды.</span><span class="sxs-lookup"><span data-stu-id="91fb4-213">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="91fb4-214">Для управления конфиденциальными данными конфигурации, например паролями, ASP.NET Core предоставляет специальный инструмент [Менеджер секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="91fb4-214">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="91fb4-215">Для секретов в рабочей среде рекомендуется использовать [Azure Key Vault](/aspnet/core/security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="91fb4-215">For production secrets, we recommend [Azure Key Vault](/aspnet/core/security/key-vault-configuration).</span></span>

<span data-ttu-id="91fb4-216">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="91fb4-216">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="options"></a><span data-ttu-id="91fb4-217">Параметры</span><span class="sxs-lookup"><span data-stu-id="91fb4-217">Options</span></span>

<span data-ttu-id="91fb4-218">Когда это возможно, ASP.NET Core следует *шаблону параметров* для хранения и получения значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="91fb4-218">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="91fb4-219">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="91fb4-219">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="91fb4-220">Например, следующий код задает параметры WebSockets:</span><span class="sxs-lookup"><span data-stu-id="91fb4-220">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="91fb4-221">Дополнительные сведения: [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="91fb4-221">For more information, see [Options](xref:fundamentals/configuration/options).</span></span>

## <a name="environments"></a><span data-ttu-id="91fb4-222">Среды</span><span class="sxs-lookup"><span data-stu-id="91fb4-222">Environments</span></span>

<span data-ttu-id="91fb4-223">Среды выполнения, включая среду *разработки*, *промежуточную* и *рабочую* среды, являются ключевыми компонентами ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="91fb4-223">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="91fb4-224">Указать среду, в которой запускается приложение, можно с помощью переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-224">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="91fb4-225">ASP.NET Core считывает переменную среды при запуске приложения и сохраняет ее значение в реализации `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-225">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="91fb4-226">Объект среды можно получить в любом месте приложения при помощи внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="91fb4-226">The environment object is available anywhere in the app via DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="91fb4-227">В следующем примере кода из класса `Startup` показана настройка приложения для вывода подробных сведений об ошибках исключительно при запуске в среде разработки:</span><span class="sxs-lookup"><span data-stu-id="91fb4-227">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

::: moniker-end

<span data-ttu-id="91fb4-228">Дополнительные сведения: [Среды](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="91fb4-228">For more information, see [Environments](xref:fundamentals/environments).</span></span>

## <a name="logging"></a><span data-ttu-id="91fb4-229">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="91fb4-229">Logging</span></span>

<span data-ttu-id="91fb4-230">ASP.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками.</span><span class="sxs-lookup"><span data-stu-id="91fb4-230">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="91fb4-231">Доступны следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="91fb4-231">Available providers include the following:</span></span>

* <span data-ttu-id="91fb4-232">Консоль</span><span class="sxs-lookup"><span data-stu-id="91fb4-232">Console</span></span>
* <span data-ttu-id="91fb4-233">Отладка</span><span class="sxs-lookup"><span data-stu-id="91fb4-233">Debug</span></span>
* <span data-ttu-id="91fb4-234">Трассировка событий Windows</span><span class="sxs-lookup"><span data-stu-id="91fb4-234">Event Tracing on Windows</span></span>
* <span data-ttu-id="91fb4-235">Журнал событий Windows</span><span class="sxs-lookup"><span data-stu-id="91fb4-235">Windows Event Log</span></span>
* <span data-ttu-id="91fb4-236">TraceSource</span><span class="sxs-lookup"><span data-stu-id="91fb4-236">TraceSource</span></span>
* <span data-ttu-id="91fb4-237">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="91fb4-237">Azure App Service</span></span>
* <span data-ttu-id="91fb4-238">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="91fb4-238">Azure Application Insights</span></span>

<span data-ttu-id="91fb4-239">Записи в журналы можно вносить в любом месте кода приложения. Для этого нужно получить объект `ILogger` из внедрения зависимостей и вызвать методы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="91fb4-239">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="91fb4-240">Ниже приведен пример кода, где используется объект `ILogger`. В коде выделены строки внедрения конструктора и вызова методов ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="91fb4-240">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

::: moniker-end

<span data-ttu-id="91fb4-241">Интерфейс `ILogger` позволяет передавать в поставщик ведения журналов любое количество полей.</span><span class="sxs-lookup"><span data-stu-id="91fb4-241">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="91fb4-242">Как правило, поля используются для создания строки сообщения, но поставщик также может отправлять их в хранилище данных в виде отдельных полей.</span><span class="sxs-lookup"><span data-stu-id="91fb4-242">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="91fb4-243">Благодаря этому поставщики могут реализовывать [семантическое (структурированное) ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="91fb4-243">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="91fb4-244">Дополнительные сведения: [Ведение журналов](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="91fb4-244">For more information, see [Logging](xref:fundamentals/logging/index).</span></span>

## <a name="routing"></a><span data-ttu-id="91fb4-245">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="91fb4-245">Routing</span></span>

<span data-ttu-id="91fb4-246">*Маршрут* — это шаблон URL-адреса, сопоставляемый с обработчиком.</span><span class="sxs-lookup"><span data-stu-id="91fb4-246">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="91fb4-247">Обычно обработчик представляет собой страницу Razor, метод действия в контроллере MVC или ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="91fb4-247">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="91fb4-248">Маршрутизация ASP.NET Core позволяет контролировать URL-адреса, используемые приложением.</span><span class="sxs-lookup"><span data-stu-id="91fb4-248">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="91fb4-249">Дополнительные сведения см. в разделе [Маршрутизация](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="91fb4-249">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="error-handling"></a><span data-ttu-id="91fb4-250">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="91fb4-250">Error handling</span></span>

<span data-ttu-id="91fb4-251">ASP.NET Core содержит встроенные функции обработки ошибок, такие как:</span><span class="sxs-lookup"><span data-stu-id="91fb4-251">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="91fb4-252">Страница исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="91fb4-252">A developer exception page</span></span>
* <span data-ttu-id="91fb4-253">Настраиваемые страницы ошибок</span><span class="sxs-lookup"><span data-stu-id="91fb4-253">Custom error pages</span></span>
* <span data-ttu-id="91fb4-254">Статические страницы с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="91fb4-254">Static status code pages</span></span>
* <span data-ttu-id="91fb4-255">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="91fb4-255">Startup exception handling</span></span>

<span data-ttu-id="91fb4-256">Дополнительные сведения: [Обработка ошибок](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="91fb4-256">For more information, see [Error handling](xref:fundamentals/error-handling).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="make-http-requests"></a><span data-ttu-id="91fb4-257">Создание HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="91fb4-257">Make HTTP requests</span></span>

<span data-ttu-id="91fb4-258">ASP.NET Core включает реализацию интерфейса `IHttpClientFactory` для создания экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-258">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="91fb4-259">Фабрика:</span><span class="sxs-lookup"><span data-stu-id="91fb4-259">The factory:</span></span>

* <span data-ttu-id="91fb4-260">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-260">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="91fb4-261">Например, можно зарегистрировать и использовать клиент *github* для доступа к GitHub.</span><span class="sxs-lookup"><span data-stu-id="91fb4-261">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="91fb4-262">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="91fb4-262">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="91fb4-263">Поддержка регистрации и связывания в цепочки множества делегирующих обработчиков для создания конвейера ПО промежуточного слоя под исходящие запросы.</span><span class="sxs-lookup"><span data-stu-id="91fb4-263">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="91fb4-264">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="91fb4-264">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="91fb4-265">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="91fb4-265">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="91fb4-266">Интеграция с *Polly* — популярной сторонней библиотекой для обработки временных сбоев.</span><span class="sxs-lookup"><span data-stu-id="91fb4-266">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="91fb4-267">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="91fb4-267">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="91fb4-268">Настройка параметров ведения журнала (с помощью *ILogger*) для всех запросов, отправляемых через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="91fb4-268">Adds a configurable logging experience (via *ILogger*) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="91fb4-269">Дополнительные сведения: [HTTP-запросы](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="91fb4-269">For more information, see [Make HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="content-root"></a><span data-ttu-id="91fb4-270">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="91fb4-270">Content root</span></span>

<span data-ttu-id="91fb4-271">Корень содержимого — это базовый путь к любому закрытому содержимому, например к файлам Razor, которое используется приложением.</span><span class="sxs-lookup"><span data-stu-id="91fb4-271">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="91fb4-272">По умолчанию корень содержимого является базовым путем исполняемого файла приложения.</span><span class="sxs-lookup"><span data-stu-id="91fb4-272">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="91fb4-273">Альтернативное расположение можно указать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="91fb4-273">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="91fb4-274">Дополнительные сведения: [Корень содержимого](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="91fb4-274">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="91fb4-275">Дополнительные сведения: [Корень содержимого](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="91fb4-275">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="91fb4-276">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="91fb4-276">Web root</span></span>

<span data-ttu-id="91fb4-277">Корневой каталог документов (*webroot*) — это базовый путь к общедоступным статическим ресурсам, включая CSS, JavaScript и файлы изображений.</span><span class="sxs-lookup"><span data-stu-id="91fb4-277">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="91fb4-278">По умолчанию ПО промежуточного слоя статических файлов обслуживает только файлы из корневого каталога документов и его подкаталогов.</span><span class="sxs-lookup"><span data-stu-id="91fb4-278">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="91fb4-279">По умолчанию используется путь *\<корневой каталог содержимого>/wwwroot*. Альтернативное расположение можно задать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="91fb4-279">The web root path defaults to *\<content root>/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="91fb4-280">Для указания на корневой каталог файлов Razor (*CSHTML*) используется символ тильды и косой черты `~/`.</span><span class="sxs-lookup"><span data-stu-id="91fb4-280">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="91fb4-281">Пути, начинающиеся с `~/`, называются виртуальными путями.</span><span class="sxs-lookup"><span data-stu-id="91fb4-281">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="91fb4-282">Дополнительные сведения см. в разделе [Статические файлы](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="91fb4-282">For more information, see [Static files](xref:fundamentals/static-files).</span></span>
