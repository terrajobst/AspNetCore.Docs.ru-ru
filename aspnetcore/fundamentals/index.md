---
title: Основы ASP.NET Core
author: rick-anderson
description: Познакомьтесь с основными принципами создания приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: a6c848987c97103864fd5410922346e85a68c353
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856237"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="7ccdf-103">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ccdf-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="7ccdf-104">В этой статье приведен обзор основных тем, связанных с разработкой приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="7ccdf-105">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="7ccdf-105">The Startup class</span></span>

<span data-ttu-id="7ccdf-106">В классе `Startup` делается следующее.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="7ccdf-107">Настраиваются службы, необходимые приложению.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="7ccdf-108">Определяется конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="7ccdf-109">*Службы* — это компоненты, которые используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="7ccdf-110">Например, службой является компонент ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-110">For example, a logging component is a service.</span></span> <span data-ttu-id="7ccdf-111">Код для настройки (или *регистрации*) служб добавляется в метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="7ccdf-112">Конвейер обработки запросов состоит из ряда компонентов *ПО промежуточного* слоя.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="7ccdf-113">Например, это ПО может обрабатывать запросы для статических файлов или перенаправлять HTTP-запросы в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="7ccdf-114">Каждый компонент ПО промежуточного слоя выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="7ccdf-115">Код для настройки конвейера обработки запросов добавляется в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="7ccdf-116">Пример класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="7ccdf-117">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7ccdf-118">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="7ccdf-118">Dependency injection (services)</span></span>

<span data-ttu-id="7ccdf-119">ASP.NET Core включает встроенную платформу внедрения зависимостей, позволяющую классам приложения обращаться к настроенным службам.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="7ccdf-120">Одним из вариантов получения экземпляра службы в классе является создание конструктора с параметром требуемого типа.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="7ccdf-121">Параметр может быть типом службы или интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="7ccdf-122">Система внедрения зависимостей предоставляет службу во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="7ccdf-123">Ниже показан класс, который использует внедрение зависимостей для получения объекта контекста Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="7ccdf-124">Выделенная строка является примером внедрения через конструктор.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="7ccdf-125">Несмотря на то, что система внедрения зависимостей является встроенной, она позволяет вам при желании подключать сторонний контейнер с инверсией управления (IoC).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="7ccdf-126">Дополнительные сведения можно найти по адресу: <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="7ccdf-127">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="7ccdf-127">Middleware</span></span>

<span data-ttu-id="7ccdf-128">Конвейер обработки запросов состоит из ряда компонентов ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="7ccdf-129">Каждый компонент выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="7ccdf-130">По принятому соглашению компонент ПО промежуточного слоя добавляется в конвейер вызовом метода расширения `Use...` в методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="7ccdf-131">Например, чтобы включить отрисовку статических файлов, вызовите `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="7ccdf-132">Код настройки конвейера обработки запросов выделен в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="7ccdf-133">ASP.NET Core включает обширный набор встроенного ПО промежуточного слоя и позволяет вам писать свое собственное.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="7ccdf-134">Дополнительные сведения можно найти по адресу: <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="7ccdf-135">Узел</span><span class="sxs-lookup"><span data-stu-id="7ccdf-135">Host</span></span>

<span data-ttu-id="7ccdf-136">Приложение ASP.NET Core создает *узел* при запуске.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="7ccdf-137">Узел — это объект, который инкапсулирует все ресурсы приложения.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="7ccdf-138">Реализация HTTP-сервера</span><span class="sxs-lookup"><span data-stu-id="7ccdf-138">An HTTP server implementation</span></span>
* <span data-ttu-id="7ccdf-139">Компоненты ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="7ccdf-139">Middleware components</span></span>
* <span data-ttu-id="7ccdf-140">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="7ccdf-140">Logging</span></span>
* <span data-ttu-id="7ccdf-141">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="7ccdf-141">DI</span></span>
* <span data-ttu-id="7ccdf-142">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="7ccdf-142">Configuration</span></span>

<span data-ttu-id="7ccdf-143">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7ccdf-144">Доступны два узла: универсальный узел и веб-узел.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="7ccdf-145">Универсальный узел является рекомендуемым вариантом, а веб-узел служит только для обеспечения обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="7ccdf-146">Код для создания узла находится в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="7ccdf-147">Методы `CreateDefaultBuilder` и `ConfigureWebHostDefaults` применяются для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="7ccdf-148">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7ccdf-149">Загрузка конфигурации из файлов *appsettings.json* и *appsettings.{имя среды}.json*, переменных среды, аргументов командной строки и других источников.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7ccdf-150">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7ccdf-151">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7ccdf-152">Доступны два узла: веб-узел и универсальный узел.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="7ccdf-153">В ASP.NET Core 2.x универсальный узел используется в сценариях, не связанных с Интернетом.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="7ccdf-154">Код для создания узла находится в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="7ccdf-155">Метод `CreateDefaultBuilder` применяется для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="7ccdf-156">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7ccdf-157">Загрузка конфигурации из файлов *appsettings.json* и *appsettings.{имя среды}.json*, переменных среды, аргументов командной строки и других источников.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7ccdf-158">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7ccdf-159">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="7ccdf-160">Сценарии, не связанные с Интернетом</span><span class="sxs-lookup"><span data-stu-id="7ccdf-160">Non-web scenarios</span></span>

<span data-ttu-id="7ccdf-161">Универсальный узел позволяет приложениям других типов использовать перекрестные расширения платформ, такие как средства ведения журналов, внедрения зависимостей, настройки и управления жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="7ccdf-162">Дополнительные сведения см. в разделах <xref:fundamentals/host/generic-host> и <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="7ccdf-163">Серверы</span><span class="sxs-lookup"><span data-stu-id="7ccdf-163">Servers</span></span>

<span data-ttu-id="7ccdf-164">Приложение ASP.NET Core использует реализацию HTTP-сервера для приема HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="7ccdf-165">Сервер отправляет приложению запросы в виде набора [функций запросов](xref:fundamentals/request-features), объединенных в `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="7ccdf-166">Windows</span><span class="sxs-lookup"><span data-stu-id="7ccdf-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="7ccdf-167">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7ccdf-168">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7ccdf-169">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7ccdf-170">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7ccdf-171">*HTTP-сервер IIS* — это сервер для Windows, который использует службы IIS.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="7ccdf-172">Он позволяет запускать приложение ASP.NET Core и службы IIS в одном процессе.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="7ccdf-173">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="7ccdf-174">macOS</span><span class="sxs-lookup"><span data-stu-id="7ccdf-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="7ccdf-175">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ccdf-176">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ccdf-177">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="7ccdf-178">Linux</span><span class="sxs-lookup"><span data-stu-id="7ccdf-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="7ccdf-179">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ccdf-180">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ccdf-181">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="7ccdf-182">Windows</span><span class="sxs-lookup"><span data-stu-id="7ccdf-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="7ccdf-183">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7ccdf-184">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7ccdf-185">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7ccdf-186">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7ccdf-187">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="7ccdf-188">macOS</span><span class="sxs-lookup"><span data-stu-id="7ccdf-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="7ccdf-189">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ccdf-190">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ccdf-191">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="7ccdf-192">Linux</span><span class="sxs-lookup"><span data-stu-id="7ccdf-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="7ccdf-193">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7ccdf-194">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7ccdf-195">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="7ccdf-196">Дополнительные сведения можно найти по адресу: <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="7ccdf-197">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="7ccdf-197">Configuration</span></span>

<span data-ttu-id="7ccdf-198">ASP.NET Core предоставляет платформу конфигурации, которая получает параметры в виде пар "имя-значение" от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="7ccdf-199">Доступны встроенные поставщики конфигурации для различных источников, таких как файлы *JSON*, *XML*, переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="7ccdf-200">Кроме того, вы можете писать поставщики конфигурации сами.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="7ccdf-201">Например, можно указать, что источником конфигурации являются файл *appsettings.json* и переменные среды.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="7ccdf-202">В этом случае при запросе значения *ConnectionString* платформа сначала выполнит поиск в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="7ccdf-203">Если значение будет найдено не только в нем, но и в переменной среды, приоритет получит значение из переменной среды.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="7ccdf-204">Для управления конфиденциальными данными конфигурации, например паролями, ASP.NET Core предоставляет специальный инструмент [Менеджер секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7ccdf-205">Для секретов в рабочей среде рекомендуется использовать [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7ccdf-206">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="7ccdf-207">Параметры</span><span class="sxs-lookup"><span data-stu-id="7ccdf-207">Options</span></span>

<span data-ttu-id="7ccdf-208">Когда это возможно, ASP.NET Core следует *шаблону параметров* для хранения и получения значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="7ccdf-209">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="7ccdf-210">Например, следующий код задает параметры WebSockets:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="7ccdf-211">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="7ccdf-212">Среды</span><span class="sxs-lookup"><span data-stu-id="7ccdf-212">Environments</span></span>

<span data-ttu-id="7ccdf-213">Среды выполнения, включая среду *разработки*, *промежуточную* и *рабочую* среды, являются ключевыми компонентами ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="7ccdf-214">Указать среду, в которой запускается приложение, можно с помощью переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7ccdf-215">ASP.NET Core считывает переменную среды при запуске приложения и сохраняет ее значение в реализации `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="7ccdf-216">Объект среды можно получить в любом месте приложения при помощи внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="7ccdf-217">В следующем примере кода из класса `Startup` показана настройка приложения для вывода подробных сведений об ошибках исключительно при запуске в среде разработки:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="7ccdf-218">Дополнительные сведения можно найти по адресу: <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="7ccdf-219">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="7ccdf-219">Logging</span></span>

<span data-ttu-id="7ccdf-220">ASP.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="7ccdf-221">Доступны следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-221">Available providers include the following:</span></span>

* <span data-ttu-id="7ccdf-222">Консоль</span><span class="sxs-lookup"><span data-stu-id="7ccdf-222">Console</span></span>
* <span data-ttu-id="7ccdf-223">Отладка</span><span class="sxs-lookup"><span data-stu-id="7ccdf-223">Debug</span></span>
* <span data-ttu-id="7ccdf-224">Трассировка событий Windows</span><span class="sxs-lookup"><span data-stu-id="7ccdf-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="7ccdf-225">Журнал событий Windows</span><span class="sxs-lookup"><span data-stu-id="7ccdf-225">Windows Event Log</span></span>
* <span data-ttu-id="7ccdf-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7ccdf-226">TraceSource</span></span>
* <span data-ttu-id="7ccdf-227">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="7ccdf-227">Azure App Service</span></span>
* <span data-ttu-id="7ccdf-228">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7ccdf-228">Azure Application Insights</span></span>

<span data-ttu-id="7ccdf-229">Записи в журналы можно вносить в любом месте кода приложения. Для этого нужно получить объект `ILogger` из внедрения зависимостей и вызвать методы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="7ccdf-230">Ниже приведен пример кода, где используется объект `ILogger`. В коде выделены строки внедрения конструктора и вызова методов ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="7ccdf-231">Интерфейс `ILogger` позволяет передавать в поставщик ведения журналов любое количество полей.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="7ccdf-232">Как правило, поля используются для создания строки сообщения, но поставщик также может отправлять их в хранилище данных в виде отдельных полей.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="7ccdf-233">Благодаря этому поставщики могут реализовывать [семантическое (структурированное) ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7ccdf-234">Дополнительные сведения можно найти по адресу: <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="7ccdf-235">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="7ccdf-235">Routing</span></span>

<span data-ttu-id="7ccdf-236">*Маршрут* — это шаблон URL-адреса, сопоставляемый с обработчиком.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="7ccdf-237">Обычно обработчик представляет собой страницу Razor, метод действия в контроллере MVC или ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="7ccdf-238">Маршрутизация ASP.NET Core позволяет контролировать URL-адреса, используемые приложением.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="7ccdf-239">Дополнительные сведения можно найти по адресу: <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="7ccdf-240">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="7ccdf-240">Error handling</span></span>

<span data-ttu-id="7ccdf-241">ASP.NET Core содержит встроенные функции обработки ошибок, такие как:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="7ccdf-242">Страница исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="7ccdf-242">A developer exception page</span></span>
* <span data-ttu-id="7ccdf-243">Настраиваемые страницы ошибок</span><span class="sxs-lookup"><span data-stu-id="7ccdf-243">Custom error pages</span></span>
* <span data-ttu-id="7ccdf-244">Статические страницы с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="7ccdf-244">Static status code pages</span></span>
* <span data-ttu-id="7ccdf-245">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="7ccdf-245">Startup exception handling</span></span>

<span data-ttu-id="7ccdf-246">Дополнительные сведения можно найти по адресу: <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="7ccdf-247">Создание HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="7ccdf-247">Make HTTP requests</span></span>

<span data-ttu-id="7ccdf-248">ASP.NET Core включает реализацию интерфейса `IHttpClientFactory` для создания экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="7ccdf-249">Фабрика:</span><span class="sxs-lookup"><span data-stu-id="7ccdf-249">The factory:</span></span>

* <span data-ttu-id="7ccdf-250">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7ccdf-251">Например, можно зарегистрировать и использовать клиент *github* для доступа к GitHub.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="7ccdf-252">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7ccdf-253">Поддержка регистрации и связывания в цепочки множества делегирующих обработчиков для создания конвейера ПО промежуточного слоя под исходящие запросы.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7ccdf-254">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7ccdf-255">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="7ccdf-256">Интеграция с *Polly* — популярной сторонней библиотекой для обработки временных сбоев.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="7ccdf-257">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7ccdf-258">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7ccdf-259">Дополнительные сведения можно найти по адресу: <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="7ccdf-260">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="7ccdf-260">Content root</span></span>

<span data-ttu-id="7ccdf-261">Корень содержимого — это базовый путь к любому закрытому содержимому, например к файлам Razor, которое используется приложением.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-261">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="7ccdf-262">По умолчанию корень содержимого является базовым путем исполняемого файла приложения.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-262">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="7ccdf-263">Альтернативное расположение можно указать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-263">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7ccdf-264">Дополнительные сведения: [Корень содержимого](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-264">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7ccdf-265">Дополнительные сведения: [Корень содержимого](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-265">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="7ccdf-266">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="7ccdf-266">Web root</span></span>

<span data-ttu-id="7ccdf-267">Корневой каталог документов (*webroot*) — это базовый путь к общедоступным статическим ресурсам, включая CSS, JavaScript и файлы изображений.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-267">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="7ccdf-268">По умолчанию ПО промежуточного слоя статических файлов обслуживает только файлы из корневого каталога документов и его подкаталогов.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-268">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="7ccdf-269">По умолчанию используется путь *{корневой каталог содержимого}/wwwroot*. Альтернативное расположение можно задать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="7ccdf-269">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="7ccdf-270">Для указания на корневой каталог файлов Razor (*CSHTML*) используется символ тильды и косой черты `~/`.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-270">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="7ccdf-271">Пути, начинающиеся с `~/`, называются виртуальными путями.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-271">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="7ccdf-272">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7ccdf-272">For more information, see <xref:fundamentals/static-files>.</span></span>
