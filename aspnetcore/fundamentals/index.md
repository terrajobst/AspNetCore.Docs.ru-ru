---
title: Основы ASP.NET Core
author: rick-anderson
description: Познакомьтесь с основными принципами создания приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: 7533242140c31a937f32cc9082d760103347ce25
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219185"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="1c757-103">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c757-103">ASP.NET Core fundamentals</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c757-104">В этой статье приведен обзор основных тем, связанных с разработкой приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c757-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="1c757-105">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="1c757-105">The Startup class</span></span>

<span data-ttu-id="1c757-106">В классе `Startup` делается следующее.</span><span class="sxs-lookup"><span data-stu-id="1c757-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="1c757-107">Настраиваются службы, необходимые приложению.</span><span class="sxs-lookup"><span data-stu-id="1c757-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="1c757-108">Определяется конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="1c757-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="1c757-109">*Службы* — это компоненты, которые используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="1c757-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="1c757-110">Например, службой является компонент ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="1c757-110">For example, a logging component is a service.</span></span> <span data-ttu-id="1c757-111">Код для настройки (или *регистрации*) служб добавляется в метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1c757-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="1c757-112">Конвейер обработки запросов состоит из ряда компонентов *ПО промежуточного* слоя.</span><span class="sxs-lookup"><span data-stu-id="1c757-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="1c757-113">Например, это ПО может обрабатывать запросы для статических файлов или перенаправлять HTTP-запросы в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1c757-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="1c757-114">Каждый компонент ПО промежуточного слоя выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="1c757-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="1c757-115">Код для настройки конвейера обработки запросов добавляется в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="1c757-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="1c757-116">Пример класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="1c757-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="1c757-117">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="1c757-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="1c757-118">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="1c757-118">Dependency injection (services)</span></span>

<span data-ttu-id="1c757-119">ASP.NET Core включает встроенную платформу внедрения зависимостей, позволяющую классам приложения обращаться к настроенным службам.</span><span class="sxs-lookup"><span data-stu-id="1c757-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="1c757-120">Одним из вариантов получения экземпляра службы в классе является создание конструктора с параметром требуемого типа.</span><span class="sxs-lookup"><span data-stu-id="1c757-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="1c757-121">Параметр может быть типом службы или интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="1c757-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="1c757-122">Система внедрения зависимостей предоставляет службу во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="1c757-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="1c757-123">Ниже показан класс, который использует внедрение зависимостей для получения объекта контекста Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1c757-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="1c757-124">Выделенная строка является примером внедрения через конструктор.</span><span class="sxs-lookup"><span data-stu-id="1c757-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="1c757-125">Несмотря на то, что система внедрения зависимостей является встроенной, она позволяет вам при желании подключать сторонний контейнер с инверсией управления (IoC).</span><span class="sxs-lookup"><span data-stu-id="1c757-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="1c757-126">Для получения дополнительной информации см. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="1c757-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="1c757-127">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="1c757-127">Middleware</span></span>

<span data-ttu-id="1c757-128">Конвейер обработки запросов состоит из ряда компонентов ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="1c757-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="1c757-129">Каждый компонент выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="1c757-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="1c757-130">По принятому соглашению компонент ПО промежуточного слоя добавляется в конвейер вызовом метода расширения `Use...` в методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="1c757-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="1c757-131">Например, чтобы включить отрисовку статических файлов, вызовите `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="1c757-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="1c757-132">Код настройки конвейера обработки запросов выделен в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1c757-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="1c757-133">ASP.NET Core включает обширный набор встроенного ПО промежуточного слоя и позволяет вам писать свое собственное.</span><span class="sxs-lookup"><span data-stu-id="1c757-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="1c757-134">Для получения дополнительной информации см. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="1c757-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="1c757-135">Узел</span><span class="sxs-lookup"><span data-stu-id="1c757-135">Host</span></span>

<span data-ttu-id="1c757-136">Приложение ASP.NET Core создает *узел* при запуске.</span><span class="sxs-lookup"><span data-stu-id="1c757-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="1c757-137">Узел — это объект, который инкапсулирует все ресурсы приложения.</span><span class="sxs-lookup"><span data-stu-id="1c757-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="1c757-138">Реализация HTTP-сервера</span><span class="sxs-lookup"><span data-stu-id="1c757-138">An HTTP server implementation</span></span>
* <span data-ttu-id="1c757-139">Компоненты ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="1c757-139">Middleware components</span></span>
* <span data-ttu-id="1c757-140">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="1c757-140">Logging</span></span>
* <span data-ttu-id="1c757-141">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="1c757-141">DI</span></span>
* <span data-ttu-id="1c757-142">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="1c757-142">Configuration</span></span>

<span data-ttu-id="1c757-143">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="1c757-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="1c757-144">Доступны два узла: универсальный узел и веб-узел.</span><span class="sxs-lookup"><span data-stu-id="1c757-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="1c757-145">Универсальный узел является рекомендуемым вариантом, а веб-узел служит только для обеспечения обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="1c757-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="1c757-146">Код для создания узла находится в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="1c757-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="1c757-147">Методы `CreateDefaultBuilder` и `ConfigureWebHostDefaults` применяются для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="1c757-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="1c757-148">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="1c757-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="1c757-149">Загрузка конфигурации из файлов *appsettings.json* и *appsettings.{имя среды}.json*, переменных среды, аргументов командной строки и других источников.</span><span class="sxs-lookup"><span data-stu-id="1c757-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="1c757-150">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="1c757-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="1c757-151">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="1c757-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="1c757-152">Сценарии, не связанные с Интернетом</span><span class="sxs-lookup"><span data-stu-id="1c757-152">Non-web scenarios</span></span>

<span data-ttu-id="1c757-153">Универсальный узел позволяет приложениям других типов использовать перекрестные расширения платформ, такие как средства ведения журналов, внедрения зависимостей, настройки и управления жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="1c757-153">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="1c757-154">Дополнительные сведения см. в разделах <xref:fundamentals/host/generic-host> и <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="1c757-154">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="1c757-155">Серверы</span><span class="sxs-lookup"><span data-stu-id="1c757-155">Servers</span></span>

<span data-ttu-id="1c757-156">Приложение ASP.NET Core использует реализацию HTTP-сервера для приема HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="1c757-156">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="1c757-157">Сервер отправляет приложению запросы в виде набора [функций запросов](xref:fundamentals/request-features), объединенных в `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="1c757-157">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

# <a name="windows"></a>[<span data-ttu-id="1c757-158">Windows</span><span class="sxs-lookup"><span data-stu-id="1c757-158">Windows</span></span>](#tab/windows)

<span data-ttu-id="1c757-159">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="1c757-159">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="1c757-160">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="1c757-160">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="1c757-161">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="1c757-161">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="1c757-162">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="1c757-162">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="1c757-163">*HTTP-сервер IIS* — это сервер для Windows, который использует службы IIS.</span><span class="sxs-lookup"><span data-stu-id="1c757-163">*IIS HTTP Server* is a server for Windows that uses IIS.</span></span> <span data-ttu-id="1c757-164">Он позволяет запускать приложение ASP.NET Core и службы IIS в одном процессе.</span><span class="sxs-lookup"><span data-stu-id="1c757-164">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="1c757-165">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="1c757-165">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="1c757-166">macOS</span><span class="sxs-lookup"><span data-stu-id="1c757-166">macOS</span></span>](#tab/macos)

<span data-ttu-id="1c757-167">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="1c757-167">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="1c757-168">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="1c757-168">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="1c757-169">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="1c757-169">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="1c757-170">Linux</span><span class="sxs-lookup"><span data-stu-id="1c757-170">Linux</span></span>](#tab/linux)

<span data-ttu-id="1c757-171">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="1c757-171">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="1c757-172">В ASP.NET Core 2.0 и более поздних версиях можно использовать Kestrel в роли общедоступного пограничного сервера с прямым доступом через Интернет.</span><span class="sxs-lookup"><span data-stu-id="1c757-172">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="1c757-173">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="1c757-173">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

<span data-ttu-id="1c757-174">Для получения дополнительной информации см. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="1c757-174">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="1c757-175">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="1c757-175">Configuration</span></span>

<span data-ttu-id="1c757-176">ASP.NET Core предоставляет платформу конфигурации, которая получает параметры в виде пар "имя-значение" от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1c757-176">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="1c757-177">Доступны встроенные поставщики конфигурации для различных источников, таких как файлы *JSON*, *XML*, переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="1c757-177">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="1c757-178">Кроме того, вы можете писать поставщики конфигурации сами.</span><span class="sxs-lookup"><span data-stu-id="1c757-178">You can also write custom configuration providers.</span></span>

<span data-ttu-id="1c757-179">Например, можно указать, что источником конфигурации являются файл *appsettings.json* и переменные среды.</span><span class="sxs-lookup"><span data-stu-id="1c757-179">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="1c757-180">В этом случае при запросе значения *ConnectionString* платформа сначала выполнит поиск в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1c757-180">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="1c757-181">Если значение будет найдено не только в нем, но и в переменной среды, приоритет получит значение из переменной среды.</span><span class="sxs-lookup"><span data-stu-id="1c757-181">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="1c757-182">Для управления конфиденциальными данными конфигурации, например паролями, ASP.NET Core предоставляет специальный инструмент [Менеджер секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="1c757-182">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="1c757-183">Для секретов в рабочей среде рекомендуется использовать [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="1c757-183">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="1c757-184">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1c757-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="1c757-185">Параметры</span><span class="sxs-lookup"><span data-stu-id="1c757-185">Options</span></span>

<span data-ttu-id="1c757-186">Когда это возможно, ASP.NET Core следует *шаблону параметров* для хранения и получения значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1c757-186">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="1c757-187">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="1c757-187">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="1c757-188">Например, следующий код задает параметры WebSockets:</span><span class="sxs-lookup"><span data-stu-id="1c757-188">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="1c757-189">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1c757-189">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="1c757-190">Среды</span><span class="sxs-lookup"><span data-stu-id="1c757-190">Environments</span></span>

<span data-ttu-id="1c757-191">Среды выполнения, включая среду *разработки*, *промежуточную* и *рабочую* среды, являются ключевыми компонентами ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c757-191">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="1c757-192">Указать среду, в которой запускается приложение, можно с помощью переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1c757-192">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="1c757-193">ASP.NET Core считывает переменную среды при запуске приложения и сохраняет ее значение в реализации `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="1c757-193">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="1c757-194">Объект среды можно получить в любом месте приложения при помощи внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1c757-194">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="1c757-195">В следующем примере кода из класса `Startup` показана настройка приложения для вывода подробных сведений об ошибках исключительно при запуске в среде разработки:</span><span class="sxs-lookup"><span data-stu-id="1c757-195">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="1c757-196">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1c757-196">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="1c757-197">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="1c757-197">Logging</span></span>

<span data-ttu-id="1c757-198">ASP.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками.</span><span class="sxs-lookup"><span data-stu-id="1c757-198">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="1c757-199">Доступны следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="1c757-199">Available providers include the following:</span></span>

* <span data-ttu-id="1c757-200">Консоль</span><span class="sxs-lookup"><span data-stu-id="1c757-200">Console</span></span>
* <span data-ttu-id="1c757-201">Отладка</span><span class="sxs-lookup"><span data-stu-id="1c757-201">Debug</span></span>
* <span data-ttu-id="1c757-202">Трассировка событий Windows</span><span class="sxs-lookup"><span data-stu-id="1c757-202">Event Tracing on Windows</span></span>
* <span data-ttu-id="1c757-203">Журнал событий Windows</span><span class="sxs-lookup"><span data-stu-id="1c757-203">Windows Event Log</span></span>
* <span data-ttu-id="1c757-204">TraceSource</span><span class="sxs-lookup"><span data-stu-id="1c757-204">TraceSource</span></span>
* <span data-ttu-id="1c757-205">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="1c757-205">Azure App Service</span></span>
* <span data-ttu-id="1c757-206">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="1c757-206">Azure Application Insights</span></span>

<span data-ttu-id="1c757-207">Записи в журналы можно вносить в любом месте кода приложения. Для этого нужно получить объект `ILogger` из внедрения зависимостей и вызвать методы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="1c757-207">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="1c757-208">Ниже приведен пример кода, где используется объект `ILogger`. В коде выделены строки внедрения конструктора и вызова методов ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="1c757-208">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="1c757-209">Интерфейс `ILogger` позволяет передавать в поставщик ведения журналов любое количество полей.</span><span class="sxs-lookup"><span data-stu-id="1c757-209">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="1c757-210">Как правило, поля используются для создания строки сообщения, но поставщик также может отправлять их в хранилище данных в виде отдельных полей.</span><span class="sxs-lookup"><span data-stu-id="1c757-210">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="1c757-211">Благодаря этому поставщики могут реализовывать [семантическое (структурированное) ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="1c757-211">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="1c757-212">Для получения дополнительной информации см. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="1c757-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="1c757-213">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="1c757-213">Routing</span></span>

<span data-ttu-id="1c757-214">*Маршрут* — это шаблон URL-адреса, сопоставляемый с обработчиком.</span><span class="sxs-lookup"><span data-stu-id="1c757-214">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="1c757-215">Обычно обработчик представляет собой страницу Razor, метод действия в контроллере MVC или ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="1c757-215">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="1c757-216">Маршрутизация ASP.NET Core позволяет контролировать URL-адреса, используемые приложением.</span><span class="sxs-lookup"><span data-stu-id="1c757-216">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="1c757-217">Для получения дополнительной информации см. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="1c757-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="1c757-218">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="1c757-218">Error handling</span></span>

<span data-ttu-id="1c757-219">ASP.NET Core содержит встроенные функции обработки ошибок, такие как:</span><span class="sxs-lookup"><span data-stu-id="1c757-219">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="1c757-220">Страница исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="1c757-220">A developer exception page</span></span>
* <span data-ttu-id="1c757-221">Настраиваемые страницы ошибок</span><span class="sxs-lookup"><span data-stu-id="1c757-221">Custom error pages</span></span>
* <span data-ttu-id="1c757-222">Статические страницы с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="1c757-222">Static status code pages</span></span>
* <span data-ttu-id="1c757-223">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="1c757-223">Startup exception handling</span></span>

<span data-ttu-id="1c757-224">Для получения дополнительной информации см. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="1c757-224">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="1c757-225">Создание HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="1c757-225">Make HTTP requests</span></span>

<span data-ttu-id="1c757-226">ASP.NET Core включает реализацию интерфейса `IHttpClientFactory` для создания экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1c757-226">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="1c757-227">Фабрика:</span><span class="sxs-lookup"><span data-stu-id="1c757-227">The factory:</span></span>

* <span data-ttu-id="1c757-228">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1c757-228">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="1c757-229">Например, можно зарегистрировать и использовать клиент *github* для доступа к GitHub.</span><span class="sxs-lookup"><span data-stu-id="1c757-229">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="1c757-230">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="1c757-230">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="1c757-231">Поддержка регистрации и связывания в цепочки множества делегирующих обработчиков для создания конвейера ПО промежуточного слоя под исходящие запросы.</span><span class="sxs-lookup"><span data-stu-id="1c757-231">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="1c757-232">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c757-232">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="1c757-233">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="1c757-233">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="1c757-234">Интеграция с *Polly* — популярной сторонней библиотекой для обработки временных сбоев.</span><span class="sxs-lookup"><span data-stu-id="1c757-234">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="1c757-235">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="1c757-235">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="1c757-236">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="1c757-236">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="1c757-237">Для получения дополнительной информации см. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="1c757-237">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="1c757-238">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="1c757-238">Content root</span></span>

<span data-ttu-id="1c757-239">Корневой каталог содержимого — это базовый путь к таким элементам:</span><span class="sxs-lookup"><span data-stu-id="1c757-239">The content root is the base path to the:</span></span>

* <span data-ttu-id="1c757-240">Исполняемый файл, в котором размещено приложение (*EXE*).</span><span class="sxs-lookup"><span data-stu-id="1c757-240">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="1c757-241">Скомпилированные сборки, составляющие приложение (*DLL*).</span><span class="sxs-lookup"><span data-stu-id="1c757-241">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="1c757-242">Файлы содержимого, не относящиеся к коду, используемые приложением, например:</span><span class="sxs-lookup"><span data-stu-id="1c757-242">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="1c757-243">Файлы Razor (*CSHTML*, *RAZOR*).</span><span class="sxs-lookup"><span data-stu-id="1c757-243">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="1c757-244">Файлы конфигурации (*JSON*, *XML*).</span><span class="sxs-lookup"><span data-stu-id="1c757-244">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="1c757-245">Файлы данных (*DB*).</span><span class="sxs-lookup"><span data-stu-id="1c757-245">Data files (*.db*)</span></span>
* <span data-ttu-id="1c757-246">[Корневой веб-каталог](#web-root), обычно опубликованная папка *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="1c757-246">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="1c757-247">Во время разработки:</span><span class="sxs-lookup"><span data-stu-id="1c757-247">During development:</span></span>

* <span data-ttu-id="1c757-248">Корень содержимого по умолчанию сбрасывается до корневого каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="1c757-248">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="1c757-249">Корневой каталог проекта используется для создания:</span><span class="sxs-lookup"><span data-stu-id="1c757-249">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="1c757-250">пути к файлам содержимого, не являющихся кодом приложения, в корневом каталоге проекта;</span><span class="sxs-lookup"><span data-stu-id="1c757-250">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="1c757-251">[корневого веб-каталога](#web-root), обычно папка *wwwroot* в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="1c757-251">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="1c757-252">Альтернативный корневой путь к содержимому может быть указан при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="1c757-252">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="1c757-253">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="1c757-253">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

## <a name="web-root"></a><span data-ttu-id="1c757-254">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="1c757-254">Web root</span></span>

<span data-ttu-id="1c757-255">Корневой веб-каталог — это базовый путь к общедоступным, не кодовым файлам статических ресурсов, например:</span><span class="sxs-lookup"><span data-stu-id="1c757-255">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="1c757-256">Таблицы стилей (*CSS*).</span><span class="sxs-lookup"><span data-stu-id="1c757-256">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="1c757-257">JavaScript (*JS*).</span><span class="sxs-lookup"><span data-stu-id="1c757-257">JavaScript (*.js*)</span></span>
* <span data-ttu-id="1c757-258">Изображения (*PNG*, *JPG*).</span><span class="sxs-lookup"><span data-stu-id="1c757-258">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="1c757-259">Статические файлы обслуживаются по умолчанию только в корневом веб-каталоге (и подкаталогах).</span><span class="sxs-lookup"><span data-stu-id="1c757-259">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="1c757-260">По умолчанию используется путь *{корневой каталог содержимого}/wwwroot*. Альтернативное расположение можно задать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="1c757-260">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="1c757-261">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="1c757-261">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

<span data-ttu-id="1c757-262">Запретите публикацию файлов в *wwwroot* с помощью [\<элемента проекта Content>](/visualstudio/msbuild/common-msbuild-project-items#content) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="1c757-262">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="1c757-263">В следующем примере запрещается публикация содержимого в каталоге *wwwroot/local* и подкаталогах:</span><span class="sxs-lookup"><span data-stu-id="1c757-263">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1c757-264">Сведения о запрете публикации ресурсов статических удостоверений в корневом веб-каталоге см. в разделе <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="1c757-264">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

<span data-ttu-id="1c757-265">Для указания на корневой каталог файлов Razor (*CSHTML*) используется символ тильды и косой черты `~/`.</span><span class="sxs-lookup"><span data-stu-id="1c757-265">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="1c757-266">Путь, начинающийся с `~/`, называется *виртуальным путем*.</span><span class="sxs-lookup"><span data-stu-id="1c757-266">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="1c757-267">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="1c757-267">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c757-268">В этой статье приведен обзор основных тем, связанных с разработкой приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c757-268">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="1c757-269">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="1c757-269">The Startup class</span></span>

<span data-ttu-id="1c757-270">В классе `Startup` делается следующее.</span><span class="sxs-lookup"><span data-stu-id="1c757-270">The `Startup` class is where:</span></span>

* <span data-ttu-id="1c757-271">Настраиваются службы, необходимые приложению.</span><span class="sxs-lookup"><span data-stu-id="1c757-271">Services required by the app are configured.</span></span>
* <span data-ttu-id="1c757-272">Определяется конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="1c757-272">The request handling pipeline is defined.</span></span>

<span data-ttu-id="1c757-273">*Службы* — это компоненты, которые используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="1c757-273">*Services* are components that are used by the app.</span></span> <span data-ttu-id="1c757-274">Например, службой является компонент ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="1c757-274">For example, a logging component is a service.</span></span> <span data-ttu-id="1c757-275">Код для настройки (или *регистрации*) служб добавляется в метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1c757-275">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="1c757-276">Конвейер обработки запросов состоит из ряда компонентов *ПО промежуточного* слоя.</span><span class="sxs-lookup"><span data-stu-id="1c757-276">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="1c757-277">Например, это ПО может обрабатывать запросы для статических файлов или перенаправлять HTTP-запросы в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1c757-277">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="1c757-278">Каждый компонент ПО промежуточного слоя выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="1c757-278">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="1c757-279">Код для настройки конвейера обработки запросов добавляется в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="1c757-279">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="1c757-280">Пример класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="1c757-280">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="1c757-281">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="1c757-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="1c757-282">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="1c757-282">Dependency injection (services)</span></span>

<span data-ttu-id="1c757-283">ASP.NET Core включает встроенную платформу внедрения зависимостей, позволяющую классам приложения обращаться к настроенным службам.</span><span class="sxs-lookup"><span data-stu-id="1c757-283">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="1c757-284">Одним из вариантов получения экземпляра службы в классе является создание конструктора с параметром требуемого типа.</span><span class="sxs-lookup"><span data-stu-id="1c757-284">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="1c757-285">Параметр может быть типом службы или интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="1c757-285">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="1c757-286">Система внедрения зависимостей предоставляет службу во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="1c757-286">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="1c757-287">Ниже показан класс, который использует внедрение зависимостей для получения объекта контекста Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1c757-287">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="1c757-288">Выделенная строка является примером внедрения через конструктор.</span><span class="sxs-lookup"><span data-stu-id="1c757-288">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="1c757-289">Несмотря на то, что система внедрения зависимостей является встроенной, она позволяет вам при желании подключать сторонний контейнер с инверсией управления (IoC).</span><span class="sxs-lookup"><span data-stu-id="1c757-289">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="1c757-290">Для получения дополнительной информации см. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="1c757-290">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="1c757-291">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="1c757-291">Middleware</span></span>

<span data-ttu-id="1c757-292">Конвейер обработки запросов состоит из ряда компонентов ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="1c757-292">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="1c757-293">Каждый компонент выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующий компонент в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="1c757-293">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="1c757-294">По принятому соглашению компонент ПО промежуточного слоя добавляется в конвейер вызовом метода расширения `Use...` в методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="1c757-294">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="1c757-295">Например, чтобы включить отрисовку статических файлов, вызовите `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="1c757-295">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="1c757-296">Код настройки конвейера обработки запросов выделен в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1c757-296">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="1c757-297">ASP.NET Core включает обширный набор встроенного ПО промежуточного слоя и позволяет вам писать свое собственное.</span><span class="sxs-lookup"><span data-stu-id="1c757-297">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="1c757-298">Для получения дополнительной информации см. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="1c757-298">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="1c757-299">Узел</span><span class="sxs-lookup"><span data-stu-id="1c757-299">Host</span></span>

<span data-ttu-id="1c757-300">Приложение ASP.NET Core создает *узел* при запуске.</span><span class="sxs-lookup"><span data-stu-id="1c757-300">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="1c757-301">Узел — это объект, который инкапсулирует все ресурсы приложения.</span><span class="sxs-lookup"><span data-stu-id="1c757-301">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="1c757-302">Реализация HTTP-сервера</span><span class="sxs-lookup"><span data-stu-id="1c757-302">An HTTP server implementation</span></span>
* <span data-ttu-id="1c757-303">Компоненты ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="1c757-303">Middleware components</span></span>
* <span data-ttu-id="1c757-304">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="1c757-304">Logging</span></span>
* <span data-ttu-id="1c757-305">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="1c757-305">DI</span></span>
* <span data-ttu-id="1c757-306">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="1c757-306">Configuration</span></span>

<span data-ttu-id="1c757-307">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="1c757-307">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="1c757-308">Доступны два узла: веб-узел и универсальный узел.</span><span class="sxs-lookup"><span data-stu-id="1c757-308">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="1c757-309">В ASP.NET Core 2.x универсальный узел используется в сценариях, не связанных с Интернетом.</span><span class="sxs-lookup"><span data-stu-id="1c757-309">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="1c757-310">Код для создания узла находится в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="1c757-310">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="1c757-311">Метод `CreateDefaultBuilder` применяется для настройки узла с часто используемыми параметрами, например:</span><span class="sxs-lookup"><span data-stu-id="1c757-311">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="1c757-312">Использование [Kestrel](#servers) в качестве веб-сервера и поддержка интеграции с IIS.</span><span class="sxs-lookup"><span data-stu-id="1c757-312">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="1c757-313">Загрузка конфигурации из файлов *appsettings.json* и *appsettings.{имя среды}.json*, переменных среды, аргументов командной строки и других источников.</span><span class="sxs-lookup"><span data-stu-id="1c757-313">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="1c757-314">Отправка выходных данных журнала в поставщики служб консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="1c757-314">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="1c757-315">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="1c757-315">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="1c757-316">Сценарии, не связанные с Интернетом</span><span class="sxs-lookup"><span data-stu-id="1c757-316">Non-web scenarios</span></span>

<span data-ttu-id="1c757-317">Универсальный узел позволяет приложениям других типов использовать перекрестные расширения платформ, такие как средства ведения журналов, внедрения зависимостей, настройки и управления жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="1c757-317">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="1c757-318">Дополнительные сведения см. в разделах <xref:fundamentals/host/generic-host> и <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="1c757-318">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="1c757-319">Серверы</span><span class="sxs-lookup"><span data-stu-id="1c757-319">Servers</span></span>

<span data-ttu-id="1c757-320">Приложение ASP.NET Core использует реализацию HTTP-сервера для приема HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="1c757-320">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="1c757-321">Сервер отправляет приложению запросы в виде набора [функций запросов](xref:fundamentals/request-features), объединенных в `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="1c757-321">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="1c757-322">Windows</span><span class="sxs-lookup"><span data-stu-id="1c757-322">Windows</span></span>](#tab/windows)

<span data-ttu-id="1c757-323">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="1c757-323">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="1c757-324">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="1c757-324">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="1c757-325">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="1c757-325">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="1c757-326">Kestrel можно использовать в роли общедоступного пограничного сервера с прямым доступом из Интернета.</span><span class="sxs-lookup"><span data-stu-id="1c757-326">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="1c757-327">*HTTP-сервер IIS* — это сервер для Windows, который использует службы IIS.</span><span class="sxs-lookup"><span data-stu-id="1c757-327">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="1c757-328">Он позволяет запускать приложение ASP.NET Core и службы IIS в одном процессе.</span><span class="sxs-lookup"><span data-stu-id="1c757-328">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="1c757-329">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="1c757-329">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="1c757-330">macOS</span><span class="sxs-lookup"><span data-stu-id="1c757-330">macOS</span></span>](#tab/macos)

<span data-ttu-id="1c757-331">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="1c757-331">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="1c757-332">Kestrel можно использовать в роли общедоступного пограничного сервера с прямым доступом из Интернета.</span><span class="sxs-lookup"><span data-stu-id="1c757-332">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="1c757-333">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="1c757-333">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="1c757-334">Linux</span><span class="sxs-lookup"><span data-stu-id="1c757-334">Linux</span></span>](#tab/linux)

<span data-ttu-id="1c757-335">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="1c757-335">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="1c757-336">Kestrel можно использовать в роли общедоступного пограничного сервера с прямым доступом из Интернета.</span><span class="sxs-lookup"><span data-stu-id="1c757-336">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="1c757-337">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="1c757-337">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="1c757-338">Windows</span><span class="sxs-lookup"><span data-stu-id="1c757-338">Windows</span></span>](#tab/windows)

<span data-ttu-id="1c757-339">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="1c757-339">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="1c757-340">*Kestrel* — это кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="1c757-340">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="1c757-341">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="1c757-341">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="1c757-342">Kestrel можно использовать в роли общедоступного пограничного сервера с прямым доступом из Интернета.</span><span class="sxs-lookup"><span data-stu-id="1c757-342">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="1c757-343">*HTTP.sys* — это сервер для Windows, который не используется со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="1c757-343">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="1c757-344">macOS</span><span class="sxs-lookup"><span data-stu-id="1c757-344">macOS</span></span>](#tab/macos)

<span data-ttu-id="1c757-345">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="1c757-345">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="1c757-346">Kestrel можно использовать в роли общедоступного пограничного сервера с прямым доступом из Интернета.</span><span class="sxs-lookup"><span data-stu-id="1c757-346">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="1c757-347">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="1c757-347">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="1c757-348">Linux</span><span class="sxs-lookup"><span data-stu-id="1c757-348">Linux</span></span>](#tab/linux)

<span data-ttu-id="1c757-349">ASP.NET Core предоставляет реализацию кроссплатформенного сервера *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="1c757-349">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="1c757-350">Kestrel можно использовать в роли общедоступного пограничного сервера с прямым доступом из Интернета.</span><span class="sxs-lookup"><span data-stu-id="1c757-350">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="1c757-351">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="1c757-351">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c757-352">Для получения дополнительной информации см. <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="1c757-352">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="1c757-353">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="1c757-353">Configuration</span></span>

<span data-ttu-id="1c757-354">ASP.NET Core предоставляет платформу конфигурации, которая получает параметры в виде пар "имя-значение" от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1c757-354">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="1c757-355">Доступны встроенные поставщики конфигурации для различных источников, таких как файлы *JSON*, *XML*, переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="1c757-355">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="1c757-356">Кроме того, вы можете писать поставщики конфигурации сами.</span><span class="sxs-lookup"><span data-stu-id="1c757-356">You can also write custom configuration providers.</span></span>

<span data-ttu-id="1c757-357">Например, можно указать, что источником конфигурации являются файл *appsettings.json* и переменные среды.</span><span class="sxs-lookup"><span data-stu-id="1c757-357">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="1c757-358">В этом случае при запросе значения *ConnectionString* платформа сначала выполнит поиск в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1c757-358">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="1c757-359">Если значение будет найдено не только в нем, но и в переменной среды, приоритет получит значение из переменной среды.</span><span class="sxs-lookup"><span data-stu-id="1c757-359">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="1c757-360">Для управления конфиденциальными данными конфигурации, например паролями, ASP.NET Core предоставляет специальный инструмент [Менеджер секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="1c757-360">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="1c757-361">Для секретов в рабочей среде рекомендуется использовать [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="1c757-361">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="1c757-362">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1c757-362">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="1c757-363">Параметры</span><span class="sxs-lookup"><span data-stu-id="1c757-363">Options</span></span>

<span data-ttu-id="1c757-364">Когда это возможно, ASP.NET Core следует *шаблону параметров* для хранения и получения значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1c757-364">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="1c757-365">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="1c757-365">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="1c757-366">Например, следующий код задает параметры WebSockets:</span><span class="sxs-lookup"><span data-stu-id="1c757-366">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="1c757-367">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1c757-367">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="1c757-368">Среды</span><span class="sxs-lookup"><span data-stu-id="1c757-368">Environments</span></span>

<span data-ttu-id="1c757-369">Среды выполнения, включая среду *разработки*, *промежуточную* и *рабочую* среды, являются ключевыми компонентами ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c757-369">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="1c757-370">Указать среду, в которой запускается приложение, можно с помощью переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1c757-370">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="1c757-371">ASP.NET Core считывает переменную среды при запуске приложения и сохраняет ее значение в реализации `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="1c757-371">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="1c757-372">Объект среды можно получить в любом месте приложения при помощи внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1c757-372">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="1c757-373">В следующем примере кода из класса `Startup` показана настройка приложения для вывода подробных сведений об ошибках исключительно при запуске в среде разработки:</span><span class="sxs-lookup"><span data-stu-id="1c757-373">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="1c757-374">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1c757-374">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="1c757-375">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="1c757-375">Logging</span></span>

<span data-ttu-id="1c757-376">ASP.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками.</span><span class="sxs-lookup"><span data-stu-id="1c757-376">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="1c757-377">Доступны следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="1c757-377">Available providers include the following:</span></span>

* <span data-ttu-id="1c757-378">Консоль</span><span class="sxs-lookup"><span data-stu-id="1c757-378">Console</span></span>
* <span data-ttu-id="1c757-379">Отладка</span><span class="sxs-lookup"><span data-stu-id="1c757-379">Debug</span></span>
* <span data-ttu-id="1c757-380">Трассировка событий Windows</span><span class="sxs-lookup"><span data-stu-id="1c757-380">Event Tracing on Windows</span></span>
* <span data-ttu-id="1c757-381">Журнал событий Windows</span><span class="sxs-lookup"><span data-stu-id="1c757-381">Windows Event Log</span></span>
* <span data-ttu-id="1c757-382">TraceSource</span><span class="sxs-lookup"><span data-stu-id="1c757-382">TraceSource</span></span>
* <span data-ttu-id="1c757-383">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="1c757-383">Azure App Service</span></span>
* <span data-ttu-id="1c757-384">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="1c757-384">Azure Application Insights</span></span>

<span data-ttu-id="1c757-385">Записи в журналы можно вносить в любом месте кода приложения. Для этого нужно получить объект `ILogger` из внедрения зависимостей и вызвать методы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="1c757-385">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="1c757-386">Ниже приведен пример кода, где используется объект `ILogger`. В коде выделены строки внедрения конструктора и вызова методов ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="1c757-386">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="1c757-387">Интерфейс `ILogger` позволяет передавать в поставщик ведения журналов любое количество полей.</span><span class="sxs-lookup"><span data-stu-id="1c757-387">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="1c757-388">Как правило, поля используются для создания строки сообщения, но поставщик также может отправлять их в хранилище данных в виде отдельных полей.</span><span class="sxs-lookup"><span data-stu-id="1c757-388">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="1c757-389">Благодаря этому поставщики могут реализовывать [семантическое (структурированное) ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="1c757-389">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="1c757-390">Для получения дополнительной информации см. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="1c757-390">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="1c757-391">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="1c757-391">Routing</span></span>

<span data-ttu-id="1c757-392">*Маршрут* — это шаблон URL-адреса, сопоставляемый с обработчиком.</span><span class="sxs-lookup"><span data-stu-id="1c757-392">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="1c757-393">Обычно обработчик представляет собой страницу Razor, метод действия в контроллере MVC или ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="1c757-393">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="1c757-394">Маршрутизация ASP.NET Core позволяет контролировать URL-адреса, используемые приложением.</span><span class="sxs-lookup"><span data-stu-id="1c757-394">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="1c757-395">Для получения дополнительной информации см. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="1c757-395">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="1c757-396">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="1c757-396">Error handling</span></span>

<span data-ttu-id="1c757-397">ASP.NET Core содержит встроенные функции обработки ошибок, такие как:</span><span class="sxs-lookup"><span data-stu-id="1c757-397">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="1c757-398">Страница исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="1c757-398">A developer exception page</span></span>
* <span data-ttu-id="1c757-399">Настраиваемые страницы ошибок</span><span class="sxs-lookup"><span data-stu-id="1c757-399">Custom error pages</span></span>
* <span data-ttu-id="1c757-400">Статические страницы с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="1c757-400">Static status code pages</span></span>
* <span data-ttu-id="1c757-401">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="1c757-401">Startup exception handling</span></span>

<span data-ttu-id="1c757-402">Для получения дополнительной информации см. <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="1c757-402">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="1c757-403">Создание HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="1c757-403">Make HTTP requests</span></span>

<span data-ttu-id="1c757-404">ASP.NET Core включает реализацию интерфейса `IHttpClientFactory` для создания экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1c757-404">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="1c757-405">Фабрика:</span><span class="sxs-lookup"><span data-stu-id="1c757-405">The factory:</span></span>

* <span data-ttu-id="1c757-406">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1c757-406">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="1c757-407">Например, можно зарегистрировать и использовать клиент *github* для доступа к GitHub.</span><span class="sxs-lookup"><span data-stu-id="1c757-407">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="1c757-408">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="1c757-408">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="1c757-409">Поддержка регистрации и связывания в цепочки множества делегирующих обработчиков для создания конвейера ПО промежуточного слоя под исходящие запросы.</span><span class="sxs-lookup"><span data-stu-id="1c757-409">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="1c757-410">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c757-410">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="1c757-411">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="1c757-411">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="1c757-412">Интеграция с *Polly* — популярной сторонней библиотекой для обработки временных сбоев.</span><span class="sxs-lookup"><span data-stu-id="1c757-412">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="1c757-413">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="1c757-413">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="1c757-414">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="1c757-414">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="1c757-415">Для получения дополнительной информации см. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="1c757-415">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="1c757-416">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="1c757-416">Content root</span></span>

<span data-ttu-id="1c757-417">Корневой каталог содержимого — это базовый путь к таким элементам:</span><span class="sxs-lookup"><span data-stu-id="1c757-417">The content root is the base path to the:</span></span>

* <span data-ttu-id="1c757-418">Исполняемый файл, в котором размещено приложение (*EXE*).</span><span class="sxs-lookup"><span data-stu-id="1c757-418">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="1c757-419">Скомпилированные сборки, составляющие приложение (*DLL*).</span><span class="sxs-lookup"><span data-stu-id="1c757-419">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="1c757-420">Файлы содержимого, не относящиеся к коду, используемые приложением, например:</span><span class="sxs-lookup"><span data-stu-id="1c757-420">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="1c757-421">Файлы Razor (*CSHTML*, *RAZOR*).</span><span class="sxs-lookup"><span data-stu-id="1c757-421">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="1c757-422">Файлы конфигурации (*JSON*, *XML*).</span><span class="sxs-lookup"><span data-stu-id="1c757-422">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="1c757-423">Файлы данных (*DB*).</span><span class="sxs-lookup"><span data-stu-id="1c757-423">Data files (*.db*)</span></span>
* <span data-ttu-id="1c757-424">[Корневой веб-каталог](#web-root), обычно опубликованная папка *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="1c757-424">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="1c757-425">Во время разработки:</span><span class="sxs-lookup"><span data-stu-id="1c757-425">During development:</span></span>

* <span data-ttu-id="1c757-426">Корень содержимого по умолчанию сбрасывается до корневого каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="1c757-426">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="1c757-427">Корневой каталог проекта используется для создания:</span><span class="sxs-lookup"><span data-stu-id="1c757-427">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="1c757-428">пути к файлам содержимого, не являющихся кодом приложения, в корневом каталоге проекта;</span><span class="sxs-lookup"><span data-stu-id="1c757-428">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="1c757-429">[корневого веб-каталога](#web-root), обычно папка *wwwroot* в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="1c757-429">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="1c757-430">Альтернативный корневой путь к содержимому может быть указан при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="1c757-430">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="1c757-431">Для получения дополнительной информации см. <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="1c757-431">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

## <a name="web-root"></a><span data-ttu-id="1c757-432">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="1c757-432">Web root</span></span>

<span data-ttu-id="1c757-433">Корневой веб-каталог — это базовый путь к общедоступным, не кодовым файлам статических ресурсов, например:</span><span class="sxs-lookup"><span data-stu-id="1c757-433">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="1c757-434">Таблицы стилей (*CSS*).</span><span class="sxs-lookup"><span data-stu-id="1c757-434">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="1c757-435">JavaScript (*JS*).</span><span class="sxs-lookup"><span data-stu-id="1c757-435">JavaScript (*.js*)</span></span>
* <span data-ttu-id="1c757-436">Изображения (*PNG*, *JPG*).</span><span class="sxs-lookup"><span data-stu-id="1c757-436">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="1c757-437">Статические файлы обслуживаются по умолчанию только в корневом веб-каталоге (и подкаталогах).</span><span class="sxs-lookup"><span data-stu-id="1c757-437">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="1c757-438">По умолчанию используется путь *{корневой каталог содержимого}/wwwroot*. Альтернативное расположение можно задать при [создании узла](#host).</span><span class="sxs-lookup"><span data-stu-id="1c757-438">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="1c757-439">Дополнительные сведения см. в разделе [Корневой веб-каталог](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="1c757-439">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

<span data-ttu-id="1c757-440">Запретите публикацию файлов в *wwwroot* с помощью [\<элемента проекта Content>](/visualstudio/msbuild/common-msbuild-project-items#content) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="1c757-440">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="1c757-441">В следующем примере запрещается публикация содержимого в каталоге *wwwroot/local* и подкаталогах:</span><span class="sxs-lookup"><span data-stu-id="1c757-441">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1c757-442">Для указания на корневой каталог файлов Razor (*CSHTML*) используется символ тильды и косой черты `~/`.</span><span class="sxs-lookup"><span data-stu-id="1c757-442">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="1c757-443">Путь, начинающийся с `~/`, называется *виртуальным путем*.</span><span class="sxs-lookup"><span data-stu-id="1c757-443">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="1c757-444">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="1c757-444">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end
