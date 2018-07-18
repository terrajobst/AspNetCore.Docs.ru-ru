---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: e9e10b0bc99b2c54bf342121b1a454be5dac66c6
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938201"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="6b0db-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="6b0db-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="6b0db-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)</span><span class="sxs-lookup"><span data-stu-id="6b0db-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6b0db-105">Приложение ASP.NET Core можно разместить в Windows без использования IIS в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="6b0db-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="6b0db-106">Размещенное в службе Windows приложение может автоматически запускаться после перезагрузок и аварийных завершений работы, не требуя вмешательства оператора.</span><span class="sxs-lookup"><span data-stu-id="6b0db-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="6b0db-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6b0db-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="6b0db-108">Начало работы</span><span class="sxs-lookup"><span data-stu-id="6b0db-108">Get started</span></span>

<span data-ttu-id="6b0db-109">Чтобы настроить существующий проект ASP.NET Core для запуска в службе, требуется следующий минимальный набор изменений:</span><span class="sxs-lookup"><span data-stu-id="6b0db-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="6b0db-110">В файле проекта:</span><span class="sxs-lookup"><span data-stu-id="6b0db-110">In the project file:</span></span>

   1. <span data-ttu-id="6b0db-111">Подтвердите наличие идентификатора среды выполнения или добавьте его в **\<PropertyGroup >**, где содержится требуемая версия платформы:</span><span class="sxs-lookup"><span data-stu-id="6b0db-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

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

   1. <span data-ttu-id="6b0db-112">Добавьте ссылку на пакет для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="6b0db-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="6b0db-113">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="6b0db-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="6b0db-114">Вызовите [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) вместо `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="6b0db-115">Вызовите [UseContentRoot](xref:fundamentals/host/web-host#content-root) и используйте путь к расположению публикации приложения вместо `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="6b0db-116">Опубликуйте приложение.</span><span class="sxs-lookup"><span data-stu-id="6b0db-116">Publish the app.</span></span> <span data-ttu-id="6b0db-117">Используйте команду [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="6b0db-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

   <span data-ttu-id="6b0db-118">Чтобы опубликовать пример приложения из командной строки, выполните следующую команду в окне консоли из папки проекта:</span><span class="sxs-lookup"><span data-stu-id="6b0db-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="6b0db-119">Используйте программу командной строки [sc.exe](https://technet.microsoft.com/library/bb490995), чтобы создать службу.</span><span class="sxs-lookup"><span data-stu-id="6b0db-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="6b0db-120">Значение `binPath` обозначает путь к исполняемому файлу приложения, включая имя самого файла.</span><span class="sxs-lookup"><span data-stu-id="6b0db-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="6b0db-121">**Требуется пробел между знаком равенства и кавычками, с которых начинается путь.**</span><span class="sxs-lookup"><span data-stu-id="6b0db-121">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="6b0db-122">Для создания службы, опубликованной в папке проекта, используйте путь к папке *publish*.</span><span class="sxs-lookup"><span data-stu-id="6b0db-122">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="6b0db-123">В следующем примере служба:</span><span class="sxs-lookup"><span data-stu-id="6b0db-123">In the following example, the service is:</span></span>

   * <span data-ttu-id="6b0db-124">называется **MyService**;</span><span class="sxs-lookup"><span data-stu-id="6b0db-124">Named **MyService**.</span></span>
   * <span data-ttu-id="6b0db-125">публикуется в папке *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish*;</span><span class="sxs-lookup"><span data-stu-id="6b0db-125">Published to the *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* folder.</span></span>
   * <span data-ttu-id="6b0db-126">представлена исполняемым файлом приложения с именем *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="6b0db-126">Represented by an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="6b0db-127">Откройте командную оболочку с правами администратора и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6b0db-127">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\<TARGET_FRAMEWORK>\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="6b0db-128">Убедитесь, что между аргументом `binPath=` и его значением есть пробел.</span><span class="sxs-lookup"><span data-stu-id="6b0db-128">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="6b0db-129">Чтобы опубликовать и запустить службу из другой папки:</span><span class="sxs-lookup"><span data-stu-id="6b0db-129">To publish and start the service from a different folder:</span></span>
   
   1. <span data-ttu-id="6b0db-130">Используйте параметр [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) команды `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-130">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span>
   1. <span data-ttu-id="6b0db-131">Создайте службу с помощью команды `sc.exe`, используя путь к папке выходных данных.</span><span class="sxs-lookup"><span data-stu-id="6b0db-131">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="6b0db-132">Включите имя исполняемого файла службы в путь, передаваемый в `binPath`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-132">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="6b0db-133">Запустите службу с помощью команды `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-133">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="6b0db-134">Чтобы запустить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6b0db-134">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="6b0db-135">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="6b0db-135">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="6b0db-136">Команду `sc query <SERVICE_NAME>` можно использовать для проверки состояния службы:</span><span class="sxs-lookup"><span data-stu-id="6b0db-136">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="6b0db-137">Чтобы проверить состояние примера службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6b0db-137">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="6b0db-138">Если служба находится в состоянии `RUNNING` и является веб-приложением, найдите приложение по его пути (по умолчанию `http://localhost:5000`, который перенаправляет на `https://localhost:5001` при использовании [ПО промежуточного слоя перенаправления на HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="6b0db-138">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="6b0db-139">Чтобы получить пример службы приложений, найдите приложение по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-139">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="6b0db-140">Остановите службу с помощью команды `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-140">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="6b0db-141">Чтобы остановить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6b0db-141">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="6b0db-142">После небольшой задержки для остановки службы удалите службу с помощью команды `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-142">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="6b0db-143">Проверьте состояние примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="6b0db-143">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="6b0db-144">Когда пример службы приложений находится в состоянии `STOPPED`, используйте следующую команду для удаления примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="6b0db-144">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="6b0db-145">Обеспечение возможности запуска за пределами службы</span><span class="sxs-lookup"><span data-stu-id="6b0db-145">Provide a way to run outside of a service</span></span>

<span data-ttu-id="6b0db-146">Тестирование и отладку проще выполнять при работе вне службы. Поэтому разработчики обычно добавляют код, который вызывает `RunAsService` только при соблюдении определенных условий.</span><span class="sxs-lookup"><span data-stu-id="6b0db-146">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="6b0db-147">Например, приложение может выполняться как консольное приложение с аргументом командной строки `--console` или при присоединении отладчика:</span><span class="sxs-lookup"><span data-stu-id="6b0db-147">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="6b0db-148">Так как для конфигурации ASP.NET Core требуется пара имя-значение для аргументов командной строки, параметр `--console` удаляется до передачи аргументов в [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="6b0db-148">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="6b0db-149">`isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-149">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="6b0db-150">Обработка событий остановки и запуска</span><span class="sxs-lookup"><span data-stu-id="6b0db-150">Handle stopping and starting events</span></span>

<span data-ttu-id="6b0db-151">Для обработки событий [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) и [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) внесите следующие дополнительные изменения:</span><span class="sxs-lookup"><span data-stu-id="6b0db-151">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="6b0db-152">Создайте класс, производный от [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="6b0db-152">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="6b0db-153">Создайте метод расширения для [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), который передает пользовательский параметр `WebHostService` в [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="6b0db-153">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="6b0db-154">В `Program.Main` измените вызов [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) на вызов нового метода расширения `RunAsCustomService`:</span><span class="sxs-lookup"><span data-stu-id="6b0db-154">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > <span data-ttu-id="6b0db-155">`isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.</span><span class="sxs-lookup"><span data-stu-id="6b0db-155">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="6b0db-156">Если пользовательский код `WebHostService` обращается к службе путем внедрения зависимости (например, к средству ведения журнала), ее можно получить из свойства [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="6b0db-156">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="6b0db-157">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="6b0db-157">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="6b0db-158">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="6b0db-158">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="6b0db-159">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="6b0db-159">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="6b0db-160">Конфигурация конечных точек Kestrel</span><span class="sxs-lookup"><span data-stu-id="6b0db-160">Kestrel endpoint configuration</span></span>

<span data-ttu-id="6b0db-161">Сведения о конфигурации конечных точек Kestrel, включая конфигурацию HTTPS и поддержку SNI, см. в разделе [Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="6b0db-161">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
