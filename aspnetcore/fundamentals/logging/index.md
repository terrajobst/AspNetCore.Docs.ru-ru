---
title: "Ведение журналов в ASP.NET Core"
author: ardalis
description: "Сведения о платформе ведения журналов в ASP.NET Core. Ознакомьтесь со встроенными поставщиками ведения журналов и получите подробные сведения о распространенных сторонних поставщиках."
keywords: "ASP.NET Core, ведение журналов, поставщики ведения журналов, Microsoft.Extensions.Logging, ILogger, ILoggerFactory, LogLevel, WithFilter, TraceSource, EventLog, EventSource, области"
ms.author: tdykstra
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/index
ms.openlocfilehash: 3eb167c961b8d089d508ef5622db6ae1cdd99088
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="4c23a-105">Общие сведения о ведении журналов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c23a-105">Introduction to logging in ASP.NET Core</span></span>

<span data-ttu-id="4c23a-106">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="4c23a-106">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4c23a-107">ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="4c23a-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="4c23a-108">Встроенные поставщики позволяют отправлять журналы в одно или несколько мест назначений. Можно подключить стороннюю платформу ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="4c23a-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="4c23a-109">В этой статье содержатся сведения об использовании в коде встроенного API и поставщиков ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="4c23a-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4c23a-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4c23a-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4c23a-113">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4c23a-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="4c23a-114">Создание журналов</span><span class="sxs-lookup"><span data-stu-id="4c23a-114">How to create logs</span></span>

<span data-ttu-id="4c23a-115">Чтобы создать журналы, получите объект `ILogger` из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="4c23a-115">To create logs, get an `ILogger` object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="4c23a-116">Затем в этом объекте средства ведения журнала вызовите методы ведения журналов:</span><span class="sxs-lookup"><span data-stu-id="4c23a-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="4c23a-117">В этом примере создаются журналы с классом `TodoController` в качестве *категории*.</span><span class="sxs-lookup"><span data-stu-id="4c23a-117">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="4c23a-118">Категории рассматриваются [далее в этой статье](#log-category).</span><span class="sxs-lookup"><span data-stu-id="4c23a-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="4c23a-119">ASP.NET Core не предоставляет асинхронные методы ведения журналов, так как ведение журналов должно выполняться настолько быстро, что использовать асинхронный метод нецелесообразно.</span><span class="sxs-lookup"><span data-stu-id="4c23a-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="4c23a-120">Если возникла обратная ситуация, рекомендуется рассмотреть возможность изменения способа ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="4c23a-120">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="4c23a-121">Если хранилище данных работает медленно, сначала записывайте сообщения журнала в быстродействующее хранилище, а затем перемещайте их в медленное хранилище.</span><span class="sxs-lookup"><span data-stu-id="4c23a-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="4c23a-122">Например, выполняйте запись в очередь сообщений, которые считываются и сохраняются в медленном хранилище другим процессом.</span><span class="sxs-lookup"><span data-stu-id="4c23a-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="4c23a-123">Добавление поставщиков</span><span class="sxs-lookup"><span data-stu-id="4c23a-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4c23a-125">Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="4c23a-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="4c23a-126">Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="4c23a-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="4c23a-127">Для использования поставщика вызовите метод расширения `Add<ProviderName>` поставщика в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c23a-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="4c23a-128">Шаблон проекта по умолчанию включает ведение журнала с помощью метода[CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___).</span><span class="sxs-lookup"><span data-stu-id="4c23a-128">The default project template enables logging with the [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4c23a-130">Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="4c23a-130">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="4c23a-131">Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="4c23a-131">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="4c23a-132">Чтобы использовать поставщик, установите его пакет NuGet и вызовите метод расширения поставщика в экземпляре `ILoggerFactory`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4c23a-132">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="4c23a-133">[Внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) ASP.NET Core предоставляет экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-133">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="4c23a-134">Методы расширения `AddConsole` и `AddDebug` определены в пакетах [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="4c23a-134">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="4c23a-135">Каждый метод расширения вызывает метод `ILoggerFactory.AddProvider`, передавая экземпляр поставщика.</span><span class="sxs-lookup"><span data-stu-id="4c23a-135">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="4c23a-136">В примере приложения в этой статье поставщики ведения журналов добавляются в метод `Configure` класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-136">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="4c23a-137">Чтобы получить выходные данные журнала из выполняемого ранее кода, добавьте поставщики ведения журналов в конструктор класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-137">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="4c23a-138">Сведения о каждом [встроенном поставщике ведения журнала](#built-in-logging-providers) и ссылки на [сторонние поставщики](#third-party-logging-providers) приведены далее в статье.</span><span class="sxs-lookup"><span data-stu-id="4c23a-138">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="4c23a-139">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="4c23a-139">Sample logging output</span></span>

<span data-ttu-id="4c23a-140">Используя пример кода из предыдущего раздела, вы увидите журналы в консоли после запуска из командной строки.</span><span class="sxs-lookup"><span data-stu-id="4c23a-140">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="4c23a-141">Ниже приведен пример выходных данных консоли:</span><span class="sxs-lookup"><span data-stu-id="4c23a-141">Here's an example of console output:</span></span>

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
 
<span data-ttu-id="4c23a-142">Эти журналы были созданы при переходе по адресу `http://localhost:5000/api/todo/0`, который запускает выполнение обоих вызовов `ILogger`, приведенных в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="4c23a-142">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="4c23a-143">Ниже приведен пример тех же журналов в том виде, как они отображаются в окне отладки при запуске примера приложения в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4c23a-143">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="4c23a-144">Журналы, которые были созданы путем вызовов `ILogger`, приведенных в предыдущем разделе, начинаются с "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="4c23a-144">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="4c23a-145">Журналы, которые начинаются с категорий "Microsoft", входят в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c23a-145">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="4c23a-146">В ASP.NET Core и коде приложения используется один и тот же API ведения журнала и одни и те же поставщики.</span><span class="sxs-lookup"><span data-stu-id="4c23a-146">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="4c23a-147">В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="4c23a-147">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="4c23a-148">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="4c23a-148">NuGet packages</span></span>

<span data-ttu-id="4c23a-149">Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="4c23a-149">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="4c23a-150">Категория журнала</span><span class="sxs-lookup"><span data-stu-id="4c23a-150">Log category</span></span>

<span data-ttu-id="4c23a-151">*Категория* входит в состав каждого создаваемого журнала.</span><span class="sxs-lookup"><span data-stu-id="4c23a-151">A *category* is included with each log that you create.</span></span> <span data-ttu-id="4c23a-152">Категория указывается при создании объекта `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-152">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="4c23a-153">Категория может быть любой строкой, однако действует соглашение по использованию полного имени класса, из которого записываются журналы.</span><span class="sxs-lookup"><span data-stu-id="4c23a-153">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="4c23a-154">Например: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="4c23a-154">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="4c23a-155">Можно указать категорию в виде строки или использовать метод расширения, который наследует категорию от типа.</span><span class="sxs-lookup"><span data-stu-id="4c23a-155">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="4c23a-156">Чтобы указать категорию как строку, вызовите `CreateLogger` в экземпляре `ILoggerFactory`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="4c23a-156">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="4c23a-157">В большинстве случаев будет проще использовать `ILogger<T>`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4c23a-157">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="4c23a-158">Это эквивалентно вызову `CreateLogger` с полным именем типа `T`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-158">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="4c23a-159">Уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="4c23a-159">Log level</span></span>

<span data-ttu-id="4c23a-160">Каждый раз при создании журнала указывается его значение перечисления [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="4c23a-160">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="4c23a-161">Уровень ведения журнала означает степень серьезности или важности.</span><span class="sxs-lookup"><span data-stu-id="4c23a-161">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="4c23a-162">Например, можно создать журнал `Information` при нормальном завершении метода, журнал `Warning`, если метод возвращает код 404, и журнал `Error` при перехвате непредвиденного исключения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-162">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="4c23a-163">В следующем примере кода имена методов (например, `LogWarning`) указывают уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="4c23a-163">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="4c23a-164">Первый параметр — это [ идентификатор события журнала](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="4c23a-164">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="4c23a-165">Второй параметр — это [шаблон сообщения](#log-message-template) с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода.</span><span class="sxs-lookup"><span data-stu-id="4c23a-165">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="4c23a-166">Параметры метода рассматриваются более подробно далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="4c23a-166">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="4c23a-167">Методы ведения журнала, которые содержат уровень в имени метода, являются [методами расширения для ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="4c23a-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="4c23a-168">На самом деле эти методы вызывают метод `Log`, принимающий параметр `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="4c23a-169">Метод `Log`, в отличие от этих методов расширения, можно вызывать напрямую, однако его синтаксис довольно сложен.</span><span class="sxs-lookup"><span data-stu-id="4c23a-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="4c23a-170">Дополнительные сведения см. в разделе [Интерфейс ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) и [Исходный код расширений средства ведения журналов](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="4c23a-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="4c23a-171">В ASP.NET Core определяются следующие [уровни ведения журнала](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), упорядоченные в этом документе от наименьшего до наибольшего уровня серьезности.</span><span class="sxs-lookup"><span data-stu-id="4c23a-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="4c23a-172">Трассировка = 0</span><span class="sxs-lookup"><span data-stu-id="4c23a-172">Trace = 0</span></span>

  <span data-ttu-id="4c23a-173">Для сведений, которые полезны, только если разработчик выполняет отладку проблемы.</span><span class="sxs-lookup"><span data-stu-id="4c23a-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="4c23a-174">Эти сообщения могут содержать конфиденциальные данные приложения, поэтому их не следует включать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="4c23a-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="4c23a-175">*По умолчанию отключено.*</span><span class="sxs-lookup"><span data-stu-id="4c23a-175">*Disabled by default.*</span></span> <span data-ttu-id="4c23a-176">Пример: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="4c23a-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="4c23a-177">Отладка = 1</span><span class="sxs-lookup"><span data-stu-id="4c23a-177">Debug = 1</span></span>

  <span data-ttu-id="4c23a-178">Для сведений, которые полезны в краткосрочной перспективе во время разработки и отладки.</span><span class="sxs-lookup"><span data-stu-id="4c23a-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="4c23a-179">Пример: `Entering method Configure with flag set to true.` как правило, журналы уровня `Debug` включаются в рабочей среде только при устранении неполадок.</span><span class="sxs-lookup"><span data-stu-id="4c23a-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="4c23a-180">Информация = 2</span><span class="sxs-lookup"><span data-stu-id="4c23a-180">Information = 2</span></span>

  <span data-ttu-id="4c23a-181">Для отслеживания общего хода выполнения приложения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="4c23a-182">Эти журналы обычно полезны в долгосрочной перспективе.</span><span class="sxs-lookup"><span data-stu-id="4c23a-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="4c23a-183">Пример: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="4c23a-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="4c23a-184">Предупреждение = 3</span><span class="sxs-lookup"><span data-stu-id="4c23a-184">Warning = 3</span></span>

  <span data-ttu-id="4c23a-185">Для нестандартных или непредвиденных событий, возникающих при выполнении приложения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="4c23a-186">В их число могут входить ошибки или другие условия, которые не приводят к остановке приложения, но которые может потребоваться изучить.</span><span class="sxs-lookup"><span data-stu-id="4c23a-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="4c23a-187">Обработанные исключения являются распространенным условием использования уровня ведения журнала `Warning`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="4c23a-188">Пример: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="4c23a-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="4c23a-189">Ошибка = 4</span><span class="sxs-lookup"><span data-stu-id="4c23a-189">Error = 4</span></span>

  <span data-ttu-id="4c23a-190">Для ошибок и исключений, которые не могут быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="4c23a-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="4c23a-191">Эти сообщения указывают на сбой текущего действия или операции (например, текущего HTTP-запроса), а не ошибку уровня приложения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="4c23a-192">Пример сообщения журнала: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="4c23a-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="4c23a-193">Критический = 5</span><span class="sxs-lookup"><span data-stu-id="4c23a-193">Critical = 5</span></span>

  <span data-ttu-id="4c23a-194">Для сбоев, которые требуют немедленного внимания.</span><span class="sxs-lookup"><span data-stu-id="4c23a-194">For failures that require immediate attention.</span></span> <span data-ttu-id="4c23a-195">Примеры: потеря данных, нехватка места на диске.</span><span class="sxs-lookup"><span data-stu-id="4c23a-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="4c23a-196">Уровень ведения журнала можно использовать для управления объемом выходных данных, записываемых на определенный носитель или в окно отображения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="4c23a-197">Например, в рабочей среде может потребоваться, чтобы все журналы уровня `Information` и ниже записывались в хранилище томов, а все журналы уровня `Warning` и выше записывались в хранилище значений.</span><span class="sxs-lookup"><span data-stu-id="4c23a-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="4c23a-198">Во время разработки можно отправлять журналы `Warning` или более высоких уровней серьезности на консоль.</span><span class="sxs-lookup"><span data-stu-id="4c23a-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="4c23a-199">Затем, если потребуется устранить неполадки, можно добавить уровень `Debug`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="4c23a-200">В разделе [Фильтрация журналов](#log-filtering) далее в этой статье приводятся сведения о том, как контролировать уровни журнала, обрабатываемые поставщиком.</span><span class="sxs-lookup"><span data-stu-id="4c23a-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="4c23a-201">Платформа ASP.NET Core создает журналы уровня `Debug` для событий платформы.</span><span class="sxs-lookup"><span data-stu-id="4c23a-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="4c23a-202">В примерах журнала ранее в этой статье не указывались журналы с уровнем ниже `Information`, поэтому журналы уровня `Debug` не отображались.</span><span class="sxs-lookup"><span data-stu-id="4c23a-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="4c23a-203">Ниже приведен пример журналов консоли при запуске примера приложения, настроенного для отображения журналов уровня `Debug` и выше для поставщика Console.</span><span class="sxs-lookup"><span data-stu-id="4c23a-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="4c23a-204">Идентификатор события журнала</span><span class="sxs-lookup"><span data-stu-id="4c23a-204">Log event ID</span></span>

<span data-ttu-id="4c23a-205">При каждом создании журнала можно указывать *идентификатор события*.</span><span class="sxs-lookup"><span data-stu-id="4c23a-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="4c23a-206">В примере приложения для этого используется локально определенный класс `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="4c23a-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="4c23a-207">Идентификатор события — это целочисленное значение, используемое для связи набора зарегистрированных событий друг с другом.</span><span class="sxs-lookup"><span data-stu-id="4c23a-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="4c23a-208">Например, журнал для добавления позиции в корзину покупок может иметь идентификатор события 1000, а журнал для завершения покупки может иметь идентификатор события 1001.</span><span class="sxs-lookup"><span data-stu-id="4c23a-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="4c23a-209">В зависимости от поставщика идентификатор события может быть сохранен в поле или включен в текст сообщения в выходных данных журнала.</span><span class="sxs-lookup"><span data-stu-id="4c23a-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="4c23a-210">Поставщик Debug не отображает идентификаторы событий, но поставщик Console показывает их в квадратных скобках после категории:</span><span class="sxs-lookup"><span data-stu-id="4c23a-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="4c23a-211">Шаблон сообщения журнала</span><span class="sxs-lookup"><span data-stu-id="4c23a-211">Log message template</span></span>

<span data-ttu-id="4c23a-212">Каждый раз при записи сообщения журнала предоставляется шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-212">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="4c23a-213">Шаблон сообщения может быть строкой либо содержать именованные заполнители, в которые помещаются значения аргументов.</span><span class="sxs-lookup"><span data-stu-id="4c23a-213">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="4c23a-214">Шаблон не является строкой формата, а заполнители должны быть именованными, а не нумерованными.</span><span class="sxs-lookup"><span data-stu-id="4c23a-214">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="4c23a-215">Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами.</span><span class="sxs-lookup"><span data-stu-id="4c23a-215">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="4c23a-216">Предположим, что имеется следующий код:</span><span class="sxs-lookup"><span data-stu-id="4c23a-216">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="4c23a-217">Полученное в результате сообщение журнала выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4c23a-217">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="4c23a-218">Платформа ведения журнала поддерживает такое форматирование сообщений, чтобы поставщики ведения журналов могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журнала](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="4c23a-218">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="4c23a-219">Так как в систему ведения журналов передаются сами аргументы, а не только отформатированный шаблон сообщения, поставщики ведения журналов могут сохранять значения параметров как поля (наряду с шаблоном сообщения).</span><span class="sxs-lookup"><span data-stu-id="4c23a-219">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="4c23a-220">Если выходные данных журналов направляются в хранилище таблиц Azure, вызов метода средства ведения журнала выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4c23a-220">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="4c23a-221">Каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журнала.</span><span class="sxs-lookup"><span data-stu-id="4c23a-221">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="4c23a-222">Можно находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-222">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="4c23a-223">Ведение журнала исключений</span><span class="sxs-lookup"><span data-stu-id="4c23a-223">Logging exceptions</span></span>

<span data-ttu-id="4c23a-224">Методы средства ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="4c23a-224">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="4c23a-225">Разные поставщики обрабатывают сведения об исключениях по-разному.</span><span class="sxs-lookup"><span data-stu-id="4c23a-225">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="4c23a-226">Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="4c23a-226">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="4c23a-227">Фильтрация журналов</span><span class="sxs-lookup"><span data-stu-id="4c23a-227">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4c23a-229">Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий.</span><span class="sxs-lookup"><span data-stu-id="4c23a-229">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="4c23a-230">Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются.</span><span class="sxs-lookup"><span data-stu-id="4c23a-230">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="4c23a-231">Чтобы запретить все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="4c23a-231">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="4c23a-232">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="4c23a-232">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="4c23a-233">**Создание правил фильтрации в конфигурации**</span><span class="sxs-lookup"><span data-stu-id="4c23a-233">**Create filter rules in configuration**</span></span>

<span data-ttu-id="4c23a-234">Шаблоны проектов создают код, который вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug.</span><span class="sxs-lookup"><span data-stu-id="4c23a-234">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="4c23a-235">Метод `CreateDefaultBuilder` также настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, используя код, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="4c23a-235">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="4c23a-236">Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="4c23a-236">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="4c23a-237">Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно, которое применяется ко всем поставщикам.</span><span class="sxs-lookup"><span data-stu-id="4c23a-237">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="4c23a-238">Далее вы увидите, как при создании объекта `ILogger` для каждого поставщика выбирается лишь одно из этих правил.</span><span class="sxs-lookup"><span data-stu-id="4c23a-238">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="4c23a-239">**Правила фильтрации в коде**</span><span class="sxs-lookup"><span data-stu-id="4c23a-239">**Filter rules in code**</span></span>

<span data-ttu-id="4c23a-240">Можно зарегистрировать правила фильтрации в коде, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="4c23a-240">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="4c23a-241">Второй `AddFilter` указывает поставщика, Debug, используя имя его типа.</span><span class="sxs-lookup"><span data-stu-id="4c23a-241">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="4c23a-242">Первый `AddFilter` применяется ко всем поставщикам, так как он не определяет тип поставщика.</span><span class="sxs-lookup"><span data-stu-id="4c23a-242">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="4c23a-243">**Применение правил фильтрации**</span><span class="sxs-lookup"><span data-stu-id="4c23a-243">**How filtering rules are applied**</span></span>

<span data-ttu-id="4c23a-244">Данные конфигурации и код `AddFilter`, приведенный в предыдущих примерах, создают правила, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="4c23a-244">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="4c23a-245">Первые шесть взяты из примера конфигурации, а последние два — из примера кода.</span><span class="sxs-lookup"><span data-stu-id="4c23a-245">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="4c23a-246">Число</span><span class="sxs-lookup"><span data-stu-id="4c23a-246">Number</span></span> | <span data-ttu-id="4c23a-247">Поставщик</span><span class="sxs-lookup"><span data-stu-id="4c23a-247">Provider</span></span>      | <span data-ttu-id="4c23a-248">Категории, которые начинаются с...</span><span class="sxs-lookup"><span data-stu-id="4c23a-248">Categories that begin with ...</span></span>          | <span data-ttu-id="4c23a-249">Минимальный уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="4c23a-249">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="4c23a-250">1</span><span class="sxs-lookup"><span data-stu-id="4c23a-250">1</span></span>      | <span data-ttu-id="4c23a-251">Отладка</span><span class="sxs-lookup"><span data-stu-id="4c23a-251">Debug</span></span>         | <span data-ttu-id="4c23a-252">Все категории</span><span class="sxs-lookup"><span data-stu-id="4c23a-252">All categories</span></span>                          | <span data-ttu-id="4c23a-253">Сведения</span><span class="sxs-lookup"><span data-stu-id="4c23a-253">Information</span></span>       |
| <span data-ttu-id="4c23a-254">2</span><span class="sxs-lookup"><span data-stu-id="4c23a-254">2</span></span>      | <span data-ttu-id="4c23a-255">Консоль</span><span class="sxs-lookup"><span data-stu-id="4c23a-255">Console</span></span>       | <span data-ttu-id="4c23a-256">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="4c23a-256">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="4c23a-257">Предупреждение</span><span class="sxs-lookup"><span data-stu-id="4c23a-257">Warning</span></span>           |
| <span data-ttu-id="4c23a-258">3</span><span class="sxs-lookup"><span data-stu-id="4c23a-258">3</span></span>      | <span data-ttu-id="4c23a-259">Консоль</span><span class="sxs-lookup"><span data-stu-id="4c23a-259">Console</span></span>       | <span data-ttu-id="4c23a-260">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="4c23a-260">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="4c23a-261">Отладка</span><span class="sxs-lookup"><span data-stu-id="4c23a-261">Debug</span></span>             |
| <span data-ttu-id="4c23a-262">4</span><span class="sxs-lookup"><span data-stu-id="4c23a-262">4</span></span>      | <span data-ttu-id="4c23a-263">Консоль</span><span class="sxs-lookup"><span data-stu-id="4c23a-263">Console</span></span>       | <span data-ttu-id="4c23a-264">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="4c23a-264">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="4c23a-265">Error</span><span class="sxs-lookup"><span data-stu-id="4c23a-265">Error</span></span>             |
| <span data-ttu-id="4c23a-266">5</span><span class="sxs-lookup"><span data-stu-id="4c23a-266">5</span></span>      | <span data-ttu-id="4c23a-267">Консоль</span><span class="sxs-lookup"><span data-stu-id="4c23a-267">Console</span></span>       | <span data-ttu-id="4c23a-268">Все категории</span><span class="sxs-lookup"><span data-stu-id="4c23a-268">All categories</span></span>                          | <span data-ttu-id="4c23a-269">Сведения</span><span class="sxs-lookup"><span data-stu-id="4c23a-269">Information</span></span>       |
| <span data-ttu-id="4c23a-270">6</span><span class="sxs-lookup"><span data-stu-id="4c23a-270">6</span></span>      | <span data-ttu-id="4c23a-271">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="4c23a-271">All providers</span></span> | <span data-ttu-id="4c23a-272">Все категории</span><span class="sxs-lookup"><span data-stu-id="4c23a-272">All categories</span></span>                          | <span data-ttu-id="4c23a-273">Отладка</span><span class="sxs-lookup"><span data-stu-id="4c23a-273">Debug</span></span>             |
| <span data-ttu-id="4c23a-274">7</span><span class="sxs-lookup"><span data-stu-id="4c23a-274">7</span></span>      | <span data-ttu-id="4c23a-275">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="4c23a-275">All providers</span></span> | <span data-ttu-id="4c23a-276">Система</span><span class="sxs-lookup"><span data-stu-id="4c23a-276">System</span></span>                                  | <span data-ttu-id="4c23a-277">Отладка</span><span class="sxs-lookup"><span data-stu-id="4c23a-277">Debug</span></span>             |
| <span data-ttu-id="4c23a-278">8</span><span class="sxs-lookup"><span data-stu-id="4c23a-278">8</span></span>      | <span data-ttu-id="4c23a-279">Отладка</span><span class="sxs-lookup"><span data-stu-id="4c23a-279">Debug</span></span>         | <span data-ttu-id="4c23a-280">Майкрософт</span><span class="sxs-lookup"><span data-stu-id="4c23a-280">Microsoft</span></span>                               | <span data-ttu-id="4c23a-281">Трассировка</span><span class="sxs-lookup"><span data-stu-id="4c23a-281">Trace</span></span>             |

<span data-ttu-id="4c23a-282">При создании объекта `ILogger`, используемого для создания журналов, объект `ILoggerFactory` выбирает одно правило для каждого поставщика, которое будет применено к этому средству ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="4c23a-282">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="4c23a-283">Все сообщения, записываемые с помощью этого объекта `ILogger`, фильтруются на основе выбранных правил.</span><span class="sxs-lookup"><span data-stu-id="4c23a-283">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="4c23a-284">Самое подходящее правило для каждой пары поставщика и категории выбирается из списка доступных правил.</span><span class="sxs-lookup"><span data-stu-id="4c23a-284">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="4c23a-285">При создании `ILogger` для данной категории для каждого поставщика используется приведенный далее алгоритм:</span><span class="sxs-lookup"><span data-stu-id="4c23a-285">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="4c23a-286">Выберите все правила, которые соответствуют поставщику или его псевдониму.</span><span class="sxs-lookup"><span data-stu-id="4c23a-286">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="4c23a-287">Если ничего не найдено, выберите все правила с пустым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="4c23a-287">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="4c23a-288">В результатах предыдущего шага выберите правила с самым длинным соответствующим префиксом категории.</span><span class="sxs-lookup"><span data-stu-id="4c23a-288">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="4c23a-289">Если ничего не найдено, выберите все правила, которые не указывают категорию.</span><span class="sxs-lookup"><span data-stu-id="4c23a-289">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="4c23a-290">Если выбрано несколько правил, примите **последнее**.</span><span class="sxs-lookup"><span data-stu-id="4c23a-290">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="4c23a-291">Если правила не выбраны, используйте `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-291">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="4c23a-292">Предположим, что у вас есть указанный выше список правил и вы создаете объект `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="4c23a-292">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="4c23a-293">К поставщику Debug применяются правила 1, 6 и 8.</span><span class="sxs-lookup"><span data-stu-id="4c23a-293">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="4c23a-294">Правило 8 является наиболее подходящим, поэтому выбрано оно.</span><span class="sxs-lookup"><span data-stu-id="4c23a-294">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="4c23a-295">К поставщику отладки применяются правила 3, 4, 5 и 6.</span><span class="sxs-lookup"><span data-stu-id="4c23a-295">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="4c23a-296">Правило 3 является наиболее подходящим.</span><span class="sxs-lookup"><span data-stu-id="4c23a-296">Rule 3 is most specific.</span></span>

<span data-ttu-id="4c23a-297">При создании журналов с помощью `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" журналы уровня `Trace` и выше перейдут поставщику отладки, а журналы уровня `Debug` и выше перейдут поставщику консоли.</span><span class="sxs-lookup"><span data-stu-id="4c23a-297">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="4c23a-298">**Псевдонимы поставщиков**</span><span class="sxs-lookup"><span data-stu-id="4c23a-298">**Provider aliases**</span></span>

<span data-ttu-id="4c23a-299">Имя типа можно использовать, чтобы указать поставщик в конфигурации, но каждый поставщик определяет более короткий и простой в использовании *псевдоним*.</span><span class="sxs-lookup"><span data-stu-id="4c23a-299">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="4c23a-300">Для встроенных поставщиков используйте следующие псевдонимы:</span><span class="sxs-lookup"><span data-stu-id="4c23a-300">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="4c23a-301">Консоль</span><span class="sxs-lookup"><span data-stu-id="4c23a-301">Console</span></span>
- <span data-ttu-id="4c23a-302">Отладка</span><span class="sxs-lookup"><span data-stu-id="4c23a-302">Debug</span></span>
- <span data-ttu-id="4c23a-303">EventLog</span><span class="sxs-lookup"><span data-stu-id="4c23a-303">EventLog</span></span>
- <span data-ttu-id="4c23a-304">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="4c23a-304">AzureAppServices</span></span>
- <span data-ttu-id="4c23a-305">TraceSource</span><span class="sxs-lookup"><span data-stu-id="4c23a-305">TraceSource</span></span>
- <span data-ttu-id="4c23a-306">EventSource</span><span class="sxs-lookup"><span data-stu-id="4c23a-306">EventSource</span></span>

<span data-ttu-id="4c23a-307">**Минимальный уровень по умолчанию**</span><span class="sxs-lookup"><span data-stu-id="4c23a-307">**Default minimum level**</span></span>

<span data-ttu-id="4c23a-308">Существует параметр минимального уровня, который вступает в силу только в том случае, если к данному поставщику или категории не применяются никакие правила из конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="4c23a-308">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="4c23a-309">В следующем примере показано, как задать минимальный уровень:</span><span class="sxs-lookup"><span data-stu-id="4c23a-309">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="4c23a-310">Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.</span><span class="sxs-lookup"><span data-stu-id="4c23a-310">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="4c23a-311">**Функции фильтрации**</span><span class="sxs-lookup"><span data-stu-id="4c23a-311">**Filter functions**</span></span>

<span data-ttu-id="4c23a-312">В функции фильтрации можно написать код для применения правил фильтрации.</span><span class="sxs-lookup"><span data-stu-id="4c23a-312">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="4c23a-313">Функция фильтрации вызывается для всех поставщиков и категорий, которым не назначены правила из конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="4c23a-313">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="4c23a-314">Код в функции имеет доступ к типу поставщика, категории и уровню ведения журнала, чтобы определить необходимость регистрации сообщений.</span><span class="sxs-lookup"><span data-stu-id="4c23a-314">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="4c23a-315">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c23a-315">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4c23a-317">Некоторые поставщики ведения журналов позволяют указывать, когда журналы следует записывать на носитель, а когда — игнорировать. При этом учитывается уровень ведения журнала и категория.</span><span class="sxs-lookup"><span data-stu-id="4c23a-317">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="4c23a-318">Методы расширения `AddConsole` и `AddDebug`предоставляют перегрузки для передачи условий фильтрации.</span><span class="sxs-lookup"><span data-stu-id="4c23a-318">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="4c23a-319">В следующем примере кода поставщик Console игнорирует журналы с уровнем ниже `Warning`, а поставщик Debug не учитывает журналы, создаваемые платформой.</span><span class="sxs-lookup"><span data-stu-id="4c23a-319">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="4c23a-320">Метод `AddEventLog` имеет перегрузку, принимающую экземпляр `EventLogSettings`, который в своем свойстве `Filter` может содержать функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="4c23a-320">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="4c23a-321">Поставщик TraceSource не предоставляет ни одну из этих перегрузок, так как его уровень ведения журнала и другие параметры основаны на используемых им `SourceSwitch` и `TraceListener`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-321">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="4c23a-322">Можно задать правила фильтрации для всех поставщиков, которые зарегистрированы в экземпляре `ILoggerFactory`, с помощью метода расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-322">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="4c23a-323">В приведенном ниже примере журналы платформы (категория начинается с "Microsoft" или "System") ограничиваются предупреждениями, однако разрешаются журналы приложений на уровне отладки.</span><span class="sxs-lookup"><span data-stu-id="4c23a-323">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="4c23a-324">Чтобы использовать фильтрацию для предотвращения создания всех журналов для определенной категории, укажите `LogLevel.None` в качестве минимального уровня ведения журнала для этой категории.</span><span class="sxs-lookup"><span data-stu-id="4c23a-324">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="4c23a-325">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="4c23a-325">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="4c23a-326">Пакет NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) предоставляет метод расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-326">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="4c23a-327">Метод возвращает новый экземпляр `ILoggerFactory` для фильтрации журнала сообщений, переданного всем поставщикам средства ведения журналов, которые зарегистрированы в этом экземпляре.</span><span class="sxs-lookup"><span data-stu-id="4c23a-327">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="4c23a-328">Он не влияет на другие экземпляры `ILoggerFactory`, включая исходный экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-328">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="4c23a-329">Области журналов</span><span class="sxs-lookup"><span data-stu-id="4c23a-329">Log scopes</span></span>

<span data-ttu-id="4c23a-330">Для присоединения одних и тех же данных к каждому журналу, созданному в рамках набора, можно сгруппировать набор логических операций в *область*.</span><span class="sxs-lookup"><span data-stu-id="4c23a-330">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span> <span data-ttu-id="4c23a-331">Например, может потребоваться, чтобы каждый журнал, созданный в ходе обработки транзакции, содержал идентификатор транзакции.</span><span class="sxs-lookup"><span data-stu-id="4c23a-331">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="4c23a-332">Область — это тип `IDisposable`, возвращаемый методом `ILogger.BeginScope<TState>` и действующий до окончательного уничтожения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-332">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="4c23a-333">Чтобы использовать область, следует заключить вызовы средства ведения журналов в блок `using`, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="4c23a-333">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="4c23a-334">Следующий код предоставляет области для поставщика Console:</span><span class="sxs-lookup"><span data-stu-id="4c23a-334">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-335">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-335">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4c23a-336">В файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c23a-336">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="4c23a-337">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="4c23a-337">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="4c23a-338">Настройка `IncludeScopes` с помощью файлов конфигурации *appsettings* будет доступна в выпуске ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="4c23a-338">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-339">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-339">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4c23a-340">В файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c23a-340">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="4c23a-341">Каждое сообщение журнала содержит ограниченную информацию:</span><span class="sxs-lookup"><span data-stu-id="4c23a-341">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="4c23a-342">Встроенные поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="4c23a-342">Built-in logging providers</span></span>

<span data-ttu-id="4c23a-343">В состав ASP.NET Core входят следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="4c23a-343">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="4c23a-344">Консоль</span><span class="sxs-lookup"><span data-stu-id="4c23a-344">Console</span></span>](#console)
* [<span data-ttu-id="4c23a-345">Отладка</span><span class="sxs-lookup"><span data-stu-id="4c23a-345">Debug</span></span>](#debug)
* [<span data-ttu-id="4c23a-346">EventSource</span><span class="sxs-lookup"><span data-stu-id="4c23a-346">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="4c23a-347">EventLog</span><span class="sxs-lookup"><span data-stu-id="4c23a-347">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="4c23a-348">TraceSource</span><span class="sxs-lookup"><span data-stu-id="4c23a-348">TraceSource</span></span>](#tracesource)
* <span data-ttu-id="4c23a-349">[служба приложений Azure](#appservice);</span><span class="sxs-lookup"><span data-stu-id="4c23a-349">[Azure App Service](#appservice)</span></span>

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="4c23a-350">Поставщик Console</span><span class="sxs-lookup"><span data-stu-id="4c23a-350">The console provider</span></span>

<span data-ttu-id="4c23a-351">Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль.</span><span class="sxs-lookup"><span data-stu-id="4c23a-351">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-352">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-352">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-353">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="4c23a-354">[Перегрузки AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) позволяют передавать минимальный уровень ведения журнала, функцию фильтрации и логическое значение, указывающее, поддерживаются ли области.</span><span class="sxs-lookup"><span data-stu-id="4c23a-354">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="4c23a-355">Другим вариантом является передача объекта `IConfiguration`, который может указать поддержку областей и уровни ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="4c23a-355">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="4c23a-356">Если вы собираетесь использовать поставщик Console в рабочей среде, имейте в виду, что он оказывает значительное влияние на производительность.</span><span class="sxs-lookup"><span data-stu-id="4c23a-356">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="4c23a-357">При создании проекта в Visual Studio метод `AddConsole` выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4c23a-357">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="4c23a-358">Этот код ссылается на раздел `Logging` файла *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="4c23a-358">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="4c23a-359">Приведенные параметры ограничивают журналы платформы предупреждениями, но разрешают регистрацию приложений на уровне отладки, как описано в разделе [Фильтрация журналов](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="4c23a-359">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="4c23a-360">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="4c23a-360">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="4c23a-361">Поставщик Debug</span><span class="sxs-lookup"><span data-stu-id="4c23a-361">The Debug provider</span></span>

<span data-ttu-id="4c23a-362">Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) (вызовов метода `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="4c23a-362">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="4c23a-363">В Linux этот поставщик записывает журналы в каталог */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="4c23a-363">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-364">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-364">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-365">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-365">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="4c23a-366">[Перегрузки AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) позволяют передавать минимальный уровень ведения журнала или функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="4c23a-366">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="4c23a-367">Поставщик EventSource</span><span class="sxs-lookup"><span data-stu-id="4c23a-367">The EventSource provider</span></span>

<span data-ttu-id="4c23a-368">Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздних версий, реализовывать события трассировки можно с помощью пакета поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource).</span><span class="sxs-lookup"><span data-stu-id="4c23a-368">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="4c23a-369">В Windows используется [трассировка событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="4c23a-369">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="4c23a-370">Поставщик является кроссплатформенным, но для Linux и macOS инструменты сбора событий и отображений пока отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="4c23a-370">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="4c23a-373">Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="4c23a-373">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="4c23a-374">Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4c23a-374">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="4c23a-375">Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**.</span><span class="sxs-lookup"><span data-stu-id="4c23a-375">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="4c23a-376">(Не пропустите звездочку в начале строки.)</span><span class="sxs-lookup"><span data-stu-id="4c23a-376">(Don't miss the asterisk at the start of the string.)</span></span>

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

<span data-ttu-id="4c23a-378">Для записи событий на сервере Nano Server следует выполнить дополнительную настройку:</span><span class="sxs-lookup"><span data-stu-id="4c23a-378">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="4c23a-379">Подключите PowerShell для удаленной работы к Nano Server:</span><span class="sxs-lookup"><span data-stu-id="4c23a-379">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="4c23a-380">Создайте сеанс трассировки событий Windows:</span><span class="sxs-lookup"><span data-stu-id="4c23a-380">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="4c23a-381">Добавьте поставщики трассировки событий Windows для [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core и другие, если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="4c23a-381">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="4c23a-382">Поставщик ASP.NET Core имеет GUID `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-382">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="4c23a-383">Запустите сайт и выполните нужные действия, для которых требуется получить сведения трассировки.</span><span class="sxs-lookup"><span data-stu-id="4c23a-383">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="4c23a-384">Остановите сеанс трассировки после окончания сбора данных:</span><span class="sxs-lookup"><span data-stu-id="4c23a-384">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="4c23a-385">Итоговый файл *C:\trace.etl* можно проанализировать в PerfView и других выпусках Windows.</span><span class="sxs-lookup"><span data-stu-id="4c23a-385">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="4c23a-386">Поставщик Windows EventLog</span><span class="sxs-lookup"><span data-stu-id="4c23a-386">The Windows EventLog provider</span></span>

<span data-ttu-id="4c23a-387">Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="4c23a-387">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-388">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-388">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-389">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-389">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="4c23a-390">[Перегрузки AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) позволяют передавать `EventLogSettings` или минимальный уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="4c23a-390">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="4c23a-391">Поставщик TraceSource</span><span class="sxs-lookup"><span data-stu-id="4c23a-391">The TraceSource provider</span></span>

<span data-ttu-id="4c23a-392">Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="4c23a-392">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-393">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-393">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-394">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-394">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="4c23a-395">[Перегрузки AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) позволяют передавать исходный параметр и прослушиватель трассировки.</span><span class="sxs-lookup"><span data-stu-id="4c23a-395">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="4c23a-396">Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core).</span><span class="sxs-lookup"><span data-stu-id="4c23a-396">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="4c23a-397">Поставщик позволяет перенаправлять сообщения различным [прослушивателям](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), например [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr), который используется в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-397">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="4c23a-398">В следующем примере показана настройка поставщика `TraceSource`, записывающего в окне консоли сообщения типа `Warning` и сообщения с более высоким уровнем серьезности.</span><span class="sxs-lookup"><span data-stu-id="4c23a-398">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="4c23a-399">Поставщик службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="4c23a-399">The Azure App Service provider</span></span>

<span data-ttu-id="4c23a-400">Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure.</span><span class="sxs-lookup"><span data-stu-id="4c23a-400">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="4c23a-401">Поставщик доступен только для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="4c23a-401">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4c23a-402">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-402">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4c23a-403">Если планируется использовать .NET Core, не нужно устанавливать пакет поставщика или явным образом вызвать `AddAzureWebAppDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-403">If targeting .NET Core, you don't have to install the provider package or explicitly call `AddAzureWebAppDiagnostics`.</span></span> <span data-ttu-id="4c23a-404">Поставщик становится автоматически доступным для приложения при развертывании приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="4c23a-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

<span data-ttu-id="4c23a-405">Если планируется использовать .NET Framework, добавьте пакет поставщика в проект и вызовите `AddAzureWebAppDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-405">If targeting .NET Framework, add the provider package to your project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4c23a-406">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4c23a-406">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="4c23a-407">Перегрузка `AddAzureWebAppDiagnostics` позволяет передавать [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) для переопределения настроек по умолчанию, таких как шаблон выходных данных ведения журнала, имя BLOB-объекта и максимально допустимый размер файла.</span><span class="sxs-lookup"><span data-stu-id="4c23a-407">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="4c23a-408">(*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам на основе того, который вы указали при вызове метода `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="4c23a-408">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="4c23a-409">При развертывании в приложение службы приложений ваше приложение учитывает параметры в разделе [Журналы диагностики](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="4c23a-409">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="4c23a-410">Изменения этих параметров вступают в силу сразу же после внесения и не требуют перезапуска приложения или повторного развертывания в нем кода.</span><span class="sxs-lookup"><span data-stu-id="4c23a-410">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Параметры ведения журнала Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="4c23a-412">По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="4c23a-412">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="4c23a-413">Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2.</span><span class="sxs-lookup"><span data-stu-id="4c23a-413">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="4c23a-414">Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="4c23a-414">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="4c23a-415">Дополнительные сведения о поведении по умолчанию см. в разделе [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="4c23a-415">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="4c23a-416">Поставщик работает только при выполнении проекта в среде Azure.</span><span class="sxs-lookup"><span data-stu-id="4c23a-416">The provider only works when your project runs in the Azure environment.</span></span> <span data-ttu-id="4c23a-417">Он не функционирует при запуске в локальной среде &mdash;; запись в локальные файлы или в локальное хранилище развертывания для больших двоичных объектов не производится.</span><span class="sxs-lookup"><span data-stu-id="4c23a-417">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="4c23a-418">Сторонние поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="4c23a-418">Third-party logging providers</span></span>

<span data-ttu-id="4c23a-419">Ниже приведены некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4c23a-419">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="4c23a-420">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) — поставщик для службы Elmah.Io.</span><span class="sxs-lookup"><span data-stu-id="4c23a-420">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="4c23a-421">[JSNLog](http://jsnlog.com) — регистрирует в серверном журнале исключения JavaScript и другие клиентские события.</span><span class="sxs-lookup"><span data-stu-id="4c23a-421">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="4c23a-422">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) — поставщик для службы Loggr.</span><span class="sxs-lookup"><span data-stu-id="4c23a-422">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="4c23a-423">[NLog](https://github.com/NLog/NLog.Extensions.Logging) — поставщик для библиотеки NLog.</span><span class="sxs-lookup"><span data-stu-id="4c23a-423">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="4c23a-424">[Serilog](https://github.com/serilog/serilog-extensions-logging) — поставщик для библиотеки Serilog.</span><span class="sxs-lookup"><span data-stu-id="4c23a-424">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="4c23a-425">Некоторые сторонние платформы поддерживают [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="4c23a-425">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="4c23a-426">Использование сторонней платформы аналогично использованию одного из встроенных поставщиков: добавьте пакет NuGet в проект и вызовите метод расширения в `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-426">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="4c23a-427">Дополнительные сведения см. в документации по каждой платформе.</span><span class="sxs-lookup"><span data-stu-id="4c23a-427">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="4c23a-428">Для поддержки других платформ ведения журналов или собственных требований к ведению журналов можно создать собственных пользовательских поставщиков.</span><span class="sxs-lookup"><span data-stu-id="4c23a-428">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="4c23a-429">Потоковая передача журналов Azure</span><span class="sxs-lookup"><span data-stu-id="4c23a-429">Azure log streaming</span></span>

<span data-ttu-id="4c23a-430">Потоковая передача журналов Azure позволяет просматривать активность журнала в режиме реального времени из следующих источников:</span><span class="sxs-lookup"><span data-stu-id="4c23a-430">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="4c23a-431">сервер приложений;</span><span class="sxs-lookup"><span data-stu-id="4c23a-431">The application server</span></span> 
* <span data-ttu-id="4c23a-432">веб-сервер;</span><span class="sxs-lookup"><span data-stu-id="4c23a-432">The web server</span></span>
* <span data-ttu-id="4c23a-433">трассировка неудачно завершенных запросов.</span><span class="sxs-lookup"><span data-stu-id="4c23a-433">Failed request tracing</span></span> 

<span data-ttu-id="4c23a-434">Настройка потоковой передачи журналов Azure</span><span class="sxs-lookup"><span data-stu-id="4c23a-434">To configure Azure log streaming:</span></span> 

* <span data-ttu-id="4c23a-435">Со страницы портала приложения перейдите на страницу **Журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="4c23a-435">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="4c23a-436">Включите параметр **Ведение журнала приложения (файловая система)**.</span><span class="sxs-lookup"><span data-stu-id="4c23a-436">Set **Application Logging (Filesystem)** to on.</span></span> 

![Страница журналов диагностики на портале Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="4c23a-438">Перейдите на страницу **Потоковая передача журналов**, чтобы просмотреть сообщения приложения.</span><span class="sxs-lookup"><span data-stu-id="4c23a-438">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="4c23a-439">Они записываются в приложении с помощью интерфейса `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="4c23a-439">They are logged by application through the `ILogger` interface.</span></span> 

![Потоковая передача журнала приложения на портале Azure](index/_static/azure-log-streaming.png)


## <a name="see-also"></a><span data-ttu-id="4c23a-441">См. также</span><span class="sxs-lookup"><span data-stu-id="4c23a-441">See also</span></span>

[<span data-ttu-id="4c23a-442">Высокопроизводительное ведение журналов с помощью LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="4c23a-442">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
