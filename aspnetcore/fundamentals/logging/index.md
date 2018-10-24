---
title: Ведение журналов в ASP.NET Core
author: ardalis
description: Сведения о платформе ведения журналов в ASP.NET Core. Ознакомьтесь со встроенными поставщиками ведения журналов и получите подробные сведения о распространенных сторонних поставщиках.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/19/2018
uid: fundamentals/logging/index
ms.openlocfilehash: 7a87791abdc91c43796ce72764d0cb3938ed90ec
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578462"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="16fa7-104">Ведение журналов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16fa7-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="16fa7-105">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="16fa7-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="16fa7-106">ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="16fa7-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="16fa7-107">Встроенные поставщики позволяют отправлять журналы в одно или несколько мест назначений. Можно подключить стороннюю платформу ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="16fa7-108">В этой статье содержатся сведения об использовании в коде встроенного API и поставщиков ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

<span data-ttu-id="16fa7-109">Сведения о журналах stdout при размещении в службах IIS см. здесь: <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="16fa7-109">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="16fa7-110">Сведения о журналах stdout при размещении в Службе приложений Azure см. здесь: <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="16fa7-110">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

<span data-ttu-id="16fa7-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16fa7-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="16fa7-112">Создание журналов</span><span class="sxs-lookup"><span data-stu-id="16fa7-112">How to create logs</span></span>

<span data-ttu-id="16fa7-113">Для создания журналов получите объект [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="16fa7-113">To create logs, obtain an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="16fa7-114">Затем в этом объекте средства ведения журнала вызовите методы ведения журналов:</span><span class="sxs-lookup"><span data-stu-id="16fa7-114">Then call logging methods on that logger object:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="16fa7-115">В этом примере создаются журналы с классом `TodoController` в качестве *категории*.</span><span class="sxs-lookup"><span data-stu-id="16fa7-115">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="16fa7-116">Категории рассматриваются [далее в этой статье](#log-category).</span><span class="sxs-lookup"><span data-stu-id="16fa7-116">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="16fa7-117">ASP.NET Core не поддерживает асинхронные методы ведения журналов. Здесь требуется настолько быстрая запись, что затраты на асинхронные операции совершенно нецелесообразны.</span><span class="sxs-lookup"><span data-stu-id="16fa7-117">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="16fa7-118">Если возникла обратная ситуация, рекомендуется рассмотреть возможность изменения способа ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-118">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="16fa7-119">Если хранилище данных работает медленно, сначала записывайте сообщения журнала в быстродействующее хранилище, а затем перемещайте их в медленное хранилище.</span><span class="sxs-lookup"><span data-stu-id="16fa7-119">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="16fa7-120">Например, записывайте сообщения в очередь, которую обрабатывает другой процесс для долгосрочного сохранения в медленном хранилище.</span><span class="sxs-lookup"><span data-stu-id="16fa7-120">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="16fa7-121">Добавление поставщиков</span><span class="sxs-lookup"><span data-stu-id="16fa7-121">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="16fa7-122">Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="16fa7-122">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="16fa7-123">Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-123">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="16fa7-124">Для использования поставщика вызовите метод расширения `Add<ProviderName>` поставщика в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="16fa7-124">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="16fa7-125">Шаблон проекта по умолчанию активирует поставщики ведения журналов Console и Debug с помощью вызова метода расширения [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="16fa7-125">The default project template enables the Console and Debug logging providers with a call to the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="16fa7-126">Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="16fa7-126">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="16fa7-127">Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-127">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="16fa7-128">Чтобы использовать поставщик, установите его пакет NuGet и вызовите метод расширения поставщика в экземпляре [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="16fa7-128">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="16fa7-129">[Внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) ASP.NET Core предоставляет экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-129">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="16fa7-130">Методы расширения `AddConsole` и `AddDebug` определены в пакетах [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="16fa7-130">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="16fa7-131">Каждый метод расширения вызывает метод `ILoggerFactory.AddProvider`, передавая экземпляр поставщика.</span><span class="sxs-lookup"><span data-stu-id="16fa7-131">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="16fa7-132">[Пример приложения 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) добавляет поставщиков ведения журнала в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-132">The [1.x sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="16fa7-133">Чтобы получить выходные данные журнала из выполняемого ранее кода, добавьте поставщики ведения журналов в конструктор класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-133">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="16fa7-134">Дополнительные сведения о встроенных поставщиках ведения журналов см. [здесь](#built-in-logging-providers). Кроме того, далее в статье приведены ссылки на [сторонние поставщики](#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="16fa7-134">Learn more about the [built-in logging providers](#built-in-logging-providers) and find links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="configuration"></a><span data-ttu-id="16fa7-135">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="16fa7-135">Configuration</span></span>

<span data-ttu-id="16fa7-136">Конфигурацию поставщика ведения журналов предоставляет как минимум один поставщик конфигураций:</span><span class="sxs-lookup"><span data-stu-id="16fa7-136">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="16fa7-137">Форматы файлов (INI, JSON и XML).</span><span class="sxs-lookup"><span data-stu-id="16fa7-137">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="16fa7-138">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="16fa7-138">Command-line arguments.</span></span>
* <span data-ttu-id="16fa7-139">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="16fa7-139">Environment variables.</span></span>
* <span data-ttu-id="16fa7-140">Объекты .NET в памяти.</span><span class="sxs-lookup"><span data-stu-id="16fa7-140">In-memory .NET objects.</span></span>
* <span data-ttu-id="16fa7-141">Незашифрованное хранилище [Secret Manager](xref:security/app-secrets) (Диспетчер секретов).</span><span class="sxs-lookup"><span data-stu-id="16fa7-141">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="16fa7-142">Зашифрованное пользовательское хранилище, например [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="16fa7-142">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="16fa7-143">Пользовательские поставщики (установленные или созданные).</span><span class="sxs-lookup"><span data-stu-id="16fa7-143">Custom providers (installed or created).</span></span>

<span data-ttu-id="16fa7-144">Например, конфигурацию ведения журналов обычно предоставляет раздел `Logging` в файле параметров приложения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-144">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="16fa7-145">В следующем примере показано содержимое типичного файла *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="16fa7-145">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="16fa7-146">Ключи `LogLevel` представляют имена журналов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-146">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="16fa7-147">Ключ `Default` применяется к журналам, которые не указаны явно.</span><span class="sxs-lookup"><span data-stu-id="16fa7-147">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="16fa7-148">Значение представляет [уровень ведения журнала](#log-level), который применен к указанному журналу.</span><span class="sxs-lookup"><span data-stu-id="16fa7-148">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="16fa7-149">Ключи журнала, которые задают `IncludeScopes` (в примере — `Console`), следует указывать, если [области ведения журнала](#log-scopes) включены для указанного журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-149">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="16fa7-150">Ключи `LogLevel` представляют имена журналов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-150">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="16fa7-151">Ключ `Default` применяется к журналам, которые не указаны явно.</span><span class="sxs-lookup"><span data-stu-id="16fa7-151">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="16fa7-152">Значение представляет [уровень ведения журнала](#log-level), который применен к указанному журналу.</span><span class="sxs-lookup"><span data-stu-id="16fa7-152">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="16fa7-153">Сведения о реализации поставщиков конфигураций см. в статье <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="16fa7-153">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="16fa7-154">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="16fa7-154">Sample logging output</span></span>

<span data-ttu-id="16fa7-155">Используя пример кода из предыдущего раздела, вы увидите журналы в консоли после запуска из командной строки.</span><span class="sxs-lookup"><span data-stu-id="16fa7-155">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="16fa7-156">Ниже приведен пример выходных данных консоли:</span><span class="sxs-lookup"><span data-stu-id="16fa7-156">Here's an example of console output:</span></span>

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

<span data-ttu-id="16fa7-157">Эти журналы были созданы при переходе по адресу `http://localhost:5000/api/todo/0`, который запускает выполнение обоих вызовов `ILogger`, приведенных в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="16fa7-157">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="16fa7-158">Ниже приведен пример тех же журналов в том виде, как они отображаются в окне отладки при запуске примера приложения в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="16fa7-158">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="16fa7-159">Журналы, которые были созданы путем вызовов `ILogger`, приведенных в предыдущем разделе, начинаются с "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="16fa7-159">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="16fa7-160">Журналы, которые начинаются с категорий "Microsoft", входят в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16fa7-160">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="16fa7-161">В ASP.NET Core и коде приложения используется один и тот же API ведения журнала и одни и те же поставщики.</span><span class="sxs-lookup"><span data-stu-id="16fa7-161">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="16fa7-162">В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-162">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="16fa7-163">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="16fa7-163">NuGet packages</span></span>

<span data-ttu-id="16fa7-164">Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="16fa7-164">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="16fa7-165">Категория журнала</span><span class="sxs-lookup"><span data-stu-id="16fa7-165">Log category</span></span>

<span data-ttu-id="16fa7-166">*Категория* входит в состав каждого создаваемого журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-166">A *category* is included with each log that you create.</span></span> <span data-ttu-id="16fa7-167">Категория указывается при создании объекта `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-167">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="16fa7-168">Категория может быть любой строкой, однако действует соглашение по использованию полного имени класса, из которого записываются журналы.</span><span class="sxs-lookup"><span data-stu-id="16fa7-168">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="16fa7-169">Например: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="16fa7-169">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="16fa7-170">Можно указать категорию в виде строки или использовать метод расширения, который наследует категорию от типа.</span><span class="sxs-lookup"><span data-stu-id="16fa7-170">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="16fa7-171">Чтобы указать категорию как строку, вызовите `CreateLogger` в экземпляре `ILoggerFactory`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="16fa7-171">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="16fa7-172">В большинстве случаев будет проще использовать `ILogger<T>`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="16fa7-172">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="16fa7-173">Это эквивалентно вызову `CreateLogger` с полным именем типа `T`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-173">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="16fa7-174">Уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="16fa7-174">Log level</span></span>

<span data-ttu-id="16fa7-175">Каждый раз при создании журнала указывается его значение перечисления [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="16fa7-175">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="16fa7-176">Уровень ведения журнала означает степень серьезности или важности.</span><span class="sxs-lookup"><span data-stu-id="16fa7-176">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="16fa7-177">Например, можно создать журнал `Information` при нормальном завершении метода, журнал `Warning`, если метод возвращает код 404, и журнал `Error` при перехвате непредвиденного исключения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-177">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="16fa7-178">В следующем примере кода имена методов (например, `LogWarning`) указывают уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-178">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="16fa7-179">Первый параметр — это [ идентификатор события журнала](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="16fa7-179">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="16fa7-180">Второй параметр — это [шаблон сообщения](#log-message-template) с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода.</span><span class="sxs-lookup"><span data-stu-id="16fa7-180">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="16fa7-181">Параметры метода рассматриваются более подробно далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="16fa7-181">The method parameters are explained in more detail later in this article.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="16fa7-182">Методы ведения журнала, которые содержат уровень в имени метода, являются [методами расширения для ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="16fa7-182">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="16fa7-183">На самом деле эти методы вызывают метод `Log`, принимающий параметр `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-183">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="16fa7-184">Метод `Log`, в отличие от этих методов расширения, можно вызывать напрямую, однако его синтаксис довольно сложен.</span><span class="sxs-lookup"><span data-stu-id="16fa7-184">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="16fa7-185">Дополнительные сведения см. в разделе [Интерфейс ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) и [Исходный код расширений средства ведения журналов](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="16fa7-185">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="16fa7-186">В ASP.NET Core определяются следующие [уровни ведения журнала](/dotnet/api/microsoft.extensions.logging.loglevel), упорядоченные в этом документе от наименьшего до наибольшего уровня серьезности.</span><span class="sxs-lookup"><span data-stu-id="16fa7-186">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="16fa7-187">Трассировка = 0</span><span class="sxs-lookup"><span data-stu-id="16fa7-187">Trace = 0</span></span>

  <span data-ttu-id="16fa7-188">Для получения сведений, которые пригодятся только разработчикам при отладке.</span><span class="sxs-lookup"><span data-stu-id="16fa7-188">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="16fa7-189">Эти сообщения могут содержать конфиденциальные данные приложения, поэтому их не следует включать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="16fa7-189">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="16fa7-190">*По умолчанию отключено.*</span><span class="sxs-lookup"><span data-stu-id="16fa7-190">*Disabled by default.*</span></span> <span data-ttu-id="16fa7-191">Пример: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="16fa7-191">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="16fa7-192">Отладка = 1</span><span class="sxs-lookup"><span data-stu-id="16fa7-192">Debug = 1</span></span>

  <span data-ttu-id="16fa7-193">Для сведений, которые полезны в краткосрочной перспективе во время разработки и отладки.</span><span class="sxs-lookup"><span data-stu-id="16fa7-193">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="16fa7-194">Пример: `Entering method Configure with flag set to true.` как правило, в рабочей среде журналы уровня `Debug` применяются только при устранении неполадок, так как они занимают большой объем.</span><span class="sxs-lookup"><span data-stu-id="16fa7-194">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="16fa7-195">Информация = 2</span><span class="sxs-lookup"><span data-stu-id="16fa7-195">Information = 2</span></span>

  <span data-ttu-id="16fa7-196">Для отслеживания общего хода выполнения приложения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-196">For tracking the general flow of the application.</span></span> <span data-ttu-id="16fa7-197">Эти журналы обычно полезны в долгосрочной перспективе.</span><span class="sxs-lookup"><span data-stu-id="16fa7-197">These logs typically have some long-term value.</span></span> <span data-ttu-id="16fa7-198">Пример: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="16fa7-198">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="16fa7-199">Предупреждение = 3</span><span class="sxs-lookup"><span data-stu-id="16fa7-199">Warning = 3</span></span>

  <span data-ttu-id="16fa7-200">Для нестандартных или непредвиденных событий, возникающих при выполнении приложения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-200">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="16fa7-201">В их число могут входить ошибки или другие условия, которые не приводят к остановке приложения, но которые может потребоваться изучить.</span><span class="sxs-lookup"><span data-stu-id="16fa7-201">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="16fa7-202">Обработанные исключения являются распространенным условием использования уровня ведения журнала `Warning`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-202">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="16fa7-203">Пример: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="16fa7-203">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="16fa7-204">Ошибка = 4</span><span class="sxs-lookup"><span data-stu-id="16fa7-204">Error = 4</span></span>

  <span data-ttu-id="16fa7-205">Для ошибок и исключений, которые не могут быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="16fa7-205">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="16fa7-206">Эти сообщения указывают на сбой текущего действия или операции (например, текущего HTTP-запроса), а не ошибку уровня приложения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-206">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="16fa7-207">Пример сообщения журнала: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="16fa7-207">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="16fa7-208">Критический = 5</span><span class="sxs-lookup"><span data-stu-id="16fa7-208">Critical = 5</span></span>

  <span data-ttu-id="16fa7-209">Для сбоев, которые требуют немедленного внимания.</span><span class="sxs-lookup"><span data-stu-id="16fa7-209">For failures that require immediate attention.</span></span> <span data-ttu-id="16fa7-210">Примеры: потеря данных, нехватка места на диске.</span><span class="sxs-lookup"><span data-stu-id="16fa7-210">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="16fa7-211">Уровень ведения журнала можно использовать для управления объемом выходных данных, записываемых на определенный носитель или в окно отображения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-211">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="16fa7-212">Например, в рабочей среде может потребоваться, чтобы все журналы уровня `Information` и ниже записывались в хранилище томов, а все журналы уровня `Warning` и выше записывались в хранилище значений.</span><span class="sxs-lookup"><span data-stu-id="16fa7-212">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="16fa7-213">Во время разработки можно отправлять журналы `Warning` или более высоких уровней серьезности на консоль.</span><span class="sxs-lookup"><span data-stu-id="16fa7-213">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="16fa7-214">Затем, если потребуется устранить неполадки, можно добавить уровень `Debug`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-214">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="16fa7-215">В разделе [Фильтрация журналов](#log-filtering) далее в этой статье приводятся сведения о том, как контролировать уровни журнала, обрабатываемые поставщиком.</span><span class="sxs-lookup"><span data-stu-id="16fa7-215">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="16fa7-216">Платформа ASP.NET Core создает журналы уровня `Debug` для событий платформы.</span><span class="sxs-lookup"><span data-stu-id="16fa7-216">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="16fa7-217">В примерах журнала ранее в этой статье не указывались журналы с уровнем ниже `Information`, поэтому журналы уровня `Debug` не отображались.</span><span class="sxs-lookup"><span data-stu-id="16fa7-217">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="16fa7-218">Ниже приведен пример журналов консоли при запуске примера приложения, настроенного для отображения журналов уровня `Debug` и выше для поставщика Console.</span><span class="sxs-lookup"><span data-stu-id="16fa7-218">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="16fa7-219">Идентификатор события журнала</span><span class="sxs-lookup"><span data-stu-id="16fa7-219">Log event ID</span></span>

<span data-ttu-id="16fa7-220">При каждом создании журнала можно указывать *идентификатор события*.</span><span class="sxs-lookup"><span data-stu-id="16fa7-220">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="16fa7-221">В примере приложения для этого используется локально определенный класс `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="16fa7-221">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="16fa7-222">Идентификатор события — это целочисленное значение, используемое для связи набора зарегистрированных событий друг с другом.</span><span class="sxs-lookup"><span data-stu-id="16fa7-222">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="16fa7-223">Например, журнал для добавления позиции в корзину покупок может иметь идентификатор события 1000, а журнал для завершения покупки может иметь идентификатор события 1001.</span><span class="sxs-lookup"><span data-stu-id="16fa7-223">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="16fa7-224">В зависимости от поставщика идентификатор события может быть сохранен в поле или включен в текст сообщения в выходных данных журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-224">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="16fa7-225">Поставщик Debug не отображает идентификаторы событий, но поставщик Console показывает их в квадратных скобках после категории:</span><span class="sxs-lookup"><span data-stu-id="16fa7-225">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="16fa7-226">Шаблон сообщения журнала</span><span class="sxs-lookup"><span data-stu-id="16fa7-226">Log message template</span></span>

<span data-ttu-id="16fa7-227">Каждый раз при записи сообщения журнала предоставляется шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-227">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="16fa7-228">Шаблон сообщения может быть строкой либо содержать именованные заполнители, в которые помещаются значения аргументов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-228">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="16fa7-229">Шаблон не является строкой формата, а заполнители должны быть именованными, а не нумерованными.</span><span class="sxs-lookup"><span data-stu-id="16fa7-229">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="16fa7-230">Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами.</span><span class="sxs-lookup"><span data-stu-id="16fa7-230">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="16fa7-231">Предположим, что имеется следующий код:</span><span class="sxs-lookup"><span data-stu-id="16fa7-231">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="16fa7-232">Полученное в результате сообщение журнала выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="16fa7-232">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="16fa7-233">Платформа ведения журнала поддерживает такое форматирование сообщений, чтобы поставщики ведения журналов могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журнала](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="16fa7-233">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="16fa7-234">Так как в систему ведения журналов передаются сами аргументы, а не только отформатированный шаблон сообщения, поставщики ведения журналов могут сохранять значения параметров как поля (наряду с шаблоном сообщения).</span><span class="sxs-lookup"><span data-stu-id="16fa7-234">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="16fa7-235">Если выходные данных журналов направляются в хранилище таблиц Azure, вызов метода средства ведения журнала выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="16fa7-235">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="16fa7-236">Каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-236">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="16fa7-237">Можно находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-237">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="16fa7-238">Ведение журнала исключений</span><span class="sxs-lookup"><span data-stu-id="16fa7-238">Logging exceptions</span></span>

<span data-ttu-id="16fa7-239">Методы средства ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="16fa7-239">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="16fa7-240">Разные поставщики обрабатывают сведения об исключениях по-разному.</span><span class="sxs-lookup"><span data-stu-id="16fa7-240">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="16fa7-241">Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="16fa7-241">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="16fa7-242">Фильтрация журналов</span><span class="sxs-lookup"><span data-stu-id="16fa7-242">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="16fa7-243">Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий.</span><span class="sxs-lookup"><span data-stu-id="16fa7-243">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="16fa7-244">Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются.</span><span class="sxs-lookup"><span data-stu-id="16fa7-244">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="16fa7-245">Чтобы запретить все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-245">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="16fa7-246">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="16fa7-246">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="16fa7-247">Создание правил фильтрации в конфигурации</span><span class="sxs-lookup"><span data-stu-id="16fa7-247">Create filter rules in configuration</span></span>

<span data-ttu-id="16fa7-248">Шаблоны проектов создают код, который вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug.</span><span class="sxs-lookup"><span data-stu-id="16fa7-248">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="16fa7-249">Метод `CreateDefaultBuilder` также настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, используя код, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="16fa7-249">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="16fa7-250">Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="16fa7-250">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="16fa7-251">Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно, которое применяется ко всем поставщикам.</span><span class="sxs-lookup"><span data-stu-id="16fa7-251">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="16fa7-252">Далее вы увидите, как при создании объекта `ILogger` для каждого поставщика выбирается лишь одно из этих правил.</span><span class="sxs-lookup"><span data-stu-id="16fa7-252">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="16fa7-253">Правила фильтрации в коде</span><span class="sxs-lookup"><span data-stu-id="16fa7-253">Filter rules in code</span></span>

<span data-ttu-id="16fa7-254">Можно зарегистрировать правила фильтрации в коде, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="16fa7-254">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="16fa7-255">Второй `AddFilter` указывает поставщика, Debug, используя имя его типа.</span><span class="sxs-lookup"><span data-stu-id="16fa7-255">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="16fa7-256">Первый `AddFilter` применяется ко всем поставщикам, так как он не определяет тип поставщика.</span><span class="sxs-lookup"><span data-stu-id="16fa7-256">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="16fa7-257">Применение правил фильтрации</span><span class="sxs-lookup"><span data-stu-id="16fa7-257">How filtering rules are applied</span></span>

<span data-ttu-id="16fa7-258">Данные конфигурации и код `AddFilter`, приведенный в предыдущих примерах, создают правила, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="16fa7-258">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="16fa7-259">Первые шесть взяты из примера конфигурации, а последние два — из примера кода.</span><span class="sxs-lookup"><span data-stu-id="16fa7-259">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="16fa7-260">Число</span><span class="sxs-lookup"><span data-stu-id="16fa7-260">Number</span></span> | <span data-ttu-id="16fa7-261">Поставщик</span><span class="sxs-lookup"><span data-stu-id="16fa7-261">Provider</span></span>      | <span data-ttu-id="16fa7-262">Категории, которые начинаются с...</span><span class="sxs-lookup"><span data-stu-id="16fa7-262">Categories that begin with ...</span></span>          | <span data-ttu-id="16fa7-263">Минимальный уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="16fa7-263">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="16fa7-264">1</span><span class="sxs-lookup"><span data-stu-id="16fa7-264">1</span></span>      | <span data-ttu-id="16fa7-265">Отладка</span><span class="sxs-lookup"><span data-stu-id="16fa7-265">Debug</span></span>         | <span data-ttu-id="16fa7-266">Все категории</span><span class="sxs-lookup"><span data-stu-id="16fa7-266">All categories</span></span>                          | <span data-ttu-id="16fa7-267">Сведения</span><span class="sxs-lookup"><span data-stu-id="16fa7-267">Information</span></span>       |
| <span data-ttu-id="16fa7-268">2</span><span class="sxs-lookup"><span data-stu-id="16fa7-268">2</span></span>      | <span data-ttu-id="16fa7-269">Консоль</span><span class="sxs-lookup"><span data-stu-id="16fa7-269">Console</span></span>       | <span data-ttu-id="16fa7-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="16fa7-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="16fa7-271">Предупреждение</span><span class="sxs-lookup"><span data-stu-id="16fa7-271">Warning</span></span>           |
| <span data-ttu-id="16fa7-272">3</span><span class="sxs-lookup"><span data-stu-id="16fa7-272">3</span></span>      | <span data-ttu-id="16fa7-273">Консоль</span><span class="sxs-lookup"><span data-stu-id="16fa7-273">Console</span></span>       | <span data-ttu-id="16fa7-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="16fa7-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="16fa7-275">Отладка</span><span class="sxs-lookup"><span data-stu-id="16fa7-275">Debug</span></span>             |
| <span data-ttu-id="16fa7-276">4</span><span class="sxs-lookup"><span data-stu-id="16fa7-276">4</span></span>      | <span data-ttu-id="16fa7-277">Консоль</span><span class="sxs-lookup"><span data-stu-id="16fa7-277">Console</span></span>       | <span data-ttu-id="16fa7-278">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="16fa7-278">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="16fa7-279">Error</span><span class="sxs-lookup"><span data-stu-id="16fa7-279">Error</span></span>             |
| <span data-ttu-id="16fa7-280">5</span><span class="sxs-lookup"><span data-stu-id="16fa7-280">5</span></span>      | <span data-ttu-id="16fa7-281">Консоль</span><span class="sxs-lookup"><span data-stu-id="16fa7-281">Console</span></span>       | <span data-ttu-id="16fa7-282">Все категории</span><span class="sxs-lookup"><span data-stu-id="16fa7-282">All categories</span></span>                          | <span data-ttu-id="16fa7-283">Сведения</span><span class="sxs-lookup"><span data-stu-id="16fa7-283">Information</span></span>       |
| <span data-ttu-id="16fa7-284">6</span><span class="sxs-lookup"><span data-stu-id="16fa7-284">6</span></span>      | <span data-ttu-id="16fa7-285">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="16fa7-285">All providers</span></span> | <span data-ttu-id="16fa7-286">Все категории</span><span class="sxs-lookup"><span data-stu-id="16fa7-286">All categories</span></span>                          | <span data-ttu-id="16fa7-287">Отладка</span><span class="sxs-lookup"><span data-stu-id="16fa7-287">Debug</span></span>             |
| <span data-ttu-id="16fa7-288">7</span><span class="sxs-lookup"><span data-stu-id="16fa7-288">7</span></span>      | <span data-ttu-id="16fa7-289">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="16fa7-289">All providers</span></span> | <span data-ttu-id="16fa7-290">Система</span><span class="sxs-lookup"><span data-stu-id="16fa7-290">System</span></span>                                  | <span data-ttu-id="16fa7-291">Отладка</span><span class="sxs-lookup"><span data-stu-id="16fa7-291">Debug</span></span>             |
| <span data-ttu-id="16fa7-292">8</span><span class="sxs-lookup"><span data-stu-id="16fa7-292">8</span></span>      | <span data-ttu-id="16fa7-293">Отладка</span><span class="sxs-lookup"><span data-stu-id="16fa7-293">Debug</span></span>         | <span data-ttu-id="16fa7-294">Майкрософт</span><span class="sxs-lookup"><span data-stu-id="16fa7-294">Microsoft</span></span>                               | <span data-ttu-id="16fa7-295">Трассировка</span><span class="sxs-lookup"><span data-stu-id="16fa7-295">Trace</span></span>             |

<span data-ttu-id="16fa7-296">При создании объекта `ILogger`, используемого для создания журналов, объект `ILoggerFactory` выбирает одно правило для каждого поставщика, которое будет применено к этому средству ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-296">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="16fa7-297">Все сообщения, записываемые с помощью этого объекта `ILogger`, фильтруются на основе выбранных правил.</span><span class="sxs-lookup"><span data-stu-id="16fa7-297">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="16fa7-298">Самое подходящее правило для каждой пары поставщика и категории выбирается из списка доступных правил.</span><span class="sxs-lookup"><span data-stu-id="16fa7-298">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="16fa7-299">При создании `ILogger` для данной категории для каждого поставщика используется приведенный далее алгоритм:</span><span class="sxs-lookup"><span data-stu-id="16fa7-299">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="16fa7-300">Выберите все правила, которые соответствуют поставщику или его псевдониму.</span><span class="sxs-lookup"><span data-stu-id="16fa7-300">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="16fa7-301">Если ничего не найдено, выберите все правила с пустым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="16fa7-301">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="16fa7-302">В результатах предыдущего шага выберите правила с самым длинным соответствующим префиксом категории.</span><span class="sxs-lookup"><span data-stu-id="16fa7-302">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="16fa7-303">Если ничего не найдено, выберите все правила, которые не указывают категорию.</span><span class="sxs-lookup"><span data-stu-id="16fa7-303">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="16fa7-304">Если выбрано несколько правил, примите **последнее**.</span><span class="sxs-lookup"><span data-stu-id="16fa7-304">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="16fa7-305">Если правила не выбраны, используйте `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-305">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="16fa7-306">Предположим, что у вас есть указанный выше список правил и вы создаете объект `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="16fa7-306">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="16fa7-307">К поставщику Debug применяются правила 1, 6 и 8.</span><span class="sxs-lookup"><span data-stu-id="16fa7-307">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="16fa7-308">Правило 8 является наиболее подходящим, поэтому выбрано оно.</span><span class="sxs-lookup"><span data-stu-id="16fa7-308">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="16fa7-309">К поставщику отладки применяются правила 3, 4, 5 и 6.</span><span class="sxs-lookup"><span data-stu-id="16fa7-309">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="16fa7-310">Правило 3 является наиболее подходящим.</span><span class="sxs-lookup"><span data-stu-id="16fa7-310">Rule 3 is most specific.</span></span>

<span data-ttu-id="16fa7-311">При создании журналов с помощью `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" журналы уровня `Trace` и выше перейдут поставщику отладки, а журналы уровня `Debug` и выше перейдут поставщику консоли.</span><span class="sxs-lookup"><span data-stu-id="16fa7-311">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="16fa7-312">Псевдонимы поставщиков</span><span class="sxs-lookup"><span data-stu-id="16fa7-312">Provider aliases</span></span>

<span data-ttu-id="16fa7-313">Имя типа можно использовать, чтобы указать поставщик в конфигурации, но каждый поставщик имеет более короткий и простой в использовании *псевдоним*.</span><span class="sxs-lookup"><span data-stu-id="16fa7-313">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="16fa7-314">Для встроенных поставщиков используйте следующие псевдонимы:</span><span class="sxs-lookup"><span data-stu-id="16fa7-314">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="16fa7-315">Консоль</span><span class="sxs-lookup"><span data-stu-id="16fa7-315">Console</span></span>
* <span data-ttu-id="16fa7-316">Отладка</span><span class="sxs-lookup"><span data-stu-id="16fa7-316">Debug</span></span>
* <span data-ttu-id="16fa7-317">EventLog</span><span class="sxs-lookup"><span data-stu-id="16fa7-317">EventLog</span></span>
* <span data-ttu-id="16fa7-318">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="16fa7-318">AzureAppServices</span></span>
* <span data-ttu-id="16fa7-319">TraceSource</span><span class="sxs-lookup"><span data-stu-id="16fa7-319">TraceSource</span></span>
* <span data-ttu-id="16fa7-320">EventSource</span><span class="sxs-lookup"><span data-stu-id="16fa7-320">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="16fa7-321">Минимальный уровень по умолчанию</span><span class="sxs-lookup"><span data-stu-id="16fa7-321">Default minimum level</span></span>

<span data-ttu-id="16fa7-322">Существует параметр минимального уровня, который применяется, только если к определенному поставщику или категории не подходят правила из конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="16fa7-322">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="16fa7-323">В следующем примере показано, как задать минимальный уровень:</span><span class="sxs-lookup"><span data-stu-id="16fa7-323">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="16fa7-324">Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.</span><span class="sxs-lookup"><span data-stu-id="16fa7-324">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="16fa7-325">Функции фильтрации</span><span class="sxs-lookup"><span data-stu-id="16fa7-325">Filter functions</span></span>

<span data-ttu-id="16fa7-326">В функции фильтрации можно написать код для применения правил фильтрации.</span><span class="sxs-lookup"><span data-stu-id="16fa7-326">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="16fa7-327">Функция фильтрации вызывается для всех поставщиков и категорий, которым в конфигурации и (или) коде не назначены правила.</span><span class="sxs-lookup"><span data-stu-id="16fa7-327">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="16fa7-328">Код в функции имеет доступ к типу поставщика, категории и уровню ведения журнала, чтобы определить необходимость регистрации сообщений.</span><span class="sxs-lookup"><span data-stu-id="16fa7-328">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="16fa7-329">Пример:</span><span class="sxs-lookup"><span data-stu-id="16fa7-329">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="16fa7-330">Некоторые поставщики ведения журналов позволяют указывать, когда журналы следует записывать на носитель, а когда — игнорировать. При этом учитывается уровень ведения журнала и категория.</span><span class="sxs-lookup"><span data-stu-id="16fa7-330">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="16fa7-331">Методы расширения `AddConsole` и `AddDebug`предоставляют перегрузки для передачи условий фильтрации.</span><span class="sxs-lookup"><span data-stu-id="16fa7-331">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="16fa7-332">В следующем примере кода поставщик Console игнорирует журналы с уровнем ниже `Warning`, а поставщик Debug не учитывает журналы, создаваемые платформой.</span><span class="sxs-lookup"><span data-stu-id="16fa7-332">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="16fa7-333">Метод `AddEventLog` имеет перегрузку, принимающую экземпляр `EventLogSettings`, который в своем свойстве `Filter` может содержать функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="16fa7-333">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="16fa7-334">Поставщик TraceSource не предоставляет эти перегрузки, так как уровень ведения журнала и другие параметры определяются для него на основе используемых `SourceSwitch` и `TraceListener`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-334">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="16fa7-335">Можно задать правила фильтрации для всех поставщиков, которые зарегистрированы в экземпляре `ILoggerFactory`, с помощью метода расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-335">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="16fa7-336">В приведенном ниже примере журналы платформы (категория начинается с "Microsoft" или "System") ограничиваются предупреждениями, однако разрешаются журналы приложений на уровне отладки.</span><span class="sxs-lookup"><span data-stu-id="16fa7-336">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="16fa7-337">Чтобы использовать фильтрацию для предотвращения создания всех журналов для определенной категории, укажите `LogLevel.None` в качестве минимального уровня ведения журнала для этой категории.</span><span class="sxs-lookup"><span data-stu-id="16fa7-337">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="16fa7-338">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="16fa7-338">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="16fa7-339">Пакет NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) предоставляет метод расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-339">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="16fa7-340">Метод возвращает новый экземпляр `ILoggerFactory` для фильтрации журнала сообщений, переданного всем поставщикам средства ведения журналов, которые зарегистрированы в этом экземпляре.</span><span class="sxs-lookup"><span data-stu-id="16fa7-340">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="16fa7-341">Он не влияет на другие экземпляры `ILoggerFactory`, включая исходный экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-341">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="16fa7-342">Области журналов</span><span class="sxs-lookup"><span data-stu-id="16fa7-342">Log scopes</span></span>

<span data-ttu-id="16fa7-343">Для присоединения одних и тех же данных к каждому журналу, созданному в рамках набора, можно сгруппировать набор логических операций в *область*.</span><span class="sxs-lookup"><span data-stu-id="16fa7-343">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="16fa7-344">Например, может потребоваться, чтобы каждый журнал, созданный в ходе обработки транзакции, содержал идентификатор транзакции.</span><span class="sxs-lookup"><span data-stu-id="16fa7-344">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="16fa7-345">Область — это тип `IDisposable`, возвращаемый методом [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) и действующий до удаления.</span><span class="sxs-lookup"><span data-stu-id="16fa7-345">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="16fa7-346">Чтобы использовать область, следует заключить вызовы средства ведения журналов в блок `using`, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="16fa7-346">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="16fa7-347">Следующий код предоставляет области для поставщика Console:</span><span class="sxs-lookup"><span data-stu-id="16fa7-347">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="16fa7-348">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="16fa7-348">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="16fa7-349">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-349">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="16fa7-350">Сведения о настройках см. в разделе [Конфигурация](#configuration).</span><span class="sxs-lookup"><span data-stu-id="16fa7-350">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="16fa7-351">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="16fa7-351">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="16fa7-352">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-352">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="16fa7-353">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="16fa7-353">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="16fa7-354">Каждое сообщение журнала содержит ограниченную информацию:</span><span class="sxs-lookup"><span data-stu-id="16fa7-354">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="16fa7-355">Встроенные поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="16fa7-355">Built-in logging providers</span></span>

<span data-ttu-id="16fa7-356">В состав ASP.NET Core входят следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="16fa7-356">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="16fa7-357">Консоль</span><span class="sxs-lookup"><span data-stu-id="16fa7-357">Console</span></span>](#console-provider)
* [<span data-ttu-id="16fa7-358">Отладка</span><span class="sxs-lookup"><span data-stu-id="16fa7-358">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="16fa7-359">EventSource</span><span class="sxs-lookup"><span data-stu-id="16fa7-359">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="16fa7-360">EventLog</span><span class="sxs-lookup"><span data-stu-id="16fa7-360">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="16fa7-361">TraceSource</span><span class="sxs-lookup"><span data-stu-id="16fa7-361">TraceSource</span></span>](#tracesource-provider)
* <span data-ttu-id="16fa7-362">[служба приложений Azure](#azure-app-service-provider);</span><span class="sxs-lookup"><span data-stu-id="16fa7-362">[Azure App Service](#azure-app-service-provider)</span></span>

### <a name="console-provider"></a><span data-ttu-id="16fa7-363">Поставщик Console</span><span class="sxs-lookup"><span data-stu-id="16fa7-363">Console provider</span></span>

<span data-ttu-id="16fa7-364">Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль.</span><span class="sxs-lookup"><span data-stu-id="16fa7-364">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="16fa7-365">[Перегрузки AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) позволяют передавать минимальный уровень ведения журнала, функцию фильтрации и логическое значение, указывающее, поддерживаются ли области.</span><span class="sxs-lookup"><span data-stu-id="16fa7-365">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="16fa7-366">Другим вариантом является передача объекта `IConfiguration`, который может указать поддержку областей и уровни ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-366">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="16fa7-367">Если вы собираетесь использовать поставщик Console в рабочей среде, имейте в виду, что он оказывает значительное влияние на производительность.</span><span class="sxs-lookup"><span data-stu-id="16fa7-367">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="16fa7-368">При создании проекта в Visual Studio метод `AddConsole` выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="16fa7-368">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="16fa7-369">Этот код ссылается на раздел `Logging` файла *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="16fa7-369">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="16fa7-370">Приведенные параметры ограничивают журналы платформы предупреждениями, но разрешают регистрацию приложений на уровне отладки, как описано в разделе [Фильтрация журналов](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="16fa7-370">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="16fa7-371">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="16fa7-371">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="16fa7-372">Поставщик Debug</span><span class="sxs-lookup"><span data-stu-id="16fa7-372">Debug provider</span></span>

<span data-ttu-id="16fa7-373">Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (вызовов метода `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="16fa7-373">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="16fa7-374">В Linux этот поставщик записывает журналы в каталог */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="16fa7-374">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="16fa7-375">[Перегрузки AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) позволяют передавать минимальный уровень ведения журнала или функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="16fa7-375">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="16fa7-376">Поставщик EventSource</span><span class="sxs-lookup"><span data-stu-id="16fa7-376">EventSource provider</span></span>

<span data-ttu-id="16fa7-377">Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздней версии, реализовывать события трассировки можно с помощью пакета поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource).</span><span class="sxs-lookup"><span data-stu-id="16fa7-377">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="16fa7-378">В Windows используется [трассировка событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="16fa7-378">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="16fa7-379">Поставщик является кроссплатформенным, но для Linux и macOS инструменты сбора событий и отображений пока отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="16fa7-379">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="16fa7-380">Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="16fa7-380">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="16fa7-381">Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="16fa7-381">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="16fa7-382">Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**.</span><span class="sxs-lookup"><span data-stu-id="16fa7-382">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="16fa7-383">(Не пропустите звездочку в начале строки.)</span><span class="sxs-lookup"><span data-stu-id="16fa7-383">(Don't miss the asterisk at the start of the string.)</span></span>

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="16fa7-385">Поставщик Windows EventLog</span><span class="sxs-lookup"><span data-stu-id="16fa7-385">Windows EventLog provider</span></span>

<span data-ttu-id="16fa7-386">Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="16fa7-386">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="16fa7-387">[Перегрузки AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) позволяют передавать `EventLogSettings` или минимальный уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="16fa7-387">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="16fa7-388">Поставщик TraceSource</span><span class="sxs-lookup"><span data-stu-id="16fa7-388">TraceSource provider</span></span>

<span data-ttu-id="16fa7-389">Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="16fa7-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="16fa7-390">[Перегрузки AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) позволяют передавать исходный параметр и прослушиватель трассировки.</span><span class="sxs-lookup"><span data-stu-id="16fa7-390">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="16fa7-391">Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core).</span><span class="sxs-lookup"><span data-stu-id="16fa7-391">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="16fa7-392">Поставщик позволяет перенаправлять сообщения различным [прослушивателям](/dotnet/framework/debug-trace-profile/trace-listeners), например [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr), который используется в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-392">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="16fa7-393">В следующем примере показана настройка поставщика `TraceSource`, записывающего в окне консоли сообщения типа `Warning` и сообщения с более высоким уровнем серьезности.</span><span class="sxs-lookup"><span data-stu-id="16fa7-393">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="16fa7-394">Поставщик службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="16fa7-394">Azure App Service provider</span></span>

<span data-ttu-id="16fa7-395">Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure.</span><span class="sxs-lookup"><span data-stu-id="16fa7-395">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="16fa7-396">Пакет поставщика доступен для приложений, предназначенных для .NET Core 1.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="16fa7-396">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="16fa7-397">Если планируется использовать .NET Core, обратите внимание на следующее.</span><span class="sxs-lookup"><span data-stu-id="16fa7-397">If targeting .NET Core, note the following points:</span></span>

* <span data-ttu-id="16fa7-398">Пакет поставщика включен в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ASP.NET Core, но не включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="16fa7-398">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) but not in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="16fa7-399">Не выполняйте явный вызов [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="16fa7-399">Don't explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="16fa7-400">Поставщик становится автоматически доступным для приложения при развертывании приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="16fa7-400">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="16fa7-401">Если планируется использовать .NET Framework или будет указана ссылка на метапакет `Microsoft.AspNetCore.App`, добавьте пакет поставщика в проект.</span><span class="sxs-lookup"><span data-stu-id="16fa7-401">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="16fa7-402">Вызовите `AddAzureWebAppDiagnostics` в экземпляре [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory).</span><span class="sxs-lookup"><span data-stu-id="16fa7-402">Invoke `AddAzureWebAppDiagnostics` on an [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) instance:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

<span data-ttu-id="16fa7-403">Перегрузка [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) позволяет передавать [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) для переопределения параметров по умолчанию, таких как шаблон выходных данных ведения журнала, имя BLOB-объекта и предельный размер файла.</span><span class="sxs-lookup"><span data-stu-id="16fa7-403">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="16fa7-404">(*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам на основе того, который вы указали при вызове метода `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="16fa7-404">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

<span data-ttu-id="16fa7-405">При развертывании в приложение службы приложений приложение учитывает параметры в разделе [Журналы диагностики](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="16fa7-405">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="16fa7-406">При обновлении этих параметров изменения вступают в силу немедленно без перезапуска или повторного развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-406">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Параметры ведения журнала Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="16fa7-408">По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="16fa7-408">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="16fa7-409">Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2.</span><span class="sxs-lookup"><span data-stu-id="16fa7-409">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="16fa7-410">Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="16fa7-410">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="16fa7-411">Дополнительные сведения о поведении по умолчанию см. в разделе [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="16fa7-411">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="16fa7-412">Поставщик работает только при выполнении проекта в среде Azure.</span><span class="sxs-lookup"><span data-stu-id="16fa7-412">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="16fa7-413">Он не функционирует при запуске проекта в локальной среде &mdash;(то есть не выполняет запись в локальные файлы или в локальное хранилище разработки для больших двоичных объектов).</span><span class="sxs-lookup"><span data-stu-id="16fa7-413">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="16fa7-414">Сторонние поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="16fa7-414">Third-party logging providers</span></span>

<span data-ttu-id="16fa7-415">Некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="16fa7-415">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="16fa7-416">[ELMAH.IO](https://elmah.io/) ([в репозитории GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="16fa7-416">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="16fa7-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([в репозитории GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="16fa7-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="16fa7-418">[JSNLog](http://jsnlog.com/) ([в репозитории GitHub](https://github.com/mperdeck/jsnlog));</span><span class="sxs-lookup"><span data-stu-id="16fa7-418">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="16fa7-419">[KissLog.net](https://kisslog.net/) ([репозиторий GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="16fa7-419">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="16fa7-420">[Loggr](http://loggr.net/) ([в репозитории GitHub](https://github.com/imobile3/Loggr.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="16fa7-420">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="16fa7-421">[NLog](http://nlog-project.org/) ([в репозитории GitHub](https://github.com/NLog/NLog.Extensions.Logging));</span><span class="sxs-lookup"><span data-stu-id="16fa7-421">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="16fa7-422">[Sentry](https://sentry.io/welcome/) ([репозиторий GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="16fa7-422">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="16fa7-423">[Serilog](https://serilog.net/) ([в репозитории GitHub](https://github.com/serilog/serilog-extensions-logging)).</span><span class="sxs-lookup"><span data-stu-id="16fa7-423">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="16fa7-424">Некоторые сторонние платформы выполняют [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="16fa7-424">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="16fa7-425">Использование сторонней платформы аналогично использованию одного из встроенных поставщиков:</span><span class="sxs-lookup"><span data-stu-id="16fa7-425">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="16fa7-426">Добавьте пакет NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="16fa7-426">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="16fa7-427">Вызовите метод расширения в `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-427">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="16fa7-428">Дополнительные сведения см. в документации по каждой платформе.</span><span class="sxs-lookup"><span data-stu-id="16fa7-428">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="16fa7-429">Потоковая передача журналов Azure</span><span class="sxs-lookup"><span data-stu-id="16fa7-429">Azure log streaming</span></span>

<span data-ttu-id="16fa7-430">Потоковая передача журналов Azure позволяет просматривать активность журнала в режиме реального времени из следующих источников:</span><span class="sxs-lookup"><span data-stu-id="16fa7-430">Azure log streaming enables you to view log activity in real time from:</span></span>

* <span data-ttu-id="16fa7-431">сервер приложений;</span><span class="sxs-lookup"><span data-stu-id="16fa7-431">The application server</span></span>
* <span data-ttu-id="16fa7-432">веб-сервер;</span><span class="sxs-lookup"><span data-stu-id="16fa7-432">The web server</span></span>
* <span data-ttu-id="16fa7-433">трассировка неудачно завершенных запросов.</span><span class="sxs-lookup"><span data-stu-id="16fa7-433">Failed request tracing</span></span>

<span data-ttu-id="16fa7-434">Настройка потоковой передачи журналов Azure</span><span class="sxs-lookup"><span data-stu-id="16fa7-434">To configure Azure log streaming:</span></span>

* <span data-ttu-id="16fa7-435">Со страницы портала приложения перейдите на страницу **Журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="16fa7-435">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="16fa7-436">Включите параметр **Ведение журнала приложения (файловая система)**.</span><span class="sxs-lookup"><span data-stu-id="16fa7-436">Set **Application Logging (Filesystem)** to on.</span></span>

![Страница журналов диагностики на портале Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="16fa7-438">Перейдите на страницу **Потоковая передача журналов**, чтобы просмотреть сообщения приложения.</span><span class="sxs-lookup"><span data-stu-id="16fa7-438">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="16fa7-439">Они передаются в приложении через интерфейс `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="16fa7-439">They're logged by application through the `ILogger` interface.</span></span>

![Потоковая передача журнала приложения на портале Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="16fa7-441">Ведение журнала трассировки Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="16fa7-441">Azure Application Insights trace logging</span></span>

<span data-ttu-id="16fa7-442">Пакет SDK [Application Insights](https://azure.microsoft.com/services/application-insights/) способен собирать телеметрию трассировки из журналов, сформированных инфраструктурой ведения журналов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16fa7-442">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="16fa7-443">Дополнительные сведения см. в разделе [Application Insights для ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) и на [вики-сайте Microsoft/ApplicationInsights-aspnetcore: ведение журнала](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="16fa7-443">For more information, see [Application Insights for ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) and [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16fa7-444">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="16fa7-444">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
