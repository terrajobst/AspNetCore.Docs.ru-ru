---
title: "Основы ASP.NET Core"
author: rick-anderson
description: "Познакомьтесь с основными понятиями для создания приложений ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: 946ccc80915c5de60976a98cbbb253cb8dfacaca
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="2a535-103">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a535-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="2a535-104">Приложение ASP.NET Core — это консольное приложение, создающее веб-сервер в своем методе `Main`:</span><span class="sxs-lookup"><span data-stu-id="2a535-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a535-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a535-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="2a535-106">Метод `Main` вызывает `WebHost.CreateDefaultBuilder`, создающий узел веб-приложения по шаблону конструктора.</span><span class="sxs-lookup"><span data-stu-id="2a535-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="2a535-107">Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="2a535-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="2a535-108">В приведенном выше примере веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) выделяется автоматически.</span><span class="sxs-lookup"><span data-stu-id="2a535-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="2a535-109">Веб-хост ASP.NET Core попытается запуститься в службах IIS, если они доступны.</span><span class="sxs-lookup"><span data-stu-id="2a535-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="2a535-110">Другие веб-серверы, такие как [HTTP.sys](xref:fundamentals/servers/httpsys), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="2a535-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="2a535-111">`UseStartup` подробно описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="2a535-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="2a535-112">`IWebHostBuilder`, тип возвращаемого значения вызова `WebHost.CreateDefaultBuilder`, предоставляет множество вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="2a535-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="2a535-113">В число таких методов входят, например, `UseHttpSys` для размещения приложения в HTTP.sys и `UseContentRoot` для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="2a535-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="2a535-114">Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="2a535-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a535-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a535-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="2a535-116">Метод `Main` применяет `WebHostBuilder`, создающий узел веб-приложения по шаблону конструктора.</span><span class="sxs-lookup"><span data-stu-id="2a535-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="2a535-117">Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="2a535-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="2a535-118">В приведенном выше примере используется веб-сервер [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2a535-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="2a535-119">Другие веб-серверы, такие как [WebListener](xref:fundamentals/servers/weblistener), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="2a535-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="2a535-120">`UseStartup` подробно описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="2a535-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="2a535-121">В `WebHostBuilder` есть множество вспомогательных методов, включая `UseIISIntegration` для размещения в службах IIS и IIS Express и `UseContentRoot` для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="2a535-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="2a535-122">Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="2a535-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="2a535-123">Запуск</span><span class="sxs-lookup"><span data-stu-id="2a535-123">Startup</span></span>

<span data-ttu-id="2a535-124">Метод `UseStartup` в `WebHostBuilder` определяет класс `Startup` для вашего приложения:</span><span class="sxs-lookup"><span data-stu-id="2a535-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a535-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a535-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a535-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a535-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="2a535-127">В классе `Startup` определяется конвейер обработки запросов и настраиваются все необходимые для приложения службы.</span><span class="sxs-lookup"><span data-stu-id="2a535-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="2a535-128">Класс `Startup` должен быть открытым и содержать следующие методы.</span><span class="sxs-lookup"><span data-stu-id="2a535-128">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="2a535-129">`ConfigureServices` определяет [службы](#dependency-injection-services), используемые вашим приложением (такие как ASP.NET Core MVC, Entity Framework Core и Identity).</span><span class="sxs-lookup"><span data-stu-id="2a535-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="2a535-130">`Configure` определяет [ПО промежуточного слоя](xref:fundamentals/middleware) для конвейера обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="2a535-130">`Configure` defines the [middleware](xref:fundamentals/middleware) for the request pipeline.</span></span>

<span data-ttu-id="2a535-131">Дополнительные сведения см. в разделе [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="2a535-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="2a535-132">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="2a535-132">Content root</span></span>

<span data-ttu-id="2a535-133">Корневой каталог содержимого — это базовый путь к любому содержимому, которое используется приложением, включая представления, [страницы Razor](xref:mvc/razor-pages/index) и статические активы.</span><span class="sxs-lookup"><span data-stu-id="2a535-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="2a535-134">По умолчанию корневой каталог содержимого совпадает с базовым путем исполняемого файла приложения.</span><span class="sxs-lookup"><span data-stu-id="2a535-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="2a535-135">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="2a535-135">Web root</span></span>

<span data-ttu-id="2a535-136">Корневой каталог документов — это каталог в проекте, содержащий открытые и статические ресурсы, такие как CSS, JavaScript и файлы изображений.</span><span class="sxs-lookup"><span data-stu-id="2a535-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="2a535-137">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="2a535-137">Dependency Injection (Services)</span></span>

<span data-ttu-id="2a535-138">Служба — это компонент, предназначенный для общего использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="2a535-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="2a535-139">Доступ к службе предоставляется путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2a535-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2a535-140">ASP.NET Core включает встроенный контейнер для **l**инверсии **o**управления **C**(IoC), который поддерживает [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2a535-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="2a535-141">При необходимости вы можете заменить собственный контейнер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2a535-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="2a535-142">Помимо формирования слабых взаимосвязей, внедрение зависимостей обеспечивает доступ к службам в рамках всего приложения (например, [ведение журнала](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="2a535-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="2a535-143">Дополнительные сведения см. в разделе [Введение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2a535-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="2a535-144">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="2a535-144">Middleware</span></span>

<span data-ttu-id="2a535-145">В ASP.NET Core конвейер запросов создается с помощью [ПО промежуточного слоя](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="2a535-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="2a535-146">ПО промежуточного слоя ASP.NET Core выполняет асинхронную логику в `HttpContext`, а затем либо вызывает следующее ПО промежуточного слоя в последовательности, либо завершает запрос напрямую.</span><span class="sxs-lookup"><span data-stu-id="2a535-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="2a535-147">Компонент ПО промежуточного слоя с именем "XYZ" добавляется путем вызова метода расширения `UseXYZ` в методе `Configure`.</span><span class="sxs-lookup"><span data-stu-id="2a535-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="2a535-148">ASP.NET Core содержит большой набор встроенных ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="2a535-148">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="2a535-149">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="2a535-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="2a535-150">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="2a535-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="2a535-151">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="2a535-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="2a535-152">ПО промежуточного слоя для сжатия ответов</span><span class="sxs-lookup"><span data-stu-id="2a535-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="2a535-153">ПО промежуточного слоя для переопределения URL-адресов</span><span class="sxs-lookup"><span data-stu-id="2a535-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="2a535-154">В приложениях ASP.NET Core можно использовать любое ПО промежуточного слоя на базе [OWIN](http://owin.org), а также писать собственные, пользовательские ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="2a535-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="2a535-155">Дополнительные сведения см. в статьях [ПО промежуточного слоя](xref:fundamentals/middleware) и [Открытый веб-интерфейс .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="2a535-155">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="2a535-156">Среды</span><span class="sxs-lookup"><span data-stu-id="2a535-156">Environments</span></span>

<span data-ttu-id="2a535-157">Окружения для разработки и работы представляют собой ключевые компоненты ASP.NET Core и могут задаваться с использованием соответствующих переменных среды.</span><span class="sxs-lookup"><span data-stu-id="2a535-157">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="2a535-158">Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="2a535-158">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="2a535-159">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="2a535-159">Configuration</span></span>

<span data-ttu-id="2a535-160">ASP.NET Core использует модель конфигурации на основе пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="2a535-160">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="2a535-161">Модель конфигурации не основана на `System.Configuration` или *web.config*. Конфигурация получает параметры от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="2a535-161">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="2a535-162">Встроенные поставщики конфигурации поддерживают различные форматы файлов (XML, JSON, INI) и переменных среды для выполнения конфигурации на основе среды.</span><span class="sxs-lookup"><span data-stu-id="2a535-162">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="2a535-163">Кроме того, можно писать и собственные, пользовательские поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="2a535-163">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="2a535-164">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="2a535-164">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="2a535-165">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="2a535-165">Logging</span></span>

<span data-ttu-id="2a535-166">ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="2a535-166">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="2a535-167">Встроенные поставщики поддерживают отправку журналов в одно расположение или несколько.</span><span class="sxs-lookup"><span data-stu-id="2a535-167">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="2a535-168">Можно также использовать сторонние платформы ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="2a535-168">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="2a535-169">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="2a535-169">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="2a535-170">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="2a535-170">Error handling</span></span>

<span data-ttu-id="2a535-171">ASP.NET Core включает встроенные функции для обработки ошибок в приложениях, включая страницу исключений разработчика, настраиваемые страницы ошибок, страницы статических кодов состояний и обработку исключений при запуске.</span><span class="sxs-lookup"><span data-stu-id="2a535-171">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="2a535-172">Дополнительные сведения см. в разделе [Обработка ошибок](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="2a535-172">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="2a535-173">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="2a535-173">Routing</span></span>

<span data-ttu-id="2a535-174">В ASP.NET Core присутствуют функции для маршрутизации запросов приложений обработчикам маршрутов.</span><span class="sxs-lookup"><span data-stu-id="2a535-174">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="2a535-175">Дополнительные сведения см. в разделе [Маршрутизация](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="2a535-175">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="2a535-176">Поставщики файлов</span><span class="sxs-lookup"><span data-stu-id="2a535-176">File providers</span></span>

<span data-ttu-id="2a535-177">ASP.NET Core ограничивает доступ к файловой системе с помощью поставщиков файлов, которые предоставляют общий интерфейс для работы с файлами на разных платформах.</span><span class="sxs-lookup"><span data-stu-id="2a535-177">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="2a535-178">Дополнительные сведения см. в разделе [Поставщики файлов](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="2a535-178">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="2a535-179">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="2a535-179">Static files</span></span>

<span data-ttu-id="2a535-180">ПО промежуточного слоя для статических файлов обрабатывает такие статические файлы, как HTML, CSS, файлы изображений и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a535-180">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="2a535-181">Дополнительные сведения см. в разделе [Работа со статическими файлами](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="2a535-181">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="2a535-182">Размещение</span><span class="sxs-lookup"><span data-stu-id="2a535-182">Hosting</span></span>

<span data-ttu-id="2a535-183">Приложения ASP.NET Core настраивают и запускают *хост*, который отвечает за запуск приложений и управление жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="2a535-183">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="2a535-184">Дополнительные сведения см. в разделе [Размещение](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="2a535-184">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="2a535-185">Состояние сеанса и приложения</span><span class="sxs-lookup"><span data-stu-id="2a535-185">Session and application state</span></span>

<span data-ttu-id="2a535-186">Состояние сеанса — это функция в ASP.NET Core, которую можно использовать для сохранения пользовательских данных во время просмотра веб-приложения пользователем.</span><span class="sxs-lookup"><span data-stu-id="2a535-186">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="2a535-187">Дополнительные сведения см. в разделе [Состояние сеанса и приложения](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="2a535-187">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="2a535-188">Серверы</span><span class="sxs-lookup"><span data-stu-id="2a535-188">Servers</span></span>

<span data-ttu-id="2a535-189">Модель размещения ASP.NET Core не прослушивает запросы напрямую.</span><span class="sxs-lookup"><span data-stu-id="2a535-189">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="2a535-190">Модель размещения полагается на реализацию HTTP-сервера для переадресации запроса в приложение.</span><span class="sxs-lookup"><span data-stu-id="2a535-190">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="2a535-191">Переадресованный запрос упаковывается как набор объектов функций, доступ к которым можно получить через интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="2a535-191">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="2a535-192">ASP.NET Core включает управляемый кроссплатформенный веб-сервер под названием [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2a535-192">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="2a535-193">Kestrel часто используется как звено, стоящее позади рабочего веб-сервера, такого как [IIS](https://www.iis.net/) или [nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="2a535-193">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="2a535-194">Kestrel можно запускать как пограничный сервер.</span><span class="sxs-lookup"><span data-stu-id="2a535-194">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="2a535-195">Дополнительные сведения см. в разделе [Серверы](xref:fundamentals/servers/index) и в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="2a535-195">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="2a535-196">Kestrel</span><span class="sxs-lookup"><span data-stu-id="2a535-196">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="2a535-197">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a535-197">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="2a535-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (ранее — [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="2a535-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="2a535-199">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="2a535-199">Globalization and localization</span></span>

<span data-ttu-id="2a535-200">Создание многоязычного веб-сайта на основе ASP.NET Core позволяет расширить аудиторию.</span><span class="sxs-lookup"><span data-stu-id="2a535-200">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="2a535-201">ASP.NET Core предоставляет службы и ПО промежуточного слоя для локализации на разные языки и для разных региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="2a535-201">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="2a535-202">Дополнительные сведения см. в разделе [Глобализация и локализация](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="2a535-202">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="2a535-203">Параметры запроса</span><span class="sxs-lookup"><span data-stu-id="2a535-203">Request features</span></span>

<span data-ttu-id="2a535-204">Сведения о реализации веб-сервера, связанные с HTTP-запросами и ответами, определяются в интерфейсах.</span><span class="sxs-lookup"><span data-stu-id="2a535-204">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="2a535-205">Эти интерфейсы используются реализациями сервера и ПО промежуточного слоя для создания и изменения конвейера размещения приложений.</span><span class="sxs-lookup"><span data-stu-id="2a535-205">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="2a535-206">Дополнительные сведения см. в разделе [Параметры запроса](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="2a535-206">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="2a535-207">Открытый веб-интерфейс для .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="2a535-207">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="2a535-208">ASP.NET Core поддерживает открытый веб-интерфейс для .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="2a535-208">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="2a535-209">OWIN позволяет ослабить зависимость веб-приложений от веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="2a535-209">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="2a535-210">Дополнительные сведения см. в разделе [Открытый веб-интерфейс для .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="2a535-210">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="2a535-211">Протокол WebSocket</span><span class="sxs-lookup"><span data-stu-id="2a535-211">WebSockets</span></span>

<span data-ttu-id="2a535-212">[WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям.</span><span class="sxs-lookup"><span data-stu-id="2a535-212">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="2a535-213">Он используется для таких приложений, как чаты, биржевые тикеры, игры, а также везде, где в веб-приложении нужны функции реального времени.</span><span class="sxs-lookup"><span data-stu-id="2a535-213">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="2a535-214">ASP.NET Core поддерживает функции WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2a535-214">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="2a535-215">Дополнительные сведения см. в разделе [Протокол WebSocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="2a535-215">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="2a535-216">Метапакет Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="2a535-216">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="2a535-217">Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:</span><span class="sxs-lookup"><span data-stu-id="2a535-217">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="2a535-218">все пакеты, поддерживаемые командой ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="2a535-218">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="2a535-219">все пакеты, поддерживаемые Entity Framework Core;</span><span class="sxs-lookup"><span data-stu-id="2a535-219">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="2a535-220">внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2a535-220">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="2a535-221">Дополнительные сведения см. в разделе [Метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="2a535-221">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="2a535-222">Выбор между .NET Core и .NET Framework</span><span class="sxs-lookup"><span data-stu-id="2a535-222">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="2a535-223">Приложения ASP.NET Core могут задавать среду выполнения .NET Core или .NET Framework как целевую.</span><span class="sxs-lookup"><span data-stu-id="2a535-223">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="2a535-224">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="2a535-224">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="2a535-225">Выбор между ASP.NET Core и ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2a535-225">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="2a535-226">Дополнительные сведения о выборе между ASP.NET Core и ASP.NET см. в разделе [Выбор между ASP.NET Core и ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="2a535-226">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
