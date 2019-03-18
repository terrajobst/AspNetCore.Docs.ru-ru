---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/08/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ecc7f3a8cd813c2803d03294e38d726905eeb1b8
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841427"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="09afc-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="09afc-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="09afc-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)</span><span class="sxs-lookup"><span data-stu-id="09afc-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="09afc-105">Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS.</span><span class="sxs-lookup"><span data-stu-id="09afc-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="09afc-106">При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="09afc-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="09afc-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="09afc-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09afc-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="09afc-108">Prerequisites</span></span>

* [<span data-ttu-id="09afc-109">PowerShell 6</span><span class="sxs-lookup"><span data-stu-id="09afc-109">PowerShell 6</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="deployment-type"></a><span data-ttu-id="09afc-110">Тип развертывания</span><span class="sxs-lookup"><span data-stu-id="09afc-110">Deployment type</span></span>

<span data-ttu-id="09afc-111">Вы можете создать зависящее от платформы или автономное развертывание службы Windows.</span><span class="sxs-lookup"><span data-stu-id="09afc-111">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="09afc-112">Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="09afc-112">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="09afc-113">развертывание, зависящее от платформы;</span><span class="sxs-lookup"><span data-stu-id="09afc-113">Framework-dependent deployment</span></span>

<span data-ttu-id="09afc-114">Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core.</span><span class="sxs-lookup"><span data-stu-id="09afc-114">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="09afc-115">При использовании сценария с FDD с приложением ASP.NET Core (службой Windows) с помощью пакета SDK создается исполняемый файл (*\*.exe*), который называется *исполняемым файлом, зависящим от платформы*.</span><span class="sxs-lookup"><span data-stu-id="09afc-115">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="09afc-116">автономное развертывание;</span><span class="sxs-lookup"><span data-stu-id="09afc-116">Self-contained deployment</span></span>

<span data-ttu-id="09afc-117">Для автономного развертывания (SCD) общие компоненты в целевой системе не требуются.</span><span class="sxs-lookup"><span data-stu-id="09afc-117">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="09afc-118">Среда выполнения и зависимости приложения развертываются с приложением в системе для размещения.</span><span class="sxs-lookup"><span data-stu-id="09afc-118">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="09afc-119">Преобразование проекта в службу Windows</span><span class="sxs-lookup"><span data-stu-id="09afc-119">Convert a project into a Windows Service</span></span>

<span data-ttu-id="09afc-120">Внесите следующие изменения в существующий проект ASP.NET Core, чтобы запустить приложение в качестве службы:</span><span class="sxs-lookup"><span data-stu-id="09afc-120">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="09afc-121">Обновления файла проекта</span><span class="sxs-lookup"><span data-stu-id="09afc-121">Project file updates</span></span>

<span data-ttu-id="09afc-122">Измените файл проекта с учетом выбранного [типа развертывания](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="09afc-122">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="09afc-123">Зависящее от платформы развертывание</span><span class="sxs-lookup"><span data-stu-id="09afc-123">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="09afc-124">Добавьте [идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows в `<PropertyGroup>`, где содержится требуемая версия платформы.</span><span class="sxs-lookup"><span data-stu-id="09afc-124">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="09afc-125">В следующем примере для RID задано значение `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="09afc-125">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="09afc-126">Добавьте свойство `<SelfContained>` со значением `false`.</span><span class="sxs-lookup"><span data-stu-id="09afc-126">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="09afc-127">Эти свойства дают пакету SDK инструкцию создать исполняемый файл (*EXE*) для Windows.</span><span class="sxs-lookup"><span data-stu-id="09afc-127">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="09afc-128">Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="09afc-128">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="09afc-129">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="09afc-129">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

::: moniker range=">= aspnetcore-2.2"

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

<span data-ttu-id="09afc-130">Добавьте свойство `<UseAppHost>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="09afc-130">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="09afc-131">Это свойство предоставляет службу с путем активации (исполняемый файл, *EXE*) для FDD.</span><span class="sxs-lookup"><span data-stu-id="09afc-131">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="09afc-132">Автономное развертывание</span><span class="sxs-lookup"><span data-stu-id="09afc-132">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="09afc-133">Подтвердите наличие [идентификатора среды выполнения](/dotnet/core/rid-catalog) Windows или добавьте его в `<PropertyGroup>`, где содержится требуемая версия платформы.</span><span class="sxs-lookup"><span data-stu-id="09afc-133">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="09afc-134">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="09afc-134">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="09afc-135">Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="09afc-135">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="09afc-136">Укажите список идентификаторов RID, разделив их точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="09afc-136">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="09afc-137">Укажите имя свойства `<RuntimeIdentifiers>` (множественное число).</span><span class="sxs-lookup"><span data-stu-id="09afc-137">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="09afc-138">Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="09afc-138">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="09afc-139">Добавьте ссылку на пакет для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="09afc-139">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="09afc-140">Чтобы включить ведение журнала событий Windows, добавьте ссылку на пакет для [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="09afc-140">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="09afc-141">Дополнительные сведения см. в разделе [Обработка событий запуска и остановки](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="09afc-141">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="09afc-142">Обновления Program.Main</span><span class="sxs-lookup"><span data-stu-id="09afc-142">Program.Main updates</span></span>

<span data-ttu-id="09afc-143">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="09afc-143">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="09afc-144">Для тестирования и отладки при работе вне службы добавьте код, чтобы определить, как выполняется приложение: в качестве службы или консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="09afc-144">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="09afc-145">Проверьте, присоединен ли отладчик или присутствует ли аргумент командной строки `--console`.</span><span class="sxs-lookup"><span data-stu-id="09afc-145">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="09afc-146">Если одно из условий имеет значение true (приложение выполняется не в качестве службы), вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> на веб-узле.</span><span class="sxs-lookup"><span data-stu-id="09afc-146">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="09afc-147">Если условия имеют значение false (приложение выполняется в качестве службы), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="09afc-147">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="09afc-148">Вызовите <xref:System.IO.Directory.SetCurrentDirectory*> и используйте путь к расположению для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="09afc-148">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="09afc-149">Не вызывайте <xref:System.IO.Directory.GetCurrentDirectory*> для получения пути, так как при вызове <xref:System.IO.Directory.GetCurrentDirectory*> приложение службы Windows возвращает папку *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="09afc-149">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="09afc-150">Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="09afc-150">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="09afc-151">Вызовите <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, чтобы запустить приложение в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="09afc-151">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="09afc-152">Так как для [поставщика конфигурации командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) требуется пара имя-значение для аргументов командной строки, параметр `--console` удаляется из аргументов, прежде чем <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> получит его.</span><span class="sxs-lookup"><span data-stu-id="09afc-152">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="09afc-153">Для записи данных в журнал событий Windows добавьте поставщик EventLog в <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="09afc-153">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="09afc-154">Задайте уровень ведения журнала с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="09afc-154">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="09afc-155">Для демонстрации и тестирования в файле параметров примера приложения для рабочей среды мы укажем такой уровень ведения журнала: `Information`.</span><span class="sxs-lookup"><span data-stu-id="09afc-155">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="09afc-156">В рабочей среде обычно присваивается значение `Error`.</span><span class="sxs-lookup"><span data-stu-id="09afc-156">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="09afc-157">Для получения дополнительной информации см. <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="09afc-157">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="09afc-158">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="09afc-158">Publish the app</span></span>

<span data-ttu-id="09afc-159">Опубликуйте приложение с помощью команды [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), [профиля публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) или Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="09afc-159">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="09afc-160">Если вы используете Visual Studio, выберите **FolderProfile** и настройте **целевое расположение**, прежде чем нажимать кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="09afc-160">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="09afc-161">Чтобы опубликовать пример приложения с помощью интерфейса командной строки (CLI), выполните в командной строке команду [dotnet publish](/dotnet/core/tools/dotnet-publish) в папке проекта с конфигурацией выпуска, передаваемой параметру [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="09afc-161">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="09afc-162">Чтобы опубликовать этот пример в папке за пределами приложения, задайте параметр [-o|--output](/dotnet/core/tools/dotnet-publish#options) и укажите путь.</span><span class="sxs-lookup"><span data-stu-id="09afc-162">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="09afc-163">Публикация зависящего от платформы развертывания</span><span class="sxs-lookup"><span data-stu-id="09afc-163">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="09afc-164">В следующем примере приложение публикуется в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="09afc-164">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="09afc-165">Публикация автономного развертывания</span><span class="sxs-lookup"><span data-stu-id="09afc-165">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="09afc-166">Идентификатор RID следует указать в свойстве `<RuntimeIdenfifier>` (или `<RuntimeIdentifiers>`) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="09afc-166">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="09afc-167">Укажите среду выполнения в параметре [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) команды `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="09afc-167">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="09afc-168">В следующем примере приложение публикуется для среды выполнения `win7-x64` в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="09afc-168">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="09afc-169">Создание учетной записи пользователя</span><span class="sxs-lookup"><span data-stu-id="09afc-169">Create a user account</span></span>

<span data-ttu-id="09afc-170">Создайте для службы учетную запись пользователя с помощью команды `net user` из административной командной оболочки PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="09afc-170">Create a user account for the service using the `net user` command from an administrative PowerShell 6 command shell:</span></span>

```powershell
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="09afc-171">Срок действия пароля по умолчанию составляет шесть недель.</span><span class="sxs-lookup"><span data-stu-id="09afc-171">The default password expiration is six weeks.</span></span>

<span data-ttu-id="09afc-172">Для примера приложения создайте учетную запись пользователя с именем `ServiceUser` и пароль.</span><span class="sxs-lookup"><span data-stu-id="09afc-172">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="09afc-173">В следующей команде замените `{PASSWORD}` на [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="09afc-173">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```powershell
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="09afc-174">Если вам нужно добавить пользователя в группу, используйте команду `net localgroup`, где `{GROUP}` — это имя группы:</span><span class="sxs-lookup"><span data-stu-id="09afc-174">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```powershell
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="09afc-175">Дополнительные сведения см. в статье [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей службы).</span><span class="sxs-lookup"><span data-stu-id="09afc-175">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="09afc-176">Альтернативный подход к управлению пользователями при работе с Active Directory заключается в применении управляемых учетных записей служб.</span><span class="sxs-lookup"><span data-stu-id="09afc-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="09afc-177">Дополнительные сведения см. в [обзоре групповых управляемых учетных записей службы](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="09afc-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="09afc-178">Установка разрешений: Вход в систему в качестве службы</span><span class="sxs-lookup"><span data-stu-id="09afc-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="09afc-179">Предоставьте доступ на запись, чтение и выполнение для папки приложения с помощью команды [icacls](/windows-server/administration/windows-commands/icacls):</span><span class="sxs-lookup"><span data-stu-id="09afc-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

```powershell
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="09afc-180">`{PATH}` &ndash; путь к папке приложения.</span><span class="sxs-lookup"><span data-stu-id="09afc-180">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="09afc-181">`{USER ACCOUNT}` &ndash; учетная запись пользователя (SID).</span><span class="sxs-lookup"><span data-stu-id="09afc-181">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="09afc-182">`(OI)` &ndash; флаг наследования объекта, который распространяет разрешения на вложенные файлы.</span><span class="sxs-lookup"><span data-stu-id="09afc-182">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="09afc-183">`(CI)` &ndash; флаг наследования контейнера, который распространяет разрешения на вложенные папки.</span><span class="sxs-lookup"><span data-stu-id="09afc-183">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="09afc-184">`{PERMISSION FLAGS}` &ndash; устанавливает разрешения для доступа к приложениям.</span><span class="sxs-lookup"><span data-stu-id="09afc-184">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="09afc-185">Запись (`W`)</span><span class="sxs-lookup"><span data-stu-id="09afc-185">Write (`W`)</span></span>
  * <span data-ttu-id="09afc-186">Чтение (`R`)</span><span class="sxs-lookup"><span data-stu-id="09afc-186">Read (`R`)</span></span>
  * <span data-ttu-id="09afc-187">Выполнение (`X`)</span><span class="sxs-lookup"><span data-stu-id="09afc-187">Execute (`X`)</span></span>
  * <span data-ttu-id="09afc-188">Полное (`F`)</span><span class="sxs-lookup"><span data-stu-id="09afc-188">Full (`F`)</span></span>
  * <span data-ttu-id="09afc-189">Изменение (`M`)</span><span class="sxs-lookup"><span data-stu-id="09afc-189">Modify (`M`)</span></span>
* <span data-ttu-id="09afc-190">`/t` &ndash; применяется рекурсивно к имеющимся вложенным папкам и файлам.</span><span class="sxs-lookup"><span data-stu-id="09afc-190">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="09afc-191">Для примера приложения, опубликованного в папке *c:\\svc*, и учетной записи `ServiceUser` с разрешениями на запись, чтение и выполнение используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="09afc-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```powershell
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="09afc-192">Дополнительные сведения см. в статье об [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="09afc-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="09afc-193">Создание службы</span><span class="sxs-lookup"><span data-stu-id="09afc-193">Create the service</span></span>

<span data-ttu-id="09afc-194">Используйте скрипт PowerShell [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts), чтобы зарегистрировать службу.</span><span class="sxs-lookup"><span data-stu-id="09afc-194">Use the [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="09afc-195">Из административной командной строки PowerShell 6 выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="09afc-195">From an administrative PowerShell 6 command prompt, execute the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Path "{PATH}" 
    -Exe {ASSEMBLY}.exe 
    -User {DOMAIN\USER}
```

<span data-ttu-id="09afc-196">В примере ниже для примера приложения указано следующее:</span><span class="sxs-lookup"><span data-stu-id="09afc-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="09afc-197">Служба называется **MyService**.</span><span class="sxs-lookup"><span data-stu-id="09afc-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="09afc-198">Опубликованная служба размещается в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="09afc-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="09afc-199">Исполняемый файл приложения с именем *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="09afc-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="09afc-200">Служба работает под учетной записью `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="09afc-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="09afc-201">В следующем примере именем локального компьютера является `Desktop-PC`.</span><span class="sxs-lookup"><span data-stu-id="09afc-201">In the following example, the local machine name is `Desktop-PC`.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Path "c:\svc" 
    -Exe SampleApp.exe 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="09afc-202">Управление службой</span><span class="sxs-lookup"><span data-stu-id="09afc-202">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="09afc-203">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="09afc-203">Start the service</span></span>

<span data-ttu-id="09afc-204">Запустите службу с помощью команды PowerShell 6 `Start-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="09afc-204">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="09afc-205">Чтобы запустить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="09afc-205">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="09afc-206">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="09afc-206">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="09afc-207">Определение состояния службы</span><span class="sxs-lookup"><span data-stu-id="09afc-207">Determine the service status</span></span>

<span data-ttu-id="09afc-208">Чтобы проверить состояние службы, используйте команду PowerShell 6 `Get-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="09afc-208">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="09afc-209">Состояние отображается одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="09afc-209">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="09afc-210">Чтобы проверить состояние примера службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="09afc-210">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="09afc-211">Обзор службы веб-приложений</span><span class="sxs-lookup"><span data-stu-id="09afc-211">Browse a web app service</span></span>

<span data-ttu-id="09afc-212">Если служба находится в состоянии `RUNNING` и является веб-приложением, найдите приложение по его пути (по умолчанию `http://localhost:5000`, который перенаправляет на `https://localhost:5001` при использовании [ПО промежуточного слоя перенаправления на HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="09afc-212">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="09afc-213">Чтобы получить пример службы приложений, найдите приложение по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="09afc-213">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="09afc-214">Остановите службу</span><span class="sxs-lookup"><span data-stu-id="09afc-214">Stop the service</span></span>

<span data-ttu-id="09afc-215">Остановите службу с помощью команды PowerShell 6 `Stop-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="09afc-215">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="09afc-216">Чтобы остановить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="09afc-216">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="09afc-217">Удаление службы</span><span class="sxs-lookup"><span data-stu-id="09afc-217">Remove the service</span></span>

<span data-ttu-id="09afc-218">После небольшой задержки для остановки службы удалите службу с помощью команды Powershell 6 `Remove-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="09afc-218">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="09afc-219">Проверьте состояние примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="09afc-219">Check the status of the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="09afc-220">Обработка событий запуска и остановки</span><span class="sxs-lookup"><span data-stu-id="09afc-220">Handle starting and stopping events</span></span>

<span data-ttu-id="09afc-221">Чтобы правильно обрабатывать события <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> и <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, внесите дополнительные изменения:</span><span class="sxs-lookup"><span data-stu-id="09afc-221">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="09afc-222">Создайте класс, производный от <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService>, с методами `OnStarting`, `OnStarted` и `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="09afc-222">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="09afc-223">Создайте метод расширения для <xref:Microsoft.AspNetCore.Hosting.IWebHost>, который передает `CustomWebHostService` в <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="09afc-223">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="09afc-224">В `Program.Main` вызовите метод расширения `RunAsCustomService` вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="09afc-224">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="09afc-225">Чтобы узнать расположение <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> в `Program.Main`, см. пример кода, приведенный в разделе [Преобразование проекта в службу Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="09afc-225">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="09afc-226">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="09afc-226">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="09afc-227">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="09afc-227">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="09afc-228">Для получения дополнительной информации см. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="09afc-228">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="09afc-229">Настройка HTTPS</span><span class="sxs-lookup"><span data-stu-id="09afc-229">Configure HTTPS</span></span>

<span data-ttu-id="09afc-230">Чтобы настроить службу с защищенной конечной точкой, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="09afc-230">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="09afc-231">Создайте сертификат X.509 для системы размещения с помощью механизмов получения и развертывания сертификата вашей платформы.</span><span class="sxs-lookup"><span data-stu-id="09afc-231">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="09afc-232">Укажите [конфигурацию конечной точки HTTPS для сервера Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration), чтобы использовать сертификат.</span><span class="sxs-lookup"><span data-stu-id="09afc-232">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="09afc-233">Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="09afc-233">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="09afc-234">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="09afc-234">Current directory and content root</span></span>

<span data-ttu-id="09afc-235">Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="09afc-235">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="09afc-236">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="09afc-236">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="09afc-237">Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="09afc-237">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="09afc-238">Указание папки приложения в качестве пути корневого каталога содержимого</span><span class="sxs-lookup"><span data-stu-id="09afc-238">Set the content root path to the app's folder</span></span>

<span data-ttu-id="09afc-239"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> — это тот же путь, предоставленный аргументом `binPath` при создании службы.</span><span class="sxs-lookup"><span data-stu-id="09afc-239">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="09afc-240">Чтобы не вызывать метод `GetCurrentDirectory` для создания путей к файлам параметров, вызовите <xref:System.IO.Directory.SetCurrentDirectory*> с указанным путем к корневому каталогу содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="09afc-240">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="09afc-241">В `Program.Main` определите путь к папке с исполняемым файлом службы и используйте этот путь, чтобы создать корневой каталог содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="09afc-241">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="09afc-242">Хранение файлов службы в подходящем месте на диске</span><span class="sxs-lookup"><span data-stu-id="09afc-242">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="09afc-243">Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="09afc-243">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09afc-244">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="09afc-244">Additional resources</span></span>

* <span data-ttu-id="09afc-245">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="09afc-245">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
