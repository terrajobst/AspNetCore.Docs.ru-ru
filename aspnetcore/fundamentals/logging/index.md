---
title: Ведение журнала в .NET Core и ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать платформу ведения журналов, предоставляемую пакетом NuGet Microsoft.Extensions.Logging.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2020
uid: fundamentals/logging/index
ms.openlocfilehash: d9ef6694b6c0b0a88b13d60915be6e04ebdffb28
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829053"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="20701-103">Ведение журнала в .NET Core и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20701-103">Logging in .NET Core and ASP.NET Core</span></span>

<span data-ttu-id="20701-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="20701-104">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="20701-105">.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками.</span><span class="sxs-lookup"><span data-stu-id="20701-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="20701-106">В этой статье описано, как использовать в коде API ведения журналов, работающего со встроенными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="20701-106">This article shows how to use the logging API with built-in providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="20701-107">Большинство примеров кода, приведенных в этой статье, взяты из приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20701-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="20701-108">Части этих фрагментов кода, относящиеся к ведению журнала, применимы ко всем приложениям .NET Core, использующим [универсальный узел](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="20701-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="20701-109">Пример использования универсального узла в приложении, не являющемся веб-консолью, см. в файле *Program.cs* [примера приложения "Фоновые задачи"](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).</span><span class="sxs-lookup"><span data-stu-id="20701-109">For an example of how to use the Generic Host in a non-web console app, see the *Program.cs* file of the [Background Tasks sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).</span></span>

<span data-ttu-id="20701-110">Код ведения журнала для приложений без универсального узла отличается тем, как [добавляются поставщики](#add-providers) и [создаются средства ведения журнала](#create-logs).</span><span class="sxs-lookup"><span data-stu-id="20701-110">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> <span data-ttu-id="20701-111">Примеры кода, не связанные с размещением, приведены в соответствующих разделах статьи.</span><span class="sxs-lookup"><span data-stu-id="20701-111">Non-host code examples are shown in those sections of the article.</span></span>

::: moniker-end

<span data-ttu-id="20701-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="20701-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="20701-113">Добавление поставщиков</span><span class="sxs-lookup"><span data-stu-id="20701-113">Add providers</span></span>

<span data-ttu-id="20701-114">Поставщик ведения журналов отображает или сохраняет журналы.</span><span class="sxs-lookup"><span data-stu-id="20701-114">A logging provider displays or stores logs.</span></span> <span data-ttu-id="20701-115">Например, поставщик Console отображает журналы на консоли, а поставщик Azure Application Insights может сохранить их в Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="20701-115">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="20701-116">Журналы могут отправляться в несколько назначений после добавления нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="20701-116">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="20701-117">Чтобы добавить поставщик в приложение, использующее универсальный узел, вызовите метод расширения `Add{provider name}` поставщика в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="20701-117">To add a provider in an app that uses Generic Host, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

<span data-ttu-id="20701-118">В консольном приложении, не использующем узел, вызовите метод расширения `Add{provider name}` поставщика при создании `LoggerFactory`:</span><span class="sxs-lookup"><span data-stu-id="20701-118">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

<span data-ttu-id="20701-119">`LoggerFactory` и `AddConsole` требуют оператор `using` для `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="20701-119">`LoggerFactory` and `AddConsole` require a `using` statement for `Microsoft.Extensions.Logging`.</span></span>

<span data-ttu-id="20701-120">Шаблоны проектов ASP.NET Core по умолчанию вызывают метод <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, который добавляет следующие поставщики ведения журналов:</span><span class="sxs-lookup"><span data-stu-id="20701-120">The default ASP.NET Core project templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* [<span data-ttu-id="20701-121">Консоль</span><span class="sxs-lookup"><span data-stu-id="20701-121">Console</span></span>](#console-provider)
* [<span data-ttu-id="20701-122">Отладка</span><span class="sxs-lookup"><span data-stu-id="20701-122">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="20701-123">EventSource</span><span class="sxs-lookup"><span data-stu-id="20701-123">EventSource</span></span>](#event-source-provider)
* <span data-ttu-id="20701-124">[Журнал событий](#windows-eventlog-provider) (только при запуске в Windows)</span><span class="sxs-lookup"><span data-stu-id="20701-124">[EventLog](#windows-eventlog-provider) (only when running on Windows)</span></span>

<span data-ttu-id="20701-125">Поставщики по умолчанию можно заменить собственными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="20701-125">You can replace the default providers with your own choices.</span></span> <span data-ttu-id="20701-126">Вызовите <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> и добавьте требуемые поставщики.</span><span class="sxs-lookup"><span data-stu-id="20701-126">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0 "

<span data-ttu-id="20701-127">Для добавления поставщика вызовите метод расширения `Add{provider name}` поставщика в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="20701-127">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="20701-128">В указанном выше коде необходимо указать ссылки на `Microsoft.Extensions.Logging` и `Microsoft.Extensions.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="20701-128">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="20701-129">Шаблон проекта по умолчанию вызывает метод <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, который добавляет следующие поставщики ведения журналов:</span><span class="sxs-lookup"><span data-stu-id="20701-129">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="20701-130">Консоль</span><span class="sxs-lookup"><span data-stu-id="20701-130">Console</span></span>
* <span data-ttu-id="20701-131">Отладка</span><span class="sxs-lookup"><span data-stu-id="20701-131">Debug</span></span>
* <span data-ttu-id="20701-132">EventSource (начиная с версии ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="20701-132">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="20701-133">Если вы используете `CreateDefaultBuilder`, поставщики по умолчанию можно заменить собственными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="20701-133">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="20701-134">Вызовите <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> и добавьте требуемые поставщики.</span><span class="sxs-lookup"><span data-stu-id="20701-134">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

<span data-ttu-id="20701-135">См. сведения о [встроенных](#built-in-logging-providers) и [сторонних](#third-party-logging-providers) поставщиках ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-135">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="20701-136">Создание журналов</span><span class="sxs-lookup"><span data-stu-id="20701-136">Create logs</span></span>

<span data-ttu-id="20701-137">Чтобы создать журналы, используйте объект <xref:Microsoft.Extensions.Logging.ILogger%601>.</span><span class="sxs-lookup"><span data-stu-id="20701-137">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="20701-138">В веб-приложении или размещенной службе получите `ILogger` посредством внедрения зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="20701-138">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="20701-139">В консольных приложениях без размещения используйте `LoggerFactory` для создания `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="20701-139">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="20701-140">В приведенном ниже примере для ASP.NET Core создается средство ведения журнала с категорией `TodoApiSample.Pages.AboutModel`.</span><span class="sxs-lookup"><span data-stu-id="20701-140">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="20701-141">*Категория* ведения журналов представляет строку, которая связана с каждым журналом.</span><span class="sxs-lookup"><span data-stu-id="20701-141">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="20701-142">Экземпляр `ILogger<T>`, предоставляемый внедрением зависимостей, создает журналы с полным именем типа `T` в качестве категории.</span><span class="sxs-lookup"><span data-stu-id="20701-142">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="20701-143">В приведенном ниже примере консольного приложения без размещения создается средство ведения журнала с категорией `LoggingConsoleApp.Program`.</span><span class="sxs-lookup"><span data-stu-id="20701-143">The following non-host console app example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

::: moniker-end

<span data-ttu-id="20701-144">В приведенных ниже примерах ASP.NET Core и консольного приложения средство ведения журнала используется для создания журналов с уровнем `Information`.</span><span class="sxs-lookup"><span data-stu-id="20701-144">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="20701-145">*Уровень* ведения журналов определяет степень серьезности или важности записанного в журнал события.</span><span class="sxs-lookup"><span data-stu-id="20701-145">The Log *level* indicates the severity of the logged event.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

<span data-ttu-id="20701-146">[Уровни](#log-level) и [категории](#log-category) рассматриваются подробнее далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="20701-146">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-3.0"

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="20701-147">Создание журналов в классе Program</span><span class="sxs-lookup"><span data-stu-id="20701-147">Create logs in the Program class</span></span>

<span data-ttu-id="20701-148">Для записи журналов в классе `Program` приложения ASP.NET Core получите экземпляр `ILogger` путем внедрения зависимостей после создания узла:</span><span class="sxs-lookup"><span data-stu-id="20701-148">To write logs in the `Program` class of an ASP.NET Core app, get an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/TodoApiSample/Program.cs?highlight=9,10)]

<span data-ttu-id="20701-149">Ведение журнала во время создания узла не поддерживается напрямую.</span><span class="sxs-lookup"><span data-stu-id="20701-149">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="20701-150">Однако можно использовать отдельное средство ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="20701-150">However, a separate logger can be used.</span></span> <span data-ttu-id="20701-151">В следующем примере для входа в `CreateHostBuilder` используется средство ведения журнала [Serilog](https://serilog.net/).</span><span class="sxs-lookup"><span data-stu-id="20701-151">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateHostBuilder`.</span></span> <span data-ttu-id="20701-152">`AddSerilog` использует статическую конфигурацию, указанную в `Log.Logger`.</span><span class="sxs-lookup"><span data-stu-id="20701-152">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return Host.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddRazorPages();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {   
                    logging.AddSerilog();
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

### <a name="create-logs-in-the-startup-class"></a><span data-ttu-id="20701-153">Создание журналов в классе Startup</span><span class="sxs-lookup"><span data-stu-id="20701-153">Create logs in the Startup class</span></span>

<span data-ttu-id="20701-154">Для записи журналов в методе `Startup.Configure` приложения ASP.NET Core включите параметр `ILogger` в сигнатуру метода:</span><span class="sxs-lookup"><span data-stu-id="20701-154">To write logs in the `Startup.Configure` method of an ASP.NET Core app, include an `ILogger` parameter in the method signature:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

<span data-ttu-id="20701-155">Запись журналов до завершения настройки контейнера внедрения зависимостей в методе `Startup.ConfigureServices` не поддерживается:</span><span class="sxs-lookup"><span data-stu-id="20701-155">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="20701-156">Внедрение средства ведения журнала в конструктор `Startup` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="20701-156">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="20701-157">Внедрение средства ведения журнала в сигнатуру метода `Startup.ConfigureServices` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="20701-157">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="20701-158">Причина этого ограничения заключается в том, что ведение журнала зависит от внедрения зависимостей и от конфигурации, которая, в свою очередь, зависит от внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="20701-158">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="20701-159">Контейнер внедрения зависимостей не настраивается, пока не завершится выполнение `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="20701-159">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="20701-160">Внедрение средства ведения журнала в класс `Startup` посредством конструктора работает в более ранних версиях ASP.NET Core, так как для веб-узла создается отдельный контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="20701-160">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="20701-161">Сведения о том, почему для универсального узла создается только один контейнер, см. в [объявлении о критическом изменении](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="20701-161">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="20701-162">Если необходимо настроить службу, которая зависит от `ILogger<T>`, это можно сделать с помощью внедрения конструктора или путем предоставления фабричного метода.</span><span class="sxs-lookup"><span data-stu-id="20701-162">If you need to configure a service that depends on `ILogger<T>`, you can still do that by using constructor injection or by providing a factory method.</span></span> <span data-ttu-id="20701-163">Фабричный метод рекомендуется использовать, только если нет других вариантов.</span><span class="sxs-lookup"><span data-stu-id="20701-163">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="20701-164">Например, предположим, что необходимо заполнить свойство службой из внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="20701-164">For example, suppose you need to fill a property with a service from DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="20701-165">Выделенный выше код — это функция `Func`, которая выполняется, когда контейнеру внедрения зависимостей впервые требуется создать экземпляр `MyService`.</span><span class="sxs-lookup"><span data-stu-id="20701-165">The preceding highlighted code is a `Func` that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="20701-166">Таким образом можно обращаться к любым зарегистрированным службам.</span><span class="sxs-lookup"><span data-stu-id="20701-166">You can access any of the registered services in this way.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="20701-167">Создание журналов в классе Startup</span><span class="sxs-lookup"><span data-stu-id="20701-167">Create logs in Startup</span></span>

<span data-ttu-id="20701-168">Для записи журналов в классе `Startup` включите параметр `ILogger` в сигнатуру конструктора:</span><span class="sxs-lookup"><span data-stu-id="20701-168">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="20701-169">Создание журналов в классе Program</span><span class="sxs-lookup"><span data-stu-id="20701-169">Create logs in the Program class</span></span>

<span data-ttu-id="20701-170">Для записи журналов в классе `Program` получите экземпляр `ILogger` путем внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="20701-170">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="20701-171">Ведение журнала во время создания узла не поддерживается напрямую.</span><span class="sxs-lookup"><span data-stu-id="20701-171">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="20701-172">Однако можно использовать отдельное средство ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="20701-172">However, a separate logger can be used.</span></span> <span data-ttu-id="20701-173">В следующем примере для входа в `CreateWebHostBuilder` используется средство ведения журнала [Serilog](https://serilog.net/).</span><span class="sxs-lookup"><span data-stu-id="20701-173">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateWebHostBuilder`.</span></span> <span data-ttu-id="20701-174">`AddSerilog` использует статическую конфигурацию, указанную в `Log.Logger`.</span><span class="sxs-lookup"><span data-stu-id="20701-174">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

```csharp
using System;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return WebHost.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddMvc();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {
                    logging.AddSerilog();
                })
                .UseStartup<Startup>();
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="20701-175">Асинхронные методы ведения журналов не выполняются</span><span class="sxs-lookup"><span data-stu-id="20701-175">No asynchronous logger methods</span></span>

<span data-ttu-id="20701-176">Скорость ведения журналов не должна влиять на производительность выполнения асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="20701-176">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="20701-177">Если хранилище данных, предназначенное для регистрации сообщений журнала, работает медленно, сначала записывайте эти сообщения в быстродействующее хранилище,</span><span class="sxs-lookup"><span data-stu-id="20701-177">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="20701-178">а затем перемещайте их в медленное хранилище.</span><span class="sxs-lookup"><span data-stu-id="20701-178">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="20701-179">Например, если вы входите в SQL Server, вам не нужно делать это непосредственно в методе `Log`, так как методы `Log` являются синхронными.</span><span class="sxs-lookup"><span data-stu-id="20701-179">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="20701-180">Вместо этого синхронно добавьте сообщения журнала в очередь в памяти, и фоновый рабочий поток извлечет сообщения из очереди для выполнения асинхронных операций передачи данных в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="20701-180">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="20701-181">Дополнительные сведения см. [здесь](https://github.com/aspnet/AspNetCore.Docs/issues/11801) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="20701-181">For more information, see [this](https://github.com/aspnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

## <a name="configuration"></a><span data-ttu-id="20701-182">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="20701-182">Configuration</span></span>

<span data-ttu-id="20701-183">Конфигурацию поставщика ведения журналов предоставляет как минимум один поставщик конфигураций:</span><span class="sxs-lookup"><span data-stu-id="20701-183">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="20701-184">Форматы файлов (INI, JSON и XML).</span><span class="sxs-lookup"><span data-stu-id="20701-184">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="20701-185">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="20701-185">Command-line arguments.</span></span>
* <span data-ttu-id="20701-186">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="20701-186">Environment variables.</span></span>
* <span data-ttu-id="20701-187">Объекты .NET в памяти.</span><span class="sxs-lookup"><span data-stu-id="20701-187">In-memory .NET objects.</span></span>
* <span data-ttu-id="20701-188">Незашифрованное хранилище [Secret Manager](xref:security/app-secrets) (Диспетчер секретов).</span><span class="sxs-lookup"><span data-stu-id="20701-188">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="20701-189">Зашифрованное пользовательское хранилище, например [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="20701-189">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="20701-190">Пользовательские поставщики (установленные или созданные).</span><span class="sxs-lookup"><span data-stu-id="20701-190">Custom providers (installed or created).</span></span>

<span data-ttu-id="20701-191">Например, конфигурацию ведения журналов обычно предоставляет раздел `Logging` в файле параметров приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-191">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="20701-192">В следующем примере показано содержимое типичного файла *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="20701-192">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="20701-193">Свойство `Logging` может иметь свойство `LogLevel` и свойства поставщика журналов (здесь — Console).</span><span class="sxs-lookup"><span data-stu-id="20701-193">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="20701-194">Свойство `LogLevel` в разделе `Logging` указывает минимальный [уровень](#log-level) журнала для выбранных категорий.</span><span class="sxs-lookup"><span data-stu-id="20701-194">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="20701-195">В этом примере категории `System` и `Microsoft` записываются на уровне `Information`, а остальные — на уровне `Debug`.</span><span class="sxs-lookup"><span data-stu-id="20701-195">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="20701-196">Другие свойства в разделе `Logging` определяют поставщиков ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-196">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="20701-197">Пример приведен для поставщика Console.</span><span class="sxs-lookup"><span data-stu-id="20701-197">The example is for the Console provider.</span></span> <span data-ttu-id="20701-198">Если поставщик поддерживает [области журналов](#log-scopes), `IncludeScopes` определяет, включены ли они.</span><span class="sxs-lookup"><span data-stu-id="20701-198">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="20701-199">Свойство поставщика (например, `Console` в этом примере) также определять свойство `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="20701-199">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="20701-200">`LogLevel` под поставщиком указывает уровни журнала для этого поставщика.</span><span class="sxs-lookup"><span data-stu-id="20701-200">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="20701-201">Если уровни указаны в `Logging.{providername}.LogLevel`, они не переопределяют ничего из того, что задано в `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="20701-201">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

<span data-ttu-id="20701-202">API ведения журнала не включает сценарий для изменения уровней журнала во время работы приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-202">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="20701-203">Однако некоторые поставщики конфигурации могут перезагружать конфигурацию, что немедленно влияет на конфигурацию ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="20701-203">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="20701-204">Например, [Поставщик конфигурации файлов](xref:fundamentals/configuration/index#file-configuration-provider), который добавляется `CreateDefaultBuilder` для чтения файлов параметров, по умолчанию перезагружает конфигурацию ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="20701-204">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), which is added by `CreateDefaultBuilder` to read settings files, reloads logging configuration by default.</span></span> <span data-ttu-id="20701-205">Если конфигурация изменяется в коде во время выполнения приложения, приложение может вызвать [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*), чтобы обновить конфигурацию ведения журнала приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-205">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

<span data-ttu-id="20701-206">Сведения о реализации поставщиков конфигураций см. в статье <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="20701-206">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="20701-207">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="20701-207">Sample logging output</span></span>

<span data-ttu-id="20701-208">В примере кода из предыдущего раздела журналы появляются в консоли после запуска приложения из командной строки.</span><span class="sxs-lookup"><span data-stu-id="20701-208">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="20701-209">Ниже приведен пример выходных данных консоли:</span><span class="sxs-lookup"><span data-stu-id="20701-209">Here's an example of console output:</span></span>

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

<span data-ttu-id="20701-210">Предыдущие журналы созданы путем отправки HTTP-запроса Get к примеру приложения по адресу `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="20701-210">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="20701-211">Ниже приведен пример этих журналов в том виде, в каком они отображаются в окне отладки при запуске примера приложения в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="20701-211">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

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

<span data-ttu-id="20701-212">Журналы, которые созданы путем вызовов `ILogger`, как показано в предыдущем разделе, начинаются с TodoApiSample.</span><span class="sxs-lookup"><span data-stu-id="20701-212">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApiSample".</span></span> <span data-ttu-id="20701-213">Журналы, которые начинаются с категорий Microsoft, входят в код платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20701-213">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="20701-214">В ASP.NET Core и коде приложения используется один и тот же API ведения журналов и одни и те же поставщики.</span><span class="sxs-lookup"><span data-stu-id="20701-214">ASP.NET Core and application code are using the same logging API and providers.</span></span>

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

<span data-ttu-id="20701-215">Журналы, которые созданы путем вызовов `ILogger`, как показано в предыдущем разделе, начинаются с TodoApi.</span><span class="sxs-lookup"><span data-stu-id="20701-215">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="20701-216">Журналы, которые начинаются с категорий Microsoft, входят в код платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20701-216">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="20701-217">В ASP.NET Core и коде приложения используется один и тот же API ведения журналов и одни и те же поставщики.</span><span class="sxs-lookup"><span data-stu-id="20701-217">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

<span data-ttu-id="20701-218">В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-218">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="20701-219">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="20701-219">NuGet packages</span></span>

<span data-ttu-id="20701-220">Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="20701-220">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="20701-221">Категория журнала</span><span class="sxs-lookup"><span data-stu-id="20701-221">Log category</span></span>

<span data-ttu-id="20701-222">При создании объекта `ILogger` для него указывается *категория*.</span><span class="sxs-lookup"><span data-stu-id="20701-222">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="20701-223">Эта категория входит в состав каждого сообщения журнала, создаваемого этим экземпляром `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="20701-223">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="20701-224">Категория может быть любой строкой, обычно используется имя класса, например TodoApi.Controllers.TodoController.</span><span class="sxs-lookup"><span data-stu-id="20701-224">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="20701-225">Используйте `ILogger<T>` для получения экземпляра `ILogger`, который использует полное имя типа `T` в качестве категории:</span><span class="sxs-lookup"><span data-stu-id="20701-225">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="20701-226">Чтобы явно указать категорию, вызовите `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="20701-226">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="20701-227">Использование `ILogger<T>` эквивалентно вызову `CreateLogger` с полным именем типа `T`.</span><span class="sxs-lookup"><span data-stu-id="20701-227">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="20701-228">Уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="20701-228">Log level</span></span>

<span data-ttu-id="20701-229">Каждый журнал задает значение <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="20701-229">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="20701-230">Уровень ведения журналов означает степень серьезности или важности.</span><span class="sxs-lookup"><span data-stu-id="20701-230">The log level indicates the severity or importance.</span></span> <span data-ttu-id="20701-231">Например, можно создать журнал `Information` при нормальном завершении метода и журнал `Warning`, если метод возвращает код состояния *404 — не найдено*.</span><span class="sxs-lookup"><span data-stu-id="20701-231">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="20701-232">Следующий код создает журналы `Information`и `Warning`:</span><span class="sxs-lookup"><span data-stu-id="20701-232">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="20701-233">В коде выше первый параметр — это [идентификатор события журнала](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="20701-233">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="20701-234">Второй параметр — это шаблон сообщения с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода.</span><span class="sxs-lookup"><span data-stu-id="20701-234">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="20701-235">Параметры метода рассматриваются более подробно в разделе, посвященном [шаблону сообщений](#log-message-template) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="20701-235">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="20701-236">Методы журналов, которые содержат уровень в имени метода (например, `LogInformation` и `LogWarning`), являются [методами расширения для ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="20701-236">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="20701-237">Эти методы вызывают метод `Log`, принимающий параметр `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="20701-237">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="20701-238">Метод `Log`, в отличие от этих методов расширения, можно вызывать напрямую, однако его синтаксис довольно сложен.</span><span class="sxs-lookup"><span data-stu-id="20701-238">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="20701-239">См. сведения о <xref:Microsoft.Extensions.Logging.ILogger> и [исходном коде расширений средства ведения журналов](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="20701-239">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="20701-240">В ASP.NET Core определяются следующие уровни ведения журналов, упорядоченные по возрастанию уровней серьезности.</span><span class="sxs-lookup"><span data-stu-id="20701-240">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="20701-241">Трассировка = 0</span><span class="sxs-lookup"><span data-stu-id="20701-241">Trace = 0</span></span>

  <span data-ttu-id="20701-242">Для получения сведений, которые полезны только при отладке.</span><span class="sxs-lookup"><span data-stu-id="20701-242">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="20701-243">Эти сообщения могут содержать конфиденциальные данные приложения, поэтому их не следует включать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="20701-243">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="20701-244">*По умолчанию отключено.*</span><span class="sxs-lookup"><span data-stu-id="20701-244">*Disabled by default.*</span></span>

* <span data-ttu-id="20701-245">Отладка = 1</span><span class="sxs-lookup"><span data-stu-id="20701-245">Debug = 1</span></span>

  <span data-ttu-id="20701-246">Для получения сведений, которые полезны при разработке и отладке.</span><span class="sxs-lookup"><span data-stu-id="20701-246">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="20701-247">Пример. `Entering method Configure with flag set to true.` Включайте уровни ведения журналов `Debug` в рабочей среде только при устранении неполадок, так как такие журналы занимают много места.</span><span class="sxs-lookup"><span data-stu-id="20701-247">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="20701-248">Информация = 2</span><span class="sxs-lookup"><span data-stu-id="20701-248">Information = 2</span></span>

  <span data-ttu-id="20701-249">Для отслеживания общего потока работы приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-249">For tracking the general flow of the app.</span></span> <span data-ttu-id="20701-250">Эти журналы обычно полезны в долгосрочной перспективе.</span><span class="sxs-lookup"><span data-stu-id="20701-250">These logs typically have some long-term value.</span></span> <span data-ttu-id="20701-251">Пример: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="20701-251">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="20701-252">Предупреждение = 3</span><span class="sxs-lookup"><span data-stu-id="20701-252">Warning = 3</span></span>

  <span data-ttu-id="20701-253">Для нестандартных или непредвиденных событий, возникающих в потоке работы приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-253">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="20701-254">Это могут быть ошибки или другие условия, которые не приводят к остановке приложения, но которые нужно изучить.</span><span class="sxs-lookup"><span data-stu-id="20701-254">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="20701-255">Обработанные исключения являются распространенным условием использования уровня ведения журнала `Warning`.</span><span class="sxs-lookup"><span data-stu-id="20701-255">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="20701-256">Пример: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="20701-256">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="20701-257">Ошибка = 4</span><span class="sxs-lookup"><span data-stu-id="20701-257">Error = 4</span></span>

  <span data-ttu-id="20701-258">Для ошибок и исключений, которые не могут быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="20701-258">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="20701-259">Эти сообщения указывают на сбой текущего действия или операции (например, текущего HTTP-запроса), а не на ошибку уровня приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-259">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="20701-260">Пример сообщения журнала: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="20701-260">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="20701-261">Критический = 5</span><span class="sxs-lookup"><span data-stu-id="20701-261">Critical = 5</span></span>

  <span data-ttu-id="20701-262">Для сбоев, которые требуют немедленного внимания.</span><span class="sxs-lookup"><span data-stu-id="20701-262">For failures that require immediate attention.</span></span> <span data-ttu-id="20701-263">Примеры: потеря данных, нехватка места на диске.</span><span class="sxs-lookup"><span data-stu-id="20701-263">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="20701-264">Уровень ведения журналов можно использовать для управления объемом выходных данных, записываемых на определенный носитель или в окно отображения.</span><span class="sxs-lookup"><span data-stu-id="20701-264">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="20701-265">Пример:</span><span class="sxs-lookup"><span data-stu-id="20701-265">For example:</span></span>

* <span data-ttu-id="20701-266">В рабочей среде:</span><span class="sxs-lookup"><span data-stu-id="20701-266">In production:</span></span>
  * <span data-ttu-id="20701-267">При ведении журнала на уровнях с `Trace` по `Information` создается большой объем подробных сообщений журнала.</span><span class="sxs-lookup"><span data-stu-id="20701-267">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="20701-268">Чтобы контролировать затраты и не превысить лимиты объема хранилища данных, записывайте сообщения на уровнях с `Trace` по `Information` в хранилище данных с низкими затратами и большим объемом.</span><span class="sxs-lookup"><span data-stu-id="20701-268">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="20701-269">Ведение журнала на уровнях с `Warning` по `Critical` обычно приводит к записи меньшего количества сообщений меньшего размера.</span><span class="sxs-lookup"><span data-stu-id="20701-269">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="20701-270">Следовательно, затраты и лимиты хранилища не становятся проблемой, что приводит к большей гибкости при выборе хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="20701-270">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="20701-271">Во время разработки:</span><span class="sxs-lookup"><span data-stu-id="20701-271">During development:</span></span>
  * <span data-ttu-id="20701-272">Записывайте сообщения уровней с `Warning` по `Critical` на консоль.</span><span class="sxs-lookup"><span data-stu-id="20701-272">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="20701-273">Добавляйте сообщения уровней с `Trace` по `Information` при устранении неполадок.</span><span class="sxs-lookup"><span data-stu-id="20701-273">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="20701-274">В разделе [Фильтрация журналов](#log-filtering) далее в этой статье приводятся сведения о том, как контролировать уровни журнала, обрабатываемые поставщиком.</span><span class="sxs-lookup"><span data-stu-id="20701-274">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="20701-275">ASP.NET Core создает журналы для событий платформы.</span><span class="sxs-lookup"><span data-stu-id="20701-275">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="20701-276">В примерах журнала ранее в этой статье не указывались журналы с уровнем ниже `Information`, поэтому журналы уровня `Debug` или `Trace` не создавались.</span><span class="sxs-lookup"><span data-stu-id="20701-276">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="20701-277">Ниже приведен пример журналов консоли, которые созданы при запуске примера приложения, настроенного на отображение журналов `Debug`:</span><span class="sxs-lookup"><span data-stu-id="20701-277">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="20701-278">Идентификатор события журнала</span><span class="sxs-lookup"><span data-stu-id="20701-278">Log event ID</span></span>

<span data-ttu-id="20701-279">Каждый журнал может указывать *идентификатор события*.</span><span class="sxs-lookup"><span data-stu-id="20701-279">Each log can specify an *event ID*.</span></span> <span data-ttu-id="20701-280">В примере приложения для этого используется локально определенный класс `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="20701-280">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="20701-281">Идентификатор события связывает набор событий.</span><span class="sxs-lookup"><span data-stu-id="20701-281">An event ID associates a set of events.</span></span> <span data-ttu-id="20701-282">Например, все журналы, связанные с отображением списка элементов на странице, могут иметь значение 1001.</span><span class="sxs-lookup"><span data-stu-id="20701-282">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="20701-283">Поставщик ведения журналов может хранить идентификатор события в поле идентификатора, в сообщении журнала, или вообще не хранить.</span><span class="sxs-lookup"><span data-stu-id="20701-283">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="20701-284">Поставщик Debug не отображает идентификаторы событий.</span><span class="sxs-lookup"><span data-stu-id="20701-284">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="20701-285">Поставщик Console отображает идентификаторы событий в квадратных скобках после категории:</span><span class="sxs-lookup"><span data-stu-id="20701-285">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="20701-286">Шаблон сообщения журнала</span><span class="sxs-lookup"><span data-stu-id="20701-286">Log message template</span></span>

<span data-ttu-id="20701-287">Каждый журнал указывает шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="20701-287">Each log specifies a message template.</span></span> <span data-ttu-id="20701-288">Шаблон сообщения может содержать заполнители, для которых предоставляются аргументы.</span><span class="sxs-lookup"><span data-stu-id="20701-288">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="20701-289">Используйте для заполнителей имена, а не числа.</span><span class="sxs-lookup"><span data-stu-id="20701-289">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="20701-290">Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами.</span><span class="sxs-lookup"><span data-stu-id="20701-290">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="20701-291">В приведенном ниже коде обратите внимание на то, что имена параметров идут не по порядку в шаблоне сообщения:</span><span class="sxs-lookup"><span data-stu-id="20701-291">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="20701-292">Этот код создает сообщение журнала со значениями параметров в определенном порядке:</span><span class="sxs-lookup"><span data-stu-id="20701-292">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="20701-293">Платформа ведения журналов поддерживает такое поведение, чтобы поставщики ведения журнала могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="20701-293">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="20701-294">Сами аргументы передаются в систему ведения журналов, а не только в отформатированный шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="20701-294">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="20701-295">Эта информация позволяет поставщикам ведения журналов хранить значения параметров как поля.</span><span class="sxs-lookup"><span data-stu-id="20701-295">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="20701-296">Предположим, например, что вызовы метода средства ведения журналов выглядят так:</span><span class="sxs-lookup"><span data-stu-id="20701-296">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="20701-297">Если вы отправляете журналы в Хранилище таблиц Azure, каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-297">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="20701-298">Запрос может находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.</span><span class="sxs-lookup"><span data-stu-id="20701-298">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="20701-299">Ведение журнала исключений</span><span class="sxs-lookup"><span data-stu-id="20701-299">Logging exceptions</span></span>

<span data-ttu-id="20701-300">Методы средства ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="20701-300">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="20701-301">Разные поставщики обрабатывают сведения об исключениях по-разному.</span><span class="sxs-lookup"><span data-stu-id="20701-301">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="20701-302">Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="20701-302">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="20701-303">Фильтрация журналов</span><span class="sxs-lookup"><span data-stu-id="20701-303">Log filtering</span></span>

<span data-ttu-id="20701-304">Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий.</span><span class="sxs-lookup"><span data-stu-id="20701-304">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="20701-305">Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются.</span><span class="sxs-lookup"><span data-stu-id="20701-305">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="20701-306">Чтобы проигнорировать все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-306">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="20701-307">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="20701-307">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="20701-308">Создание правил фильтрации в конфигурации</span><span class="sxs-lookup"><span data-stu-id="20701-308">Create filter rules in configuration</span></span>

<span data-ttu-id="20701-309">Код шаблона проектов вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console, Debug и EventSource (ASP.NET Core 2.2 или более поздняя версия).</span><span class="sxs-lookup"><span data-stu-id="20701-309">The project template code calls `CreateDefaultBuilder` to set up logging for the Console, Debug, and EventSource (ASP.NET Core 2.2 or later) providers.</span></span> <span data-ttu-id="20701-310">Метод `CreateDefaultBuilder` настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, как было описано [ранее в этой статье](#configuration).</span><span class="sxs-lookup"><span data-stu-id="20701-310">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="20701-311">Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="20701-311">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

::: moniker-end

<span data-ttu-id="20701-312">Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно для всех поставщиков.</span><span class="sxs-lookup"><span data-stu-id="20701-312">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="20701-313">При создании объекта `ILogger` для каждого поставщика выбирается только одно из этих правил.</span><span class="sxs-lookup"><span data-stu-id="20701-313">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="20701-314">Правила фильтрации в коде</span><span class="sxs-lookup"><span data-stu-id="20701-314">Filter rules in code</span></span>

<span data-ttu-id="20701-315">В следующем примере показано, как зарегистрировать в коде правила фильтрации:</span><span class="sxs-lookup"><span data-stu-id="20701-315">The following example shows how to register filter rules in code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

<span data-ttu-id="20701-316">Второй `AddFilter` указывает поставщика, Debug, используя имя его типа.</span><span class="sxs-lookup"><span data-stu-id="20701-316">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="20701-317">Первый `AddFilter` применяется ко всем поставщикам, так как он не определяет тип поставщика.</span><span class="sxs-lookup"><span data-stu-id="20701-317">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="20701-318">Применение правил фильтрации</span><span class="sxs-lookup"><span data-stu-id="20701-318">How filtering rules are applied</span></span>

<span data-ttu-id="20701-319">Данные конфигурации и код `AddFilter`, приведенный в предыдущих примерах, создают правила, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="20701-319">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="20701-320">Первые шесть взяты из примера конфигурации, а последние два — из примера кода.</span><span class="sxs-lookup"><span data-stu-id="20701-320">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="20701-321">Число</span><span class="sxs-lookup"><span data-stu-id="20701-321">Number</span></span> | <span data-ttu-id="20701-322">Поставщик</span><span class="sxs-lookup"><span data-stu-id="20701-322">Provider</span></span>      | <span data-ttu-id="20701-323">Категории, которые начинаются с...</span><span class="sxs-lookup"><span data-stu-id="20701-323">Categories that begin with ...</span></span>          | <span data-ttu-id="20701-324">Минимальный уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="20701-324">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="20701-325">1</span><span class="sxs-lookup"><span data-stu-id="20701-325">1</span></span>      | <span data-ttu-id="20701-326">Отладка</span><span class="sxs-lookup"><span data-stu-id="20701-326">Debug</span></span>         | <span data-ttu-id="20701-327">Все категории</span><span class="sxs-lookup"><span data-stu-id="20701-327">All categories</span></span>                          | <span data-ttu-id="20701-328">Сведения</span><span class="sxs-lookup"><span data-stu-id="20701-328">Information</span></span>       |
| <span data-ttu-id="20701-329">2</span><span class="sxs-lookup"><span data-stu-id="20701-329">2</span></span>      | <span data-ttu-id="20701-330">Консоль</span><span class="sxs-lookup"><span data-stu-id="20701-330">Console</span></span>       | <span data-ttu-id="20701-331">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="20701-331">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="20701-332">Предупреждение</span><span class="sxs-lookup"><span data-stu-id="20701-332">Warning</span></span>           |
| <span data-ttu-id="20701-333">3</span><span class="sxs-lookup"><span data-stu-id="20701-333">3</span></span>      | <span data-ttu-id="20701-334">Консоль</span><span class="sxs-lookup"><span data-stu-id="20701-334">Console</span></span>       | <span data-ttu-id="20701-335">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="20701-335">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="20701-336">Отладка</span><span class="sxs-lookup"><span data-stu-id="20701-336">Debug</span></span>             |
| <span data-ttu-id="20701-337">4</span><span class="sxs-lookup"><span data-stu-id="20701-337">4</span></span>      | <span data-ttu-id="20701-338">Консоль</span><span class="sxs-lookup"><span data-stu-id="20701-338">Console</span></span>       | <span data-ttu-id="20701-339">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="20701-339">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="20701-340">Ошибка</span><span class="sxs-lookup"><span data-stu-id="20701-340">Error</span></span>             |
| <span data-ttu-id="20701-341">5</span><span class="sxs-lookup"><span data-stu-id="20701-341">5</span></span>      | <span data-ttu-id="20701-342">Консоль</span><span class="sxs-lookup"><span data-stu-id="20701-342">Console</span></span>       | <span data-ttu-id="20701-343">Все категории</span><span class="sxs-lookup"><span data-stu-id="20701-343">All categories</span></span>                          | <span data-ttu-id="20701-344">Сведения</span><span class="sxs-lookup"><span data-stu-id="20701-344">Information</span></span>       |
| <span data-ttu-id="20701-345">6</span><span class="sxs-lookup"><span data-stu-id="20701-345">6</span></span>      | <span data-ttu-id="20701-346">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="20701-346">All providers</span></span> | <span data-ttu-id="20701-347">Все категории</span><span class="sxs-lookup"><span data-stu-id="20701-347">All categories</span></span>                          | <span data-ttu-id="20701-348">Отладка</span><span class="sxs-lookup"><span data-stu-id="20701-348">Debug</span></span>             |
| <span data-ttu-id="20701-349">7</span><span class="sxs-lookup"><span data-stu-id="20701-349">7</span></span>      | <span data-ttu-id="20701-350">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="20701-350">All providers</span></span> | <span data-ttu-id="20701-351">Система</span><span class="sxs-lookup"><span data-stu-id="20701-351">System</span></span>                                  | <span data-ttu-id="20701-352">Отладка</span><span class="sxs-lookup"><span data-stu-id="20701-352">Debug</span></span>             |
| <span data-ttu-id="20701-353">8</span><span class="sxs-lookup"><span data-stu-id="20701-353">8</span></span>      | <span data-ttu-id="20701-354">Отладка</span><span class="sxs-lookup"><span data-stu-id="20701-354">Debug</span></span>         | <span data-ttu-id="20701-355">Майкрософт</span><span class="sxs-lookup"><span data-stu-id="20701-355">Microsoft</span></span>                               | <span data-ttu-id="20701-356">Трассировка</span><span class="sxs-lookup"><span data-stu-id="20701-356">Trace</span></span>             |

<span data-ttu-id="20701-357">При создании объекта `ILogger` объект `ILoggerFactory` выбирает одно правило для каждого поставщика, которое будет применено к этому средству ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-357">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="20701-358">Все сообщения, записываемые с помощью экземпляра `ILogger`, фильтруются на основе выбранных правил.</span><span class="sxs-lookup"><span data-stu-id="20701-358">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="20701-359">Самое подходящее правило для каждой пары поставщика и категории выбирается из списка доступных правил.</span><span class="sxs-lookup"><span data-stu-id="20701-359">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="20701-360">При создании `ILogger` для данной категории для каждого поставщика используется приведенный далее алгоритм:</span><span class="sxs-lookup"><span data-stu-id="20701-360">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="20701-361">Выберите все правила, которые соответствуют поставщику или его псевдониму.</span><span class="sxs-lookup"><span data-stu-id="20701-361">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="20701-362">Если ничего не найдено, выберите все правила с пустым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="20701-362">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="20701-363">В результатах предыдущего шага выберите правила с самым длинным соответствующим префиксом категории.</span><span class="sxs-lookup"><span data-stu-id="20701-363">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="20701-364">Если ничего не найдено, выберите все правила, которые не указывают категорию.</span><span class="sxs-lookup"><span data-stu-id="20701-364">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="20701-365">Если выбрано несколько правил, примите **последнее**.</span><span class="sxs-lookup"><span data-stu-id="20701-365">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="20701-366">Если правила не выбраны, используйте `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="20701-366">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="20701-367">Предположим, у вас есть указанный выше список правил и вы создаете объект `ILogger` для категории Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine:</span><span class="sxs-lookup"><span data-stu-id="20701-367">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="20701-368">К поставщику Debug применяются правила 1, 6 и 8.</span><span class="sxs-lookup"><span data-stu-id="20701-368">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="20701-369">Правило 8 является наиболее подходящим, поэтому выбрано оно.</span><span class="sxs-lookup"><span data-stu-id="20701-369">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="20701-370">К поставщику отладки применяются правила 3, 4, 5 и 6.</span><span class="sxs-lookup"><span data-stu-id="20701-370">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="20701-371">Правило 3 является наиболее подходящим.</span><span class="sxs-lookup"><span data-stu-id="20701-371">Rule 3 is most specific.</span></span>

<span data-ttu-id="20701-372">Полученный в результате экземпляр `ILogger` отправляет журналы уровня `Trace` и выше в поставщик Debug.</span><span class="sxs-lookup"><span data-stu-id="20701-372">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="20701-373">Журналы уровня `Debug` и выше отправляются в поставщик Console.</span><span class="sxs-lookup"><span data-stu-id="20701-373">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="20701-374">Псевдонимы поставщиков</span><span class="sxs-lookup"><span data-stu-id="20701-374">Provider aliases</span></span>

<span data-ttu-id="20701-375">Каждый поставщик определяет *псевдоним*, используемый в конфигурации вместо полного имени типа.</span><span class="sxs-lookup"><span data-stu-id="20701-375">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="20701-376">Для встроенных поставщиков используйте следующие псевдонимы:</span><span class="sxs-lookup"><span data-stu-id="20701-376">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="20701-377">Консоль</span><span class="sxs-lookup"><span data-stu-id="20701-377">Console</span></span>
* <span data-ttu-id="20701-378">Отладка</span><span class="sxs-lookup"><span data-stu-id="20701-378">Debug</span></span>
* <span data-ttu-id="20701-379">EventSource</span><span class="sxs-lookup"><span data-stu-id="20701-379">EventSource</span></span>
* <span data-ttu-id="20701-380">EventLog</span><span class="sxs-lookup"><span data-stu-id="20701-380">EventLog</span></span>
* <span data-ttu-id="20701-381">TraceSource</span><span class="sxs-lookup"><span data-stu-id="20701-381">TraceSource</span></span>
* <span data-ttu-id="20701-382">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="20701-382">AzureAppServicesFile</span></span>
* <span data-ttu-id="20701-383">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="20701-383">AzureAppServicesBlob</span></span>
* <span data-ttu-id="20701-384">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="20701-384">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="20701-385">Минимальный уровень по умолчанию</span><span class="sxs-lookup"><span data-stu-id="20701-385">Default minimum level</span></span>

<span data-ttu-id="20701-386">Существует параметр минимального уровня, который применяется, только если к определенному поставщику или категории не подходят правила из конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="20701-386">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="20701-387">В следующем примере показано, как задать минимальный уровень:</span><span class="sxs-lookup"><span data-stu-id="20701-387">The following example shows how to set the minimum level:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

<span data-ttu-id="20701-388">Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.</span><span class="sxs-lookup"><span data-stu-id="20701-388">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="20701-389">Функции фильтрации</span><span class="sxs-lookup"><span data-stu-id="20701-389">Filter functions</span></span>

<span data-ttu-id="20701-390">Функция фильтрации вызывается для всех поставщиков и категорий, которым в конфигурации и (или) коде не назначены правила.</span><span class="sxs-lookup"><span data-stu-id="20701-390">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="20701-391">Код в функции имеет доступ к типу поставщика, категории и уровню ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-391">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="20701-392">Пример:</span><span class="sxs-lookup"><span data-stu-id="20701-392">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="20701-393">Системные категории и уровни</span><span class="sxs-lookup"><span data-stu-id="20701-393">System categories and levels</span></span>

<span data-ttu-id="20701-394">Ниже приведены некоторые категории, используемые ASP.NET Core и Entity Framework Core с заметками о журналах:</span><span class="sxs-lookup"><span data-stu-id="20701-394">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="20701-395">Категория</span><span class="sxs-lookup"><span data-stu-id="20701-395">Category</span></span>                            | <span data-ttu-id="20701-396">Примечания</span><span class="sxs-lookup"><span data-stu-id="20701-396">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="20701-397">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="20701-397">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="20701-398">Общая диагностика ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20701-398">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="20701-399">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="20701-399">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="20701-400">Распознанные, найденные и использованные ключи.</span><span class="sxs-lookup"><span data-stu-id="20701-400">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="20701-401">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="20701-401">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="20701-402">Разрешенные узлы.</span><span class="sxs-lookup"><span data-stu-id="20701-402">Hosts allowed.</span></span> |
| <span data-ttu-id="20701-403">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="20701-403">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="20701-404">Время начала и длительность выполнения HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="20701-404">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="20701-405">Определение загруженных начальных сборок размещения.</span><span class="sxs-lookup"><span data-stu-id="20701-405">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="20701-406">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="20701-406">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="20701-407">Диагностика MVC и Razor.</span><span class="sxs-lookup"><span data-stu-id="20701-407">MVC and Razor diagnostics.</span></span> <span data-ttu-id="20701-408">Привязка моделей, выполнение фильтра, компиляция представлений, выбор действия.</span><span class="sxs-lookup"><span data-stu-id="20701-408">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="20701-409">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="20701-409">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="20701-410">Перенаправление соответствующих сведений.</span><span class="sxs-lookup"><span data-stu-id="20701-410">Route matching information.</span></span> |
| <span data-ttu-id="20701-411">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="20701-411">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="20701-412">Запуск, остановка и сохранение ответов.</span><span class="sxs-lookup"><span data-stu-id="20701-412">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="20701-413">Сведения о сертификате HTTPS.</span><span class="sxs-lookup"><span data-stu-id="20701-413">HTTPS certificate information.</span></span> |
| <span data-ttu-id="20701-414">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="20701-414">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="20701-415">Обработанные файлы.</span><span class="sxs-lookup"><span data-stu-id="20701-415">Files served.</span></span> |
| <span data-ttu-id="20701-416">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="20701-416">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="20701-417">Общая диагностика Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="20701-417">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="20701-418">Действия и конфигурация базы данных, обнаружение изменений, переносы.</span><span class="sxs-lookup"><span data-stu-id="20701-418">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="20701-419">Области журналов</span><span class="sxs-lookup"><span data-stu-id="20701-419">Log scopes</span></span>

 <span data-ttu-id="20701-420">*Область* может группировать набор логических операций.</span><span class="sxs-lookup"><span data-stu-id="20701-420">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="20701-421">Эту возможность можно использовать для присоединения одних и тех же данных к каждому журналу, созданному как часть набора.</span><span class="sxs-lookup"><span data-stu-id="20701-421">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="20701-422">Например, каждый журнал, созданный в ходе обработки транзакции, может включать идентификатор транзакции.</span><span class="sxs-lookup"><span data-stu-id="20701-422">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="20701-423">Область — это тип `IDisposable`, возвращаемый методом <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> и действующий до удаления.</span><span class="sxs-lookup"><span data-stu-id="20701-423">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="20701-424">Используйте область, заключив вызовы средства ведения журналов в блок `using`:</span><span class="sxs-lookup"><span data-stu-id="20701-424">Use a scope by wrapping logger calls in a `using` block:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

<span data-ttu-id="20701-425">Следующий код предоставляет области для поставщика Console:</span><span class="sxs-lookup"><span data-stu-id="20701-425">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="20701-426">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="20701-426">*Program.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="20701-427">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-427">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="20701-428">Сведения о настройках см. в разделе [Конфигурация](#configuration).</span><span class="sxs-lookup"><span data-stu-id="20701-428">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="20701-429">Каждое сообщение журнала содержит ограниченную информацию:</span><span class="sxs-lookup"><span data-stu-id="20701-429">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="20701-430">Встроенные поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="20701-430">Built-in logging providers</span></span>

<span data-ttu-id="20701-431">В состав ASP.NET Core входят следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="20701-431">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="20701-432">Консоль</span><span class="sxs-lookup"><span data-stu-id="20701-432">Console</span></span>](#console-provider)
* [<span data-ttu-id="20701-433">Отладка</span><span class="sxs-lookup"><span data-stu-id="20701-433">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="20701-434">EventSource</span><span class="sxs-lookup"><span data-stu-id="20701-434">EventSource</span></span>](#event-source-provider)
* [<span data-ttu-id="20701-435">EventLog</span><span class="sxs-lookup"><span data-stu-id="20701-435">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="20701-436">TraceSource</span><span class="sxs-lookup"><span data-stu-id="20701-436">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="20701-437">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="20701-437">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="20701-438">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="20701-438">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="20701-439">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="20701-439">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="20701-440">Сведения о stdout и ведении журнала отладки с помощью модуля ASP.NET Core см. в статьях <xref:test/troubleshoot-azure-iis> и <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="20701-440">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="20701-441">Поставщик Console</span><span class="sxs-lookup"><span data-stu-id="20701-441">Console provider</span></span>

<span data-ttu-id="20701-442">Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль.</span><span class="sxs-lookup"><span data-stu-id="20701-442">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="20701-443">Чтобы просмотреть выходные данные ведения журналов в консоли, откройте командную строку, перейдите в папку проекта и запустите приведенную ниже команду:</span><span class="sxs-lookup"><span data-stu-id="20701-443">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="20701-444">Поставщик Debug</span><span class="sxs-lookup"><span data-stu-id="20701-444">Debug provider</span></span>

<span data-ttu-id="20701-445">Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (вызовов метода `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="20701-445">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="20701-446">В Linux этот поставщик записывает журналы в каталог */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="20701-446">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a><span data-ttu-id="20701-447">Поставщик источника событий</span><span class="sxs-lookup"><span data-stu-id="20701-447">Event Source provider</span></span>

<span data-ttu-id="20701-448">Пакет поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) записывает данные в источник событий на кросс-платформенный сервер с именем `Microsoft-Extensions-Logging`.</span><span class="sxs-lookup"><span data-stu-id="20701-448">The [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package writes to an Event Source cross-platform with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="20701-449">В Windows поставщик использует [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="20701-449">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="20701-450">Поставщик источника событий добавляется автоматически при вызове `CreateDefaultBuilder` для сборки узла.</span><span class="sxs-lookup"><span data-stu-id="20701-450">The Event Source provider is added automatically when `CreateDefaultBuilder` is called to build the host.</span></span>

::: moniker range=">= aspnetcore-3.0"

#### <a name="dotnet-trace-tooling"></a><span data-ttu-id="20701-451">Средства трассировки dotnet</span><span class="sxs-lookup"><span data-stu-id="20701-451">dotnet trace tooling</span></span>

<span data-ttu-id="20701-452">Средство [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) — это универсальное кроссплатформенное средство командной строки, которое выполняет сбор трассировок .NET Core для запущенного процесса.</span><span class="sxs-lookup"><span data-stu-id="20701-452">The [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) tool is a cross-platform CLI global tool that enables the collection of .NET Core traces of a running process.</span></span> <span data-ttu-id="20701-453">Средство собирает данные поставщика <xref:Microsoft.Extensions.Logging.EventSource> с помощью <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span><span class="sxs-lookup"><span data-stu-id="20701-453">The tool collects <xref:Microsoft.Extensions.Logging.EventSource> provider data using a <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span></span>

<span data-ttu-id="20701-454">Установите средства трассировки dotnet с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="20701-454">Install the dotnet trace tooling with the following command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-trace
```

<span data-ttu-id="20701-455">Используйте средства трассировки dotnet, чтобы получить трассировку из приложения:</span><span class="sxs-lookup"><span data-stu-id="20701-455">Use the dotnet trace tooling to collect a trace from an app:</span></span>

1. <span data-ttu-id="20701-456">Если приложение не создает узел с `CreateDefaultBuilder`, добавьте [Поставщика источника событий](#event-source-provider) в конфигурацию ведения журнала приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-456">If the app doesn't build the host with `CreateDefaultBuilder`, add the [Event Source provider](#event-source-provider) to the app's logging configuration.</span></span>

1. <span data-ttu-id="20701-457">Запустите приложение с помощью команды `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="20701-457">Run the app with the `dotnet run` command.</span></span>

1. <span data-ttu-id="20701-458">Определите идентификатор процесса (PID) приложения .NET Core:</span><span class="sxs-lookup"><span data-stu-id="20701-458">Determine the process identifier (PID) of the .NET Core app:</span></span>

   * <span data-ttu-id="20701-459">В Windows воспользуйтесь одним из перечисленных ниже подходов:</span><span class="sxs-lookup"><span data-stu-id="20701-459">On Windows, use one of the following approaches:</span></span>
     * <span data-ttu-id="20701-460">Диспетчер задач (CTRL+ALT+DEL)</span><span class="sxs-lookup"><span data-stu-id="20701-460">Task Manager (Ctrl+Alt+Del)</span></span>
     * [<span data-ttu-id="20701-461">Команда tasklist</span><span class="sxs-lookup"><span data-stu-id="20701-461">tasklist command</span></span>](/windows-server/administration/windows-commands/tasklist)
     * [<span data-ttu-id="20701-462">Команда Powershell Get-Process</span><span class="sxs-lookup"><span data-stu-id="20701-462">Get-Process Powershell command</span></span>](/powershell/module/microsoft.powershell.management/get-process)
   * <span data-ttu-id="20701-463">В Linux используйте [команду pidof](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span><span class="sxs-lookup"><span data-stu-id="20701-463">On Linux, use the [pidof command](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span></span>

   <span data-ttu-id="20701-464">Найдите идентификатор процесса, имя которого совпадает с именем сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-464">Find the PID for the process that has the same name as the app's assembly.</span></span>

1. <span data-ttu-id="20701-465">Выполните команду `dotnet trace`.</span><span class="sxs-lookup"><span data-stu-id="20701-465">Execute the `dotnet trace` command.</span></span>

   <span data-ttu-id="20701-466">Общий синтаксис команды:</span><span class="sxs-lookup"><span data-stu-id="20701-466">General command syntax:</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   <span data-ttu-id="20701-467">При использовании командной оболочки PowerShell заключите значение `--providers` в одинарные кавычки (`'`):</span><span class="sxs-lookup"><span data-stu-id="20701-467">When using a PowerShell command shell, enclose the `--providers` value in single quotes (`'`):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   <span data-ttu-id="20701-468">На платформах, отличных от Windows, добавьте параметр `-f speedscope`, чтобы изменить формат выходного файла трассировки на `speedscope`.</span><span class="sxs-lookup"><span data-stu-id="20701-468">On non-Windows platforms, add the `-f speedscope` option to change the format of the output trace file to `speedscope`.</span></span>

   | <span data-ttu-id="20701-469">Ключевое слово</span><span class="sxs-lookup"><span data-stu-id="20701-469">Keyword</span></span> | <span data-ttu-id="20701-470">Описание</span><span class="sxs-lookup"><span data-stu-id="20701-470">Description</span></span> |
   | :-----: | ----------- |
   | <span data-ttu-id="20701-471">1</span><span class="sxs-lookup"><span data-stu-id="20701-471">1</span></span>       | <span data-ttu-id="20701-472">Занесите в журнал метасобытия о `LoggingEventSource`.</span><span class="sxs-lookup"><span data-stu-id="20701-472">Log meta events about the `LoggingEventSource`.</span></span> <span data-ttu-id="20701-473">Не заносит в журнал события из `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="20701-473">Doesn't log events from `ILogger`).</span></span> |
   | <span data-ttu-id="20701-474">2</span><span class="sxs-lookup"><span data-stu-id="20701-474">2</span></span>       | <span data-ttu-id="20701-475">Включает событие `Message` при вызове `ILogger.Log()`.</span><span class="sxs-lookup"><span data-stu-id="20701-475">Turns on the `Message` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="20701-476">Предоставляет сведения в исходном виде (без форматирования).</span><span class="sxs-lookup"><span data-stu-id="20701-476">Provides information in a programmatic (not formatted) way.</span></span> |
   | <span data-ttu-id="20701-477">4</span><span class="sxs-lookup"><span data-stu-id="20701-477">4</span></span>       | <span data-ttu-id="20701-478">Включает событие `FormatMessage` при вызове `ILogger.Log()`.</span><span class="sxs-lookup"><span data-stu-id="20701-478">Turns on the `FormatMessage` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="20701-479">Предоставляет сведения в виде отформатированной строки.</span><span class="sxs-lookup"><span data-stu-id="20701-479">Provides the formatted string version of the information.</span></span> |
   | <span data-ttu-id="20701-480">8</span><span class="sxs-lookup"><span data-stu-id="20701-480">8</span></span>       | <span data-ttu-id="20701-481">Включает событие `MessageJson` при вызове `ILogger.Log()`.</span><span class="sxs-lookup"><span data-stu-id="20701-481">Turns on the `MessageJson` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="20701-482">Предоставляет представление аргументов в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="20701-482">Provides a JSON representation of the arguments.</span></span> |

   | <span data-ttu-id="20701-483">Уровень события</span><span class="sxs-lookup"><span data-stu-id="20701-483">Event Level</span></span> | <span data-ttu-id="20701-484">Описание</span><span class="sxs-lookup"><span data-stu-id="20701-484">Description</span></span>     |
   | :---------: | --------------- |
   | <span data-ttu-id="20701-485">0</span><span class="sxs-lookup"><span data-stu-id="20701-485">0</span></span>           | `LogAlways`     |
   | <span data-ttu-id="20701-486">1</span><span class="sxs-lookup"><span data-stu-id="20701-486">1</span></span>           | `Critical`      |
   | <span data-ttu-id="20701-487">2</span><span class="sxs-lookup"><span data-stu-id="20701-487">2</span></span>           | `Error`         |
   | <span data-ttu-id="20701-488">3</span><span class="sxs-lookup"><span data-stu-id="20701-488">3</span></span>           | `Warning`       |
   | <span data-ttu-id="20701-489">4</span><span class="sxs-lookup"><span data-stu-id="20701-489">4</span></span>           | `Informational` |
   | <span data-ttu-id="20701-490">5</span><span class="sxs-lookup"><span data-stu-id="20701-490">5</span></span>           | `Verbose`       |

   <span data-ttu-id="20701-491">Записи `FilterSpecs` для `{Logger Category}` и `{Event Level}` представляют дополнительные условия фильтрации журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-491">`FilterSpecs` entries for `{Logger Category}` and `{Event Level}` represent additional log filtering conditions.</span></span> <span data-ttu-id="20701-492">Отдельные записи `FilterSpecs` разделяются точкой с запятой (`;`).</span><span class="sxs-lookup"><span data-stu-id="20701-492">Separate `FilterSpecs` entries with a semicolon (`;`).</span></span>

   <span data-ttu-id="20701-493">Пример использования командной оболочки Windows (**не** одинарные кавычки вокруг значения `--providers`):</span><span class="sxs-lookup"><span data-stu-id="20701-493">Example using a Windows command shell (**no** single quotes around the `--providers` value):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   <span data-ttu-id="20701-494">Предыдущая команда активирует:</span><span class="sxs-lookup"><span data-stu-id="20701-494">The preceding command activates:</span></span>

   * <span data-ttu-id="20701-495">Средство ведения журнала источника событий для создания форматированных строк (`4`) для ошибок (`2`).</span><span class="sxs-lookup"><span data-stu-id="20701-495">The Event Source logger to produce formatted strings (`4`) for errors (`2`).</span></span>
   * <span data-ttu-id="20701-496">Ведение журнала `Microsoft.AspNetCore.Hosting` на уровне ведения журнала `Informational` (`4`).</span><span class="sxs-lookup"><span data-stu-id="20701-496">`Microsoft.AspNetCore.Hosting` logging at the `Informational` logging level (`4`).</span></span>

1. <span data-ttu-id="20701-497">Остановите средства трассировки dotnet, нажав клавишу ENTER или CTRL+C.</span><span class="sxs-lookup"><span data-stu-id="20701-497">Stop the dotnet trace tooling by pressing the Enter key or Ctrl+C.</span></span>

   <span data-ttu-id="20701-498">Трассировка сохраняется с именем *trace.nettrace* в папке, в которой выполняется команда `dotnet trace`.</span><span class="sxs-lookup"><span data-stu-id="20701-498">The trace is saved with the name *trace.nettrace* in the folder where the `dotnet trace` command is executed.</span></span>

1. <span data-ttu-id="20701-499">Откройте трассировку с помощью [Perfview](#perfview).</span><span class="sxs-lookup"><span data-stu-id="20701-499">Open the trace with [Perfview](#perfview).</span></span> <span data-ttu-id="20701-500">Откройте файл *trace.nettrace* и изучите события трассировки.</span><span class="sxs-lookup"><span data-stu-id="20701-500">Open the *trace.nettrace* file and explore the trace events.</span></span>

<span data-ttu-id="20701-501">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="20701-501">For more information, see:</span></span>

* <span data-ttu-id="20701-502">[Трассировка для программы анализа производительности (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (документация по .NET Core)</span><span class="sxs-lookup"><span data-stu-id="20701-502">[Trace for performance analysis utility (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (.NET Core documentation)</span></span>
* <span data-ttu-id="20701-503">[Трассировка для программы анализа производительности (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (документация по репозиторию GitHub dotnet/diagnostics)</span><span class="sxs-lookup"><span data-stu-id="20701-503">[Trace for performance analysis utility (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (dotnet/diagnostics GitHub repository documentation)</span></span>
* <span data-ttu-id="20701-504">[Класс LoggingEventSource](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (обозреватель API .NET)</span><span class="sxs-lookup"><span data-stu-id="20701-504">[LoggingEventSource Class](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (.NET API Browser)</span></span>
* <xref:System.Diagnostics.Tracing.EventLevel>
* <span data-ttu-id="20701-505">[Источник ссылки LoggingEventSource (3.0)](https://github.com/aspnet/Extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) &ndash; Чтобы получить источник ссылки для другой версии, измените ветку на `release/{Version}`, где `{Version}` — это нужная версия ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20701-505">[LoggingEventSource reference source (3.0)](https://github.com/aspnet/Extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) &ndash; To obtain reference source for a different version, change the branch to `release/{Version}`, where `{Version}` is the version of ASP.NET Core desired.</span></span>
* <span data-ttu-id="20701-506">[Perfview](#perfview) &ndash; полезный инструмент для просмотра трассировок источника событий.</span><span class="sxs-lookup"><span data-stu-id="20701-506">[Perfview](#perfview) &ndash; Useful for viewing Event Source traces.</span></span>

#### <a name="perfview"></a><span data-ttu-id="20701-507">Perfview</span><span class="sxs-lookup"><span data-stu-id="20701-507">Perfview</span></span>

::: moniker-end

<span data-ttu-id="20701-508">Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="20701-508">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="20701-509">Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20701-509">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="20701-510">Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**.</span><span class="sxs-lookup"><span data-stu-id="20701-510">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="20701-511">(Не пропустите звездочку в начале строки.)</span><span class="sxs-lookup"><span data-stu-id="20701-511">(Don't miss the asterisk at the start of the string.)</span></span>

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="20701-513">Поставщик Windows EventLog</span><span class="sxs-lookup"><span data-stu-id="20701-513">Windows EventLog provider</span></span>

<span data-ttu-id="20701-514">Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="20701-514">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="20701-515">[Перегрузки AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) позволяют передавать <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span><span class="sxs-lookup"><span data-stu-id="20701-515">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="20701-516">Если `null` или не указан, используются следующие параметры по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="20701-516">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="20701-517">`LogName` &ndash; "Приложение"</span><span class="sxs-lookup"><span data-stu-id="20701-517">`LogName` &ndash; "Application"</span></span>
* <span data-ttu-id="20701-518">`SourceName` &ndash; "Среда выполнения .NET"</span><span class="sxs-lookup"><span data-stu-id="20701-518">`SourceName` &ndash; ".NET Runtime"</span></span>
* <span data-ttu-id="20701-519">`MachineName` &ndash; (локальный компьютер)</span><span class="sxs-lookup"><span data-stu-id="20701-519">`MachineName` &ndash; local machine</span></span>

<span data-ttu-id="20701-520">События регистрируются для [уровня предупреждения и более высоких уровней](#log-level).</span><span class="sxs-lookup"><span data-stu-id="20701-520">Events are logged for [Warning level and higher](#log-level).</span></span> <span data-ttu-id="20701-521">Чтобы регистрировать события с уровнем ниже `Warning`, следует явно задать уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="20701-521">To log events lower than `Warning`, explicitly set the log level.</span></span> <span data-ttu-id="20701-522">Например, добавьте следующий код в файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="20701-522">For example, add the following to the *appsettings.json* file:</span></span>

```json
"EventLog": {
  "LogLevel": {
    "Default": "Information"
  }
}
```

### <a name="tracesource-provider"></a><span data-ttu-id="20701-523">Поставщик TraceSource</span><span class="sxs-lookup"><span data-stu-id="20701-523">TraceSource provider</span></span>

<span data-ttu-id="20701-524">Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="20701-524">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="20701-525">[Перегрузки AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) позволяют передавать исходный параметр и прослушиватель трассировки.</span><span class="sxs-lookup"><span data-stu-id="20701-525">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="20701-526">Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core).</span><span class="sxs-lookup"><span data-stu-id="20701-526">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="20701-527">Поставщик позволяет перенаправлять сообщения различным [прослушивателям](/dotnet/framework/debug-trace-profile/trace-listeners), например <xref:System.Diagnostics.TextWriterTraceListener>, который используется в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-527">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="20701-528">Поставщик службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="20701-528">Azure App Service provider</span></span>

<span data-ttu-id="20701-529">Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure.</span><span class="sxs-lookup"><span data-stu-id="20701-529">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="20701-530">Пакет поставщика не включен в общую платформу.</span><span class="sxs-lookup"><span data-stu-id="20701-530">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="20701-531">Чтобы использовать поставщик, добавьте пакет поставщика в проект.</span><span class="sxs-lookup"><span data-stu-id="20701-531">To use the provider, add the provider package to the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="20701-532">Этот пакет не входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="20701-532">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="20701-533">Если планируется использовать .NET Framework или будет указана ссылка на метапакет `Microsoft.AspNetCore.App`, добавьте пакет поставщика в проект.</span><span class="sxs-lookup"><span data-stu-id="20701-533">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="20701-534">Для настройки параметров поставщика используйте <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> и <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="20701-534">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="20701-535">Для настройки параметров поставщика используйте <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> и <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="20701-535">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="20701-536">Перегрузка <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> позволяет передавать <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="20701-536">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="20701-537">Объект параметров может переопределять параметры по умолчанию, например шаблон выходных данных ведения журналов, имя BLOB-объекта и максимально допустимый размер файла.</span><span class="sxs-lookup"><span data-stu-id="20701-537">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="20701-538">(*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам наряду с тем, что предоставляется при вызове метода `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="20701-538">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

<span data-ttu-id="20701-539">При развертывании в приложение Службы приложений ваше приложение использует параметры в разделе [Журналы Службы приложений](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="20701-539">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="20701-540">При обновлении следующих параметров изменения вступают в силу немедленно без перезапуска или повторного развертывания приложения:</span><span class="sxs-lookup"><span data-stu-id="20701-540">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="20701-541">**Ведение журнала приложения (файловая система)** ;</span><span class="sxs-lookup"><span data-stu-id="20701-541">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="20701-542">**Ведение журнала приложения (BLOB-объект)** .</span><span class="sxs-lookup"><span data-stu-id="20701-542">**Application Logging (Blob)**</span></span>

<span data-ttu-id="20701-543">По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="20701-543">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="20701-544">Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2.</span><span class="sxs-lookup"><span data-stu-id="20701-544">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="20701-545">Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="20701-545">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="20701-546">Поставщик работает только при выполнении проекта в среде Azure.</span><span class="sxs-lookup"><span data-stu-id="20701-546">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="20701-547">Он не функционирует при запуске проекта в локальной среде &mdash;(то есть не выполняет запись в локальные файлы или в локальное хранилище разработки для больших двоичных объектов).</span><span class="sxs-lookup"><span data-stu-id="20701-547">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="20701-548">Потоковая передача журналов Azure</span><span class="sxs-lookup"><span data-stu-id="20701-548">Azure log streaming</span></span>

<span data-ttu-id="20701-549">Потоковая передача журналов Azure позволяет просматривать активность журнала в реальном времени из следующих источников:</span><span class="sxs-lookup"><span data-stu-id="20701-549">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="20701-550">сервер приложений;</span><span class="sxs-lookup"><span data-stu-id="20701-550">The app server</span></span>
* <span data-ttu-id="20701-551">веб-сервер;</span><span class="sxs-lookup"><span data-stu-id="20701-551">The web server</span></span>
* <span data-ttu-id="20701-552">трассировка неудачно завершенных запросов.</span><span class="sxs-lookup"><span data-stu-id="20701-552">Failed request tracing</span></span>

<span data-ttu-id="20701-553">Настройка потоковой передачи журналов Azure</span><span class="sxs-lookup"><span data-stu-id="20701-553">To configure Azure log streaming:</span></span>

* <span data-ttu-id="20701-554">Со страницы портала приложения перейдите на страницу **Журналы Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="20701-554">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="20701-555">**Включите** параметр **Ведение журнала приложения (файловая система)** .</span><span class="sxs-lookup"><span data-stu-id="20701-555">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="20701-556">Выберите **уровень** ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="20701-556">Choose the log **Level**.</span></span> <span data-ttu-id="20701-557">Этот параметр применяется только к потоковой передаче журналов Azure, а не к другим поставщикам ведения журнала в приложении.</span><span class="sxs-lookup"><span data-stu-id="20701-557">This setting only applies to Azure log streaming, not other logging providers in the app.</span></span>

<span data-ttu-id="20701-558">Перейдите на страницу **Поток журналов**, чтобы просмотреть сообщения приложения.</span><span class="sxs-lookup"><span data-stu-id="20701-558">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="20701-559">Они записываются в журнал приложением через интерфейс `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="20701-559">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="20701-560">Ведение журнала трассировки Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="20701-560">Azure Application Insights trace logging</span></span>

<span data-ttu-id="20701-561">Пакет поставщика [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) записывает журналы в Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="20701-561">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="20701-562">Служба Application Insights отслеживает веб-приложения и предоставляет средства для создания запросов и анализа данных телеметрии.</span><span class="sxs-lookup"><span data-stu-id="20701-562">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="20701-563">Используя этого поставщика, вы сможете выполнять запросы к журналам и их анализ с помощью средств Application Insights.</span><span class="sxs-lookup"><span data-stu-id="20701-563">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="20701-564">Поставщик ведения журнала включается как зависимость [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Этот пакет предоставляет всю доступную телеметрию для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20701-564">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="20701-565">Если вы используете этот пакет, пакет поставщика устанавливать не нужно.</span><span class="sxs-lookup"><span data-stu-id="20701-565">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="20701-566">Не используйте пакет [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web), который предназначен для ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="20701-566">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="20701-567">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="20701-567">For more information, see the following resources:</span></span>

* [<span data-ttu-id="20701-568">Общие сведения об Application Insights</span><span class="sxs-lookup"><span data-stu-id="20701-568">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="20701-569">[Application Insights для приложений ASP.NET Core](/azure/azure-monitor/app/asp-net-core) — начните изучение с этой статьи, если вы хотите полностью реализовать возможности Application Insights для телеметрии и ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-569">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="20701-570">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) (Поставщик ведения журналов Application Insights для журналов .NET Core ILogger) — начните изучение с этой статьи, если вы хотите внедрить поставщика ведения журналов, не используя остальные возможности Application Insights для телеметрии.</span><span class="sxs-lookup"><span data-stu-id="20701-570">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="20701-571">[Адаптеры ведения журналов в Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="20701-571">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="20701-572">[Инструментирование серверного кода веб-приложения с помощью Application Insights](/learn/modules/instrument-web-app-code-with-application-insights) — интерактивный учебник на сайте Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="20701-572">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="20701-573">Сторонние поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="20701-573">Third-party logging providers</span></span>

<span data-ttu-id="20701-574">Некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="20701-574">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="20701-575">[ELMAH.IO](https://elmah.io/) ([в репозитории GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="20701-575">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="20701-576">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([в репозитории GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="20701-576">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="20701-577">[JSNLog](https://jsnlog.com/) ([в репозитории GitHub](https://github.com/mperdeck/jsnlog));</span><span class="sxs-lookup"><span data-stu-id="20701-577">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="20701-578">[KissLog.net](https://kisslog.net/) ([репозиторий GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="20701-578">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="20701-579">[Log4Net](https://logging.apache.org/log4net/) ([репозиторий GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="20701-579">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="20701-580">[Loggr](https://loggr.net/) ([в репозитории GitHub](https://github.com/imobile3/Loggr.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="20701-580">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="20701-581">[NLog](https://nlog-project.org/) ([в репозитории GitHub](https://github.com/NLog/NLog.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="20701-581">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="20701-582">[Sentry](https://sentry.io/welcome/) ([репозиторий GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="20701-582">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="20701-583">[Serilog](https://serilog.net/) ([в репозитории GitHub](https://github.com/serilog/serilog-aspnetcore)).</span><span class="sxs-lookup"><span data-stu-id="20701-583">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="20701-584">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([репозиторий Github](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="20701-584">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="20701-585">Некоторые сторонние платформы выполняют [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="20701-585">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="20701-586">Использование сторонней платформы аналогично использованию одного из встроенных поставщиков:</span><span class="sxs-lookup"><span data-stu-id="20701-586">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="20701-587">Добавьте пакет NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="20701-587">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="20701-588">Вызовите метод расширения `ILoggerFactory`, предоставляемый платформой ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="20701-588">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="20701-589">Дополнительные сведения см. в документации по каждому поставщику.</span><span class="sxs-lookup"><span data-stu-id="20701-589">For more information, see each provider's documentation.</span></span> <span data-ttu-id="20701-590">Сторонние поставщики ведения журналов не поддерживаются корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="20701-590">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="20701-591">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="20701-591">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
