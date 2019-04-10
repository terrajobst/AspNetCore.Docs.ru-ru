---
title: Ведение журналов в ASP.NET Core
author: tdykstra
description: Сведения о платформе ведения журналов в ASP.NET Core. Ознакомьтесь со встроенными поставщиками ведения журналов и получите подробные сведения о распространенных сторонних поставщиках.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 065b2016d3a2dcc2243ec6869e027c5fabe4dad8
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068408"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="96c19-104">Ведение журналов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96c19-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="96c19-105">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="96c19-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="96c19-106">ASP.NET Core поддерживает API ведения журналов, который работает с разными встроенными и сторонними поставщиками.</span><span class="sxs-lookup"><span data-stu-id="96c19-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="96c19-107">В этой статье описано, как использовать в коде API ведения журналов, работающего со встроенными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="96c19-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="96c19-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96c19-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="96c19-109">Добавление поставщиков</span><span class="sxs-lookup"><span data-stu-id="96c19-109">Add providers</span></span>

<span data-ttu-id="96c19-110">Поставщик ведения журналов отображает или сохраняет журналы.</span><span class="sxs-lookup"><span data-stu-id="96c19-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="96c19-111">Например, поставщик Console отображает журналы на консоли, а поставщик Azure Application Insights может сохранить их в Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="96c19-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="96c19-112">Журналы могут отправляться в несколько назначений после добавления нескольких поставщиков.</span><span class="sxs-lookup"><span data-stu-id="96c19-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="96c19-113">Для добавления поставщика вызовите метод расширения `Add{provider name}` поставщика в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="96c19-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

<span data-ttu-id="96c19-114">Шаблон проекта по умолчанию вызывает метод <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, который добавляет следующие поставщики ведения журналов:</span><span class="sxs-lookup"><span data-stu-id="96c19-114">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="96c19-115">Консоль</span><span class="sxs-lookup"><span data-stu-id="96c19-115">Console</span></span>
* <span data-ttu-id="96c19-116">Отладка</span><span class="sxs-lookup"><span data-stu-id="96c19-116">Debug</span></span>
* <span data-ttu-id="96c19-117">EventSource (начиная с версии ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="96c19-117">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="96c19-118">Если вы используете `CreateDefaultBuilder`, поставщики по умолчанию можно заменить собственными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="96c19-118">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="96c19-119">Вызовите <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> и добавьте требуемые поставщики.</span><span class="sxs-lookup"><span data-stu-id="96c19-119">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="96c19-120">Чтобы использовать поставщик, установите его пакет NuGet и вызовите метод расширения поставщика в экземпляре <xref:Microsoft.Extensions.Logging.ILoggerFactory>.</span><span class="sxs-lookup"><span data-stu-id="96c19-120">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="96c19-121">[Внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) ASP.NET Core предоставляет экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="96c19-121">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="96c19-122">Методы расширения `AddConsole` и `AddDebug` определены в пакетах [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="96c19-122">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="96c19-123">Каждый метод расширения вызывает метод `ILoggerFactory.AddProvider`, передавая экземпляр поставщика.</span><span class="sxs-lookup"><span data-stu-id="96c19-123">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="96c19-124">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) добавляет поставщиков ведения журнала в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="96c19-124">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="96c19-125">Чтобы получить выходные данные журнала из выполняемого ранее кода, добавьте поставщики ведения журналов в конструктор класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="96c19-125">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="96c19-126">См. сведения о [встроенных](#built-in-logging-providers) и [сторонних](#third-party-logging-providers) поставщиках ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-126">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="96c19-127">Создание журналов</span><span class="sxs-lookup"><span data-stu-id="96c19-127">Create logs</span></span>

<span data-ttu-id="96c19-128">Получите <xref:Microsoft.Extensions.Logging.ILogger%601> объект путем внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="96c19-128">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="96c19-129">В следующем примере контроллера создаются журналы `Information` и `Warning`.</span><span class="sxs-lookup"><span data-stu-id="96c19-129">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="96c19-130">*Категория* — это `TodoApiSample.Controllers.TodoController` (полное имя класса `TodoController` в примере приложения):</span><span class="sxs-lookup"><span data-stu-id="96c19-130">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="96c19-131">В следующем примере Razor Pages создаются журналы с `Information` в качестве *категории* и `TodoApiSample.Pages.AboutModel` в качестве *уровня*:</span><span class="sxs-lookup"><span data-stu-id="96c19-131">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="96c19-132">В предыдущем примере создаются журналы с `Information` и `Warning` в качестве *уровня* и классом `TodoController` в качестве *категории*.</span><span class="sxs-lookup"><span data-stu-id="96c19-132">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="96c19-133">*Уровень* ведения журналов определяет степень серьезности или важности записанного в журнал события.</span><span class="sxs-lookup"><span data-stu-id="96c19-133">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="96c19-134">*Категория* ведения журналов представляет строку, которая связана с каждым журналом.</span><span class="sxs-lookup"><span data-stu-id="96c19-134">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="96c19-135">Экземпляр `ILogger<T>` создает журналы с полным именем типа `T` в качестве категории.</span><span class="sxs-lookup"><span data-stu-id="96c19-135">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="96c19-136">[Уровни](#log-level) и [категории](#log-category) рассматриваются подробнее далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="96c19-136">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="96c19-137">Создание журналов в классе Startup</span><span class="sxs-lookup"><span data-stu-id="96c19-137">Create logs in Startup</span></span>

<span data-ttu-id="96c19-138">Для записи журналов в классе `Startup` включите параметр `ILogger` в сигнатуру конструктора:</span><span class="sxs-lookup"><span data-stu-id="96c19-138">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="96c19-139">Создание журналов в классе Program</span><span class="sxs-lookup"><span data-stu-id="96c19-139">Create logs in Program</span></span>

<span data-ttu-id="96c19-140">Для записи журналов в классе `Program` получите экземпляр `ILogger` путем внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="96c19-140">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="96c19-141">Асинхронные методы ведения журналов не выполняются</span><span class="sxs-lookup"><span data-stu-id="96c19-141">No asynchronous logger methods</span></span>

<span data-ttu-id="96c19-142">Скорость ведения журналов не должна влиять на производительность выполнения асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="96c19-142">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="96c19-143">Если хранилище данных, предназначенное для регистрации сообщений журнала, работает медленно, сначала записывайте эти сообщения в быстродействующее хранилище,</span><span class="sxs-lookup"><span data-stu-id="96c19-143">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="96c19-144">а затем перемещайте их в медленное хранилище.</span><span class="sxs-lookup"><span data-stu-id="96c19-144">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="96c19-145">Например, если вы входите в SQL Server, вам не нужно делать это непосредственно в методе `Log`, так как методы `Log` являются синхронными.</span><span class="sxs-lookup"><span data-stu-id="96c19-145">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="96c19-146">Вместо этого синхронно добавьте сообщения журнала в очередь в памяти, и фоновый рабочий поток извлечет сообщения из очереди для выполнения асинхронных операций передачи данных в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="96c19-146">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="96c19-147">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="96c19-147">Configuration</span></span>

<span data-ttu-id="96c19-148">Конфигурацию поставщика ведения журналов предоставляет как минимум один поставщик конфигураций:</span><span class="sxs-lookup"><span data-stu-id="96c19-148">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="96c19-149">Форматы файлов (INI, JSON и XML).</span><span class="sxs-lookup"><span data-stu-id="96c19-149">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="96c19-150">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="96c19-150">Command-line arguments.</span></span>
* <span data-ttu-id="96c19-151">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="96c19-151">Environment variables.</span></span>
* <span data-ttu-id="96c19-152">Объекты .NET в памяти.</span><span class="sxs-lookup"><span data-stu-id="96c19-152">In-memory .NET objects.</span></span>
* <span data-ttu-id="96c19-153">Незашифрованное хранилище [Secret Manager](xref:security/app-secrets) (Диспетчер секретов).</span><span class="sxs-lookup"><span data-stu-id="96c19-153">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="96c19-154">Зашифрованное пользовательское хранилище, например [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="96c19-154">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="96c19-155">Пользовательские поставщики (установленные или созданные).</span><span class="sxs-lookup"><span data-stu-id="96c19-155">Custom providers (installed or created).</span></span>

<span data-ttu-id="96c19-156">Например, конфигурацию ведения журналов обычно предоставляет раздел `Logging` в файле параметров приложения.</span><span class="sxs-lookup"><span data-stu-id="96c19-156">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="96c19-157">В следующем примере показано содержимое типичного файла *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="96c19-157">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="96c19-158">Свойство `Logging` может иметь свойство `LogLevel` и свойства поставщика журналов (здесь — Console).</span><span class="sxs-lookup"><span data-stu-id="96c19-158">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="96c19-159">Свойство `LogLevel` в разделе `Logging` указывает минимальный [уровень](#log-level) журнала для выбранных категорий.</span><span class="sxs-lookup"><span data-stu-id="96c19-159">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="96c19-160">В этом примере категории `System` и `Microsoft` записываются на уровне `Information`, а остальные — на уровне `Debug`.</span><span class="sxs-lookup"><span data-stu-id="96c19-160">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="96c19-161">Другие свойства в разделе `Logging` определяют поставщиков ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-161">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="96c19-162">Пример приведен для поставщика Console.</span><span class="sxs-lookup"><span data-stu-id="96c19-162">The example is for the Console provider.</span></span> <span data-ttu-id="96c19-163">Если поставщик поддерживает [области журналов](#log-scopes), `IncludeScopes` определяет, включены ли они.</span><span class="sxs-lookup"><span data-stu-id="96c19-163">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="96c19-164">Свойство поставщика (например, `Console` в этом примере) также определять свойство `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="96c19-164">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> `LogLevel` <span data-ttu-id="96c19-165">под поставщиком указывает уровни журнала для этого поставщика.</span><span class="sxs-lookup"><span data-stu-id="96c19-165">under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="96c19-166">Если уровни указаны в `Logging.{providername}.LogLevel`, они не переопределяют ничего из того, что задано в `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="96c19-166">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

`LogLevel` <span data-ttu-id="96c19-167">представляют имена журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-167">keys represent log names.</span></span> <span data-ttu-id="96c19-168">Ключ `Default` применяется к журналам, которые не указаны явно.</span><span class="sxs-lookup"><span data-stu-id="96c19-168">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="96c19-169">Значение представляет [уровень ведения журнала](#log-level), который применен к указанному журналу.</span><span class="sxs-lookup"><span data-stu-id="96c19-169">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="96c19-170">Сведения о реализации поставщиков конфигураций см. в статье <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="96c19-170">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="96c19-171">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="96c19-171">Sample logging output</span></span>

<span data-ttu-id="96c19-172">В примере кода из предыдущего раздела журналы появляются в консоли после запуска приложения из командной строки.</span><span class="sxs-lookup"><span data-stu-id="96c19-172">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="96c19-173">Ниже приведен пример выходных данных консоли:</span><span class="sxs-lookup"><span data-stu-id="96c19-173">Here's an example of console output:</span></span>

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

<span data-ttu-id="96c19-174">Предыдущие журналы созданы путем отправки HTTP-запроса Get к примеру приложения по адресу `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="96c19-174">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="96c19-175">Ниже приведен пример этих журналов в том виде, в каком они отображаются в окне отладки при запуске примера приложения в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="96c19-175">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="96c19-176">Журналы, которые созданы путем вызовов `ILogger`, как показано в предыдущем разделе, начинаются с TodoApi.Controllers.TodoController.</span><span class="sxs-lookup"><span data-stu-id="96c19-176">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="96c19-177">Журналы, которые начинаются с категорий Microsoft, входят в код платформы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96c19-177">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="96c19-178">В ASP.NET Core и коде приложения используется один и тот же API ведения журналов и одни и те же поставщики.</span><span class="sxs-lookup"><span data-stu-id="96c19-178">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="96c19-179">В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-179">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="96c19-180">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="96c19-180">NuGet packages</span></span>

<span data-ttu-id="96c19-181">Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="96c19-181">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="96c19-182">Категория журнала</span><span class="sxs-lookup"><span data-stu-id="96c19-182">Log category</span></span>

<span data-ttu-id="96c19-183">При создании объекта `ILogger` для него указывается *категория*.</span><span class="sxs-lookup"><span data-stu-id="96c19-183">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="96c19-184">Эта категория входит в состав каждого сообщения журнала, создаваемого этим экземпляром `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="96c19-184">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="96c19-185">Категория может быть любой строкой, обычно используется имя класса, например TodoApi.Controllers.TodoController.</span><span class="sxs-lookup"><span data-stu-id="96c19-185">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="96c19-186">Используйте `ILogger<T>` для получения экземпляра `ILogger`, который использует полное имя типа `T` в качестве категории:</span><span class="sxs-lookup"><span data-stu-id="96c19-186">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="96c19-187">Чтобы явно указать категорию, вызовите `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="96c19-187">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>` <span data-ttu-id="96c19-188">эквивалентно вызову `CreateLogger` с полным именем типа `T`.</span><span class="sxs-lookup"><span data-stu-id="96c19-188">is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="96c19-189">Уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="96c19-189">Log level</span></span>

<span data-ttu-id="96c19-190">Каждый журнал задает значение <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="96c19-190">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="96c19-191">Уровень ведения журналов означает степень серьезности или важности.</span><span class="sxs-lookup"><span data-stu-id="96c19-191">The log level indicates the severity or importance.</span></span> <span data-ttu-id="96c19-192">Например, можно создать журнал `Information` при нормальном завершении метода и журнал `Warning`, если метод возвращает код состояния *404 — не найдено*.</span><span class="sxs-lookup"><span data-stu-id="96c19-192">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="96c19-193">Следующий код создает журналы `Information`и `Warning`:</span><span class="sxs-lookup"><span data-stu-id="96c19-193">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="96c19-194">В коде выше первый параметр — это [идентификатор события журнала](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="96c19-194">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="96c19-195">Второй параметр — это шаблон сообщения с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода.</span><span class="sxs-lookup"><span data-stu-id="96c19-195">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="96c19-196">Параметры метода рассматриваются более подробно в разделе, посвященном [шаблону сообщений](#log-message-template) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="96c19-196">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="96c19-197">Методы журналов, которые содержат уровень в имени метода (например, `LogInformation` и `LogWarning`), являются [методами расширения для ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="96c19-197">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="96c19-198">Эти методы вызывают метод `Log`, принимающий параметр `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="96c19-198">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="96c19-199">Метод `Log`, в отличие от этих методов расширения, можно вызывать напрямую, однако его синтаксис довольно сложен.</span><span class="sxs-lookup"><span data-stu-id="96c19-199">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="96c19-200">См. сведения о <xref:Microsoft.Extensions.Logging.ILogger> и [исходном коде расширений средства ведения журналов](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="96c19-200">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="96c19-201">В ASP.NET Core определяются следующие уровни ведения журналов, упорядоченные по возрастанию уровней серьезности.</span><span class="sxs-lookup"><span data-stu-id="96c19-201">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="96c19-202">Трассировка = 0</span><span class="sxs-lookup"><span data-stu-id="96c19-202">Trace = 0</span></span>

  <span data-ttu-id="96c19-203">Для получения сведений, которые полезны только при отладке.</span><span class="sxs-lookup"><span data-stu-id="96c19-203">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="96c19-204">Эти сообщения могут содержать конфиденциальные данные приложения, поэтому их не следует включать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="96c19-204">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> *<span data-ttu-id="96c19-205">По умолчанию отключено.</span><span class="sxs-lookup"><span data-stu-id="96c19-205">Disabled by default.</span></span>*

* <span data-ttu-id="96c19-206">Отладка = 1</span><span class="sxs-lookup"><span data-stu-id="96c19-206">Debug = 1</span></span>

  <span data-ttu-id="96c19-207">Для получения сведений, которые полезны при разработке и отладке.</span><span class="sxs-lookup"><span data-stu-id="96c19-207">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="96c19-208">Пример `Entering method Configure with flag set to true.` Включайте уровни ведения журналов `Debug` в рабочей среде только при устранении неполадок, так как такие журналы занимают много места.</span><span class="sxs-lookup"><span data-stu-id="96c19-208">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="96c19-209">Информация = 2</span><span class="sxs-lookup"><span data-stu-id="96c19-209">Information = 2</span></span>

  <span data-ttu-id="96c19-210">Для отслеживания общего потока работы приложения.</span><span class="sxs-lookup"><span data-stu-id="96c19-210">For tracking the general flow of the app.</span></span> <span data-ttu-id="96c19-211">Эти журналы обычно полезны в долгосрочной перспективе.</span><span class="sxs-lookup"><span data-stu-id="96c19-211">These logs typically have some long-term value.</span></span> <span data-ttu-id="96c19-212">Пример</span><span class="sxs-lookup"><span data-stu-id="96c19-212">Example:</span></span> `Request received for path /api/todo`

* <span data-ttu-id="96c19-213">Предупреждение = 3</span><span class="sxs-lookup"><span data-stu-id="96c19-213">Warning = 3</span></span>

  <span data-ttu-id="96c19-214">Для нестандартных или непредвиденных событий, возникающих в потоке работы приложения.</span><span class="sxs-lookup"><span data-stu-id="96c19-214">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="96c19-215">Это могут быть ошибки или другие условия, которые не приводят к остановке приложения, но которые нужно изучить.</span><span class="sxs-lookup"><span data-stu-id="96c19-215">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="96c19-216">Обработанные исключения являются распространенным условием использования уровня ведения журнала `Warning`.</span><span class="sxs-lookup"><span data-stu-id="96c19-216">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="96c19-217">Пример</span><span class="sxs-lookup"><span data-stu-id="96c19-217">Example:</span></span> `FileNotFoundException for file quotes.txt.`

* <span data-ttu-id="96c19-218">Ошибка = 4</span><span class="sxs-lookup"><span data-stu-id="96c19-218">Error = 4</span></span>

  <span data-ttu-id="96c19-219">Для ошибок и исключений, которые не могут быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="96c19-219">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="96c19-220">Эти сообщения указывают на сбой текущего действия или операции (например, текущего HTTP-запроса), а не на ошибку уровня приложения.</span><span class="sxs-lookup"><span data-stu-id="96c19-220">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="96c19-221">Пример сообщения журнала:</span><span class="sxs-lookup"><span data-stu-id="96c19-221">Example log message:</span></span> `Cannot insert record due to duplicate key violation.`

* <span data-ttu-id="96c19-222">Критический = 5</span><span class="sxs-lookup"><span data-stu-id="96c19-222">Critical = 5</span></span>

  <span data-ttu-id="96c19-223">Для сбоев, которые требуют немедленного внимания.</span><span class="sxs-lookup"><span data-stu-id="96c19-223">For failures that require immediate attention.</span></span> <span data-ttu-id="96c19-224">Примеры: потеря данных, нехватка места на диске.</span><span class="sxs-lookup"><span data-stu-id="96c19-224">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="96c19-225">Уровень ведения журналов можно использовать для управления объемом выходных данных, записываемых на определенный носитель или в окно отображения.</span><span class="sxs-lookup"><span data-stu-id="96c19-225">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="96c19-226">Например:</span><span class="sxs-lookup"><span data-stu-id="96c19-226">For example:</span></span>

* <span data-ttu-id="96c19-227">В рабочей среде отправьте `Trace` в контексте уровня `Information` в хранилище томов.</span><span class="sxs-lookup"><span data-stu-id="96c19-227">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="96c19-228">Отправьте `Warning` в контексте уровня `Critical` в хранилище томов.</span><span class="sxs-lookup"><span data-stu-id="96c19-228">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="96c19-229">При разработке отправьте `Warning` в контексте `Critical` в консоль и добавьте `Trace` в контексте `Information` при устранении неполадок.</span><span class="sxs-lookup"><span data-stu-id="96c19-229">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="96c19-230">В разделе [Фильтрация журналов](#log-filtering) далее в этой статье приводятся сведения о том, как контролировать уровни журнала, обрабатываемые поставщиком.</span><span class="sxs-lookup"><span data-stu-id="96c19-230">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="96c19-231">ASP.NET Core создает журналы для событий платформы.</span><span class="sxs-lookup"><span data-stu-id="96c19-231">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="96c19-232">В примерах журнала ранее в этой статье не указывались журналы с уровнем ниже `Information`, поэтому журналы уровня `Debug` или `Trace` не создавались.</span><span class="sxs-lookup"><span data-stu-id="96c19-232">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="96c19-233">Ниже приведен пример журналов консоли, которые созданы при запуске примера приложения, настроенного на отображение журналов `Debug`:</span><span class="sxs-lookup"><span data-stu-id="96c19-233">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="96c19-234">Идентификатор события журнала</span><span class="sxs-lookup"><span data-stu-id="96c19-234">Log event ID</span></span>

<span data-ttu-id="96c19-235">Каждый журнал может указывать *идентификатор события*.</span><span class="sxs-lookup"><span data-stu-id="96c19-235">Each log can specify an *event ID*.</span></span> <span data-ttu-id="96c19-236">В примере приложения для этого используется локально определенный класс `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="96c19-236">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="96c19-237">Идентификатор события связывает набор событий.</span><span class="sxs-lookup"><span data-stu-id="96c19-237">An event ID associates a set of events.</span></span> <span data-ttu-id="96c19-238">Например, все журналы, связанные с отображением списка элементов на странице, могут иметь значение 1001.</span><span class="sxs-lookup"><span data-stu-id="96c19-238">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="96c19-239">Поставщик ведения журналов может хранить идентификатор события в поле идентификатора, в сообщении журнала, или вообще не хранить.</span><span class="sxs-lookup"><span data-stu-id="96c19-239">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="96c19-240">Поставщик Debug не отображает идентификаторы событий.</span><span class="sxs-lookup"><span data-stu-id="96c19-240">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="96c19-241">Поставщик Console отображает идентификаторы событий в квадратных скобках после категории:</span><span class="sxs-lookup"><span data-stu-id="96c19-241">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="96c19-242">Шаблон сообщения журнала</span><span class="sxs-lookup"><span data-stu-id="96c19-242">Log message template</span></span>

<span data-ttu-id="96c19-243">Каждый журнал указывает шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="96c19-243">Each log specifies a message template.</span></span> <span data-ttu-id="96c19-244">Шаблон сообщения может содержать заполнители, для которых предоставляются аргументы.</span><span class="sxs-lookup"><span data-stu-id="96c19-244">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="96c19-245">Используйте для заполнителей имена, а не числа.</span><span class="sxs-lookup"><span data-stu-id="96c19-245">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="96c19-246">Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами.</span><span class="sxs-lookup"><span data-stu-id="96c19-246">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="96c19-247">В приведенном ниже коде обратите внимание на то, что имена параметров идут не по порядку в шаблоне сообщения:</span><span class="sxs-lookup"><span data-stu-id="96c19-247">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="96c19-248">Этот код создает сообщение журнала со значениями параметров в определенном порядке:</span><span class="sxs-lookup"><span data-stu-id="96c19-248">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="96c19-249">Платформа ведения журналов поддерживает такое поведение, чтобы поставщики ведения журнала могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="96c19-249">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="96c19-250">Сами аргументы передаются в систему ведения журналов, а не только в отформатированный шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="96c19-250">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="96c19-251">Эта информация позволяет поставщикам ведения журналов хранить значения параметров как поля.</span><span class="sxs-lookup"><span data-stu-id="96c19-251">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="96c19-252">Предположим, например, что вызовы метода средства ведения журналов выглядят так:</span><span class="sxs-lookup"><span data-stu-id="96c19-252">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="96c19-253">Если вы отправляете журналы в Хранилище таблиц Azure, каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-253">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="96c19-254">Запрос может находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.</span><span class="sxs-lookup"><span data-stu-id="96c19-254">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="96c19-255">Ведение журнала исключений</span><span class="sxs-lookup"><span data-stu-id="96c19-255">Logging exceptions</span></span>

<span data-ttu-id="96c19-256">Методы средства ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="96c19-256">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="96c19-257">Разные поставщики обрабатывают сведения об исключениях по-разному.</span><span class="sxs-lookup"><span data-stu-id="96c19-257">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="96c19-258">Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="96c19-258">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="96c19-259">Фильтрация журналов</span><span class="sxs-lookup"><span data-stu-id="96c19-259">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="96c19-260">Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий.</span><span class="sxs-lookup"><span data-stu-id="96c19-260">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="96c19-261">Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются.</span><span class="sxs-lookup"><span data-stu-id="96c19-261">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="96c19-262">Чтобы проигнорировать все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-262">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="96c19-263">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="96c19-263">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="96c19-264">Создание правил фильтрации в конфигурации</span><span class="sxs-lookup"><span data-stu-id="96c19-264">Create filter rules in configuration</span></span>

<span data-ttu-id="96c19-265">Код шаблона проектов вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug.</span><span class="sxs-lookup"><span data-stu-id="96c19-265">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="96c19-266">Метод `CreateDefaultBuilder` также настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, используя код, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="96c19-266">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

<span data-ttu-id="96c19-267">Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="96c19-267">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="96c19-268">Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно для всех поставщиков.</span><span class="sxs-lookup"><span data-stu-id="96c19-268">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="96c19-269">При создании объекта `ILogger` для каждого поставщика выбирается только одно из этих правил.</span><span class="sxs-lookup"><span data-stu-id="96c19-269">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="96c19-270">Правила фильтрации в коде</span><span class="sxs-lookup"><span data-stu-id="96c19-270">Filter rules in code</span></span>

<span data-ttu-id="96c19-271">В следующем примере показано, как зарегистрировать в коде правила фильтрации:</span><span class="sxs-lookup"><span data-stu-id="96c19-271">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="96c19-272">Второй `AddFilter` указывает поставщика, Debug, используя имя его типа.</span><span class="sxs-lookup"><span data-stu-id="96c19-272">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="96c19-273">Первый `AddFilter` применяется ко всем поставщикам, так как он не определяет тип поставщика.</span><span class="sxs-lookup"><span data-stu-id="96c19-273">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="96c19-274">Применение правил фильтрации</span><span class="sxs-lookup"><span data-stu-id="96c19-274">How filtering rules are applied</span></span>

<span data-ttu-id="96c19-275">Данные конфигурации и код `AddFilter`, приведенный в предыдущих примерах, создают правила, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="96c19-275">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="96c19-276">Первые шесть взяты из примера конфигурации, а последние два — из примера кода.</span><span class="sxs-lookup"><span data-stu-id="96c19-276">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="96c19-277">Число</span><span class="sxs-lookup"><span data-stu-id="96c19-277">Number</span></span> | <span data-ttu-id="96c19-278">Поставщик</span><span class="sxs-lookup"><span data-stu-id="96c19-278">Provider</span></span>      | <span data-ttu-id="96c19-279">Категории, которые начинаются с...</span><span class="sxs-lookup"><span data-stu-id="96c19-279">Categories that begin with ...</span></span>          | <span data-ttu-id="96c19-280">Минимальный уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="96c19-280">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="96c19-281">1</span><span class="sxs-lookup"><span data-stu-id="96c19-281">1</span></span>      | <span data-ttu-id="96c19-282">Отладка</span><span class="sxs-lookup"><span data-stu-id="96c19-282">Debug</span></span>         | <span data-ttu-id="96c19-283">Все категории</span><span class="sxs-lookup"><span data-stu-id="96c19-283">All categories</span></span>                          | <span data-ttu-id="96c19-284">Сведения</span><span class="sxs-lookup"><span data-stu-id="96c19-284">Information</span></span>       |
| <span data-ttu-id="96c19-285">2</span><span class="sxs-lookup"><span data-stu-id="96c19-285">2</span></span>      | <span data-ttu-id="96c19-286">Консоль</span><span class="sxs-lookup"><span data-stu-id="96c19-286">Console</span></span>       | <span data-ttu-id="96c19-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="96c19-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="96c19-288">Предупреждение</span><span class="sxs-lookup"><span data-stu-id="96c19-288">Warning</span></span>           |
| <span data-ttu-id="96c19-289">3</span><span class="sxs-lookup"><span data-stu-id="96c19-289">3</span></span>      | <span data-ttu-id="96c19-290">Консоль</span><span class="sxs-lookup"><span data-stu-id="96c19-290">Console</span></span>       | <span data-ttu-id="96c19-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="96c19-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="96c19-292">Отладка</span><span class="sxs-lookup"><span data-stu-id="96c19-292">Debug</span></span>             |
| <span data-ttu-id="96c19-293">4</span><span class="sxs-lookup"><span data-stu-id="96c19-293">4</span></span>      | <span data-ttu-id="96c19-294">Консоль</span><span class="sxs-lookup"><span data-stu-id="96c19-294">Console</span></span>       | <span data-ttu-id="96c19-295">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="96c19-295">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="96c19-296">Error</span><span class="sxs-lookup"><span data-stu-id="96c19-296">Error</span></span>             |
| <span data-ttu-id="96c19-297">5</span><span class="sxs-lookup"><span data-stu-id="96c19-297">5</span></span>      | <span data-ttu-id="96c19-298">Консоль</span><span class="sxs-lookup"><span data-stu-id="96c19-298">Console</span></span>       | <span data-ttu-id="96c19-299">Все категории</span><span class="sxs-lookup"><span data-stu-id="96c19-299">All categories</span></span>                          | <span data-ttu-id="96c19-300">Сведения</span><span class="sxs-lookup"><span data-stu-id="96c19-300">Information</span></span>       |
| <span data-ttu-id="96c19-301">6</span><span class="sxs-lookup"><span data-stu-id="96c19-301">6</span></span>      | <span data-ttu-id="96c19-302">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="96c19-302">All providers</span></span> | <span data-ttu-id="96c19-303">Все категории</span><span class="sxs-lookup"><span data-stu-id="96c19-303">All categories</span></span>                          | <span data-ttu-id="96c19-304">Отладка</span><span class="sxs-lookup"><span data-stu-id="96c19-304">Debug</span></span>             |
| <span data-ttu-id="96c19-305">7</span><span class="sxs-lookup"><span data-stu-id="96c19-305">7</span></span>      | <span data-ttu-id="96c19-306">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="96c19-306">All providers</span></span> | <span data-ttu-id="96c19-307">Система</span><span class="sxs-lookup"><span data-stu-id="96c19-307">System</span></span>                                  | <span data-ttu-id="96c19-308">Отладка</span><span class="sxs-lookup"><span data-stu-id="96c19-308">Debug</span></span>             |
| <span data-ttu-id="96c19-309">8</span><span class="sxs-lookup"><span data-stu-id="96c19-309">8</span></span>      | <span data-ttu-id="96c19-310">Отладка</span><span class="sxs-lookup"><span data-stu-id="96c19-310">Debug</span></span>         | <span data-ttu-id="96c19-311">Майкрософт</span><span class="sxs-lookup"><span data-stu-id="96c19-311">Microsoft</span></span>                               | <span data-ttu-id="96c19-312">Трассировка</span><span class="sxs-lookup"><span data-stu-id="96c19-312">Trace</span></span>             |

<span data-ttu-id="96c19-313">При создании объекта `ILogger` объект `ILoggerFactory` выбирает одно правило для каждого поставщика, которое будет применено к этому средству ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-313">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="96c19-314">Все сообщения, записываемые с помощью экземпляра `ILogger`, фильтруются на основе выбранных правил.</span><span class="sxs-lookup"><span data-stu-id="96c19-314">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="96c19-315">Самое подходящее правило для каждой пары поставщика и категории выбирается из списка доступных правил.</span><span class="sxs-lookup"><span data-stu-id="96c19-315">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="96c19-316">При создании `ILogger` для данной категории для каждого поставщика используется приведенный далее алгоритм:</span><span class="sxs-lookup"><span data-stu-id="96c19-316">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="96c19-317">Выберите все правила, которые соответствуют поставщику или его псевдониму.</span><span class="sxs-lookup"><span data-stu-id="96c19-317">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="96c19-318">Если ничего не найдено, выберите все правила с пустым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="96c19-318">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="96c19-319">В результатах предыдущего шага выберите правила с самым длинным соответствующим префиксом категории.</span><span class="sxs-lookup"><span data-stu-id="96c19-319">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="96c19-320">Если ничего не найдено, выберите все правила, которые не указывают категорию.</span><span class="sxs-lookup"><span data-stu-id="96c19-320">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="96c19-321">Если выбрано несколько правил, примите **последнее**.</span><span class="sxs-lookup"><span data-stu-id="96c19-321">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="96c19-322">Если правила не выбраны, используйте `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="96c19-322">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="96c19-323">Предположим, у вас есть указанный выше список правил и вы создаете объект `ILogger` для категории Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine:</span><span class="sxs-lookup"><span data-stu-id="96c19-323">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="96c19-324">К поставщику Debug применяются правила 1, 6 и 8.</span><span class="sxs-lookup"><span data-stu-id="96c19-324">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="96c19-325">Правило 8 является наиболее подходящим, поэтому выбрано оно.</span><span class="sxs-lookup"><span data-stu-id="96c19-325">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="96c19-326">К поставщику отладки применяются правила 3, 4, 5 и 6.</span><span class="sxs-lookup"><span data-stu-id="96c19-326">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="96c19-327">Правило 3 является наиболее подходящим.</span><span class="sxs-lookup"><span data-stu-id="96c19-327">Rule 3 is most specific.</span></span>

<span data-ttu-id="96c19-328">Полученный в результате экземпляр `ILogger` отправляет журналы уровня `Trace` и выше в поставщик Debug.</span><span class="sxs-lookup"><span data-stu-id="96c19-328">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="96c19-329">Журналы уровня `Debug` и выше отправляются в поставщик Console.</span><span class="sxs-lookup"><span data-stu-id="96c19-329">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="96c19-330">Псевдонимы поставщиков</span><span class="sxs-lookup"><span data-stu-id="96c19-330">Provider aliases</span></span>

<span data-ttu-id="96c19-331">Каждый поставщик определяет *псевдоним*, используемый в конфигурации вместо полного имени типа.</span><span class="sxs-lookup"><span data-stu-id="96c19-331">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="96c19-332">Для встроенных поставщиков используйте следующие псевдонимы:</span><span class="sxs-lookup"><span data-stu-id="96c19-332">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="96c19-333">Консоль</span><span class="sxs-lookup"><span data-stu-id="96c19-333">Console</span></span>
* <span data-ttu-id="96c19-334">Отладка</span><span class="sxs-lookup"><span data-stu-id="96c19-334">Debug</span></span>
* <span data-ttu-id="96c19-335">EventLog</span><span class="sxs-lookup"><span data-stu-id="96c19-335">EventLog</span></span>
* <span data-ttu-id="96c19-336">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="96c19-336">AzureAppServices</span></span>
* <span data-ttu-id="96c19-337">TraceSource</span><span class="sxs-lookup"><span data-stu-id="96c19-337">TraceSource</span></span>
* <span data-ttu-id="96c19-338">EventSource</span><span class="sxs-lookup"><span data-stu-id="96c19-338">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="96c19-339">Минимальный уровень по умолчанию</span><span class="sxs-lookup"><span data-stu-id="96c19-339">Default minimum level</span></span>

<span data-ttu-id="96c19-340">Существует параметр минимального уровня, который применяется, только если к определенному поставщику или категории не подходят правила из конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="96c19-340">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="96c19-341">В следующем примере показано, как задать минимальный уровень:</span><span class="sxs-lookup"><span data-stu-id="96c19-341">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="96c19-342">Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.</span><span class="sxs-lookup"><span data-stu-id="96c19-342">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="96c19-343">Функции фильтрации</span><span class="sxs-lookup"><span data-stu-id="96c19-343">Filter functions</span></span>

<span data-ttu-id="96c19-344">Функция фильтрации вызывается для всех поставщиков и категорий, которым в конфигурации и (или) коде не назначены правила.</span><span class="sxs-lookup"><span data-stu-id="96c19-344">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="96c19-345">Код в функции имеет доступ к типу поставщика, категории и уровню ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-345">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="96c19-346">Например:</span><span class="sxs-lookup"><span data-stu-id="96c19-346">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="96c19-347">Некоторые поставщики ведения журналов позволяют указывать, когда журналы следует записывать на носитель, а когда — игнорировать. При этом учитывается уровень ведения журнала и категория.</span><span class="sxs-lookup"><span data-stu-id="96c19-347">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="96c19-348">Методы расширения `AddConsole` и `AddDebug`предоставляют перегрузки для принятия условий фильтрации.</span><span class="sxs-lookup"><span data-stu-id="96c19-348">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="96c19-349">В следующем примере кода поставщик Console игнорирует журналы с уровнем ниже `Warning`, а поставщик Debug не учитывает журналы, создаваемые платформой.</span><span class="sxs-lookup"><span data-stu-id="96c19-349">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="96c19-350">Метод `AddEventLog` имеет перегрузку, принимающую экземпляр `EventLogSettings`, который в своем свойстве `Filter` может содержать функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="96c19-350">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="96c19-351">Поставщик TraceSource не предоставляет эти перегрузки, так как уровень ведения журнала и другие параметры определяются для него на основе используемых `SourceSwitch` и `TraceListener`.</span><span class="sxs-lookup"><span data-stu-id="96c19-351">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="96c19-352">Чтобы задать правила фильтрации для всех поставщиков, которые зарегистрированы в экземпляре `ILoggerFactory`, используйте метод расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="96c19-352">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="96c19-353">В приведенном ниже примере журналы платформы (категория начинается с Microsoft или System) ограничиваются предупреждениями. При этом разрешается ведение журналов на уровне отладки для журналов, созданных кодом приложения.</span><span class="sxs-lookup"><span data-stu-id="96c19-353">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="96c19-354">Чтобы запретить запись журналов, укажите `LogLevel.None` как минимальный уровень ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-354">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="96c19-355">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="96c19-355">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="96c19-356">Пакет NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) предоставляет метод расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="96c19-356">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="96c19-357">Метод возвращает новый экземпляр `ILoggerFactory` для фильтрации журнала сообщений, переданного всем поставщикам средства ведения журналов, которые зарегистрированы в этом экземпляре.</span><span class="sxs-lookup"><span data-stu-id="96c19-357">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="96c19-358">Он не влияет на другие экземпляры `ILoggerFactory`, включая исходный экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="96c19-358">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="96c19-359">Системные категории и уровни</span><span class="sxs-lookup"><span data-stu-id="96c19-359">System categories and levels</span></span>

<span data-ttu-id="96c19-360">Ниже приведены некоторые категории, используемые ASP.NET Core и Entity Framework Core с заметками о журналах:</span><span class="sxs-lookup"><span data-stu-id="96c19-360">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="96c19-361">Категория</span><span class="sxs-lookup"><span data-stu-id="96c19-361">Category</span></span>                            | <span data-ttu-id="96c19-362">Примечания</span><span class="sxs-lookup"><span data-stu-id="96c19-362">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="96c19-363">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="96c19-363">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="96c19-364">Общая диагностика ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96c19-364">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="96c19-365">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="96c19-365">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="96c19-366">Распознанные, найденные и использованные ключи.</span><span class="sxs-lookup"><span data-stu-id="96c19-366">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="96c19-367">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="96c19-367">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="96c19-368">Разрешенные узлы.</span><span class="sxs-lookup"><span data-stu-id="96c19-368">Hosts allowed.</span></span> |
| <span data-ttu-id="96c19-369">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="96c19-369">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="96c19-370">Время начала и длительность выполнения HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="96c19-370">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="96c19-371">Определение загруженных начальных сборок размещения.</span><span class="sxs-lookup"><span data-stu-id="96c19-371">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="96c19-372">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="96c19-372">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="96c19-373">Диагностика MVC и Razor.</span><span class="sxs-lookup"><span data-stu-id="96c19-373">MVC and Razor diagnostics.</span></span> <span data-ttu-id="96c19-374">Привязка моделей, выполнение фильтра, компиляция представлений, выбор действия.</span><span class="sxs-lookup"><span data-stu-id="96c19-374">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="96c19-375">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="96c19-375">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="96c19-376">Перенаправление соответствующих сведений.</span><span class="sxs-lookup"><span data-stu-id="96c19-376">Route matching information.</span></span> |
| <span data-ttu-id="96c19-377">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="96c19-377">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="96c19-378">Запуск, остановка и сохранение ответов.</span><span class="sxs-lookup"><span data-stu-id="96c19-378">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="96c19-379">Сведения о сертификате HTTPS.</span><span class="sxs-lookup"><span data-stu-id="96c19-379">HTTPS certificate information.</span></span> |
| <span data-ttu-id="96c19-380">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="96c19-380">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="96c19-381">Обработанные файлы.</span><span class="sxs-lookup"><span data-stu-id="96c19-381">Files served.</span></span> |
| <span data-ttu-id="96c19-382">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="96c19-382">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="96c19-383">Общая диагностика Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="96c19-383">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="96c19-384">Действия и конфигурация базы данных, обнаружение изменений, переносы.</span><span class="sxs-lookup"><span data-stu-id="96c19-384">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="96c19-385">Области журналов</span><span class="sxs-lookup"><span data-stu-id="96c19-385">Log scopes</span></span>

 <span data-ttu-id="96c19-386">*Область* может группировать набор логических операций.</span><span class="sxs-lookup"><span data-stu-id="96c19-386">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="96c19-387">Эту возможность можно использовать для присоединения одних и тех же данных к каждому журналу, созданному как часть набора.</span><span class="sxs-lookup"><span data-stu-id="96c19-387">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="96c19-388">Например, каждый журнал, созданный в ходе обработки транзакции, может включать идентификатор транзакции.</span><span class="sxs-lookup"><span data-stu-id="96c19-388">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="96c19-389">Область — это тип `IDisposable`, возвращаемый методом <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> и действующий до удаления.</span><span class="sxs-lookup"><span data-stu-id="96c19-389">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="96c19-390">Используйте область, заключив вызовы средства ведения журналов в блок `using`:</span><span class="sxs-lookup"><span data-stu-id="96c19-390">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="96c19-391">Следующий код предоставляет области для поставщика Console:</span><span class="sxs-lookup"><span data-stu-id="96c19-391">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="96c19-392">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="96c19-392">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="96c19-393">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-393">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="96c19-394">Сведения о настройках см. в разделе [Конфигурация](#configuration).</span><span class="sxs-lookup"><span data-stu-id="96c19-394">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="96c19-395">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="96c19-395">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="96c19-396">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="96c19-396">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="96c19-397">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="96c19-397">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="96c19-398">Каждое сообщение журнала содержит ограниченную информацию:</span><span class="sxs-lookup"><span data-stu-id="96c19-398">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="96c19-399">Встроенные поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="96c19-399">Built-in logging providers</span></span>

<span data-ttu-id="96c19-400">В состав ASP.NET Core входят следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="96c19-400">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="96c19-401">Консоль</span><span class="sxs-lookup"><span data-stu-id="96c19-401">Console</span></span>](#console-provider)
* [<span data-ttu-id="96c19-402">Отладка</span><span class="sxs-lookup"><span data-stu-id="96c19-402">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="96c19-403">EventSource</span><span class="sxs-lookup"><span data-stu-id="96c19-403">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="96c19-404">EventLog</span><span class="sxs-lookup"><span data-stu-id="96c19-404">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="96c19-405">TraceSource</span><span class="sxs-lookup"><span data-stu-id="96c19-405">TraceSource</span></span>](#tracesource-provider)

<span data-ttu-id="96c19-406">Параметры для [ведения журналов в Azure](#logging-in-azure) рассматриваются далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="96c19-406">Options for [Logging in Azure](#logging-in-azure) are covered later in this article.</span></span>

<span data-ttu-id="96c19-407">См. сведения о <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>ведении журналов stdout<xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="96c19-407">For information about stdout logging, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> and <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="96c19-408">Поставщик Console</span><span class="sxs-lookup"><span data-stu-id="96c19-408">Console provider</span></span>

<span data-ttu-id="96c19-409">Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль.</span><span class="sxs-lookup"><span data-stu-id="96c19-409">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="96c19-410">[Перегрузки AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) позволяют передавать минимальный уровень ведения журналов, функцию фильтрации и логическое значение, определяющее поддержку областей.</span><span class="sxs-lookup"><span data-stu-id="96c19-410">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="96c19-411">Другим вариантом является передача объекта `IConfiguration`, который может указать поддержку областей и уровни ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="96c19-411">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="96c19-412">Поставщик Console оказывает значительное влияние на производительность и потому, как правило, не подходит для использования в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="96c19-412">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="96c19-413">При создании проекта в Visual Studio метод `AddConsole` выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="96c19-413">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="96c19-414">Этот код ссылается на раздел `Logging` файла *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="96c19-414">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="96c19-415">Приведенные параметры ограничивают журналы платформы предупреждениями, но разрешают регистрацию приложений на уровне отладки, как описано в разделе [Фильтрация журналов](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="96c19-415">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="96c19-416">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="96c19-416">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="96c19-417">Чтобы просмотреть выходные данные ведения журналов в консоли, откройте командную строку, перейдите в папку проекта и запустите приведенную ниже команду:</span><span class="sxs-lookup"><span data-stu-id="96c19-417">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="96c19-418">Поставщик Debug</span><span class="sxs-lookup"><span data-stu-id="96c19-418">Debug provider</span></span>

<span data-ttu-id="96c19-419">Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (вызовов метода `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="96c19-419">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="96c19-420">В Linux этот поставщик записывает журналы в каталог */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="96c19-420">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="96c19-421">[Перегрузки AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) позволяют передавать минимальный уровень ведения журнала или функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="96c19-421">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="96c19-422">Поставщик EventSource</span><span class="sxs-lookup"><span data-stu-id="96c19-422">EventSource provider</span></span>

<span data-ttu-id="96c19-423">Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии, реализовывать события трассировки можно с помощью пакета поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource).</span><span class="sxs-lookup"><span data-stu-id="96c19-423">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="96c19-424">В Windows используется [трассировка событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="96c19-424">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="96c19-425">Поставщик является кроссплатформенным, но для Linux и macOS инструменты сбора событий и отображений пока отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="96c19-425">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

<span data-ttu-id="96c19-426">Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="96c19-426">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="96c19-427">Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="96c19-427">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="96c19-428">Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**.</span><span class="sxs-lookup"><span data-stu-id="96c19-428">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="96c19-429">(Не пропустите звездочку в начале строки.)</span><span class="sxs-lookup"><span data-stu-id="96c19-429">(Don't miss the asterisk at the start of the string.)</span></span>

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="96c19-431">Поставщик Windows EventLog</span><span class="sxs-lookup"><span data-stu-id="96c19-431">Windows EventLog provider</span></span>

<span data-ttu-id="96c19-432">Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="96c19-432">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="96c19-433">[Перегрузки AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) позволяют передавать `EventLogSettings` или минимальный уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="96c19-433">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="96c19-434">Поставщик TraceSource</span><span class="sxs-lookup"><span data-stu-id="96c19-434">TraceSource provider</span></span>

<span data-ttu-id="96c19-435">Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="96c19-435">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

<span data-ttu-id="96c19-436">[Перегрузки AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) позволяют передавать исходный параметр и прослушиватель трассировки.</span><span class="sxs-lookup"><span data-stu-id="96c19-436">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="96c19-437">Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core).</span><span class="sxs-lookup"><span data-stu-id="96c19-437">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="96c19-438">Поставщик позволяет перенаправлять сообщения различным [прослушивателям](/dotnet/framework/debug-trace-profile/trace-listeners), например <xref:System.Diagnostics.TextWriterTraceListener>, который используется в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="96c19-438">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="96c19-439">В следующем примере показана настройка поставщика `TraceSource`, записывающего в окне консоли сообщения типа `Warning` и сообщения с более высоким уровнем серьезности.</span><span class="sxs-lookup"><span data-stu-id="96c19-439">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a><span data-ttu-id="96c19-440">Ведение журналов в Azure</span><span class="sxs-lookup"><span data-stu-id="96c19-440">Logging in Azure</span></span>

<span data-ttu-id="96c19-441">См. сведения о ведении журналов в Azure:</span><span class="sxs-lookup"><span data-stu-id="96c19-441">For information about logging in Azure, see the following sections:</span></span>

* [<span data-ttu-id="96c19-442">Поставщик службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="96c19-442">Azure App Service provider</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="96c19-443">Потоковая передача журналов Azure</span><span class="sxs-lookup"><span data-stu-id="96c19-443">Azure log streaming</span></span>](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [<span data-ttu-id="96c19-444">Ведение журнала трассировки Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="96c19-444">Azure Application Insights trace logging</span></span>](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="96c19-445">Поставщик службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="96c19-445">Azure App Service provider</span></span>

<span data-ttu-id="96c19-446">Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure.</span><span class="sxs-lookup"><span data-stu-id="96c19-446">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="96c19-447">Пакет поставщика доступен для приложений, предназначенных для .NET Core 1.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="96c19-447">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="96c19-448">Если планируется использовать .NET Core, обратите внимание на следующее.</span><span class="sxs-lookup"><span data-stu-id="96c19-448">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="96c19-449">Пакет поставщиков включен в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96c19-449">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="96c19-450">Этот пакет не входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="96c19-450">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="96c19-451">Чтобы использовать поставщик, установите пакет.</span><span class="sxs-lookup"><span data-stu-id="96c19-451">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="96c19-452">Не вызывайте <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> явным образом.</span><span class="sxs-lookup"><span data-stu-id="96c19-452">Don't explicitly call <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span></span> <span data-ttu-id="96c19-453">Поставщик становится автоматически доступным для приложения при развертывании приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="96c19-453">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="96c19-454">Если планируется использовать .NET Framework или будет указана ссылка на метапакет `Microsoft.AspNetCore.App`, добавьте пакет поставщика в проект.</span><span class="sxs-lookup"><span data-stu-id="96c19-454">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="96c19-455">Вызовите `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="96c19-455">Invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="96c19-456">Перегрузка <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> позволяет передавать <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="96c19-456">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="96c19-457">Объект параметров может переопределять параметры по умолчанию, например шаблон выходных данных ведения журналов, имя BLOB-объекта и максимально допустимый размер файла.</span><span class="sxs-lookup"><span data-stu-id="96c19-457">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="96c19-458">(*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам наряду с тем, что предоставляется при вызове метода `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="96c19-458">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="96c19-459">Для настройки параметров поставщика используйте <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> и <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="96c19-459">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="96c19-460">При развертывании в приложение службы приложений ваше приложение учитывает параметры в разделе [Журналы диагностики](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="96c19-460">When you deploy to an App Service app, the application honors the settings in the [Diagnostic Logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="96c19-461">При обновлении этих параметров изменения вступают в силу немедленно без перезапуска или повторного развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="96c19-461">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Параметры ведения журнала Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="96c19-463">По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="96c19-463">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="96c19-464">Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2.</span><span class="sxs-lookup"><span data-stu-id="96c19-464">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="96c19-465">Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="96c19-465">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="96c19-466">Поставщик работает только при выполнении проекта в среде Azure.</span><span class="sxs-lookup"><span data-stu-id="96c19-466">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="96c19-467">Он не функционирует при запуске проекта в локальной среде &mdash;(то есть не выполняет запись в локальные файлы или в локальное хранилище разработки для больших двоичных объектов).</span><span class="sxs-lookup"><span data-stu-id="96c19-467">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

### <a name="azure-log-streaming"></a><span data-ttu-id="96c19-468">Потоковая передача журналов Azure</span><span class="sxs-lookup"><span data-stu-id="96c19-468">Azure log streaming</span></span>

<span data-ttu-id="96c19-469">Потоковая передача журналов Azure позволяет просматривать активность журнала в реальном времени из следующих источников:</span><span class="sxs-lookup"><span data-stu-id="96c19-469">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="96c19-470">сервер приложений;</span><span class="sxs-lookup"><span data-stu-id="96c19-470">The app server</span></span>
* <span data-ttu-id="96c19-471">веб-сервер;</span><span class="sxs-lookup"><span data-stu-id="96c19-471">The web server</span></span>
* <span data-ttu-id="96c19-472">трассировка неудачно завершенных запросов.</span><span class="sxs-lookup"><span data-stu-id="96c19-472">Failed request tracing</span></span>

<span data-ttu-id="96c19-473">Настройка потоковой передачи журналов Azure</span><span class="sxs-lookup"><span data-stu-id="96c19-473">To configure Azure log streaming:</span></span>

* <span data-ttu-id="96c19-474">Со страницы портала приложения перейдите на страницу **Журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="96c19-474">Navigate to the **Diagnostics Logs** page from your app's portal page.</span></span>
* <span data-ttu-id="96c19-475">**Включите** параметр **Ведение журнала приложения (файловая система)**.</span><span class="sxs-lookup"><span data-stu-id="96c19-475">Set **Application Logging (Filesystem)** to **On**.</span></span>

![Страница журналов диагностики на портале Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="96c19-477">Перейдите на страницу **Потоковая передача журналов**, чтобы просмотреть сообщения приложения.</span><span class="sxs-lookup"><span data-stu-id="96c19-477">Navigate to the **Log Streaming** page to view app messages.</span></span> <span data-ttu-id="96c19-478">Они записываются в журнал приложением через интерфейс `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="96c19-478">They're logged by the app through the `ILogger` interface.</span></span>

![Потоковая передача журнала приложения на портале Azure](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="96c19-480">Ведение журнала трассировки Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="96c19-480">Azure Application Insights trace logging</span></span>

<span data-ttu-id="96c19-481">Пакет SDK Application Insights может собирать и отправлять журналы, созданные инфраструктурой ведения журналов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96c19-481">The Application Insights SDK can collect and report logs generated by the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="96c19-482">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="96c19-482">For more information, see the following resources:</span></span>

* [<span data-ttu-id="96c19-483">Общие сведения об Application Insights</span><span class="sxs-lookup"><span data-stu-id="96c19-483">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* [<span data-ttu-id="96c19-484">Application Insights для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96c19-484">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* <span data-ttu-id="96c19-485">[Адаптеры ведения журналов в Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span><span class="sxs-lookup"><span data-stu-id="96c19-485">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* [<span data-ttu-id="96c19-486">Примеры реализации Application Insights ILogger</span><span class="sxs-lookup"><span data-stu-id="96c19-486">Application Insights ILogger implementation samples</span></span>](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a><span data-ttu-id="96c19-487">Сторонние поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="96c19-487">Third-party logging providers</span></span>

<span data-ttu-id="96c19-488">Некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="96c19-488">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="96c19-489">[ELMAH.IO](https://elmah.io/) ([в репозитории GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="96c19-489">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="96c19-490">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([в репозитории GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="96c19-490">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="96c19-491">[JSNLog](http://jsnlog.com/) ([в репозитории GitHub](https://github.com/mperdeck/jsnlog));</span><span class="sxs-lookup"><span data-stu-id="96c19-491">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="96c19-492">[KissLog.net](https://kisslog.net/) ([репозиторий GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="96c19-492">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="96c19-493">[Loggr](http://loggr.net/) ([в репозитории GitHub](https://github.com/imobile3/Loggr.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="96c19-493">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="96c19-494">[NLog](http://nlog-project.org/) ([в репозитории GitHub](https://github.com/NLog/NLog.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="96c19-494">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="96c19-495">[Sentry](https://sentry.io/welcome/) ([репозиторий GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="96c19-495">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="96c19-496">[Serilog](https://serilog.net/) ([в репозитории GitHub](https://github.com/serilog/serilog-extensions-logging)).</span><span class="sxs-lookup"><span data-stu-id="96c19-496">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>
* <span data-ttu-id="96c19-497">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([репозиторий Github](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="96c19-497">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="96c19-498">Некоторые сторонние платформы выполняют [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="96c19-498">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="96c19-499">Использование сторонней платформы аналогично использованию одного из встроенных поставщиков:</span><span class="sxs-lookup"><span data-stu-id="96c19-499">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="96c19-500">Добавьте пакет NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="96c19-500">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="96c19-501">Вызовите `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="96c19-501">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="96c19-502">Дополнительные сведения см. в документации по каждому поставщику.</span><span class="sxs-lookup"><span data-stu-id="96c19-502">For more information, see each provider's documentation.</span></span> <span data-ttu-id="96c19-503">Сторонние поставщики ведения журналов не поддерживаются корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="96c19-503">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96c19-504">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="96c19-504">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
