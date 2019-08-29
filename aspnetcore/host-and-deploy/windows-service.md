---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 308a8bd10371cc70c431b8858ef7d82c1bb624da
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975414"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="ccf25-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="ccf25-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="ccf25-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)</span><span class="sxs-lookup"><span data-stu-id="ccf25-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ccf25-105">Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS.</span><span class="sxs-lookup"><span data-stu-id="ccf25-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="ccf25-106">При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки сервера.</span><span class="sxs-lookup"><span data-stu-id="ccf25-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="ccf25-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ccf25-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ccf25-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ccf25-108">Prerequisites</span></span>

* [<span data-ttu-id="ccf25-109">Пакет SDK для ASP.NET Core 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="ccf25-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="ccf25-110">PowerShell 6.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="ccf25-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="ccf25-111">Шаблон службы рабочей роли</span><span class="sxs-lookup"><span data-stu-id="ccf25-111">Worker Service template</span></span>

<span data-ttu-id="ccf25-112">Шаблон службы рабочей роли ASP.NET Core может служить отправной точкой для написания длительно выполняющихся приложений служб.</span><span class="sxs-lookup"><span data-stu-id="ccf25-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="ccf25-113">Чтобы использовать шаблон в качестве основы для приложения службы Windows, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="ccf25-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="ccf25-114">Создайте приложение службы рабочей роли на основе шаблона .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ccf25-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="ccf25-115">Согласно указаниям в разделе [Конфигурация приложения](#app-configuration) измените приложение службы рабочей роли так, чтобы оно могло выполняться как служба Windows.</span><span class="sxs-lookup"><span data-stu-id="ccf25-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ccf25-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ccf25-116">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ccf25-117">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="ccf25-117">Create a new project.</span></span>
1. <span data-ttu-id="ccf25-118">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ccf25-119">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-119">Select **Next**.</span></span>
1. <span data-ttu-id="ccf25-120">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ccf25-120">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="ccf25-121">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-121">Select **Create**.</span></span>
1. <span data-ttu-id="ccf25-122">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-122">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="ccf25-123">Выберите шаблон **Служба рабочей роли**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-123">Select the **Worker Service** template.</span></span> <span data-ttu-id="ccf25-124">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-124">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ccf25-125">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ccf25-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ccf25-126">Используйте шаблон службы рабочей роли (`worker`) с командой [dotnet new](/dotnet/core/tools/dotnet-new) из командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="ccf25-126">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="ccf25-127">В приведенном ниже примере создается приложение службы рабочей роли с именем `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-127">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="ccf25-128">Папка для приложения `ContosoWorkerService` создается автоматически при выполнении команды.</span><span class="sxs-lookup"><span data-stu-id="ccf25-128">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="ccf25-129">Конфигурация приложения</span><span class="sxs-lookup"><span data-stu-id="ccf25-129">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ccf25-130">`IHostBuilder.UseWindowsService` в составе пакета [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) вызывается при создании узла.</span><span class="sxs-lookup"><span data-stu-id="ccf25-130">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="ccf25-131">Если приложение выполняется как служба Windows, метод отвечает за следующие действия:</span><span class="sxs-lookup"><span data-stu-id="ccf25-131">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="ccf25-132">Задает для узла время существования `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-132">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="ccf25-133">Задает корневой каталог содержимого.</span><span class="sxs-lookup"><span data-stu-id="ccf25-133">Sets the content root.</span></span>
* <span data-ttu-id="ccf25-134">Включает ведение журнала событий с именем приложения в качестве имени источника по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ccf25-134">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="ccf25-135">Уровень ведения журнала можно задать с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="ccf25-135">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="ccf25-136">Только администраторы могут создавать источники событий.</span><span class="sxs-lookup"><span data-stu-id="ccf25-136">Only administrators can create new event sources.</span></span> <span data-ttu-id="ccf25-137">Если источник событий создать нельзя, используя имя приложения, для источника *Приложение* регистрируется предупреждение и журналы событий отключаются.</span><span class="sxs-lookup"><span data-stu-id="ccf25-137">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ccf25-138">Приложение требует ссылки на пакеты для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) и [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="ccf25-138">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="ccf25-139">Для тестирования и отладки при работе вне службы добавьте код, чтобы определить, как выполняется приложение: в качестве службы или консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="ccf25-139">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="ccf25-140">Проверьте, присоединен ли отладчик или присутствует ли параметр `--console`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-140">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="ccf25-141">Если одно из условий имеет значение true (приложение выполняется не в качестве службы), вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="ccf25-141">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="ccf25-142">Если условия имеют значение false (приложение выполняется в качестве службы), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="ccf25-142">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="ccf25-143">Вызовите <xref:System.IO.Directory.SetCurrentDirectory*> и используйте путь к расположению для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="ccf25-143">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="ccf25-144">Не вызывайте <xref:System.IO.Directory.GetCurrentDirectory*> для получения пути, так как при вызове <xref:System.IO.Directory.GetCurrentDirectory*> приложение службы Windows возвращает папку *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="ccf25-144">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="ccf25-145">Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="ccf25-145">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="ccf25-146">Этот шаг выполняется до того, как приложение будет настроено в `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-146">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="ccf25-147">Вызовите <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, чтобы запустить приложение в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="ccf25-147">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="ccf25-148">Так как [поставщик конфигурации командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) требует указания пар "имя-значение" для аргументов командной строки, параметр `--console` удаляется из аргументов, прежде чем <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> получит их.</span><span class="sxs-lookup"><span data-stu-id="ccf25-148">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="ccf25-149">Для записи данных в журнал событий Windows добавьте поставщик EventLog в <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="ccf25-149">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="ccf25-150">Задайте уровень ведения журнала с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="ccf25-150">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="ccf25-151">В следующем примере `RunAsCustomService` вызывается вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> для обработки событий времени существования в приложении.</span><span class="sxs-lookup"><span data-stu-id="ccf25-151">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="ccf25-152">Дополнительные сведения см. в разделе [Обработка событий запуска и остановки](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="ccf25-152">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="ccf25-153">Тип развертывания</span><span class="sxs-lookup"><span data-stu-id="ccf25-153">Deployment type</span></span>

<span data-ttu-id="ccf25-154">Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="ccf25-154">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="ccf25-155">Зависящее от платформы развертывание (FDD)</span><span class="sxs-lookup"><span data-stu-id="ccf25-155">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="ccf25-156">Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ccf25-156">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="ccf25-157">Если сценарий с FDD используется согласно инструкций в этой статье, пакет SDK создаcт исполняемый файл ( *.exe*), который называется *исполняемым файлом, зависящим от платформы*.</span><span class="sxs-lookup"><span data-stu-id="ccf25-157">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ccf25-158">Добавьте следующие элементы свойства в файл проекта:</span><span class="sxs-lookup"><span data-stu-id="ccf25-158">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="ccf25-159">`<OutputType>` — тип выходных данных приложения (`Exe` для исполняемого файла).</span><span class="sxs-lookup"><span data-stu-id="ccf25-159">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="ccf25-160">`<LangVersion>` — версия языка C# (`latest` или `preview`).</span><span class="sxs-lookup"><span data-stu-id="ccf25-160">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="ccf25-161">Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="ccf25-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ccf25-162">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ccf25-163">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) содержит целевую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="ccf25-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="ccf25-164">В следующем примере для RID задано значение `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="ccf25-165">Свойству `<SelfContained>` задано значение `false`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="ccf25-166">Эти свойства указывают пакету SDK создать исполняемый файл ( *.exe*) для Windows и приложение, которое зависит от общей платформы .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ccf25-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="ccf25-167">Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="ccf25-167">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ccf25-168">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-168">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="ccf25-169">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) содержит целевую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="ccf25-169">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="ccf25-170">В следующем примере для RID задано значение `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-170">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="ccf25-171">Свойству `<SelfContained>` задано значение `false`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-171">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="ccf25-172">Эти свойства указывают пакету SDK создать исполняемый файл ( *.exe*) для Windows и приложение, которое зависит от общей платформы .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ccf25-172">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="ccf25-173">Свойству `<UseAppHost>` задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-173">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="ccf25-174">Это свойство предоставляет службу с путем активации (исполняемый файл, *EXE*) для FDD.</span><span class="sxs-lookup"><span data-stu-id="ccf25-174">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="ccf25-175">Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="ccf25-175">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ccf25-176">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="ccf25-176">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="ccf25-177">Автономное развертывание</span><span class="sxs-lookup"><span data-stu-id="ccf25-177">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="ccf25-178">Для автономного развертывания необязательно наличие общей платформы в системе размещения.</span><span class="sxs-lookup"><span data-stu-id="ccf25-178">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="ccf25-179">Среда выполнения и зависимости приложения развертываются с приложением.</span><span class="sxs-lookup"><span data-stu-id="ccf25-179">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="ccf25-180">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows включается в `<PropertyGroup>`, где содержится целевая платформа:</span><span class="sxs-lookup"><span data-stu-id="ccf25-180">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="ccf25-181">Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="ccf25-181">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="ccf25-182">Укажите список идентификаторов RID, разделив их точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="ccf25-182">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="ccf25-183">Используйте имя свойства [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (во множественном числе).</span><span class="sxs-lookup"><span data-stu-id="ccf25-183">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="ccf25-184">Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="ccf25-184">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ccf25-185">Для свойства `<SelfContained>` задано значение `true`:</span><span class="sxs-lookup"><span data-stu-id="ccf25-185">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="ccf25-186">Учетная запись пользователя службы</span><span class="sxs-lookup"><span data-stu-id="ccf25-186">Service user account</span></span>

<span data-ttu-id="ccf25-187">Создайте для службы учетную запись пользователя с помощью командлета [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) в административной оболочке PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="ccf25-187">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="ccf25-188">Обновление Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763) или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="ccf25-188">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="ccf25-189">Версия Windows, предшествующая обновлению Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="ccf25-189">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="ccf25-190">Укажите [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="ccf25-190">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="ccf25-191">Параметр срока действия учетной записи <xref:System.DateTime> можно задать с помощью параметра `-AccountExpires`, передаваемого в командлет [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser).</span><span class="sxs-lookup"><span data-stu-id="ccf25-191">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="ccf25-192">Дополнительные сведения см. в статьях [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) и [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей служб).</span><span class="sxs-lookup"><span data-stu-id="ccf25-192">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="ccf25-193">Альтернативный подход к управлению пользователями при работе с Active Directory заключается в применении управляемых учетных записей служб.</span><span class="sxs-lookup"><span data-stu-id="ccf25-193">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="ccf25-194">Дополнительные сведения см. в [обзоре групповых управляемых учетных записей службы](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="ccf25-194">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="ccf25-195">Права на вход в качестве службы</span><span class="sxs-lookup"><span data-stu-id="ccf25-195">Log on as a service rights</span></span>

<span data-ttu-id="ccf25-196">Чтобы настроить право *Вход в качестве службы* для учетной записи пользователя службы, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="ccf25-196">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="ccf25-197">Откройте редактор локальной политики безопасности, запустив *secpol.msc*.</span><span class="sxs-lookup"><span data-stu-id="ccf25-197">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="ccf25-198">Разверните узел **Локальные политики** и выберите **Назначение прав пользователя**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-198">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="ccf25-199">Откройте политику **Вход в качестве службы**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-199">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="ccf25-200">Щелкните **Добавить пользователя или группу**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-200">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="ccf25-201">Укажите имя объекта (учетная запись пользователя) одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="ccf25-201">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="ccf25-202">Укажите учетную запись пользователя (`{DOMAIN OR COMPUTER NAME\USER}`) в поле для имени объекта и нажмите **ОК**, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="ccf25-202">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="ccf25-203">Выберите **Дополнительно**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-203">Select **Advanced**.</span></span> <span data-ttu-id="ccf25-204">Нажмите **Найти**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-204">Select **Find Now**.</span></span> <span data-ttu-id="ccf25-205">Выберите учетную запись пользователя из списка.</span><span class="sxs-lookup"><span data-stu-id="ccf25-205">Select the user account from the list.</span></span> <span data-ttu-id="ccf25-206">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ccf25-206">Select **OK**.</span></span> <span data-ttu-id="ccf25-207">Нажмите **ОК** еще раз, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="ccf25-207">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="ccf25-208">Нажмите **ОК** или **Применить**, чтобы сохранить изменения.</span><span class="sxs-lookup"><span data-stu-id="ccf25-208">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="ccf25-209">Создание службы Windows и управление ею</span><span class="sxs-lookup"><span data-stu-id="ccf25-209">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="ccf25-210">Создание службы</span><span class="sxs-lookup"><span data-stu-id="ccf25-210">Create a service</span></span>

<span data-ttu-id="ccf25-211">Зарегистрируйте службу с помощью команды PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccf25-211">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="ccf25-212">В административной оболочке PowerShell 6 выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="ccf25-212">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="ccf25-213">`{EXE PATH}` — путь к папке приложения на узле (например, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="ccf25-213">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="ccf25-214">Не включайте исполняемый файл приложения в путь.</span><span class="sxs-lookup"><span data-stu-id="ccf25-214">Don't include the app's executable in the path.</span></span> <span data-ttu-id="ccf25-215">Завершающая косая черта не требуется.</span><span class="sxs-lookup"><span data-stu-id="ccf25-215">A trailing slash isn't required.</span></span>
* <span data-ttu-id="ccf25-216">`{DOMAIN OR COMPUTER NAME\USER}` —учетная запись пользователя службы (например, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="ccf25-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="ccf25-217">`{NAME}` — имя службы (например, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="ccf25-217">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="ccf25-218">`{EXE FILE PATH}` — путь к исполняемому файлу приложения (например, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="ccf25-218">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="ccf25-219">Включите имя исполняемого файла с расширением.</span><span class="sxs-lookup"><span data-stu-id="ccf25-219">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="ccf25-220">`{DESCRIPTION}` — описание службы (например, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="ccf25-220">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="ccf25-221">`{DISPLAY NAME}` — отображаемое имя службы (например, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="ccf25-221">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="ccf25-222">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="ccf25-222">Start a service</span></span>

<span data-ttu-id="ccf25-223">Запустите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="ccf25-223">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="ccf25-224">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="ccf25-224">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="ccf25-225">Определение состояния службы</span><span class="sxs-lookup"><span data-stu-id="ccf25-225">Determine a service's status</span></span>

<span data-ttu-id="ccf25-226">Чтобы проверить состояние службы, используйте следующую команду PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="ccf25-226">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="ccf25-227">Состояние отображается одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="ccf25-227">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="ccf25-228">Остановка службы</span><span class="sxs-lookup"><span data-stu-id="ccf25-228">Stop a service</span></span>

<span data-ttu-id="ccf25-229">Остановите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="ccf25-229">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="ccf25-230">Удаление службы</span><span class="sxs-lookup"><span data-stu-id="ccf25-230">Remove a service</span></span>

<span data-ttu-id="ccf25-231">После небольшой задержки для остановки службы удалите службу с помощью следующей команды Powershell 6:</span><span class="sxs-lookup"><span data-stu-id="ccf25-231">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="ccf25-232">Обработка событий запуска и остановки</span><span class="sxs-lookup"><span data-stu-id="ccf25-232">Handle starting and stopping events</span></span>

<span data-ttu-id="ccf25-233">Чтобы обработать события <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> и <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="ccf25-233">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="ccf25-234">Создайте класс, производный от <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService>, с методами `OnStarting`, `OnStarted` и `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="ccf25-234">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="ccf25-235">Создайте метод расширения для <xref:Microsoft.AspNetCore.Hosting.IWebHost>, который передает `CustomWebHostService` в <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="ccf25-235">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="ccf25-236">В `Program.Main` вызовите метод расширения `RunAsCustomService` вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="ccf25-236">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="ccf25-237">Чтобы узнать расположение <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> в `Program.Main`, см. пример кода из раздела [Тип развертывания](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="ccf25-237">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ccf25-238">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="ccf25-238">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ccf25-239">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="ccf25-239">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="ccf25-240">Дополнительные сведения можно найти по адресу: <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="ccf25-240">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="ccf25-241">Настройка HTTPS</span><span class="sxs-lookup"><span data-stu-id="ccf25-241">Configure HTTPS</span></span>

<span data-ttu-id="ccf25-242">Чтобы настроить службу с защищенной конечной точкой, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="ccf25-242">To configure a service with a secure endpoint:</span></span>

1. <span data-ttu-id="ccf25-243">Создайте сертификат X.509 для системы размещения с помощью механизмов получения и развертывания сертификата вашей платформы.</span><span class="sxs-lookup"><span data-stu-id="ccf25-243">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="ccf25-244">Укажите [конфигурацию конечной точки HTTPS для сервера Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration), чтобы использовать сертификат.</span><span class="sxs-lookup"><span data-stu-id="ccf25-244">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="ccf25-245">Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="ccf25-245">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="ccf25-246">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="ccf25-246">Current directory and content root</span></span>

<span data-ttu-id="ccf25-247">Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="ccf25-247">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="ccf25-248">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="ccf25-248">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="ccf25-249">Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="ccf25-249">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="ccf25-250">Использование ContentRootPath или ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="ccf25-250">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="ccf25-251">Используйте [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) или <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> для поиска ресурсов приложения.</span><span class="sxs-lookup"><span data-stu-id="ccf25-251">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="ccf25-252">Указание папки приложения в качестве пути корневого каталога содержимого</span><span class="sxs-lookup"><span data-stu-id="ccf25-252">Set the content root path to the app's folder</span></span>

<span data-ttu-id="ccf25-253"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> — это тот же путь, который предоставляется аргументу `binPath` при создании службы.</span><span class="sxs-lookup"><span data-stu-id="ccf25-253">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="ccf25-254">Чтобы не вызывать метод `GetCurrentDirectory` для создания путей к файлам параметров, вызовите <xref:System.IO.Directory.SetCurrentDirectory*> с указанным путем к корневому каталогу содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="ccf25-254">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="ccf25-255">В `Program.Main` определите путь к папке с исполняемым файлом службы и используйте этот путь, чтобы создать корневой каталог содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="ccf25-255">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="ccf25-256">Хранение файлов службы в подходящем расположении на диске</span><span class="sxs-lookup"><span data-stu-id="ccf25-256">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="ccf25-257">Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ccf25-257">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ccf25-258">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ccf25-258">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="ccf25-259">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="ccf25-259">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="ccf25-260">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="ccf25-260">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
