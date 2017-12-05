---
title: "Основы ASP.NET Core"
author: rick-anderson
description: "Познакомьтесь с основными понятиями для создания приложений ASP.NET Core."
keywords: "ASP.NET Core, основы, обзор"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 83bed4676be3ca752442da3fe560f1f2a4d728a1
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="d8a6d-104">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8a6d-104">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="d8a6d-105">Приложение ASP.NET Core — это консольное приложение, создающее веб-сервер в своем методе `Main`:</span><span class="sxs-lookup"><span data-stu-id="d8a6d-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d8a6d-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d8a6d-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="d8a6d-107">Метод `Main` вызывает `WebHost.CreateDefaultBuilder`, создающий узел веб-приложения по шаблону конструктора.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-107">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d8a6d-108">Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-108">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d8a6d-109">В приведенном выше примере веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) выделяется автоматически.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-109">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="d8a6d-110">Веб-хост ASP.NET Core попытается запуститься в службах IIS, если они доступны.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-110">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="d8a6d-111">Другие веб-серверы, такие как [HTTP.sys](xref:fundamentals/servers/httpsys), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-111">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d8a6d-112">`UseStartup` подробно описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-112">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d8a6d-113">`IWebHostBuilder`, тип возвращаемого значения вызова `WebHost.CreateDefaultBuilder`, предоставляет множество вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-113">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="d8a6d-114">В число таких методов входят, например, `UseHttpSys` для размещения приложения в HTTP.sys и `UseContentRoot` для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-114">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d8a6d-115">Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-115">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d8a6d-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d8a6d-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="d8a6d-117">Метод `Main` применяет `WebHostBuilder`, создающий узел веб-приложения по шаблону конструктора.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-117">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d8a6d-118">Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-118">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d8a6d-119">В приведенном выше примере используется веб-сервер [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-119">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="d8a6d-120">Другие веб-серверы, такие как [WebListener](xref:fundamentals/servers/weblistener), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-120">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d8a6d-121">`UseStartup` подробно описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-121">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d8a6d-122">В `WebHostBuilder` есть множество вспомогательных методов, включая `UseIISIntegration` для размещения в службах IIS и IIS Express и `UseContentRoot` для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-122">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d8a6d-123">Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-123">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="d8a6d-124">Запуск</span><span class="sxs-lookup"><span data-stu-id="d8a6d-124">Startup</span></span>

<span data-ttu-id="d8a6d-125">Метод `UseStartup` в `WebHostBuilder` определяет класс `Startup` для вашего приложения:</span><span class="sxs-lookup"><span data-stu-id="d8a6d-125">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d8a6d-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d8a6d-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d8a6d-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d8a6d-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="d8a6d-128">В классе `Startup` определяется конвейер обработки запросов и настраиваются все необходимые для приложения службы.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-128">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="d8a6d-129">Класс `Startup` должен быть открытым и содержать следующие методы.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-129">The `Startup` class must be public and contain the following methods:</span></span>

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

<span data-ttu-id="d8a6d-130">`ConfigureServices` определяет [службы](#dependency-injection-services), используемые вашим приложением (такие как ASP.NET Core MVC, Entity Framework Core и Identity).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-130">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="d8a6d-131">`Configure` определяет [ПО промежуточного слоя](xref:fundamentals/middleware) для конвейера обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-131">`Configure` defines the [middleware](xref:fundamentals/middleware) for the request pipeline.</span></span>

<span data-ttu-id="d8a6d-132">Дополнительные сведения см. в разделе [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-132">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="d8a6d-133">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="d8a6d-133">Content root</span></span>

<span data-ttu-id="d8a6d-134">Корневой каталог содержимого — это базовый путь к любому содержимому, которое используется приложением, включая представления, [страницы Razor](xref:mvc/razor-pages/index) и статические активы.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-134">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="d8a6d-135">По умолчанию корневой каталог содержимого совпадает с базовым путем исполняемого файла приложения.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-135">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="d8a6d-136">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="d8a6d-136">Web root</span></span>

<span data-ttu-id="d8a6d-137">Корневой каталог документов — это каталог в проекте, содержащий открытые и статические ресурсы, такие как CSS, JavaScript и файлы изображений.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-137">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="d8a6d-138">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="d8a6d-138">Dependency Injection (Services)</span></span>

<span data-ttu-id="d8a6d-139">Служба — это компонент, предназначенный для общего использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-139">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="d8a6d-140">Доступ к службе предоставляется путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-140">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d8a6d-141">ASP.NET Core включает собственный **я**nversion **o**f **C**(IoC) непечатаемые контейнер, который поддерживает [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-141">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="d8a6d-142">При необходимости вы можете заменить собственный контейнер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-142">You can replace the default native container if you wish.</span></span> <span data-ttu-id="d8a6d-143">Помимо формирования слабых взаимосвязей, внедрение зависимостей обеспечивает доступ к службам в рамках всего приложения (например, [ведение журнала](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-143">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="d8a6d-144">Дополнительные сведения см. в разделе [Введение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="d8a6d-145">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="d8a6d-145">Middleware</span></span>

<span data-ttu-id="d8a6d-146">В ASP.NET Core конвейер запросов создается с помощью [ПО промежуточного слоя](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-146">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="d8a6d-147">ПО промежуточного слоя ASP.NET Core выполняет асинхронную логику в `HttpContext`, а затем либо вызывает следующее ПО промежуточного слоя в последовательности, либо завершает запрос напрямую.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="d8a6d-148">Компонент ПО промежуточного слоя с именем "XYZ" добавляется путем вызова метода расширения `UseXYZ` в методе `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-148">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="d8a6d-149">ASP.NET Core содержит большой набор встроенных ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="d8a6d-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="d8a6d-150">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="d8a6d-150">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="d8a6d-151">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="d8a6d-151">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="d8a6d-152">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="d8a6d-152">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="d8a6d-153">ПО промежуточного слоя для сжатия ответов</span><span class="sxs-lookup"><span data-stu-id="d8a6d-153">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="d8a6d-154">ПО промежуточного слоя для переопределения URL-адресов</span><span class="sxs-lookup"><span data-stu-id="d8a6d-154">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="d8a6d-155">В приложениях ASP.NET Core можно использовать любое ПО промежуточного слоя на базе [OWIN](http://owin.org), а также писать собственные, пользовательские ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-155">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="d8a6d-156">Дополнительные сведения см. в статьях [ПО промежуточного слоя](xref:fundamentals/middleware) и [Открытый веб-интерфейс .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-156">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="d8a6d-157">Среды</span><span class="sxs-lookup"><span data-stu-id="d8a6d-157">Environments</span></span>

<span data-ttu-id="d8a6d-158">Окружения для разработки и работы представляют собой ключевые компоненты ASP.NET Core и могут задаваться с использованием соответствующих переменных среды.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-158">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="d8a6d-159">Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-159">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="d8a6d-160">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="d8a6d-160">Configuration</span></span>

<span data-ttu-id="d8a6d-161">ASP.NET Core использует модель конфигурации на основе пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="d8a6d-161">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="d8a6d-162">Модель конфигурации не основана на `System.Configuration` или *web.config*. Конфигурация получает параметры от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-162">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="d8a6d-163">Встроенные поставщики конфигурации поддерживают различные форматы файлов (XML, JSON, INI) и переменных среды для выполнения конфигурации на основе среды.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-163">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="d8a6d-164">Кроме того, можно писать и собственные, пользовательские поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-164">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="d8a6d-165">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-165">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="d8a6d-166">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="d8a6d-166">Logging</span></span>

<span data-ttu-id="d8a6d-167">ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-167">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="d8a6d-168">Встроенные поставщики поддерживают отправку журналов в одно расположение или несколько.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-168">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="d8a6d-169">Можно также использовать сторонние платформы ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-169">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="d8a6d-170">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="d8a6d-170">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="d8a6d-171">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="d8a6d-171">Error handling</span></span>

<span data-ttu-id="d8a6d-172">ASP.NET Core включает встроенные функции для обработки ошибок в приложениях, включая страницу исключений разработчика, настраиваемые страницы ошибок, страницы статических кодов состояний и обработку исключений при запуске.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-172">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="d8a6d-173">Дополнительные сведения см. в разделе [Обработка ошибок](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-173">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="d8a6d-174">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="d8a6d-174">Routing</span></span>

<span data-ttu-id="d8a6d-175">В ASP.NET Core присутствуют функции для маршрутизации запросов приложений обработчикам маршрутов.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-175">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="d8a6d-176">Дополнительные сведения см. в разделе [Маршрутизация](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-176">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="d8a6d-177">Поставщики файлов</span><span class="sxs-lookup"><span data-stu-id="d8a6d-177">File providers</span></span>

<span data-ttu-id="d8a6d-178">ASP.NET Core ограничивает доступ к файловой системе с помощью поставщиков файлов, которые предоставляют общий интерфейс для работы с файлами на разных платформах.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-178">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="d8a6d-179">Дополнительные сведения см. в разделе [Поставщики файлов](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-179">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="d8a6d-180">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="d8a6d-180">Static files</span></span>

<span data-ttu-id="d8a6d-181">ПО промежуточного слоя для статических файлов обрабатывает такие статические файлы, как HTML, CSS, файлы изображений и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-181">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="d8a6d-182">Дополнительные сведения см. в разделе [Работа со статическими файлами](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-182">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="d8a6d-183">Размещение</span><span class="sxs-lookup"><span data-stu-id="d8a6d-183">Hosting</span></span>

<span data-ttu-id="d8a6d-184">Приложения ASP.NET Core настраивают и запускают *хост*, который отвечает за запуск приложений и управление жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-184">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="d8a6d-185">Дополнительные сведения см. в разделе [Размещение](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-185">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="d8a6d-186">Состояние сеанса и приложения</span><span class="sxs-lookup"><span data-stu-id="d8a6d-186">Session and application state</span></span>

<span data-ttu-id="d8a6d-187">Состояние сеанса — это функция в ASP.NET Core, которую можно использовать для сохранения пользовательских данных во время просмотра веб-приложения пользователем.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-187">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="d8a6d-188">Дополнительные сведения см. в разделе [Состояние сеанса и приложения](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-188">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="d8a6d-189">Серверы</span><span class="sxs-lookup"><span data-stu-id="d8a6d-189">Servers</span></span>

<span data-ttu-id="d8a6d-190">Модель размещения ASP.NET Core не прослушивает запросы напрямую.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-190">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="d8a6d-191">Модель размещения полагается на реализацию HTTP-сервера для переадресации запроса в приложение.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-191">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="d8a6d-192">Переадресованный запрос упаковывается как набор объектов функций, доступ к которым можно получить через интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-192">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="d8a6d-193">ASP.NET Core включает управляемый кроссплатформенный веб-сервер под названием [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-193">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="d8a6d-194">Часто Kestrel выполняется за рабочим веб-сервером, таким как [IIS](https://www.iis.net/) или [nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-194">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span> <span data-ttu-id="d8a6d-195">Kestrel можно запускать как пограничный сервер.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-195">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="d8a6d-196">Дополнительные сведения см. в разделе [Серверы](xref:fundamentals/servers/index) и в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-196">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="d8a6d-197">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d8a6d-197">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="d8a6d-198">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8a6d-198">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="d8a6d-199">[HTTP.sys](xref:fundamentals/servers/httpsys) (ранее — [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="d8a6d-199">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="d8a6d-200">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="d8a6d-200">Globalization and localization</span></span>

<span data-ttu-id="d8a6d-201">Создание многоязычного веб-сайта на основе ASP.NET Core позволяет расширить аудиторию.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-201">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="d8a6d-202">ASP.NET Core предоставляет службы и ПО промежуточного слоя для локализации на разные языки и для разных региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-202">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="d8a6d-203">Дополнительные сведения см. в разделе [Глобализация и локализация](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-203">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="d8a6d-204">Параметры запроса</span><span class="sxs-lookup"><span data-stu-id="d8a6d-204">Request features</span></span>

<span data-ttu-id="d8a6d-205">Сведения о реализации веб-сервера, связанные с HTTP-запросами и ответами, определяются в интерфейсах.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-205">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="d8a6d-206">Эти интерфейсы используются реализациями сервера и ПО промежуточного слоя для создания и изменения конвейера размещения приложений.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-206">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="d8a6d-207">Дополнительные сведения см. в разделе [Параметры запроса](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-207">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="d8a6d-208">Открытый веб-интерфейс для .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="d8a6d-208">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="d8a6d-209">ASP.NET Core поддерживает открытый веб-интерфейс для .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-209">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="d8a6d-210">OWIN позволяет ослабить зависимость веб-приложений от веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-210">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="d8a6d-211">Дополнительные сведения см. в разделе [Открытый веб-интерфейс для .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-211">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="d8a6d-212">Протокол WebSocket</span><span class="sxs-lookup"><span data-stu-id="d8a6d-212">WebSockets</span></span>

<span data-ttu-id="d8a6d-213">[WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-213">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="d8a6d-214">Он используется для таких приложений, как чаты, биржевые тикеры, игры, а также везде, где в веб-приложении нужны функции реального времени.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-214">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="d8a6d-215">ASP.NET Core поддерживает функции WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-215">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="d8a6d-216">Дополнительные сведения см. в разделе [Протокол WebSocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-216">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="d8a6d-217">Метапакет Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="d8a6d-217">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="d8a6d-218">Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:</span><span class="sxs-lookup"><span data-stu-id="d8a6d-218">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="d8a6d-219">все пакеты, поддерживаемые командой ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="d8a6d-219">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="d8a6d-220">все пакеты, поддерживаемые Entity Framework Core;</span><span class="sxs-lookup"><span data-stu-id="d8a6d-220">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="d8a6d-221">внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-221">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="d8a6d-222">Дополнительные сведения см. в разделе [Метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-222">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="d8a6d-223">Выбор между .NET Core и .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d8a6d-223">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="d8a6d-224">Приложения ASP.NET Core могут задавать среду выполнения .NET Core или .NET Framework как целевую.</span><span class="sxs-lookup"><span data-stu-id="d8a6d-224">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="d8a6d-225">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-225">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="d8a6d-226">Выбор между ASP.NET Core и ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d8a6d-226">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="d8a6d-227">Дополнительные сведения о выборе между ASP.NET Core и ASP.NET см. в разделе [Выбор между ASP.NET Core и ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="d8a6d-227">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
