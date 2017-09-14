---
title: "Основы ASP.NET Core"
author: rick-anderson
description: "В этой статье приводится общий обзор основных понятий, которые нужно знать для сборки приложений ASP.NET Core."
keywords: "ASP.NET Core, основы, обзор"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d8ca35b0e2e4b6e9b1ec745a3a7cf7c3983c461
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-fundamentals-overview"></a><span data-ttu-id="88980-104">ASP.NET Core, основы, обзор</span><span class="sxs-lookup"><span data-stu-id="88980-104">ASP.NET Core fundamentals overview</span></span>

<span data-ttu-id="88980-105">Приложение ASP.NET Core — это консольное приложение, создающее веб-сервер в своем методе `Main`:</span><span class="sxs-lookup"><span data-stu-id="88980-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88980-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88980-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="88980-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span><span class="sxs-lookup"><span data-stu-id="88980-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span></span>

<span data-ttu-id="88980-108">Метод `Main` вызывает `WebHost.CreateDefaultBuilder`, создающий узел веб-приложения по шаблону конструктора.</span><span class="sxs-lookup"><span data-stu-id="88980-108">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="88980-109">Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="88980-109">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="88980-110">В приведенном выше примере веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) выделяется автоматически.</span><span class="sxs-lookup"><span data-stu-id="88980-110">In the preceding example, a [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="88980-111">Веб-узел ASP.NET Core попытается запуститься на сервере IIS, если он будет доступен.</span><span class="sxs-lookup"><span data-stu-id="88980-111">ASP.NET Core's web host will attempt to run on IIS, if it is available.</span></span> <span data-ttu-id="88980-112">Другие веб-серверы, такие как [HTTP.sys](xref:fundamentals/servers/httpsys), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="88980-112">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="88980-113">`UseStartup` подробно описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="88980-113">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="88980-114">`IWebHostBuilder`, тип возвращаемого значения вызова `WebHost.CreateDefaultBuilder`, предоставляет множество вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="88980-114">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="88980-115">В число таких методов входят, например, `UseHttpSys` для размещения приложения на веб-сервере HTTP.sys и `UseContentRoot` для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="88980-115">Some of these methods include `UseHttpSys` for hosting the application in HTTP.sys, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="88980-116">Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="88980-116">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88980-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88980-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="88980-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="88980-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span></span>

<span data-ttu-id="88980-119">Метод `Main` применяет `WebHostBuilder`, создающий узел веб-приложения по шаблону конструктора.</span><span class="sxs-lookup"><span data-stu-id="88980-119">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="88980-120">Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="88980-120">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="88980-121">В приведенном выше примере используется веб-сервер [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="88980-121">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="88980-122">Другие веб-серверы, такие как [WebListener](xref:fundamentals/servers/weblistener), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="88980-122">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="88980-123">`UseStartup` подробно описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="88980-123">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="88980-124">`WebHostBuilder` предоставляет множество вспомогательных методов, включая `UseIISIntegration` для размещения в IIS и IIS Express и `UseContentRoot` для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="88980-124">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="88980-125">Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="88980-125">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="88980-126">Запуск</span><span class="sxs-lookup"><span data-stu-id="88980-126">Startup</span></span>

<span data-ttu-id="88980-127">Метод `UseStartup` в `WebHostBuilder` определяет класс `Startup` для вашего приложения:</span><span class="sxs-lookup"><span data-stu-id="88980-127">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88980-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88980-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="88980-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="88980-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88980-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88980-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="88980-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="88980-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span></span>

---

<span data-ttu-id="88980-132">В классе `Startup` определяется конвейер обработки запросов и настраиваются все необходимые для приложения службы.</span><span class="sxs-lookup"><span data-stu-id="88980-132">The `Startup` class is where you define the request handling pipeline and where any services needed by the application are configured.</span></span> <span data-ttu-id="88980-133">Класс `Startup` должен быть открытым и содержать следующие методы.</span><span class="sxs-lookup"><span data-stu-id="88980-133">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* <span data-ttu-id="88980-134">`ConfigureServices` определяет [службы](#services), используемые вашим приложением (такие как MVC ASP.NET Core, Entity Framework Core, Identity и т. д.).</span><span class="sxs-lookup"><span data-stu-id="88980-134">`ConfigureServices` defines the [Services](#services) used by your application (such as ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span></span>

* <span data-ttu-id="88980-135">`Configure` определяет [ПО промежуточного слоя](xref:fundamentals/middleware) в конвейере обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="88980-135">`Configure` defines the [middleware](xref:fundamentals/middleware) in the request pipeline.</span></span>

<span data-ttu-id="88980-136">Дополнительные сведения см. в разделе [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="88980-136">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="services"></a><span data-ttu-id="88980-137">Службы</span><span class="sxs-lookup"><span data-stu-id="88980-137">Services</span></span>

<span data-ttu-id="88980-138">Служба — это компонент, предназначенный для общего использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="88980-138">A service is a component that is intended for common consumption in an application.</span></span> <span data-ttu-id="88980-139">Доступ к службе предоставляется путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="88980-139">Services are made available through [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="88980-140">ASP.NET Core включает встроенный контейнер для инверсии управления (IoC), который поддерживает [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="88980-140">ASP.NET Core includes a native inversion of control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="88980-141">Встроенный контейнер можно заменить на контейнер по вашему выбору.</span><span class="sxs-lookup"><span data-stu-id="88980-141">The native container can be replaced with your container of choice.</span></span> <span data-ttu-id="88980-142">Помимо формирования слабых взаимосвязей внедрение зависимостей обеспечивает доступ к службам в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="88980-142">In addition to its loose coupling benefit, DI makes services available throughout your application.</span></span> <span data-ttu-id="88980-143">Например, в любой части вашего приложения доступна служба [входа](xref:fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="88980-143">For example, [logging](xref:fundamentals/logging) is available throughout your application.</span></span>

<span data-ttu-id="88980-144">Дополнительные сведения см. в разделе [Введение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="88980-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="88980-145">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="88980-145">Middleware</span></span>

<span data-ttu-id="88980-146">В ASP.NET Core конвейер запросов создается с помощью [ПО промежуточного слоя](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="88980-146">In ASP.NET Core, you compose your request pipeline using [Middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="88980-147">ПО промежуточного слоя ASP.NET Core выполняет асинхронную логику в `HttpContext`, а затем либо вызывает следующее ПО промежуточного слоя в последовательности, либо завершает запрос напрямую.</span><span class="sxs-lookup"><span data-stu-id="88980-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="88980-148">Компонент ПО промежуточного слоя с именем "XYZ" добавляется путем вызова метода расширения `UseXYZ` в методе `Configure`.</span><span class="sxs-lookup"><span data-stu-id="88980-148">A middleware component called "XYZ" is added by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="88980-149">ASP.NET Core содержит большой набор встроенных ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="88980-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="88980-150">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="88980-150">Static files</span></span>](xref:fundamentals/static-files)

* [<span data-ttu-id="88980-151">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="88980-151">Routing</span></span>](xref:fundamentals/routing)

* [<span data-ttu-id="88980-152">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="88980-152">Authentication</span></span>](xref:security/authentication/index)

<span data-ttu-id="88980-153">В ASP.NET Core можно использовать любое ПО промежуточного слоя на базе [OWIN](http://owin.org), а также писать собственные, пользовательские ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="88980-153">You can use any [OWIN](http://owin.org)-based middleware with ASP.NET Core, and you can write your own custom middleware.</span></span>

<span data-ttu-id="88980-154">Дополнительные сведения см. в статьях [ПО промежуточного слоя](xref:fundamentals/middleware) и [Открытый веб-интерфейс .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="88980-154">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="servers"></a><span data-ttu-id="88980-155">Серверы</span><span class="sxs-lookup"><span data-stu-id="88980-155">Servers</span></span>

<span data-ttu-id="88980-156">В модели размещения ASP.NET Core запросы не прослушиваются напрямую. Переадресация запросов в приложение происходит через реализацию сервера HTTP.</span><span class="sxs-lookup"><span data-stu-id="88980-156">The ASP.NET Core hosting model does not directly listen for requests; rather, it relies on an HTTP server implementation to forward the request to the application.</span></span> <span data-ttu-id="88980-157">Переадресованный запрос упаковывается как набор объектов функций, доступ к которым можно получить через интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="88980-157">The forwarded request is wrapped as a set of feature objects that you can access through interfaces.</span></span> <span data-ttu-id="88980-158">Приложение внедряет этот набор в `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="88980-158">The application composes this set into an `HttpContext`.</span></span> <span data-ttu-id="88980-159">ASP.NET Core включает управляемый кроссплатформенный веб-сервер под названием [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="88980-159">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="88980-160">Обычно Kestrel выполняется за рабочим веб-сервером, таким как [IIS](https://www.iis.net/) или [nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="88980-160">Kestrel is typically run behind a production web server like [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span>

<span data-ttu-id="88980-161">Дополнительные сведения см. в статьях [Серверы](xref:fundamentals/servers/index) и [Размещения](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="88980-161">For more information, see [Servers](xref:fundamentals/servers/index) and [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="content-root"></a><span data-ttu-id="88980-162">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="88980-162">Content root</span></span>

<span data-ttu-id="88980-163">Корневой каталог содержимого — это базовый путь к любому содержимому, которое используется приложением, включая представления, [страницы Razor](xref:mvc/razor-pages/index) и статические активы.</span><span class="sxs-lookup"><span data-stu-id="88980-163">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="88980-164">По умолчанию корневой каталог содержимого совпадает с базовой папкой, в которой хранится исполняемый файл приложения.</span><span class="sxs-lookup"><span data-stu-id="88980-164">By default, the content root is the same as application base path for the executable hosting the application.</span></span> <span data-ttu-id="88980-165">Другое расположение для корневого каталога содержимого определяет параметр `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="88980-165">An alternative location for content root is specified with `WebHostBuilder`.</span></span>

## <a name="web-root"></a><span data-ttu-id="88980-166">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="88980-166">Web root</span></span>

<span data-ttu-id="88980-167">Корневой веб-узел приложения — это каталог в проекте, содержащий открытые статические ресурсы, такие как CSS, JavaScript и файлы изображений.</span><span class="sxs-lookup"><span data-stu-id="88980-167">The web root of an application is the directory in the project containing public, static resources like CSS, JavaScript, and image files.</span></span> <span data-ttu-id="88980-168">По умолчанию ПО промежуточного слоя статических файлов обслуживает только файлы из корневого веб-каталога и его подпапок.</span><span class="sxs-lookup"><span data-stu-id="88980-168">By default, the static files middleware will only serve files from the web root directory and its sub-directories.</span></span> <span data-ttu-id="88980-169">Дополнительные сведения см. в статье о [работе со статическими файлами](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="88980-169">See [working with static files](xref:fundamentals/static-files) for more info.</span></span> <span data-ttu-id="88980-170">По умолчанию путь к корневому веб-узлу указывает на папку */wwwroot*; другое местонахождение можно указать с помощью параметра `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="88980-170">The web root path defaults to */wwwroot*, but you can specify a different location using the `WebHostBuilder`.</span></span>

## <a name="configuration"></a><span data-ttu-id="88980-171">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="88980-171">Configuration</span></span>

<span data-ttu-id="88980-172">В ASP.NET Core для обработки простых пар имен и значений используется новая модель конфигурации.</span><span class="sxs-lookup"><span data-stu-id="88980-172">ASP.NET Core uses a new configuration model for handling simple name-value pairs.</span></span> <span data-ttu-id="88980-173">Новая модель конфигурации опирается не на `System.Configuration` или *web.config*, а на упорядоченный набор поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="88980-173">The new configuration model is not based on `System.Configuration` or *web.config*; rather, it pulls from an ordered set of configuration providers.</span></span> <span data-ttu-id="88980-174">Встроенные поставщики конфигурации поддерживают различные форматы файлов (XML, JSON, INI) и переменных среды для выполнения конфигурации на основе среды.</span><span class="sxs-lookup"><span data-stu-id="88980-174">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="88980-175">Кроме того, можно писать и собственные, пользовательские поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="88980-175">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="88980-176">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="88980-176">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="environments"></a><span data-ttu-id="88980-177">Среды</span><span class="sxs-lookup"><span data-stu-id="88980-177">Environments</span></span>

<span data-ttu-id="88980-178">Среды разработка и рабочая среда представляют собой основные составляющие в ASP.NET Core и могут задаваться с использованием соответствующих переменных.</span><span class="sxs-lookup"><span data-stu-id="88980-178">Environments, like "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="88980-179">Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="88980-179">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="88980-180">Выбор между .NET Core и .NET Framework</span><span class="sxs-lookup"><span data-stu-id="88980-180">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="88980-181">Приложения ASP.NET Core могут предназначаться для среды .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="88980-181">An ASP.NET Core application can target the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="88980-182">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="88980-182">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="additional-information"></a><span data-ttu-id="88980-183">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="88980-183">Additional information</span></span>

<span data-ttu-id="88980-184">См. также следующие статьи.</span><span class="sxs-lookup"><span data-stu-id="88980-184">See also the following topics:</span></span>

- [<span data-ttu-id="88980-185">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="88980-185">Error Handling</span></span>](xref:fundamentals/error-handling)
- [<span data-ttu-id="88980-186">Поставщики файлов</span><span class="sxs-lookup"><span data-stu-id="88980-186">File Providers</span></span>](xref:fundamentals/file-providers)
- [<span data-ttu-id="88980-187">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="88980-187">Globalization and localization</span></span>](xref:fundamentals/localization)
- [<span data-ttu-id="88980-188">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="88980-188">Logging</span></span>](xref:fundamentals/logging)
- [<span data-ttu-id="88980-189">Управление состоянием приложения</span><span class="sxs-lookup"><span data-stu-id="88980-189">Managing Application State</span></span>](xref:fundamentals/app-state)
