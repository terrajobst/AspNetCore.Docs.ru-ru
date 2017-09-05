---
title: "Размещение в ASP.NET Core | Документы Microsoft"
author: ardalis
description: "Общие сведения о веб-узлы в ASP.NET Core."
keywords: "ASP.NET Core, веб-узел, IWebHost"
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a><span data-ttu-id="9bd7c-104">Общие сведения о размещении в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9bd7c-104">Introduction to hosting in ASP.NET Core</span></span>

<span data-ttu-id="9bd7c-105">По [Стив Смит](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="9bd7c-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="9bd7c-106">Для запуска приложения ASP.NET Core, необходимо настроить и запустить узла с помощью `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-106">To run an ASP.NET Core app, you need to configure and launch a host using `WebHostBuilder`.</span></span>

## <a name="what-is-a-host"></a><span data-ttu-id="9bd7c-107">Что такое узел?</span><span class="sxs-lookup"><span data-stu-id="9bd7c-107">What is a Host?</span></span>

<span data-ttu-id="9bd7c-108">Приложения ASP.NET Core требуется *узла* в которой выполняется.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-108">ASP.NET Core apps require a *host* in which to execute.</span></span> <span data-ttu-id="9bd7c-109">Узел должен реализовать `IWebHost` интерфейс, который предоставляет доступ к коллекции компонентов и служб, и `Start` метод.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-109">A host must implement the `IWebHost` interface, which exposes collections of features and services, and a `Start` method.</span></span> <span data-ttu-id="9bd7c-110">Узел обычно создается с использованием экземпляра `WebHostBuilder`, который создает и возвращает `WebHost` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-110">The host is typically created using an instance of a `WebHostBuilder`, which builds and returns a  `WebHost` instance.</span></span> <span data-ttu-id="9bd7c-111">`WebHost` Ссылается на сервер, который будет обрабатывать запросы.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-111">The `WebHost` references the server that will handle requests.</span></span> <span data-ttu-id="9bd7c-112">Дополнительные сведения о [серверов](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-112">Learn more about [servers](servers/index.md).</span></span>

### <a name="what-is-the-difference-between-a-host-and-a-server"></a><span data-ttu-id="9bd7c-113">Какова разница между узлом и сервером?</span><span class="sxs-lookup"><span data-stu-id="9bd7c-113">What is the difference between a host and a server?</span></span>

<span data-ttu-id="9bd7c-114">Основное приложение отвечает за управление запуском и временем существования приложения.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-114">The host is responsible for application startup and lifetime management.</span></span> <span data-ttu-id="9bd7c-115">Сервер отвечает за принятие HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-115">The server is responsible for accepting HTTP requests.</span></span> <span data-ttu-id="9bd7c-116">Часть ответственности узла включает в том, что службы приложения и сервер доступен и правильно настроен.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-116">Part of the host's responsibility includes ensuring the application's services and the server are available and properly configured.</span></span> <span data-ttu-id="9bd7c-117">Можно рассматривать как оболочка сервера узла.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-117">You can think of the host as being a wrapper around the server.</span></span> <span data-ttu-id="9bd7c-118">Узел настроен на использование конкретного сервера; Поэтому серверу оно неизвестно его узла.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-118">The host is configured to use a particular server; the server is unaware of its host.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="9bd7c-119">Настройка узла</span><span class="sxs-lookup"><span data-stu-id="9bd7c-119">Setting up a Host</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9bd7c-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9bd7c-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9bd7c-121">Создание узла с помощью экземпляра `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-121">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="9bd7c-122">Обычно это делается в точке входа приложения: `public static void Main` (который в шаблонах проектов находится в *Program.cs* файла).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-122">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="9bd7c-123">Типичный *Program.cs*, показанный ниже, демонстрирует использование `WebHostBuilder` для создания узла.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-123">A typical *Program.cs*, shown below, demonstrates how to use a `WebHostBuilder` to build a host.</span></span>

<span data-ttu-id="9bd7c-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span><span class="sxs-lookup"><span data-stu-id="9bd7c-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span></span>

<span data-ttu-id="9bd7c-125">`WebHostBuilder` Отвечает за создание узла, который будет начальной загрузки сервера для приложения.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-125">The `WebHostBuilder` is responsible for creating the host that will bootstrap the server for the app.</span></span> <span data-ttu-id="9bd7c-126">`WebHostBuilder`требуется указать сервер, который реализует `IServer` (`UseKestrel` в приведенном выше коде).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-126">`WebHostBuilder` requires you provide a server that implements `IServer` (`UseKestrel` in the code above).</span></span> <span data-ttu-id="9bd7c-127">`UseKestrel`Указывает, что сервер Kestrel будет использоваться приложением.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-127">`UseKestrel` specifies the Kestrel server will be used by the app.</span></span>

<span data-ttu-id="9bd7c-128">Сервер *содержимое корневого* определяет, где он выполняет поиск содержимого файлах, таких как представление MVC.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-128">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="9bd7c-129">Содержимое корневого по умолчанию — это папка, с которой выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-129">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="9bd7c-130">Указание `Directory.GetCurrentDirectory` как содержимое корневого использовать корневую папку веб-проект как корневой элемент содержимого приложения при запуске приложения из этой папки (например, вызов метода `dotnet run` из веб-папки проекта).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-130">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="9bd7c-131">Это значение по умолчанию, используемых в Visual Studio и `dotnet new` шаблонов.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-131">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="9bd7c-132">Чтобы использовать в качестве обратного прокси-сервера IIS, вызовите `UseIISIntegration` как часть построения узла.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-132">To use IIS as a reverse proxy, call `UseIISIntegration` as part of building the host.</span></span> 

<span data-ttu-id="9bd7c-133">Обратите внимание, что `UseIISIntegration` неправильно настраивает *сервера*, таких как `UseKestrel` does.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-133">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="9bd7c-134">Чтобы использовать IIS с ASP.NET Core, необходимо указать `UseKestrel` и `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-134">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="9bd7c-135">`UseKestrel`создает веб-сервер и размещено приложение.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-135">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="9bd7c-136">`UseIISIntegration`проверяет переменные среды, используемые IIS/IISExpress и настраивает параметры, такие как порт для прослушивания и заголовки для использования.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-136">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9bd7c-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9bd7c-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9bd7c-138">Создание узла с помощью экземпляра `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-138">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="9bd7c-139">Обычно это делается в точке входа приложения: `public static void Main` (который в шаблонах проектов находится в *Program.cs* файла).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-139">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="9bd7c-140">Типичный *Program.cs*описанных ниже вызовы `CreateDefaultbuilder` для создания узла:</span><span class="sxs-lookup"><span data-stu-id="9bd7c-140">A typical *Program.cs*, shown below, calls `CreateDefaultbuilder` to build a host:</span></span>

<span data-ttu-id="9bd7c-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="9bd7c-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span></span>

<span data-ttu-id="9bd7c-142">`CreateDefaultbuilder`Создает экземпляр `WebHostBuilder` для построения узел, который загружает сервера для приложения.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-142">`CreateDefaultbuilder` creates an instance of `WebHostBuilder` to build the host that bootstraps the server for the app.</span></span> <span data-ttu-id="9bd7c-143">Узлу требуется [сервера, который реализует IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-143">The host requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="9bd7c-144">Встроенные серверы являются [Kestrel](servers/kestrel.md) и [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` use Kestrel by default.</span></span>

<span data-ttu-id="9bd7c-145">`CreateDefaultbuilder`выполняет задачи настройки, помимо настройки Kestrel и веб-сервер:</span><span class="sxs-lookup"><span data-stu-id="9bd7c-145">`CreateDefaultbuilder` performs set-up tasks in addition to configuring Kestrel as the web server:</span></span>

* <span data-ttu-id="9bd7c-146">Задает содержимое корневого `Directory.GetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-146">Sets the content root to `Directory.GetCurrentDirectory`.</span></span>
* <span data-ttu-id="9bd7c-147">Загружает конфигурацию из:</span><span class="sxs-lookup"><span data-stu-id="9bd7c-147">Loads configuration from:</span></span>
  * <span data-ttu-id="9bd7c-148">*appSettings.JSON*</span><span class="sxs-lookup"><span data-stu-id="9bd7c-148">*appsettings.json*</span></span>
  * <span data-ttu-id="9bd7c-149">*appSettings. \<EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-149">*appsettings.\<EnvironmentName>.json*.</span></span>
  * <span data-ttu-id="9bd7c-150">секретные данные пользователя при запуске приложения в среде разработки</span><span class="sxs-lookup"><span data-stu-id="9bd7c-150">user secrets when the app runs in the Development environment</span></span>
  * <span data-ttu-id="9bd7c-151">переменные среды</span><span class="sxs-lookup"><span data-stu-id="9bd7c-151">environment variables</span></span>
  * <span data-ttu-id="9bd7c-152">args предоставленного командной строки</span><span class="sxs-lookup"><span data-stu-id="9bd7c-152">supplied command line args</span></span>
* <span data-ttu-id="9bd7c-153">Настраивает ведение журнала для выходных данных консоли и отладки с помощью фильтрации правила, указанные в разделе конфигурации ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-153">Configures logging for console and debug output, with filtering rules specified in a Logging configuration section.</span></span>
* <span data-ttu-id="9bd7c-154">Обеспечивает интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-154">Enables IIS integration.</span></span>
* <span data-ttu-id="9bd7c-155">Добавление страницы исключения разработчик, при запуске приложения в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-155">Adds the developer exception page when the app runs in the Development environment.</span></span>

<span data-ttu-id="9bd7c-156">Сервер *содержимое корневого* определяет, где он выполняет поиск содержимого файлах, таких как представление MVC.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-156">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="9bd7c-157">Содержимое корневого по умолчанию — это папка, с которой выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-157">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="9bd7c-158">Указание `Directory.GetCurrentDirectory` как содержимое корневого использовать корневую папку веб-проект как корневой элемент содержимого приложения при запуске приложения из этой папки (например, вызов метода `dotnet run` из веб-папки проекта).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-158">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="9bd7c-159">Это значение по умолчанию, используемых в Visual Studio и `dotnet new` шаблонов.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-159">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="9bd7c-160">При использовании служб IIS в качестве обратного прокси-сервера ASP.NET Core автоматически вызывает `UseIISIntegration` как часть построения узла.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-160">When you use IIS as a reverse proxy, ASP.NET Core automatically calls `UseIISIntegration` as part of building the host.</span></span> <span data-ttu-id="9bd7c-161">Дополнительные сведения см. в разделе [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-161">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="9bd7c-162">Обратите внимание, что `UseIISIntegration` неправильно настраивает *сервера*, таких как `UseKestrel` does.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-162">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="9bd7c-163">`UseKestrel`создает веб-сервер и размещено приложение.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-163">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="9bd7c-164">`UseIISIntegration`проверяет переменные среды, используемые IIS/IISExpress и настраивает параметры, такие как порт для прослушивания и заголовки для использования.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-164">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

---

<span data-ttu-id="9bd7c-165">Минимальная реализация настройки узла (и приложения ASP.NET Core) будет включать только сервера и настройки конвейера запросов приложения:</span><span class="sxs-lookup"><span data-stu-id="9bd7c-165">A minimal implementation of configuring a host (and an ASP.NET Core app) would include just a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> <span data-ttu-id="9bd7c-166">При настройке узла, чтобы обеспечить `Configure` и `ConfigureServices` методов вместо или в дополнение к определению `Startup` класса (которого необходимо определить эти методы - в разделе [запуска приложения](startup.md)).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-166">When setting up a host, you can provide `Configure` and `ConfigureServices` methods, instead of or in addition to specifying a `Startup` class (which must also define these methods - see [Application Startup](startup.md)).</span></span> <span data-ttu-id="9bd7c-167">Несколько вызовов `ConfigureServices` друг с другом, будут добавлены; вызовы `Configure` или `UseStartup` заменяет предыдущие параметры.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-167">Multiple calls to `ConfigureServices` will append to one another; calls to `Configure` or `UseStartup` will replace previous settings.</span></span>

## <a name="configuring-a-host"></a><span data-ttu-id="9bd7c-168">Настройка узла</span><span class="sxs-lookup"><span data-stu-id="9bd7c-168">Configuring a Host</span></span>

<span data-ttu-id="9bd7c-169">`WebHostBuilder` Предоставляет методы для настройки большинства значений конфигурации, доступные для узла, который также можно задать непосредственно с помощью `UseSetting` и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-169">The `WebHostBuilder` provides methods for setting most of the available configuration values for the host, which can also be set directly using `UseSetting` and associated key.</span></span>

### <a name="host-configuration-values"></a><span data-ttu-id="9bd7c-170">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="9bd7c-170">Host Configuration Values</span></span>

<span data-ttu-id="9bd7c-171">**Запись ошибки при загрузке**`bool`</span><span class="sxs-lookup"><span data-stu-id="9bd7c-171">**Capture Startup Errors** `bool`</span></span>

<span data-ttu-id="9bd7c-172">Ключ: `captureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-172">Key: `captureStartupErrors`.</span></span> <span data-ttu-id="9bd7c-173">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-173">Defaults to `false`.</span></span> <span data-ttu-id="9bd7c-174">Когда `false`, ошибки во время запуска результат в узле выхода.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-174">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="9bd7c-175">Когда `true`, узел соберет все исключения из `Startup` класса и попытки запуска сервера.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-175">When `true`, the host will capture any exceptions from the `Startup` class and attempt to start the server.</span></span> <span data-ttu-id="9bd7c-176">Он будет отображаться страница ошибки (универсальный или подробные, на основе параметра подробных сообщений об ошибках, ниже) для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-176">It will display an error page (generic, or detailed, based on the Detailed Errors setting, below) for every request.</span></span> <span data-ttu-id="9bd7c-177">Задать с помощью `CaptureStartupErrors` метод.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-177">Set using the `CaptureStartupErrors` method.</span></span>

<span data-ttu-id="9bd7c-178">Примечание: При запуске приложения с Kestrel и IIS по умолчанию выполняется для записи ошибок запуска.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-178">Note: When your app runs with Kestrel and IIS, the default behavior is to capture startup errors.</span></span> 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

<span data-ttu-id="9bd7c-179">**Содержимое корневой**`string`</span><span class="sxs-lookup"><span data-stu-id="9bd7c-179">**Content Root** `string`</span></span>

<span data-ttu-id="9bd7c-180">Ключ: `contentRoot`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-180">Key: `contentRoot`.</span></span> <span data-ttu-id="9bd7c-181">Значения по умолчанию в папку, в котором находится сборка приложения (для Kestrel; Службы IIS будут использовать корневой папке веб-проекта по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-181">Defaults to the folder where the application assembly resides (for Kestrel; IIS will use the web project root by default).</span></span> <span data-ttu-id="9bd7c-182">Этот параметр определяет, где ASP.NET Core начнется поиск файлов содержимого, такие как представления MVC.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-182">This setting determines where ASP.NET Core will begin searching for content files, such as MVC Views.</span></span> <span data-ttu-id="9bd7c-183">Также используется как базовый путь для [корневого веб-параметром](#web-root-setting).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-183">Also used as the base path for the [Web Root Setting](#web-root-setting).</span></span> <span data-ttu-id="9bd7c-184">Задать с помощью `UseContentRoot` метод.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-184">Set using the `UseContentRoot` method.</span></span> <span data-ttu-id="9bd7c-185">Путь должен существовать, или узел не смогут запуститься.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-185">Path must exist, or host will fail to start.</span></span>

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

<span data-ttu-id="9bd7c-186">**Подробных описаний ошибок**`bool`</span><span class="sxs-lookup"><span data-stu-id="9bd7c-186">**Detailed Errors** `bool`</span></span>

<span data-ttu-id="9bd7c-187">Ключ: `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-187">Key: `detailedErrors`.</span></span> <span data-ttu-id="9bd7c-188">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-188">Defaults to `false`.</span></span> <span data-ttu-id="9bd7c-189">При `true` (или если среды имеет значение «Разработка»), приложение будет отображать сведения о запуска исключения, а не просто универсальная страница ошибки.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-189">When `true` (or when Environment is set to "Development"), the app will display details of startup exceptions, instead of just a generic error page.</span></span> <span data-ttu-id="9bd7c-190">Задать с помощью `UseSetting`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-190">Set using `UseSetting`.</span></span>

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

<span data-ttu-id="9bd7c-191">Если подробные сообщения об ошибках равно `false` и отслеживания ошибок запуска `true`, отображается страница универсальное сообщение об ошибке в ответ на каждый запрос к серверу.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-191">When Detailed Errors is set to `false` and Capture Startup Errors is `true`, a generic error page is displayed in response to every request to the server.</span></span>

![Универсальная страница ошибки](hosting/_static/generic-error-page.png)

<span data-ttu-id="9bd7c-193">Если подробные сообщения об ошибках равно `true` и отслеживания ошибок запуска `true`, отображается страница подробные сведения об ошибке в ответ на каждый запрос к серверу.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-193">When Detailed Errors is set to `true` and Capture Startup Errors is `true`, a detailed error page is displayed in response to every request to the server.</span></span>

![Подробные сведения об ошибке страницы](hosting/_static/detailed-error-page.png)

<span data-ttu-id="9bd7c-195">**Среда**`string`</span><span class="sxs-lookup"><span data-stu-id="9bd7c-195">**Environment** `string`</span></span>

<span data-ttu-id="9bd7c-196">Ключ: `environment`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-196">Key: `environment`.</span></span> <span data-ttu-id="9bd7c-197">Значение по умолчанию «Production».</span><span class="sxs-lookup"><span data-stu-id="9bd7c-197">Defaults to "Production".</span></span> <span data-ttu-id="9bd7c-198">Может быть присвоено любое значение.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-198">May be set to any value.</span></span> <span data-ttu-id="9bd7c-199">Определяемые платформой значения включают «Разработка», «Промежуточный» и «Production».</span><span class="sxs-lookup"><span data-stu-id="9bd7c-199">Framework-defined values include "Development", "Staging", and "Production".</span></span> <span data-ttu-id="9bd7c-200">Значения не учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-200">Values are not case sensitive.</span></span> <span data-ttu-id="9bd7c-201">В разделе [работа с несколькими средами](environments.md).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-201">See [Working with Multiple Environments](environments.md).</span></span> <span data-ttu-id="9bd7c-202">Задать с помощью `UseEnvironment` метод.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-202">Set using the `UseEnvironment` method.</span></span>

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> <span data-ttu-id="9bd7c-203">По умолчанию среды считывается из `ASPNETCORE_ENVIRONMENT` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-203">By default, the environment is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="9bd7c-204">При использовании Visual Studio, переменные среды может быть задан в *launchSettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-204">When using Visual Studio, environment variables may be set in the *launchSettings.json* file.</span></span>

<a id="server-urls"></a>

<span data-ttu-id="9bd7c-205">**URL-адреса серверов**`string`</span><span class="sxs-lookup"><span data-stu-id="9bd7c-205">**Server URLs** `string`</span></span>

<span data-ttu-id="9bd7c-206">Ключ: `urls`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-206">Key: `urls`.</span></span> <span data-ttu-id="9bd7c-207">Задать как точку с запятой (;) запятыми список URL-адрес префиксов, для которого сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-207">Set to a semicolon (;) separated list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="9bd7c-208">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-208">For example, `http://localhost:123`.</span></span> <span data-ttu-id="9bd7c-209">Можно заменить имя домена и узла «\*» для указания сервер должен прослушивать запросы по любому IP-адресу или размещения, используя указанный порт и протокол (например, `http://*:5000` или `https://*:5001`).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-209">The domain/host name can be replaced with "\*" to indicate the server should listen to requests on any IP address or host using the specified port and protocol (for example, `http://*:5000` or `https://*:5001`).</span></span> <span data-ttu-id="9bd7c-210">Протокол (`http://` или `https://`) должен быть включен, все URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-210">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="9bd7c-211">Префиксы интерпретируются настроенный сервер; форматы, поддерживаемые будут различаться для разных серверов.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-211">The prefixes are interpreted by the configured server; supported formats will vary between servers.</span></span>

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="9bd7c-212">В ASP.NET Core 2.0 Kestrel имеет собственную конфигурацию конечной точки API и не поддерживает `https://` в `urls` строку.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-212">In ASP.NET Core 2.0, Kestrel has its own endpoint configuration API and does not support `https://` in the `urls` string.</span></span> <span data-ttu-id="9bd7c-213">Дополнительные сведения см. в разделе [введение в Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-213">For more information, see [Introduction to Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

<span data-ttu-id="9bd7c-214">**При запуске сборки**`string`</span><span class="sxs-lookup"><span data-stu-id="9bd7c-214">**Startup Assembly** `string`</span></span>

<span data-ttu-id="9bd7c-215">Ключ: `startupAssembly`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-215">Key: `startupAssembly`.</span></span> <span data-ttu-id="9bd7c-216">Определяет сборку для поиска `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-216">Determines the assembly to search for the `Startup` class.</span></span> <span data-ttu-id="9bd7c-217">Задать с помощью `UseStartup` метод.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-217">Set using the `UseStartup` method.</span></span> <span data-ttu-id="9bd7c-218">Вместо этого ссылаться с помощью определенного типа `WebHostBuilder.UseStartup<StartupType>`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-218">May instead reference specific type using `WebHostBuilder.UseStartup<StartupType>`.</span></span> <span data-ttu-id="9bd7c-219">При наличии нескольких `UseStartup` методы вызываются, приоритет имеет последняя.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-219">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

<span data-ttu-id="9bd7c-220">**Корневой веб-**`string`</span><span class="sxs-lookup"><span data-stu-id="9bd7c-220">**Web Root** `string`</span></span>

<span data-ttu-id="9bd7c-221">Ключ: `webroot`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-221">Key: `webroot`.</span></span> <span data-ttu-id="9bd7c-222">Если не указано значение по умолчанию — `(Content Root Path)\wwwroot`, если он существует.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-222">If not specified the default is `(Content Root Path)\wwwroot`, if it exists.</span></span> <span data-ttu-id="9bd7c-223">Если этот путь не существует, то используется поставщик холостой файла.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-223">If this path doesn't exist, then a no-op file provider is used.</span></span> <span data-ttu-id="9bd7c-224">Задать с помощью `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-224">Set using `UseWebRoot`.</span></span>

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a><span data-ttu-id="9bd7c-225">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="9bd7c-225">Overriding Configuration</span></span>

<span data-ttu-id="9bd7c-226">Используйте [конфигурации](configuration.md) для задания значений конфигурации, используемые узлом.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-226">Use [Configuration](configuration.md) to set configuration values to be used by the host.</span></span> <span data-ttu-id="9bd7c-227">Эти значения могут переопределяться впоследствии.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-227">These values may be subsequently overridden.</span></span> <span data-ttu-id="9bd7c-228">Этот параметр указан с помощью `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-228">This is specified using `UseConfiguration`.</span></span>

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

<span data-ttu-id="9bd7c-229">В приведенном выше примере аргументы командной строки может быть передан в настроить узел или параметры конфигурации при необходимости могут быть указаны в *hosting.json* файла.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-229">In the example above, command-line arguments may be passed in to configure the host, or configuration settings may optionally be specified in a *hosting.json* file.</span></span> <span data-ttu-id="9bd7c-230">Для указания выполнения на определенный URL-адрес узла, можно передавать в нужное значение из командной строки:</span><span class="sxs-lookup"><span data-stu-id="9bd7c-230">To specify the host run on a particular URL, you could pass in the desired value from a command prompt:</span></span>

```console
dotnet run --urls "http://*:5000"
```

<span data-ttu-id="9bd7c-231">`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-231">The `Run` method starts the web app and blocks the calling thread until the host is shutdown.</span></span>

```csharp
host.Run();
```

<span data-ttu-id="9bd7c-232">Узел без блокирования запускается путем вызова его `Start` метод:</span><span class="sxs-lookup"><span data-stu-id="9bd7c-232">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="9bd7c-233">Передайте список URL-адресов для `Start` метод и он будет прослушивать URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="9bd7c-233">Pass a list of URLs to the `Start` method and it will listen on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="9bd7c-234">Форматы URL-адресов, которые являются допустимыми здесь зависят от сервера, которую вы используете.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-234">The URL formats that are valid here depend on the server you're using.</span></span> <span data-ttu-id="9bd7c-235">Дополнительные сведения см. в разделе [URL-адреса серверов](#server-urls) ранее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-235">For more information, see [Server URLs](#server-urls) earlier in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="9bd7c-236">`UseConfiguration` Метода расширения не может в настоящее время синтаксического анализа раздела конфигурации, возвращенных `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-236">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="9bd7c-237">`GetSection` Метод фильтрует ключи в запрошенный раздел конфигурации, но оставляет имя раздела по ключам (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-237">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="9bd7c-238">`UseConfiguration` Метод ожидает ключи для сопоставления `WebHostBuilder` ключи (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-238">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="9bd7c-239">Имя раздела по ключам присутствия запрещает настраивать узла значения этого раздела.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-239">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="9bd7c-240">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-240">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="9bd7c-241">Дополнительные сведения и способы решения проблем см. в разделе [передачи раздела конфигурации в WebHostBuilder.UseConfiguration использует полное ключи](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-241">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="ordering-importance"></a><span data-ttu-id="9bd7c-242">Упорядочение важности</span><span class="sxs-lookup"><span data-stu-id="9bd7c-242">Ordering Importance</span></span>

<span data-ttu-id="9bd7c-243">`WebHostBuilder`Параметры сначала считываются из определенные переменные среды, если задать.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-243">`WebHostBuilder` settings are first read from certain environment variables, if set.</span></span> <span data-ttu-id="9bd7c-244">Эти переменные среды следует использовать формат `ASPNETCORE_{configurationKey}`, поэтому, например задать URL-адреса, сервер будет прослушивать по умолчанию, задайте `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-244">These environment variables must use the format `ASPNETCORE_{configurationKey}`, so for example to set the URLs the server will listen on by default, you would set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="9bd7c-245">Эти значения переменной среды можно переопределить, указав конфигурацию (с помощью `UseConfiguration`) или явно задать значение (с помощью `UseUrls` для экземпляра).</span><span class="sxs-lookup"><span data-stu-id="9bd7c-245">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseUrls` for instance).</span></span> <span data-ttu-id="9bd7c-246">Узел использует какой бы параметр задает значение последнего.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-246">The host will use whichever option sets the value last.</span></span> <span data-ttu-id="9bd7c-247">По этой причине `UseIISIntegration` должны располагаться после `UseUrls`, так как он заменяет URL-адрес с одним динамически, реализуемую IIS.</span><span class="sxs-lookup"><span data-stu-id="9bd7c-247">For this reason, `UseIISIntegration` must appear after `UseUrls`, because it replaces the URL with one dynamically provided by IIS.</span></span> <span data-ttu-id="9bd7c-248">Если требуется программно присвоено одно значение по умолчанию URL-адрес, однако возможность их переопределения с конфигурацией, можно настроить узел следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9bd7c-248">If you want to programmatically set the default URL to one value, but allow it to be overridden with configuration, you could configure the host as follows:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="9bd7c-249">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9bd7c-249">Additional resources</span></span>

* [<span data-ttu-id="9bd7c-250">Публикация в Windows с помощью служб IIS</span><span class="sxs-lookup"><span data-stu-id="9bd7c-250">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="9bd7c-251">Публикация в Linux с помощью Nginx</span><span class="sxs-lookup"><span data-stu-id="9bd7c-251">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="9bd7c-252">Публикация в Linux с помощью Apache</span><span class="sxs-lookup"><span data-stu-id="9bd7c-252">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="9bd7c-253">Узел в службе Windows</span><span class="sxs-lookup"><span data-stu-id="9bd7c-253">Host in a Windows Service</span></span>](xref:hosting/windows-service)

