---
title: Запуск приложения в ASP.NET Core
author: rick-anderson
description: Сведения о том, как класс Startup в ASP.NET Core настраивает службы и конвейер запросов приложения.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: fundamentals/startup
ms.openlocfilehash: 2468c685850f74b8dafb3e0abea6d7b83c417af0
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880522"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="82495-103">Запуск приложения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82495-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="82495-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Люк Лэтэм](https://github.com/guardrex) (Luke Latham) и [Стив Смит](https://ardalis.com) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="82495-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="82495-105">Класс `Startup` настраивает службы и конвейер запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="82495-106">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="82495-106">The Startup class</span></span>

<span data-ttu-id="82495-107">Приложения ASP.NET Core используют класс `Startup`, который по соглашению называется `Startup`.</span><span class="sxs-lookup"><span data-stu-id="82495-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="82495-108">Класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="82495-108">The `Startup` class:</span></span>

* <span data-ttu-id="82495-109">При необходимости содержит метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> для настройки *служб* приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="82495-110">Служба — многократно используемый компонент, обеспечивающий функциональность приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="82495-111">Службы *регистрируются* в `ConfigureServices` и используются в приложении с помощью [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection) или <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span><span class="sxs-lookup"><span data-stu-id="82495-111">Services are *registered* in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="82495-112">Содержит метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> для создания конвейера обработки запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="82495-113">`ConfigureServices` и `Configure` вызываются средой выполнения ASP.NET Core при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="82495-113">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="82495-114">Предыдущий пример предназначен для [Razor Pages](xref:razor-pages/index); версия для MVC похожа.</span><span class="sxs-lookup"><span data-stu-id="82495-114">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

<span data-ttu-id="82495-115">Класс `Startup` указывается при создании [узла](xref:fundamentals/index#host) приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-115">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="82495-116">Класс `Startup` обычно указывается путем вызова метода [WebHostBuilderExtensions.UseStartup\<TStartup >](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) в построителе узлов.</span><span class="sxs-lookup"><span data-stu-id="82495-116">The `Startup` class is typically specified by calling the [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="82495-117">Узел предоставляет службы, которые доступны конструктору классов `Startup`.</span><span class="sxs-lookup"><span data-stu-id="82495-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="82495-118">Приложение добавляет дополнительные службы через `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="82495-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="82495-119">Как службы узла, так и службы приложения доступны в `Configure` и во всем приложении.</span><span class="sxs-lookup"><span data-stu-id="82495-119">Both the host and app services are available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="82495-120">При использовании [универсального узла](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>) в конструктор `Startup` могут внедряться только следующие типы служб:</span><span class="sxs-lookup"><span data-stu-id="82495-120">Only the following service types can be injected into the `Startup` constructor when using the [Generic Host](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

<span data-ttu-id="82495-121">Большинство служб недоступны, пока не будет вызван метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="82495-121">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="82495-122">Узел предоставляет службы, которые доступны конструктору классов `Startup`.</span><span class="sxs-lookup"><span data-stu-id="82495-122">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="82495-123">Приложение добавляет дополнительные службы через `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="82495-123">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="82495-124">Как службы узла, так и службы приложения доступны в `Configure` и во всем приложении.</span><span class="sxs-lookup"><span data-stu-id="82495-124">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="82495-125">Типичным применением [внедрения зависимостей](xref:fundamentals/dependency-injection) в класс `Startup` является внедрение:</span><span class="sxs-lookup"><span data-stu-id="82495-125">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="82495-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> для настройки служб средой;</span><span class="sxs-lookup"><span data-stu-id="82495-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="82495-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> для чтения конфигурации;</span><span class="sxs-lookup"><span data-stu-id="82495-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="82495-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> для создания средства ведения журнала в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="82495-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

<span data-ttu-id="82495-129">Большинство служб недоступны, пока не будет вызван метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="82495-129">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

### <a name="multiple-startup"></a><span data-ttu-id="82495-130">Несколько запусков</span><span class="sxs-lookup"><span data-stu-id="82495-130">Multiple Startup</span></span>

<span data-ttu-id="82495-131">Когда приложение определяет отдельные классы `Startup` для различных сред (например, `StartupDevelopment`), подходящий класс `Startup` выбирается во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="82495-131">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="82495-132">Класс, у которого суффикс имени соответствует текущей среде, получает приоритет.</span><span class="sxs-lookup"><span data-stu-id="82495-132">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="82495-133">Если приложение выполняется в среде разработки и включает в себя оба класса — `Startup` и `StartupDevelopment`, используется класс `StartupDevelopment`.</span><span class="sxs-lookup"><span data-stu-id="82495-133">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="82495-134">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="82495-134">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="82495-135">Дополнительные сведения об узле см. в разделе [Узел](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="82495-135">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="82495-136">Сведения об обработке ошибок во время запуска см. в разделе [Обработка исключений при запуске](xref:fundamentals/error-handling#startup-exception-handling).</span><span class="sxs-lookup"><span data-stu-id="82495-136">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="82495-137">Метод ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="82495-137">The ConfigureServices method</span></span>

<span data-ttu-id="82495-138">Метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>:</span><span class="sxs-lookup"><span data-stu-id="82495-138">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="82495-139">Необязательный параметр.</span><span class="sxs-lookup"><span data-stu-id="82495-139">Optional.</span></span>
* <span data-ttu-id="82495-140">Вызывается узлом перед методом `Configure` для настройки служб приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-140">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="82495-141">По соглашению используется для задания [параметров конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="82495-141">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="82495-142">Узел может настраивать некоторые службы перед вызовом методов `Startup`.</span><span class="sxs-lookup"><span data-stu-id="82495-142">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="82495-143">Дополнительную информацию см. в разделе [Узел](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="82495-143">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="82495-144">Для функций, нуждающихся в значительной настройке, существуют методы расширения `Add{Service}` в <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span><span class="sxs-lookup"><span data-stu-id="82495-144">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="82495-145">Например, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores и **Add**RazorPages:</span><span class="sxs-lookup"><span data-stu-id="82495-145">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

<span data-ttu-id="82495-146">Добавление служб в контейнер служб делает их доступными в приложении и в методе `Configure`.</span><span class="sxs-lookup"><span data-stu-id="82495-146">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="82495-147">Службы разрешаются посредством [внедрения зависимостей](xref:fundamentals/dependency-injection) или из <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span><span class="sxs-lookup"><span data-stu-id="82495-147">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="82495-148">Подробнее о `SetCompatibilityVersion` см. в сведениях о [SetCompatibilityVersion](xref:mvc/compatibility-version).</span><span class="sxs-lookup"><span data-stu-id="82495-148">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

::: moniker-end

## <a name="the-configure-method"></a><span data-ttu-id="82495-149">Метод Configure </span><span class="sxs-lookup"><span data-stu-id="82495-149">The Configure method</span></span>

<span data-ttu-id="82495-150">Метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> используется для указания того, как приложение реагирует на HTTP-запросы.</span><span class="sxs-lookup"><span data-stu-id="82495-150">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="82495-151">Конвейер запросов настраивается путем добавления компонентов [ПО промежуточного слоя](xref:fundamentals/middleware/index) в экземпляр <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="82495-151">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="82495-152">`IApplicationBuilder` доступен для метода `Configure`, но он не зарегистрирован в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="82495-152">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="82495-153">При размещении создается `IApplicationBuilder` и передается непосредственно в `Configure`.</span><span class="sxs-lookup"><span data-stu-id="82495-153">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="82495-154">[Шаблоны ASP.NET Core](/dotnet/core/tools/dotnet-new) настраивают конвейер с поддержкой следующих компонентов и функций:</span><span class="sxs-lookup"><span data-stu-id="82495-154">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="82495-155">Страница со сведениями об исключении для разработчика</span><span class="sxs-lookup"><span data-stu-id="82495-155">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* <span data-ttu-id="82495-156">[обработчиков исключений](xref:fundamentals/error-handling#exception-handler-page);</span><span class="sxs-lookup"><span data-stu-id="82495-156">[Exception handler](xref:fundamentals/error-handling#exception-handler-page)</span></span>
* [<span data-ttu-id="82495-157">HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="82495-157">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* <span data-ttu-id="82495-158">[перенаправления HTTPS](xref:security/enforcing-ssl);</span><span class="sxs-lookup"><span data-stu-id="82495-158">[HTTPS redirection](xref:security/enforcing-ssl)</span></span>
* [<span data-ttu-id="82495-159">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="82495-159">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="82495-160">ASP.NET Core [MVC](xref:mvc/overview) и [Razor Pages](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="82495-160">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="82495-161">[общего регламента по защите данных (GDPR)](xref:security/gdpr);</span><span class="sxs-lookup"><span data-stu-id="82495-161">[General Data Protection Regulation (GDPR)](xref:security/gdpr)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="82495-162">Предыдущий пример предназначен для [Razor Pages](xref:razor-pages/index); версия для MVC похожа.</span><span class="sxs-lookup"><span data-stu-id="82495-162">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

<span data-ttu-id="82495-163">Каждый метод расширения `Use` добавляет один или несколько компонентов ПО промежуточного слоя в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="82495-163">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="82495-164">Например, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> настраивает [ПО промежуточного слоя](xref:fundamentals/middleware/index) для обслуживания [статических файлов](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="82495-164">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="82495-165">Каждый компонент ПО промежуточного слоя в конвейере запросов отвечает за вызов следующего компонента в конвейере или замыкает цепочку, если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="82495-165">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="82495-166">В сигнатуре метода `Configure` могут быть указаны дополнительные службы, такие как `IWebHostEnvironment`, `ILoggerFactory` или любые службы, определенные в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="82495-166">Additional services, such as `IWebHostEnvironment`, `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="82495-167">Эти службы внедряются, если доступны.</span><span class="sxs-lookup"><span data-stu-id="82495-167">These services are injected if they're available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="82495-168">В сигнатуре метода `Configure` могут быть указаны дополнительные службы, такие как `IHostingEnvironment`, `ILoggerFactory` или любые службы, определенные в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="82495-168">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="82495-169">Эти службы внедряются, если доступны.</span><span class="sxs-lookup"><span data-stu-id="82495-169">These services are injected if they're available.</span></span>

::: moniker-end

<span data-ttu-id="82495-170">Дополнительные сведения об использовании `IApplicationBuilder` и порядке обработки ПО промежуточного слоя см. в статье <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="82495-170">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="82495-171">Настройка служб без запуска</span><span class="sxs-lookup"><span data-stu-id="82495-171">Configure services without Startup</span></span>

<span data-ttu-id="82495-172">Для настройки служб и конвейера обработки запросов в построителе узлов вместо класса `Startup` можно использовать удобные методы `ConfigureServices` и `Configure`.</span><span class="sxs-lookup"><span data-stu-id="82495-172">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="82495-173">Несколько вызовов `ConfigureServices` добавляются друг к другу.</span><span class="sxs-lookup"><span data-stu-id="82495-173">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="82495-174">При наличии нескольких вызовов метода `Configure` используется последний вызов `Configure`.</span><span class="sxs-lookup"><span data-stu-id="82495-174">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="82495-175">Расширение класса Startup с использованием фильтров запуска</span><span class="sxs-lookup"><span data-stu-id="82495-175">Extend Startup with startup filters</span></span>

<span data-ttu-id="82495-176">Используйте <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="82495-176">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span></span>

* <span data-ttu-id="82495-177">Для настройки ПО промежуточного слоя в начале или конце конвейера ПО промежуточного слоя [Configure](#the-configure-method) приложения без явного вызова `Use{Middleware}`.</span><span class="sxs-lookup"><span data-stu-id="82495-177">To configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline without an explicit call to `Use{Middleware}`.</span></span> <span data-ttu-id="82495-178">ASP.NET Core использует `IStartupFilter` для добавления значений по умолчанию в начало конвейера, исключая необходимость явной регистрации ПО промежуточного слоя по умолчанию автором приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-178">`IStartupFilter` is used by ASP.NET Core to add defaults to the beginning of the pipeline without having to make the app author explicitly register the default middleware.</span></span> <span data-ttu-id="82495-179">`IStartupFilter` разрешает другим компонентам вызывать `Use{Middleware}` от имени автора приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-179">`IStartupFilter` allows a different component call `Use{Middleware}` on behalf of the app author.</span></span>
* <span data-ttu-id="82495-180">Для создания конвейера методов `Configure`.</span><span class="sxs-lookup"><span data-stu-id="82495-180">To create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="82495-181">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) может обеспечивать запуск ПО промежуточного слоя до или после ПО промежуточного слоя, добавляемого библиотеками.</span><span class="sxs-lookup"><span data-stu-id="82495-181">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="82495-182">`IStartupFilter` реализует метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, который принимает и возвращает `Action<IApplicationBuilder>`.</span><span class="sxs-lookup"><span data-stu-id="82495-182">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="82495-183"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> определяет класс для настройки конвейера запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-183">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="82495-184">Дополнительные сведения см. в разделе [Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="82495-184">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="82495-185">Каждый интерфейс `IStartupFilter` может добавлять один или несколько компонентов ПО промежуточного слоя в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="82495-185">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="82495-186">Фильтры вызываются в том порядке, в котором они были добавлены в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="82495-186">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="82495-187">Фильтры могут добавлять ПО промежуточного слоя до или после передачи управления следующему фильтру, поэтому они добавляются в начало или конец конвейера приложения.</span><span class="sxs-lookup"><span data-stu-id="82495-187">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="82495-188">В следующем примере показана регистрация ПО промежуточного слоя с помощью `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="82495-188">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="82495-189">ПО промежуточного слоя `RequestSetOptionsMiddleware` задает значения параметров из параметра строки запроса:</span><span class="sxs-lookup"><span data-stu-id="82495-189">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="82495-190">`RequestSetOptionsMiddleware` настраивается в классе `RequestSetOptionsStartupFilter`:</span><span class="sxs-lookup"><span data-stu-id="82495-190">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="82495-191">`RequestSetOptionsMiddleware` настраивается в классе `RequestSetOptionsStartupFilter`:</span><span class="sxs-lookup"><span data-stu-id="82495-191">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

<span data-ttu-id="82495-192">`IStartupFilter` регистрируется в контейнере службы в <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span><span class="sxs-lookup"><span data-stu-id="82495-192">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

<span data-ttu-id="82495-193">Если указан параметр строки запроса для `option`, ПО промежуточного слоя обрабатывает присвоение значения до того, как ПО промежуточного слоя ASP.NET Core отображает ответ.</span><span class="sxs-lookup"><span data-stu-id="82495-193">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="82495-194">Порядок выполнения ПО промежуточного слоя определяется порядком регистраций `IStartupFilter`:</span><span class="sxs-lookup"><span data-stu-id="82495-194">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="82495-195">Несколько реализаций `IStartupFilter` могут взаимодействовать с одними и теми же объектами.</span><span class="sxs-lookup"><span data-stu-id="82495-195">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="82495-196">Если важен порядок, упорядочите регистрации службы `IStartupFilter` в соответствии с требуемым порядком выполнения ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="82495-196">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="82495-197">Библиотеки могут добавлять ПО промежуточного слоя с одной или несколькими реализациями `IStartupFilter`, которые выполняются до или после другого ПО промежуточного слоя приложения, зарегистрированного с помощью `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="82495-197">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="82495-198">Чтобы вызвать ПО промежуточного слоя `IStartupFilter` до ПО промежуточного слоя, добавляемого библиотекой `IStartupFilter`, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="82495-198">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="82495-199">расположите регистрацию службы до добавления библиотеки в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="82495-199">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="82495-200">Чтобы вызвать его после этого момента, расположите регистрацию службы после добавления библиотеки.</span><span class="sxs-lookup"><span data-stu-id="82495-200">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="82495-201">Добавление конфигурации из внешней сборки при запуске</span><span class="sxs-lookup"><span data-stu-id="82495-201">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="82495-202">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="82495-202">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="82495-203">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="82495-203">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82495-204">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="82495-204">Additional resources</span></span>

* [<span data-ttu-id="82495-205">Узел</span><span class="sxs-lookup"><span data-stu-id="82495-205">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
