---
title: Основы ASP.NET Core
author: rick-anderson
description: Познакомьтесь с основными понятиями для создания приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/index
ms.openlocfilehash: 8bd447632f915cadcc5199ec50b292ad27f6c3ba
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861593"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="b8d97-103">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8d97-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="b8d97-104">Приложение ASP.NET Core — это консольное приложение, создающее веб-сервер в своем методе `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="b8d97-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="b8d97-105">Метод `Main` является *управляемой точкой входа* приложения:</span><span class="sxs-lookup"><span data-stu-id="b8d97-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="b8d97-106">Узел .NET Core:</span><span class="sxs-lookup"><span data-stu-id="b8d97-106">The .NET Core Host:</span></span>

* <span data-ttu-id="b8d97-107">загружает среду выполнения [.NET Core](https://github.com/dotnet/coreclr);</span><span class="sxs-lookup"><span data-stu-id="b8d97-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="b8d97-108">использует первый аргумент командной строки в качестве пути к управляемому двоичному файлу, который содержит точку входа (`Main`) и начинает выполнение кода.</span><span class="sxs-lookup"><span data-stu-id="b8d97-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="b8d97-109">Метод `Main` вызывает [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), создающий веб-узел по [шаблону конструктора](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="b8d97-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="b8d97-110">Конструктор содержит методы, определяющие веб-сервер (например, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), а также класс запуска (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="b8d97-110">The builder has methods that define a web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="b8d97-111">В приведенном выше примере веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) выделяется автоматически.</span><span class="sxs-lookup"><span data-stu-id="b8d97-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="b8d97-112">Веб-хост ASP.NET Core попытается запуститься в [службах IIS](https://www.iis.net/), если они доступны.</span><span class="sxs-lookup"><span data-stu-id="b8d97-112">ASP.NET Core's web host attempts to run on [Internet Information Services (IIS)](https://www.iis.net/), if available.</span></span> <span data-ttu-id="b8d97-113">Другие веб-серверы, такие как [HTTP.sys](xref:fundamentals/servers/httpsys), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="b8d97-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="b8d97-114">`UseStartup` подробно описывается в разделе [Для стартапов](#startup).</span><span class="sxs-lookup"><span data-stu-id="b8d97-114">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="b8d97-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, тип возвращаемого значения вызова `WebHost.CreateDefaultBuilder`, предоставляет множество вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="b8d97-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="b8d97-116">В число таких методов входят, например, `UseHttpSys` для размещения приложения в HTTP.sys и <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="b8d97-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="b8d97-117">Методы <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> и <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> собирают объект <xref:Microsoft.AspNetCore.Hosting.IWebHost>, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="b8d97-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="b8d97-118">Узел .NET Core:</span><span class="sxs-lookup"><span data-stu-id="b8d97-118">The .NET Core Host:</span></span>

* <span data-ttu-id="b8d97-119">загружает среду выполнения [.NET Core](https://github.com/dotnet/coreclr);</span><span class="sxs-lookup"><span data-stu-id="b8d97-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="b8d97-120">использует первый аргумент командной строки в качестве пути к управляемому двоичному файлу, который содержит точку входа (`Main`) и начинает выполнение кода.</span><span class="sxs-lookup"><span data-stu-id="b8d97-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="b8d97-121">Метод `Main` применяет <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, создающий узел веб-приложения по [шаблону конструктора](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="b8d97-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="b8d97-122">Конструктор содержит методы, определяющие веб-сервер (например, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), а также класс запуска (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="b8d97-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="b8d97-123">В приведенном выше примере используется веб-сервер [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b8d97-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="b8d97-124">Другие веб-серверы, такие как [WebListener](xref:fundamentals/servers/weblistener), можно использовать, вызывая соответствующий метод расширения.</span><span class="sxs-lookup"><span data-stu-id="b8d97-124">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="b8d97-125">`UseStartup` подробно описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="b8d97-125">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="b8d97-126">В `WebHostBuilder` есть множество вспомогательных методов, включая <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> для размещения в службах IIS и IIS Express и <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> для указания корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="b8d97-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="b8d97-127">Методы <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> и <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> собирают объект <xref:Microsoft.AspNetCore.Hosting.IWebHost>, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="b8d97-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="b8d97-128">Запуск</span><span class="sxs-lookup"><span data-stu-id="b8d97-128">Startup</span></span>

<span data-ttu-id="b8d97-129">Метод `UseStartup` в `WebHostBuilder` определяет класс `Startup` для вашего приложения:</span><span class="sxs-lookup"><span data-stu-id="b8d97-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="b8d97-130">В классе `Startup` определяется конвейер обработки запросов и настраиваются все необходимые для приложения службы.</span><span class="sxs-lookup"><span data-stu-id="b8d97-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="b8d97-131">Класс `Startup` должен быть открытым и содержать следующие методы.</span><span class="sxs-lookup"><span data-stu-id="b8d97-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="b8d97-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> определяет [службы](#dependency-injection-services), используемые вашим приложением (такие как ASP.NET Core MVC, Entity Framework Core и Identity).</span><span class="sxs-lookup"><span data-stu-id="b8d97-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="b8d97-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> определяет [ПО промежуточного слоя](xref:fundamentals/middleware/index), вызываемое в конвейере обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="b8d97-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="b8d97-134">Дополнительные сведения см. в разделе <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="b8d97-135">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="b8d97-135">Content root</span></span>

<span data-ttu-id="b8d97-136">Корневой каталог содержимого — это базовый путь к любому содержимому, которое используется приложением, включая представления MVC, [Razor Pages](xref:razor-pages/index) и статические активы.</span><span class="sxs-lookup"><span data-stu-id="b8d97-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="b8d97-137">По умолчанию корневой каталог содержимого совпадает с базовым путем исполняемого файла приложения.</span><span class="sxs-lookup"><span data-stu-id="b8d97-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="b8d97-138">Корневой каталог (webroot)</span><span class="sxs-lookup"><span data-stu-id="b8d97-138">Web root (webroot)</span></span>

<span data-ttu-id="b8d97-139">Корневой каталог приложения — это каталог в проекте, содержащий открытые и статические ресурсы, такие как CSS, JavaScript и файлы изображений.</span><span class="sxs-lookup"><span data-stu-id="b8d97-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="b8d97-140">По умолчанию корневым каталогом является *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="b8d97-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="b8d97-141">Для указания на корневой каталог файлов Razor (*.cshtml*) используется символ тильды и косой черты `~/`.</span><span class="sxs-lookup"><span data-stu-id="b8d97-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="b8d97-142">Пути, начинающиеся с `~/`, называются виртуальными путями.</span><span class="sxs-lookup"><span data-stu-id="b8d97-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="b8d97-143">Введение зависимостей (службы)</span><span class="sxs-lookup"><span data-stu-id="b8d97-143">Dependency injection (services)</span></span>

<span data-ttu-id="b8d97-144">*Служба* — это компонент, предназначенный для общего использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="b8d97-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="b8d97-145">Доступ к службе предоставляется путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b8d97-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b8d97-146">ASP.NET Core включает встроенный контейнер для инверсии управления (IoC), который поддерживает [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b8d97-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="b8d97-147">При необходимости вы можете заменить контейнер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b8d97-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="b8d97-148">Помимо формирования [слабых взаимосвязей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), внедрение зависимостей обеспечивает доступ к службам (например, [ведение журнала](xref:fundamentals/logging/index)) в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="b8d97-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="b8d97-149">Дополнительные сведения см. в разделе <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="b8d97-150">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="b8d97-150">Middleware</span></span>

<span data-ttu-id="b8d97-151">В ASP.NET Core конвейер запросов создается с помощью [ПО промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="b8d97-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="b8d97-152">ПО промежуточного слоя ASP.NET Core выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующее ПО промежуточного слоя в конвейере, либо завершает запрос.</span><span class="sxs-lookup"><span data-stu-id="b8d97-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="b8d97-153">По соглашению компонент ПО промежуточного слоя с именем "XYZ" добавляется в конвейер путем вызова метода расширения `UseXYZ` в методе `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b8d97-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="b8d97-154">ASP.NET Core включает целый ряд встроенного ПО промежуточного слоя и позволяет писать собственные пользовательские ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="b8d97-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="b8d97-155">[Открытый веб-интерфейс для .NET (OWIN)](xref:fundamentals/owin), позволяющий ослабить связь веб-приложений с веб-серверами, поддерживается в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b8d97-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="b8d97-156">Дополнительные сведения см. в разделах <xref:fundamentals/middleware/index> и <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="b8d97-157">Инициирование HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="b8d97-157">Initiate HTTP requests</span></span>

<span data-ttu-id="b8d97-158">Можно использовать <xref:System.Net.Http.IHttpClientFactory> для доступа к экземплярам <xref:System.Net.Http.HttpClient> для HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="b8d97-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="b8d97-159">Дополнительные сведения см. в разделе <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="b8d97-160">Среды</span><span class="sxs-lookup"><span data-stu-id="b8d97-160">Environments</span></span>

<span data-ttu-id="b8d97-161">Среды, например *среда разработки* и *рабочая среда*, представляют собой ключевые компоненты ASP.NET Core и могут задаваться с использованием соответствующих переменных среды, файлов параметров и аргумента командной строки.</span><span class="sxs-lookup"><span data-stu-id="b8d97-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="b8d97-162">Дополнительные сведения см. в разделе <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="b8d97-163">Размещение</span><span class="sxs-lookup"><span data-stu-id="b8d97-163">Hosting</span></span>

<span data-ttu-id="b8d97-164">Приложения ASP.NET Core настраивают и запускают *хост*, который отвечает за запуск приложений и управление жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="b8d97-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="b8d97-165">Дополнительные сведения см. в разделе <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="b8d97-166">Серверы</span><span class="sxs-lookup"><span data-stu-id="b8d97-166">Servers</span></span>

<span data-ttu-id="b8d97-167">Модель размещения ASP.NET Core не прослушивает запросы напрямую.</span><span class="sxs-lookup"><span data-stu-id="b8d97-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="b8d97-168">Модель размещения полагается на реализацию HTTP-сервера для переадресации запроса в приложение.</span><span class="sxs-lookup"><span data-stu-id="b8d97-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="b8d97-169">Windows</span><span class="sxs-lookup"><span data-stu-id="b8d97-169">Windows</span></span>](#tab/windows)

<span data-ttu-id="b8d97-170">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="b8d97-170">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="b8d97-171">Сервер [Kestrel](xref:fundamentals/servers/kestrel) — это управляемый кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="b8d97-171">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="b8d97-172">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="b8d97-172">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="b8d97-173">Kestrel также можно использовать в роли общедоступного пограничного сервера с прямым доступом к Интернету в ASP.NET Core 2.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="b8d97-173">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="b8d97-174">HTTP-сервер службы IIS (`IISHttpServer`) является [внутрипроцессным сервером службы IIS](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="b8d97-174">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model).</span></span>
* <span data-ttu-id="b8d97-175">Сервер [HTTP.sys](xref:fundamentals/servers/httpsys) является сервером для ASP.NET Core в Windows.</span><span class="sxs-lookup"><span data-stu-id="b8d97-175">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="b8d97-176">macOS</span><span class="sxs-lookup"><span data-stu-id="b8d97-176">macOS</span></span>](#tab/macos)

<span data-ttu-id="b8d97-177">ASP.NET Core использует реализацию сервера [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b8d97-177">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="b8d97-178">Kestrel — это управляемый кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="b8d97-178">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="b8d97-179">Kestrel также можно использовать в роли общедоступного пограничного сервера с прямым доступом к Интернету в ASP.NET Core 2.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="b8d97-179">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b8d97-180">Linux</span><span class="sxs-lookup"><span data-stu-id="b8d97-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="b8d97-181">ASP.NET Core использует реализацию сервера [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b8d97-181">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="b8d97-182">Kestrel — это управляемый кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="b8d97-182">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="b8d97-183">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="b8d97-183">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="b8d97-184">Kestrel также можно использовать в роли общедоступного пограничного сервера с прямым доступом к Интернету в ASP.NET Core 2.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="b8d97-184">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="b8d97-185">Windows</span><span class="sxs-lookup"><span data-stu-id="b8d97-185">Windows</span></span>](#tab/windows)

<span data-ttu-id="b8d97-186">ASP.NET Core предоставляет следующие реализации серверов:</span><span class="sxs-lookup"><span data-stu-id="b8d97-186">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="b8d97-187">Сервер [Kestrel](xref:fundamentals/servers/kestrel) — это управляемый кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="b8d97-187">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="b8d97-188">Он часто запускается в реконфигурации прокси-сервера с помощью [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="b8d97-188">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="b8d97-189">Kestrel также можно использовать в роли общедоступного пограничного сервера с прямым доступом к Интернету в ASP.NET Core 2.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="b8d97-189">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="b8d97-190">Сервер [HTTP.sys](xref:fundamentals/servers/httpsys) является сервером для ASP.NET Core в Windows.</span><span class="sxs-lookup"><span data-stu-id="b8d97-190">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="b8d97-191">macOS</span><span class="sxs-lookup"><span data-stu-id="b8d97-191">macOS</span></span>](#tab/macos)

<span data-ttu-id="b8d97-192">ASP.NET Core использует реализацию сервера [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b8d97-192">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="b8d97-193">Kestrel — это управляемый кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="b8d97-193">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="b8d97-194">Kestrel также можно использовать в роли общедоступного пограничного сервера с прямым доступом к Интернету в ASP.NET Core 2.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="b8d97-194">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b8d97-195">Linux</span><span class="sxs-lookup"><span data-stu-id="b8d97-195">Linux</span></span>](#tab/linux)

<span data-ttu-id="b8d97-196">ASP.NET Core использует реализацию сервера [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b8d97-196">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="b8d97-197">Kestrel — это управляемый кроссплатформенный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="b8d97-197">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="b8d97-198">Он часто запускается в реконфигурации прокси-сервера с помощью [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="b8d97-198">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="b8d97-199">Kestrel также можно использовать в роли общедоступного пограничного сервера с прямым доступом к Интернету в ASP.NET Core 2.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="b8d97-199">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

<span data-ttu-id="b8d97-200">Дополнительные сведения см. в разделе <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-200">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="b8d97-201">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="b8d97-201">Configuration</span></span>

<span data-ttu-id="b8d97-202">ASP.NET Core использует модель конфигурации на основе пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="b8d97-202">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="b8d97-203">Модель конфигурации не основана на <xref:System.Configuration> или *web.config*. Конфигурация получает параметры от упорядоченного набора поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b8d97-203">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="b8d97-204">Встроенные поставщики конфигурации поддерживают различные форматы файлов (XML, JSON, INI), переменных среды и аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="b8d97-204">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="b8d97-205">Кроме того, можно писать и собственные, пользовательские поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b8d97-205">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="b8d97-206">Дополнительные сведения см. в разделе <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="b8d97-207">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="b8d97-207">Logging</span></span>

<span data-ttu-id="b8d97-208">ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="b8d97-208">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="b8d97-209">Встроенные поставщики поддерживают отправку журналов в одно расположение или несколько.</span><span class="sxs-lookup"><span data-stu-id="b8d97-209">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="b8d97-210">Можно также использовать сторонние платформы ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b8d97-210">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="b8d97-211">Дополнительные сведения см. в разделе <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-211">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="b8d97-212">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="b8d97-212">Error handling</span></span>

<span data-ttu-id="b8d97-213">ASP.NET Core включает встроенные сценарии для обработки ошибок в приложениях, включая страницу исключений разработчика, настраиваемые страницы ошибок, страницы статических кодов состояний и обработку исключений при запуске.</span><span class="sxs-lookup"><span data-stu-id="b8d97-213">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="b8d97-214">Дополнительные сведения см. в разделе <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-214">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="b8d97-215">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="b8d97-215">Routing</span></span>

<span data-ttu-id="b8d97-216">В ASP.NET Core присутствуют сценарии для маршрутизации запросов приложений обработчикам маршрутов.</span><span class="sxs-lookup"><span data-stu-id="b8d97-216">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="b8d97-217">Дополнительные сведения см. в разделе <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="b8d97-218">Фоновые задачи</span><span class="sxs-lookup"><span data-stu-id="b8d97-218">Background tasks</span></span>

<span data-ttu-id="b8d97-219">Фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="b8d97-219">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="b8d97-220">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-220">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="b8d97-221">Дополнительные сведения см. в разделе <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-221">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="b8d97-222">Доступ к HttpContext</span><span class="sxs-lookup"><span data-stu-id="b8d97-222">Access HttpContext</span></span>

<span data-ttu-id="b8d97-223">Доступ к `HttpContext` предоставляется автоматически при обработке запросов с Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="b8d97-223">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="b8d97-224">Если к `HttpContext` нет доступа, вы можете получить доступ к `HttpContext` через интерфейс <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> и реализацию по умолчанию <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-224">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="b8d97-225">Дополнительные сведения см. в разделе <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="b8d97-225">For more information, see <xref:fundamentals/httpcontext>.</span></span>
