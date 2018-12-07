---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.2'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f857e96108b68bb6ec64a85910bf4d889cdf2822
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458521"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="ccfbc-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="ccfbc-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="ccfbc-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)</span><span class="sxs-lookup"><span data-stu-id="ccfbc-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ccfbc-105">Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="ccfbc-106">При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="ccfbc-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ccfbc-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="ccfbc-108">Тип развертывания</span><span class="sxs-lookup"><span data-stu-id="ccfbc-108">Deployment type</span></span>

<span data-ttu-id="ccfbc-109">Вы можете создать зависящее от платформы или автономное развертывание службы Windows.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="ccfbc-110">Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="ccfbc-111">развертывание, зависящее от платформы;</span><span class="sxs-lookup"><span data-stu-id="ccfbc-111">Framework-dependent deployment</span></span>

<span data-ttu-id="ccfbc-112">Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="ccfbc-113">При использовании сценария с FDD с приложением ASP.NET Core (службой Windows) с помощью пакета SDK создается исполняемый файл (*\*.exe*), который называется *исполняемым файлом, зависящим от платформы*.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="ccfbc-114">автономное развертывание;</span><span class="sxs-lookup"><span data-stu-id="ccfbc-114">Self-contained deployment</span></span>

<span data-ttu-id="ccfbc-115">Для автономного развертывания (SCD) общие компоненты в целевой системе не требуются.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="ccfbc-116">Среда выполнения и зависимости приложения развертываются с приложением в системе для размещения.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="ccfbc-117">Преобразование проекта в службу Windows</span><span class="sxs-lookup"><span data-stu-id="ccfbc-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="ccfbc-118">Внесите следующие изменения в существующий проект ASP.NET Core, чтобы запустить приложение в качестве службы:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

1. <span data-ttu-id="ccfbc-119">Измените файл проекта с учетом выбранного [типа развертывания](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-119">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

   * <span data-ttu-id="ccfbc-120">**Зависящее от платформы развертывание** &ndash; добавьте [идентификатор среды выполнения](/dotnet/core/rid-catalog) в `<PropertyGroup>`, где содержится требуемая версия платформы.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-120">**Framework-dependent Deployment (FDD)** &ndash; Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="ccfbc-121">Добавьте свойство `<SelfContained>` со значением `false`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-121">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="ccfbc-122">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-122">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

     ```xml
     <PropertyGroup>
       <TargetFramework>netcoreapp2.2</TargetFramework>
       <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
       <SelfContained>false</SelfContained>
       <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
     </PropertyGroup>
     ```

     <span data-ttu-id="ccfbc-123">**Автономное развертывание** &ndash; подтвердите наличие [идентификатора среды выполнения](/dotnet/core/rid-catalog) Windows или добавьте его в `<PropertyGroup>`, где содержится требуемая версия платформы.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-123">**Self-contained Deployment (SCD)** &ndash; Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="ccfbc-124">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-124">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

     ```xml
     <PropertyGroup>
       <TargetFramework>netcoreapp2.2</TargetFramework>
       <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
       <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
     </PropertyGroup>
     ```

     <span data-ttu-id="ccfbc-125">Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-125">To publish for multiple RIDs:</span></span>

     * <span data-ttu-id="ccfbc-126">Укажите список идентификаторов RID, разделив их точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-126">Provide the RIDs in a semicolon-delimited list.</span></span>
     * <span data-ttu-id="ccfbc-127">Укажите имя свойства `<RuntimeIdentifiers>` (множественное число).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-127">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

     <span data-ttu-id="ccfbc-128">Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-128">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="ccfbc-129">Добавьте ссылку на пакет для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-129">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

   * <span data-ttu-id="ccfbc-130">Чтобы включить ведение журнала событий Windows, добавьте ссылку на пакет для [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-130">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

     <span data-ttu-id="ccfbc-131">Дополнительные сведения см. в разделе [Обработка событий запуска и остановки](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-131">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

1. <span data-ttu-id="ccfbc-132">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-132">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="ccfbc-133">Для тестирования и отладки при работе вне службы добавьте код, чтобы определить, как выполняется приложение: в качестве службы или консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-133">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="ccfbc-134">Проверьте, присоединен ли отладчик или присутствует ли аргумент командной строки `--console`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-134">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

     <span data-ttu-id="ccfbc-135">Если одно из условий имеет значение true (приложение выполняется не в качестве службы), вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> на веб-узле.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-135">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

     <span data-ttu-id="ccfbc-136">Если условия имеют значение false (приложение выполняется в качестве службы), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-136">If the conditions are false (the app is run as a service):</span></span>

     * <span data-ttu-id="ccfbc-137">Вызовите <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> и используйте путь к расположению для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-137">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> and use a path to the app's published location.</span></span> <span data-ttu-id="ccfbc-138">Не вызывайте <xref:System.IO.Directory.GetCurrentDirectory*> для получения пути, так как при вызове `GetCurrentDirectory` приложение службы Windows возвращает папку *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-138">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when `GetCurrentDirectory` is called.</span></span> <span data-ttu-id="ccfbc-139">Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-139">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
     * <span data-ttu-id="ccfbc-140">Вызовите <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, чтобы запустить приложение в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-140">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

     <span data-ttu-id="ccfbc-141">Так как для [поставщика конфигурации командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) требуется пара имя-значение для аргументов командной строки, параметр `--console` удаляется из аргументов, прежде чем <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> получит его.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-141">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

   * <span data-ttu-id="ccfbc-142">Для записи данных в журнал событий Windows добавьте поставщик EventLog в <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-142">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="ccfbc-143">Задайте уровень ведения журнала с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-143">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="ccfbc-144">Для демонстрации и тестирования в файле параметров примера приложения для рабочей среды мы укажем такой уровень ведения журнала: `Information`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-144">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="ccfbc-145">В рабочей среде обычно присваивается значение `Error`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-145">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="ccfbc-146">Дополнительные сведения см. в разделе <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-146">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

1. <span data-ttu-id="ccfbc-147">Опубликуйте приложение с помощью команды [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), [профиля публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) или Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-147">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="ccfbc-148">Если вы используете Visual Studio, выберите **FolderProfile** и настройте **целевое расположение**, прежде чем нажимать кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-148">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="ccfbc-149">Чтобы опубликовать пример приложения с помощью интерфейса командной строки (CLI), выполните в командной строке команду [dotnet publish](/dotnet/core/tools/dotnet-publish) в папке проекта с конфигурацией выпуска, передаваемой параметру [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-149">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="ccfbc-150">Чтобы опубликовать этот пример в папке за пределами приложения, задайте параметр [-o|--output](/dotnet/core/tools/dotnet-publish#options) и укажите путь.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-150">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

   * <span data-ttu-id="ccfbc-151">**Развертывание, зависящее от платформы**</span><span class="sxs-lookup"><span data-stu-id="ccfbc-151">**Framework-dependent Deployment (FDD)**</span></span>

     <span data-ttu-id="ccfbc-152">В следующем примере приложение публикуется в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-152">In the following example, the app is published to the *c:\\svc* folder:</span></span>

     ```console
     dotnet publish --configuration Release --output c:\svc
     ```

   * <span data-ttu-id="ccfbc-153">**Автономное развертывание** &ndash; идентификатор RID следует указать в свойстве `<RuntimeIdenfifier>` (или `<RuntimeIdentifiers>`) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-153">**Self-contained Deployment (SCD)** &ndash; The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="ccfbc-154">Укажите среду выполнения в параметре [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) команды `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-154">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

     <span data-ttu-id="ccfbc-155">В следующем примере приложение публикуется для среды выполнения `win7-x64` в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-155">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

     ```console
     dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
     ```

1. <span data-ttu-id="ccfbc-156">Создайте учетную запись пользователя для службы с помощью команды `net user`:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-156">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="ccfbc-157">Для примера приложения создайте учетную запись пользователя с именем `ServiceUser` и пароль.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-157">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="ccfbc-158">В следующей команде замените `{PASSWORD}` на [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-158">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="ccfbc-159">Если вам нужно добавить пользователя в группу, используйте команду `net localgroup`, где `{GROUP}` — это имя группы:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-159">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="ccfbc-160">Дополнительные сведения см. в статье [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей службы).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-160">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="ccfbc-161">Предоставьте доступ на запись, чтение и выполнение для папки приложения с помощью команды [icacls](/windows-server/administration/windows-commands/icacls):</span><span class="sxs-lookup"><span data-stu-id="ccfbc-161">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="ccfbc-162">`{PATH}` &ndash; путь к папке приложения.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-162">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="ccfbc-163">`{USER ACCOUNT}` &ndash; учетная запись пользователя (SID).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-163">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="ccfbc-164">`(OI)` &ndash; флаг наследования объекта, который распространяет разрешения на вложенные файлы.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-164">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="ccfbc-165">`(CI)` &ndash; флаг наследования контейнера, который распространяет разрешения на вложенные папки.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-165">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="ccfbc-166">`{PERMISSION FLAGS}` &ndash; устанавливает разрешения для доступа к приложениям.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-166">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="ccfbc-167">Запись (`W`)</span><span class="sxs-lookup"><span data-stu-id="ccfbc-167">Write (`W`)</span></span>
     * <span data-ttu-id="ccfbc-168">Чтение (`R`)</span><span class="sxs-lookup"><span data-stu-id="ccfbc-168">Read (`R`)</span></span>
     * <span data-ttu-id="ccfbc-169">Выполнение (`X`)</span><span class="sxs-lookup"><span data-stu-id="ccfbc-169">Execute (`X`)</span></span>
     * <span data-ttu-id="ccfbc-170">Полное (`F`)</span><span class="sxs-lookup"><span data-stu-id="ccfbc-170">Full (`F`)</span></span>
     * <span data-ttu-id="ccfbc-171">Изменение (`M`)</span><span class="sxs-lookup"><span data-stu-id="ccfbc-171">Modify (`M`)</span></span>
   * <span data-ttu-id="ccfbc-172">`/t` &ndash; применяется рекурсивно к имеющимся вложенным папкам и файлам.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-172">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="ccfbc-173">Для примера приложения, опубликованного в папке *c:\\svc*, и учетной записи `ServiceUser` с разрешениями на запись, чтение и выполнение используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-173">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="ccfbc-174">Дополнительные сведения см. в статье об [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-174">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="ccfbc-175">Используйте программу командной строки [sc.exe](https://technet.microsoft.com/library/bb490995), чтобы создать службу.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-175">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="ccfbc-176">Значение `binPath` обозначает путь к исполняемому файлу приложения, включая имя самого файла.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-176">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="ccfbc-177">**Требуется пробел между знаком равенства и кавычками для каждого обязательного параметра и значения.**</span><span class="sxs-lookup"><span data-stu-id="ccfbc-177">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="ccfbc-178">`{SERVICE NAME}` &ndash; имя, присваиваемое службе в [диспетчере служб](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-178">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="ccfbc-179">`{PATH}` &ndash; путь к исполняемому файлу.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-179">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="ccfbc-180">`{DOMAIN}` &ndash; домен, к которому присоединен компьютер.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-180">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="ccfbc-181">Если компьютер не присоединен к домену, указывается имя локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-181">If the machine isn't domain-joined, the local machine name.</span></span>
   * <span data-ttu-id="ccfbc-182">`{USER ACCOUNT}` &ndash; учетная запись пользователя, которая используется для запуска службы.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-182">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
   * <span data-ttu-id="ccfbc-183">`{PASSWORD}` &ndash; пароль учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-183">`{PASSWORD}` &ndash; The user account password.</span></span>

   > [!WARNING]
   > <span data-ttu-id="ccfbc-184">**Не** пропускайте параметр `obj`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-184">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="ccfbc-185">Значение по умолчанию параметра `obj` — это [учетная запись LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-185">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="ccfbc-186">Выполнение службы под учетной записью `LocalSystem` представляет собой серьезную угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-186">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="ccfbc-187">Всегда запускайте службу, используя учетную запись пользователя с ограниченными разрешениями.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-187">Always run a service with a user account that has restricted privileges.</span></span>

   <span data-ttu-id="ccfbc-188">В примере ниже для примера приложения указано следующее:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-188">In the following example for the sample app:</span></span>

   * <span data-ttu-id="ccfbc-189">Служба называется **MyService**.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-189">The service is named **MyService**.</span></span>
   * <span data-ttu-id="ccfbc-190">Опубликованная служба размещается в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-190">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="ccfbc-191">Исполняемый файл приложения с именем *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-191">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="ccfbc-192">Значение `binPath` заключается в двойные кавычки (").</span><span class="sxs-lookup"><span data-stu-id="ccfbc-192">Enclose the `binPath` value in double quotation marks (").</span></span>
   * <span data-ttu-id="ccfbc-193">Служба работает под учетной записью `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-193">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="ccfbc-194">Замените `{DOMAIN}` на домен учетной записи пользователя или имя локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-194">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="ccfbc-195">Значение `obj` заключается в двойные кавычки (").</span><span class="sxs-lookup"><span data-stu-id="ccfbc-195">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="ccfbc-196">Пример: если система размещения — это локальный компьютер с именем `MairaPC`, задайте для параметра `obj` значение `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-196">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="ccfbc-197">Замените `{PASSWORD}` на пароль учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-197">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="ccfbc-198">Значение `password` заключается в двойные кавычки (").</span><span class="sxs-lookup"><span data-stu-id="ccfbc-198">Enclose the `password` value in double quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="ccfbc-199">Убедитесь, что между знаками равенства и значениями параметров есть пробелы.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-199">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="ccfbc-200">Запустите службу с помощью команды `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-200">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="ccfbc-201">Чтобы запустить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-201">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="ccfbc-202">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-202">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="ccfbc-203">Чтобы проверить состояние службы, используйте команду `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-203">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="ccfbc-204">Состояние отображается одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-204">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="ccfbc-205">Чтобы проверить состояние примера службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-205">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="ccfbc-206">Если служба находится в состоянии `RUNNING` и является веб-приложением, найдите приложение по его пути (по умолчанию `http://localhost:5000`, который перенаправляет на `https://localhost:5001` при использовании [ПО промежуточного слоя перенаправления на HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-206">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="ccfbc-207">Чтобы получить пример службы приложений, найдите приложение по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-207">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="ccfbc-208">Остановите службу с помощью команды `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-208">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="ccfbc-209">Чтобы остановить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-209">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="ccfbc-210">После небольшой задержки для остановки службы удалите службу с помощью команды `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-210">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="ccfbc-211">Проверьте состояние примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-211">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="ccfbc-212">Когда пример службы приложений находится в состоянии `STOPPED`, используйте следующую команду для удаления примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-212">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="ccfbc-213">Обработка событий запуска и остановки</span><span class="sxs-lookup"><span data-stu-id="ccfbc-213">Handle starting and stopping events</span></span>

<span data-ttu-id="ccfbc-214">Чтобы правильно обрабатывать события <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> и <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, внесите дополнительные изменения:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-214">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="ccfbc-215">Создайте класс, производный от <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService>, с методами `OnStarting`, `OnStarted` и `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-215">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="ccfbc-216">Создайте метод расширения для <xref:Microsoft.AspNetCore.Hosting.IWebHost>, который передает `CustomWebHostService` в <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-216">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="ccfbc-217">В `Program.Main` вызовите метод расширения `RunAsCustomService` вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-217">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="ccfbc-218">Чтобы узнать расположение <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> в `Program.Main`, см. пример кода, приведенный в разделе [Преобразование проекта в службу Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-218">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ccfbc-219">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="ccfbc-219">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ccfbc-220">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-220">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="ccfbc-221">Дополнительные сведения см. в разделе <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-221">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="ccfbc-222">Настройка HTTPS</span><span class="sxs-lookup"><span data-stu-id="ccfbc-222">Configure HTTPS</span></span>

<span data-ttu-id="ccfbc-223">Чтобы настроить службу с защищенной конечной точкой, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="ccfbc-223">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="ccfbc-224">Создайте сертификат X.509 для системы размещения с помощью механизмов получения и развертывания сертификата вашей платформы.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-224">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="ccfbc-225">Укажите [конфигурацию конечной точки HTTPS для сервера Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration), чтобы использовать сертификат.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-225">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="ccfbc-226">Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-226">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="ccfbc-227">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="ccfbc-227">Current directory and content root</span></span>

<span data-ttu-id="ccfbc-228">Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-228">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="ccfbc-229">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="ccfbc-229">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="ccfbc-230">Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-230">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="ccfbc-231">Указание папки приложения в качестве пути корневого каталога содержимого</span><span class="sxs-lookup"><span data-stu-id="ccfbc-231">Set the content root path to the app's folder</span></span>

<span data-ttu-id="ccfbc-232"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> — это тот же путь, предоставленный аргументом `binPath` при создании службы.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-232">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="ccfbc-233">Чтобы не вызывать метод `GetCurrentDirectory` для создания путей к файлам параметров, вызовите <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> с указанным путем к корневому каталогу содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-233">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> with the path to the app's content root.</span></span>

<span data-ttu-id="ccfbc-234">В `Program.Main` определите путь к папке с исполняемым файлом службы и используйте этот путь, чтобы создать корневой каталог содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-234">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="ccfbc-235">Хранение файлов службы в подходящем месте на диске</span><span class="sxs-lookup"><span data-stu-id="ccfbc-235">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="ccfbc-236">Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ccfbc-236">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ccfbc-237">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ccfbc-237">Additional resources</span></span>

* <span data-ttu-id="ccfbc-238">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="ccfbc-238">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
