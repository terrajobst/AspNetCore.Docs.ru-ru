---
title: Настройка проверки подлинности Windows в ASP.NET Core
author: scottaddie
description: Узнайте, как настроить проверку подлинности Windows в ASP.NET Core для IIS и HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/05/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 900bbf5f14b1876ad537b2b77e4ba07d7aa168f2
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750163"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="04bef-103">Настройка проверки подлинности Windows в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04bef-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="04bef-104">По [Scott Addie](https://twitter.com/Scott_Addie) и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="04bef-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="04bef-105">[Проверка подлинности Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) можно настроить для приложений ASP.NET Core, размещенных с [IIS](xref:host-and-deploy/iis/index) или [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="04bef-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="04bef-106">Проверка подлинности Windows зависит от операционной системы для проверки подлинности пользователей, приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="04bef-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="04bef-107">Можно использовать проверку подлинности Windows, когда сервер работает в корпоративной сети с помощью удостоверения домена Active Directory или учетных записей Windows для идентификации пользователей.</span><span class="sxs-lookup"><span data-stu-id="04bef-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="04bef-108">Проверка подлинности Windows является наилучшим образом подходит для среды интрасети, где пользователи, клиентские приложения и веб-серверы принадлежат к тому же домену Windows.</span><span class="sxs-lookup"><span data-stu-id="04bef-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="04bef-109">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="04bef-109">IIS/IIS Express</span></span>

<span data-ttu-id="04bef-110">Добавить службы проверки подлинности путем вызова <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> пространства имен) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="04bef-110">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="04bef-111">Параметры (отладчик) запуска</span><span class="sxs-lookup"><span data-stu-id="04bef-111">Launch settings (debugger)</span></span>

<span data-ttu-id="04bef-112">Конфигурация параметров запуска влияет только на *Properties/launchSettings.json* файл для IIS Express и не настраивает IIS для Windows проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="04bef-112">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="04bef-113">Конфигурация сервера описан в [IIS](#iis) раздел.</span><span class="sxs-lookup"><span data-stu-id="04bef-113">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="04bef-114">**Веб-приложение** шаблон, доступный через Visual Studio или .NET Core CLI можно настроить для поддержки проверки подлинности Windows, которая обновляет *Properties/launchSettings.json* файла автоматически.</span><span class="sxs-lookup"><span data-stu-id="04bef-114">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="04bef-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04bef-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="04bef-116">**новый проект**</span><span class="sxs-lookup"><span data-stu-id="04bef-116">**New project**</span></span>

1. <span data-ttu-id="04bef-117">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="04bef-117">Create a new project.</span></span>
1. <span data-ttu-id="04bef-118">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="04bef-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="04bef-119">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="04bef-119">Select **Next**.</span></span>
1. <span data-ttu-id="04bef-120">Введите имя в **имя_проекта** поля.</span><span class="sxs-lookup"><span data-stu-id="04bef-120">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="04bef-121">Подтвердите **расположение** запись правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="04bef-121">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="04bef-122">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="04bef-122">Select **Create**.</span></span>
1. <span data-ttu-id="04bef-123">Выберите **изменение** под **проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="04bef-123">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="04bef-124">В **изменить способ проверки подлинности** выберите **проверки подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="04bef-124">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="04bef-125">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="04bef-125">Select **OK**.</span></span>
1. <span data-ttu-id="04bef-126">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="04bef-126">Select **Web Application**.</span></span>
1. <span data-ttu-id="04bef-127">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="04bef-127">Select **Create**.</span></span>

<span data-ttu-id="04bef-128">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="04bef-128">Run the app.</span></span> <span data-ttu-id="04bef-129">Имя пользователя отображается в готовом для просмотра приложения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="04bef-129">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="04bef-130">**Существующий проект**</span><span class="sxs-lookup"><span data-stu-id="04bef-130">**Existing project**</span></span>

<span data-ttu-id="04bef-131">Свойства проекта, включите проверку подлинности Windows и отключить анонимную проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="04bef-131">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="04bef-132">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="04bef-132">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="04bef-133">Выберите вкладку **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="04bef-133">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="04bef-134">Снимите флажок для **Включение анонимной проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="04bef-134">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="04bef-135">Установите флажок для **включить проверку подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="04bef-135">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="04bef-136">Сохранить и закрыть страницу его свойств.</span><span class="sxs-lookup"><span data-stu-id="04bef-136">Save and close the property page.</span></span>

<span data-ttu-id="04bef-137">Кроме того, можно настроить свойства в `iisSettings` узел *launchSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="04bef-137">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="04bef-138">Visual Studio Code или .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="04bef-138">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="04bef-139">**новый проект**</span><span class="sxs-lookup"><span data-stu-id="04bef-139">**New project**</span></span>

<span data-ttu-id="04bef-140">Выполнение [команды dotnet new](/dotnet/core/tools/dotnet-new) с `webapp` аргумент (веб-приложения ASP.NET Core) и `--auth Windows` переключения:</span><span class="sxs-lookup"><span data-stu-id="04bef-140">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="04bef-141">**Существующий проект**</span><span class="sxs-lookup"><span data-stu-id="04bef-141">**Existing project**</span></span>

<span data-ttu-id="04bef-142">Обновление `iisSettings` узел *launchSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="04bef-142">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="04bef-143">При изменении существующего проекта, убедитесь, что файл проекта содержит ссылку на пакет для [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **или** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="04bef-143">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="04bef-144">IIS</span><span class="sxs-lookup"><span data-stu-id="04bef-144">IIS</span></span>

<span data-ttu-id="04bef-145">Службы IIS используют [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="04bef-145">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="04bef-146">Проверка подлинности Windows настроена для IIS с помощью *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="04bef-146">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="04bef-147">В следующих разделах показано как:</span><span class="sxs-lookup"><span data-stu-id="04bef-147">The following sections show how to:</span></span>

* <span data-ttu-id="04bef-148">Укажите локальный *web.config* файл, который активирует проверку подлинности Windows на сервере, при развертывании приложения.</span><span class="sxs-lookup"><span data-stu-id="04bef-148">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="04bef-149">Использование диспетчера служб IIS для настройки *web.config* файл приложения ASP.NET Core, которое уже развернуто на сервер.</span><span class="sxs-lookup"><span data-stu-id="04bef-149">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="04bef-150">Если вы еще не сделали, включении служб IIS для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="04bef-150">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="04bef-151">Дополнительные сведения см. в разделе <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="04bef-151">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="04bef-152">Включение службы роли IIS для проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="04bef-152">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="04bef-153">Дополнительные сведения см. в разделе [включить проверку подлинности Windows в службы роли IIS (см. шаг 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="04bef-153">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="04bef-154">[По промежуточного слоя для интеграции IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) настраивается для автоматической проверки подлинности запросов по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="04bef-154">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="04bef-155">Дополнительные сведения см. в разделе [размещение ASP.NET Core в Windows со службами IIS: Параметры служб IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="04bef-155">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="04bef-156">Модуль ASP.NET Core настроен на пересылку токена проверки подлинности Windows в приложение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="04bef-156">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="04bef-157">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core: Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="04bef-157">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="04bef-158">Используйте **либо** из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="04bef-158">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="04bef-159">**Перед публикацией и развертывание проекта,** добавьте следующий *web.config* файл в корневую папку проекта:</span><span class="sxs-lookup"><span data-stu-id="04bef-159">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="04bef-160">При публикации проекта с помощью пакета SDK .NET Core (без `<IsTransformWebConfigDisabled>` свойство значение `true` в файле проекта), опубликованной *web.config* файл включает `<location><system.webServer><security><authentication>` раздел.</span><span class="sxs-lookup"><span data-stu-id="04bef-160">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="04bef-161">Дополнительные сведения о `<IsTransformWebConfigDisabled>` свойство, см. в разделе <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="04bef-161">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="04bef-162">**После публикации и развертывании проекта,** выполнения конфигурации на стороне сервера с помощью диспетчера IIS:</span><span class="sxs-lookup"><span data-stu-id="04bef-162">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="04bef-163">В диспетчере IIS выберите сайт IIS, в разделе **сайты** узел **подключений** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="04bef-163">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="04bef-164">Дважды щелкните **проверки подлинности** в **IIS** области.</span><span class="sxs-lookup"><span data-stu-id="04bef-164">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="04bef-165">Выберите **анонимную проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="04bef-165">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="04bef-166">Выберите **отключить** в **действия** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="04bef-166">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="04bef-167">Выберите **проверки подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="04bef-167">Select **Windows Authentication**.</span></span> <span data-ttu-id="04bef-168">Выберите **включить** в **действия** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="04bef-168">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="04bef-169">При этих действий, IIS Manager вносит изменения в приложения *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="04bef-169">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="04bef-170">Объект `<system.webServer><security><authentication>` добавляется узел с обновленными параметрами для `anonymousAuthentication` и `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="04bef-170">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="04bef-171">`<system.webServer>` Добавлен в раздел *web.config* файл диспетчером IIS находится за пределами приложения `<location>` добавлен с помощью пакета SDK .NET Core, когда приложение публикуется раздел.</span><span class="sxs-lookup"><span data-stu-id="04bef-171">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="04bef-172">Поскольку раздел добавляется вне `<location>` узел, параметры, наследуются [дочерние приложения](xref:host-and-deploy/iis/index#sub-applications) с текущим приложением.</span><span class="sxs-lookup"><span data-stu-id="04bef-172">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="04bef-173">Чтобы запретить наследование, переместите добавленного `<security>` разделе внутри `<location><system.webServer>` раздел, в котором указан пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="04bef-173">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="04bef-174">Когда диспетчер IIS используется для добавления конфигурации IIS, оно влияет только на приложения *web.config* файл на сервере.</span><span class="sxs-lookup"><span data-stu-id="04bef-174">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="04bef-175">Последующие развертывания приложения может перезаписать параметры на сервере, если копия сервера *web.config* заменяется проекта *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="04bef-175">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="04bef-176">Используйте **либо** из следующих методов для управления параметрами:</span><span class="sxs-lookup"><span data-stu-id="04bef-176">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="04bef-177">Используйте диспетчер служб IIS, чтобы присвоить параметрам в *web.config* файл после файл перезаписывается при развертывании.</span><span class="sxs-lookup"><span data-stu-id="04bef-177">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="04bef-178">Добавить *файл web.config* приложение локально с параметрами.</span><span class="sxs-lookup"><span data-stu-id="04bef-178">Add a *web.config file* to the app locally with the settings.</span></span>

## <a name="httpsys"></a><span data-ttu-id="04bef-179">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="04bef-179">HTTP.sys</span></span>

<span data-ttu-id="04bef-180">В резидентных сценариях [Kestrel](xref:fundamentals/servers/kestrel) не поддержки проверки подлинности Windows, но вы можете использовать [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="04bef-180">In self-hosted scenarios, [Kestrel](xref:fundamentals/servers/kestrel) doesn't support Windows Authentication, but you can use [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="04bef-181">Добавить службы проверки подлинности путем вызова <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> пространства имен) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="04bef-181">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="04bef-182">Настроить узел веб-приложения для использования HTTP.sys с проверкой подлинности Windows (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="04bef-182">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="04bef-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> находится в пространстве имен <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="04bef-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="04bef-184">HTTP.sys делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos.</span><span class="sxs-lookup"><span data-stu-id="04bef-184">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="04bef-185">Проверка подлинности в режиме пользователя не поддерживается с Kerberos и HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="04bef-185">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="04bef-186">Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="04bef-186">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="04bef-187">Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.</span><span class="sxs-lookup"><span data-stu-id="04bef-187">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="04bef-188">HTTP.sys не поддерживается на сервере Nano Server версии 1709 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="04bef-188">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="04bef-189">Чтобы использовать проверку подлинности Windows и HTTP.sys с сервером Nano Server, используйте [контейнера Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="04bef-189">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="04bef-190">Дополнительные сведения о Server Core см. в разделе [Какова вариант установки Server Core в Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="04bef-190">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="04bef-191">Авторизация пользователей</span><span class="sxs-lookup"><span data-stu-id="04bef-191">Authorize users</span></span>

<span data-ttu-id="04bef-192">Состояние конфигурации анонимный доступ определяет способ, которым `[Authorize]` и `[AllowAnonymous]` атрибуты используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="04bef-192">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="04bef-193">Следующих двух разделах объясняется, как обрабатывать запрещенные и разрешенные конфигурации состояния анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="04bef-193">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="04bef-194">Запретить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="04bef-194">Disallow anonymous access</span></span>

<span data-ttu-id="04bef-195">Если включена проверка подлинности Windows и анонимный доступ отключен, `[Authorize]` и `[AllowAnonymous]` атрибуты никак не влияют.</span><span class="sxs-lookup"><span data-stu-id="04bef-195">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="04bef-196">Если узел IIS настроен на запрет анонимного доступа, никогда не достигнут приложения.</span><span class="sxs-lookup"><span data-stu-id="04bef-196">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="04bef-197">По этой причине `[AllowAnonymous]` атрибут не применяется.</span><span class="sxs-lookup"><span data-stu-id="04bef-197">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="04bef-198">Разрешить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="04bef-198">Allow anonymous access</span></span>

<span data-ttu-id="04bef-199">Если включены как проверка подлинности Windows, так и анонимный доступ, используйте `[Authorize]` и `[AllowAnonymous]` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="04bef-199">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="04bef-200">`[Authorize]` Атрибут позволяет защищать конечные точки приложения, которые требуют проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="04bef-200">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="04bef-201">`[AllowAnonymous]` Атрибут переопределения `[Authorize]` атрибут в приложениях с возможностью анонимного доступа.</span><span class="sxs-lookup"><span data-stu-id="04bef-201">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="04bef-202">Дополнительные сведения об использовании атрибута см. в разделе <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="04bef-202">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="04bef-203">По умолчанию пользователи, у которых отсутствует разрешение на доступ к странице, откроется пустой ответ HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="04bef-203">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="04bef-204">[StatusCodePages по промежуточного слоя](xref:fundamentals/error-handling#usestatuscodepages) можно настроить для предоставления пользователям более комфортные «Отказано в доступе».</span><span class="sxs-lookup"><span data-stu-id="04bef-204">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="04bef-205">Олицетворение</span><span class="sxs-lookup"><span data-stu-id="04bef-205">Impersonation</span></span>

<span data-ttu-id="04bef-206">ASP.NET Core не реализует олицетворения.</span><span class="sxs-lookup"><span data-stu-id="04bef-206">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="04bef-207">Приложения запускаются с удостоверение приложения для всех запросов, с помощью удостоверения пула или процесс приложения.</span><span class="sxs-lookup"><span data-stu-id="04bef-207">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="04bef-208">Если приложение должно выполнить действие от имени пользователя, используйте [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) в [терминалов встроенного ПО промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="04bef-208">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="04bef-209">Запустить одно действие в этом контексте, а затем закройте контекста.</span><span class="sxs-lookup"><span data-stu-id="04bef-209">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="04bef-210">`RunImpersonated` не поддерживает асинхронные операции и не должны использоваться для сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="04bef-210">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="04bef-211">Например упаковки всего запросов или по промежуточного слоя цепочек не поддерживается и не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="04bef-211">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

## <a name="claims-transformations"></a><span data-ttu-id="04bef-212">Преобразования утверждений</span><span class="sxs-lookup"><span data-stu-id="04bef-212">Claims transformations</span></span>

<span data-ttu-id="04bef-213">При размещении в режиме в процессе IIS <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя.</span><span class="sxs-lookup"><span data-stu-id="04bef-213">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="04bef-214">Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="04bef-214">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="04bef-215">Дополнительные сведения и пример кода, который активирует преобразования утверждений, при размещении в процессе, см. в разделе <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="04bef-215">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04bef-216">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="04bef-216">Additional resources</span></span>

* [<span data-ttu-id="04bef-217">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="04bef-217">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
