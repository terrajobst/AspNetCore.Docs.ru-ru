---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/30/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 014585cd1e170fc94f7f577e11ec19824e54572f
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659852"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="b6e78-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="b6e78-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="b6e78-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="b6e78-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b6e78-105">Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS.</span><span class="sxs-lookup"><span data-stu-id="b6e78-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="b6e78-106">При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки сервера.</span><span class="sxs-lookup"><span data-stu-id="b6e78-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="b6e78-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b6e78-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6e78-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="b6e78-108">Prerequisites</span></span>

* [<span data-ttu-id="b6e78-109">Пакет SDK для ASP.NET Core 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="b6e78-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="b6e78-110">PowerShell 6.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="b6e78-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="b6e78-111">Шаблон службы рабочей роли</span><span class="sxs-lookup"><span data-stu-id="b6e78-111">Worker Service template</span></span>

<span data-ttu-id="b6e78-112">Шаблон службы рабочей роли ASP.NET Core может служить отправной точкой для написания длительно выполняющихся приложений служб.</span><span class="sxs-lookup"><span data-stu-id="b6e78-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="b6e78-113">Чтобы использовать шаблон в качестве основы для приложения службы Windows, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="b6e78-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="b6e78-114">Создайте приложение службы рабочей роли на основе шаблона .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6e78-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="b6e78-115">Согласно указаниям в разделе [Конфигурация приложения](#app-configuration) измените приложение службы рабочей роли так, чтобы оно могло выполняться как служба Windows.</span><span class="sxs-lookup"><span data-stu-id="b6e78-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="b6e78-116">Конфигурация приложения</span><span class="sxs-lookup"><span data-stu-id="b6e78-116">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b6e78-117">Приложению требуется ссылка на пакет для [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="b6e78-117">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="b6e78-118">`IHostBuilder.UseWindowsService` вызывается при сборке узла.</span><span class="sxs-lookup"><span data-stu-id="b6e78-118">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="b6e78-119">Если приложение выполняется как служба Windows, метод отвечает за следующие действия:</span><span class="sxs-lookup"><span data-stu-id="b6e78-119">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="b6e78-120">Задает для узла время существования `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-120">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="b6e78-121">Задает [корневой каталог содержимого](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="b6e78-121">Sets the [content root](xref:fundamentals/index#content-root).</span></span>
* <span data-ttu-id="b6e78-122">Включает ведение журнала событий с именем приложения в качестве имени источника по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b6e78-122">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="b6e78-123">Уровень ведения журнала можно задать с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="b6e78-123">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="b6e78-124">Только администраторы могут создавать источники событий.</span><span class="sxs-lookup"><span data-stu-id="b6e78-124">Only administrators can create new event sources.</span></span> <span data-ttu-id="b6e78-125">Если источник событий создать нельзя, используя имя приложения, для источника *Приложение* регистрируется предупреждение и журналы событий отключаются.</span><span class="sxs-lookup"><span data-stu-id="b6e78-125">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="b6e78-126">В разделе `CreateHostBuilder` файла *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b6e78-126">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="b6e78-127">Этот раздел сопровождают следующие примеры приложений:</span><span class="sxs-lookup"><span data-stu-id="b6e78-127">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="b6e78-128">Пример фоновой службы рабочих ролей &ndash; пример приложения, не являющегося веб-приложением, на основе [шаблона службы рабочих ролей](#worker-service-template), который использует [размещенные службы](xref:fundamentals/host/hosted-services) для фоновых задач.</span><span class="sxs-lookup"><span data-stu-id="b6e78-128">Background Worker Service Sample &ndash; A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="b6e78-129">Пример службы веб-приложений &ndash; пример веб-приложения Razor Pages, который выполняется как служба Windows с [размещенными службами](xref:fundamentals/host/hosted-services) для фоновых задач.</span><span class="sxs-lookup"><span data-stu-id="b6e78-129">Web App Service Sample &ndash; A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="b6e78-130">Рекомендации по MVC см. в статьях <xref:mvc/overview> и <xref:migration/22-to-30>.</span><span class="sxs-lookup"><span data-stu-id="b6e78-130">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b6e78-131">Приложение требует ссылки на пакеты для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) и [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="b6e78-131">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="b6e78-132">Для тестирования и отладки при работе вне службы добавьте код, чтобы определить, как выполняется приложение: в качестве службы или консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="b6e78-132">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="b6e78-133">Проверьте, присоединен ли отладчик или присутствует ли параметр `--console`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-133">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="b6e78-134">Если одно из условий имеет значение true (приложение выполняется не в качестве службы), вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="b6e78-134">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="b6e78-135">Если условия имеют значение false (приложение выполняется в качестве службы), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="b6e78-135">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="b6e78-136">Вызовите <xref:System.IO.Directory.SetCurrentDirectory*> и используйте путь к расположению для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="b6e78-136">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="b6e78-137">Не вызывайте <xref:System.IO.Directory.GetCurrentDirectory*> для получения пути, так как при вызове <xref:System.IO.Directory.GetCurrentDirectory*> приложение службы Windows возвращает папку *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="b6e78-137">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="b6e78-138">Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="b6e78-138">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="b6e78-139">Этот шаг выполняется до того, как приложение будет настроено в `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-139">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="b6e78-140">Вызовите <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, чтобы запустить приложение в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="b6e78-140">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="b6e78-141">Так как [поставщик конфигурации командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) требует указания пар "имя-значение" для аргументов командной строки, параметр `--console` удаляется из аргументов, прежде чем <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> получит их.</span><span class="sxs-lookup"><span data-stu-id="b6e78-141">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="b6e78-142">Для записи данных в журнал событий Windows добавьте поставщик EventLog в <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="b6e78-142">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="b6e78-143">Задайте уровень ведения журнала с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="b6e78-143">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="b6e78-144">В следующем примере `RunAsCustomService` вызывается вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> для обработки событий времени существования в приложении.</span><span class="sxs-lookup"><span data-stu-id="b6e78-144">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="b6e78-145">Дополнительные сведения см. в разделе [Обработка событий запуска и остановки](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="b6e78-145">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="b6e78-146">Тип развертывания</span><span class="sxs-lookup"><span data-stu-id="b6e78-146">Deployment type</span></span>

<span data-ttu-id="b6e78-147">Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="b6e78-147">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="b6e78-148">SDK</span><span class="sxs-lookup"><span data-stu-id="b6e78-148">SDK</span></span>

<span data-ttu-id="b6e78-149">Для службы на основе веб-приложений, которая использует платформы MVC или Razor Pages, укажите веб-пакет SDK в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="b6e78-149">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="b6e78-150">Если служба выполняет только фоновые задачи (например, [размещенные службы](xref:fundamentals/host/hosted-services)), укажите пакет SDK рабочей роли в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="b6e78-150">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="b6e78-151">Зависящее от платформы развертывание (FDD)</span><span class="sxs-lookup"><span data-stu-id="b6e78-151">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="b6e78-152">Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6e78-152">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="b6e78-153">Если сценарий с FDD используется согласно инструкций в этой статье, пакет SDK создаcт исполняемый файл ( *.exe*), который называется *исполняемым файлом, зависящим от платформы*.</span><span class="sxs-lookup"><span data-stu-id="b6e78-153">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b6e78-154">При использовании [веб-пакета SDK](#sdk), файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="b6e78-154">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="b6e78-155">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-155">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="b6e78-156">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) содержит целевую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="b6e78-156">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="b6e78-157">В следующем примере для RID задано значение `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-157">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="b6e78-158">Свойству `<SelfContained>` задано значение `false`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-158">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="b6e78-159">Эти свойства указывают пакету SDK создать исполняемый файл ( *.exe*) для Windows и приложение, которое зависит от общей платформы .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6e78-159">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="b6e78-160">Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="b6e78-160">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="b6e78-161">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-161">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="b6e78-162">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) содержит целевую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="b6e78-162">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="b6e78-163">В следующем примере для RID задано значение `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-163">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="b6e78-164">Свойству `<SelfContained>` задано значение `false`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-164">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="b6e78-165">Эти свойства указывают пакету SDK создать исполняемый файл ( *.exe*) для Windows и приложение, которое зависит от общей платформы .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6e78-165">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="b6e78-166">Свойству `<UseAppHost>` задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-166">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="b6e78-167">Это свойство предоставляет службу с путем активации (исполняемый файл, *EXE*) для FDD.</span><span class="sxs-lookup"><span data-stu-id="b6e78-167">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="b6e78-168">Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="b6e78-168">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="b6e78-169">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-169">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="b6e78-170">Автономное развертывание</span><span class="sxs-lookup"><span data-stu-id="b6e78-170">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="b6e78-171">Для автономного развертывания необязательно наличие общей платформы в системе размещения.</span><span class="sxs-lookup"><span data-stu-id="b6e78-171">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="b6e78-172">Среда выполнения и зависимости приложения развертываются с приложением.</span><span class="sxs-lookup"><span data-stu-id="b6e78-172">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="b6e78-173">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows включается в `<PropertyGroup>`, где содержится целевая платформа:</span><span class="sxs-lookup"><span data-stu-id="b6e78-173">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="b6e78-174">Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="b6e78-174">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="b6e78-175">Укажите список идентификаторов RID, разделив их точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="b6e78-175">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="b6e78-176">Используйте имя свойства [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (во множественном числе).</span><span class="sxs-lookup"><span data-stu-id="b6e78-176">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="b6e78-177">Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="b6e78-177">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b6e78-178">Для свойства `<SelfContained>` задано значение `true`:</span><span class="sxs-lookup"><span data-stu-id="b6e78-178">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="b6e78-179">Учетная запись пользователя службы</span><span class="sxs-lookup"><span data-stu-id="b6e78-179">Service user account</span></span>

<span data-ttu-id="b6e78-180">Создайте для службы учетную запись пользователя с помощью командлета [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) в административной оболочке PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="b6e78-180">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="b6e78-181">Обновление Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763) или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="b6e78-181">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="b6e78-182">Версия Windows, предшествующая обновлению Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="b6e78-182">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="b6e78-183">Укажите [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="b6e78-183">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="b6e78-184">Параметр срока действия учетной записи <xref:System.DateTime> можно задать с помощью параметра `-AccountExpires`, передаваемого в командлет [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser).</span><span class="sxs-lookup"><span data-stu-id="b6e78-184">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="b6e78-185">Дополнительные сведения см. в статьях [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) и [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей служб).</span><span class="sxs-lookup"><span data-stu-id="b6e78-185">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="b6e78-186">Альтернативный подход к управлению пользователями при работе с Active Directory заключается в применении управляемых учетных записей служб.</span><span class="sxs-lookup"><span data-stu-id="b6e78-186">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="b6e78-187">Дополнительные сведения см. в [обзоре групповых управляемых учетных записей службы](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="b6e78-187">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="b6e78-188">Права на вход в качестве службы</span><span class="sxs-lookup"><span data-stu-id="b6e78-188">Log on as a service rights</span></span>

<span data-ttu-id="b6e78-189">Чтобы настроить право *Вход в качестве службы* для учетной записи пользователя службы, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="b6e78-189">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="b6e78-190">Откройте редактор локальной политики безопасности, запустив *secpol.msc*.</span><span class="sxs-lookup"><span data-stu-id="b6e78-190">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="b6e78-191">Разверните узел **Локальные политики** и выберите **Назначение прав пользователя**.</span><span class="sxs-lookup"><span data-stu-id="b6e78-191">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="b6e78-192">Откройте политику **Вход в качестве службы**.</span><span class="sxs-lookup"><span data-stu-id="b6e78-192">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="b6e78-193">Щелкните **Добавить пользователя или группу**.</span><span class="sxs-lookup"><span data-stu-id="b6e78-193">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="b6e78-194">Укажите имя объекта (учетная запись пользователя) одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="b6e78-194">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="b6e78-195">Укажите учетную запись пользователя (`{DOMAIN OR COMPUTER NAME\USER}`) в поле для имени объекта и нажмите **ОК**, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="b6e78-195">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="b6e78-196">Выберите **Дополнительно**.</span><span class="sxs-lookup"><span data-stu-id="b6e78-196">Select **Advanced**.</span></span> <span data-ttu-id="b6e78-197">Нажмите **Найти**.</span><span class="sxs-lookup"><span data-stu-id="b6e78-197">Select **Find Now**.</span></span> <span data-ttu-id="b6e78-198">Выберите учетную запись пользователя из списка.</span><span class="sxs-lookup"><span data-stu-id="b6e78-198">Select the user account from the list.</span></span> <span data-ttu-id="b6e78-199">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b6e78-199">Select **OK**.</span></span> <span data-ttu-id="b6e78-200">Нажмите **ОК** еще раз, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="b6e78-200">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="b6e78-201">Нажмите **ОК** или **Применить**, чтобы сохранить изменения.</span><span class="sxs-lookup"><span data-stu-id="b6e78-201">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="b6e78-202">Создание службы Windows и управление ею</span><span class="sxs-lookup"><span data-stu-id="b6e78-202">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="b6e78-203">Создание службы</span><span class="sxs-lookup"><span data-stu-id="b6e78-203">Create a service</span></span>

<span data-ttu-id="b6e78-204">Зарегистрируйте службу с помощью команды PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b6e78-204">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="b6e78-205">В административной оболочке PowerShell 6 выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="b6e78-205">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="b6e78-206">`{EXE PATH}` — путь к папке приложения на узле (например, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="b6e78-206">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="b6e78-207">Не включайте исполняемый файл приложения в путь.</span><span class="sxs-lookup"><span data-stu-id="b6e78-207">Don't include the app's executable in the path.</span></span> <span data-ttu-id="b6e78-208">Завершающая косая черта не требуется.</span><span class="sxs-lookup"><span data-stu-id="b6e78-208">A trailing slash isn't required.</span></span>
* <span data-ttu-id="b6e78-209">`{DOMAIN OR COMPUTER NAME\USER}` —учетная запись пользователя службы (например, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="b6e78-209">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="b6e78-210">`{NAME}` — имя службы (например, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="b6e78-210">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="b6e78-211">`{EXE FILE PATH}` — путь к исполняемому файлу приложения (например, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="b6e78-211">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="b6e78-212">Включите имя исполняемого файла с расширением.</span><span class="sxs-lookup"><span data-stu-id="b6e78-212">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="b6e78-213">`{DESCRIPTION}` — описание службы (например, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="b6e78-213">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="b6e78-214">`{DISPLAY NAME}` — отображаемое имя службы (например, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="b6e78-214">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="b6e78-215">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="b6e78-215">Start a service</span></span>

<span data-ttu-id="b6e78-216">Запустите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="b6e78-216">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="b6e78-217">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="b6e78-217">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="b6e78-218">Определение состояния службы</span><span class="sxs-lookup"><span data-stu-id="b6e78-218">Determine a service's status</span></span>

<span data-ttu-id="b6e78-219">Чтобы проверить состояние службы, используйте следующую команду PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="b6e78-219">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="b6e78-220">Состояние отображается одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="b6e78-220">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="b6e78-221">Остановка службы</span><span class="sxs-lookup"><span data-stu-id="b6e78-221">Stop a service</span></span>

<span data-ttu-id="b6e78-222">Остановите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="b6e78-222">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="b6e78-223">Удаление службы</span><span class="sxs-lookup"><span data-stu-id="b6e78-223">Remove a service</span></span>

<span data-ttu-id="b6e78-224">После небольшой задержки для остановки службы удалите службу с помощью следующей команды Powershell 6:</span><span class="sxs-lookup"><span data-stu-id="b6e78-224">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="b6e78-225">Обработка событий запуска и остановки</span><span class="sxs-lookup"><span data-stu-id="b6e78-225">Handle starting and stopping events</span></span>

<span data-ttu-id="b6e78-226">Чтобы обработать события <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> и <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="b6e78-226">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="b6e78-227">Создайте класс, производный от <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService>, с методами `OnStarting`, `OnStarted` и `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="b6e78-227">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="b6e78-228">Создайте метод расширения для <xref:Microsoft.AspNetCore.Hosting.IWebHost>, который передает `CustomWebHostService` в <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="b6e78-228">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="b6e78-229">В `Program.Main` вызовите метод расширения `RunAsCustomService` вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="b6e78-229">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="b6e78-230">Чтобы узнать расположение <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> в `Program.Main`, см. пример кода из раздела [Тип развертывания](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="b6e78-230">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b6e78-231">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="b6e78-231">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b6e78-232">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="b6e78-232">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="b6e78-233">Дополнительные сведения можно найти по адресу: <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="b6e78-233">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="b6e78-234">Настройка конечных точек</span><span class="sxs-lookup"><span data-stu-id="b6e78-234">Configure endpoints</span></span>

<span data-ttu-id="b6e78-235">По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-235">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b6e78-236">Настройте URL-адрес и порт, задав переменную среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="b6e78-236">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="b6e78-237">Дополнительные сведения о подходах к настройке URL-адресов и портов см. в соответствующей статье сервера:</span><span class="sxs-lookup"><span data-stu-id="b6e78-237">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="b6e78-238">В предыдущем руководстве рассматривается поддержка конечных точек HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b6e78-238">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="b6e78-239">Например, настройте приложение для HTTPS, если проверка подлинности используется со службой Windows.</span><span class="sxs-lookup"><span data-stu-id="b6e78-239">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="b6e78-240">Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="b6e78-240">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="b6e78-241">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="b6e78-241">Current directory and content root</span></span>

<span data-ttu-id="b6e78-242">Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="b6e78-242">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="b6e78-243">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="b6e78-243">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="b6e78-244">Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="b6e78-244">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="b6e78-245">Использование ContentRootPath или ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="b6e78-245">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="b6e78-246">Используйте [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) или <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> для поиска ресурсов приложения.</span><span class="sxs-lookup"><span data-stu-id="b6e78-246">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="b6e78-247">Указание папки приложения в качестве пути корневого каталога содержимого</span><span class="sxs-lookup"><span data-stu-id="b6e78-247">Set the content root path to the app's folder</span></span>

<span data-ttu-id="b6e78-248"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> — это тот же путь, который предоставляется аргументу `binPath` при создании службы.</span><span class="sxs-lookup"><span data-stu-id="b6e78-248">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="b6e78-249">Чтобы не вызывать метод `GetCurrentDirectory` для создания путей к файлам параметров, вызовите <xref:System.IO.Directory.SetCurrentDirectory*> с указанным путем к [корневому каталогу содержимого](xref:fundamentals/index#content-root) приложения.</span><span class="sxs-lookup"><span data-stu-id="b6e78-249">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="b6e78-250">В `Program.Main` определите путь к папке с исполняемым файлом службы и используйте этот путь, чтобы создать корневой каталог содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="b6e78-250">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="b6e78-251">Хранение файлов службы в подходящем расположении на диске</span><span class="sxs-lookup"><span data-stu-id="b6e78-251">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="b6e78-252">Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b6e78-252">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6e78-253">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b6e78-253">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="b6e78-254">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="b6e78-254">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="b6e78-255">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="b6e78-255">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
