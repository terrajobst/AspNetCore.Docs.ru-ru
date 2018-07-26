---
title: Ведение журналов в ASP.NET Core
author: ardalis
description: Сведения о платформе ведения журналов в ASP.NET Core. Ознакомьтесь со встроенными поставщиками ведения журналов и получите подробные сведения о распространенных сторонних поставщиках.
ms.author: tdykstra
ms.date: 07/24/2018
uid: fundamentals/logging/index
ms.openlocfilehash: 0181566aeab1fa055435ac90887c019eef52878c
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228641"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="2e323-104">Ведение журналов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e323-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="2e323-105">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="2e323-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="2e323-106">ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="2e323-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="2e323-107">Встроенные поставщики позволяют отправлять журналы в одно или несколько мест назначений. Можно подключить стороннюю платформу ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="2e323-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="2e323-108">В этой статье содержатся сведения об использовании в коде встроенного API и поставщиков ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="2e323-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e323-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2e323-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2e323-110">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2e323-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="2e323-111">Сведения о журналах stdout при размещении в службах IIS см. здесь: <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="2e323-111">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="2e323-112">Сведения о журналах stdout при размещении в Службе приложений Azure см. здесь: <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="2e323-112">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="2e323-113">Создание журналов</span><span class="sxs-lookup"><span data-stu-id="2e323-113">How to create logs</span></span>

<span data-ttu-id="2e323-114">Чтобы создать журналы, примените объект [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2e323-114">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="2e323-115">Затем в этом объекте средства ведения журнала вызовите методы ведения журналов:</span><span class="sxs-lookup"><span data-stu-id="2e323-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="2e323-116">В этом примере создаются журналы с классом `TodoController` в качестве *категории*.</span><span class="sxs-lookup"><span data-stu-id="2e323-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="2e323-117">Категории рассматриваются [далее в этой статье](#log-category).</span><span class="sxs-lookup"><span data-stu-id="2e323-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="2e323-118">ASP.NET Core не поддерживает асинхронные методы ведения журналов. Здесь требуется настолько быстрая запись, что затраты на асинхронные операции совершенно нецелесообразны.</span><span class="sxs-lookup"><span data-stu-id="2e323-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="2e323-119">Если возникла обратная ситуация, рекомендуется рассмотреть возможность изменения способа ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="2e323-120">Если хранилище данных работает медленно, сначала записывайте сообщения журнала в быстродействующее хранилище, а затем перемещайте их в медленное хранилище.</span><span class="sxs-lookup"><span data-stu-id="2e323-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="2e323-121">Например, записывайте сообщения в очередь, которую обрабатывает другой процесс для долгосрочного сохранения в медленном хранилище.</span><span class="sxs-lookup"><span data-stu-id="2e323-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="2e323-122">Добавление поставщиков</span><span class="sxs-lookup"><span data-stu-id="2e323-122">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e323-123">Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="2e323-123">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="2e323-124">Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="2e323-124">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="2e323-125">Для использования поставщика вызовите метод расширения `Add<ProviderName>` поставщика в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2e323-125">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="2e323-126">Шаблон проекта по умолчанию включает ведение журнала с помощью метода[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___).</span><span class="sxs-lookup"><span data-stu-id="2e323-126">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2e323-127">Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="2e323-127">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="2e323-128">Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="2e323-128">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="2e323-129">Чтобы использовать поставщик, установите его пакет NuGet и вызовите метод расширения поставщика в экземпляре [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="2e323-129">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="2e323-130">[Внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) ASP.NET Core предоставляет экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="2e323-130">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="2e323-131">Методы расширения `AddConsole` и `AddDebug` определены в пакетах [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="2e323-131">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="2e323-132">Каждый метод расширения вызывает метод `ILoggerFactory.AddProvider`, передавая экземпляр поставщика.</span><span class="sxs-lookup"><span data-stu-id="2e323-132">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="2e323-133">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) добавляет поставщиков ведения журнала в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2e323-133">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="2e323-134">Чтобы получить выходные данные журнала из выполняемого ранее кода, добавьте поставщики ведения журналов в конструктор класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="2e323-134">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="2e323-135">Сведения о каждом [встроенном поставщике ведения журнала](#built-in-logging-providers) и ссылки на [сторонние поставщики](#third-party-logging-providers) приведены далее в статье.</span><span class="sxs-lookup"><span data-stu-id="2e323-135">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="settings-file-configuration"></a><span data-ttu-id="2e323-136">Конфигурация файла параметров</span><span class="sxs-lookup"><span data-stu-id="2e323-136">Settings file configuration</span></span>

<span data-ttu-id="2e323-137">Во всех предыдущих примерах в разделе [Добавление поставщиков](#how-to-add-providers) конфигурация поставщика ведения журнала загружается из раздела `Logging` файлов параметров приложений.</span><span class="sxs-lookup"><span data-stu-id="2e323-137">Each of the preceding examples in the [How to add providers](#how-to-add-providers) section loads logging provider configuration from the `Logging` section of app settings files.</span></span> <span data-ttu-id="2e323-138">В следующем примере показано содержимое типичного файла *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="2e323-138">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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
      "IncludeScopes": "true"
    }
  }
}
```

<span data-ttu-id="2e323-139">Ключи `LogLevel` представляют имена журналов.</span><span class="sxs-lookup"><span data-stu-id="2e323-139">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="2e323-140">Ключ `Default` применяется к журналам, которые не указаны явно.</span><span class="sxs-lookup"><span data-stu-id="2e323-140">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="2e323-141">Значение представляет [уровень ведения журнала](#log-level), который применен к указанному журналу.</span><span class="sxs-lookup"><span data-stu-id="2e323-141">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="2e323-142">Ключи журнала, которые задают `IncludeScopes` (в примере — `Console`), следует указывать, если [области ведения журнала](#log-scopes) включены для указанного журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-142">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="2e323-143">Ключи `LogLevel` представляют имена журналов.</span><span class="sxs-lookup"><span data-stu-id="2e323-143">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="2e323-144">Ключ `Default` применяется к журналам, которые не указаны явно.</span><span class="sxs-lookup"><span data-stu-id="2e323-144">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="2e323-145">Значение представляет [уровень ведения журнала](#log-level), который применен к указанному журналу.</span><span class="sxs-lookup"><span data-stu-id="2e323-145">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

## <a name="sample-logging-output"></a><span data-ttu-id="2e323-146">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="2e323-146">Sample logging output</span></span>

<span data-ttu-id="2e323-147">Используя пример кода из предыдущего раздела, вы увидите журналы в консоли после запуска из командной строки.</span><span class="sxs-lookup"><span data-stu-id="2e323-147">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="2e323-148">Ниже приведен пример выходных данных консоли:</span><span class="sxs-lookup"><span data-stu-id="2e323-148">Here's an example of console output:</span></span>

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

<span data-ttu-id="2e323-149">Эти журналы были созданы при переходе по адресу `http://localhost:5000/api/todo/0`, который запускает выполнение обоих вызовов `ILogger`, приведенных в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="2e323-149">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="2e323-150">Ниже приведен пример тех же журналов в том виде, как они отображаются в окне отладки при запуске примера приложения в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="2e323-150">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="2e323-151">Журналы, которые были созданы путем вызовов `ILogger`, приведенных в предыдущем разделе, начинаются с "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="2e323-151">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="2e323-152">Журналы, которые начинаются с категорий "Microsoft", входят в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2e323-152">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="2e323-153">В ASP.NET Core и коде приложения используется один и тот же API ведения журнала и одни и те же поставщики.</span><span class="sxs-lookup"><span data-stu-id="2e323-153">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="2e323-154">В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="2e323-154">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="2e323-155">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="2e323-155">NuGet packages</span></span>

<span data-ttu-id="2e323-156">Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="2e323-156">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="2e323-157">Категория журнала</span><span class="sxs-lookup"><span data-stu-id="2e323-157">Log category</span></span>

<span data-ttu-id="2e323-158">*Категория* входит в состав каждого создаваемого журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-158">A *category* is included with each log that you create.</span></span> <span data-ttu-id="2e323-159">Категория указывается при создании объекта `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="2e323-159">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="2e323-160">Категория может быть любой строкой, однако действует соглашение по использованию полного имени класса, из которого записываются журналы.</span><span class="sxs-lookup"><span data-stu-id="2e323-160">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="2e323-161">Например: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="2e323-161">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="2e323-162">Можно указать категорию в виде строки или использовать метод расширения, который наследует категорию от типа.</span><span class="sxs-lookup"><span data-stu-id="2e323-162">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="2e323-163">Чтобы указать категорию как строку, вызовите `CreateLogger` в экземпляре `ILoggerFactory`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="2e323-163">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="2e323-164">В большинстве случаев будет проще использовать `ILogger<T>`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="2e323-164">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="2e323-165">Это эквивалентно вызову `CreateLogger` с полным именем типа `T`.</span><span class="sxs-lookup"><span data-stu-id="2e323-165">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="2e323-166">Уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="2e323-166">Log level</span></span>

<span data-ttu-id="2e323-167">Каждый раз при создании журнала указывается его значение перечисления [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="2e323-167">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="2e323-168">Уровень ведения журнала означает степень серьезности или важности.</span><span class="sxs-lookup"><span data-stu-id="2e323-168">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="2e323-169">Например, можно создать журнал `Information` при нормальном завершении метода, журнал `Warning`, если метод возвращает код 404, и журнал `Error` при перехвате непредвиденного исключения.</span><span class="sxs-lookup"><span data-stu-id="2e323-169">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="2e323-170">В следующем примере кода имена методов (например, `LogWarning`) указывают уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-170">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="2e323-171">Первый параметр — это [ идентификатор события журнала](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="2e323-171">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="2e323-172">Второй параметр — это [шаблон сообщения](#log-message-template) с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода.</span><span class="sxs-lookup"><span data-stu-id="2e323-172">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="2e323-173">Параметры метода рассматриваются более подробно далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="2e323-173">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="2e323-174">Методы ведения журнала, которые содержат уровень в имени метода, являются [методами расширения для ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="2e323-174">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="2e323-175">На самом деле эти методы вызывают метод `Log`, принимающий параметр `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="2e323-175">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="2e323-176">Метод `Log`, в отличие от этих методов расширения, можно вызывать напрямую, однако его синтаксис довольно сложен.</span><span class="sxs-lookup"><span data-stu-id="2e323-176">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="2e323-177">Дополнительные сведения см. в разделе [Интерфейс ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) и [Исходный код расширений средства ведения журналов](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="2e323-177">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="2e323-178">В ASP.NET Core определяются следующие [уровни ведения журнала](/dotnet/api/microsoft.extensions.logging.loglevel), упорядоченные в этом документе от наименьшего до наибольшего уровня серьезности.</span><span class="sxs-lookup"><span data-stu-id="2e323-178">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="2e323-179">Трассировка = 0</span><span class="sxs-lookup"><span data-stu-id="2e323-179">Trace = 0</span></span>

  <span data-ttu-id="2e323-180">Для получения сведений, которые пригодятся только разработчикам при отладке.</span><span class="sxs-lookup"><span data-stu-id="2e323-180">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="2e323-181">Эти сообщения могут содержать конфиденциальные данные приложения, поэтому их не следует включать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="2e323-181">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="2e323-182">*По умолчанию отключено.*</span><span class="sxs-lookup"><span data-stu-id="2e323-182">*Disabled by default.*</span></span> <span data-ttu-id="2e323-183">Пример: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="2e323-183">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="2e323-184">Отладка = 1</span><span class="sxs-lookup"><span data-stu-id="2e323-184">Debug = 1</span></span>

  <span data-ttu-id="2e323-185">Для сведений, которые полезны в краткосрочной перспективе во время разработки и отладки.</span><span class="sxs-lookup"><span data-stu-id="2e323-185">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="2e323-186">Пример: `Entering method Configure with flag set to true.` как правило, в рабочей среде журналы уровня `Debug` применяются только при устранении неполадок, так как они занимают большой объем.</span><span class="sxs-lookup"><span data-stu-id="2e323-186">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="2e323-187">Информация = 2</span><span class="sxs-lookup"><span data-stu-id="2e323-187">Information = 2</span></span>

  <span data-ttu-id="2e323-188">Для отслеживания общего хода выполнения приложения.</span><span class="sxs-lookup"><span data-stu-id="2e323-188">For tracking the general flow of the application.</span></span> <span data-ttu-id="2e323-189">Эти журналы обычно полезны в долгосрочной перспективе.</span><span class="sxs-lookup"><span data-stu-id="2e323-189">These logs typically have some long-term value.</span></span> <span data-ttu-id="2e323-190">Пример: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="2e323-190">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="2e323-191">Предупреждение = 3</span><span class="sxs-lookup"><span data-stu-id="2e323-191">Warning = 3</span></span>

  <span data-ttu-id="2e323-192">Для нестандартных или непредвиденных событий, возникающих при выполнении приложения.</span><span class="sxs-lookup"><span data-stu-id="2e323-192">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="2e323-193">В их число могут входить ошибки или другие условия, которые не приводят к остановке приложения, но которые может потребоваться изучить.</span><span class="sxs-lookup"><span data-stu-id="2e323-193">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="2e323-194">Обработанные исключения являются распространенным условием использования уровня ведения журнала `Warning`.</span><span class="sxs-lookup"><span data-stu-id="2e323-194">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="2e323-195">Пример: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="2e323-195">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="2e323-196">Ошибка = 4</span><span class="sxs-lookup"><span data-stu-id="2e323-196">Error = 4</span></span>

  <span data-ttu-id="2e323-197">Для ошибок и исключений, которые не могут быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="2e323-197">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="2e323-198">Эти сообщения указывают на сбой текущего действия или операции (например, текущего HTTP-запроса), а не ошибку уровня приложения.</span><span class="sxs-lookup"><span data-stu-id="2e323-198">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="2e323-199">Пример сообщения журнала: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="2e323-199">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="2e323-200">Критический = 5</span><span class="sxs-lookup"><span data-stu-id="2e323-200">Critical = 5</span></span>

  <span data-ttu-id="2e323-201">Для сбоев, которые требуют немедленного внимания.</span><span class="sxs-lookup"><span data-stu-id="2e323-201">For failures that require immediate attention.</span></span> <span data-ttu-id="2e323-202">Примеры: потеря данных, нехватка места на диске.</span><span class="sxs-lookup"><span data-stu-id="2e323-202">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="2e323-203">Уровень ведения журнала можно использовать для управления объемом выходных данных, записываемых на определенный носитель или в окно отображения.</span><span class="sxs-lookup"><span data-stu-id="2e323-203">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="2e323-204">Например, в рабочей среде может потребоваться, чтобы все журналы уровня `Information` и ниже записывались в хранилище томов, а все журналы уровня `Warning` и выше записывались в хранилище значений.</span><span class="sxs-lookup"><span data-stu-id="2e323-204">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="2e323-205">Во время разработки можно отправлять журналы `Warning` или более высоких уровней серьезности на консоль.</span><span class="sxs-lookup"><span data-stu-id="2e323-205">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="2e323-206">Затем, если потребуется устранить неполадки, можно добавить уровень `Debug`.</span><span class="sxs-lookup"><span data-stu-id="2e323-206">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="2e323-207">В разделе [Фильтрация журналов](#log-filtering) далее в этой статье приводятся сведения о том, как контролировать уровни журнала, обрабатываемые поставщиком.</span><span class="sxs-lookup"><span data-stu-id="2e323-207">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="2e323-208">Платформа ASP.NET Core создает журналы уровня `Debug` для событий платформы.</span><span class="sxs-lookup"><span data-stu-id="2e323-208">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="2e323-209">В примерах журнала ранее в этой статье не указывались журналы с уровнем ниже `Information`, поэтому журналы уровня `Debug` не отображались.</span><span class="sxs-lookup"><span data-stu-id="2e323-209">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="2e323-210">Ниже приведен пример журналов консоли при запуске примера приложения, настроенного для отображения журналов уровня `Debug` и выше для поставщика Console.</span><span class="sxs-lookup"><span data-stu-id="2e323-210">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="2e323-211">Идентификатор события журнала</span><span class="sxs-lookup"><span data-stu-id="2e323-211">Log event ID</span></span>

<span data-ttu-id="2e323-212">При каждом создании журнала можно указывать *идентификатор события*.</span><span class="sxs-lookup"><span data-stu-id="2e323-212">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="2e323-213">В примере приложения для этого используется локально определенный класс `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="2e323-213">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="2e323-214">Идентификатор события — это целочисленное значение, используемое для связи набора зарегистрированных событий друг с другом.</span><span class="sxs-lookup"><span data-stu-id="2e323-214">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="2e323-215">Например, журнал для добавления позиции в корзину покупок может иметь идентификатор события 1000, а журнал для завершения покупки может иметь идентификатор события 1001.</span><span class="sxs-lookup"><span data-stu-id="2e323-215">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="2e323-216">В зависимости от поставщика идентификатор события может быть сохранен в поле или включен в текст сообщения в выходных данных журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-216">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="2e323-217">Поставщик Debug не отображает идентификаторы событий, но поставщик Console показывает их в квадратных скобках после категории:</span><span class="sxs-lookup"><span data-stu-id="2e323-217">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="2e323-218">Шаблон сообщения журнала</span><span class="sxs-lookup"><span data-stu-id="2e323-218">Log message template</span></span>

<span data-ttu-id="2e323-219">Каждый раз при записи сообщения журнала предоставляется шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="2e323-219">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="2e323-220">Шаблон сообщения может быть строкой либо содержать именованные заполнители, в которые помещаются значения аргументов.</span><span class="sxs-lookup"><span data-stu-id="2e323-220">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="2e323-221">Шаблон не является строкой формата, а заполнители должны быть именованными, а не нумерованными.</span><span class="sxs-lookup"><span data-stu-id="2e323-221">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="2e323-222">Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами.</span><span class="sxs-lookup"><span data-stu-id="2e323-222">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="2e323-223">Предположим, что имеется следующий код:</span><span class="sxs-lookup"><span data-stu-id="2e323-223">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="2e323-224">Полученное в результате сообщение журнала выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2e323-224">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="2e323-225">Платформа ведения журнала поддерживает такое форматирование сообщений, чтобы поставщики ведения журналов могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журнала](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="2e323-225">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="2e323-226">Так как в систему ведения журналов передаются сами аргументы, а не только отформатированный шаблон сообщения, поставщики ведения журналов могут сохранять значения параметров как поля (наряду с шаблоном сообщения).</span><span class="sxs-lookup"><span data-stu-id="2e323-226">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="2e323-227">Если выходные данных журналов направляются в хранилище таблиц Azure, вызов метода средства ведения журнала выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2e323-227">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="2e323-228">Каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-228">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="2e323-229">Можно находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.</span><span class="sxs-lookup"><span data-stu-id="2e323-229">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="2e323-230">Ведение журнала исключений</span><span class="sxs-lookup"><span data-stu-id="2e323-230">Logging exceptions</span></span>

<span data-ttu-id="2e323-231">Методы средства ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="2e323-231">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="2e323-232">Разные поставщики обрабатывают сведения об исключениях по-разному.</span><span class="sxs-lookup"><span data-stu-id="2e323-232">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="2e323-233">Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="2e323-233">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="2e323-234">Фильтрация журналов</span><span class="sxs-lookup"><span data-stu-id="2e323-234">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e323-235">Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий.</span><span class="sxs-lookup"><span data-stu-id="2e323-235">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="2e323-236">Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются.</span><span class="sxs-lookup"><span data-stu-id="2e323-236">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="2e323-237">Чтобы запретить все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-237">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="2e323-238">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="2e323-238">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="2e323-239">Создание правил фильтрации в конфигурации</span><span class="sxs-lookup"><span data-stu-id="2e323-239">Create filter rules in configuration</span></span>

<span data-ttu-id="2e323-240">Шаблоны проектов создают код, который вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug.</span><span class="sxs-lookup"><span data-stu-id="2e323-240">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="2e323-241">Метод `CreateDefaultBuilder` также настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, используя код, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="2e323-241">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="2e323-242">Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="2e323-242">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="2e323-243">Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно, которое применяется ко всем поставщикам.</span><span class="sxs-lookup"><span data-stu-id="2e323-243">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="2e323-244">Далее вы увидите, как при создании объекта `ILogger` для каждого поставщика выбирается лишь одно из этих правил.</span><span class="sxs-lookup"><span data-stu-id="2e323-244">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="2e323-245">Правила фильтрации в коде</span><span class="sxs-lookup"><span data-stu-id="2e323-245">Filter rules in code</span></span>

<span data-ttu-id="2e323-246">Можно зарегистрировать правила фильтрации в коде, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="2e323-246">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="2e323-247">Второй `AddFilter` указывает поставщика, Debug, используя имя его типа.</span><span class="sxs-lookup"><span data-stu-id="2e323-247">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="2e323-248">Первый `AddFilter` применяется ко всем поставщикам, так как он не определяет тип поставщика.</span><span class="sxs-lookup"><span data-stu-id="2e323-248">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="2e323-249">Применение правил фильтрации</span><span class="sxs-lookup"><span data-stu-id="2e323-249">How filtering rules are applied</span></span>

<span data-ttu-id="2e323-250">Данные конфигурации и код `AddFilter`, приведенный в предыдущих примерах, создают правила, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="2e323-250">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="2e323-251">Первые шесть взяты из примера конфигурации, а последние два — из примера кода.</span><span class="sxs-lookup"><span data-stu-id="2e323-251">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="2e323-252">Число</span><span class="sxs-lookup"><span data-stu-id="2e323-252">Number</span></span> | <span data-ttu-id="2e323-253">Поставщик</span><span class="sxs-lookup"><span data-stu-id="2e323-253">Provider</span></span>      | <span data-ttu-id="2e323-254">Категории, которые начинаются с...</span><span class="sxs-lookup"><span data-stu-id="2e323-254">Categories that begin with ...</span></span>          | <span data-ttu-id="2e323-255">Минимальный уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="2e323-255">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="2e323-256">1</span><span class="sxs-lookup"><span data-stu-id="2e323-256">1</span></span>      | <span data-ttu-id="2e323-257">Отладка</span><span class="sxs-lookup"><span data-stu-id="2e323-257">Debug</span></span>         | <span data-ttu-id="2e323-258">Все категории</span><span class="sxs-lookup"><span data-stu-id="2e323-258">All categories</span></span>                          | <span data-ttu-id="2e323-259">Сведения</span><span class="sxs-lookup"><span data-stu-id="2e323-259">Information</span></span>       |
| <span data-ttu-id="2e323-260">2</span><span class="sxs-lookup"><span data-stu-id="2e323-260">2</span></span>      | <span data-ttu-id="2e323-261">Консоль</span><span class="sxs-lookup"><span data-stu-id="2e323-261">Console</span></span>       | <span data-ttu-id="2e323-262">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="2e323-262">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="2e323-263">Предупреждение</span><span class="sxs-lookup"><span data-stu-id="2e323-263">Warning</span></span>           |
| <span data-ttu-id="2e323-264">3</span><span class="sxs-lookup"><span data-stu-id="2e323-264">3</span></span>      | <span data-ttu-id="2e323-265">Консоль</span><span class="sxs-lookup"><span data-stu-id="2e323-265">Console</span></span>       | <span data-ttu-id="2e323-266">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="2e323-266">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="2e323-267">Отладка</span><span class="sxs-lookup"><span data-stu-id="2e323-267">Debug</span></span>             |
| <span data-ttu-id="2e323-268">4</span><span class="sxs-lookup"><span data-stu-id="2e323-268">4</span></span>      | <span data-ttu-id="2e323-269">Консоль</span><span class="sxs-lookup"><span data-stu-id="2e323-269">Console</span></span>       | <span data-ttu-id="2e323-270">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="2e323-270">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="2e323-271">Error</span><span class="sxs-lookup"><span data-stu-id="2e323-271">Error</span></span>             |
| <span data-ttu-id="2e323-272">5</span><span class="sxs-lookup"><span data-stu-id="2e323-272">5</span></span>      | <span data-ttu-id="2e323-273">Консоль</span><span class="sxs-lookup"><span data-stu-id="2e323-273">Console</span></span>       | <span data-ttu-id="2e323-274">Все категории</span><span class="sxs-lookup"><span data-stu-id="2e323-274">All categories</span></span>                          | <span data-ttu-id="2e323-275">Сведения</span><span class="sxs-lookup"><span data-stu-id="2e323-275">Information</span></span>       |
| <span data-ttu-id="2e323-276">6</span><span class="sxs-lookup"><span data-stu-id="2e323-276">6</span></span>      | <span data-ttu-id="2e323-277">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="2e323-277">All providers</span></span> | <span data-ttu-id="2e323-278">Все категории</span><span class="sxs-lookup"><span data-stu-id="2e323-278">All categories</span></span>                          | <span data-ttu-id="2e323-279">Отладка</span><span class="sxs-lookup"><span data-stu-id="2e323-279">Debug</span></span>             |
| <span data-ttu-id="2e323-280">7</span><span class="sxs-lookup"><span data-stu-id="2e323-280">7</span></span>      | <span data-ttu-id="2e323-281">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="2e323-281">All providers</span></span> | <span data-ttu-id="2e323-282">Система</span><span class="sxs-lookup"><span data-stu-id="2e323-282">System</span></span>                                  | <span data-ttu-id="2e323-283">Отладка</span><span class="sxs-lookup"><span data-stu-id="2e323-283">Debug</span></span>             |
| <span data-ttu-id="2e323-284">8</span><span class="sxs-lookup"><span data-stu-id="2e323-284">8</span></span>      | <span data-ttu-id="2e323-285">Отладка</span><span class="sxs-lookup"><span data-stu-id="2e323-285">Debug</span></span>         | <span data-ttu-id="2e323-286">Майкрософт</span><span class="sxs-lookup"><span data-stu-id="2e323-286">Microsoft</span></span>                               | <span data-ttu-id="2e323-287">Трассировка</span><span class="sxs-lookup"><span data-stu-id="2e323-287">Trace</span></span>             |

<span data-ttu-id="2e323-288">При создании объекта `ILogger`, используемого для создания журналов, объект `ILoggerFactory` выбирает одно правило для каждого поставщика, которое будет применено к этому средству ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-288">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="2e323-289">Все сообщения, записываемые с помощью этого объекта `ILogger`, фильтруются на основе выбранных правил.</span><span class="sxs-lookup"><span data-stu-id="2e323-289">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="2e323-290">Самое подходящее правило для каждой пары поставщика и категории выбирается из списка доступных правил.</span><span class="sxs-lookup"><span data-stu-id="2e323-290">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="2e323-291">При создании `ILogger` для данной категории для каждого поставщика используется приведенный далее алгоритм:</span><span class="sxs-lookup"><span data-stu-id="2e323-291">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="2e323-292">Выберите все правила, которые соответствуют поставщику или его псевдониму.</span><span class="sxs-lookup"><span data-stu-id="2e323-292">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="2e323-293">Если ничего не найдено, выберите все правила с пустым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="2e323-293">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="2e323-294">В результатах предыдущего шага выберите правила с самым длинным соответствующим префиксом категории.</span><span class="sxs-lookup"><span data-stu-id="2e323-294">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="2e323-295">Если ничего не найдено, выберите все правила, которые не указывают категорию.</span><span class="sxs-lookup"><span data-stu-id="2e323-295">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="2e323-296">Если выбрано несколько правил, примите **последнее**.</span><span class="sxs-lookup"><span data-stu-id="2e323-296">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="2e323-297">Если правила не выбраны, используйте `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="2e323-297">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="2e323-298">Предположим, что у вас есть указанный выше список правил и вы создаете объект `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="2e323-298">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="2e323-299">К поставщику Debug применяются правила 1, 6 и 8.</span><span class="sxs-lookup"><span data-stu-id="2e323-299">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="2e323-300">Правило 8 является наиболее подходящим, поэтому выбрано оно.</span><span class="sxs-lookup"><span data-stu-id="2e323-300">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="2e323-301">К поставщику отладки применяются правила 3, 4, 5 и 6.</span><span class="sxs-lookup"><span data-stu-id="2e323-301">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="2e323-302">Правило 3 является наиболее подходящим.</span><span class="sxs-lookup"><span data-stu-id="2e323-302">Rule 3 is most specific.</span></span>

<span data-ttu-id="2e323-303">При создании журналов с помощью `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" журналы уровня `Trace` и выше перейдут поставщику отладки, а журналы уровня `Debug` и выше перейдут поставщику консоли.</span><span class="sxs-lookup"><span data-stu-id="2e323-303">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="2e323-304">Псевдонимы поставщиков</span><span class="sxs-lookup"><span data-stu-id="2e323-304">Provider aliases</span></span>

<span data-ttu-id="2e323-305">Имя типа можно использовать, чтобы указать поставщик в конфигурации, но каждый поставщик имеет более короткий и простой в использовании *псевдоним*.</span><span class="sxs-lookup"><span data-stu-id="2e323-305">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="2e323-306">Для встроенных поставщиков используйте следующие псевдонимы:</span><span class="sxs-lookup"><span data-stu-id="2e323-306">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="2e323-307">Консоль</span><span class="sxs-lookup"><span data-stu-id="2e323-307">Console</span></span>
* <span data-ttu-id="2e323-308">Отладка</span><span class="sxs-lookup"><span data-stu-id="2e323-308">Debug</span></span>
* <span data-ttu-id="2e323-309">EventLog</span><span class="sxs-lookup"><span data-stu-id="2e323-309">EventLog</span></span>
* <span data-ttu-id="2e323-310">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="2e323-310">AzureAppServices</span></span>
* <span data-ttu-id="2e323-311">TraceSource</span><span class="sxs-lookup"><span data-stu-id="2e323-311">TraceSource</span></span>
* <span data-ttu-id="2e323-312">EventSource</span><span class="sxs-lookup"><span data-stu-id="2e323-312">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="2e323-313">Минимальный уровень по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2e323-313">Default minimum level</span></span>

<span data-ttu-id="2e323-314">Существует параметр минимального уровня, который применяется, только если к определенному поставщику или категории не подходят правила из конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="2e323-314">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="2e323-315">В следующем примере показано, как задать минимальный уровень:</span><span class="sxs-lookup"><span data-stu-id="2e323-315">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="2e323-316">Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.</span><span class="sxs-lookup"><span data-stu-id="2e323-316">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="2e323-317">Функции фильтрации</span><span class="sxs-lookup"><span data-stu-id="2e323-317">Filter functions</span></span>

<span data-ttu-id="2e323-318">В функции фильтрации можно написать код для применения правил фильтрации.</span><span class="sxs-lookup"><span data-stu-id="2e323-318">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="2e323-319">Функция фильтрации вызывается для всех поставщиков и категорий, которым в конфигурации и (или) коде не назначены правила.</span><span class="sxs-lookup"><span data-stu-id="2e323-319">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="2e323-320">Код в функции имеет доступ к типу поставщика, категории и уровню ведения журнала, чтобы определить необходимость регистрации сообщений.</span><span class="sxs-lookup"><span data-stu-id="2e323-320">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="2e323-321">Пример:</span><span class="sxs-lookup"><span data-stu-id="2e323-321">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2e323-322">Некоторые поставщики ведения журналов позволяют указывать, когда журналы следует записывать на носитель, а когда — игнорировать. При этом учитывается уровень ведения журнала и категория.</span><span class="sxs-lookup"><span data-stu-id="2e323-322">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="2e323-323">Методы расширения `AddConsole` и `AddDebug`предоставляют перегрузки для передачи условий фильтрации.</span><span class="sxs-lookup"><span data-stu-id="2e323-323">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="2e323-324">В следующем примере кода поставщик Console игнорирует журналы с уровнем ниже `Warning`, а поставщик Debug не учитывает журналы, создаваемые платформой.</span><span class="sxs-lookup"><span data-stu-id="2e323-324">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="2e323-325">Метод `AddEventLog` имеет перегрузку, принимающую экземпляр `EventLogSettings`, который в своем свойстве `Filter` может содержать функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="2e323-325">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="2e323-326">Поставщик TraceSource не предоставляет эти перегрузки, так как уровень ведения журнала и другие параметры определяются для него на основе используемых `SourceSwitch` и `TraceListener`.</span><span class="sxs-lookup"><span data-stu-id="2e323-326">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="2e323-327">Можно задать правила фильтрации для всех поставщиков, которые зарегистрированы в экземпляре `ILoggerFactory`, с помощью метода расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="2e323-327">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="2e323-328">В приведенном ниже примере журналы платформы (категория начинается с "Microsoft" или "System") ограничиваются предупреждениями, однако разрешаются журналы приложений на уровне отладки.</span><span class="sxs-lookup"><span data-stu-id="2e323-328">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="2e323-329">Чтобы использовать фильтрацию для предотвращения создания всех журналов для определенной категории, укажите `LogLevel.None` в качестве минимального уровня ведения журнала для этой категории.</span><span class="sxs-lookup"><span data-stu-id="2e323-329">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="2e323-330">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="2e323-330">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="2e323-331">Пакет NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) предоставляет метод расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="2e323-331">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="2e323-332">Метод возвращает новый экземпляр `ILoggerFactory` для фильтрации журнала сообщений, переданного всем поставщикам средства ведения журналов, которые зарегистрированы в этом экземпляре.</span><span class="sxs-lookup"><span data-stu-id="2e323-332">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="2e323-333">Он не влияет на другие экземпляры `ILoggerFactory`, включая исходный экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="2e323-333">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="2e323-334">Области журналов</span><span class="sxs-lookup"><span data-stu-id="2e323-334">Log scopes</span></span>

<span data-ttu-id="2e323-335">Для присоединения одних и тех же данных к каждому журналу, созданному в рамках набора, можно сгруппировать набор логических операций в *область*.</span><span class="sxs-lookup"><span data-stu-id="2e323-335">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="2e323-336">Например, может потребоваться, чтобы каждый журнал, созданный в ходе обработки транзакции, содержал идентификатор транзакции.</span><span class="sxs-lookup"><span data-stu-id="2e323-336">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="2e323-337">Область — это тип `IDisposable`, возвращаемый методом [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) и действующий до удаления.</span><span class="sxs-lookup"><span data-stu-id="2e323-337">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="2e323-338">Чтобы использовать область, следует заключить вызовы средства ведения журналов в блок `using`, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="2e323-338">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="2e323-339">Следующий код предоставляет области для поставщика Console:</span><span class="sxs-lookup"><span data-stu-id="2e323-339">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="2e323-340">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2e323-340">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="2e323-341">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="2e323-341">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="2e323-342">`IncludeScopes` можно настроить в файле конфигурации *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="2e323-342">`IncludeScopes` can be configured via *appsettings* configuration files.</span></span> <span data-ttu-id="2e323-343">Дополнительные сведения см. в разделе [Конфигурация файла параметров](#settings-file-configuration).</span><span class="sxs-lookup"><span data-stu-id="2e323-343">For more information, see the [Settings file configuration](#settings-file-configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2e323-344">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2e323-344">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="2e323-345">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="2e323-345">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2e323-346">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2e323-346">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="2e323-347">Каждое сообщение журнала содержит ограниченную информацию:</span><span class="sxs-lookup"><span data-stu-id="2e323-347">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="2e323-348">Встроенные поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="2e323-348">Built-in logging providers</span></span>

<span data-ttu-id="2e323-349">В состав ASP.NET Core входят следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="2e323-349">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="2e323-350">Консоль</span><span class="sxs-lookup"><span data-stu-id="2e323-350">Console</span></span>](#console-provider)
* [<span data-ttu-id="2e323-351">Отладка</span><span class="sxs-lookup"><span data-stu-id="2e323-351">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="2e323-352">EventSource</span><span class="sxs-lookup"><span data-stu-id="2e323-352">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="2e323-353">EventLog</span><span class="sxs-lookup"><span data-stu-id="2e323-353">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="2e323-354">TraceSource</span><span class="sxs-lookup"><span data-stu-id="2e323-354">TraceSource</span></span>](#tracesource-provider)
* <span data-ttu-id="2e323-355">[служба приложений Azure](#azure-app-service-provider);</span><span class="sxs-lookup"><span data-stu-id="2e323-355">[Azure App Service](#azure-app-service-provider)</span></span>

### <a name="console-provider"></a><span data-ttu-id="2e323-356">Поставщик Console</span><span class="sxs-lookup"><span data-stu-id="2e323-356">Console provider</span></span>

<span data-ttu-id="2e323-357">Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль.</span><span class="sxs-lookup"><span data-stu-id="2e323-357">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="2e323-358">[Перегрузки AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) позволяют передавать минимальный уровень ведения журнала, функцию фильтрации и логическое значение, указывающее, поддерживаются ли области.</span><span class="sxs-lookup"><span data-stu-id="2e323-358">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="2e323-359">Другим вариантом является передача объекта `IConfiguration`, который может указать поддержку областей и уровни ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-359">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="2e323-360">Если вы собираетесь использовать поставщик Console в рабочей среде, имейте в виду, что он оказывает значительное влияние на производительность.</span><span class="sxs-lookup"><span data-stu-id="2e323-360">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="2e323-361">При создании проекта в Visual Studio метод `AddConsole` выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2e323-361">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="2e323-362">Этот код ссылается на раздел `Logging` файла *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="2e323-362">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="2e323-363">Приведенные параметры ограничивают журналы платформы предупреждениями, но разрешают регистрацию приложений на уровне отладки, как описано в разделе [Фильтрация журналов](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="2e323-363">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="2e323-364">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="2e323-364">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="2e323-365">Поставщик Debug</span><span class="sxs-lookup"><span data-stu-id="2e323-365">Debug provider</span></span>

<span data-ttu-id="2e323-366">Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (вызовов метода `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="2e323-366">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="2e323-367">В Linux этот поставщик записывает журналы в каталог */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="2e323-367">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="2e323-368">[Перегрузки AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) позволяют передавать минимальный уровень ведения журнала или функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="2e323-368">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="2e323-369">Поставщик EventSource</span><span class="sxs-lookup"><span data-stu-id="2e323-369">EventSource provider</span></span>

<span data-ttu-id="2e323-370">Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии, реализовывать события трассировки можно с помощью пакета поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource).</span><span class="sxs-lookup"><span data-stu-id="2e323-370">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="2e323-371">В Windows используется [трассировка событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="2e323-371">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="2e323-372">Поставщик является кроссплатформенным, но для Linux и macOS инструменты сбора событий и отображений пока отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="2e323-372">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="2e323-373">Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="2e323-373">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="2e323-374">Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2e323-374">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="2e323-375">Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**.</span><span class="sxs-lookup"><span data-stu-id="2e323-375">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="2e323-376">(Не пропустите звездочку в начале строки.)</span><span class="sxs-lookup"><span data-stu-id="2e323-376">(Don't miss the asterisk at the start of the string.)</span></span>

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="2e323-378">Поставщик Windows EventLog</span><span class="sxs-lookup"><span data-stu-id="2e323-378">Windows EventLog provider</span></span>

<span data-ttu-id="2e323-379">Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="2e323-379">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="2e323-380">[Перегрузки AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) позволяют передавать `EventLogSettings` или минимальный уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="2e323-380">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="2e323-381">Поставщик TraceSource</span><span class="sxs-lookup"><span data-stu-id="2e323-381">TraceSource provider</span></span>

<span data-ttu-id="2e323-382">Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="2e323-382">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="2e323-383">[Перегрузки AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) позволяют передавать исходный параметр и прослушиватель трассировки.</span><span class="sxs-lookup"><span data-stu-id="2e323-383">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="2e323-384">Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core).</span><span class="sxs-lookup"><span data-stu-id="2e323-384">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="2e323-385">Поставщик позволяет перенаправлять сообщения различным [прослушивателям](/dotnet/framework/debug-trace-profile/trace-listeners), например [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr), который используется в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="2e323-385">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="2e323-386">В следующем примере показана настройка поставщика `TraceSource`, записывающего в окне консоли сообщения типа `Warning` и сообщения с более высоким уровнем серьезности.</span><span class="sxs-lookup"><span data-stu-id="2e323-386">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="2e323-387">Поставщик службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="2e323-387">Azure App Service provider</span></span>

<span data-ttu-id="2e323-388">Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure.</span><span class="sxs-lookup"><span data-stu-id="2e323-388">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="2e323-389">Пакет поставщика доступен для приложений, предназначенных для .NET Core 1.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="2e323-389">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e323-390">Если планируется использовать .NET Core, обратите внимание на следующее.</span><span class="sxs-lookup"><span data-stu-id="2e323-390">If targeting .NET Core, note the following points:</span></span>

* <span data-ttu-id="2e323-391">Пакет поставщика включен в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ASP.NET Core, но не включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2e323-391">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) but not in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="2e323-392">Не выполняйте явный вызов [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="2e323-392">Don't explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="2e323-393">Поставщик становится автоматически доступным для приложения при развертывании приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="2e323-393">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="2e323-394">Если планируется использовать .NET Framework или будет указана ссылка на метапакет `Microsoft.AspNetCore.App`, добавьте пакет поставщика в проект.</span><span class="sxs-lookup"><span data-stu-id="2e323-394">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="2e323-395">Вызовите `AddAzureWebAppDiagnostics` в экземпляре [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory).</span><span class="sxs-lookup"><span data-stu-id="2e323-395">Invoke `AddAzureWebAppDiagnostics` on an [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) instance:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

<span data-ttu-id="2e323-396">Перегрузка [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) позволяет передавать [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) для переопределения параметров по умолчанию, таких как шаблон выходных данных ведения журнала, имя BLOB-объекта и предельный размер файла.</span><span class="sxs-lookup"><span data-stu-id="2e323-396">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="2e323-397">(*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам на основе того, который вы указали при вызове метода `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="2e323-397">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

<span data-ttu-id="2e323-398">При развертывании в приложение службы приложений приложение учитывает параметры в разделе [Журналы диагностики](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="2e323-398">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="2e323-399">При обновлении этих параметров изменения вступают в силу немедленно без перезапуска или повторного развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="2e323-399">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Параметры ведения журнала Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="2e323-401">По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="2e323-401">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="2e323-402">Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2.</span><span class="sxs-lookup"><span data-stu-id="2e323-402">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="2e323-403">Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="2e323-403">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="2e323-404">Дополнительные сведения о поведении по умолчанию см. в разделе [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="2e323-404">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="2e323-405">Поставщик работает только при выполнении проекта в среде Azure.</span><span class="sxs-lookup"><span data-stu-id="2e323-405">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="2e323-406">Он не функционирует при запуске проекта в локальной среде &mdash;(то есть не выполняет запись в локальные файлы или в локальное хранилище разработки для больших двоичных объектов).</span><span class="sxs-lookup"><span data-stu-id="2e323-406">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="2e323-407">Сторонние поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="2e323-407">Third-party logging providers</span></span>

<span data-ttu-id="2e323-408">Некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2e323-408">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="2e323-409">[ELMAH.IO](https://elmah.io/) ([в репозитории GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="2e323-409">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="2e323-410">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([в репозитории GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="2e323-410">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="2e323-411">[JSNLog](http://jsnlog.com/) ([в репозитории GitHub](https://github.com/mperdeck/jsnlog));</span><span class="sxs-lookup"><span data-stu-id="2e323-411">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="2e323-412">[Loggr](http://loggr.net/) ([в репозитории GitHub](https://github.com/imobile3/Loggr.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="2e323-412">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="2e323-413">[NLog](http://nlog-project.org/) ([в репозитории GitHub](https://github.com/NLog/NLog.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="2e323-413">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="2e323-414">[Serilog](https://serilog.net/) ([в репозитории GitHub](https://github.com/serilog/serilog-extensions-logging)).</span><span class="sxs-lookup"><span data-stu-id="2e323-414">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="2e323-415">Некоторые сторонние платформы выполняют [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="2e323-415">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="2e323-416">Использование сторонней платформы аналогично использованию одного из встроенных поставщиков:</span><span class="sxs-lookup"><span data-stu-id="2e323-416">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="2e323-417">Добавьте пакет NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="2e323-417">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="2e323-418">Вызовите метод расширения в `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="2e323-418">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="2e323-419">Дополнительные сведения см. в документации по каждой платформе.</span><span class="sxs-lookup"><span data-stu-id="2e323-419">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="2e323-420">Потоковая передача журналов Azure</span><span class="sxs-lookup"><span data-stu-id="2e323-420">Azure log streaming</span></span>

<span data-ttu-id="2e323-421">Потоковая передача журналов Azure позволяет просматривать активность журнала в режиме реального времени из следующих источников:</span><span class="sxs-lookup"><span data-stu-id="2e323-421">Azure log streaming enables you to view log activity in real time from:</span></span>

* <span data-ttu-id="2e323-422">сервер приложений;</span><span class="sxs-lookup"><span data-stu-id="2e323-422">The application server</span></span>
* <span data-ttu-id="2e323-423">веб-сервер;</span><span class="sxs-lookup"><span data-stu-id="2e323-423">The web server</span></span>
* <span data-ttu-id="2e323-424">трассировка неудачно завершенных запросов.</span><span class="sxs-lookup"><span data-stu-id="2e323-424">Failed request tracing</span></span>

<span data-ttu-id="2e323-425">Настройка потоковой передачи журналов Azure</span><span class="sxs-lookup"><span data-stu-id="2e323-425">To configure Azure log streaming:</span></span>

* <span data-ttu-id="2e323-426">Со страницы портала приложения перейдите на страницу **Журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="2e323-426">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="2e323-427">Включите параметр **Ведение журнала приложения (файловая система)**.</span><span class="sxs-lookup"><span data-stu-id="2e323-427">Set **Application Logging (Filesystem)** to on.</span></span>

![Страница журналов диагностики на портале Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="2e323-429">Перейдите на страницу **Потоковая передача журналов**, чтобы просмотреть сообщения приложения.</span><span class="sxs-lookup"><span data-stu-id="2e323-429">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="2e323-430">Они передаются в приложении через интерфейс `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="2e323-430">They're logged by application through the `ILogger` interface.</span></span>

![Потоковая передача журнала приложения на портале Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="2e323-432">Ведение журнала трассировки Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="2e323-432">Azure Application Insights trace logging</span></span>

<span data-ttu-id="2e323-433">Пакет SDK [Application Insights](https://azure.microsoft.com/services/application-insights/) способен собирать телеметрию трассировки из журналов, сформированных инфраструктурой ведения журналов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2e323-433">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="2e323-434">Дополнительные сведения см. в [вики-сайте Microsoft/ApplicationInsights-aspnetcore: ведение журнала](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="2e323-434">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e323-435">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2e323-435">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
