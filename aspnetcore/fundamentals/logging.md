---
title: "Ведение журнала в ASP.NET Core"
author: ardalis
description: "Дополнительные сведения о платформы ведения журналов в ASP.NET Core. Обнаружение встроенных регистраторов и Дополнительные сведения о распространенных сторонних поставщиков."
keywords: "ASP.NET Core, ведение журнала, ведение журнала providers,Microsoft.Extensions.Logging,ILogger,ILoggerFactory,LogLevel,WithFilter,TraceSource,EventLog,EventSource,scopes"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9557e9f6915507450de3ffe500582839a28c3f0c
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="8961a-105">Общие сведения о входе ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8961a-105">Introduction to Logging in ASP.NET Core</span></span>

<span data-ttu-id="8961a-106">По [Стив Смит](https://ardalis.com/) и [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8961a-106">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8961a-107">ASP.NET Core поддерживает API ведения журнала, который работает с множеством регистраторов.</span><span class="sxs-lookup"><span data-stu-id="8961a-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="8961a-108">Встроенные поставщики позволяют отправлять журналы одному или нескольким назначениям, а можно подключить платформа ведения журналов сторонних разработчиков.</span><span class="sxs-lookup"><span data-stu-id="8961a-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="8961a-109">В этой статье показано, как использовать API встроенного ведения журнала и поставщики в коде.</span><span class="sxs-lookup"><span data-stu-id="8961a-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8961a-111">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2) ([загрузке](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8961a-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8961a-113">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8961a-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="8961a-114">Создание журналов</span><span class="sxs-lookup"><span data-stu-id="8961a-114">How to create logs</span></span>

<span data-ttu-id="8961a-115">Чтобы создать журналы, получить `ILogger` объекта из [внедрения зависимостей](dependency-injection.md) контейнера:</span><span class="sxs-lookup"><span data-stu-id="8961a-115">To create logs, get an `ILogger` object from the [dependency injection](dependency-injection.md) container:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="8961a-116">Затем вызовите методы ведения журнала на этот объект средства ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="8961a-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="8961a-117">В этом примере создается журналы с `TodoController` классов как *категории*.</span><span class="sxs-lookup"><span data-stu-id="8961a-117">This example creates logs with the `TodoController` class as the *category*.</span></span>  <span data-ttu-id="8961a-118">Описываются категории [далее в этой статье](#log-category).</span><span class="sxs-lookup"><span data-stu-id="8961a-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="8961a-119">ASP.NET Core не поддерживает асинхронные методы ведения журнала так как ведение журнала должна быть настолько быстро, что это не стоит затрат на использование асинхронного.</span><span class="sxs-lookup"><span data-stu-id="8961a-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="8961a-120">Если вы в ситуации, когда, не имеет значение true, рассмотрите возможность изменения способа входа.</span><span class="sxs-lookup"><span data-stu-id="8961a-120">If you're in a situation where that's not true, consider changing the way you log.</span></span>  <span data-ttu-id="8961a-121">Если хранилище данных выполняется медленно, сначала запись сообщений журнала быстрого хранилища, а затем переместите их в медленных хранилище позже.</span><span class="sxs-lookup"><span data-stu-id="8961a-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="8961a-122">Например войдите в очередь сообщений, считываемых и сохраняются в медленных хранилище другим процессом.</span><span class="sxs-lookup"><span data-stu-id="8961a-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="8961a-123">Добавление поставщиков</span><span class="sxs-lookup"><span data-stu-id="8961a-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8961a-125">Регистратор принимает сообщения, созданные с помощью `ILogger` и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="8961a-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="8961a-126">Например поставщик консоли отображает сообщения на консоль и поставщика службы приложений Azure можно хранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="8961a-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="8961a-127">Для использования поставщика, вызывающих поставщик `Add<ProviderName>` метод расширения в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8961a-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="8961a-128">Шаблон проекта по умолчанию настраивает ведение журнала способом, вы видите в приведенном выше коде, но `ConfigureLogging` осуществляется вызов `CreateDefaultBuilder` метод.</span><span class="sxs-lookup"><span data-stu-id="8961a-128">The default project template sets up logging the way you see it in the preceding code, but the `ConfigureLogging` call is done by the `CreateDefaultBuilder` method.</span></span> <span data-ttu-id="8961a-129">Ниже приведен код *Program.cs* , создаваемый с помощью шаблонов проекта:</span><span class="sxs-lookup"><span data-stu-id="8961a-129">Here's the code in *Program.cs* that is created by project templates:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8961a-131">Регистратор принимает сообщения, созданные с помощью `ILogger` и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="8961a-131">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="8961a-132">Например поставщик консоли отображает сообщения на консоль и поставщика службы приложений Azure можно хранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="8961a-132">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="8961a-133">Для использования поставщика, установите его пакет NuGet и вызов метода расширения поставщика экземпляра `ILoggerFactory`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="8961a-133">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="8961a-134">ASP.NET Core [внедрения зависимостей](dependency-injection.md) (DI) предоставляет `ILoggerFactory` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="8961a-134">ASP.NET Core [dependency injection](dependency-injection.md) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="8961a-135">`AddConsole` И `AddDebug` методы расширения определяются в [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) пакетов.</span><span class="sxs-lookup"><span data-stu-id="8961a-135">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="8961a-136">Вызывает каждый метод расширения `ILoggerFactory.AddProvider` метод, передавая экземпляр поставщика.</span><span class="sxs-lookup"><span data-stu-id="8961a-136">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="8961a-137">Образец приложения для данной статьи добавляет регистраторов в `Configure` метод `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="8961a-137">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="8961a-138">Если вы хотите получить выходные данные журнала из кода, который выполняется в более ранних версий, добавьте регистраторов в `Startup` конструктора класса.</span><span class="sxs-lookup"><span data-stu-id="8961a-138">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="8961a-139">Вы найдете сведения о каждом [встроенные регистратора](#built-in-logging-providers) и ссылки на [сторонних регистраторов](#third-party-logging-providers) далее в статье.</span><span class="sxs-lookup"><span data-stu-id="8961a-139">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="8961a-140">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="8961a-140">Sample logging output</span></span>

<span data-ttu-id="8961a-141">Пример кода, показанный в предыдущем разделе вы увидите журналы в консоли при запуске из командной строки.</span><span class="sxs-lookup"><span data-stu-id="8961a-141">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="8961a-142">Ниже приведен пример выходных данных консоли:</span><span class="sxs-lookup"><span data-stu-id="8961a-142">Here's an example of console output:</span></span>

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
 
<span data-ttu-id="8961a-143">Были созданы эти журналы, последовательно выбрав пункты `http://localhost:5000/api/todo/0`, которое запускает выполнение обоих `ILogger` вызовы, показанный в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="8961a-143">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="8961a-144">Ниже приведен пример того же журналов, как они отображаются в окне отладки при запуске примера приложения в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8961a-144">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="8961a-145">Журналы, которые были созданы путем `ILogger` вызовы, показанный в предыдущем разделе начинаются с «TodoApi.Controllers.TodoController».</span><span class="sxs-lookup"><span data-stu-id="8961a-145">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="8961a-146">Журналы, которые начинаются с «Microsoft» категорий: от ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8961a-146">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="8961a-147">ASP.NET Core сам и код приложения используют один API ведения журнала и те же поставщики ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-147">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="8961a-148">В оставшейся части этой статьи объясняется некоторые сведения и параметры для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-148">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="8961a-149">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="8961a-149">NuGet packages</span></span>

<span data-ttu-id="8961a-150">`ILogger` И `ILoggerFactory` интерфейсы являются в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), и в реализации по умолчанию для них [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="8961a-150">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="8961a-151">Категории журналов</span><span class="sxs-lookup"><span data-stu-id="8961a-151">Log category</span></span>

<span data-ttu-id="8961a-152">Объект *категории* входит в состав каждого журнала, вы создаете.</span><span class="sxs-lookup"><span data-stu-id="8961a-152">A *category* is included with each log that you create.</span></span>  <span data-ttu-id="8961a-153">Указать категорию при создании `ILogger` объекта.</span><span class="sxs-lookup"><span data-stu-id="8961a-153">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="8961a-154">Категория может быть любой строкой, но соглашение заключается в использовании полное имя класса, из которого записываются журналы.</span><span class="sxs-lookup"><span data-stu-id="8961a-154">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span>  <span data-ttu-id="8961a-155">Например: «TodoApi.Controllers.TodoController».</span><span class="sxs-lookup"><span data-stu-id="8961a-155">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="8961a-156">Можно указать категорию, в виде строки или использовать метод расширения, который наследуется от типа категории.</span><span class="sxs-lookup"><span data-stu-id="8961a-156">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="8961a-157">Чтобы указать категории, как строку, вызовите `CreateLogger` на `ILoggerFactory` экземпляра, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="8961a-157">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="8961a-158">В большинстве случаев, он будет проще использовать `ILogger<T>`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="8961a-158">Most of the time, it will be easier to use  `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="8961a-159">Это эквивалентно вызову `CreateLogger` именем полное имя типа `T`.</span><span class="sxs-lookup"><span data-stu-id="8961a-159">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="8961a-160">Уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="8961a-160">Log level</span></span>

<span data-ttu-id="8961a-161">Каждый раз при создании журнала, указать его [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="8961a-161">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="8961a-162">Уровень ведения журнала указывает степень серьезность или важности.</span><span class="sxs-lookup"><span data-stu-id="8961a-162">The log level indicates the degree of severity or importance.</span></span>  <span data-ttu-id="8961a-163">Например, можно написать `Information` входа при завершении метода, как правило, `Warning` журнала, когда метод возвращает код возврата 404 и `Error` запись в журнал при перехватывать непредвиденное исключение.</span><span class="sxs-lookup"><span data-stu-id="8961a-163">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="8961a-164">В следующем примере кода, имена методов (например, `LogWarning`) укажите уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-164">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span>  <span data-ttu-id="8961a-165">Первый параметр — [входа событие с кодом](#log-event-id) (описано ниже в этой статье).</span><span class="sxs-lookup"><span data-stu-id="8961a-165">The first parameter is the [Log event ID](#log-event-id) (explained later in this article).</span></span>  <span data-ttu-id="8961a-166">Остальные параметры построения строки сообщения журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-166">The remaining parameters construct a log message string.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="8961a-167">Методы журнала, которые включают уровень из имени метода, [методы расширения для ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="8961a-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="8961a-168">На самом деле эти методы вызывают `Log` метода, принимающего `LogLevel` параметра.</span><span class="sxs-lookup"><span data-stu-id="8961a-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="8961a-169">Можно вызвать `Log` непосредственно, а не один из этих методов расширения, но синтаксис относительно сложен.</span><span class="sxs-lookup"><span data-stu-id="8961a-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="8961a-170">Дополнительные сведения см. в разделе [ILogger интерфейс](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) и [расширения регистратора исходный код](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="8961a-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="8961a-171">ASP.NET Core определяет следующие [уровня ведения журнала](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), упорядоченные здесь из как минимум с наибольшим уровнем серьезности.</span><span class="sxs-lookup"><span data-stu-id="8961a-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="8961a-172">Трассировка = 0</span><span class="sxs-lookup"><span data-stu-id="8961a-172">Trace = 0</span></span>

  <span data-ttu-id="8961a-173">Сведения, особенно полезны, если только для отладки проблемы разработчика.</span><span class="sxs-lookup"><span data-stu-id="8961a-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="8961a-174">Эти сообщения может содержать приложение конфиденциальные данные и поэтому не следует включать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="8961a-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="8961a-175">*По умолчанию отключено.*</span><span class="sxs-lookup"><span data-stu-id="8961a-175">*Disabled by default.*</span></span> <span data-ttu-id="8961a-176">Пример: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="8961a-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="8961a-177">Отладка = 1</span><span class="sxs-lookup"><span data-stu-id="8961a-177">Debug = 1</span></span>

  <span data-ttu-id="8961a-178">Для получения сведений с краткосрочной полезность во время разработки и отладки.</span><span class="sxs-lookup"><span data-stu-id="8961a-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="8961a-179">Пример: `Entering method Configure with flag set to true.` вы обычно не позволит `Debug` уровень журналы в рабочей среде, если нужно устранить, из-за большого объема журналов.</span><span class="sxs-lookup"><span data-stu-id="8961a-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="8961a-180">Сведения = 2</span><span class="sxs-lookup"><span data-stu-id="8961a-180">Information = 2</span></span>

  <span data-ttu-id="8961a-181">Для отслеживания общего потока приложения.</span><span class="sxs-lookup"><span data-stu-id="8961a-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="8961a-182">Эти журналы обычно имеют некоторые долговечность.</span><span class="sxs-lookup"><span data-stu-id="8961a-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="8961a-183">Пример: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="8961a-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="8961a-184">Предупреждение = 3</span><span class="sxs-lookup"><span data-stu-id="8961a-184">Warning = 3</span></span>

  <span data-ttu-id="8961a-185">Для нестандартных или непредвиденные события в потоке приложения.</span><span class="sxs-lookup"><span data-stu-id="8961a-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="8961a-186">Они могут включать ошибок или других условий, не вызывают остановку приложения, но может, который необходимо исследовать.</span><span class="sxs-lookup"><span data-stu-id="8961a-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="8961a-187">Обработанные исключения — это единый механизм для использования `Warning` уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="8961a-188">Пример: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="8961a-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="8961a-189">Ошибка = 4</span><span class="sxs-lookup"><span data-stu-id="8961a-189">Error = 4</span></span>

  <span data-ttu-id="8961a-190">Для ошибок и исключений, не может быть обработан.</span><span class="sxs-lookup"><span data-stu-id="8961a-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="8961a-191">Эти сообщения указывают сбоя текущего действия или операции (например, текущего HTTP-запроса), не произошла ошибка уровня приложения.</span><span class="sxs-lookup"><span data-stu-id="8961a-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="8961a-192">Пример сообщения журнала:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="8961a-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="8961a-193">Критические = 5</span><span class="sxs-lookup"><span data-stu-id="8961a-193">Critical = 5</span></span>

  <span data-ttu-id="8961a-194">Для сбоев, которые требуют немедленного внимания.</span><span class="sxs-lookup"><span data-stu-id="8961a-194">For failures that require immediate attention.</span></span> <span data-ttu-id="8961a-195">Примеры: потери данных, недостаточно места на диске.</span><span class="sxs-lookup"><span data-stu-id="8961a-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="8961a-196">Уровень ведения журнала можно использовать для отображения окна или управления, сколько выходные данные журнала записываются на определенный носитель.</span><span class="sxs-lookup"><span data-stu-id="8961a-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="8961a-197">Например, в рабочей среде может потребоваться все журналы `Information` уровня и сокращения, чтобы перейти на томе хранилища данных и все журналы `Warning` уровня и более поздних версий, чтобы перейти к хранилищу данных значение.</span><span class="sxs-lookup"><span data-stu-id="8961a-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="8961a-198">Во время разработки, обычно может отправить журналы `Warning` или более поздней версии серьезность на консоль.</span><span class="sxs-lookup"><span data-stu-id="8961a-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="8961a-199">Затем при необходимости для устранения неполадок, можно добавить `Debug` уровне.</span><span class="sxs-lookup"><span data-stu-id="8961a-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="8961a-200">[Фильтрации журнала](#log-filtering) далее в этой статье объясняется, как управлять какие уровни журнала, поставщик обрабатывает.</span><span class="sxs-lookup"><span data-stu-id="8961a-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="8961a-201">Платформа ASP.NET Core записывает `Debug` уровня журналы для событий framework.</span><span class="sxs-lookup"><span data-stu-id="8961a-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="8961a-202">Примеры журнала ранее в этой статье исключен журналы ниже `Information` уровне, без `Debug` были выявлены уровня журналы.</span><span class="sxs-lookup"><span data-stu-id="8961a-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="8961a-203">Ниже приведен пример журнала консоли при запуске примера приложения, настроить отображение `Debug` и выше журналах поставщика консоли.</span><span class="sxs-lookup"><span data-stu-id="8961a-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="8961a-204">Идентификатор журнала событий</span><span class="sxs-lookup"><span data-stu-id="8961a-204">Log event ID</span></span>

<span data-ttu-id="8961a-205">Каждый раз при создании журнала, можно указать *событие с кодом*.</span><span class="sxs-lookup"><span data-stu-id="8961a-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="8961a-206">Пример приложения делает это с помощью локально определенные `LoggingEvents` класса:</span><span class="sxs-lookup"><span data-stu-id="8961a-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="8961a-207">Код события — целочисленное значение, которое можно использовать, чтобы связать набор событий, зарегистрированных друг с другом.</span><span class="sxs-lookup"><span data-stu-id="8961a-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="8961a-208">Для экземпляра журнала для добавления в список покупок элемента может быть событие с кодом 1000 и журнала для завершения покупки может быть событие с кодом 1001.</span><span class="sxs-lookup"><span data-stu-id="8961a-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="8961a-209">В выходных данных ведения журнала идентификатор события может хранится в поле или сообщение содержит текст, в зависимости от поставщика.</span><span class="sxs-lookup"><span data-stu-id="8961a-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span>  <span data-ttu-id="8961a-210">Поставщик отладки не показывает коды событий, однако поставщик консоли отображается их в квадратные скобки после категории:</span><span class="sxs-lookup"><span data-stu-id="8961a-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a><span data-ttu-id="8961a-211">Строка формата сообщений журнала</span><span class="sxs-lookup"><span data-stu-id="8961a-211">Log message format string</span></span>

<span data-ttu-id="8961a-212">Каждый раз, когда записи журнала, укажите текст сообщения.</span><span class="sxs-lookup"><span data-stu-id="8961a-212">Each time you write a log, you provide a text message.</span></span> <span data-ttu-id="8961a-213">Строка сообщения может содержать именованные заполнители в аргумент, в котором размещаются значения, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8961a-213">The message string can contain named placeholders into which argument values are placed, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="8961a-214">Порядок заполнители, а не их имена, определяет, какие параметры будут использоваться для них.</span><span class="sxs-lookup"><span data-stu-id="8961a-214">The order of placeholders, not their names, determines which parameters are used for them.</span></span> <span data-ttu-id="8961a-215">Например, предположим, что имеется следующий код:</span><span class="sxs-lookup"><span data-stu-id="8961a-215">For example, if you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="8961a-216">Полученное в результате сообщение журнала будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8961a-216">The resulting log message would look like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="8961a-217">Платформа ведения журнала сообщений форматирование таким образом, чтобы сделать возможным для поставщиков ведения журнала для реализации [семантической ведения журналов, также известные как структурированный ведения журнала](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="8961a-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="8961a-218">Так как сами аргументы передаются в систему ведения журналов не только строковое сообщение, отформатированное, регистраторов можно хранить значения параметров, как поля, кроме строку сообщения.</span><span class="sxs-lookup"><span data-stu-id="8961a-218">Because the arguments themselves are passed to the logging system, not just the formatted message string, logging providers can store the parameter values as fields in addition to the message string.</span></span> <span data-ttu-id="8961a-219">Например если Направляемая выходные данные журналов для табличного хранилища Azure и вызова метода средство ведения журнала выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8961a-219">For example, if you are directing your log output to Azure Table Storage, and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="8961a-220">Каждая сущность таблицы Azure может иметь `ID` и `RequestTime` свойства, упрощающими запросы на данные журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-220">Each Azure Table entity could have `ID` and `RequestTime` properties, which would simplify queries on log data.</span></span> <span data-ttu-id="8961a-221">Может найти все журналы в пределах определенного `RequestTime` диапазона, без необходимости анализировать времени ожидания текстового сообщения.</span><span class="sxs-lookup"><span data-stu-id="8961a-221">You could find all logs within a particular `RequestTime` range, without having to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="8961a-222">Ведение журнала исключений</span><span class="sxs-lookup"><span data-stu-id="8961a-222">Logging exceptions</span></span>

<span data-ttu-id="8961a-223">Методы ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8961a-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="8961a-224">Различные поставщики обрабатывать сведения об исключении по-разному.</span><span class="sxs-lookup"><span data-stu-id="8961a-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="8961a-225">Ниже приведен пример выходных данных отладки поставщика из приведенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="8961a-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="8961a-226">Фильтрацию журнала</span><span class="sxs-lookup"><span data-stu-id="8961a-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8961a-228">Можно указать минимальный уровень для определенного поставщика и категории, или для всех поставщиков или всех категориях.</span><span class="sxs-lookup"><span data-stu-id="8961a-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span>  <span data-ttu-id="8961a-229">Все журналы с минимальным размером не передачей с этим поставщиком, поэтому они не получить отображаются или записываются.</span><span class="sxs-lookup"><span data-stu-id="8961a-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="8961a-230">Если вы хотите запретить все журналы, можно указать `LogLevel.None` как уровень минимального ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="8961a-231">Целочисленное значение `LogLevel.None` — 6, то есть выше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="8961a-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="8961a-232">**Создание правила фильтрации в конфигурации**</span><span class="sxs-lookup"><span data-stu-id="8961a-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="8961a-233">Шаблоны проектов создают код, который вызывает `CreateDefaultBuilder` настройке ведения журнала для поставщиков консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="8961a-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="8961a-234">`CreateDefaultBuilder` Метод также устанавливает ведения журнала для поиска конфигурации в `Logging` статьи, используя код, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="8961a-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="8961a-235">Данные конфигурации указывает уровни минимальное журнала поставщиком и категории, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8961a-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](logging/sample2/appsettings.json)]

<span data-ttu-id="8961a-236">Этот JSON создает шесть правила фильтрации, один для отладки поставщика, четырех для поставщика консоли и которая применяется ко всем поставщикам.</span><span class="sxs-lookup"><span data-stu-id="8961a-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="8961a-237">Вы увидите, как одну из этих правил, выбранного для каждого поставщика при `ILogger` создан объект.</span><span class="sxs-lookup"><span data-stu-id="8961a-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="8961a-238">**Правила фильтрации в коде**</span><span class="sxs-lookup"><span data-stu-id="8961a-238">**Filter rules in code**</span></span>

<span data-ttu-id="8961a-239">Правила фильтрации можно зарегистрировать в коде, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8961a-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="8961a-240">Второй `AddFilter` указывает поставщика, отладки, используя имя его типа.</span><span class="sxs-lookup"><span data-stu-id="8961a-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="8961a-241">Первый `AddFilter` применяется для всех поставщиков, так как он не определяет тип поставщика.</span><span class="sxs-lookup"><span data-stu-id="8961a-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="8961a-242">**Как правило фильтрации применяются**</span><span class="sxs-lookup"><span data-stu-id="8961a-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="8961a-243">Данные конфигурации и `AddFilter` код, приведенный в предыдущих примерах создавать правила, показаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="8961a-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="8961a-244">Первые шесть извлекаются из примера конфигурации и последние две берутся из примера кода.</span><span class="sxs-lookup"><span data-stu-id="8961a-244">The first six come from the configuration example and the last two come from the code example.</span></span>

<span data-ttu-id="8961a-245">Число</span><span class="sxs-lookup"><span data-stu-id="8961a-245">Number</span></span>|<span data-ttu-id="8961a-246">Поставщик</span><span class="sxs-lookup"><span data-stu-id="8961a-246">Provider</span></span>|<span data-ttu-id="8961a-247">Категории, которые начинаются с</span><span class="sxs-lookup"><span data-stu-id="8961a-247">Categories that begin with</span></span>|<span data-ttu-id="8961a-248">Уровень минимального ведения журнала</span><span class="sxs-lookup"><span data-stu-id="8961a-248">Minimum log level</span></span>|
------|--------|--------------------------|-----------------|
<span data-ttu-id="8961a-249">1</span><span class="sxs-lookup"><span data-stu-id="8961a-249">1</span></span>|<span data-ttu-id="8961a-250">Отладка</span><span class="sxs-lookup"><span data-stu-id="8961a-250">Debug</span></span>|<span data-ttu-id="8961a-251">Все категории</span><span class="sxs-lookup"><span data-stu-id="8961a-251">All categories</span></span>|<span data-ttu-id="8961a-252">Сведения</span><span class="sxs-lookup"><span data-stu-id="8961a-252">Information</span></span>|
<span data-ttu-id="8961a-253">2</span><span class="sxs-lookup"><span data-stu-id="8961a-253">2</span></span>|<span data-ttu-id="8961a-254">Консоль</span><span class="sxs-lookup"><span data-stu-id="8961a-254">Console</span></span>|<span data-ttu-id="8961a-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="8961a-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span>|<span data-ttu-id="8961a-256">Предупреждение</span><span class="sxs-lookup"><span data-stu-id="8961a-256">Warning</span></span>|
<span data-ttu-id="8961a-257">3</span><span class="sxs-lookup"><span data-stu-id="8961a-257">3</span></span>|<span data-ttu-id="8961a-258">Консоль</span><span class="sxs-lookup"><span data-stu-id="8961a-258">Console</span></span>|<span data-ttu-id="8961a-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="8961a-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>|<span data-ttu-id="8961a-260">Отладка</span><span class="sxs-lookup"><span data-stu-id="8961a-260">Debug</span></span>|
<span data-ttu-id="8961a-261">4</span><span class="sxs-lookup"><span data-stu-id="8961a-261">4</span></span>|<span data-ttu-id="8961a-262">Консоль</span><span class="sxs-lookup"><span data-stu-id="8961a-262">Console</span></span>|<span data-ttu-id="8961a-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="8961a-263">Microsoft.AspNetCore.Mvc.Razor</span></span>|<span data-ttu-id="8961a-264">Ошибка</span><span class="sxs-lookup"><span data-stu-id="8961a-264">Error</span></span>|
<span data-ttu-id="8961a-265">5</span><span class="sxs-lookup"><span data-stu-id="8961a-265">5</span></span>|<span data-ttu-id="8961a-266">Консоль</span><span class="sxs-lookup"><span data-stu-id="8961a-266">Console</span></span>|<span data-ttu-id="8961a-267">Все категории</span><span class="sxs-lookup"><span data-stu-id="8961a-267">All categories</span></span>|<span data-ttu-id="8961a-268">Сведения</span><span class="sxs-lookup"><span data-stu-id="8961a-268">Information</span></span>|
<span data-ttu-id="8961a-269">6</span><span class="sxs-lookup"><span data-stu-id="8961a-269">6</span></span>|<span data-ttu-id="8961a-270">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="8961a-270">All providers</span></span>|<span data-ttu-id="8961a-271">Все категории</span><span class="sxs-lookup"><span data-stu-id="8961a-271">All categories</span></span>|<span data-ttu-id="8961a-272">Отладка</span><span class="sxs-lookup"><span data-stu-id="8961a-272">Debug</span></span>
<span data-ttu-id="8961a-273">7</span><span class="sxs-lookup"><span data-stu-id="8961a-273">7</span></span>|<span data-ttu-id="8961a-274">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="8961a-274">All providers</span></span>|<span data-ttu-id="8961a-275">Система</span><span class="sxs-lookup"><span data-stu-id="8961a-275">System</span></span>|<span data-ttu-id="8961a-276">Отладка</span><span class="sxs-lookup"><span data-stu-id="8961a-276">Debug</span></span>
<span data-ttu-id="8961a-277">8</span><span class="sxs-lookup"><span data-stu-id="8961a-277">8</span></span>|<span data-ttu-id="8961a-278">Отладка</span><span class="sxs-lookup"><span data-stu-id="8961a-278">Debug</span></span>|<span data-ttu-id="8961a-279">Майкрософт</span><span class="sxs-lookup"><span data-stu-id="8961a-279">Microsoft</span></span>|<span data-ttu-id="8961a-280">Трассировка</span><span class="sxs-lookup"><span data-stu-id="8961a-280">Trace</span></span>

<span data-ttu-id="8961a-281">При создании `ILogger` объект для записи журналов, `ILoggerFactory` выбирает одно правило каждого поставщика, чтобы применить для этого средства ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="8961a-282">Все сообщения записываются, `ILogger` объект фильтруются на основе выбранных правил.</span><span class="sxs-lookup"><span data-stu-id="8961a-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="8961a-283">Наиболее определенное правило, возможных для каждого поставщика и категории пары выбираются из доступных правил.</span><span class="sxs-lookup"><span data-stu-id="8961a-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="8961a-284">Следующий запрошенный алгоритм используется для каждого поставщика при `ILogger` создается для данной категории:</span><span class="sxs-lookup"><span data-stu-id="8961a-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="8961a-285">Выберите все правила, которые соответствуют поставщика или его псевдоним.</span><span class="sxs-lookup"><span data-stu-id="8961a-285">Select all rules that match the provider or its alias.</span></span>  <span data-ttu-id="8961a-286">Если ничего не найдено, выберите все правила с поставщиком пустым.</span><span class="sxs-lookup"><span data-stu-id="8961a-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="8961a-287">В результатах предыдущего шага выберите правила, которые с наиболее длительным временем соответствующий префикс категории.</span><span class="sxs-lookup"><span data-stu-id="8961a-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="8961a-288">Если ничего не найдено, выберите все правила, которые не указать категорию.</span><span class="sxs-lookup"><span data-stu-id="8961a-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="8961a-289">Если выбрано несколько правил занять **последний** один.</span><span class="sxs-lookup"><span data-stu-id="8961a-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="8961a-290">Если правила не выбраны, используйте `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="8961a-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="8961a-291">Предположим, у вас есть вышеуказанных правил и создании `ILogger` объекта для категории «Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine»:</span><span class="sxs-lookup"><span data-stu-id="8961a-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="8961a-292">Для отладки поставщика применяются правила 1, 6 и 8.</span><span class="sxs-lookup"><span data-stu-id="8961a-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="8961a-293">Правило 8 является наиболее подходящим, то есть выбранный.</span><span class="sxs-lookup"><span data-stu-id="8961a-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="8961a-294">Для поставщика консоли применяются следующие правила, 3, 4, 5 и 6.</span><span class="sxs-lookup"><span data-stu-id="8961a-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="8961a-295">Правило 3 не является наиболее подходящим.</span><span class="sxs-lookup"><span data-stu-id="8961a-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="8961a-296">При создании журналов с помощью `ILogger` для категории «Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine» журналы из `Trace` уровне и более поздних версий будет отправлена отладки поставщика и журналы `Debug` уровня и выше перейдет к поставщику консоли.</span><span class="sxs-lookup"><span data-stu-id="8961a-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="8961a-297">**Поставщик псевдонимов**</span><span class="sxs-lookup"><span data-stu-id="8961a-297">**Provider aliases**</span></span>

<span data-ttu-id="8961a-298">Имя типа можно использовать, чтобы указать поставщика в конфигурации, но каждый поставщик определяет более коротким *псевдоним* проще в использовании.</span><span class="sxs-lookup"><span data-stu-id="8961a-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="8961a-299">Для встроенных поставщиков используйте следующие псевдонимы:</span><span class="sxs-lookup"><span data-stu-id="8961a-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="8961a-300">Консоль</span><span class="sxs-lookup"><span data-stu-id="8961a-300">Console</span></span>
- <span data-ttu-id="8961a-301">Отладка</span><span class="sxs-lookup"><span data-stu-id="8961a-301">Debug</span></span>
- <span data-ttu-id="8961a-302">Журнал событий</span><span class="sxs-lookup"><span data-stu-id="8961a-302">EventLog</span></span>
- <span data-ttu-id="8961a-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="8961a-303">AzureAppServices</span></span>
- <span data-ttu-id="8961a-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="8961a-304">TraceSource</span></span>
- <span data-ttu-id="8961a-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="8961a-305">EventSource</span></span>

<span data-ttu-id="8961a-306">**Минимальный уровень по умолчанию**</span><span class="sxs-lookup"><span data-stu-id="8961a-306">**Default minimum level**</span></span>

<span data-ttu-id="8961a-307">Нет минимальный уровень, вступает в силу только в том случае, если правила из конфигурации или кода не применяются для данного поставщика и категории.</span><span class="sxs-lookup"><span data-stu-id="8961a-307">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="8961a-308">В следующем примере показано, как задать минимальный уровень:</span><span class="sxs-lookup"><span data-stu-id="8961a-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="8961a-309">Если минимальный уровень не задано явно, значение по умолчанию — `Information`, означающее, что `Trace` и `Debug` журналы учитываются.</span><span class="sxs-lookup"><span data-stu-id="8961a-309">IF you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="8961a-310">**Функции фильтров**</span><span class="sxs-lookup"><span data-stu-id="8961a-310">**Filter functions**</span></span>

<span data-ttu-id="8961a-311">Можно написать код в функцию фильтрации для применения правил фильтрации.</span><span class="sxs-lookup"><span data-stu-id="8961a-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="8961a-312">Функция фильтрации вызывается для всех поставщиков и категорий, которые не имел правил, назначенные им по конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="8961a-312">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="8961a-313">Код в функции имеет доступ к тип поставщика, категорию и уровень ведения журнала, чтобы определить, будут ли занесены сообщения.</span><span class="sxs-lookup"><span data-stu-id="8961a-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="8961a-314">Пример:</span><span class="sxs-lookup"><span data-stu-id="8961a-314">For example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8961a-316">Некоторые поставщики ведения журнала позволяют указать при журналы должны записываются на носителе или обрабатывается на основе уровень ведения журнала и категории.</span><span class="sxs-lookup"><span data-stu-id="8961a-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="8961a-317">`AddConsole` И `AddDebug` методы расширения предоставляют перегрузки, которые позволяют передавать в критериям фильтрации.</span><span class="sxs-lookup"><span data-stu-id="8961a-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="8961a-318">В следующем примере кода вызывает службу консоли, чтобы игнорировать журналы ниже `Warning` уровня, во время отладки поставщик не учитывает журналы, которые платформа создает.</span><span class="sxs-lookup"><span data-stu-id="8961a-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="8961a-319">`AddEventLog` Имеет перегрузку, которая принимает `EventLogSettings` экземпляр, который может содержать функции фильтрации в его `Filter` свойство.</span><span class="sxs-lookup"><span data-stu-id="8961a-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="8961a-320">TraceSource поставщик не предоставляет никаких из этих перегрузок с момента его уровень ведения журнала, а также другие параметры основаны на `SourceSwitch` и `TraceListener` его использует.</span><span class="sxs-lookup"><span data-stu-id="8961a-320">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the  `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="8961a-321">Можно задать правила фильтрации для всех поставщиков, которые зарегистрированы с `ILoggerFactory` экземпляра с помощью `WithFilter` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="8961a-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="8961a-322">В приведенном ниже примере ограничивает журналы framework (категория начинается с «Microsoft» или «Система»), чтобы предупреждения при этом разрешив журнала отладки на уровне приложения.</span><span class="sxs-lookup"><span data-stu-id="8961a-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="8961a-323">Если вы хотите использовать фильтрацию, чтобы предотвратить запись для определенной категории все журналы, можно указать `LogLevel.None` как уровень минимального ведения журнала для этой категории.</span><span class="sxs-lookup"><span data-stu-id="8961a-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="8961a-324">Целочисленное значение `LogLevel.None` — 6, то есть выше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="8961a-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="8961a-325">`WithFilter` Предоставляется метод расширения [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="8961a-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="8961a-326">Метод возвращает новую `ILoggerFactory` экземпляра, чтобы отфильтровать журнал сообщения, передаваемые для всех поставщиков средства ведения журнала, зарегистрированных в нем.</span><span class="sxs-lookup"><span data-stu-id="8961a-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="8961a-327">Он не влияет на любой другой `ILoggerFactory` экземплярам, включая исходные `ILoggerFactory` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="8961a-327">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="8961a-328">Областей журнала</span><span class="sxs-lookup"><span data-stu-id="8961a-328">Log scopes</span></span>

<span data-ttu-id="8961a-329">Можно сгруппировать набор логических операций в *область* для присоединения те же данные для каждого журнала, созданные в рамках этого набора.</span><span class="sxs-lookup"><span data-stu-id="8961a-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span>  <span data-ttu-id="8961a-330">Например может потребоваться каждого журнала, созданного в ходе обработки транзакций включать идентификатор транзакции.</span><span class="sxs-lookup"><span data-stu-id="8961a-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="8961a-331">Область — `IDisposable` тип, возвращаемый `ILogger.BeginScope<TState>` метод и остается включенным до их освобождения.</span><span class="sxs-lookup"><span data-stu-id="8961a-331">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="8961a-332">Использовать область, если ее заключить в средство ведения журнала вызовов в `using` блока, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="8961a-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="8961a-333">Следующий пример кода позволяет областей для поставщика консоли:</span><span class="sxs-lookup"><span data-stu-id="8961a-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8961a-335">В *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8961a-335">In *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-336">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-336">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8961a-337">В *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8961a-337">In *Startup.cs*:</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="8961a-338">Каждое сообщение журнала включает информации:</span><span class="sxs-lookup"><span data-stu-id="8961a-338">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="8961a-339">Встроенные регистраторов.</span><span class="sxs-lookup"><span data-stu-id="8961a-339">Built-in logging providers</span></span>

<span data-ttu-id="8961a-340">ASP.NET Core поставляется следующих поставщиков:</span><span class="sxs-lookup"><span data-stu-id="8961a-340">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="8961a-341">Консоль</span><span class="sxs-lookup"><span data-stu-id="8961a-341">Console</span></span>](#console)
* [<span data-ttu-id="8961a-342">Отладка</span><span class="sxs-lookup"><span data-stu-id="8961a-342">Debug</span></span>](#debug)
* [<span data-ttu-id="8961a-343">EventSource</span><span class="sxs-lookup"><span data-stu-id="8961a-343">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="8961a-344">EventLog</span><span class="sxs-lookup"><span data-stu-id="8961a-344">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="8961a-345">TraceSource</span><span class="sxs-lookup"><span data-stu-id="8961a-345">TraceSource</span></span>](#tracesource)
* <span data-ttu-id="8961a-346">[служба приложений Azure](#appservice);</span><span class="sxs-lookup"><span data-stu-id="8961a-346">[Azure App Service](#appservice)</span></span>

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="8961a-347">Поставщик консоли</span><span class="sxs-lookup"><span data-stu-id="8961a-347">The console provider</span></span>

<span data-ttu-id="8961a-348">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) пакета поставщик отправляет выходные данные журнала на консоль.</span><span class="sxs-lookup"><span data-stu-id="8961a-348">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-350">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="8961a-351">[Перегрузки AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) позволяют передать минимальный уровень, функция фильтрации и логическое значение, указывающее, поддерживаются ли области.</span><span class="sxs-lookup"><span data-stu-id="8961a-351">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span>  <span data-ttu-id="8961a-352">Другим вариантом является передача `IConfiguration` объект, который можно указать поддержку областей и уровней ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-352">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="8961a-353">Если вы собираетесь консоль поставщик для использования в рабочей среде, имейте в виду, что он имеет значительное влияние на производительность.</span><span class="sxs-lookup"><span data-stu-id="8961a-353">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="8961a-354">При создании нового проекта в Visual Studio `AddConsole` метод выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8961a-354">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="8961a-355">Этот код ссылается на `Logging` раздел *appSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="8961a-355">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](logging/sample//appsettings.json)]

<span data-ttu-id="8961a-356">Параметры, отображаемые предел framework журналы для предупреждений, позволяет приложению для входа на уровне отладки, как описано в статье [фильтрации журнала](#log-filtering) раздела.</span><span class="sxs-lookup"><span data-stu-id="8961a-356">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="8961a-357">Дополнительные сведения см. в разделе [Конфигурация](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="8961a-357">For more information, see [Configuration](configuration.md).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="8961a-358">Отладка поставщика</span><span class="sxs-lookup"><span data-stu-id="8961a-358">The Debug provider</span></span>

<span data-ttu-id="8961a-359">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) пакет поставщика записывает выходные данные журналов с помощью [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) класса (`Debug.WriteLine` вызовы методов).</span><span class="sxs-lookup"><span data-stu-id="8961a-359">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="8961a-360">В Linux, этот поставщик записывает журналы на */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="8961a-360">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="8961a-363">[Перегрузки AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) позволяют передать минимальный уровень или функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="8961a-363">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="8961a-364">Поставщик EventSource</span><span class="sxs-lookup"><span data-stu-id="8961a-364">The EventSource provider</span></span>

<span data-ttu-id="8961a-365">Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) пакет поставщика могут реализовывать события трассировки.</span><span class="sxs-lookup"><span data-stu-id="8961a-365">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="8961a-366">В Windows, он использует [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="8961a-366">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="8961a-367">Поставщик является кросс платформенных, но есть событие не сбора и отображения программ еще для Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="8961a-367">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-368">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-368">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-369">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-369">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="8961a-370">Для сбора и просмотра журналов рекомендуется использовать [PerfView программа](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="8961a-370">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="8961a-371">Существуют другие средства просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, генерируемой ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8961a-371">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="8961a-372">Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` для **дополнительные поставщики** списка.</span><span class="sxs-lookup"><span data-stu-id="8961a-372">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="8961a-373">(Не пропустите звездочки в начале строки.)</span><span class="sxs-lookup"><span data-stu-id="8961a-373">(Don't miss the asterisk at the start of the string.)</span></span>

![Дополнительные поставщики Perfview](logging/_static/perfview-additional-providers.png)

<span data-ttu-id="8961a-375">Захват событий на сервере Nano Server требует дополнительной настройки:</span><span class="sxs-lookup"><span data-stu-id="8961a-375">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="8961a-376">Подключение удаленного взаимодействия PowerShell в Nano Server:</span><span class="sxs-lookup"><span data-stu-id="8961a-376">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="8961a-377">Создание сеанса трассировки событий Windows:</span><span class="sxs-lookup"><span data-stu-id="8961a-377">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="8961a-378">Добавьте поставщиков трассировки событий Windows для [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core и другим пользователям при необходимости.</span><span class="sxs-lookup"><span data-stu-id="8961a-378">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="8961a-379">Идентификатор GUID поставщика ASP.NET Core `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="8961a-379">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="8961a-380">Запустить узел и выполните нужные действия требуется получить сведения трассировки.</span><span class="sxs-lookup"><span data-stu-id="8961a-380">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="8961a-381">Остановите сеанс трассировки после окончания:</span><span class="sxs-lookup"><span data-stu-id="8961a-381">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="8961a-382">Итоговый *C:\trace.etl* файла могут быть проанализированы с PerfView, как и в других выпусках Windows.</span><span class="sxs-lookup"><span data-stu-id="8961a-382">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="8961a-383">Поставщик журнала событий Windows</span><span class="sxs-lookup"><span data-stu-id="8961a-383">The Windows EventLog provider</span></span>

<span data-ttu-id="8961a-384">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) пакета поставщик отправляет выходные данные журнала в журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="8961a-384">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-385">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-385">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-386">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-386">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="8961a-387">[Перегрузки AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) позволяют передать `EventLogSettings` или минимальный уровень.</span><span class="sxs-lookup"><span data-stu-id="8961a-387">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="8961a-388">Поставщик TraceSource</span><span class="sxs-lookup"><span data-stu-id="8961a-388">The TraceSource provider</span></span>

<span data-ttu-id="8961a-389">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) пакет поставщик использует [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) библиотеки и поставщики.</span><span class="sxs-lookup"><span data-stu-id="8961a-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-390">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-390">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-391">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-391">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="8961a-392">[Перегрузки AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) позволяют передать переключатель источника и прослушиватель трассировки.</span><span class="sxs-lookup"><span data-stu-id="8961a-392">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="8961a-393">Чтобы использовать этот поставщик, приложения должен выполняться на .NET Framework (а не .NET Core).</span><span class="sxs-lookup"><span data-stu-id="8961a-393">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="8961a-394">Поставщик позволяет, маршрутизации сообщений на различные [прослушиватели](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), такие как [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) используется в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="8961a-394">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="8961a-395">В следующем примере настраивается `TraceSource` поставщик, записывающий в журнал `Warning` и более поздней версии сообщения в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="8961a-395">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="8961a-396">Поставщик службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="8961a-396">The Azure App Service provider</span></span>

<span data-ttu-id="8961a-397">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) пакет поставщика записи журналов в текстовые файлы в файловой системе приложения службы приложений Azure и [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="8961a-397">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="8961a-398">Поставщик является доступной только для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="8961a-398">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8961a-399">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8961a-399">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

> [!NOTE]
> <span data-ttu-id="8961a-400">ASP.NET 2.0 лежит в режиме предварительного просмотра.</span><span class="sxs-lookup"><span data-stu-id="8961a-400">ASP.NET Core 2.0 is in preview.</span></span>  <span data-ttu-id="8961a-401">Приложениям, созданным с помощью последнего предварительного выпуска может не работать при развертывании в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="8961a-401">Apps created with the latest preview release may not run when deployed to Azure App Service.</span></span> <span data-ttu-id="8961a-402">При выпуске ASP.NET 2.0 основной службе приложений Azure будет выполняться 2.0 приложения и службы приложений Azure, поставщик будет работать, как указано ниже.</span><span class="sxs-lookup"><span data-stu-id="8961a-402">When ASP.NET Core 2.0 is released, Azure App Service will run 2.0 apps, and the Azure App Service provider will work as indicated here.</span></span>

<span data-ttu-id="8961a-403">Не нужно устанавливать пакет поставщика или вызов `AddAzureWebAppDiagnostics` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="8961a-403">You don't have to install the provider package or call the `AddAzureWebAppDiagnostics` extension method.</span></span>  <span data-ttu-id="8961a-404">Поставщик является автоматически доступны для приложения при развертывании приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="8961a-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8961a-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8961a-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="8961a-406">`AddAzureWebAppDiagnostics` Перегрузка позволяет передавать в [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), с помощью которого можно переопределить параметры по умолчанию, такие как ведение журнала выходных данных шаблона, имя BLOB-объекта и максимального размера файла.</span><span class="sxs-lookup"><span data-stu-id="8961a-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="8961a-407">(*Выходные данные шаблона* является строка формата сообщений, применяется для всех журналов на основе того, который вы указали при вызове `ILogger` метода.)</span><span class="sxs-lookup"><span data-stu-id="8961a-407">(*Output template* is a message format string that is applied to all logs, on top of the one that you provide when you call an `ILogger` method.)</span></span>  

---

<span data-ttu-id="8961a-408">При развертывании приложения службы приложений, приложение учитывает параметры в [журналы диагностики](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) раздел **службы приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="8961a-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="8961a-409">При изменении этих параметров, изменения установки вступили в силу немедленно, без необходимости перезапустить приложение или повторно развернуть код, чтобы он.</span><span class="sxs-lookup"><span data-stu-id="8961a-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Параметры ведения журнала Azure](logging/_static/azure-logging-settings.png)

<span data-ttu-id="8961a-411">По умолчанию для файлов журнала находится в *D:\\домашней\\LogFiles\\приложения* папку и имя файла по умолчанию — *yyyymmdd.txt диагностики*.</span><span class="sxs-lookup"><span data-stu-id="8961a-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="8961a-412">Максимальный размер файла по умолчанию составляет 10 МБ, а по умолчанию файлы сохраняются не более 2.</span><span class="sxs-lookup"><span data-stu-id="8961a-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="8961a-413">Имя BLOB-объектов по умолчанию *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="8961a-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="8961a-414">Дополнительные сведения о поведении по умолчанию см. в разделе [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="8961a-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="8961a-415">Поставщик работает только при запуске проекта в среде Azure.</span><span class="sxs-lookup"><span data-stu-id="8961a-415">The provider only works when your project runs in the Azure environment.</span></span>  <span data-ttu-id="8961a-416">Не оказывает никакого эффекта при локальном запуске &mdash; не записываются в локальных файлах или в хранилище локального развертывания для больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="8961a-416">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="8961a-417">Ведение журнала сторонних поставщиков</span><span class="sxs-lookup"><span data-stu-id="8961a-417">Third-party logging providers</span></span>

<span data-ttu-id="8961a-418">Ниже приведены некоторые платформ ведения журнала сторонних разработчиков, которые работают с ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8961a-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="8961a-419">[ELMAH.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -поставщика для службы Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="8961a-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="8961a-420">[JSNLog](http://jsnlog.com) -регистрирует в журнале серверные исключения JavaScript и других событий на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="8961a-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="8961a-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -поставщика для службы Loggr</span><span class="sxs-lookup"><span data-stu-id="8961a-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="8961a-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) -поставщик для библиотеки NLog</span><span class="sxs-lookup"><span data-stu-id="8961a-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="8961a-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) -поставщик для библиотеки Serilog</span><span class="sxs-lookup"><span data-stu-id="8961a-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="8961a-424">Некоторые сторонние платформы можно сделать [семантической ведения журналов, также известные как структурированный ведения журнала](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="8961a-424">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="8961a-425">С использованием сторонней платформы похоже на использование одного из встроенных поставщиков: добавьте пакет NuGet в проект и вызов метода расширения на `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="8961a-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="8961a-426">Дополнительные сведения см. в документации каждого framework.</span><span class="sxs-lookup"><span data-stu-id="8961a-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="8961a-427">Собственных поставщиков также можно создать для поддержки других платформ ведения журналов или требований ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8961a-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>
