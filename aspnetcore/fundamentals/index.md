---
title: Основы ASP.NET Core
author: rick-anderson
description: Познакомьтесь с основными принципами создания приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: 3fbfc7c4c0d5e568339bc00a7cbe84a3932acf1f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78644554"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="9d620-103">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d620-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="9d620-104">В этой статье приведен обзор основных тем, связанных с разработкой приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d620-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="9d620-105">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="9d620-105">The Startup class</span></span>

<span data-ttu-id="9d620-106">В классе `Startup` делается следующее.</span><span class="sxs-lookup"><span data-stu-id="9d620-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="9d620-107">Настраиваются службы, необходимые приложению.</span><span class="sxs-lookup"><span data-stu-id="9d620-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="9d620-108">Определяется конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="9d620-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="9d620-109">*Службы* — это компоненты, которые используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="9d620-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="9d620-110">Например, службой является компонент ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="9d620-110">For example, a logging component is a service.</span></span> <span data-ttu-id="9d620-111">Код для настройки (или *регистрации*) служб добавляется в метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9d620-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="9d620-112">Конвейер обработки запросов состоит из ряда компонентов *ПО промежуточного* слоя.</span><span class="sxs-lookup"><span data-stu-id="9d620-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="9d620-113">Например, это ПО может обрабатывать запросы для статических файлов или перенаправлять HTTP-запросы в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9d620-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="9d620-114">Каждый компонент ПО промежуточного слоя выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="9d620-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="9d620-115">Код для настройки конвейера обработки запросов добавляется в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9d620-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="9d620-116">Пример класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="9d620-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="9d620-117">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="9d620-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="9d620-118">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="9d620-118">Dependency injection (services)</span></span>

<span data-ttu-id="9d620-119">ASP.NET Core включает встроенную платформу внедрения зависимостей, позволяющую классам приложения обращаться к настроенным службам.</span><span class="sxs-lookup"><span data-stu-id="9d620-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="9d620-120">Одним из вариантов получения экземпляра службы в классе является создание конструктора с параметром требуемого типа.</span><span class="sxs-lookup"><span data-stu-id="9d620-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="9d620-121">Параметр может быть типом службы или интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="9d620-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="9d620-122">Система внедрения зависимостей предоставляет службу во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="9d620-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="9d620-123">Ниже показан класс, который использует внедрение зависимостей для получения объекта контекста Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9d620-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="9d620-124">Выделенная строка является примером внедрения через конструктор.</span><span class="sxs-lookup"><span data-stu-id="9d620-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="9d620-125">Несмотря на то, что система внедрения зависимостей является встроенной, она позволяет вам при желании подключать сторонний контейнер с инверсией управления (IoC).</span><span class="sxs-lookup"><span data-stu-id="9d620-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="9d620-126">Для получения дополнительной информации см. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="9d620-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="9d620-127">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="9d620-127">Middleware</span></span>

<span data-ttu-id="9d620-128">Конвейер обработки запросов состоит из ряда компонентов ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="9d620-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="9d620-129">Каждый компонент выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="9d620-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="9d620-130">По принятому соглашению компонент ПО промежуточного слоя добавляется в конвейер вызовом метода расширения `Use...` в методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9d620-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="9d620-131">Например, чтобы включить отрисовку статических файлов, вызовите `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="9d620-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="9d620-132">Код настройки конвейера обработки запросов выделен в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="9d620-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="9d620-133">ASP.NET Core включает обширный набор встроенного ПО промежуточного слоя и позволяет вам писать свое собственное.</span><span class="sxs-lookup"><span data-stu-id="9d620-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="9d620-134">Для получения дополнительной информации см. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="9d620-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="9d620-135">Узел</span><span class="sxs-lookup"><span data-stu-id="9d620-135">Host</span></span>

<span data-ttu-id="9d620-136">Приложение ASP.NET Core создает *узел* при запуске.</span><span class="sxs-lookup"><span data-stu-id="9d620-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="9d620-137">Узел — это объект, который инкапсулирует все ресурсы приложения.</span><span class="sxs-lookup"><span data-stu-id="9d620-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="9d620-138">Реализация HTTP-сервера</span><span class="sxs-lookup"><span data-stu-id="9d620-138">An HTTP server implementation</span></span>
* <span data-ttu-id="9d620-139">Компоненты ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="9d620-139">Middleware components</span></span>
* <span data-ttu-id="9d620-140">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="9d620-140">Logging</span></span>
* <span data-ttu-id="9d620-141">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="9d620-141">DI</span></span>
* <span data-ttu-id="9d620-142">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="9d620-142">Configuration</span></span>

<span data-ttu-id="9d620-143">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="9d620-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d620-144">Доступны два узла: универсальный узел и веб-узел.</span><span class="sxs-lookup"><span data-stu-id="9d620-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="9d620-145">Универсальный узел является рекомендуемым вариантом, а веб-узел служит только для обеспечения обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="9d620-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="9d620-146">Код для создания узла находится в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="9d620-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="9d620-147">Методы `CreateDefaultBuilder` и `ConfigureWebHostDefaults` применяются для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="9d620-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="9d620-148">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="9d620-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="9d620-149">Загрузка конфигурации из файлов *appsettings.json* и *appsettings.{имя среды}.json*, переменных среды, аргументов командной строки и других источников.</span><span class="sxs-lookup"><span data-stu-id="9d620-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="9d620-150">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="9d620-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="9d620-151">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="9d620-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d620-152">Доступны два узла: веб-узел и универсальный узел.</span><span class="sxs-lookup"><span data-stu-id="9d620-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="9d620-153">В ASP.NET Core 2.x универсальный узел используется в сценариях, не связанных с Интернетом.</span><span class="sxs-lookup"><span data-stu-id="9d620-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="9d620-154">Код для создания узла находится в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="9d620-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="9d620-155">Метод `CreateDefaultBuilder` применяется для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="9d620-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="9d620-156">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="9d620-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="9d620-157">Загрузка конфигурации из файлов *appsettings.json* и *appsettings.{имя среды}.json*, переменных среды, аргументов командной строки и других источников.</span><span class="sxs-lookup"><span data-stu-id="9d620-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="9d620-158">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="9d620-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="9d620-159">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="9d620-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="9d620-160">Сценарии, не связанные с Интернетом</span><span class="sxs-lookup"><span data-stu-id="9d620-160">Non-web scenarios</span></span>

<span data-ttu-id="9d620-161">Универсальный узел позволяет приложениям других типов использовать перекрестные расширения платформ, такие как средства ведения журналов, внедрения зависимостей, настройки и управления жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="9d620-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="9d620-162">Дополнительные сведения см. в разделах <xref:fundamentals/host/generic-host> и <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="9d620-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="9d620-163">Серверы</span><span class="sxs-lookup"><span data-stu-id="9d620-163">Servers</span></span>

<span data-ttu-id="9d620-164">Приложение ASP.NET Core использует реализацию HTTP-сервера для приема HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="9d620-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="9d620-165">Сервер отправляет приложению запросы в виде набора [функций запросов](xref:fundamentals/request-features), объединенных в `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="9d620-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="9d620-166">Windows</span><span class="sxs-lookup"><span data-stu-id="9d620-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="9d620-167">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="9d620-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="9d620-168">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="9d620-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="9d620-169">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="9d620-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="9d620-170">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="9d620-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="9d620-171">*HTTP-сервер IIS* — это сервер для Windows, который использует службы IIS.</span><span class="sxs-lookup"><span data-stu-id="9d620-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="9d620-172">Он позволяет запускать приложение ASP.NET Core и службы IIS в одном процессе.</span><span class="sxs-lookup"><span data-stu-id="9d620-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="9d620-173">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="9d620-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="9d620-174">macOS</span><span class="sxs-lookup"><span data-stu-id="9d620-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="9d620-175">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="9d620-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="9d620-176">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="9d620-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="9d620-177">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="9d620-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="9d620-178">Linux</span><span class="sxs-lookup"><span data-stu-id="9d620-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="9d620-179">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="9d620-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="9d620-180">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="9d620-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="9d620-181">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="9d620-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="9d620-182">Windows</span><span class="sxs-lookup"><span data-stu-id="9d620-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="9d620-183">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="9d620-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="9d620-184">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="9d620-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="9d620-185">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="9d620-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="9d620-186">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="9d620-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="9d620-187">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="9d620-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="9d620-188">macOS</span><span class="sxs-lookup"><span data-stu-id="9d620-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="9d620-189">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="9d620-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="9d620-190">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="9d620-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="9d620-191">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="9d620-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="9d620-192">Linux</span><span class="sxs-lookup"><span data-stu-id="9d620-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="9d620-193">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="9d620-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="9d620-194">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="9d620-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="9d620-195">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="9d620-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="9d620-196">Для получения дополнительной информации см. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="9d620-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="9d620-197">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="9d620-197">Configuration</span></span>

<span data-ttu-id="9d620-198">ASP.NET Core предоставляет платформу конфигурации, которая получает параметры в виде пар "имя-значение" от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="9d620-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="9d620-199">Доступны встроенные поставщики конфигурации для различных источников, таких как файлы *JSON*, *XML*, переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="9d620-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="9d620-200">Кроме того, вы можете писать поставщики конфигурации сами.</span><span class="sxs-lookup"><span data-stu-id="9d620-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="9d620-201">Например, можно указать, что источником конфигурации являются файл *appsettings.json* и переменные среды.</span><span class="sxs-lookup"><span data-stu-id="9d620-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="9d620-202">В этом случае при запросе значения *ConnectionString* платформа сначала выполнит поиск в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9d620-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="9d620-203">Если значение будет найдено не только в нем, но и в переменной среды, приоритет получит значение из переменной среды.</span><span class="sxs-lookup"><span data-stu-id="9d620-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="9d620-204">Для управления конфиденциальными данными конфигурации, например паролями, ASP.NET Core предоставляет специальный инструмент [Менеджер секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="9d620-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="9d620-205">Для секретов в рабочей среде рекомендуется использовать [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="9d620-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="9d620-206">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9d620-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="9d620-207">Параметры</span><span class="sxs-lookup"><span data-stu-id="9d620-207">Options</span></span>

<span data-ttu-id="9d620-208">Когда это возможно, ASP.NET Core следует *шаблону параметров* для хранения и получения значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="9d620-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="9d620-209">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="9d620-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="9d620-210">Например, следующий код задает параметры WebSockets:</span><span class="sxs-lookup"><span data-stu-id="9d620-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="9d620-211">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="9d620-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="9d620-212">Среды</span><span class="sxs-lookup"><span data-stu-id="9d620-212">Environments</span></span>

<span data-ttu-id="9d620-213">Среды выполнения, включая среду *разработки*, *промежуточную* и *рабочую* среды, являются ключевыми компонентами ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d620-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="9d620-214">Указать среду, в которой запускается приложение, можно с помощью переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="9d620-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="9d620-215">ASP.NET Core считывает переменную среды при запуске приложения и сохраняет ее значение в реализации `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="9d620-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="9d620-216">Объект среды можно получить в любом месте приложения при помощи внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9d620-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="9d620-217">В следующем примере кода из класса `Startup` показана настройка приложения для вывода подробных сведений об ошибках исключительно при запуске в среде разработки:</span><span class="sxs-lookup"><span data-stu-id="9d620-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="9d620-218">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="9d620-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="9d620-219">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="9d620-219">Logging</span></span>

<span data-ttu-id="9d620-220">ASP.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками.</span><span class="sxs-lookup"><span data-stu-id="9d620-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="9d620-221">Доступны следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="9d620-221">Available providers include the following:</span></span>

* <span data-ttu-id="9d620-222">Консоль</span><span class="sxs-lookup"><span data-stu-id="9d620-222">Console</span></span>
* <span data-ttu-id="9d620-223">Отладка</span><span class="sxs-lookup"><span data-stu-id="9d620-223">Debug</span></span>
* <span data-ttu-id="9d620-224">Трассировка событий Windows</span><span class="sxs-lookup"><span data-stu-id="9d620-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="9d620-225">Журнал событий Windows</span><span class="sxs-lookup"><span data-stu-id="9d620-225">Windows Event Log</span></span>
* <span data-ttu-id="9d620-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9d620-226">TraceSource</span></span>
* <span data-ttu-id="9d620-227">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="9d620-227">Azure App Service</span></span>
* <span data-ttu-id="9d620-228">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="9d620-228">Azure Application Insights</span></span>

<span data-ttu-id="9d620-229">Записи в журналы можно вносить в любом месте кода приложения. Для этого нужно получить объект `ILogger` из внедрения зависимостей и вызвать методы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="9d620-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="9d620-230">Ниже приведен пример кода, где используется объект `ILogger`. В коде выделены строки внедрения конструктора и вызова методов ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="9d620-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="9d620-231">Интерфейс `ILogger` позволяет передавать в поставщик ведения журналов любое количество полей.</span><span class="sxs-lookup"><span data-stu-id="9d620-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="9d620-232">Как правило, поля используются для создания строки сообщения, но поставщик также может отправлять их в хранилище данных в виде отдельных полей.</span><span class="sxs-lookup"><span data-stu-id="9d620-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="9d620-233">Благодаря этому поставщики могут реализовывать [семантическое (структурированное) ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="9d620-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="9d620-234">Для получения дополнительной информации см. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="9d620-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="9d620-235">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="9d620-235">Routing</span></span>

<span data-ttu-id="9d620-236">*Маршрут* — это шаблон URL-адреса, сопоставляемый с обработчиком.</span><span class="sxs-lookup"><span data-stu-id="9d620-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="9d620-237">Обычно обработчик представляет собой страницу Razor, метод действия в контроллере MVC или ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="9d620-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="9d620-238">Маршрутизация ASP.NET Core позволяет контролировать URL-адреса, используемые приложением.</span><span class="sxs-lookup"><span data-stu-id="9d620-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="9d620-239">Для получения дополнительной информации см. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="9d620-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="9d620-240">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="9d620-240">Error handling</span></span>

<span data-ttu-id="9d620-241">ASP.NET Core содержит встроенные функции обработки ошибок, такие как:</span><span class="sxs-lookup"><span data-stu-id="9d620-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="9d620-242">Страница исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="9d620-242">A developer exception page</span></span>
* <span data-ttu-id="9d620-243">Настраиваемые страницы ошибок</span><span class="sxs-lookup"><span data-stu-id="9d620-243">Custom error pages</span></span>
* <span data-ttu-id="9d620-244">Статические страницы с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="9d620-244">Static status code pages</span></span>
* <span data-ttu-id="9d620-245">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="9d620-245">Startup exception handling</span></span>

<span data-ttu-id="9d620-246">Для получения дополнительной информации см. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="9d620-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="9d620-247">Создание HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="9d620-247">Make HTTP requests</span></span>

<span data-ttu-id="9d620-248">ASP.NET Core включает реализацию интерфейса `IHttpClientFactory` для создания экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="9d620-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="9d620-249">Фабрика:</span><span class="sxs-lookup"><span data-stu-id="9d620-249">The factory:</span></span>

* <span data-ttu-id="9d620-250">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="9d620-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="9d620-251">Например, можно зарегистрировать и использовать клиент *github* для доступа к GitHub.</span><span class="sxs-lookup"><span data-stu-id="9d620-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="9d620-252">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="9d620-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="9d620-253">Поддержка регистрации и связывания в цепочки множества делегирующих обработчиков для создания конвейера ПО промежуточного слоя под исходящие запросы.</span><span class="sxs-lookup"><span data-stu-id="9d620-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="9d620-254">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d620-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="9d620-255">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="9d620-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="9d620-256">Интеграция с *Polly* — популярной сторонней библиотекой для обработки временных сбоев.</span><span class="sxs-lookup"><span data-stu-id="9d620-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="9d620-257">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="9d620-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="9d620-258">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="9d620-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="9d620-259">Для получения дополнительной информации см. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="9d620-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="9d620-260">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="9d620-260">Content root</span></span>

<span data-ttu-id="9d620-261">Корневой каталог содержимого — это базовый путь к таким элементам:</span><span class="sxs-lookup"><span data-stu-id="9d620-261">The content root is the base path to the:</span></span>

* <span data-ttu-id="9d620-262">Исполняемый файл, в котором размещено приложение (*EXE*).</span><span class="sxs-lookup"><span data-stu-id="9d620-262">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="9d620-263">Скомпилированные сборки, составляющие приложение (*DLL*).</span><span class="sxs-lookup"><span data-stu-id="9d620-263">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="9d620-264">Файлы содержимого, не относящиеся к коду, используемые приложением, например:</span><span class="sxs-lookup"><span data-stu-id="9d620-264">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="9d620-265">Файлы Razor (*CSHTML*, *RAZOR*).</span><span class="sxs-lookup"><span data-stu-id="9d620-265">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="9d620-266">Файлы конфигурации (*JSON*, *XML*).</span><span class="sxs-lookup"><span data-stu-id="9d620-266">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="9d620-267">Файлы данных (*DB*).</span><span class="sxs-lookup"><span data-stu-id="9d620-267">Data files (*.db*)</span></span>
* <span data-ttu-id="9d620-268">[Корневой веб-каталог](#web-root), обычно опубликованная папка *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="9d620-268">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="9d620-269">Во время разработки:</span><span class="sxs-lookup"><span data-stu-id="9d620-269">During development:</span></span>

* <span data-ttu-id="9d620-270">Корень содержимого по умолчанию сбрасывается до корневого каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="9d620-270">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="9d620-271">Корневой каталог проекта используется для создания:</span><span class="sxs-lookup"><span data-stu-id="9d620-271">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="9d620-272">пути к файлам содержимого, не являющихся кодом приложения, в корневом каталоге проекта;</span><span class="sxs-lookup"><span data-stu-id="9d620-272">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="9d620-273">[корневого веб-каталога](#web-root), обычно папка *wwwroot* в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="9d620-273">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d620-274">Альтернативный корневой путь к содержимому может быть указан при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="9d620-274">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="9d620-275">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="9d620-275">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d620-276">Альтернативный корневой путь к содержимому может быть указан при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="9d620-276">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="9d620-277">Для получения дополнительной информации см. <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="9d620-277">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="9d620-278">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="9d620-278">Web root</span></span>

<span data-ttu-id="9d620-279">Корневой веб-каталог — это базовый путь к общедоступным, не кодовым файлам статических ресурсов, например:</span><span class="sxs-lookup"><span data-stu-id="9d620-279">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="9d620-280">Таблицы стилей (*CSS*).</span><span class="sxs-lookup"><span data-stu-id="9d620-280">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="9d620-281">JavaScript (*JS*).</span><span class="sxs-lookup"><span data-stu-id="9d620-281">JavaScript (*.js*)</span></span>
* <span data-ttu-id="9d620-282">Изображения (*PNG*, *JPG*).</span><span class="sxs-lookup"><span data-stu-id="9d620-282">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="9d620-283">Статические файлы обслуживаются по умолчанию только в корневом веб-каталоге (и подкаталогах).</span><span class="sxs-lookup"><span data-stu-id="9d620-283">Static files are only served by default from the web root directory (and sub-directories).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d620-284">По умолчанию используется путь *{корневой каталог содержимого}/wwwroot*. Альтернативное расположение можно задать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="9d620-284">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="9d620-285">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="9d620-285">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d620-286">По умолчанию используется путь *{корневой каталог содержимого}/wwwroot*. Альтернативное расположение можно задать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="9d620-286">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="9d620-287">Дополнительные сведения см. в разделе [Корневой веб-каталог](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="9d620-287">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

::: moniker-end

<span data-ttu-id="9d620-288">Запретите публикацию файлов в *wwwroot* с помощью [\<элемента проекта Content>](/visualstudio/msbuild/common-msbuild-project-items#content) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="9d620-288">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="9d620-289">В следующем примере запрещается публикация содержимого в каталоге *wwwroot/local* и подкаталогах:</span><span class="sxs-lookup"><span data-stu-id="9d620-289">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d620-290">Сведения о запрете публикации ресурсов статических удостоверений в корневом веб-каталоге см. в разделе <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="9d620-290">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

::: moniker-end

<span data-ttu-id="9d620-291">Для указания на корневой каталог файлов Razor (*CSHTML*) используется символ тильды и косой черты `~/`.</span><span class="sxs-lookup"><span data-stu-id="9d620-291">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="9d620-292">Путь, начинающийся с `~/`, называется *виртуальным путем*.</span><span class="sxs-lookup"><span data-stu-id="9d620-292">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="9d620-293">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="9d620-293">For more information, see <xref:fundamentals/static-files>.</span></span>
