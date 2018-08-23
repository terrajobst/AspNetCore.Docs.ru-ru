---
title: Основы ASP.NET Core
author: rick-anderson
description: Познакомьтесь с основными понятиями для создания приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 68760f179c4d6e806510b727e2284f8c2c4a4ff6
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2018
ms.locfileid: "41746088"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="36fb2-103">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36fb2-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="36fb2-104">Приложение ASP.NET Core — это консольное приложение, создающее веб-сервер в своем методе `Main`:</span><span class="sxs-lookup"><span data-stu-id="36fb2-104">An ASP.NET Core app is a console app that creates a web server in its `Main` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="36fb2-105">Метод `Main` вызывает [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), создающий веб-узел по [шаблону конструктора](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="36fb2-105">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="36fb2-106">Конструктор содержит методы, определяющие веб-сервер (например, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), а также класс запуска (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="36fb2-106">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="36fb2-107">В приведенном выше примере веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) выделяется автоматически.</span><span class="sxs-lookup"><span data-stu-id="36fb2-107">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="36fb2-108">Веб-хост ASP.NET Core попытается запуститься в службах IIS, если они доступны.</span><span class="sxs-lookup"><span data-stu-id="36fb2-108">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="36fb2-109">Другие веб-серверы, такие как [HTTP.sys](xref:fundamentals/servers/httpsys), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="36fb2-109">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="36fb2-110">`UseStartup` подробно описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="36fb2-110">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="36fb2-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, тип возвращаемого значения вызова `WebHost.CreateDefaultBuilder`, предоставляет множество вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="36fb2-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="36fb2-112">В число таких методов входят, например, `UseHttpSys` для размещения приложения в HTTP.sys и <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="36fb2-112">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="36fb2-113">Методы <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> и <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> собирают объект <xref:Microsoft.AspNetCore.Hosting.IWebHost>, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="36fb2-113">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="36fb2-114">Метод `Main` применяет <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, создающий узел веб-приложения по [шаблону конструктора](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="36fb2-114">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="36fb2-115">Конструктор содержит методы, определяющие веб-сервер (например, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), а также класс запуска (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="36fb2-115">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="36fb2-116">В приведенном выше примере используется веб-сервер [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="36fb2-116">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="36fb2-117">Другие веб-серверы, такие как [WebListener](xref:fundamentals/servers/weblistener), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="36fb2-117">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="36fb2-118">`UseStartup` подробно описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="36fb2-118">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="36fb2-119">В `WebHostBuilder` есть множество вспомогательных методов, включая <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> для размещения в службах IIS и IIS Express и <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="36fb2-119">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="36fb2-120">Методы <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> и <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> собирают объект <xref:Microsoft.AspNetCore.Hosting.IWebHost>, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="36fb2-120">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="36fb2-121">Запуск</span><span class="sxs-lookup"><span data-stu-id="36fb2-121">Startup</span></span>

<span data-ttu-id="36fb2-122">Метод `UseStartup` в `WebHostBuilder` определяет класс `Startup` для вашего приложения:</span><span class="sxs-lookup"><span data-stu-id="36fb2-122">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="36fb2-123">В классе `Startup` определяется конвейер обработки запросов и настраиваются все необходимые для приложения службы.</span><span class="sxs-lookup"><span data-stu-id="36fb2-123">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="36fb2-124">Класс `Startup` должен быть открытым и содержать следующие методы.</span><span class="sxs-lookup"><span data-stu-id="36fb2-124">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="36fb2-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> определяет [службы](#dependency-injection-services), используемые вашим приложением (такие как ASP.NET Core MVC, Entity Framework Core и Identity).</span><span class="sxs-lookup"><span data-stu-id="36fb2-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="36fb2-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> определяет [ПО промежуточного слоя](xref:fundamentals/middleware/index), вызываемое в конвейере обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="36fb2-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="36fb2-127">Дополнительные сведения см. в разделе <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-127">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="36fb2-128">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="36fb2-128">Content root</span></span>

<span data-ttu-id="36fb2-129">Корневой каталог содержимого — это базовый путь к любому содержимому, которое используется приложением, включая представления MVC, [Razor Pages](xref:razor-pages/index) и статические активы.</span><span class="sxs-lookup"><span data-stu-id="36fb2-129">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="36fb2-130">По умолчанию корневой каталог содержимого совпадает с базовым путем исполняемого файла приложения.</span><span class="sxs-lookup"><span data-stu-id="36fb2-130">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="36fb2-131">Корневой веб-узел</span><span class="sxs-lookup"><span data-stu-id="36fb2-131">Web root</span></span>

<span data-ttu-id="36fb2-132">Корневой каталог документов — это каталог в проекте, содержащий открытые и статические ресурсы, такие как CSS, JavaScript и файлы изображений.</span><span class="sxs-lookup"><span data-stu-id="36fb2-132">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="36fb2-133">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="36fb2-133">Dependency injection (services)</span></span>

<span data-ttu-id="36fb2-134">*Служба* — это компонент, предназначенный для общего использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="36fb2-134">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="36fb2-135">Доступ к службе предоставляется путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="36fb2-135">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="36fb2-136">ASP.NET Core включает встроенный контейнер для инверсии управления (IoC), который поддерживает [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="36fb2-136">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="36fb2-137">При необходимости вы можете заменить контейнер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="36fb2-137">You can replace the default container if you wish.</span></span> <span data-ttu-id="36fb2-138">Помимо формирования [слабых взаимосвязей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), внедрение зависимостей обеспечивает доступ к службам (например, [ведение журнала](xref:fundamentals/logging/index)) в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="36fb2-138">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="36fb2-139">Дополнительные сведения см. в разделе <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-139">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="36fb2-140">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="36fb2-140">Middleware</span></span>

<span data-ttu-id="36fb2-141">В ASP.NET Core конвейер запросов создается с помощью [ПО промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="36fb2-141">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="36fb2-142">ПО промежуточного слоя ASP.NET Core выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующее ПО промежуточного слоя в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="36fb2-142">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="36fb2-143">По соглашению компонент ПО промежуточного слоя с именем "XYZ" добавляется в конвейер путем вызова метода расширения `UseXYZ` в методе `Configure`.</span><span class="sxs-lookup"><span data-stu-id="36fb2-143">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="36fb2-144">ASP.NET Core включает целый ряд встроенного ПО промежуточного слоя и позволяет писать собственные пользовательские ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="36fb2-144">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="36fb2-145">[Открытый веб-интерфейс для .NET (OWIN)](xref:fundamentals/owin), позволяющий ослабить связь веб-приложений с веб-серверами, поддерживается в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="36fb2-145">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="36fb2-146">Дополнительные сведения см. в разделах <xref:fundamentals/middleware/index> и <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-146">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="36fb2-147">Инициирование HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="36fb2-147">Initiate HTTP requests</span></span>

<span data-ttu-id="36fb2-148">Можно использовать <xref:System.Net.Http.IHttpClientFactory> для доступа к экземплярам <xref:System.Net.Http.HttpClient> для HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="36fb2-148"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="36fb2-149">Дополнительные сведения см. в разделе <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-149">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="36fb2-150">Среды</span><span class="sxs-lookup"><span data-stu-id="36fb2-150">Environments</span></span>

<span data-ttu-id="36fb2-151">Среды, например *среда разработки* и *рабочая среда*, представляют собой ключевые компоненты ASP.NET Core и могут задаваться с использованием соответствующих переменных среды, файлов параметров и аргумента командной строки.</span><span class="sxs-lookup"><span data-stu-id="36fb2-151">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="36fb2-152">Дополнительные сведения см. в разделе <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-152">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="36fb2-153">Размещение</span><span class="sxs-lookup"><span data-stu-id="36fb2-153">Hosting</span></span>

<span data-ttu-id="36fb2-154">Приложения ASP.NET Core настраивают и запускают *хост*, который отвечает за запуск приложений и управление жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="36fb2-154">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="36fb2-155">Дополнительные сведения см. в разделе <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-155">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="36fb2-156">Серверы</span><span class="sxs-lookup"><span data-stu-id="36fb2-156">Servers</span></span>

<span data-ttu-id="36fb2-157">Модель размещения ASP.NET Core не прослушивает запросы напрямую.</span><span class="sxs-lookup"><span data-stu-id="36fb2-157">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="36fb2-158">Модель размещения полагается на реализацию HTTP-сервера для переадресации запроса в приложение.</span><span class="sxs-lookup"><span data-stu-id="36fb2-158">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="36fb2-159">Переадресованный запрос упаковывается как набор объектов функций, доступ к которым можно получить через интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="36fb2-159">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="36fb2-160">ASP.NET Core включает управляемый кроссплатформенный веб-сервер под названием [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="36fb2-160">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="36fb2-161">Kestrel обычно выполняется позади рабочего веб-сервера, например [IIS](https://www.iis.net/) или [Nginx](http://nginx.org), в обратной конфигурации прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="36fb2-161">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="36fb2-162">Kestrel также может выполняться в качестве пограничного сервера с прямым доступом к Интернет в ASP.NET Core 2.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="36fb2-162">Kestrel can also be run as an edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="36fb2-163">Дополнительные сведения см. в разделе <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-163">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="36fb2-164">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="36fb2-164">Configuration</span></span>

<span data-ttu-id="36fb2-165">ASP.NET Core использует модель конфигурации на основе пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="36fb2-165">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="36fb2-166">Модель конфигурации не основана на <xref:System.Configuration> или *web.config*. Конфигурация получает параметры от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="36fb2-166">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="36fb2-167">Встроенные поставщики конфигурации поддерживают различные форматы файлов (XML, JSON, INI), переменных среды и аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="36fb2-167">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="36fb2-168">Кроме того, можно писать и собственные, пользовательские поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="36fb2-168">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="36fb2-169">Дополнительные сведения см. в разделе <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-169">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="36fb2-170">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="36fb2-170">Logging</span></span>

<span data-ttu-id="36fb2-171">ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="36fb2-171">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="36fb2-172">Встроенные поставщики поддерживают отправку журналов в одно расположение или несколько.</span><span class="sxs-lookup"><span data-stu-id="36fb2-172">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="36fb2-173">Можно также использовать сторонние платформы ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="36fb2-173">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="36fb2-174">Дополнительные сведения см. в разделе <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-174">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="36fb2-175">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="36fb2-175">Error handling</span></span>

<span data-ttu-id="36fb2-176">ASP.NET Core включает встроенные сценарии для обработки ошибок в приложениях, включая страницу исключений разработчика, настраиваемые страницы ошибок, страницы статических кодов состояний и обработку исключений при запуске.</span><span class="sxs-lookup"><span data-stu-id="36fb2-176">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="36fb2-177">Дополнительные сведения см. в разделе <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-177">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="36fb2-178">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="36fb2-178">Routing</span></span>

<span data-ttu-id="36fb2-179">В ASP.NET Core присутствуют сценарии для маршрутизации запросов приложений обработчикам маршрутов.</span><span class="sxs-lookup"><span data-stu-id="36fb2-179">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="36fb2-180">Дополнительные сведения см. в разделе <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="36fb2-181">Поставщики файлов</span><span class="sxs-lookup"><span data-stu-id="36fb2-181">File Providers</span></span>

<span data-ttu-id="36fb2-182">ASP.NET Core ограничивает доступ к файловой системе с помощью поставщиков файлов, которые предоставляют общий интерфейс для работы с файлами на разных платформах.</span><span class="sxs-lookup"><span data-stu-id="36fb2-182">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="36fb2-183">Дополнительные сведения см. в разделе <xref:fundamentals/file-providers>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-183">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="36fb2-184">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="36fb2-184">Static files</span></span>

<span data-ttu-id="36fb2-185">ПО промежуточного слоя для статических файлов обрабатывает такие статические файлы, как HTML, CSS, файлы изображений и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="36fb2-185">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="36fb2-186">Дополнительные сведения см. в разделе <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-186">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="36fb2-187">Состояние сеанса и приложения</span><span class="sxs-lookup"><span data-stu-id="36fb2-187">Session and app state</span></span>

<span data-ttu-id="36fb2-188">ASP.NET Core предлагает несколько способов сохранения состояния сеанса и приложения во время работы пользователя в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="36fb2-188">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="36fb2-189">Дополнительные сведения см. в разделе <xref:fundamentals/app-state>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-189">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="36fb2-190">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="36fb2-190">Globalization and localization</span></span>

<span data-ttu-id="36fb2-191">Создание многоязычного веб-сайта на основе ASP.NET Core позволяет расширить аудиторию.</span><span class="sxs-lookup"><span data-stu-id="36fb2-191">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="36fb2-192">ASP.NET Core предоставляет службы и ПО промежуточного слоя для локализации содержимого на разные языки и для разных региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="36fb2-192">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="36fb2-193">Дополнительные сведения см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-193">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="36fb2-194">Параметры запроса</span><span class="sxs-lookup"><span data-stu-id="36fb2-194">Request features</span></span>

<span data-ttu-id="36fb2-195">Сведения о реализации веб-сервера, связанные с HTTP-запросами и ответами, определяются в интерфейсах.</span><span class="sxs-lookup"><span data-stu-id="36fb2-195">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="36fb2-196">Эти интерфейсы используются реализациями сервера и ПО промежуточного слоя для создания и изменения конвейера размещения приложений.</span><span class="sxs-lookup"><span data-stu-id="36fb2-196">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="36fb2-197">Дополнительные сведения см. в разделе <xref:fundamentals/request-features>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-197">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="36fb2-198">Фоновые задачи</span><span class="sxs-lookup"><span data-stu-id="36fb2-198">Background tasks</span></span>

<span data-ttu-id="36fb2-199">Фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="36fb2-199">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="36fb2-200">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-200">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="36fb2-201">Дополнительные сведения см. в разделе <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-201">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="36fb2-202">Доступ к HttpContext</span><span class="sxs-lookup"><span data-stu-id="36fb2-202">Access HttpContext</span></span>

<span data-ttu-id="36fb2-203">Доступ к `HttpContext` предоставляется автоматически при обработке запросов с Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="36fb2-203">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="36fb2-204">Если к `HttpContext` нет доступа, вы можете получить доступ к `HttpContext` через интерфейс <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> и реализацию по умолчанию <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-204">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="36fb2-205">Дополнительные сведения см. в разделе <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-205">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="36fb2-206">Протокол WebSocket</span><span class="sxs-lookup"><span data-stu-id="36fb2-206">WebSockets</span></span>

<span data-ttu-id="36fb2-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям.</span><span class="sxs-lookup"><span data-stu-id="36fb2-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="36fb2-208">Он используется для таких приложений, как чаты, биржевые тикеры, игры, а также везде, где в веб-приложении нужны функции реального времени.</span><span class="sxs-lookup"><span data-stu-id="36fb2-208">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="36fb2-209">ASP.NET Core поддерживает сценарии WebSocket.</span><span class="sxs-lookup"><span data-stu-id="36fb2-209">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="36fb2-210">Дополнительные сведения см. в разделе <xref:fundamentals/websockets>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-210">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="36fb2-211">Метапакет Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="36fb2-211">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="36fb2-212">Метапакет [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) упрощает управление пакетами.</span><span class="sxs-lookup"><span data-stu-id="36fb2-212">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="36fb2-213">Дополнительные сведения см. в разделе <xref:fundamentals/metapackage-app>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-213">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="36fb2-214">Метапакет Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="36fb2-214">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="36fb2-215">Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:</span><span class="sxs-lookup"><span data-stu-id="36fb2-215">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="36fb2-216">все пакеты, поддерживаемые командой ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="36fb2-216">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="36fb2-217">все пакеты, поддерживаемые Entity Framework Core;</span><span class="sxs-lookup"><span data-stu-id="36fb2-217">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="36fb2-218">внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="36fb2-218">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="36fb2-219">Дополнительные сведения см. в разделе <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-219">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="36fb2-220">Выбор между .NET Core и .NET Framework</span><span class="sxs-lookup"><span data-stu-id="36fb2-220">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="36fb2-221">Приложения ASP.NET Core могут задавать среду выполнения .NET Core или .NET Framework как целевую.</span><span class="sxs-lookup"><span data-stu-id="36fb2-221">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="36fb2-222">Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="36fb2-222">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="36fb2-223">Выбор между ASP.NET Core и ASP.NET</span><span class="sxs-lookup"><span data-stu-id="36fb2-223">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="36fb2-224">Дополнительные сведения о выборе между ASP.NET Core и ASP.NET см. в разделе <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span><span class="sxs-lookup"><span data-stu-id="36fb2-224">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
