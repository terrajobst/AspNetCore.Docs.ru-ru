---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/windows-service
ms.openlocfilehash: 829c282606e60a80682233555e1268acb706090e
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172331"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="32ed5-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="32ed5-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="32ed5-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="32ed5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32ed5-105">Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS.</span><span class="sxs-lookup"><span data-stu-id="32ed5-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="32ed5-106">При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки сервера.</span><span class="sxs-lookup"><span data-stu-id="32ed5-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="32ed5-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32ed5-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32ed5-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="32ed5-108">Prerequisites</span></span>

* [<span data-ttu-id="32ed5-109">Пакет SDK для ASP.NET Core 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32ed5-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="32ed5-110">PowerShell 6.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32ed5-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="worker-service-template"></a><span data-ttu-id="32ed5-111">Шаблон службы рабочей роли</span><span class="sxs-lookup"><span data-stu-id="32ed5-111">Worker Service template</span></span>

<span data-ttu-id="32ed5-112">Шаблон службы рабочей роли ASP.NET Core может служить отправной точкой для написания длительно выполняющихся приложений служб.</span><span class="sxs-lookup"><span data-stu-id="32ed5-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="32ed5-113">Чтобы использовать шаблон в качестве основы для приложения службы Windows, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="32ed5-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="32ed5-114">Создайте приложение службы рабочей роли на основе шаблона .NET Core.</span><span class="sxs-lookup"><span data-stu-id="32ed5-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="32ed5-115">Согласно указаниям в разделе [Конфигурация приложения](#app-configuration) измените приложение службы рабочей роли так, чтобы оно могло выполняться как служба Windows.</span><span class="sxs-lookup"><span data-stu-id="32ed5-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="app-configuration"></a><span data-ttu-id="32ed5-116">Конфигурация приложения</span><span class="sxs-lookup"><span data-stu-id="32ed5-116">App configuration</span></span>

<span data-ttu-id="32ed5-117">Приложению требуется ссылка на пакет для [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="32ed5-117">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="32ed5-118">`IHostBuilder.UseWindowsService` вызывается при сборке узла.</span><span class="sxs-lookup"><span data-stu-id="32ed5-118">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="32ed5-119">Если приложение выполняется как служба Windows, метод отвечает за следующие действия:</span><span class="sxs-lookup"><span data-stu-id="32ed5-119">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="32ed5-120">Задает для узла время существования `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-120">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="32ed5-121">Задает для [корневого каталога содержимого](xref:fundamentals/index#content-root) значение [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="32ed5-121">Sets the [content root](xref:fundamentals/index#content-root) to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span> <span data-ttu-id="32ed5-122">Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="32ed5-122">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
* <span data-ttu-id="32ed5-123">Активирует ведение журнала событий.</span><span class="sxs-lookup"><span data-stu-id="32ed5-123">Enables logging to the event log:</span></span>
  * <span data-ttu-id="32ed5-124">В качестве имени источника по умолчанию используется имя приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-124">The application name is used as the default source name.</span></span>
  * <span data-ttu-id="32ed5-125">Уровень ведения журнала по умолчанию — как минимум *Предупреждение* для приложения на базе шаблона ASP.NET Core, вызывающего `CreateDefaultBuilder` для сборки узла.</span><span class="sxs-lookup"><span data-stu-id="32ed5-125">The default log level is *Warning* or higher for an app based on an ASP.NET Core template that calls `CreateDefaultBuilder` to build the host.</span></span>
  * <span data-ttu-id="32ed5-126">Уровень ведения журнала по умолчанию переопределяется с помощью ключа `Logging:EventLog:LogLevel:Default` в *appsettings.json*/*appsettings.{Environment}.json* или в другом поставщике конфигурации.</span><span class="sxs-lookup"><span data-stu-id="32ed5-126">Override the default log level with the `Logging:EventLog:LogLevel:Default` key in *appsettings.json*/*appsettings.{Environment}.json* or other configuration provider.</span></span>
  * <span data-ttu-id="32ed5-127">Только администраторы могут создавать источники событий.</span><span class="sxs-lookup"><span data-stu-id="32ed5-127">Only administrators can create new event sources.</span></span> <span data-ttu-id="32ed5-128">Если источник событий создать нельзя, используя имя приложения, для источника *Приложение* регистрируется предупреждение и журналы событий отключаются.</span><span class="sxs-lookup"><span data-stu-id="32ed5-128">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="32ed5-129">В разделе `CreateHostBuilder` файла *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="32ed5-129">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="32ed5-130">Этот раздел сопровождают следующие примеры приложений:</span><span class="sxs-lookup"><span data-stu-id="32ed5-130">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="32ed5-131">Пример фоновой службы рабочих ролей &ndash; пример приложения, не являющегося веб-приложением, на основе [шаблона службы рабочих ролей](#worker-service-template), который использует [размещенные службы](xref:fundamentals/host/hosted-services) для фоновых задач.</span><span class="sxs-lookup"><span data-stu-id="32ed5-131">Background Worker Service Sample &ndash; A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="32ed5-132">Пример службы веб-приложений &ndash; пример веб-приложения Razor Pages, который выполняется как служба Windows с [размещенными службами](xref:fundamentals/host/hosted-services) для фоновых задач.</span><span class="sxs-lookup"><span data-stu-id="32ed5-132">Web App Service Sample &ndash; A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="32ed5-133">Рекомендации по MVC см. в статьях <xref:mvc/overview> и <xref:migration/22-to-30>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-133">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

## <a name="deployment-type"></a><span data-ttu-id="32ed5-134">Тип развертывания</span><span class="sxs-lookup"><span data-stu-id="32ed5-134">Deployment type</span></span>

<span data-ttu-id="32ed5-135">Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="32ed5-135">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="32ed5-136">SDK</span><span class="sxs-lookup"><span data-stu-id="32ed5-136">SDK</span></span>

<span data-ttu-id="32ed5-137">Для службы на основе веб-приложений, которая использует платформы MVC или Razor Pages, укажите веб-пакет SDK в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="32ed5-137">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="32ed5-138">Если служба выполняет только фоновые задачи (например, [размещенные службы](xref:fundamentals/host/hosted-services)), укажите пакет SDK рабочей роли в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="32ed5-138">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="32ed5-139">Зависящее от платформы развертывание (FDD)</span><span class="sxs-lookup"><span data-stu-id="32ed5-139">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="32ed5-140">Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core.</span><span class="sxs-lookup"><span data-stu-id="32ed5-140">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="32ed5-141">Если сценарий с FDD используется согласно инструкций в этой статье, пакет SDK создаcт исполняемый файл ( *.exe*), который называется *исполняемым файлом, зависящим от платформы*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-141">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="32ed5-142">При использовании [веб-пакета SDK](#sdk), файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="32ed5-142">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="32ed5-143">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-143">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="32ed5-144">Автономное развертывание</span><span class="sxs-lookup"><span data-stu-id="32ed5-144">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="32ed5-145">Для автономного развертывания необязательно наличие общей платформы в системе размещения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-145">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="32ed5-146">Среда выполнения и зависимости приложения развертываются с приложением.</span><span class="sxs-lookup"><span data-stu-id="32ed5-146">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="32ed5-147">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows включается в `<PropertyGroup>`, где содержится целевая платформа:</span><span class="sxs-lookup"><span data-stu-id="32ed5-147">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="32ed5-148">Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="32ed5-148">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="32ed5-149">Укажите список идентификаторов RID, разделив их точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="32ed5-149">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="32ed5-150">Используйте имя свойства [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (во множественном числе).</span><span class="sxs-lookup"><span data-stu-id="32ed5-150">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="32ed5-151">Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="32ed5-151">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

## <a name="service-user-account"></a><span data-ttu-id="32ed5-152">Учетная запись пользователя службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-152">Service user account</span></span>

<span data-ttu-id="32ed5-153">Создайте для службы учетную запись пользователя с помощью командлета [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) в административной оболочке PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="32ed5-153">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="32ed5-154">Обновление Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763) или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="32ed5-154">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="32ed5-155">Версия Windows, предшествующая обновлению Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="32ed5-155">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="32ed5-156">Укажите [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="32ed5-156">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="32ed5-157">Параметр срока действия учетной записи <xref:System.DateTime> можно задать с помощью параметра `-AccountExpires`, передаваемого в командлет [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser).</span><span class="sxs-lookup"><span data-stu-id="32ed5-157">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="32ed5-158">Дополнительные сведения см. в статьях [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) и [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей служб).</span><span class="sxs-lookup"><span data-stu-id="32ed5-158">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="32ed5-159">Альтернативный подход к управлению пользователями при работе с Active Directory заключается в применении управляемых учетных записей служб.</span><span class="sxs-lookup"><span data-stu-id="32ed5-159">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="32ed5-160">Дополнительные сведения см. в [обзоре групповых управляемых учетных записей службы](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="32ed5-160">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="32ed5-161">Права на вход в качестве службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-161">Log on as a service rights</span></span>

<span data-ttu-id="32ed5-162">Чтобы настроить право *Вход в качестве службы* для учетной записи пользователя службы, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="32ed5-162">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="32ed5-163">Откройте редактор локальной политики безопасности, запустив *secpol.msc*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-163">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="32ed5-164">Разверните узел **Локальные политики** и выберите **Назначение прав пользователя**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-164">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="32ed5-165">Откройте политику **Вход в качестве службы**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-165">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="32ed5-166">Щелкните **Добавить пользователя или группу**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-166">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="32ed5-167">Укажите имя объекта (учетная запись пользователя) одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="32ed5-167">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="32ed5-168">Укажите учетную запись пользователя (`{DOMAIN OR COMPUTER NAME\USER}`) в поле для имени объекта и нажмите **ОК**, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="32ed5-168">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="32ed5-169">Выберите **Дополнительно**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-169">Select **Advanced**.</span></span> <span data-ttu-id="32ed5-170">Нажмите **Найти**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-170">Select **Find Now**.</span></span> <span data-ttu-id="32ed5-171">Выберите учетную запись пользователя из списка.</span><span class="sxs-lookup"><span data-stu-id="32ed5-171">Select the user account from the list.</span></span> <span data-ttu-id="32ed5-172">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-172">Select **OK**.</span></span> <span data-ttu-id="32ed5-173">Нажмите **ОК** еще раз, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="32ed5-173">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="32ed5-174">Нажмите **ОК** или **Применить**, чтобы сохранить изменения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-174">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="32ed5-175">Создание службы Windows и управление ею</span><span class="sxs-lookup"><span data-stu-id="32ed5-175">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="32ed5-176">Создание службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-176">Create a service</span></span>

<span data-ttu-id="32ed5-177">Зарегистрируйте службу с помощью команды PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32ed5-177">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="32ed5-178">В административной оболочке PowerShell 6 выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="32ed5-178">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="32ed5-179">`{EXE PATH}` &ndash; — путь к папке приложения на узле (например, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-179">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="32ed5-180">Не включайте исполняемый файл приложения в путь.</span><span class="sxs-lookup"><span data-stu-id="32ed5-180">Don't include the app's executable in the path.</span></span> <span data-ttu-id="32ed5-181">Завершающая косая черта не требуется.</span><span class="sxs-lookup"><span data-stu-id="32ed5-181">A trailing slash isn't required.</span></span>
* <span data-ttu-id="32ed5-182">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; —учетная запись пользователя службы (например, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-182">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="32ed5-183">`{SERVICE NAME}` &ndash; — имя службы (например, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-183">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="32ed5-184">`{EXE FILE PATH}` &ndash; — путь к исполняемому файлу приложения (например, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-184">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="32ed5-185">Включите имя исполняемого файла с расширением.</span><span class="sxs-lookup"><span data-stu-id="32ed5-185">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="32ed5-186">`{DESCRIPTION}` &ndash; — описание службы (например, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-186">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="32ed5-187">`{DISPLAY NAME}` &ndash; — отображаемое имя службы (например, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-187">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="32ed5-188">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-188">Start a service</span></span>

<span data-ttu-id="32ed5-189">Запустите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-189">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="32ed5-190">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="32ed5-190">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="32ed5-191">Определение состояния службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-191">Determine a service's status</span></span>

<span data-ttu-id="32ed5-192">Чтобы проверить состояние службы, используйте следующую команду PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-192">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="32ed5-193">Состояние отображается одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="32ed5-193">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="32ed5-194">Остановка службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-194">Stop a service</span></span>

<span data-ttu-id="32ed5-195">Остановите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-195">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="32ed5-196">Удаление службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-196">Remove a service</span></span>

<span data-ttu-id="32ed5-197">После небольшой задержки для остановки службы удалите службу с помощью следующей команды Powershell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-197">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="32ed5-198">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="32ed5-198">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="32ed5-199">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="32ed5-199">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="32ed5-200">Для получения дополнительной информации см. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-200">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="32ed5-201">Настройка конечных точек</span><span class="sxs-lookup"><span data-stu-id="32ed5-201">Configure endpoints</span></span>

<span data-ttu-id="32ed5-202">По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-202">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="32ed5-203">Настройте URL-адрес и порт, задав переменную среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-203">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="32ed5-204">Дополнительные сведения о подходах к настройке URL-адресов и портов см. в соответствующей статье сервера:</span><span class="sxs-lookup"><span data-stu-id="32ed5-204">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="32ed5-205">В предыдущем руководстве рассматривается поддержка конечных точек HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32ed5-205">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="32ed5-206">Например, настройте приложение для HTTPS, если проверка подлинности используется со службой Windows.</span><span class="sxs-lookup"><span data-stu-id="32ed5-206">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="32ed5-207">Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="32ed5-207">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="32ed5-208">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="32ed5-208">Current directory and content root</span></span>

<span data-ttu-id="32ed5-209">Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-209">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="32ed5-210">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="32ed5-210">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="32ed5-211">Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="32ed5-211">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="32ed5-212">Использование ContentRootPath или ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="32ed5-212">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="32ed5-213">Используйте [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) или <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> для поиска ресурсов приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-213">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

<span data-ttu-id="32ed5-214">Когда приложение запускается как служба, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> задает для свойства <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> значение [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="32ed5-214">When the app runs as a service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> sets the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span>

<span data-ttu-id="32ed5-215">Файлы стандартных параметров приложения *appsettings.json* и *appsettings.{среда}.json* загружаются из корневого каталога содержимого приложения путем вызова [CreateDefaultBuilder во время создания узла](xref:fundamentals/host/generic-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="32ed5-215">The app's default settings files, *appsettings.json* and *appsettings.{Environment}.json*, are loaded from the app's content root by calling [CreateDefaultBuilder during host construction](xref:fundamentals/host/generic-host#set-up-a-host).</span></span>

<span data-ttu-id="32ed5-216">Для других файлов параметров, загруженных с помощью кода разработчика в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, не нужно вызывать <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-216">For other settings files loaded by developer code in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, there's no need to call <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="32ed5-217">В следующем примере файл *custom_settings. JSON* существует в корневом каталоге содержимого приложения и загружается без явного указания базового пути:</span><span class="sxs-lookup"><span data-stu-id="32ed5-217">In the following example, the *custom_settings.json* file exists in the app's content root and is loaded without explicitly setting a base path:</span></span>

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

<span data-ttu-id="32ed5-218">Не пытайтесь использовать <xref:System.IO.Directory.GetCurrentDirectory*>, чтобы получить путь к ресурсу, так как приложение службы Windows возвращает папку *C:\\WINDOWS\\system32* в качестве текущего каталога.</span><span class="sxs-lookup"><span data-stu-id="32ed5-218">Don't attempt to use <xref:System.IO.Directory.GetCurrentDirectory*> to obtain a resource path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder as its current directory.</span></span>

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="32ed5-219">Хранение файлов службы в подходящем расположении на диске</span><span class="sxs-lookup"><span data-stu-id="32ed5-219">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="32ed5-220">Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-220">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="32ed5-221">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="32ed5-221">Troubleshoot</span></span>

<span data-ttu-id="32ed5-222">Сведения об устранении неполадок в приложении службы Windows см. в статье <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-222">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="32ed5-223">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="32ed5-223">Common errors</span></span>

* <span data-ttu-id="32ed5-224">Используется старая или предварительная версия PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32ed5-224">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="32ed5-225">Зарегистрированная служба не использует **опубликованные** выходные данные приложения, возвращенные командой [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="32ed5-225">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="32ed5-226">Выходные данные команды [dotnet build](/dotnet/core/tools/dotnet-build) не поддерживаются для развертывания приложений.</span><span class="sxs-lookup"><span data-stu-id="32ed5-226">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="32ed5-227">В зависимости от типа развертывания опубликованные ресурсы находятся в одной из следующих папок:</span><span class="sxs-lookup"><span data-stu-id="32ed5-227">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="32ed5-228">*bin/Release/{TARGET FRAMEWORK}/publish* (зависящее от платформы развертывание);</span><span class="sxs-lookup"><span data-stu-id="32ed5-228">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="32ed5-229">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (автономное развертывание).</span><span class="sxs-lookup"><span data-stu-id="32ed5-229">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="32ed5-230">Служба не находится в состоянии выполнения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-230">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="32ed5-231">Пути к используемым приложением ресурсам (например, к сертификатам) неверные.</span><span class="sxs-lookup"><span data-stu-id="32ed5-231">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="32ed5-232">Базовый путь к службе Windows — *c:\\Windows\\System32*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-232">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="32ed5-233">У пользователя отсутствуют права для *входа в систему в качестве службы*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-233">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="32ed5-234">Пароль был указан неверно или его срок действия истек при выполнении команды PowerShell `New-Service`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-234">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="32ed5-235">Для приложения требуется выполнить проверку подлинности в ASP.NET Core, однако оно не настроено для безопасных подключений (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="32ed5-235">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="32ed5-236">Порт URL-адреса запроса неправильно указан или настроен в приложении.</span><span class="sxs-lookup"><span data-stu-id="32ed5-236">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="32ed5-237">Журналы событий системы и приложений</span><span class="sxs-lookup"><span data-stu-id="32ed5-237">System and Application Event Logs</span></span>

<span data-ttu-id="32ed5-238">Получите доступ к журналам событий системы и приложений:</span><span class="sxs-lookup"><span data-stu-id="32ed5-238">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="32ed5-239">Откройте меню "Пуск", выполните поиск по фразе *Просмотр событий* и выберите приложение **Просмотр событий**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-239">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="32ed5-240">В **средстве просмотра событий** откройте узел **Журналы Windows**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-240">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="32ed5-241">Выберите **Система**, чтобы открыть журнал системных событий.</span><span class="sxs-lookup"><span data-stu-id="32ed5-241">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="32ed5-242">Выберите **Приложение**, чтобы открыть журнал событий приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-242">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="32ed5-243">Проверьте здесь наличие ошибок, связанных с проблемным приложением.</span><span class="sxs-lookup"><span data-stu-id="32ed5-243">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="32ed5-244">Запуск приложения в командной строке</span><span class="sxs-lookup"><span data-stu-id="32ed5-244">Run the app at a command prompt</span></span>

<span data-ttu-id="32ed5-245">Многие ошибки запуска не создают полезные сведения в журналах событий.</span><span class="sxs-lookup"><span data-stu-id="32ed5-245">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="32ed5-246">Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-246">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="32ed5-247">Чтобы записать дополнительные сведения из приложения, снизьте [уровень ведения журнала](xref:fundamentals/logging/index#log-level) или запустите приложение в [среде разработки](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="32ed5-247">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="32ed5-248">Очистка кэшей пакетов</span><span class="sxs-lookup"><span data-stu-id="32ed5-248">Clear package caches</span></span>

<span data-ttu-id="32ed5-249">Приложения-функции могут перестать работать сразу после обновления пакета SDK для .NET Core на компьютере разработки или обновления версии пакетов в самом приложении.</span><span class="sxs-lookup"><span data-stu-id="32ed5-249">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="32ed5-250">В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-250">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="32ed5-251">Большинство этих проблем можно исправить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="32ed5-251">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="32ed5-252">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-252">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="32ed5-253">Очистите кэши пакетов, выполнив команду [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) из командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="32ed5-253">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="32ed5-254">Очистку кэшей пакетов также можно выполнить с помощью средства [nuget.exe](https://www.nuget.org/downloads) или команды `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-254">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="32ed5-255">*NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="32ed5-255">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="32ed5-256">Восстановите и перестройте проект.</span><span class="sxs-lookup"><span data-stu-id="32ed5-256">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="32ed5-257">Удалите все файлы из папки развертывания на сервере, прежде чем повторно развернуть приложение.</span><span class="sxs-lookup"><span data-stu-id="32ed5-257">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="32ed5-258">Медленное или зависающее приложение</span><span class="sxs-lookup"><span data-stu-id="32ed5-258">Slow or hanging app</span></span>

<span data-ttu-id="32ed5-259">*Аварийный дамп* — это моментальный снимок системной памяти, который может помочь определить причину аварийного завершения, сбоя запуска или медленной работы приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-259">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="32ed5-260">Аварийное завершение работы приложения или исключение</span><span class="sxs-lookup"><span data-stu-id="32ed5-260">App crashes or encounters an exception</span></span>

<span data-ttu-id="32ed5-261">Получите и проанализируйте дамп из [отчетов об ошибках Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="32ed5-261">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="32ed5-262">Создайте папку для хранения файлов аварийного дампа в `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-262">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="32ed5-263">Запустите [скрипт PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) с именем исполняемого файла приложения:</span><span class="sxs-lookup"><span data-stu-id="32ed5-263">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="32ed5-264">Запустите приложение в условиях, вызывающих аварийное завершение.</span><span class="sxs-lookup"><span data-stu-id="32ed5-264">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="32ed5-265">После аварийного завершения запустите [скрипт PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="32ed5-265">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="32ed5-266">Когда приложение аварийно завершит работу и сбор дампов будет выполнен, приложение сможет завершить работу обычным образом.</span><span class="sxs-lookup"><span data-stu-id="32ed5-266">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="32ed5-267">Скрипт PowerShell настраивает отчеты об ошибках Windows для сбора до пяти дампов для приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-267">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="32ed5-268">Аварийные дампы могут занимать много места на диске (до нескольких гигабайтов каждый).</span><span class="sxs-lookup"><span data-stu-id="32ed5-268">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="32ed5-269">Приложение перестает отвечать на запросы, не запускается или работает в обычном режиме</span><span class="sxs-lookup"><span data-stu-id="32ed5-269">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="32ed5-270">Когда приложение *перестает отвечать на запросы* (но аварийное завершение не происходит), не запускается или работает в обычном режиме, см. раздел [Файлы дампа пользовательского режима: выбор лучшего инструмента](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool), чтобы выбрать подходящий инструмент для создания дампа.</span><span class="sxs-lookup"><span data-stu-id="32ed5-270">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="32ed5-271">Анализ дампа</span><span class="sxs-lookup"><span data-stu-id="32ed5-271">Analyze the dump</span></span>

<span data-ttu-id="32ed5-272">Дамп можно проанализировать несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="32ed5-272">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="32ed5-273">Дополнительные сведения см. в разделе [Анализ файла дампа пользовательского режима](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="32ed5-273">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="32ed5-274">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="32ed5-274">Additional resources</span></span>

* <span data-ttu-id="32ed5-275">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="32ed5-275">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="32ed5-276">Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS.</span><span class="sxs-lookup"><span data-stu-id="32ed5-276">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="32ed5-277">При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки сервера.</span><span class="sxs-lookup"><span data-stu-id="32ed5-277">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="32ed5-278">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32ed5-278">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32ed5-279">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="32ed5-279">Prerequisites</span></span>

* [<span data-ttu-id="32ed5-280">Пакет SDK для ASP.NET Core 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32ed5-280">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="32ed5-281">PowerShell 6.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32ed5-281">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="32ed5-282">Конфигурация приложения</span><span class="sxs-lookup"><span data-stu-id="32ed5-282">App configuration</span></span>

<span data-ttu-id="32ed5-283">Приложение требует ссылки на пакеты для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) и [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="32ed5-283">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="32ed5-284">Для тестирования и отладки при работе вне службы добавьте код, чтобы определить, как выполняется приложение: в качестве службы или консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-284">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="32ed5-285">Проверьте, присоединен ли отладчик или присутствует ли параметр `--console`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-285">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="32ed5-286">Если одно из условий имеет значение true (приложение выполняется не в качестве службы), вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-286">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="32ed5-287">Если условия имеют значение false (приложение выполняется в качестве службы), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="32ed5-287">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="32ed5-288">Вызовите <xref:System.IO.Directory.SetCurrentDirectory*> и используйте путь к расположению для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-288">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="32ed5-289">Не вызывайте <xref:System.IO.Directory.GetCurrentDirectory*> для получения пути, так как при вызове <xref:System.IO.Directory.GetCurrentDirectory*> приложение службы Windows возвращает папку *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-289">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="32ed5-290">Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="32ed5-290">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="32ed5-291">Этот шаг выполняется до того, как приложение будет настроено в `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-291">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="32ed5-292">Вызовите <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, чтобы запустить приложение в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="32ed5-292">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="32ed5-293">Так как [поставщик конфигурации командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) требует указания пар "имя-значение" для аргументов командной строки, параметр `--console` удаляется из аргументов, прежде чем <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> получит их.</span><span class="sxs-lookup"><span data-stu-id="32ed5-293">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="32ed5-294">Для записи данных в журнал событий Windows добавьте поставщик EventLog в <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-294">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="32ed5-295">Задайте уровень ведения журнала с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-295">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="32ed5-296">В следующем примере `RunAsCustomService` вызывается вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> для обработки событий времени существования в приложении.</span><span class="sxs-lookup"><span data-stu-id="32ed5-296">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="32ed5-297">Дополнительные сведения см. в разделе [Обработка событий запуска и остановки](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="32ed5-297">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="deployment-type"></a><span data-ttu-id="32ed5-298">Тип развертывания</span><span class="sxs-lookup"><span data-stu-id="32ed5-298">Deployment type</span></span>

<span data-ttu-id="32ed5-299">Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="32ed5-299">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="32ed5-300">SDK</span><span class="sxs-lookup"><span data-stu-id="32ed5-300">SDK</span></span>

<span data-ttu-id="32ed5-301">Для службы на основе веб-приложений, которая использует платформы MVC или Razor Pages, укажите веб-пакет SDK в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="32ed5-301">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="32ed5-302">Если служба выполняет только фоновые задачи (например, [размещенные службы](xref:fundamentals/host/hosted-services)), укажите пакет SDK рабочей роли в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="32ed5-302">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="32ed5-303">Зависящее от платформы развертывание (FDD)</span><span class="sxs-lookup"><span data-stu-id="32ed5-303">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="32ed5-304">Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core.</span><span class="sxs-lookup"><span data-stu-id="32ed5-304">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="32ed5-305">Если сценарий с FDD используется согласно инструкций в этой статье, пакет SDK создаcт исполняемый файл ( *.exe*), который называется *исполняемым файлом, зависящим от платформы*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-305">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="32ed5-306">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) содержит целевую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="32ed5-306">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="32ed5-307">В следующем примере для RID задано значение `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-307">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="32ed5-308">Свойству `<SelfContained>` задано значение `false`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-308">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="32ed5-309">Эти свойства указывают пакету SDK создать исполняемый файл ( *.exe*) для Windows и приложение, которое зависит от общей платформы .NET Core.</span><span class="sxs-lookup"><span data-stu-id="32ed5-309">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="32ed5-310">Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="32ed5-310">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="32ed5-311">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-311">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="32ed5-312">Автономное развертывание</span><span class="sxs-lookup"><span data-stu-id="32ed5-312">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="32ed5-313">Для автономного развертывания необязательно наличие общей платформы в системе размещения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-313">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="32ed5-314">Среда выполнения и зависимости приложения развертываются с приложением.</span><span class="sxs-lookup"><span data-stu-id="32ed5-314">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="32ed5-315">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows включается в `<PropertyGroup>`, где содержится целевая платформа:</span><span class="sxs-lookup"><span data-stu-id="32ed5-315">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="32ed5-316">Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="32ed5-316">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="32ed5-317">Укажите список идентификаторов RID, разделив их точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="32ed5-317">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="32ed5-318">Используйте имя свойства [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (во множественном числе).</span><span class="sxs-lookup"><span data-stu-id="32ed5-318">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="32ed5-319">Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="32ed5-319">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="32ed5-320">Для свойства `<SelfContained>` задано значение `true`:</span><span class="sxs-lookup"><span data-stu-id="32ed5-320">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

## <a name="service-user-account"></a><span data-ttu-id="32ed5-321">Учетная запись пользователя службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-321">Service user account</span></span>

<span data-ttu-id="32ed5-322">Создайте для службы учетную запись пользователя с помощью командлета [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) в административной оболочке PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="32ed5-322">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="32ed5-323">Обновление Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763) или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="32ed5-323">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="32ed5-324">Версия Windows, предшествующая обновлению Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="32ed5-324">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="32ed5-325">Укажите [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="32ed5-325">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="32ed5-326">Параметр срока действия учетной записи <xref:System.DateTime> можно задать с помощью параметра `-AccountExpires`, передаваемого в командлет [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser).</span><span class="sxs-lookup"><span data-stu-id="32ed5-326">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="32ed5-327">Дополнительные сведения см. в статьях [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) и [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей служб).</span><span class="sxs-lookup"><span data-stu-id="32ed5-327">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="32ed5-328">Альтернативный подход к управлению пользователями при работе с Active Directory заключается в применении управляемых учетных записей служб.</span><span class="sxs-lookup"><span data-stu-id="32ed5-328">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="32ed5-329">Дополнительные сведения см. в [обзоре групповых управляемых учетных записей службы](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="32ed5-329">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="32ed5-330">Права на вход в качестве службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-330">Log on as a service rights</span></span>

<span data-ttu-id="32ed5-331">Чтобы настроить право *Вход в качестве службы* для учетной записи пользователя службы, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="32ed5-331">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="32ed5-332">Откройте редактор локальной политики безопасности, запустив *secpol.msc*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-332">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="32ed5-333">Разверните узел **Локальные политики** и выберите **Назначение прав пользователя**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-333">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="32ed5-334">Откройте политику **Вход в качестве службы**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-334">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="32ed5-335">Щелкните **Добавить пользователя или группу**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-335">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="32ed5-336">Укажите имя объекта (учетная запись пользователя) одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="32ed5-336">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="32ed5-337">Укажите учетную запись пользователя (`{DOMAIN OR COMPUTER NAME\USER}`) в поле для имени объекта и нажмите **ОК**, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="32ed5-337">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="32ed5-338">Выберите **Дополнительно**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-338">Select **Advanced**.</span></span> <span data-ttu-id="32ed5-339">Нажмите **Найти**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-339">Select **Find Now**.</span></span> <span data-ttu-id="32ed5-340">Выберите учетную запись пользователя из списка.</span><span class="sxs-lookup"><span data-stu-id="32ed5-340">Select the user account from the list.</span></span> <span data-ttu-id="32ed5-341">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-341">Select **OK**.</span></span> <span data-ttu-id="32ed5-342">Нажмите **ОК** еще раз, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="32ed5-342">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="32ed5-343">Нажмите **ОК** или **Применить**, чтобы сохранить изменения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-343">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="32ed5-344">Создание службы Windows и управление ею</span><span class="sxs-lookup"><span data-stu-id="32ed5-344">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="32ed5-345">Создание службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-345">Create a service</span></span>

<span data-ttu-id="32ed5-346">Зарегистрируйте службу с помощью команды PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32ed5-346">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="32ed5-347">В административной оболочке PowerShell 6 выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="32ed5-347">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="32ed5-348">`{EXE PATH}` &ndash; — путь к папке приложения на узле (например, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-348">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="32ed5-349">Не включайте исполняемый файл приложения в путь.</span><span class="sxs-lookup"><span data-stu-id="32ed5-349">Don't include the app's executable in the path.</span></span> <span data-ttu-id="32ed5-350">Завершающая косая черта не требуется.</span><span class="sxs-lookup"><span data-stu-id="32ed5-350">A trailing slash isn't required.</span></span>
* <span data-ttu-id="32ed5-351">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; —учетная запись пользователя службы (например, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-351">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="32ed5-352">`{SERVICE NAME}` &ndash; — имя службы (например, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-352">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="32ed5-353">`{EXE FILE PATH}` &ndash; — путь к исполняемому файлу приложения (например, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-353">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="32ed5-354">Включите имя исполняемого файла с расширением.</span><span class="sxs-lookup"><span data-stu-id="32ed5-354">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="32ed5-355">`{DESCRIPTION}` &ndash; — описание службы (например, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-355">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="32ed5-356">`{DISPLAY NAME}` &ndash; — отображаемое имя службы (например, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-356">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="32ed5-357">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-357">Start a service</span></span>

<span data-ttu-id="32ed5-358">Запустите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-358">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="32ed5-359">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="32ed5-359">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="32ed5-360">Определение состояния службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-360">Determine a service's status</span></span>

<span data-ttu-id="32ed5-361">Чтобы проверить состояние службы, используйте следующую команду PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-361">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="32ed5-362">Состояние отображается одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="32ed5-362">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="32ed5-363">Остановка службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-363">Stop a service</span></span>

<span data-ttu-id="32ed5-364">Остановите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-364">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="32ed5-365">Удаление службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-365">Remove a service</span></span>

<span data-ttu-id="32ed5-366">После небольшой задержки для остановки службы удалите службу с помощью следующей команды Powershell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-366">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="32ed5-367">Обработка событий запуска и остановки</span><span class="sxs-lookup"><span data-stu-id="32ed5-367">Handle starting and stopping events</span></span>

<span data-ttu-id="32ed5-368">Чтобы обработать события <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> и <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="32ed5-368">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="32ed5-369">Создайте класс, производный от <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService>, с методами `OnStarting`, `OnStarted` и `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="32ed5-369">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="32ed5-370">Создайте метод расширения для <xref:Microsoft.AspNetCore.Hosting.IWebHost>, который передает `CustomWebHostService` в <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="32ed5-370">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="32ed5-371">В `Program.Main` вызовите метод расширения `RunAsCustomService` вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="32ed5-371">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="32ed5-372">Чтобы узнать расположение <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> в `Program.Main`, см. пример кода из раздела [Тип развертывания](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="32ed5-372">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="32ed5-373">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="32ed5-373">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="32ed5-374">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="32ed5-374">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="32ed5-375">Для получения дополнительной информации см. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-375">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="32ed5-376">Настройка конечных точек</span><span class="sxs-lookup"><span data-stu-id="32ed5-376">Configure endpoints</span></span>

<span data-ttu-id="32ed5-377">По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-377">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="32ed5-378">Настройте URL-адрес и порт, задав переменную среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-378">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="32ed5-379">Дополнительные сведения о подходах к настройке URL-адресов и портов см. в соответствующей статье сервера:</span><span class="sxs-lookup"><span data-stu-id="32ed5-379">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="32ed5-380">В предыдущем руководстве рассматривается поддержка конечных точек HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32ed5-380">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="32ed5-381">Например, настройте приложение для HTTPS, если проверка подлинности используется со службой Windows.</span><span class="sxs-lookup"><span data-stu-id="32ed5-381">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="32ed5-382">Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="32ed5-382">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="32ed5-383">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="32ed5-383">Current directory and content root</span></span>

<span data-ttu-id="32ed5-384">Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-384">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="32ed5-385">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="32ed5-385">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="32ed5-386">Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="32ed5-386">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="32ed5-387">Указание папки приложения в качестве пути корневого каталога содержимого</span><span class="sxs-lookup"><span data-stu-id="32ed5-387">Set the content root path to the app's folder</span></span>

<span data-ttu-id="32ed5-388"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> — это тот же путь, который предоставляется аргументу `binPath` при создании службы.</span><span class="sxs-lookup"><span data-stu-id="32ed5-388">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="32ed5-389">Чтобы не вызывать метод `GetCurrentDirectory` для создания путей к файлам параметров, вызовите <xref:System.IO.Directory.SetCurrentDirectory*> с указанным путем к [корневому каталогу содержимого](xref:fundamentals/index#content-root) приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-389">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="32ed5-390">В `Program.Main` определите путь к папке с исполняемым файлом службы и используйте этот путь, чтобы создать корневой каталог содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-390">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="32ed5-391">Хранение файлов службы в подходящем расположении на диске</span><span class="sxs-lookup"><span data-stu-id="32ed5-391">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="32ed5-392">Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-392">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="32ed5-393">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="32ed5-393">Troubleshoot</span></span>

<span data-ttu-id="32ed5-394">Сведения об устранении неполадок в приложении службы Windows см. в статье <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-394">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="32ed5-395">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="32ed5-395">Common errors</span></span>

* <span data-ttu-id="32ed5-396">Используется старая или предварительная версия PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32ed5-396">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="32ed5-397">Зарегистрированная служба не использует **опубликованные** выходные данные приложения, возвращенные командой [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="32ed5-397">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="32ed5-398">Выходные данные команды [dotnet build](/dotnet/core/tools/dotnet-build) не поддерживаются для развертывания приложений.</span><span class="sxs-lookup"><span data-stu-id="32ed5-398">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="32ed5-399">В зависимости от типа развертывания опубликованные ресурсы находятся в одной из следующих папок:</span><span class="sxs-lookup"><span data-stu-id="32ed5-399">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="32ed5-400">*bin/Release/{TARGET FRAMEWORK}/publish* (зависящее от платформы развертывание);</span><span class="sxs-lookup"><span data-stu-id="32ed5-400">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="32ed5-401">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (автономное развертывание).</span><span class="sxs-lookup"><span data-stu-id="32ed5-401">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="32ed5-402">Служба не находится в состоянии выполнения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-402">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="32ed5-403">Пути к используемым приложением ресурсам (например, к сертификатам) неверные.</span><span class="sxs-lookup"><span data-stu-id="32ed5-403">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="32ed5-404">Базовый путь к службе Windows — *c:\\Windows\\System32*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-404">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="32ed5-405">У пользователя отсутствуют права для *входа в систему в качестве службы*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-405">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="32ed5-406">Пароль был указан неверно или его срок действия истек при выполнении команды PowerShell `New-Service`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-406">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="32ed5-407">Для приложения требуется выполнить проверку подлинности в ASP.NET Core, однако оно не настроено для безопасных подключений (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="32ed5-407">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="32ed5-408">Порт URL-адреса запроса неправильно указан или настроен в приложении.</span><span class="sxs-lookup"><span data-stu-id="32ed5-408">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="32ed5-409">Журналы событий системы и приложений</span><span class="sxs-lookup"><span data-stu-id="32ed5-409">System and Application Event Logs</span></span>

<span data-ttu-id="32ed5-410">Получите доступ к журналам событий системы и приложений:</span><span class="sxs-lookup"><span data-stu-id="32ed5-410">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="32ed5-411">Откройте меню "Пуск", выполните поиск по фразе *Просмотр событий* и выберите приложение **Просмотр событий**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-411">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="32ed5-412">В **средстве просмотра событий** откройте узел **Журналы Windows**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-412">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="32ed5-413">Выберите **Система**, чтобы открыть журнал системных событий.</span><span class="sxs-lookup"><span data-stu-id="32ed5-413">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="32ed5-414">Выберите **Приложение**, чтобы открыть журнал событий приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-414">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="32ed5-415">Проверьте здесь наличие ошибок, связанных с проблемным приложением.</span><span class="sxs-lookup"><span data-stu-id="32ed5-415">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="32ed5-416">Запуск приложения в командной строке</span><span class="sxs-lookup"><span data-stu-id="32ed5-416">Run the app at a command prompt</span></span>

<span data-ttu-id="32ed5-417">Многие ошибки запуска не создают полезные сведения в журналах событий.</span><span class="sxs-lookup"><span data-stu-id="32ed5-417">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="32ed5-418">Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-418">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="32ed5-419">Чтобы записать дополнительные сведения из приложения, снизьте [уровень ведения журнала](xref:fundamentals/logging/index#log-level) или запустите приложение в [среде разработки](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="32ed5-419">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="32ed5-420">Очистка кэшей пакетов</span><span class="sxs-lookup"><span data-stu-id="32ed5-420">Clear package caches</span></span>

<span data-ttu-id="32ed5-421">Приложения-функции могут перестать работать сразу после обновления пакета SDK для .NET Core на компьютере разработки или обновления версии пакетов в самом приложении.</span><span class="sxs-lookup"><span data-stu-id="32ed5-421">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="32ed5-422">В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-422">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="32ed5-423">Большинство этих проблем можно исправить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="32ed5-423">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="32ed5-424">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-424">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="32ed5-425">Очистите кэши пакетов, выполнив команду [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) из командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="32ed5-425">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="32ed5-426">Очистку кэшей пакетов также можно выполнить с помощью средства [nuget.exe](https://www.nuget.org/downloads) или команды `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-426">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="32ed5-427">*NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="32ed5-427">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="32ed5-428">Восстановите и перестройте проект.</span><span class="sxs-lookup"><span data-stu-id="32ed5-428">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="32ed5-429">Удалите все файлы из папки развертывания на сервере, прежде чем повторно развернуть приложение.</span><span class="sxs-lookup"><span data-stu-id="32ed5-429">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="32ed5-430">Медленное или зависающее приложение</span><span class="sxs-lookup"><span data-stu-id="32ed5-430">Slow or hanging app</span></span>

<span data-ttu-id="32ed5-431">*Аварийный дамп* — это моментальный снимок системной памяти, который может помочь определить причину аварийного завершения, сбоя запуска или медленной работы приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-431">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="32ed5-432">Аварийное завершение работы приложения или исключение</span><span class="sxs-lookup"><span data-stu-id="32ed5-432">App crashes or encounters an exception</span></span>

<span data-ttu-id="32ed5-433">Получите и проанализируйте дамп из [отчетов об ошибках Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="32ed5-433">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="32ed5-434">Создайте папку для хранения файлов аварийного дампа в `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-434">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="32ed5-435">Запустите [скрипт PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) с именем исполняемого файла приложения:</span><span class="sxs-lookup"><span data-stu-id="32ed5-435">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="32ed5-436">Запустите приложение в условиях, вызывающих аварийное завершение.</span><span class="sxs-lookup"><span data-stu-id="32ed5-436">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="32ed5-437">После аварийного завершения запустите [скрипт PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="32ed5-437">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="32ed5-438">Когда приложение аварийно завершит работу и сбор дампов будет выполнен, приложение сможет завершить работу обычным образом.</span><span class="sxs-lookup"><span data-stu-id="32ed5-438">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="32ed5-439">Скрипт PowerShell настраивает отчеты об ошибках Windows для сбора до пяти дампов для приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-439">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="32ed5-440">Аварийные дампы могут занимать много места на диске (до нескольких гигабайтов каждый).</span><span class="sxs-lookup"><span data-stu-id="32ed5-440">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="32ed5-441">Приложение перестает отвечать на запросы, не запускается или работает в обычном режиме</span><span class="sxs-lookup"><span data-stu-id="32ed5-441">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="32ed5-442">Когда приложение *перестает отвечать на запросы* (но аварийное завершение не происходит), не запускается или работает в обычном режиме, см. раздел [Файлы дампа пользовательского режима: выбор лучшего инструмента](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool), чтобы выбрать подходящий инструмент для создания дампа.</span><span class="sxs-lookup"><span data-stu-id="32ed5-442">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="32ed5-443">Анализ дампа</span><span class="sxs-lookup"><span data-stu-id="32ed5-443">Analyze the dump</span></span>

<span data-ttu-id="32ed5-444">Дамп можно проанализировать несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="32ed5-444">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="32ed5-445">Дополнительные сведения см. в разделе [Анализ файла дампа пользовательского режима](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="32ed5-445">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="32ed5-446">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="32ed5-446">Additional resources</span></span>

* <span data-ttu-id="32ed5-447">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="32ed5-447">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="32ed5-448">Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS.</span><span class="sxs-lookup"><span data-stu-id="32ed5-448">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="32ed5-449">При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки сервера.</span><span class="sxs-lookup"><span data-stu-id="32ed5-449">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="32ed5-450">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32ed5-450">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32ed5-451">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="32ed5-451">Prerequisites</span></span>

* [<span data-ttu-id="32ed5-452">Пакет SDK для ASP.NET Core 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32ed5-452">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="32ed5-453">PowerShell 6.2 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="32ed5-453">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="32ed5-454">Конфигурация приложения</span><span class="sxs-lookup"><span data-stu-id="32ed5-454">App configuration</span></span>

<span data-ttu-id="32ed5-455">Приложение требует ссылки на пакеты для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) и [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="32ed5-455">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="32ed5-456">Для тестирования и отладки при работе вне службы добавьте код, чтобы определить, как выполняется приложение: в качестве службы или консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-456">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="32ed5-457">Проверьте, присоединен ли отладчик или присутствует ли параметр `--console`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-457">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="32ed5-458">Если одно из условий имеет значение true (приложение выполняется не в качестве службы), вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-458">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="32ed5-459">Если условия имеют значение false (приложение выполняется в качестве службы), сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="32ed5-459">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="32ed5-460">Вызовите <xref:System.IO.Directory.SetCurrentDirectory*> и используйте путь к расположению для публикации приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-460">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="32ed5-461">Не вызывайте <xref:System.IO.Directory.GetCurrentDirectory*> для получения пути, так как при вызове <xref:System.IO.Directory.GetCurrentDirectory*> приложение службы Windows возвращает папку *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-461">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="32ed5-462">Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="32ed5-462">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="32ed5-463">Этот шаг выполняется до того, как приложение будет настроено в `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-463">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="32ed5-464">Вызовите <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, чтобы запустить приложение в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="32ed5-464">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="32ed5-465">Так как [поставщик конфигурации командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) требует указания пар "имя-значение" для аргументов командной строки, параметр `--console` удаляется из аргументов, прежде чем <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> получит их.</span><span class="sxs-lookup"><span data-stu-id="32ed5-465">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="32ed5-466">Для записи данных в журнал событий Windows добавьте поставщик EventLog в <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-466">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="32ed5-467">Задайте уровень ведения журнала с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-467">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="32ed5-468">В следующем примере `RunAsCustomService` вызывается вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> для обработки событий времени существования в приложении.</span><span class="sxs-lookup"><span data-stu-id="32ed5-468">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="32ed5-469">Дополнительные сведения см. в разделе [Обработка событий запуска и остановки](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="32ed5-469">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="deployment-type"></a><span data-ttu-id="32ed5-470">Тип развертывания</span><span class="sxs-lookup"><span data-stu-id="32ed5-470">Deployment type</span></span>

<span data-ttu-id="32ed5-471">Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="32ed5-471">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="32ed5-472">SDK</span><span class="sxs-lookup"><span data-stu-id="32ed5-472">SDK</span></span>

<span data-ttu-id="32ed5-473">Для службы на основе веб-приложений, которая использует платформы MVC или Razor Pages, укажите веб-пакет SDK в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="32ed5-473">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="32ed5-474">Если служба выполняет только фоновые задачи (например, [размещенные службы](xref:fundamentals/host/hosted-services)), укажите пакет SDK рабочей роли в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="32ed5-474">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="32ed5-475">Зависящее от платформы развертывание (FDD)</span><span class="sxs-lookup"><span data-stu-id="32ed5-475">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="32ed5-476">Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core.</span><span class="sxs-lookup"><span data-stu-id="32ed5-476">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="32ed5-477">Если сценарий с FDD используется согласно инструкций в этой статье, пакет SDK создаcт исполняемый файл ( *.exe*), который называется *исполняемым файлом, зависящим от платформы*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-477">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="32ed5-478">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) содержит целевую версию платформы.</span><span class="sxs-lookup"><span data-stu-id="32ed5-478">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="32ed5-479">В следующем примере для RID задано значение `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-479">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="32ed5-480">Свойству `<SelfContained>` задано значение `false`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-480">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="32ed5-481">Эти свойства указывают пакету SDK создать исполняемый файл ( *.exe*) для Windows и приложение, которое зависит от общей платформы .NET Core.</span><span class="sxs-lookup"><span data-stu-id="32ed5-481">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="32ed5-482">Свойству `<UseAppHost>` задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-482">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="32ed5-483">Это свойство предоставляет службу с путем активации (исполняемый файл, *EXE*) для FDD.</span><span class="sxs-lookup"><span data-stu-id="32ed5-483">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="32ed5-484">Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows.</span><span class="sxs-lookup"><span data-stu-id="32ed5-484">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="32ed5-485">Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-485">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="32ed5-486">Автономное развертывание</span><span class="sxs-lookup"><span data-stu-id="32ed5-486">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="32ed5-487">Для автономного развертывания необязательно наличие общей платформы в системе размещения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-487">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="32ed5-488">Среда выполнения и зависимости приложения развертываются с приложением.</span><span class="sxs-lookup"><span data-stu-id="32ed5-488">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="32ed5-489">[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows включается в `<PropertyGroup>`, где содержится целевая платформа:</span><span class="sxs-lookup"><span data-stu-id="32ed5-489">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="32ed5-490">Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="32ed5-490">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="32ed5-491">Укажите список идентификаторов RID, разделив их точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="32ed5-491">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="32ed5-492">Используйте имя свойства [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (во множественном числе).</span><span class="sxs-lookup"><span data-stu-id="32ed5-492">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="32ed5-493">Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="32ed5-493">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="32ed5-494">Для свойства `<SelfContained>` задано значение `true`:</span><span class="sxs-lookup"><span data-stu-id="32ed5-494">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

## <a name="service-user-account"></a><span data-ttu-id="32ed5-495">Учетная запись пользователя службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-495">Service user account</span></span>

<span data-ttu-id="32ed5-496">Создайте для службы учетную запись пользователя с помощью командлета [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) в административной оболочке PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="32ed5-496">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="32ed5-497">Обновление Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763) или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="32ed5-497">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="32ed5-498">Версия Windows, предшествующая обновлению Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="32ed5-498">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="32ed5-499">Укажите [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="32ed5-499">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="32ed5-500">Параметр срока действия учетной записи <xref:System.DateTime> можно задать с помощью параметра `-AccountExpires`, передаваемого в командлет [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser).</span><span class="sxs-lookup"><span data-stu-id="32ed5-500">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="32ed5-501">Дополнительные сведения см. в статьях [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) и [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей служб).</span><span class="sxs-lookup"><span data-stu-id="32ed5-501">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="32ed5-502">Альтернативный подход к управлению пользователями при работе с Active Directory заключается в применении управляемых учетных записей служб.</span><span class="sxs-lookup"><span data-stu-id="32ed5-502">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="32ed5-503">Дополнительные сведения см. в [обзоре групповых управляемых учетных записей службы](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="32ed5-503">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="32ed5-504">Права на вход в качестве службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-504">Log on as a service rights</span></span>

<span data-ttu-id="32ed5-505">Чтобы настроить право *Вход в качестве службы* для учетной записи пользователя службы, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="32ed5-505">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="32ed5-506">Откройте редактор локальной политики безопасности, запустив *secpol.msc*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-506">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="32ed5-507">Разверните узел **Локальные политики** и выберите **Назначение прав пользователя**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-507">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="32ed5-508">Откройте политику **Вход в качестве службы**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-508">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="32ed5-509">Щелкните **Добавить пользователя или группу**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-509">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="32ed5-510">Укажите имя объекта (учетная запись пользователя) одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="32ed5-510">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="32ed5-511">Укажите учетную запись пользователя (`{DOMAIN OR COMPUTER NAME\USER}`) в поле для имени объекта и нажмите **ОК**, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="32ed5-511">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="32ed5-512">Выберите **Дополнительно**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-512">Select **Advanced**.</span></span> <span data-ttu-id="32ed5-513">Нажмите **Найти**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-513">Select **Find Now**.</span></span> <span data-ttu-id="32ed5-514">Выберите учетную запись пользователя из списка.</span><span class="sxs-lookup"><span data-stu-id="32ed5-514">Select the user account from the list.</span></span> <span data-ttu-id="32ed5-515">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-515">Select **OK**.</span></span> <span data-ttu-id="32ed5-516">Нажмите **ОК** еще раз, чтобы назначить политику пользователю.</span><span class="sxs-lookup"><span data-stu-id="32ed5-516">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="32ed5-517">Нажмите **ОК** или **Применить**, чтобы сохранить изменения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-517">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="32ed5-518">Создание службы Windows и управление ею</span><span class="sxs-lookup"><span data-stu-id="32ed5-518">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="32ed5-519">Создание службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-519">Create a service</span></span>

<span data-ttu-id="32ed5-520">Зарегистрируйте службу с помощью команды PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32ed5-520">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="32ed5-521">В административной оболочке PowerShell 6 выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="32ed5-521">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="32ed5-522">`{EXE PATH}` &ndash; — путь к папке приложения на узле (например, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-522">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="32ed5-523">Не включайте исполняемый файл приложения в путь.</span><span class="sxs-lookup"><span data-stu-id="32ed5-523">Don't include the app's executable in the path.</span></span> <span data-ttu-id="32ed5-524">Завершающая косая черта не требуется.</span><span class="sxs-lookup"><span data-stu-id="32ed5-524">A trailing slash isn't required.</span></span>
* <span data-ttu-id="32ed5-525">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; —учетная запись пользователя службы (например, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-525">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="32ed5-526">`{SERVICE NAME}` &ndash; — имя службы (например, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-526">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="32ed5-527">`{EXE FILE PATH}` &ndash; — путь к исполняемому файлу приложения (например, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-527">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="32ed5-528">Включите имя исполняемого файла с расширением.</span><span class="sxs-lookup"><span data-stu-id="32ed5-528">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="32ed5-529">`{DESCRIPTION}` &ndash; — описание службы (например, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-529">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="32ed5-530">`{DISPLAY NAME}` &ndash; — отображаемое имя службы (например, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="32ed5-530">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="32ed5-531">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-531">Start a service</span></span>

<span data-ttu-id="32ed5-532">Запустите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-532">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="32ed5-533">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="32ed5-533">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="32ed5-534">Определение состояния службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-534">Determine a service's status</span></span>

<span data-ttu-id="32ed5-535">Чтобы проверить состояние службы, используйте следующую команду PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-535">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="32ed5-536">Состояние отображается одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="32ed5-536">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="32ed5-537">Остановка службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-537">Stop a service</span></span>

<span data-ttu-id="32ed5-538">Остановите службу с помощью следующей команды PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-538">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="32ed5-539">Удаление службы</span><span class="sxs-lookup"><span data-stu-id="32ed5-539">Remove a service</span></span>

<span data-ttu-id="32ed5-540">После небольшой задержки для остановки службы удалите службу с помощью следующей команды Powershell 6:</span><span class="sxs-lookup"><span data-stu-id="32ed5-540">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="32ed5-541">Обработка событий запуска и остановки</span><span class="sxs-lookup"><span data-stu-id="32ed5-541">Handle starting and stopping events</span></span>

<span data-ttu-id="32ed5-542">Чтобы обработать события <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> и <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="32ed5-542">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="32ed5-543">Создайте класс, производный от <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService>, с методами `OnStarting`, `OnStarted` и `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="32ed5-543">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="32ed5-544">Создайте метод расширения для <xref:Microsoft.AspNetCore.Hosting.IWebHost>, который передает `CustomWebHostService` в <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="32ed5-544">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="32ed5-545">В `Program.Main` вызовите метод расширения `RunAsCustomService` вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="32ed5-545">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="32ed5-546">Чтобы узнать расположение <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> в `Program.Main`, см. пример кода из раздела [Тип развертывания](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="32ed5-546">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="32ed5-547">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="32ed5-547">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="32ed5-548">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="32ed5-548">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="32ed5-549">Для получения дополнительной информации см. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-549">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="32ed5-550">Настройка конечных точек</span><span class="sxs-lookup"><span data-stu-id="32ed5-550">Configure endpoints</span></span>

<span data-ttu-id="32ed5-551">По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-551">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="32ed5-552">Настройте URL-адрес и порт, задав переменную среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-552">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="32ed5-553">Дополнительные сведения о подходах к настройке URL-адресов и портов см. в соответствующей статье сервера:</span><span class="sxs-lookup"><span data-stu-id="32ed5-553">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="32ed5-554">В предыдущем руководстве рассматривается поддержка конечных точек HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32ed5-554">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="32ed5-555">Например, настройте приложение для HTTPS, если проверка подлинности используется со службой Windows.</span><span class="sxs-lookup"><span data-stu-id="32ed5-555">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="32ed5-556">Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="32ed5-556">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="32ed5-557">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="32ed5-557">Current directory and content root</span></span>

<span data-ttu-id="32ed5-558">Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-558">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="32ed5-559">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="32ed5-559">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="32ed5-560">Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="32ed5-560">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="32ed5-561">Указание папки приложения в качестве пути корневого каталога содержимого</span><span class="sxs-lookup"><span data-stu-id="32ed5-561">Set the content root path to the app's folder</span></span>

<span data-ttu-id="32ed5-562"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> — это тот же путь, который предоставляется аргументу `binPath` при создании службы.</span><span class="sxs-lookup"><span data-stu-id="32ed5-562">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="32ed5-563">Чтобы не вызывать метод `GetCurrentDirectory` для создания путей к файлам параметров, вызовите <xref:System.IO.Directory.SetCurrentDirectory*> с указанным путем к [корневому каталогу содержимого](xref:fundamentals/index#content-root) приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-563">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="32ed5-564">В `Program.Main` определите путь к папке с исполняемым файлом службы и используйте этот путь, чтобы создать корневой каталог содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-564">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="32ed5-565">Хранение файлов службы в подходящем расположении на диске</span><span class="sxs-lookup"><span data-stu-id="32ed5-565">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="32ed5-566">Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-566">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="32ed5-567">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="32ed5-567">Troubleshoot</span></span>

<span data-ttu-id="32ed5-568">Сведения об устранении неполадок в приложении службы Windows см. в статье <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="32ed5-568">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="32ed5-569">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="32ed5-569">Common errors</span></span>

* <span data-ttu-id="32ed5-570">Используется старая или предварительная версия PowerShell.</span><span class="sxs-lookup"><span data-stu-id="32ed5-570">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="32ed5-571">Зарегистрированная служба не использует **опубликованные** выходные данные приложения, возвращенные командой [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="32ed5-571">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="32ed5-572">Выходные данные команды [dotnet build](/dotnet/core/tools/dotnet-build) не поддерживаются для развертывания приложений.</span><span class="sxs-lookup"><span data-stu-id="32ed5-572">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="32ed5-573">В зависимости от типа развертывания опубликованные ресурсы находятся в одной из следующих папок:</span><span class="sxs-lookup"><span data-stu-id="32ed5-573">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="32ed5-574">*bin/Release/{TARGET FRAMEWORK}/publish* (зависящее от платформы развертывание);</span><span class="sxs-lookup"><span data-stu-id="32ed5-574">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="32ed5-575">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (автономное развертывание).</span><span class="sxs-lookup"><span data-stu-id="32ed5-575">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="32ed5-576">Служба не находится в состоянии выполнения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-576">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="32ed5-577">Пути к используемым приложением ресурсам (например, к сертификатам) неверные.</span><span class="sxs-lookup"><span data-stu-id="32ed5-577">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="32ed5-578">Базовый путь к службе Windows — *c:\\Windows\\System32*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-578">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="32ed5-579">У пользователя отсутствуют права для *входа в систему в качестве службы*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-579">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="32ed5-580">Пароль был указан неверно или его срок действия истек при выполнении команды PowerShell `New-Service`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-580">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="32ed5-581">Для приложения требуется выполнить проверку подлинности в ASP.NET Core, однако оно не настроено для безопасных подключений (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="32ed5-581">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="32ed5-582">Порт URL-адреса запроса неправильно указан или настроен в приложении.</span><span class="sxs-lookup"><span data-stu-id="32ed5-582">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="32ed5-583">Журналы событий системы и приложений</span><span class="sxs-lookup"><span data-stu-id="32ed5-583">System and Application Event Logs</span></span>

<span data-ttu-id="32ed5-584">Получите доступ к журналам событий системы и приложений:</span><span class="sxs-lookup"><span data-stu-id="32ed5-584">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="32ed5-585">Откройте меню "Пуск", выполните поиск по фразе *Просмотр событий* и выберите приложение **Просмотр событий**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-585">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="32ed5-586">В **средстве просмотра событий** откройте узел **Журналы Windows**.</span><span class="sxs-lookup"><span data-stu-id="32ed5-586">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="32ed5-587">Выберите **Система**, чтобы открыть журнал системных событий.</span><span class="sxs-lookup"><span data-stu-id="32ed5-587">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="32ed5-588">Выберите **Приложение**, чтобы открыть журнал событий приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-588">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="32ed5-589">Проверьте здесь наличие ошибок, связанных с проблемным приложением.</span><span class="sxs-lookup"><span data-stu-id="32ed5-589">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="32ed5-590">Запуск приложения в командной строке</span><span class="sxs-lookup"><span data-stu-id="32ed5-590">Run the app at a command prompt</span></span>

<span data-ttu-id="32ed5-591">Многие ошибки запуска не создают полезные сведения в журналах событий.</span><span class="sxs-lookup"><span data-stu-id="32ed5-591">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="32ed5-592">Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-592">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="32ed5-593">Чтобы записать дополнительные сведения из приложения, снизьте [уровень ведения журнала](xref:fundamentals/logging/index#log-level) или запустите приложение в [среде разработки](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="32ed5-593">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="32ed5-594">Очистка кэшей пакетов</span><span class="sxs-lookup"><span data-stu-id="32ed5-594">Clear package caches</span></span>

<span data-ttu-id="32ed5-595">Приложения-функции могут перестать работать сразу после обновления пакета SDK для .NET Core на компьютере разработки или обновления версии пакетов в самом приложении.</span><span class="sxs-lookup"><span data-stu-id="32ed5-595">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="32ed5-596">В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-596">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="32ed5-597">Большинство этих проблем можно исправить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="32ed5-597">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="32ed5-598">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="32ed5-598">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="32ed5-599">Очистите кэши пакетов, выполнив команду [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) из командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="32ed5-599">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="32ed5-600">Очистку кэшей пакетов также можно выполнить с помощью средства [nuget.exe](https://www.nuget.org/downloads) или команды `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-600">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="32ed5-601">*NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="32ed5-601">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="32ed5-602">Восстановите и перестройте проект.</span><span class="sxs-lookup"><span data-stu-id="32ed5-602">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="32ed5-603">Удалите все файлы из папки развертывания на сервере, прежде чем повторно развернуть приложение.</span><span class="sxs-lookup"><span data-stu-id="32ed5-603">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="32ed5-604">Медленное или зависающее приложение</span><span class="sxs-lookup"><span data-stu-id="32ed5-604">Slow or hanging app</span></span>

<span data-ttu-id="32ed5-605">*Аварийный дамп* — это моментальный снимок системной памяти, который может помочь определить причину аварийного завершения, сбоя запуска или медленной работы приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-605">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="32ed5-606">Аварийное завершение работы приложения или исключение</span><span class="sxs-lookup"><span data-stu-id="32ed5-606">App crashes or encounters an exception</span></span>

<span data-ttu-id="32ed5-607">Получите и проанализируйте дамп из [отчетов об ошибках Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="32ed5-607">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="32ed5-608">Создайте папку для хранения файлов аварийного дампа в `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="32ed5-608">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="32ed5-609">Запустите [скрипт PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) с именем исполняемого файла приложения:</span><span class="sxs-lookup"><span data-stu-id="32ed5-609">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="32ed5-610">Запустите приложение в условиях, вызывающих аварийное завершение.</span><span class="sxs-lookup"><span data-stu-id="32ed5-610">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="32ed5-611">После аварийного завершения запустите [скрипт PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="32ed5-611">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="32ed5-612">Когда приложение аварийно завершит работу и сбор дампов будет выполнен, приложение сможет завершить работу обычным образом.</span><span class="sxs-lookup"><span data-stu-id="32ed5-612">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="32ed5-613">Скрипт PowerShell настраивает отчеты об ошибках Windows для сбора до пяти дампов для приложения.</span><span class="sxs-lookup"><span data-stu-id="32ed5-613">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="32ed5-614">Аварийные дампы могут занимать много места на диске (до нескольких гигабайтов каждый).</span><span class="sxs-lookup"><span data-stu-id="32ed5-614">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="32ed5-615">Приложение перестает отвечать на запросы, не запускается или работает в обычном режиме</span><span class="sxs-lookup"><span data-stu-id="32ed5-615">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="32ed5-616">Когда приложение *перестает отвечать на запросы* (но аварийное завершение не происходит), не запускается или работает в обычном режиме, см. раздел [Файлы дампа пользовательского режима: выбор лучшего инструмента](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool), чтобы выбрать подходящий инструмент для создания дампа.</span><span class="sxs-lookup"><span data-stu-id="32ed5-616">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="32ed5-617">Анализ дампа</span><span class="sxs-lookup"><span data-stu-id="32ed5-617">Analyze the dump</span></span>

<span data-ttu-id="32ed5-618">Дамп можно проанализировать несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="32ed5-618">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="32ed5-619">Дополнительные сведения см. в разделе [Анализ файла дампа пользовательского режима](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="32ed5-619">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="32ed5-620">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="32ed5-620">Additional resources</span></span>

* <span data-ttu-id="32ed5-621">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="32ed5-621">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
