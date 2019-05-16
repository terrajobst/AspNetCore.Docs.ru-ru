---
title: Основы ASP.NET Core
author: rick-anderson
description: Познакомьтесь с основными принципами создания приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: 9c7bc25d813ad17825ef03f5176882993cc2dd63
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610329"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="ac72a-103">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac72a-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="ac72a-104">В этой статье приведен обзор основных тем, связанных с разработкой приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac72a-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="ac72a-105">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="ac72a-105">The Startup class</span></span>

<span data-ttu-id="ac72a-106">В классе `Startup` делается следующее.</span><span class="sxs-lookup"><span data-stu-id="ac72a-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="ac72a-107">Настраиваются все службы, необходимые приложению.</span><span class="sxs-lookup"><span data-stu-id="ac72a-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="ac72a-108">Определяется конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="ac72a-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="ac72a-109">Код для настройки (или *регистрации*) служб добавляется в метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ac72a-110">*Службы* — это компоненты, которые используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="ac72a-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="ac72a-111">Например, объект контекста Entity Framework Core является службой.</span><span class="sxs-lookup"><span data-stu-id="ac72a-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="ac72a-112">Код для настройки конвейера обработки запросов добавляется в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="ac72a-113">Конвейер состоит из ряда компонентов *ПО промежуточного слоя*.</span><span class="sxs-lookup"><span data-stu-id="ac72a-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="ac72a-114">Например, это ПО может обрабатывать запросы для статических файлов или перенаправлять HTTP-запросы в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ac72a-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="ac72a-115">Каждый компонент ПО промежуточного слоя выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="ac72a-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="ac72a-116">Пример класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="ac72a-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="ac72a-117">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="ac72a-118">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="ac72a-118">Dependency injection (services)</span></span>

<span data-ttu-id="ac72a-119">ASP.NET Core включает встроенную платформу внедрения зависимостей, позволяющую классам приложения обращаться к настроенным службам.</span><span class="sxs-lookup"><span data-stu-id="ac72a-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="ac72a-120">Одним из вариантов получения экземпляра службы в классе является создание конструктора с параметром требуемого типа.</span><span class="sxs-lookup"><span data-stu-id="ac72a-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="ac72a-121">Параметр может быть типом службы или интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="ac72a-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="ac72a-122">Система внедрения зависимостей предоставляет службу во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="ac72a-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="ac72a-123">Ниже показан класс, который использует внедрение зависимостей для получения объекта контекста Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ac72a-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="ac72a-124">Выделенная строка является примером внедрения через конструктор.</span><span class="sxs-lookup"><span data-stu-id="ac72a-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="ac72a-125">Несмотря на то, что система внедрения зависимостей является встроенной, она позволяет вам при желании подключать сторонний контейнер с инверсией управления (IoC).</span><span class="sxs-lookup"><span data-stu-id="ac72a-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="ac72a-126">Для получения дополнительной информации см. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="ac72a-127">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="ac72a-127">Middleware</span></span>

<span data-ttu-id="ac72a-128">Конвейер обработки запросов состоит из ряда компонентов ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="ac72a-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="ac72a-129">Каждый компонент выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="ac72a-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="ac72a-130">По принятому соглашению компонент ПО промежуточного слоя добавляется в конвейер вызовом метода расширения `Use...` в методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="ac72a-131">Например, чтобы включить отрисовку статических файлов, вызовите `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="ac72a-132">Код настройки конвейера обработки запросов выделен в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="ac72a-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="ac72a-133">ASP.NET Core включает обширный набор встроенного ПО промежуточного слоя и позволяет вам писать свое собственное.</span><span class="sxs-lookup"><span data-stu-id="ac72a-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="ac72a-134">Для получения дополнительной информации см. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="ac72a-135">Узел</span><span class="sxs-lookup"><span data-stu-id="ac72a-135">The host</span></span>

<span data-ttu-id="ac72a-136">Приложение ASP.NET Core создает *узел* при запуске.</span><span class="sxs-lookup"><span data-stu-id="ac72a-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="ac72a-137">Узел — это объект, который инкапсулирует все ресурсы приложения.</span><span class="sxs-lookup"><span data-stu-id="ac72a-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="ac72a-138">Реализация HTTP-сервера</span><span class="sxs-lookup"><span data-stu-id="ac72a-138">An HTTP server implementation</span></span>
* <span data-ttu-id="ac72a-139">Компоненты ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="ac72a-139">Middleware components</span></span>
* <span data-ttu-id="ac72a-140">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="ac72a-140">Logging</span></span>
* <span data-ttu-id="ac72a-141">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="ac72a-141">DI</span></span>
* <span data-ttu-id="ac72a-142">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="ac72a-142">Configuration</span></span>

<span data-ttu-id="ac72a-143">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="ac72a-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="ac72a-144">Код для создания узла находится в `Program.Main` и следует [шаблону построителя](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="ac72a-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="ac72a-145">Для настройки каждого ресурса, входящего в узел, вызываются соответствующие методы.</span><span class="sxs-lookup"><span data-stu-id="ac72a-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="ac72a-146">Чтобы свести все компоненты воедино и создать экземпляр объекта узла, вызывается метод построителя.</span><span class="sxs-lookup"><span data-stu-id="ac72a-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac72a-147">`CreateHostBuilder` — это специальное имя, которое определяет метод построителя для внешних компонентов, таких как [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="ac72a-147">`CreateHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="ac72a-148">В веб-приложении ASP.NET Core 3.0 или более поздней версии можно использовать универсальный узел (класс `Host`) или веб-узел (класс `WebHost`).</span><span class="sxs-lookup"><span data-stu-id="ac72a-148">In ASP.NET Core 3.0 or later, Generic Host (`Host` class) or Web Host (`WebHost` class) can be used in a web app.</span></span> <span data-ttu-id="ac72a-149">Универсальный узел является рекомендуемым вариантом, а веб-узел служит для обеспечения обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="ac72a-149">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="ac72a-150">Платформа предоставляет методы `CreateDefaultBuilder` и `ConfigureWebHostDefaults` для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="ac72a-150">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="ac72a-151">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="ac72a-151">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="ac72a-152">Загрузка конфигурации из файлов *appsettings.json* и *appsettings.{имя среды}.json*, переменных среды, аргументов командной строки и других источников.</span><span class="sxs-lookup"><span data-stu-id="ac72a-152">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="ac72a-153">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="ac72a-153">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="ac72a-154">Ниже приведен пример кода для создания узла.</span><span class="sxs-lookup"><span data-stu-id="ac72a-154">Here's sample code that builds a host.</span></span> <span data-ttu-id="ac72a-155">В нем выделены методы для настройки узла с часто используемыми параметрами:</span><span class="sxs-lookup"><span data-stu-id="ac72a-155">The methods that set up the host with commonly used options are highlighted:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="ac72a-156">Дополнительные сведения см. в разделах <xref:fundamentals/host/generic-host> и <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-156">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac72a-157">`CreateWebHostBuilder` — это специальное имя, которое определяет метод построителя для внешних компонентов, таких как [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="ac72a-157">`CreateWebHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="ac72a-158">ASP.NET Core 2.x использует веб-узел (класс `WebHost`) для веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="ac72a-158">ASP.NET Core 2.x uses Web Host (`WebHost` class) for web apps.</span></span> <span data-ttu-id="ac72a-159">Платформа предоставляет метод `CreateDefaultBuilder` для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="ac72a-159">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="ac72a-160">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="ac72a-160">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="ac72a-161">Загрузка конфигурации из файлов *appsettings.json* и *appsettings.{имя среды}.json*, переменных среды, аргументов командной строки и других источников.</span><span class="sxs-lookup"><span data-stu-id="ac72a-161">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="ac72a-162">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="ac72a-162">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="ac72a-163">Пример кода для создания узла:</span><span class="sxs-lookup"><span data-stu-id="ac72a-163">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="ac72a-164">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="ac72a-165">Сложные сценарии использования узла</span><span class="sxs-lookup"><span data-stu-id="ac72a-165">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac72a-166">Универсальный узел доступен для любых приложений .NET Core &mdash; не только для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac72a-166">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="ac72a-167">Универсальный узел (класс `Host`) позволяет приложениям других типов использовать перекрестные расширения платформ, такие как средства ведения журналов, внедрения зависимостей, настройки и управления жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="ac72a-167">Generic Host (`Host` class) allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="ac72a-168">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac72a-169">Веб-узел должен содержать реализацию HTTP-сервера, которая не требуется для приложений .NET других типов.</span><span class="sxs-lookup"><span data-stu-id="ac72a-169">Web Host is designed to include an HTTP server implementation, which isn't required for other kinds of .NET apps.</span></span> <span data-ttu-id="ac72a-170">Начиная с версии ASP.NET Core 2.1 универсальный узел (класс `Host`) доступен для любых приложений .NET Core &mdash; не только для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac72a-170">Starting in ASP.NET Core 2.1, the Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="ac72a-171">Универсальный узел позволяет приложениям других типов использовать перекрестные расширения платформ, такие как средства ведения журналов, внедрения зависимостей, настройки и управления жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="ac72a-171">Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="ac72a-172">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-172">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

<span data-ttu-id="ac72a-173">Узел можно также использовать для выполнения фоновых задач.</span><span class="sxs-lookup"><span data-stu-id="ac72a-173">You can also use the host to run background tasks.</span></span> <span data-ttu-id="ac72a-174">Для получения дополнительной информации см. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-174">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="ac72a-175">Серверы</span><span class="sxs-lookup"><span data-stu-id="ac72a-175">Servers</span></span>

<span data-ttu-id="ac72a-176">Приложение ASP.NET Core использует реализацию HTTP-сервера для приема HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="ac72a-176">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="ac72a-177">Сервер отправляет приложению запросы в виде набора [функций запросов](xref:fundamentals/request-features), объединенных в `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-177">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="ac72a-178">Windows</span><span class="sxs-lookup"><span data-stu-id="ac72a-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="ac72a-179">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="ac72a-179">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="ac72a-180">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="ac72a-180">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="ac72a-181">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ac72a-181">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="ac72a-182">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="ac72a-182">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="ac72a-183">*HTTP-сервер IIS* — это сервер для Windows, который использует службы IIS.</span><span class="sxs-lookup"><span data-stu-id="ac72a-183">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="ac72a-184">Он позволяет запускать приложение ASP.NET Core и службы IIS в одном процессе.</span><span class="sxs-lookup"><span data-stu-id="ac72a-184">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="ac72a-185">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="ac72a-185">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ac72a-186">macOS</span><span class="sxs-lookup"><span data-stu-id="ac72a-186">macOS</span></span>](#tab/macos)

<span data-ttu-id="ac72a-187">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="ac72a-187">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="ac72a-188">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="ac72a-188">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="ac72a-189">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ac72a-189">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ac72a-190">Linux</span><span class="sxs-lookup"><span data-stu-id="ac72a-190">Linux</span></span>](#tab/linux)

<span data-ttu-id="ac72a-191">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="ac72a-191">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="ac72a-192">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="ac72a-192">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="ac72a-193">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ac72a-193">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="ac72a-194">Windows</span><span class="sxs-lookup"><span data-stu-id="ac72a-194">Windows</span></span>](#tab/windows)

<span data-ttu-id="ac72a-195">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="ac72a-195">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="ac72a-196">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="ac72a-196">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="ac72a-197">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ac72a-197">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="ac72a-198">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="ac72a-198">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="ac72a-199">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="ac72a-199">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ac72a-200">macOS</span><span class="sxs-lookup"><span data-stu-id="ac72a-200">macOS</span></span>](#tab/macos)

<span data-ttu-id="ac72a-201">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="ac72a-201">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="ac72a-202">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="ac72a-202">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="ac72a-203">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ac72a-203">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ac72a-204">Linux</span><span class="sxs-lookup"><span data-stu-id="ac72a-204">Linux</span></span>](#tab/linux)

<span data-ttu-id="ac72a-205">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="ac72a-205">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="ac72a-206">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="ac72a-206">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="ac72a-207">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ac72a-207">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="ac72a-208">Для получения дополнительной информации см. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-208">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="ac72a-209">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="ac72a-209">Configuration</span></span>

<span data-ttu-id="ac72a-210">ASP.NET Core предоставляет платформу конфигурации, которая получает параметры в виде пар "имя-значение" от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ac72a-210">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="ac72a-211">Доступны встроенные поставщики конфигурации для различных источников, таких как файлы *JSON*, *XML*, переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="ac72a-211">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="ac72a-212">Кроме того, вы можете писать поставщики конфигурации сами.</span><span class="sxs-lookup"><span data-stu-id="ac72a-212">You can also write custom configuration providers.</span></span>

<span data-ttu-id="ac72a-213">Например, можно указать, что источником конфигурации являются файл *appsettings.json* и переменные среды.</span><span class="sxs-lookup"><span data-stu-id="ac72a-213">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="ac72a-214">В этом случае при запросе значения *ConnectionString* платформа сначала выполнит поиск в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ac72a-214">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="ac72a-215">Если значение будет найдено не только в нем, но и в переменной среды, приоритет получит значение из переменной среды.</span><span class="sxs-lookup"><span data-stu-id="ac72a-215">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="ac72a-216">Для управления конфиденциальными данными конфигурации, например паролями, ASP.NET Core предоставляет специальный инструмент [Менеджер секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="ac72a-216">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="ac72a-217">Для секретов в рабочей среде рекомендуется использовать [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="ac72a-217">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="ac72a-218">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-218">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="ac72a-219">Параметры</span><span class="sxs-lookup"><span data-stu-id="ac72a-219">Options</span></span>

<span data-ttu-id="ac72a-220">Когда это возможно, ASP.NET Core следует *шаблону параметров* для хранения и получения значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ac72a-220">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="ac72a-221">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="ac72a-221">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="ac72a-222">Например, следующий код задает параметры WebSockets:</span><span class="sxs-lookup"><span data-stu-id="ac72a-222">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="ac72a-223">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-223">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="ac72a-224">Среды</span><span class="sxs-lookup"><span data-stu-id="ac72a-224">Environments</span></span>

<span data-ttu-id="ac72a-225">Среды выполнения, включая среду *разработки*, *промежуточную* и *рабочую* среды, являются ключевыми компонентами ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac72a-225">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="ac72a-226">Указать среду, в которой запускается приложение, можно с помощью переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-226">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="ac72a-227">ASP.NET Core считывает переменную среды при запуске приложения и сохраняет ее значение в реализации `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-227">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="ac72a-228">Объект среды можно получить в любом месте приложения при помощи внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="ac72a-228">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="ac72a-229">В следующем примере кода из класса `Startup` показана настройка приложения для вывода подробных сведений об ошибках исключительно при запуске в среде разработки:</span><span class="sxs-lookup"><span data-stu-id="ac72a-229">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="ac72a-230">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-230">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="ac72a-231">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="ac72a-231">Logging</span></span>

<span data-ttu-id="ac72a-232">ASP.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками.</span><span class="sxs-lookup"><span data-stu-id="ac72a-232">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="ac72a-233">Доступны следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="ac72a-233">Available providers include the following:</span></span>

* <span data-ttu-id="ac72a-234">Консоль</span><span class="sxs-lookup"><span data-stu-id="ac72a-234">Console</span></span>
* <span data-ttu-id="ac72a-235">Отладка</span><span class="sxs-lookup"><span data-stu-id="ac72a-235">Debug</span></span>
* <span data-ttu-id="ac72a-236">Трассировка событий Windows</span><span class="sxs-lookup"><span data-stu-id="ac72a-236">Event Tracing on Windows</span></span>
* <span data-ttu-id="ac72a-237">Журнал событий Windows</span><span class="sxs-lookup"><span data-stu-id="ac72a-237">Windows Event Log</span></span>
* <span data-ttu-id="ac72a-238">TraceSource</span><span class="sxs-lookup"><span data-stu-id="ac72a-238">TraceSource</span></span>
* <span data-ttu-id="ac72a-239">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="ac72a-239">Azure App Service</span></span>
* <span data-ttu-id="ac72a-240">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="ac72a-240">Azure Application Insights</span></span>

<span data-ttu-id="ac72a-241">Записи в журналы можно вносить в любом месте кода приложения. Для этого нужно получить объект `ILogger` из внедрения зависимостей и вызвать методы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="ac72a-241">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="ac72a-242">Ниже приведен пример кода, где используется объект `ILogger`. В коде выделены строки внедрения конструктора и вызова методов ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="ac72a-242">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="ac72a-243">Интерфейс `ILogger` позволяет передавать в поставщик ведения журналов любое количество полей.</span><span class="sxs-lookup"><span data-stu-id="ac72a-243">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="ac72a-244">Как правило, поля используются для создания строки сообщения, но поставщик также может отправлять их в хранилище данных в виде отдельных полей.</span><span class="sxs-lookup"><span data-stu-id="ac72a-244">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="ac72a-245">Благодаря этому поставщики могут реализовывать [семантическое (структурированное) ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="ac72a-245">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="ac72a-246">Для получения дополнительной информации см. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-246">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="ac72a-247">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="ac72a-247">Routing</span></span>

<span data-ttu-id="ac72a-248">*Маршрут* — это шаблон URL-адреса, сопоставляемый с обработчиком.</span><span class="sxs-lookup"><span data-stu-id="ac72a-248">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="ac72a-249">Обычно обработчик представляет собой страницу Razor, метод действия в контроллере MVC или ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="ac72a-249">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="ac72a-250">Маршрутизация ASP.NET Core позволяет контролировать URL-адреса, используемые приложением.</span><span class="sxs-lookup"><span data-stu-id="ac72a-250">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="ac72a-251">Для получения дополнительной информации см. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-251">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="ac72a-252">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="ac72a-252">Error handling</span></span>

<span data-ttu-id="ac72a-253">ASP.NET Core содержит встроенные функции обработки ошибок, такие как:</span><span class="sxs-lookup"><span data-stu-id="ac72a-253">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="ac72a-254">Страница исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="ac72a-254">A developer exception page</span></span>
* <span data-ttu-id="ac72a-255">Настраиваемые страницы ошибок</span><span class="sxs-lookup"><span data-stu-id="ac72a-255">Custom error pages</span></span>
* <span data-ttu-id="ac72a-256">Статические страницы с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="ac72a-256">Static status code pages</span></span>
* <span data-ttu-id="ac72a-257">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="ac72a-257">Startup exception handling</span></span>

<span data-ttu-id="ac72a-258">Для получения дополнительной информации см. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-258">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="ac72a-259">Создание HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="ac72a-259">Make HTTP requests</span></span>

<span data-ttu-id="ac72a-260">ASP.NET Core включает реализацию интерфейса `IHttpClientFactory` для создания экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-260">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="ac72a-261">Фабрика:</span><span class="sxs-lookup"><span data-stu-id="ac72a-261">The factory:</span></span>

* <span data-ttu-id="ac72a-262">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-262">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="ac72a-263">Например, можно зарегистрировать и использовать клиент *github* для доступа к GitHub.</span><span class="sxs-lookup"><span data-stu-id="ac72a-263">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="ac72a-264">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="ac72a-264">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="ac72a-265">Поддержка регистрации и связывания в цепочки множества делегирующих обработчиков для создания конвейера ПО промежуточного слоя под исходящие запросы.</span><span class="sxs-lookup"><span data-stu-id="ac72a-265">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="ac72a-266">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac72a-266">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="ac72a-267">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="ac72a-267">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="ac72a-268">Интеграция с *Polly* — популярной сторонней библиотекой для обработки временных сбоев.</span><span class="sxs-lookup"><span data-stu-id="ac72a-268">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="ac72a-269">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="ac72a-269">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="ac72a-270">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="ac72a-270">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="ac72a-271">Для получения дополнительной информации см. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-271">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="ac72a-272">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="ac72a-272">Content root</span></span>

<span data-ttu-id="ac72a-273">Корень содержимого — это базовый путь к любому закрытому содержимому, например к файлам Razor, которое используется приложением.</span><span class="sxs-lookup"><span data-stu-id="ac72a-273">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="ac72a-274">По умолчанию корень содержимого является базовым путем исполняемого файла приложения.</span><span class="sxs-lookup"><span data-stu-id="ac72a-274">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="ac72a-275">Альтернативное расположение можно указать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="ac72a-275">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac72a-276">Дополнительные сведения: [Корень содержимого](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="ac72a-276">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac72a-277">Дополнительные сведения: [Корень содержимого](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="ac72a-277">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="ac72a-278">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="ac72a-278">Web root</span></span>

<span data-ttu-id="ac72a-279">Корневой каталог документов (*webroot*) — это базовый путь к общедоступным статическим ресурсам, включая CSS, JavaScript и файлы изображений.</span><span class="sxs-lookup"><span data-stu-id="ac72a-279">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="ac72a-280">По умолчанию ПО промежуточного слоя статических файлов обслуживает только файлы из корневого каталога документов и его подкаталогов.</span><span class="sxs-lookup"><span data-stu-id="ac72a-280">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="ac72a-281">По умолчанию используется путь *{корневой каталог содержимого}/wwwroot*. Альтернативное расположение можно задать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="ac72a-281">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="ac72a-282">Для указания на корневой каталог файлов Razor (*CSHTML*) используется символ тильды и косой черты `~/`.</span><span class="sxs-lookup"><span data-stu-id="ac72a-282">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="ac72a-283">Пути, начинающиеся с `~/`, называются виртуальными путями.</span><span class="sxs-lookup"><span data-stu-id="ac72a-283">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="ac72a-284">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="ac72a-284">For more information, see <xref:fundamentals/static-files>.</span></span>
