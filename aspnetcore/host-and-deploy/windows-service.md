---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 68afe77b05a717cffecc32188f18e9fde208b81f
ms.sourcegitcommit: 3ca20ed63bf1469f4365f0c1fbd00c98a3191c84
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2018
ms.locfileid: "41755608"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="b1499-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="b1499-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="b1499-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)</span><span class="sxs-lookup"><span data-stu-id="b1499-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b1499-105">Приложение ASP.NET Core можно разместить в Windows без использования IIS в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="b1499-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="b1499-106">Размещенное в службе Windows приложение может автоматически запускаться после перезагрузок и аварийных завершений работы, не требуя вмешательства оператора.</span><span class="sxs-lookup"><span data-stu-id="b1499-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="b1499-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b1499-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="b1499-108">Начало работы</span><span class="sxs-lookup"><span data-stu-id="b1499-108">Get started</span></span>

<span data-ttu-id="b1499-109">Чтобы настроить существующий проект ASP.NET Core для запуска в службе, требуется следующий минимальный набор изменений:</span><span class="sxs-lookup"><span data-stu-id="b1499-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="b1499-110">В файле проекта:</span><span class="sxs-lookup"><span data-stu-id="b1499-110">In the project file:</span></span>

   1. <span data-ttu-id="b1499-111">Подтвердите наличие идентификатора среды выполнения или добавьте его в **\<PropertyGroup >**, где содержится требуемая версия платформы:</span><span class="sxs-lookup"><span data-stu-id="b1499-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

   1. <span data-ttu-id="b1499-112">Добавьте ссылку на пакет для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="b1499-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="b1499-113">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b1499-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="b1499-114">Вызовите [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) вместо `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="b1499-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="b1499-115">Вызовите [UseContentRoot](xref:fundamentals/host/web-host#content-root) и используйте путь к расположению публикации приложения вместо `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="b1499-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="b1499-116">Опубликуйте приложение.</span><span class="sxs-lookup"><span data-stu-id="b1499-116">Publish the app.</span></span> <span data-ttu-id="b1499-117">Используйте команду [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="b1499-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="b1499-118">Работая в Visual Studio, выберите **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="b1499-118">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="b1499-119">Чтобы опубликовать пример приложения из командной строки, выполните следующую команду в окне консоли из папки проекта:</span><span class="sxs-lookup"><span data-stu-id="b1499-119">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="b1499-120">Используйте программу командной строки [sc.exe](https://technet.microsoft.com/library/bb490995), чтобы создать службу.</span><span class="sxs-lookup"><span data-stu-id="b1499-120">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="b1499-121">Значение `binPath` обозначает путь к исполняемому файлу приложения, включая имя самого файла.</span><span class="sxs-lookup"><span data-stu-id="b1499-121">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="b1499-122">**Требуется пробел между знаком равенства и кавычками, с которых начинается путь.**</span><span class="sxs-lookup"><span data-stu-id="b1499-122">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="b1499-123">Для создания службы, опубликованной в папке проекта, используйте путь к папке *publish*.</span><span class="sxs-lookup"><span data-stu-id="b1499-123">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="b1499-124">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="b1499-124">In the following example:</span></span>

   * <span data-ttu-id="b1499-125">Проект находится в папке `c:\my_services\AspNetCoreService`.</span><span class="sxs-lookup"><span data-stu-id="b1499-125">The project resides in the `c:\my_services\AspNetCoreService` folder.</span></span>
   * <span data-ttu-id="b1499-126">Проект публикуется в конфигурации `Release`.</span><span class="sxs-lookup"><span data-stu-id="b1499-126">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="b1499-127">Моникер целевой платформы (TFM) — `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="b1499-127">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="b1499-128">Идентификатор среды выполнения (RID) — `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="b1499-128">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="b1499-129">Имя исполняемого файла приложения — *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="b1499-129">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="b1499-130">Служба называется **MyService**.</span><span class="sxs-lookup"><span data-stu-id="b1499-130">The service is named **MyService**.</span></span>

   <span data-ttu-id="b1499-131">Пример</span><span class="sxs-lookup"><span data-stu-id="b1499-131">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="b1499-132">Убедитесь, что между аргументом `binPath=` и его значением есть пробел.</span><span class="sxs-lookup"><span data-stu-id="b1499-132">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="b1499-133">Чтобы опубликовать и запустить службу из другой папки:</span><span class="sxs-lookup"><span data-stu-id="b1499-133">To publish and start the service from a different folder:</span></span>
   
      1. <span data-ttu-id="b1499-134">Используйте параметр [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) команды `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="b1499-134">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="b1499-135">Если вы используете Visual Studio, настройте **Целевое расположение** на странице свойств публикации **FolderProfile**, прежде чем нажимать кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="b1499-135">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
   1. <span data-ttu-id="b1499-136">Создайте службу с помощью команды `sc.exe`, используя путь к папке выходных данных.</span><span class="sxs-lookup"><span data-stu-id="b1499-136">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="b1499-137">Включите имя исполняемого файла службы в путь, передаваемый в `binPath`.</span><span class="sxs-lookup"><span data-stu-id="b1499-137">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="b1499-138">Запустите службу с помощью команды `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="b1499-138">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="b1499-139">Чтобы запустить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b1499-139">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="b1499-140">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="b1499-140">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="b1499-141">Команду `sc query <SERVICE_NAME>` можно использовать для проверки состояния службы:</span><span class="sxs-lookup"><span data-stu-id="b1499-141">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="b1499-142">Чтобы проверить состояние примера службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b1499-142">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="b1499-143">Если служба находится в состоянии `RUNNING` и является веб-приложением, найдите приложение по его пути (по умолчанию `http://localhost:5000`, который перенаправляет на `https://localhost:5001` при использовании [ПО промежуточного слоя перенаправления на HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="b1499-143">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="b1499-144">Чтобы получить пример службы приложений, найдите приложение по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b1499-144">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="b1499-145">Остановите службу с помощью команды `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="b1499-145">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="b1499-146">Чтобы остановить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b1499-146">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="b1499-147">После небольшой задержки для остановки службы удалите службу с помощью команды `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="b1499-147">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="b1499-148">Проверьте состояние примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="b1499-148">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="b1499-149">Когда пример службы приложений находится в состоянии `STOPPED`, используйте следующую команду для удаления примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="b1499-149">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="b1499-150">Обеспечение возможности запуска за пределами службы</span><span class="sxs-lookup"><span data-stu-id="b1499-150">Provide a way to run outside of a service</span></span>

<span data-ttu-id="b1499-151">Тестирование и отладку проще выполнять при работе вне службы. Поэтому разработчики обычно добавляют код, который вызывает `RunAsService` только при соблюдении определенных условий.</span><span class="sxs-lookup"><span data-stu-id="b1499-151">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="b1499-152">Например, приложение может выполняться как консольное приложение с аргументом командной строки `--console` или при присоединении отладчика:</span><span class="sxs-lookup"><span data-stu-id="b1499-152">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="b1499-153">Так как для конфигурации ASP.NET Core требуется пара имя-значение для аргументов командной строки, параметр `--console` удаляется до передачи аргументов в [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="b1499-153">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="b1499-154">`isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.</span><span class="sxs-lookup"><span data-stu-id="b1499-154">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="b1499-155">Обработка событий остановки и запуска</span><span class="sxs-lookup"><span data-stu-id="b1499-155">Handle stopping and starting events</span></span>

<span data-ttu-id="b1499-156">Для обработки событий [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) и [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) внесите следующие дополнительные изменения:</span><span class="sxs-lookup"><span data-stu-id="b1499-156">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="b1499-157">Создайте класс, производный от [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="b1499-157">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="b1499-158">Создайте метод расширения для [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), который передает пользовательский параметр `WebHostService` в [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="b1499-158">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="b1499-159">В `Program.Main` измените вызов [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) на вызов нового метода расширения `RunAsCustomService`:</span><span class="sxs-lookup"><span data-stu-id="b1499-159">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="b1499-160">`isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.</span><span class="sxs-lookup"><span data-stu-id="b1499-160">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="b1499-161">Если пользовательский код `WebHostService` обращается к службе путем внедрения зависимости (например, к средству ведения журнала), ее можно получить из свойства [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="b1499-161">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b1499-162">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="b1499-162">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b1499-163">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="b1499-163">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="b1499-164">Дополнительные сведения см. в разделе <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="b1499-164">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="b1499-165">Настройка HTTPS</span><span class="sxs-lookup"><span data-stu-id="b1499-165">Configure HTTPS</span></span>

<span data-ttu-id="b1499-166">Укажите [конфигурацию конечной точки HTTPS для сервера Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="b1499-166">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="b1499-167">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="b1499-167">Current directory and content root</span></span>

<span data-ttu-id="b1499-168">Текущий рабочий каталог, возвращенный путем вызова `Directory.GetCurrentDirectory()` для службы Windows, — это папка *C:\WINDOWS\system32*.</span><span class="sxs-lookup"><span data-stu-id="b1499-168">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\WINDOWS\system32* folder.</span></span> <span data-ttu-id="b1499-169">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="b1499-169">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="b1499-170">Используйте один из следующих методов для сохранения и доступа к ресурсам и файлам параметров службы с помощью [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) при использовании [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="b1499-170">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="b1499-171">Используйте путь корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="b1499-171">Use the content root path.</span></span> <span data-ttu-id="b1499-172">`IHostingEnvironment.ContentRootPath` — это тот же путь, предоставленный аргументом `binPath` при создании службы.</span><span class="sxs-lookup"><span data-stu-id="b1499-172">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="b1499-173">Вместо использования `Directory.GetCurrentDirectory()` для создания путей к файлам параметров используйте путь корневого каталога содержимого и храните файлы в корневом каталоге содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="b1499-173">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="b1499-174">Храните файлы в подходящем месте на диске.</span><span class="sxs-lookup"><span data-stu-id="b1499-174">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="b1499-175">Укажите абсолютный путь с помощью `SetBasePath` к папке, содержащей файлы.</span><span class="sxs-lookup"><span data-stu-id="b1499-175">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1499-176">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b1499-176">Additional resources</span></span>

* <span data-ttu-id="b1499-177">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="b1499-177">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
