---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f53c303dc63e092f08e933fea79eb805523cde9b
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861398"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="6de7e-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="6de7e-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="6de7e-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)</span><span class="sxs-lookup"><span data-stu-id="6de7e-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6de7e-105">Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS.</span><span class="sxs-lookup"><span data-stu-id="6de7e-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="6de7e-106">При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="6de7e-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="6de7e-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6de7e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="6de7e-108">Тип развертывания</span><span class="sxs-lookup"><span data-stu-id="6de7e-108">Deployment type</span></span>

<span data-ttu-id="6de7e-109">Вы можете создать зависящее от платформы или автономное развертывание службы Windows.</span><span class="sxs-lookup"><span data-stu-id="6de7e-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="6de7e-110">Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="6de7e-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="6de7e-111">развертывание, зависящее от платформы;</span><span class="sxs-lookup"><span data-stu-id="6de7e-111">Framework-dependent deployment</span></span>

<span data-ttu-id="6de7e-112">Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6de7e-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="6de7e-113">При использовании сценария с FDD с приложением ASP.NET Core (службой Windows) с помощью пакета SDK создается исполняемый файл (*\*.exe*), который называется *исполняемым файлом, зависящим от платформы*.</span><span class="sxs-lookup"><span data-stu-id="6de7e-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="6de7e-114">автономное развертывание;</span><span class="sxs-lookup"><span data-stu-id="6de7e-114">Self-contained deployment</span></span>

<span data-ttu-id="6de7e-115">Для автономного развертывания (SCD) общие компоненты в целевой системе не требуются.</span><span class="sxs-lookup"><span data-stu-id="6de7e-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="6de7e-116">Среда выполнения и зависимости приложения развертываются с приложением в системе для размещения.</span><span class="sxs-lookup"><span data-stu-id="6de7e-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="6de7e-117">Преобразование проекта в службу Windows</span><span class="sxs-lookup"><span data-stu-id="6de7e-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="6de7e-118">Внесите следующие изменения в существующий проект ASP.NET Core, чтобы запустить приложение в качестве службы:</span><span class="sxs-lookup"><span data-stu-id="6de7e-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="6de7e-119">Обновления файла проекта</span><span class="sxs-lookup"><span data-stu-id="6de7e-119">Project file updates</span></span>

<span data-ttu-id="6de7e-120">Измените файл проекта с учетом выбранного [типа развертывания](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="6de7e-120">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="6de7e-121">Зависящее от платформы развертывание</span><span class="sxs-lookup"><span data-stu-id="6de7e-121">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="6de7e-122">Добавьте [идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows в `<PropertyGroup>`, где содержится требуемая версия платформы.</span><span class="sxs-lookup"><span data-stu-id="6de7e-122">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="6de7e-123">Добавьте свойство `<SelfContained>` со значением `false`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-123">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="6de7e-124">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-124">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="6de7e-125">Автономное развертывание</span><span class="sxs-lookup"><span data-stu-id="6de7e-125">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="6de7e-126">Подтвердите наличие [идентификатора среды выполнения](/dotnet/core/rid-catalog) Windows или добавьте его в `<PropertyGroup>`, где содержится требуемая версия платформы.</span><span class="sxs-lookup"><span data-stu-id="6de7e-126">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="6de7e-127">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-127">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="6de7e-128">Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="6de7e-128">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="6de7e-129">Укажите список идентификаторов RID, разделив их точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="6de7e-129">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="6de7e-130">Укажите имя свойства `<RuntimeIdentifiers>` (множественное число).</span><span class="sxs-lookup"><span data-stu-id="6de7e-130">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="6de7e-131">Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="6de7e-131">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="6de7e-132">Добавьте ссылку на пакет для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="6de7e-132">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="6de7e-133">Чтобы включить ведение журнала событий Windows, добавьте ссылку на пакет для [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="6de7e-133">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="6de7e-134">Дополнительные сведения см. в разделе [Обработка событий запуска и остановки](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="6de7e-134">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="6de7e-135">Обновления Program.Main</span><span class="sxs-lookup"><span data-stu-id="6de7e-135">Program.Main updates</span></span>

<span data-ttu-id="6de7e-136">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="6de7e-136">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="6de7e-137">Для тестирования и отладки при работе вне службы добавьте код, чтобы определить, как выполняется приложение: в качестве службы или консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="6de7e-137">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="6de7e-138">Проверьте, присоединен ли отладчик или присутствует ли аргумент командной строки `--console`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-138">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="6de7e-139">Если одно из условий имеет значение true (приложение выполняется не в качестве службы), вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> на веб-узле.</span><span class="sxs-lookup"><span data-stu-id="6de7e-139">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="6de7e-140">Если условия имеют значение false (приложение выполняется в качестве службы), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="6de7e-140">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="6de7e-141">Вызовите <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> и используйте путь к расположению для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="6de7e-141">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> and use a path to the app's published location.</span></span> <span data-ttu-id="6de7e-142">Не вызывайте <xref:System.IO.Directory.GetCurrentDirectory*> для получения пути, так как при вызове `GetCurrentDirectory` приложение службы Windows возвращает папку *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="6de7e-142">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when `GetCurrentDirectory` is called.</span></span> <span data-ttu-id="6de7e-143">Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="6de7e-143">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="6de7e-144">Вызовите <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, чтобы запустить приложение в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="6de7e-144">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="6de7e-145">Так как для [поставщика конфигурации командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) требуется пара имя-значение для аргументов командной строки, параметр `--console` удаляется из аргументов, прежде чем <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> получит его.</span><span class="sxs-lookup"><span data-stu-id="6de7e-145">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="6de7e-146">Для записи данных в журнал событий Windows добавьте поставщик EventLog в <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="6de7e-146">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="6de7e-147">Задайте уровень ведения журнала с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="6de7e-147">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="6de7e-148">Для демонстрации и тестирования в файле параметров примера приложения для рабочей среды мы укажем такой уровень ведения журнала: `Information`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-148">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="6de7e-149">В рабочей среде обычно присваивается значение `Error`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-149">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="6de7e-150">Дополнительные сведения см. в разделе <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="6de7e-150">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a><span data-ttu-id="6de7e-151">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="6de7e-151">Publish the app</span></span>

<span data-ttu-id="6de7e-152">Опубликуйте приложение с помощью команды [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), [профиля публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) или Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6de7e-152">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="6de7e-153">Если вы используете Visual Studio, выберите **FolderProfile** и настройте **целевое расположение**, прежде чем нажимать кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="6de7e-153">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="6de7e-154">Чтобы опубликовать пример приложения с помощью интерфейса командной строки (CLI), выполните в командной строке команду [dotnet publish](/dotnet/core/tools/dotnet-publish) в папке проекта с конфигурацией выпуска, передаваемой параметру [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="6de7e-154">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="6de7e-155">Чтобы опубликовать этот пример в папке за пределами приложения, задайте параметр [-o|--output](/dotnet/core/tools/dotnet-publish#options) и укажите путь.</span><span class="sxs-lookup"><span data-stu-id="6de7e-155">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

#### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="6de7e-156">Публикация зависящего от платформы развертывания</span><span class="sxs-lookup"><span data-stu-id="6de7e-156">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="6de7e-157">В следующем примере приложение публикуется в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="6de7e-157">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="6de7e-158">Публикация автономного развертывания</span><span class="sxs-lookup"><span data-stu-id="6de7e-158">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="6de7e-159">Идентификатор RID следует указать в свойстве `<RuntimeIdenfifier>` (или `<RuntimeIdentifiers>`) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="6de7e-159">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="6de7e-160">Укажите среду выполнения в параметре [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) команды `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-160">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="6de7e-161">В следующем примере приложение публикуется для среды выполнения `win7-x64` в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="6de7e-161">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a><span data-ttu-id="6de7e-162">Создание учетной записи пользователя</span><span class="sxs-lookup"><span data-stu-id="6de7e-162">Create a user account</span></span>

<span data-ttu-id="6de7e-163">Создайте учетную запись пользователя для службы с помощью команды `net user`:</span><span class="sxs-lookup"><span data-stu-id="6de7e-163">Create a user account for the service using the `net user` command:</span></span>

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="6de7e-164">Для примера приложения создайте учетную запись пользователя с именем `ServiceUser` и пароль.</span><span class="sxs-lookup"><span data-stu-id="6de7e-164">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="6de7e-165">В следующей команде замените `{PASSWORD}` на [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="6de7e-165">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```console
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="6de7e-166">Если вам нужно добавить пользователя в группу, используйте команду `net localgroup`, где `{GROUP}` — это имя группы:</span><span class="sxs-lookup"><span data-stu-id="6de7e-166">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="6de7e-167">Дополнительные сведения см. в статье [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей службы).</span><span class="sxs-lookup"><span data-stu-id="6de7e-167">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

### <a name="set-permissions"></a><span data-ttu-id="6de7e-168">Настройка разрешений</span><span class="sxs-lookup"><span data-stu-id="6de7e-168">Set permissions</span></span>

<span data-ttu-id="6de7e-169">Предоставьте доступ на запись, чтение и выполнение для папки приложения с помощью команды [icacls](/windows-server/administration/windows-commands/icacls):</span><span class="sxs-lookup"><span data-stu-id="6de7e-169">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="6de7e-170">`{PATH}` &ndash; путь к папке приложения.</span><span class="sxs-lookup"><span data-stu-id="6de7e-170">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="6de7e-171">`{USER ACCOUNT}` &ndash; учетная запись пользователя (SID).</span><span class="sxs-lookup"><span data-stu-id="6de7e-171">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="6de7e-172">`(OI)` &ndash; флаг наследования объекта, который распространяет разрешения на вложенные файлы.</span><span class="sxs-lookup"><span data-stu-id="6de7e-172">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="6de7e-173">`(CI)` &ndash; флаг наследования контейнера, который распространяет разрешения на вложенные папки.</span><span class="sxs-lookup"><span data-stu-id="6de7e-173">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="6de7e-174">`{PERMISSION FLAGS}` &ndash; устанавливает разрешения для доступа к приложениям.</span><span class="sxs-lookup"><span data-stu-id="6de7e-174">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="6de7e-175">Запись (`W`)</span><span class="sxs-lookup"><span data-stu-id="6de7e-175">Write (`W`)</span></span>
  * <span data-ttu-id="6de7e-176">Чтение (`R`)</span><span class="sxs-lookup"><span data-stu-id="6de7e-176">Read (`R`)</span></span>
  * <span data-ttu-id="6de7e-177">Выполнение (`X`)</span><span class="sxs-lookup"><span data-stu-id="6de7e-177">Execute (`X`)</span></span>
  * <span data-ttu-id="6de7e-178">Полное (`F`)</span><span class="sxs-lookup"><span data-stu-id="6de7e-178">Full (`F`)</span></span>
  * <span data-ttu-id="6de7e-179">Изменение (`M`)</span><span class="sxs-lookup"><span data-stu-id="6de7e-179">Modify (`M`)</span></span>
* <span data-ttu-id="6de7e-180">`/t` &ndash; применяется рекурсивно к имеющимся вложенным папкам и файлам.</span><span class="sxs-lookup"><span data-stu-id="6de7e-180">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="6de7e-181">Для примера приложения, опубликованного в папке *c:\\svc*, и учетной записи `ServiceUser` с разрешениями на запись, чтение и выполнение используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6de7e-181">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="6de7e-182">Дополнительные сведения см. в статье об [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="6de7e-182">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="manage-the-service"></a><span data-ttu-id="6de7e-183">Управление службой</span><span class="sxs-lookup"><span data-stu-id="6de7e-183">Manage the service</span></span>

### <a name="create-the-service"></a><span data-ttu-id="6de7e-184">Создание службы</span><span class="sxs-lookup"><span data-stu-id="6de7e-184">Create the service</span></span>

<span data-ttu-id="6de7e-185">Используйте программу командной строки [sc.exe](https://technet.microsoft.com/library/bb490995), чтобы создать службу.</span><span class="sxs-lookup"><span data-stu-id="6de7e-185">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="6de7e-186">Значение `binPath` обозначает путь к исполняемому файлу приложения, включая имя самого файла.</span><span class="sxs-lookup"><span data-stu-id="6de7e-186">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="6de7e-187">**Требуется пробел между знаком равенства и кавычками для каждого обязательного параметра и значения.**</span><span class="sxs-lookup"><span data-stu-id="6de7e-187">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* <span data-ttu-id="6de7e-188">`{SERVICE NAME}` &ndash; имя, присваиваемое службе в [диспетчере служб](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="6de7e-188">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
* <span data-ttu-id="6de7e-189">`{PATH}` &ndash; путь к исполняемому файлу.</span><span class="sxs-lookup"><span data-stu-id="6de7e-189">`{PATH}` &ndash; The path to the service executable.</span></span>
* <span data-ttu-id="6de7e-190">`{DOMAIN}` &ndash; домен, к которому присоединен компьютер.</span><span class="sxs-lookup"><span data-stu-id="6de7e-190">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="6de7e-191">Если компьютер не присоединен к домену, указывается имя локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="6de7e-191">If the machine isn't domain-joined, the local machine name.</span></span>
* <span data-ttu-id="6de7e-192">`{USER ACCOUNT}` &ndash; учетная запись пользователя, которая используется для запуска службы.</span><span class="sxs-lookup"><span data-stu-id="6de7e-192">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
* <span data-ttu-id="6de7e-193">`{PASSWORD}` &ndash; пароль учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="6de7e-193">`{PASSWORD}` &ndash; The user account password.</span></span>

> [!WARNING]
> <span data-ttu-id="6de7e-194">**Не** пропускайте параметр `obj`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-194">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="6de7e-195">Значение по умолчанию параметра `obj` — это [учетная запись LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="6de7e-195">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="6de7e-196">Выполнение службы под учетной записью `LocalSystem` представляет собой серьезную угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="6de7e-196">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="6de7e-197">Всегда запускайте службу, используя учетную запись пользователя с ограниченными разрешениями.</span><span class="sxs-lookup"><span data-stu-id="6de7e-197">Always run a service with a user account that has restricted privileges.</span></span>

<span data-ttu-id="6de7e-198">В примере ниже для примера приложения указано следующее:</span><span class="sxs-lookup"><span data-stu-id="6de7e-198">In the following example for the sample app:</span></span>

* <span data-ttu-id="6de7e-199">Служба называется **MyService**.</span><span class="sxs-lookup"><span data-stu-id="6de7e-199">The service is named **MyService**.</span></span>
* <span data-ttu-id="6de7e-200">Опубликованная служба размещается в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="6de7e-200">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="6de7e-201">Исполняемый файл приложения с именем *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="6de7e-201">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="6de7e-202">Значение `binPath` заключается в двойные кавычки (").</span><span class="sxs-lookup"><span data-stu-id="6de7e-202">Enclose the `binPath` value in double quotation marks (").</span></span>
* <span data-ttu-id="6de7e-203">Служба работает под учетной записью `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-203">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="6de7e-204">Замените `{DOMAIN}` на домен учетной записи пользователя или имя локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="6de7e-204">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="6de7e-205">Значение `obj` заключается в двойные кавычки (").</span><span class="sxs-lookup"><span data-stu-id="6de7e-205">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="6de7e-206">Пример: если система размещения — это локальный компьютер с именем `MairaPC`, задайте для параметра `obj` значение `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-206">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
* <span data-ttu-id="6de7e-207">Замените `{PASSWORD}` на пароль учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="6de7e-207">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="6de7e-208">Значение `password` заключается в двойные кавычки (").</span><span class="sxs-lookup"><span data-stu-id="6de7e-208">Enclose the `password` value in double quotation marks (").</span></span>

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> <span data-ttu-id="6de7e-209">Убедитесь, что между знаками равенства и значениями параметров есть пробелы.</span><span class="sxs-lookup"><span data-stu-id="6de7e-209">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

### <a name="start-the-service"></a><span data-ttu-id="6de7e-210">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="6de7e-210">Start the service</span></span>

<span data-ttu-id="6de7e-211">Запустите службу с помощью команды `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-211">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

<span data-ttu-id="6de7e-212">Чтобы запустить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6de7e-212">To start the sample app service, use the following command:</span></span>

```console
sc start MyService
```

<span data-ttu-id="6de7e-213">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="6de7e-213">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="6de7e-214">Определение состояния службы</span><span class="sxs-lookup"><span data-stu-id="6de7e-214">Determine the service status</span></span>

<span data-ttu-id="6de7e-215">Чтобы проверить состояние службы, используйте команду `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-215">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="6de7e-216">Состояние отображается одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="6de7e-216">The status is reported as one of the following values:</span></span>

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

<span data-ttu-id="6de7e-217">Чтобы проверить состояние примера службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6de7e-217">Use the following command to check the status of the sample app service:</span></span>

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="6de7e-218">Обзор службы веб-приложений</span><span class="sxs-lookup"><span data-stu-id="6de7e-218">Browse a web app service</span></span>

<span data-ttu-id="6de7e-219">Если служба находится в состоянии `RUNNING` и является веб-приложением, найдите приложение по его пути (по умолчанию `http://localhost:5000`, который перенаправляет на `https://localhost:5001` при использовании [ПО промежуточного слоя перенаправления на HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="6de7e-219">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="6de7e-220">Чтобы получить пример службы приложений, найдите приложение по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-220">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="6de7e-221">Остановите службу</span><span class="sxs-lookup"><span data-stu-id="6de7e-221">Stop the service</span></span>

<span data-ttu-id="6de7e-222">Остановите службу с помощью команды `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-222">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

<span data-ttu-id="6de7e-223">Чтобы остановить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6de7e-223">The following command stops the sample app service:</span></span>

```console
sc stop MyService
```

### <a name="delete-the-service"></a><span data-ttu-id="6de7e-224">Удаление службы</span><span class="sxs-lookup"><span data-stu-id="6de7e-224">Delete the service</span></span>

<span data-ttu-id="6de7e-225">После небольшой задержки для остановки службы удалите службу с помощью команды `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="6de7e-225">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

<span data-ttu-id="6de7e-226">Проверьте состояние примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="6de7e-226">Check the status of the sample app service:</span></span>

```console
sc query MyService
```

<span data-ttu-id="6de7e-227">Когда пример службы приложений находится в состоянии `STOPPED`, используйте следующую команду для удаления примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="6de7e-227">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="6de7e-228">Обработка событий запуска и остановки</span><span class="sxs-lookup"><span data-stu-id="6de7e-228">Handle starting and stopping events</span></span>

<span data-ttu-id="6de7e-229">Чтобы правильно обрабатывать события <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> и <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, внесите дополнительные изменения:</span><span class="sxs-lookup"><span data-stu-id="6de7e-229">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="6de7e-230">Создайте класс, производный от <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService>, с методами `OnStarting`, `OnStarted` и `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="6de7e-230">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="6de7e-231">Создайте метод расширения для <xref:Microsoft.AspNetCore.Hosting.IWebHost>, который передает `CustomWebHostService` в <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="6de7e-231">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="6de7e-232">В `Program.Main` вызовите метод расширения `RunAsCustomService` вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="6de7e-232">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="6de7e-233">Чтобы узнать расположение <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> в `Program.Main`, см. пример кода, приведенный в разделе [Преобразование проекта в службу Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="6de7e-233">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="6de7e-234">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="6de7e-234">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="6de7e-235">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="6de7e-235">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="6de7e-236">Для получения дополнительной информации см. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="6de7e-236">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="6de7e-237">Настройка HTTPS</span><span class="sxs-lookup"><span data-stu-id="6de7e-237">Configure HTTPS</span></span>

<span data-ttu-id="6de7e-238">Чтобы настроить службу с защищенной конечной точкой, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="6de7e-238">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="6de7e-239">Создайте сертификат X.509 для системы размещения с помощью механизмов получения и развертывания сертификата вашей платформы.</span><span class="sxs-lookup"><span data-stu-id="6de7e-239">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="6de7e-240">Укажите [конфигурацию конечной точки HTTPS для сервера Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration), чтобы использовать сертификат.</span><span class="sxs-lookup"><span data-stu-id="6de7e-240">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="6de7e-241">Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="6de7e-241">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="6de7e-242">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="6de7e-242">Current directory and content root</span></span>

<span data-ttu-id="6de7e-243">Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="6de7e-243">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="6de7e-244">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="6de7e-244">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="6de7e-245">Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="6de7e-245">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="6de7e-246">Указание папки приложения в качестве пути корневого каталога содержимого</span><span class="sxs-lookup"><span data-stu-id="6de7e-246">Set the content root path to the app's folder</span></span>

<span data-ttu-id="6de7e-247"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> — это тот же путь, предоставленный аргументом `binPath` при создании службы.</span><span class="sxs-lookup"><span data-stu-id="6de7e-247">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="6de7e-248">Чтобы не вызывать метод `GetCurrentDirectory` для создания путей к файлам параметров, вызовите <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> с указанным путем к корневому каталогу содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="6de7e-248">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> with the path to the app's content root.</span></span>

<span data-ttu-id="6de7e-249">В `Program.Main` определите путь к папке с исполняемым файлом службы и используйте этот путь, чтобы создать корневой каталог содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="6de7e-249">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="6de7e-250">Хранение файлов службы в подходящем месте на диске</span><span class="sxs-lookup"><span data-stu-id="6de7e-250">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="6de7e-251">Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6de7e-251">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6de7e-252">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6de7e-252">Additional resources</span></span>

* <span data-ttu-id="6de7e-253">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="6de7e-253">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
