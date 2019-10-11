---
title: Ведение журнала в .NET Core и ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать платформу ведения журналов, предоставляемую пакетом NuGet Microsoft.Extensions.Logging.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 697e6cf0cd1b51ad6c2942e21bc084d1fe6bfa4e
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259731"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="73f8f-103">Ведение журнала в .NET Core и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73f8f-103">Logging in .NET Core and ASP.NET Core</span></span>

<span data-ttu-id="73f8f-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="73f8f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="73f8f-105">.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками.</span><span class="sxs-lookup"><span data-stu-id="73f8f-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="73f8f-106">В этой статье описано, как использовать в коде API ведения журналов, работающего со встроенными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="73f8f-106">This article shows how to use the logging API with built-in providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="73f8f-107">Большинство примеров кода, приведенных в этой статье, взяты из приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73f8f-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="73f8f-108">Части этих фрагментов кода, относящиеся к ведению журнала, применимы ко всем приложениям .NET Core, использующим [универсальный узел](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="73f8f-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="73f8f-109">Сведения об использовании универсального узла в приложениях, не выполняющихся в веб-консоли, см. в разделе [Размещенные службы](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="73f8f-109">For information about how to use the Generic Host in non-web console apps, see [Hosted services](xref:fundamentals/host/hosted-services).</span></span>

<span data-ttu-id="73f8f-110">Код ведения журнала для приложений без универсального узла отличается тем, как [добавляются поставщики](#add-providers) и [создаются средства ведения журнала](#create-logs).</span><span class="sxs-lookup"><span data-stu-id="73f8f-110">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> <span data-ttu-id="73f8f-111">Примеры кода, не связанные с размещением, приведены в соответствующих разделах статьи.</span><span class="sxs-lookup"><span data-stu-id="73f8f-111">Non-host code examples are shown in those sections of the article.</span></span>

::: moniker-end

<span data-ttu-id="73f8f-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="73f8f-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="73f8f-113">Добавление поставщиков</span><span class="sxs-lookup"><span data-stu-id="73f8f-113">Add providers</span></span>

<span data-ttu-id="73f8f-114">Поставщик ведения журналов отображает или сохраняет журналы.</span><span class="sxs-lookup"><span data-stu-id="73f8f-114">A logging provider displays or stores logs.</span></span> <span data-ttu-id="73f8f-115">Например, поставщик Console отображает журналы на консоли, а поставщик Azure Application Insights может сохранить их в Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="73f8f-115">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="73f8f-116">Журналы могут отправляться в несколько назначений после добавления нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="73f8f-116">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="73f8f-117">Чтобы добавить поставщик в приложение, использующее универсальный узел, вызовите метод расширения `Add{provider name}` поставщика в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="73f8f-117">To add a provider in an app that uses Generic Host, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

<span data-ttu-id="73f8f-118">В консольном приложении, не использующем узел, вызовите метод расширения `Add{provider name}` поставщика при создании `LoggerFactory`:</span><span class="sxs-lookup"><span data-stu-id="73f8f-118">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

<span data-ttu-id="73f8f-119">`LoggerFactory` и `AddConsole` требуют оператор `using` для `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-119">`LoggerFactory` and `AddConsole` require a `using` statement for `Microsoft.Extensions.Logging`.</span></span>

<span data-ttu-id="73f8f-120">Шаблоны проектов ASP.NET Core по умолчанию вызывают метод <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, который добавляет следующие поставщики ведения журналов:</span><span class="sxs-lookup"><span data-stu-id="73f8f-120">The default ASP.NET Core project templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="73f8f-121">Консоль</span><span class="sxs-lookup"><span data-stu-id="73f8f-121">Console</span></span>
* <span data-ttu-id="73f8f-122">Отладка</span><span class="sxs-lookup"><span data-stu-id="73f8f-122">Debug</span></span>
* <span data-ttu-id="73f8f-123">EventSource</span><span class="sxs-lookup"><span data-stu-id="73f8f-123">EventSource</span></span>
* <span data-ttu-id="73f8f-124">Журнал событий (только при запуске в Windows)</span><span class="sxs-lookup"><span data-stu-id="73f8f-124">EventLog (only when running on Windows)</span></span>

<span data-ttu-id="73f8f-125">Поставщики по умолчанию можно заменить собственными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="73f8f-125">You can replace the default providers with your own choices.</span></span> <span data-ttu-id="73f8f-126">Вызовите <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> и добавьте требуемые поставщики.</span><span class="sxs-lookup"><span data-stu-id="73f8f-126">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0 "

<span data-ttu-id="73f8f-127">Для добавления поставщика вызовите метод расширения `Add{provider name}` поставщика в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="73f8f-127">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="73f8f-128">В указанном выше коде необходимо указать ссылки на `Microsoft.Extensions.Logging` и `Microsoft.Extensions.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-128">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="73f8f-129">Шаблон проекта по умолчанию вызывает метод <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, который добавляет следующие поставщики ведения журналов:</span><span class="sxs-lookup"><span data-stu-id="73f8f-129">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="73f8f-130">Консоль</span><span class="sxs-lookup"><span data-stu-id="73f8f-130">Console</span></span>
* <span data-ttu-id="73f8f-131">Отладка</span><span class="sxs-lookup"><span data-stu-id="73f8f-131">Debug</span></span>
* <span data-ttu-id="73f8f-132">EventSource (начиная с версии ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="73f8f-132">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="73f8f-133">Если вы используете `CreateDefaultBuilder`, поставщики по умолчанию можно заменить собственными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="73f8f-133">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="73f8f-134">Вызовите <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> и добавьте требуемые поставщики.</span><span class="sxs-lookup"><span data-stu-id="73f8f-134">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

<span data-ttu-id="73f8f-135">См. сведения о [встроенных](#built-in-logging-providers) и [сторонних](#third-party-logging-providers) поставщиках ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-135">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="73f8f-136">Создание журналов</span><span class="sxs-lookup"><span data-stu-id="73f8f-136">Create logs</span></span>

<span data-ttu-id="73f8f-137">Чтобы создать журналы, используйте объект <xref:Microsoft.Extensions.Logging.ILogger%601>.</span><span class="sxs-lookup"><span data-stu-id="73f8f-137">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="73f8f-138">В веб-приложении или размещенной службе получите `ILogger` посредством внедрения зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="73f8f-138">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="73f8f-139">В консольных приложениях без размещения используйте `LoggerFactory` для создания `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-139">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="73f8f-140">В приведенном ниже примере для ASP.NET Core создается средство ведения журнала с категорией `TodoApiSample.Pages.AboutModel`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-140">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="73f8f-141">*Категория* ведения журналов представляет строку, которая связана с каждым журналом.</span><span class="sxs-lookup"><span data-stu-id="73f8f-141">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="73f8f-142">Экземпляр `ILogger<T>`, предоставляемый внедрением зависимостей, создает журналы с полным именем типа `T` в качестве категории.</span><span class="sxs-lookup"><span data-stu-id="73f8f-142">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="73f8f-143">В приведенном ниже примере консольного приложения без размещения создается средство ведения журнала с категорией `LoggingConsoleApp.Program`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-143">The following non-host console app example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

::: moniker-end

<span data-ttu-id="73f8f-144">В приведенных ниже примерах ASP.NET Core и консольного приложения средство ведения журнала используется для создания журналов с уровнем `Information`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-144">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="73f8f-145">*Уровень* ведения журналов определяет степень серьезности или важности записанного в журнал события.</span><span class="sxs-lookup"><span data-stu-id="73f8f-145">The Log *level* indicates the severity of the logged event.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

<span data-ttu-id="73f8f-146">[Уровни](#log-level) и [категории](#log-category) рассматриваются подробнее далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="73f8f-146">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-3.0"

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="73f8f-147">Создание журналов в классе Program</span><span class="sxs-lookup"><span data-stu-id="73f8f-147">Create logs in the Program class</span></span>

<span data-ttu-id="73f8f-148">Для записи журналов в классе `Program` приложения ASP.NET Core получите экземпляр `ILogger` путем внедрения зависимостей после создания узла:</span><span class="sxs-lookup"><span data-stu-id="73f8f-148">To write logs in the `Program` class of an ASP.NET Core app, get an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

### <a name="create-logs-in-the-startup-class"></a><span data-ttu-id="73f8f-149">Создание журналов в классе Startup</span><span class="sxs-lookup"><span data-stu-id="73f8f-149">Create logs in the Startup class</span></span>

<span data-ttu-id="73f8f-150">Для записи журналов в методе `Startup.Configure` приложения ASP.NET Core включите параметр `ILogger` в сигнатуру метода:</span><span class="sxs-lookup"><span data-stu-id="73f8f-150">To write logs in the `Startup.Configure` method of an ASP.NET Core app, include an `ILogger` parameter in the method signature:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

<span data-ttu-id="73f8f-151">Запись журналов до завершения настройки контейнера внедрения зависимостей в методе `Startup.ConfigureServices` не поддерживается:</span><span class="sxs-lookup"><span data-stu-id="73f8f-151">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="73f8f-152">Внедрение средства ведения журнала в конструктор `Startup` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="73f8f-152">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="73f8f-153">Внедрение средства ведения журнала в сигнатуру метода `Startup.ConfigureServices` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="73f8f-153">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="73f8f-154">Причина этого ограничения заключается в том, что ведение журнала зависит от внедрения зависимостей и от конфигурации, которая, в свою очередь, зависит от внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="73f8f-154">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="73f8f-155">Контейнер внедрения зависимостей не настраивается, пока не завершится выполнение `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-155">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="73f8f-156">Внедрение средства ведения журнала в класс `Startup` посредством конструктора работает в более ранних версиях ASP.NET Core, так как для веб-узла создается отдельный контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="73f8f-156">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="73f8f-157">Сведения о том, почему для универсального узла создается только один контейнер, см. в [объявлении о критическом изменении](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="73f8f-157">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="73f8f-158">Если необходимо настроить службу, которая зависит от `ILogger<T>`, это можно сделать с помощью внедрения конструктора или путем предоставления фабричного метода.</span><span class="sxs-lookup"><span data-stu-id="73f8f-158">If you need to configure a service that depends on `ILogger<T>`, you can still do that by using constructor injection or by providing a factory method.</span></span> <span data-ttu-id="73f8f-159">Фабричный метод рекомендуется использовать, только если нет других вариантов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-159">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="73f8f-160">Например, предположим, что необходимо заполнить свойство службой из внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="73f8f-160">For example, suppose you need to fill a property with a service from DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="73f8f-161">Выделенный выше код — это функция `Func`, которая выполняется, когда контейнеру внедрения зависимостей впервые требуется создать экземпляр `MyService`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-161">The preceding highlighted code is a `Func` that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="73f8f-162">Таким образом можно обращаться к любым зарегистрированным службам.</span><span class="sxs-lookup"><span data-stu-id="73f8f-162">You can access any of the registered services in this way.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="73f8f-163">Создание журналов в классе Startup</span><span class="sxs-lookup"><span data-stu-id="73f8f-163">Create logs in Startup</span></span>

<span data-ttu-id="73f8f-164">Для записи журналов в классе `Startup` включите параметр `ILogger` в сигнатуру конструктора:</span><span class="sxs-lookup"><span data-stu-id="73f8f-164">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="73f8f-165">Создание журналов в классе Program</span><span class="sxs-lookup"><span data-stu-id="73f8f-165">Create logs in the Program class</span></span>

<span data-ttu-id="73f8f-166">Для записи журналов в классе `Program` получите экземпляр `ILogger` путем внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="73f8f-166">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="73f8f-167">Асинхронные методы ведения журналов не выполняются</span><span class="sxs-lookup"><span data-stu-id="73f8f-167">No asynchronous logger methods</span></span>

<span data-ttu-id="73f8f-168">Скорость ведения журналов не должна влиять на производительность выполнения асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="73f8f-168">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="73f8f-169">Если хранилище данных, предназначенное для регистрации сообщений журнала, работает медленно, сначала записывайте эти сообщения в быстродействующее хранилище,</span><span class="sxs-lookup"><span data-stu-id="73f8f-169">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="73f8f-170">а затем перемещайте их в медленное хранилище.</span><span class="sxs-lookup"><span data-stu-id="73f8f-170">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="73f8f-171">Например, если вы входите в SQL Server, вам не нужно делать это непосредственно в методе `Log`, так как методы `Log` являются синхронными.</span><span class="sxs-lookup"><span data-stu-id="73f8f-171">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="73f8f-172">Вместо этого синхронно добавьте сообщения журнала в очередь в памяти, и фоновый рабочий поток извлечет сообщения из очереди для выполнения асинхронных операций передачи данных в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="73f8f-172">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="73f8f-173">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="73f8f-173">Configuration</span></span>

<span data-ttu-id="73f8f-174">Конфигурацию поставщика ведения журналов предоставляет как минимум один поставщик конфигураций:</span><span class="sxs-lookup"><span data-stu-id="73f8f-174">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="73f8f-175">Форматы файлов (INI, JSON и XML).</span><span class="sxs-lookup"><span data-stu-id="73f8f-175">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="73f8f-176">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="73f8f-176">Command-line arguments.</span></span>
* <span data-ttu-id="73f8f-177">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="73f8f-177">Environment variables.</span></span>
* <span data-ttu-id="73f8f-178">Объекты .NET в памяти.</span><span class="sxs-lookup"><span data-stu-id="73f8f-178">In-memory .NET objects.</span></span>
* <span data-ttu-id="73f8f-179">Незашифрованное хранилище [Secret Manager](xref:security/app-secrets) (Диспетчер секретов).</span><span class="sxs-lookup"><span data-stu-id="73f8f-179">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="73f8f-180">Зашифрованное пользовательское хранилище, например [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="73f8f-180">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="73f8f-181">Пользовательские поставщики (установленные или созданные).</span><span class="sxs-lookup"><span data-stu-id="73f8f-181">Custom providers (installed or created).</span></span>

<span data-ttu-id="73f8f-182">Например, конфигурацию ведения журналов обычно предоставляет раздел `Logging` в файле параметров приложения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-182">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="73f8f-183">В следующем примере показано содержимое типичного файла *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="73f8f-183">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

<span data-ttu-id="73f8f-184">Свойство `Logging` может иметь свойство `LogLevel` и свойства поставщика журналов (здесь — Console).</span><span class="sxs-lookup"><span data-stu-id="73f8f-184">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="73f8f-185">Свойство `LogLevel` в разделе `Logging` указывает минимальный [уровень](#log-level) журнала для выбранных категорий.</span><span class="sxs-lookup"><span data-stu-id="73f8f-185">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="73f8f-186">В этом примере категории `System` и `Microsoft` записываются на уровне `Information`, а остальные — на уровне `Debug`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-186">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="73f8f-187">Другие свойства в разделе `Logging` определяют поставщиков ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-187">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="73f8f-188">Пример приведен для поставщика Console.</span><span class="sxs-lookup"><span data-stu-id="73f8f-188">The example is for the Console provider.</span></span> <span data-ttu-id="73f8f-189">Если поставщик поддерживает [области журналов](#log-scopes), `IncludeScopes` определяет, включены ли они.</span><span class="sxs-lookup"><span data-stu-id="73f8f-189">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="73f8f-190">Свойство поставщика (например, `Console` в этом примере) также определять свойство `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-190">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="73f8f-191">`LogLevel` под поставщиком указывает уровни журнала для этого поставщика.</span><span class="sxs-lookup"><span data-stu-id="73f8f-191">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="73f8f-192">Если уровни указаны в `Logging.{providername}.LogLevel`, они не переопределяют ничего из того, что задано в `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-192">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

<span data-ttu-id="73f8f-193">Сведения о реализации поставщиков конфигураций см. в статье <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="73f8f-193">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="73f8f-194">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="73f8f-194">Sample logging output</span></span>

<span data-ttu-id="73f8f-195">В примере кода из предыдущего раздела журналы появляются в консоли после запуска приложения из командной строки.</span><span class="sxs-lookup"><span data-stu-id="73f8f-195">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="73f8f-196">Ниже приведен пример выходных данных консоли:</span><span class="sxs-lookup"><span data-stu-id="73f8f-196">Here's an example of console output:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 84.26180000000001ms 307
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/api/todo/0
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

::: moniker-end

<span data-ttu-id="73f8f-197">Предыдущие журналы созданы путем отправки HTTP-запроса Get к примеру приложения по адресу `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-197">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="73f8f-198">Ниже приведен пример этих журналов в том виде, в каком они отображаются в окне отладки при запуске примера приложения в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73f8f-198">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request starting HTTP/2.0 GET https://localhost:44328/api/todo/0  
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
TodoApiSample.Controllers.TodoController: Information: Getting item 0
TodoApiSample.Controllers.TodoController: Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult: Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 34.167ms
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request finished in 98.41300000000001ms 404
```

<span data-ttu-id="73f8f-199">Журналы, которые созданы путем вызовов `ILogger`, как показано в предыдущем разделе, начинаются с TodoApiSample.</span><span class="sxs-lookup"><span data-stu-id="73f8f-199">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApiSample".</span></span> <span data-ttu-id="73f8f-200">Журналы, которые начинаются с категорий Microsoft, входят в код платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73f8f-200">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="73f8f-201">В ASP.NET Core и коде приложения используется один и тот же API ведения журналов и одни и те же поставщики.</span><span class="sxs-lookup"><span data-stu-id="73f8f-201">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="73f8f-202">Журналы, которые созданы путем вызовов `ILogger`, как показано в предыдущем разделе, начинаются с TodoApi.</span><span class="sxs-lookup"><span data-stu-id="73f8f-202">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="73f8f-203">Журналы, которые начинаются с категорий Microsoft, входят в код платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73f8f-203">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="73f8f-204">В ASP.NET Core и коде приложения используется один и тот же API ведения журналов и одни и те же поставщики.</span><span class="sxs-lookup"><span data-stu-id="73f8f-204">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

<span data-ttu-id="73f8f-205">В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-205">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="73f8f-206">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="73f8f-206">NuGet packages</span></span>

<span data-ttu-id="73f8f-207">Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="73f8f-207">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="73f8f-208">Категория журнала</span><span class="sxs-lookup"><span data-stu-id="73f8f-208">Log category</span></span>

<span data-ttu-id="73f8f-209">При создании объекта `ILogger` для него указывается *категория*.</span><span class="sxs-lookup"><span data-stu-id="73f8f-209">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="73f8f-210">Эта категория входит в состав каждого сообщения журнала, создаваемого этим экземпляром `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-210">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="73f8f-211">Категория может быть любой строкой, обычно используется имя класса, например TodoApi.Controllers.TodoController.</span><span class="sxs-lookup"><span data-stu-id="73f8f-211">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="73f8f-212">Используйте `ILogger<T>` для получения экземпляра `ILogger`, который использует полное имя типа `T` в качестве категории:</span><span class="sxs-lookup"><span data-stu-id="73f8f-212">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="73f8f-213">Чтобы явно указать категорию, вызовите `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="73f8f-213">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="73f8f-214">Использование `ILogger<T>` эквивалентно вызову `CreateLogger` с полным именем типа `T`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-214">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="73f8f-215">Уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="73f8f-215">Log level</span></span>

<span data-ttu-id="73f8f-216">Каждый журнал задает значение <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="73f8f-216">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="73f8f-217">Уровень ведения журналов означает степень серьезности или важности.</span><span class="sxs-lookup"><span data-stu-id="73f8f-217">The log level indicates the severity or importance.</span></span> <span data-ttu-id="73f8f-218">Например, можно создать журнал `Information` при нормальном завершении метода и журнал `Warning`, если метод возвращает код состояния *404 — не найдено*.</span><span class="sxs-lookup"><span data-stu-id="73f8f-218">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="73f8f-219">Следующий код создает журналы `Information`и `Warning`:</span><span class="sxs-lookup"><span data-stu-id="73f8f-219">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="73f8f-220">В коде выше первый параметр — это [идентификатор события журнала](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="73f8f-220">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="73f8f-221">Второй параметр — это шаблон сообщения с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода.</span><span class="sxs-lookup"><span data-stu-id="73f8f-221">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="73f8f-222">Параметры метода рассматриваются более подробно в разделе, посвященном [шаблону сообщений](#log-message-template) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="73f8f-222">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="73f8f-223">Методы журналов, которые содержат уровень в имени метода (например, `LogInformation` и `LogWarning`), являются [методами расширения для ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="73f8f-223">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="73f8f-224">Эти методы вызывают метод `Log`, принимающий параметр `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-224">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="73f8f-225">Метод `Log`, в отличие от этих методов расширения, можно вызывать напрямую, однако его синтаксис довольно сложен.</span><span class="sxs-lookup"><span data-stu-id="73f8f-225">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="73f8f-226">См. сведения о <xref:Microsoft.Extensions.Logging.ILogger> и [исходном коде расширений средства ведения журналов](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="73f8f-226">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="73f8f-227">В ASP.NET Core определяются следующие уровни ведения журналов, упорядоченные по возрастанию уровней серьезности.</span><span class="sxs-lookup"><span data-stu-id="73f8f-227">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="73f8f-228">Трассировка = 0</span><span class="sxs-lookup"><span data-stu-id="73f8f-228">Trace = 0</span></span>

  <span data-ttu-id="73f8f-229">Для получения сведений, которые полезны только при отладке.</span><span class="sxs-lookup"><span data-stu-id="73f8f-229">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="73f8f-230">Эти сообщения могут содержать конфиденциальные данные приложения, поэтому их не следует включать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="73f8f-230">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="73f8f-231">*По умолчанию отключено.*</span><span class="sxs-lookup"><span data-stu-id="73f8f-231">*Disabled by default.*</span></span>

* <span data-ttu-id="73f8f-232">Отладка = 1</span><span class="sxs-lookup"><span data-stu-id="73f8f-232">Debug = 1</span></span>

  <span data-ttu-id="73f8f-233">Для получения сведений, которые полезны при разработке и отладке.</span><span class="sxs-lookup"><span data-stu-id="73f8f-233">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="73f8f-234">Пример `Entering method Configure with flag set to true.` Включайте уровни ведения журналов `Debug` в рабочей среде только при устранении неполадок, так как такие журналы занимают много места.</span><span class="sxs-lookup"><span data-stu-id="73f8f-234">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="73f8f-235">Информация = 2</span><span class="sxs-lookup"><span data-stu-id="73f8f-235">Information = 2</span></span>

  <span data-ttu-id="73f8f-236">Для отслеживания общего потока работы приложения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-236">For tracking the general flow of the app.</span></span> <span data-ttu-id="73f8f-237">Эти журналы обычно полезны в долгосрочной перспективе.</span><span class="sxs-lookup"><span data-stu-id="73f8f-237">These logs typically have some long-term value.</span></span> <span data-ttu-id="73f8f-238">Пример: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="73f8f-238">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="73f8f-239">Предупреждение = 3</span><span class="sxs-lookup"><span data-stu-id="73f8f-239">Warning = 3</span></span>

  <span data-ttu-id="73f8f-240">Для нестандартных или непредвиденных событий, возникающих в потоке работы приложения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-240">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="73f8f-241">Это могут быть ошибки или другие условия, которые не приводят к остановке приложения, но которые нужно изучить.</span><span class="sxs-lookup"><span data-stu-id="73f8f-241">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="73f8f-242">Обработанные исключения являются распространенным условием использования уровня ведения журнала `Warning`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-242">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="73f8f-243">Пример: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="73f8f-243">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="73f8f-244">Ошибка = 4</span><span class="sxs-lookup"><span data-stu-id="73f8f-244">Error = 4</span></span>

  <span data-ttu-id="73f8f-245">Для ошибок и исключений, которые не могут быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="73f8f-245">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="73f8f-246">Эти сообщения указывают на сбой текущего действия или операции (например, текущего HTTP-запроса), а не на ошибку уровня приложения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-246">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="73f8f-247">Пример сообщения журнала: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="73f8f-247">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="73f8f-248">Критический = 5</span><span class="sxs-lookup"><span data-stu-id="73f8f-248">Critical = 5</span></span>

  <span data-ttu-id="73f8f-249">Для сбоев, которые требуют немедленного внимания.</span><span class="sxs-lookup"><span data-stu-id="73f8f-249">For failures that require immediate attention.</span></span> <span data-ttu-id="73f8f-250">Примеры: потеря данных, нехватка места на диске.</span><span class="sxs-lookup"><span data-stu-id="73f8f-250">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="73f8f-251">Уровень ведения журналов можно использовать для управления объемом выходных данных, записываемых на определенный носитель или в окно отображения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-251">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="73f8f-252">Например:</span><span class="sxs-lookup"><span data-stu-id="73f8f-252">For example:</span></span>

* <span data-ttu-id="73f8f-253">В рабочей среде:</span><span class="sxs-lookup"><span data-stu-id="73f8f-253">In production:</span></span>
  * <span data-ttu-id="73f8f-254">При ведении журнала на уровнях с `Trace` по `Information` создается большой объем подробных сообщений журнала.</span><span class="sxs-lookup"><span data-stu-id="73f8f-254">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="73f8f-255">Чтобы контролировать затраты и не превысить лимиты объема хранилища данных, записывайте сообщения на уровнях с `Trace` по `Information` в хранилище данных с низкими затратами и большим объемом.</span><span class="sxs-lookup"><span data-stu-id="73f8f-255">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="73f8f-256">Ведение журнала на уровнях с `Warning` по `Critical` обычно приводит к записи меньшего количества сообщений меньшего размера.</span><span class="sxs-lookup"><span data-stu-id="73f8f-256">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="73f8f-257">Следовательно, затраты и лимиты хранилища не становятся проблемой, что приводит к большей гибкости при выборе хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="73f8f-257">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="73f8f-258">Во время разработки:</span><span class="sxs-lookup"><span data-stu-id="73f8f-258">During development:</span></span>
  * <span data-ttu-id="73f8f-259">Записывайте сообщения уровней с `Warning` по `Critical` на консоль.</span><span class="sxs-lookup"><span data-stu-id="73f8f-259">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="73f8f-260">Добавляйте сообщения уровней с `Trace` по `Information` при устранении неполадок.</span><span class="sxs-lookup"><span data-stu-id="73f8f-260">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="73f8f-261">В разделе [Фильтрация журналов](#log-filtering) далее в этой статье приводятся сведения о том, как контролировать уровни журнала, обрабатываемые поставщиком.</span><span class="sxs-lookup"><span data-stu-id="73f8f-261">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="73f8f-262">ASP.NET Core создает журналы для событий платформы.</span><span class="sxs-lookup"><span data-stu-id="73f8f-262">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="73f8f-263">В примерах журнала ранее в этой статье не указывались журналы с уровнем ниже `Information`, поэтому журналы уровня `Debug` или `Trace` не создавались.</span><span class="sxs-lookup"><span data-stu-id="73f8f-263">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="73f8f-264">Ниже приведен пример журналов консоли, которые созданы при запуске примера приложения, настроенного на отображение журналов `Debug`:</span><span class="sxs-lookup"><span data-stu-id="73f8f-264">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of authorization filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of resource filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of action filters (in the following order): Microsoft.AspNetCore.Mvc.Filters.ControllerActionFilter (Order: -2147483648), Microsoft.AspNetCore.Mvc.ModelBinding.UnsupportedContentTypeFilter (Order: -3000)
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of exception filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of result filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[22]
      Attempting to bind parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[44]
      Attempting to bind parameter 'id' of type 'System.String' using the name 'id' in request data ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[45]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[23]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[26]
      Attempting to validate the bound parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[27]
      Done attempting to validate the bound parameter 'id' of type 'System.String'.
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[2]
      Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 32.690400000000004ms
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 176.9103ms 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

::: moniker-end

## <a name="log-event-id"></a><span data-ttu-id="73f8f-265">Идентификатор события журнала</span><span class="sxs-lookup"><span data-stu-id="73f8f-265">Log event ID</span></span>

<span data-ttu-id="73f8f-266">Каждый журнал может указывать *идентификатор события*.</span><span class="sxs-lookup"><span data-stu-id="73f8f-266">Each log can specify an *event ID*.</span></span> <span data-ttu-id="73f8f-267">В примере приложения для этого используется локально определенный класс `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="73f8f-267">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="73f8f-268">Идентификатор события связывает набор событий.</span><span class="sxs-lookup"><span data-stu-id="73f8f-268">An event ID associates a set of events.</span></span> <span data-ttu-id="73f8f-269">Например, все журналы, связанные с отображением списка элементов на странице, могут иметь значение 1001.</span><span class="sxs-lookup"><span data-stu-id="73f8f-269">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="73f8f-270">Поставщик ведения журналов может хранить идентификатор события в поле идентификатора, в сообщении журнала, или вообще не хранить.</span><span class="sxs-lookup"><span data-stu-id="73f8f-270">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="73f8f-271">Поставщик Debug не отображает идентификаторы событий.</span><span class="sxs-lookup"><span data-stu-id="73f8f-271">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="73f8f-272">Поставщик Console отображает идентификаторы событий в квадратных скобках после категории:</span><span class="sxs-lookup"><span data-stu-id="73f8f-272">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="73f8f-273">Шаблон сообщения журнала</span><span class="sxs-lookup"><span data-stu-id="73f8f-273">Log message template</span></span>

<span data-ttu-id="73f8f-274">Каждый журнал указывает шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-274">Each log specifies a message template.</span></span> <span data-ttu-id="73f8f-275">Шаблон сообщения может содержать заполнители, для которых предоставляются аргументы.</span><span class="sxs-lookup"><span data-stu-id="73f8f-275">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="73f8f-276">Используйте для заполнителей имена, а не числа.</span><span class="sxs-lookup"><span data-stu-id="73f8f-276">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="73f8f-277">Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами.</span><span class="sxs-lookup"><span data-stu-id="73f8f-277">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="73f8f-278">В приведенном ниже коде обратите внимание на то, что имена параметров идут не по порядку в шаблоне сообщения:</span><span class="sxs-lookup"><span data-stu-id="73f8f-278">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="73f8f-279">Этот код создает сообщение журнала со значениями параметров в определенном порядке:</span><span class="sxs-lookup"><span data-stu-id="73f8f-279">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="73f8f-280">Платформа ведения журналов поддерживает такое поведение, чтобы поставщики ведения журнала могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="73f8f-280">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="73f8f-281">Сами аргументы передаются в систему ведения журналов, а не только в отформатированный шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-281">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="73f8f-282">Эта информация позволяет поставщикам ведения журналов хранить значения параметров как поля.</span><span class="sxs-lookup"><span data-stu-id="73f8f-282">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="73f8f-283">Предположим, например, что вызовы метода средства ведения журналов выглядят так:</span><span class="sxs-lookup"><span data-stu-id="73f8f-283">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="73f8f-284">Если вы отправляете журналы в Хранилище таблиц Azure, каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-284">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="73f8f-285">Запрос может находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-285">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="73f8f-286">Ведение журнала исключений</span><span class="sxs-lookup"><span data-stu-id="73f8f-286">Logging exceptions</span></span>

<span data-ttu-id="73f8f-287">Методы средства ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="73f8f-287">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="73f8f-288">Разные поставщики обрабатывают сведения об исключениях по-разному.</span><span class="sxs-lookup"><span data-stu-id="73f8f-288">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="73f8f-289">Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="73f8f-289">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="73f8f-290">Фильтрация журналов</span><span class="sxs-lookup"><span data-stu-id="73f8f-290">Log filtering</span></span>

<span data-ttu-id="73f8f-291">Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий.</span><span class="sxs-lookup"><span data-stu-id="73f8f-291">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="73f8f-292">Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются.</span><span class="sxs-lookup"><span data-stu-id="73f8f-292">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="73f8f-293">Чтобы проигнорировать все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-293">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="73f8f-294">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="73f8f-294">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="73f8f-295">Создание правил фильтрации в конфигурации</span><span class="sxs-lookup"><span data-stu-id="73f8f-295">Create filter rules in configuration</span></span>

<span data-ttu-id="73f8f-296">Код шаблона проектов вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug.</span><span class="sxs-lookup"><span data-stu-id="73f8f-296">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="73f8f-297">Метод `CreateDefaultBuilder` настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, как было описано [ранее в этой статье](#configuration).</span><span class="sxs-lookup"><span data-stu-id="73f8f-297">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="73f8f-298">Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="73f8f-298">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

::: moniker-end

<span data-ttu-id="73f8f-299">Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно для всех поставщиков.</span><span class="sxs-lookup"><span data-stu-id="73f8f-299">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="73f8f-300">При создании объекта `ILogger` для каждого поставщика выбирается только одно из этих правил.</span><span class="sxs-lookup"><span data-stu-id="73f8f-300">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="73f8f-301">Правила фильтрации в коде</span><span class="sxs-lookup"><span data-stu-id="73f8f-301">Filter rules in code</span></span>

<span data-ttu-id="73f8f-302">В следующем примере показано, как зарегистрировать в коде правила фильтрации:</span><span class="sxs-lookup"><span data-stu-id="73f8f-302">The following example shows how to register filter rules in code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

<span data-ttu-id="73f8f-303">Второй `AddFilter` указывает поставщика, Debug, используя имя его типа.</span><span class="sxs-lookup"><span data-stu-id="73f8f-303">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="73f8f-304">Первый `AddFilter` применяется ко всем поставщикам, так как он не определяет тип поставщика.</span><span class="sxs-lookup"><span data-stu-id="73f8f-304">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="73f8f-305">Применение правил фильтрации</span><span class="sxs-lookup"><span data-stu-id="73f8f-305">How filtering rules are applied</span></span>

<span data-ttu-id="73f8f-306">Данные конфигурации и код `AddFilter`, приведенный в предыдущих примерах, создают правила, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="73f8f-306">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="73f8f-307">Первые шесть взяты из примера конфигурации, а последние два — из примера кода.</span><span class="sxs-lookup"><span data-stu-id="73f8f-307">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="73f8f-308">Число</span><span class="sxs-lookup"><span data-stu-id="73f8f-308">Number</span></span> | <span data-ttu-id="73f8f-309">Поставщик</span><span class="sxs-lookup"><span data-stu-id="73f8f-309">Provider</span></span>      | <span data-ttu-id="73f8f-310">Категории, которые начинаются с...</span><span class="sxs-lookup"><span data-stu-id="73f8f-310">Categories that begin with ...</span></span>          | <span data-ttu-id="73f8f-311">Минимальный уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="73f8f-311">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="73f8f-312">1</span><span class="sxs-lookup"><span data-stu-id="73f8f-312">1</span></span>      | <span data-ttu-id="73f8f-313">Отладка</span><span class="sxs-lookup"><span data-stu-id="73f8f-313">Debug</span></span>         | <span data-ttu-id="73f8f-314">Все категории</span><span class="sxs-lookup"><span data-stu-id="73f8f-314">All categories</span></span>                          | <span data-ttu-id="73f8f-315">Сведения</span><span class="sxs-lookup"><span data-stu-id="73f8f-315">Information</span></span>       |
| <span data-ttu-id="73f8f-316">2</span><span class="sxs-lookup"><span data-stu-id="73f8f-316">2</span></span>      | <span data-ttu-id="73f8f-317">Консоль</span><span class="sxs-lookup"><span data-stu-id="73f8f-317">Console</span></span>       | <span data-ttu-id="73f8f-318">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="73f8f-318">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="73f8f-319">Предупреждение</span><span class="sxs-lookup"><span data-stu-id="73f8f-319">Warning</span></span>           |
| <span data-ttu-id="73f8f-320">3</span><span class="sxs-lookup"><span data-stu-id="73f8f-320">3</span></span>      | <span data-ttu-id="73f8f-321">Консоль</span><span class="sxs-lookup"><span data-stu-id="73f8f-321">Console</span></span>       | <span data-ttu-id="73f8f-322">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="73f8f-322">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="73f8f-323">Отладка</span><span class="sxs-lookup"><span data-stu-id="73f8f-323">Debug</span></span>             |
| <span data-ttu-id="73f8f-324">4</span><span class="sxs-lookup"><span data-stu-id="73f8f-324">4</span></span>      | <span data-ttu-id="73f8f-325">Консоль</span><span class="sxs-lookup"><span data-stu-id="73f8f-325">Console</span></span>       | <span data-ttu-id="73f8f-326">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="73f8f-326">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="73f8f-327">Ошибка</span><span class="sxs-lookup"><span data-stu-id="73f8f-327">Error</span></span>             |
| <span data-ttu-id="73f8f-328">5</span><span class="sxs-lookup"><span data-stu-id="73f8f-328">5</span></span>      | <span data-ttu-id="73f8f-329">Консоль</span><span class="sxs-lookup"><span data-stu-id="73f8f-329">Console</span></span>       | <span data-ttu-id="73f8f-330">Все категории</span><span class="sxs-lookup"><span data-stu-id="73f8f-330">All categories</span></span>                          | <span data-ttu-id="73f8f-331">Сведения</span><span class="sxs-lookup"><span data-stu-id="73f8f-331">Information</span></span>       |
| <span data-ttu-id="73f8f-332">6</span><span class="sxs-lookup"><span data-stu-id="73f8f-332">6</span></span>      | <span data-ttu-id="73f8f-333">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="73f8f-333">All providers</span></span> | <span data-ttu-id="73f8f-334">Все категории</span><span class="sxs-lookup"><span data-stu-id="73f8f-334">All categories</span></span>                          | <span data-ttu-id="73f8f-335">Отладка</span><span class="sxs-lookup"><span data-stu-id="73f8f-335">Debug</span></span>             |
| <span data-ttu-id="73f8f-336">7</span><span class="sxs-lookup"><span data-stu-id="73f8f-336">7</span></span>      | <span data-ttu-id="73f8f-337">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="73f8f-337">All providers</span></span> | <span data-ttu-id="73f8f-338">Система</span><span class="sxs-lookup"><span data-stu-id="73f8f-338">System</span></span>                                  | <span data-ttu-id="73f8f-339">Отладка</span><span class="sxs-lookup"><span data-stu-id="73f8f-339">Debug</span></span>             |
| <span data-ttu-id="73f8f-340">8</span><span class="sxs-lookup"><span data-stu-id="73f8f-340">8</span></span>      | <span data-ttu-id="73f8f-341">Отладка</span><span class="sxs-lookup"><span data-stu-id="73f8f-341">Debug</span></span>         | <span data-ttu-id="73f8f-342">Майкрософт</span><span class="sxs-lookup"><span data-stu-id="73f8f-342">Microsoft</span></span>                               | <span data-ttu-id="73f8f-343">Трассировка</span><span class="sxs-lookup"><span data-stu-id="73f8f-343">Trace</span></span>             |

<span data-ttu-id="73f8f-344">При создании объекта `ILogger` объект `ILoggerFactory` выбирает одно правило для каждого поставщика, которое будет применено к этому средству ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-344">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="73f8f-345">Все сообщения, записываемые с помощью экземпляра `ILogger`, фильтруются на основе выбранных правил.</span><span class="sxs-lookup"><span data-stu-id="73f8f-345">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="73f8f-346">Самое подходящее правило для каждой пары поставщика и категории выбирается из списка доступных правил.</span><span class="sxs-lookup"><span data-stu-id="73f8f-346">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="73f8f-347">При создании `ILogger` для данной категории для каждого поставщика используется приведенный далее алгоритм:</span><span class="sxs-lookup"><span data-stu-id="73f8f-347">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="73f8f-348">Выберите все правила, которые соответствуют поставщику или его псевдониму.</span><span class="sxs-lookup"><span data-stu-id="73f8f-348">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="73f8f-349">Если ничего не найдено, выберите все правила с пустым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="73f8f-349">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="73f8f-350">В результатах предыдущего шага выберите правила с самым длинным соответствующим префиксом категории.</span><span class="sxs-lookup"><span data-stu-id="73f8f-350">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="73f8f-351">Если ничего не найдено, выберите все правила, которые не указывают категорию.</span><span class="sxs-lookup"><span data-stu-id="73f8f-351">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="73f8f-352">Если выбрано несколько правил, примите **последнее**.</span><span class="sxs-lookup"><span data-stu-id="73f8f-352">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="73f8f-353">Если правила не выбраны, используйте `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-353">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="73f8f-354">Предположим, у вас есть указанный выше список правил и вы создаете объект `ILogger` для категории Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine:</span><span class="sxs-lookup"><span data-stu-id="73f8f-354">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="73f8f-355">К поставщику Debug применяются правила 1, 6 и 8.</span><span class="sxs-lookup"><span data-stu-id="73f8f-355">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="73f8f-356">Правило 8 является наиболее подходящим, поэтому выбрано оно.</span><span class="sxs-lookup"><span data-stu-id="73f8f-356">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="73f8f-357">К поставщику отладки применяются правила 3, 4, 5 и 6.</span><span class="sxs-lookup"><span data-stu-id="73f8f-357">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="73f8f-358">Правило 3 является наиболее подходящим.</span><span class="sxs-lookup"><span data-stu-id="73f8f-358">Rule 3 is most specific.</span></span>

<span data-ttu-id="73f8f-359">Полученный в результате экземпляр `ILogger` отправляет журналы уровня `Trace` и выше в поставщик Debug.</span><span class="sxs-lookup"><span data-stu-id="73f8f-359">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="73f8f-360">Журналы уровня `Debug` и выше отправляются в поставщик Console.</span><span class="sxs-lookup"><span data-stu-id="73f8f-360">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="73f8f-361">Псевдонимы поставщиков</span><span class="sxs-lookup"><span data-stu-id="73f8f-361">Provider aliases</span></span>

<span data-ttu-id="73f8f-362">Каждый поставщик определяет *псевдоним*, используемый в конфигурации вместо полного имени типа.</span><span class="sxs-lookup"><span data-stu-id="73f8f-362">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="73f8f-363">Для встроенных поставщиков используйте следующие псевдонимы:</span><span class="sxs-lookup"><span data-stu-id="73f8f-363">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="73f8f-364">Консоль</span><span class="sxs-lookup"><span data-stu-id="73f8f-364">Console</span></span>
* <span data-ttu-id="73f8f-365">Отладка</span><span class="sxs-lookup"><span data-stu-id="73f8f-365">Debug</span></span>
* <span data-ttu-id="73f8f-366">EventSource</span><span class="sxs-lookup"><span data-stu-id="73f8f-366">EventSource</span></span>
* <span data-ttu-id="73f8f-367">EventLog</span><span class="sxs-lookup"><span data-stu-id="73f8f-367">EventLog</span></span>
* <span data-ttu-id="73f8f-368">TraceSource</span><span class="sxs-lookup"><span data-stu-id="73f8f-368">TraceSource</span></span>
* <span data-ttu-id="73f8f-369">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="73f8f-369">AzureAppServicesFile</span></span>
* <span data-ttu-id="73f8f-370">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="73f8f-370">AzureAppServicesBlob</span></span>
* <span data-ttu-id="73f8f-371">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="73f8f-371">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="73f8f-372">Минимальный уровень по умолчанию</span><span class="sxs-lookup"><span data-stu-id="73f8f-372">Default minimum level</span></span>

<span data-ttu-id="73f8f-373">Существует параметр минимального уровня, который применяется, только если к определенному поставщику или категории не подходят правила из конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="73f8f-373">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="73f8f-374">В следующем примере показано, как задать минимальный уровень:</span><span class="sxs-lookup"><span data-stu-id="73f8f-374">The following example shows how to set the minimum level:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

<span data-ttu-id="73f8f-375">Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.</span><span class="sxs-lookup"><span data-stu-id="73f8f-375">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="73f8f-376">Функции фильтрации</span><span class="sxs-lookup"><span data-stu-id="73f8f-376">Filter functions</span></span>

<span data-ttu-id="73f8f-377">Функция фильтрации вызывается для всех поставщиков и категорий, которым в конфигурации и (или) коде не назначены правила.</span><span class="sxs-lookup"><span data-stu-id="73f8f-377">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="73f8f-378">Код в функции имеет доступ к типу поставщика, категории и уровню ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-378">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="73f8f-379">Например:</span><span class="sxs-lookup"><span data-stu-id="73f8f-379">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="73f8f-380">Системные категории и уровни</span><span class="sxs-lookup"><span data-stu-id="73f8f-380">System categories and levels</span></span>

<span data-ttu-id="73f8f-381">Ниже приведены некоторые категории, используемые ASP.NET Core и Entity Framework Core с заметками о журналах:</span><span class="sxs-lookup"><span data-stu-id="73f8f-381">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="73f8f-382">Категория</span><span class="sxs-lookup"><span data-stu-id="73f8f-382">Category</span></span>                            | <span data-ttu-id="73f8f-383">Примечания</span><span class="sxs-lookup"><span data-stu-id="73f8f-383">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="73f8f-384">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="73f8f-384">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="73f8f-385">Общая диагностика ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73f8f-385">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="73f8f-386">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="73f8f-386">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="73f8f-387">Распознанные, найденные и использованные ключи.</span><span class="sxs-lookup"><span data-stu-id="73f8f-387">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="73f8f-388">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="73f8f-388">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="73f8f-389">Разрешенные узлы.</span><span class="sxs-lookup"><span data-stu-id="73f8f-389">Hosts allowed.</span></span> |
| <span data-ttu-id="73f8f-390">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="73f8f-390">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="73f8f-391">Время начала и длительность выполнения HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-391">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="73f8f-392">Определение загруженных начальных сборок размещения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-392">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="73f8f-393">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="73f8f-393">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="73f8f-394">Диагностика MVC и Razor.</span><span class="sxs-lookup"><span data-stu-id="73f8f-394">MVC and Razor diagnostics.</span></span> <span data-ttu-id="73f8f-395">Привязка моделей, выполнение фильтра, компиляция представлений, выбор действия.</span><span class="sxs-lookup"><span data-stu-id="73f8f-395">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="73f8f-396">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="73f8f-396">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="73f8f-397">Перенаправление соответствующих сведений.</span><span class="sxs-lookup"><span data-stu-id="73f8f-397">Route matching information.</span></span> |
| <span data-ttu-id="73f8f-398">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="73f8f-398">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="73f8f-399">Запуск, остановка и сохранение ответов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-399">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="73f8f-400">Сведения о сертификате HTTPS.</span><span class="sxs-lookup"><span data-stu-id="73f8f-400">HTTPS certificate information.</span></span> |
| <span data-ttu-id="73f8f-401">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="73f8f-401">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="73f8f-402">Обработанные файлы.</span><span class="sxs-lookup"><span data-stu-id="73f8f-402">Files served.</span></span> |
| <span data-ttu-id="73f8f-403">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="73f8f-403">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="73f8f-404">Общая диагностика Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="73f8f-404">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="73f8f-405">Действия и конфигурация базы данных, обнаружение изменений, переносы.</span><span class="sxs-lookup"><span data-stu-id="73f8f-405">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="73f8f-406">Области журналов</span><span class="sxs-lookup"><span data-stu-id="73f8f-406">Log scopes</span></span>

 <span data-ttu-id="73f8f-407">*Область* может группировать набор логических операций.</span><span class="sxs-lookup"><span data-stu-id="73f8f-407">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="73f8f-408">Эту возможность можно использовать для присоединения одних и тех же данных к каждому журналу, созданному как часть набора.</span><span class="sxs-lookup"><span data-stu-id="73f8f-408">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="73f8f-409">Например, каждый журнал, созданный в ходе обработки транзакции, может включать идентификатор транзакции.</span><span class="sxs-lookup"><span data-stu-id="73f8f-409">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="73f8f-410">Область — это тип `IDisposable`, возвращаемый методом <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> и действующий до удаления.</span><span class="sxs-lookup"><span data-stu-id="73f8f-410">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="73f8f-411">Используйте область, заключив вызовы средства ведения журналов в блок `using`:</span><span class="sxs-lookup"><span data-stu-id="73f8f-411">Use a scope by wrapping logger calls in a `using` block:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

<span data-ttu-id="73f8f-412">Следующий код предоставляет области для поставщика Console:</span><span class="sxs-lookup"><span data-stu-id="73f8f-412">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="73f8f-413">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="73f8f-413">*Program.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="73f8f-414">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-414">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="73f8f-415">Сведения о настройках см. в разделе [Конфигурация](#configuration).</span><span class="sxs-lookup"><span data-stu-id="73f8f-415">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="73f8f-416">Каждое сообщение журнала содержит ограниченную информацию:</span><span class="sxs-lookup"><span data-stu-id="73f8f-416">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="73f8f-417">Встроенные поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="73f8f-417">Built-in logging providers</span></span>

<span data-ttu-id="73f8f-418">В состав ASP.NET Core входят следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="73f8f-418">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="73f8f-419">Консоль</span><span class="sxs-lookup"><span data-stu-id="73f8f-419">Console</span></span>](#console-provider)
* [<span data-ttu-id="73f8f-420">Отладка</span><span class="sxs-lookup"><span data-stu-id="73f8f-420">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="73f8f-421">EventSource</span><span class="sxs-lookup"><span data-stu-id="73f8f-421">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="73f8f-422">EventLog</span><span class="sxs-lookup"><span data-stu-id="73f8f-422">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="73f8f-423">TraceSource</span><span class="sxs-lookup"><span data-stu-id="73f8f-423">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="73f8f-424">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="73f8f-424">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="73f8f-425">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="73f8f-425">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="73f8f-426">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="73f8f-426">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="73f8f-427">Сведения о stdout и ведении журнала отладки с помощью модуля ASP.NET Core см. в статьях <xref:test/troubleshoot-azure-iis> и <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="73f8f-427">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="73f8f-428">Поставщик Console</span><span class="sxs-lookup"><span data-stu-id="73f8f-428">Console provider</span></span>

<span data-ttu-id="73f8f-429">Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль.</span><span class="sxs-lookup"><span data-stu-id="73f8f-429">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="73f8f-430">Чтобы просмотреть выходные данные ведения журналов в консоли, откройте командную строку, перейдите в папку проекта и запустите приведенную ниже команду:</span><span class="sxs-lookup"><span data-stu-id="73f8f-430">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="73f8f-431">Поставщик Debug</span><span class="sxs-lookup"><span data-stu-id="73f8f-431">Debug provider</span></span>

<span data-ttu-id="73f8f-432">Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (вызовов метода `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="73f8f-432">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="73f8f-433">В Linux этот поставщик записывает журналы в каталог */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="73f8f-433">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="eventsource-provider"></a><span data-ttu-id="73f8f-434">Поставщик EventSource</span><span class="sxs-lookup"><span data-stu-id="73f8f-434">EventSource provider</span></span>

<span data-ttu-id="73f8f-435">Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии, реализовывать события трассировки можно с помощью пакета поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource).</span><span class="sxs-lookup"><span data-stu-id="73f8f-435">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="73f8f-436">В Windows используется [трассировка событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="73f8f-436">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="73f8f-437">Поставщик является кроссплатформенным, но для Linux и macOS инструменты сбора событий и отображений пока отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="73f8f-437">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="73f8f-438">Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="73f8f-438">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="73f8f-439">Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73f8f-439">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="73f8f-440">Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**.</span><span class="sxs-lookup"><span data-stu-id="73f8f-440">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="73f8f-441">(Не пропустите звездочку в начале строки.)</span><span class="sxs-lookup"><span data-stu-id="73f8f-441">(Don't miss the asterisk at the start of the string.)</span></span>

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="73f8f-443">Поставщик Windows EventLog</span><span class="sxs-lookup"><span data-stu-id="73f8f-443">Windows EventLog provider</span></span>

<span data-ttu-id="73f8f-444">Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="73f8f-444">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="73f8f-445">[Перегрузки AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) позволяют передавать <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span><span class="sxs-lookup"><span data-stu-id="73f8f-445">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span>

### <a name="tracesource-provider"></a><span data-ttu-id="73f8f-446">Поставщик TraceSource</span><span class="sxs-lookup"><span data-stu-id="73f8f-446">TraceSource provider</span></span>

<span data-ttu-id="73f8f-447">Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="73f8f-447">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="73f8f-448">[Перегрузки AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) позволяют передавать исходный параметр и прослушиватель трассировки.</span><span class="sxs-lookup"><span data-stu-id="73f8f-448">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="73f8f-449">Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core).</span><span class="sxs-lookup"><span data-stu-id="73f8f-449">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="73f8f-450">Поставщик позволяет перенаправлять сообщения различным [прослушивателям](/dotnet/framework/debug-trace-profile/trace-listeners), например <xref:System.Diagnostics.TextWriterTraceListener>, который используется в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-450">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="73f8f-451">Поставщик службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="73f8f-451">Azure App Service provider</span></span>

<span data-ttu-id="73f8f-452">Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure.</span><span class="sxs-lookup"><span data-stu-id="73f8f-452">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="73f8f-453">Пакет поставщика не включен в общую платформу.</span><span class="sxs-lookup"><span data-stu-id="73f8f-453">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="73f8f-454">Чтобы использовать поставщик, добавьте пакет поставщика в проект.</span><span class="sxs-lookup"><span data-stu-id="73f8f-454">To use the provider, add the provider package to the project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="73f8f-455">Этот пакет не входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="73f8f-455">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="73f8f-456">Если планируется использовать .NET Framework или будет указана ссылка на метапакет `Microsoft.AspNetCore.App`, добавьте пакет поставщика в проект.</span><span class="sxs-lookup"><span data-stu-id="73f8f-456">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="73f8f-457">Для настройки параметров поставщика используйте <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> и <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="73f8f-457">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="73f8f-458">Для настройки параметров поставщика используйте <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> и <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="73f8f-458">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="73f8f-459">Перегрузка <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> позволяет передавать <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="73f8f-459">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="73f8f-460">Объект параметров может переопределять параметры по умолчанию, например шаблон выходных данных ведения журналов, имя BLOB-объекта и максимально допустимый размер файла.</span><span class="sxs-lookup"><span data-stu-id="73f8f-460">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="73f8f-461">(*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам наряду с тем, что предоставляется при вызове метода `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="73f8f-461">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

<span data-ttu-id="73f8f-462">При развертывании в приложение Службы приложений ваше приложение использует параметры в разделе [Журналы Службы приложений](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="73f8f-462">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="73f8f-463">При обновлении следующих параметров изменения вступают в силу немедленно без перезапуска или повторного развертывания приложения:</span><span class="sxs-lookup"><span data-stu-id="73f8f-463">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="73f8f-464">**Ведение журнала приложения (файловая система)** ;</span><span class="sxs-lookup"><span data-stu-id="73f8f-464">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="73f8f-465">**Ведение журнала приложения (BLOB-объект)** .</span><span class="sxs-lookup"><span data-stu-id="73f8f-465">**Application Logging (Blob)**</span></span>

<span data-ttu-id="73f8f-466">По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="73f8f-466">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="73f8f-467">Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2.</span><span class="sxs-lookup"><span data-stu-id="73f8f-467">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="73f8f-468">Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="73f8f-468">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="73f8f-469">Поставщик работает только при выполнении проекта в среде Azure.</span><span class="sxs-lookup"><span data-stu-id="73f8f-469">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="73f8f-470">Он не функционирует при запуске проекта в локальной среде &mdash;(то есть не выполняет запись в локальные файлы или в локальное хранилище разработки для больших двоичных объектов).</span><span class="sxs-lookup"><span data-stu-id="73f8f-470">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="73f8f-471">Потоковая передача журналов Azure</span><span class="sxs-lookup"><span data-stu-id="73f8f-471">Azure log streaming</span></span>

<span data-ttu-id="73f8f-472">Потоковая передача журналов Azure позволяет просматривать активность журнала в реальном времени из следующих источников:</span><span class="sxs-lookup"><span data-stu-id="73f8f-472">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="73f8f-473">сервер приложений;</span><span class="sxs-lookup"><span data-stu-id="73f8f-473">The app server</span></span>
* <span data-ttu-id="73f8f-474">веб-сервер;</span><span class="sxs-lookup"><span data-stu-id="73f8f-474">The web server</span></span>
* <span data-ttu-id="73f8f-475">трассировка неудачно завершенных запросов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-475">Failed request tracing</span></span>

<span data-ttu-id="73f8f-476">Настройка потоковой передачи журналов Azure</span><span class="sxs-lookup"><span data-stu-id="73f8f-476">To configure Azure log streaming:</span></span>

* <span data-ttu-id="73f8f-477">Со страницы портала приложения перейдите на страницу **Журналы Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="73f8f-477">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="73f8f-478">**Включите** параметр **Ведение журнала приложения (файловая система)** .</span><span class="sxs-lookup"><span data-stu-id="73f8f-478">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="73f8f-479">Выберите **уровень** ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="73f8f-479">Choose the log **Level**.</span></span>

<span data-ttu-id="73f8f-480">Перейдите на страницу **Поток журналов**, чтобы просмотреть сообщения приложения.</span><span class="sxs-lookup"><span data-stu-id="73f8f-480">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="73f8f-481">Они записываются в журнал приложением через интерфейс `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="73f8f-481">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="73f8f-482">Ведение журнала трассировки Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="73f8f-482">Azure Application Insights trace logging</span></span>

<span data-ttu-id="73f8f-483">Пакет поставщика [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) записывает журналы в Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="73f8f-483">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="73f8f-484">Служба Application Insights отслеживает веб-приложения и предоставляет средства для создания запросов и анализа данных телеметрии.</span><span class="sxs-lookup"><span data-stu-id="73f8f-484">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="73f8f-485">Используя этого поставщика, вы сможете выполнять запросы к журналам и их анализ с помощью средств Application Insights.</span><span class="sxs-lookup"><span data-stu-id="73f8f-485">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="73f8f-486">Поставщик ведения журнала включается как зависимость [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Этот пакет предоставляет всю доступную телеметрию для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73f8f-486">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="73f8f-487">Если вы используете этот пакет, пакет поставщика устанавливать не нужно.</span><span class="sxs-lookup"><span data-stu-id="73f8f-487">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="73f8f-488">Не используйте пакет [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web), который предназначен для ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="73f8f-488">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="73f8f-489">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="73f8f-489">For more information, see the following resources:</span></span>

* [<span data-ttu-id="73f8f-490">Общие сведения об Application Insights</span><span class="sxs-lookup"><span data-stu-id="73f8f-490">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="73f8f-491">[Application Insights для приложений ASP.NET Core](/azure/azure-monitor/app/asp-net-core) — начните изучение с этой статьи, если вы хотите полностью реализовать возможности Application Insights для телеметрии и ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-491">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="73f8f-492">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) (Поставщик ведения журналов Application Insights для журналов .NET Core ILogger) — начните изучение с этой статьи, если вы хотите внедрить поставщика ведения журналов, не используя остальные возможности Application Insights для телеметрии.</span><span class="sxs-lookup"><span data-stu-id="73f8f-492">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="73f8f-493">[Адаптеры ведения журналов в Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="73f8f-493">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="73f8f-494">[Инструментирование серверного кода веб-приложения с помощью Application Insights](/learn/modules/instrument-web-app-code-with-application-insights) — интерактивный учебник на сайте Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="73f8f-494">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="73f8f-495">Сторонние поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="73f8f-495">Third-party logging providers</span></span>

<span data-ttu-id="73f8f-496">Некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="73f8f-496">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="73f8f-497">[ELMAH.IO](https://elmah.io/) ([в репозитории GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="73f8f-497">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="73f8f-498">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([в репозитории GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="73f8f-498">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="73f8f-499">[JSNLog](https://jsnlog.com/) ([в репозитории GitHub](https://github.com/mperdeck/jsnlog));</span><span class="sxs-lookup"><span data-stu-id="73f8f-499">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="73f8f-500">[KissLog.net](https://kisslog.net/) ([репозиторий GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="73f8f-500">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="73f8f-501">[Log4Net](https://logging.apache.org/log4net/) ([репозиторий GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="73f8f-501">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="73f8f-502">[Loggr](https://loggr.net/) ([в репозитории GitHub](https://github.com/imobile3/Loggr.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="73f8f-502">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="73f8f-503">[NLog](https://nlog-project.org/) ([в репозитории GitHub](https://github.com/NLog/NLog.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="73f8f-503">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="73f8f-504">[Sentry](https://sentry.io/welcome/) ([репозиторий GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="73f8f-504">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="73f8f-505">[Serilog](https://serilog.net/) ([в репозитории GitHub](https://github.com/serilog/serilog-aspnetcore)).</span><span class="sxs-lookup"><span data-stu-id="73f8f-505">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="73f8f-506">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([репозиторий Github](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="73f8f-506">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="73f8f-507">Некоторые сторонние платформы выполняют [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="73f8f-507">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="73f8f-508">Использование сторонней платформы аналогично использованию одного из встроенных поставщиков:</span><span class="sxs-lookup"><span data-stu-id="73f8f-508">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="73f8f-509">Добавьте пакет NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="73f8f-509">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="73f8f-510">Вызовите метод расширения `ILoggerFactory`, предоставляемый платформой ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="73f8f-510">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="73f8f-511">Дополнительные сведения см. в документации по каждому поставщику.</span><span class="sxs-lookup"><span data-stu-id="73f8f-511">For more information, see each provider's documentation.</span></span> <span data-ttu-id="73f8f-512">Сторонние поставщики ведения журналов не поддерживаются корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="73f8f-512">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73f8f-513">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="73f8f-513">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
