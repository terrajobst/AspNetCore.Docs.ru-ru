---
title: Настройка проверки подлинности Windows в ASP.NET Core
author: scottaddie
description: Узнайте, как настроить проверку подлинности Windows в ASP.NET Core, используя IIS Express, службы IIS и HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/03/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: bd4ffa79c4d1e0070c820fa9c06b0a84c3aaae74
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468657"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="9275f-103">Настройка проверки подлинности Windows в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9275f-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="9275f-104">По [Scott Addie](https://twitter.com/Scott_Addie) и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9275f-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9275f-105">[Проверка подлинности Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) можно настроить для приложений ASP.NET Core, размещенных с [IIS](xref:host-and-deploy/iis/index) или [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="9275f-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="9275f-106">Проверка подлинности Windows зависит от операционной системы для проверки подлинности пользователей, приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9275f-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="9275f-107">Можно использовать проверку подлинности Windows, когда сервер работает в корпоративной сети с помощью удостоверения домена Active Directory или учетных записей Windows для идентификации пользователей.</span><span class="sxs-lookup"><span data-stu-id="9275f-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="9275f-108">Проверка подлинности Windows является наилучшим образом подходит для среды интрасети, где пользователи, клиентские приложения и веб-серверы принадлежат к тому же домену Windows.</span><span class="sxs-lookup"><span data-stu-id="9275f-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="9275f-109">Включить проверку подлинности Windows в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9275f-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="9275f-110">**Веб-приложение** шаблон, доступный через Visual Studio или .NET Core CLI можно настроить для поддержки проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="9275f-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="9275f-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9275f-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="9275f-112">Шаблон приложения проверки подлинности Windows можно использовать для нового проекта</span><span class="sxs-lookup"><span data-stu-id="9275f-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="9275f-113">В Visual Studio сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="9275f-113">In Visual Studio:</span></span>

1. <span data-ttu-id="9275f-114">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="9275f-114">Create a new project.</span></span>
1. <span data-ttu-id="9275f-115">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9275f-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="9275f-116">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9275f-116">Select **Next**.</span></span>
1. <span data-ttu-id="9275f-117">Введите имя в **имя_проекта** поля.</span><span class="sxs-lookup"><span data-stu-id="9275f-117">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="9275f-118">Подтвердите **расположение** запись правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="9275f-118">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="9275f-119">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9275f-119">Select **Create**.</span></span>
1. <span data-ttu-id="9275f-120">Выберите **изменение** под **проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="9275f-120">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="9275f-121">В **изменить способ проверки подлинности** выберите **проверки подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="9275f-121">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="9275f-122">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9275f-122">Select **OK**.</span></span>
1. <span data-ttu-id="9275f-123">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="9275f-123">Select **Web Application**.</span></span>
1. <span data-ttu-id="9275f-124">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9275f-124">Select **Create**.</span></span>

<span data-ttu-id="9275f-125">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="9275f-125">Run the app.</span></span> <span data-ttu-id="9275f-126">Имя пользователя отображается в готовом для просмотра приложения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9275f-126">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="9275f-127">Ручная настройка для существующего проекта</span><span class="sxs-lookup"><span data-stu-id="9275f-127">Manual configuration for an existing project</span></span>

<span data-ttu-id="9275f-128">Свойства проекта позволяют включить проверку подлинности Windows и отключить анонимную проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="9275f-128">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="9275f-129">Щелкните правой кнопкой мыши проект в Visual Studio **обозревателе решений** и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="9275f-129">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="9275f-130">Выберите вкладку **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="9275f-130">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="9275f-131">Снимите флажок для **Включение анонимной проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="9275f-131">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="9275f-132">Установите флажок для **включить проверку подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="9275f-132">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="9275f-133">Кроме того, можно настроить свойства в `iisSettings` узел *launchSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="9275f-133">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# [<a name="net-core-cli"></a><span data-ttu-id="9275f-134">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="9275f-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9275f-135">Используйте **проверки подлинности Windows** шаблон приложения.</span><span class="sxs-lookup"><span data-stu-id="9275f-135">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="9275f-136">Выполнение [команды dotnet new](/dotnet/core/tools/dotnet-new) с `webapp` аргумент (веб-приложения ASP.NET Core) и `--auth Windows` переключения:</span><span class="sxs-lookup"><span data-stu-id="9275f-136">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

<span data-ttu-id="9275f-137">При изменении существующего проекта, убедитесь, что файл проекта содержит ссылку на пакет для [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **или** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="9275f-137">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="9275f-138">Включить проверку подлинности Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="9275f-138">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="9275f-139">Службы IIS используют [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9275f-139">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="9275f-140">Проверка подлинности Windows настроена для IIS с помощью *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="9275f-140">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="9275f-141">В следующих разделах показано как:</span><span class="sxs-lookup"><span data-stu-id="9275f-141">The following sections show how to:</span></span>

* <span data-ttu-id="9275f-142">Укажите локальный *web.config* файл, который активирует проверку подлинности Windows на сервере, при развертывании приложения.</span><span class="sxs-lookup"><span data-stu-id="9275f-142">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="9275f-143">Использование диспетчера служб IIS для настройки *web.config* файл приложения ASP.NET Core, которое уже развернуто на сервер.</span><span class="sxs-lookup"><span data-stu-id="9275f-143">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="9275f-144">Конфигурация IIS</span><span class="sxs-lookup"><span data-stu-id="9275f-144">IIS configuration</span></span>

<span data-ttu-id="9275f-145">Если вы еще не сделали, включении служб IIS для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9275f-145">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="9275f-146">Дополнительные сведения см. в разделе <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="9275f-146">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="9275f-147">Включение службы роли IIS для проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="9275f-147">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="9275f-148">Дополнительные сведения см. в разделе [включить проверку подлинности Windows в службы роли IIS (см. шаг 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="9275f-148">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="9275f-149">По умолчанию по промежуточного слоя для интеграции IIS настроен для автоматической проверки подлинности запросов.</span><span class="sxs-lookup"><span data-stu-id="9275f-149">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="9275f-150">Дополнительные сведения см. в разделе [размещение ASP.NET Core в Windows со службами IIS: Параметры служб IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="9275f-150">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="9275f-151">Модуль ASP.NET Core настроен на пересылку токена проверки подлинности Windows в приложение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9275f-151">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="9275f-152">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core: Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="9275f-152">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="9275f-153">Создание нового сайта IIS</span><span class="sxs-lookup"><span data-stu-id="9275f-153">Create a new IIS site</span></span>

<span data-ttu-id="9275f-154">Укажите имя и папку и разрешите ему создать новый пул приложений.</span><span class="sxs-lookup"><span data-stu-id="9275f-154">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="9275f-155">Включить проверку подлинности Windows для приложения в IIS</span><span class="sxs-lookup"><span data-stu-id="9275f-155">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="9275f-156">Используйте **либо** из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="9275f-156">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="9275f-157">[Настройка на стороне разработки перед публикацией приложения](#development-side-configuration-with-a-local-webconfig-file) (*рекомендуется*)</span><span class="sxs-lookup"><span data-stu-id="9275f-157">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="9275f-158">Настройка на стороне сервера после публикации приложения</span><span class="sxs-lookup"><span data-stu-id="9275f-158">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="9275f-159">Настройка на стороне разработки с помощью файла web.config локального</span><span class="sxs-lookup"><span data-stu-id="9275f-159">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="9275f-160">Выполните следующие действия **перед** вы [публикация и развертывание проекта](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="9275f-160">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="9275f-161">Добавьте следующий *web.config* файл в корневую папку проекта:</span><span class="sxs-lookup"><span data-stu-id="9275f-161">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="9275f-162">При публикации проекта с помощью пакета SDK (без `<IsTransformWebConfigDisabled>` свойство значение `true` в файле проекта), опубликованной *web.config* файл включает `<location><system.webServer><security><authentication>` раздел.</span><span class="sxs-lookup"><span data-stu-id="9275f-162">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="9275f-163">Дополнительные сведения о `<IsTransformWebConfigDisabled>` свойство, см. в разделе <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="9275f-163">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="9275f-164">Настройка на стороне сервера с помощью диспетчера IIS</span><span class="sxs-lookup"><span data-stu-id="9275f-164">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="9275f-165">Выполните следующие действия **после** вы [публикация и развертывание проекта](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="9275f-165">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="9275f-166">В диспетчере IIS выберите сайт IIS, в разделе **сайты** узел **подключений** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="9275f-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="9275f-167">Дважды щелкните **проверки подлинности** в **IIS** области.</span><span class="sxs-lookup"><span data-stu-id="9275f-167">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="9275f-168">Выберите **анонимную проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="9275f-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="9275f-169">Выберите **отключить** в **действия** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="9275f-169">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="9275f-170">Выберите **проверки подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="9275f-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="9275f-171">Выберите **включить** в **действия** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="9275f-171">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="9275f-172">При этих действий, IIS Manager вносит изменения в приложения *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="9275f-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="9275f-173">Объект `<system.webServer><security><authentication>` добавляется узел с обновленными параметрами для `anonymousAuthentication` и `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="9275f-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="9275f-174">`<system.webServer>` Добавлен в раздел *web.config* файл диспетчером IIS находится за пределами приложения `<location>` добавлен с помощью пакета SDK .NET Core, когда приложение публикуется раздел.</span><span class="sxs-lookup"><span data-stu-id="9275f-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="9275f-175">Поскольку раздел добавляется вне `<location>` узел, параметры, наследуются [дочерние приложения](xref:host-and-deploy/iis/index#sub-applications) с текущим приложением.</span><span class="sxs-lookup"><span data-stu-id="9275f-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="9275f-176">Чтобы запретить наследование, переместите добавленного `<security>` разделе внутри `<location><system.webServer>` раздел, в котором пакет SDK.</span><span class="sxs-lookup"><span data-stu-id="9275f-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="9275f-177">Когда диспетчер IIS используется для добавления конфигурации IIS, оно влияет только на приложения *web.config* файл на сервере.</span><span class="sxs-lookup"><span data-stu-id="9275f-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="9275f-178">Последующие развертывания приложения может перезаписать параметры на сервере, если копия сервера *web.config* заменяется проекта *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="9275f-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="9275f-179">Используйте **либо** из следующих методов для управления параметрами:</span><span class="sxs-lookup"><span data-stu-id="9275f-179">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="9275f-180">Используйте диспетчер служб IIS, чтобы присвоить параметрам в *web.config* файл после файл перезаписывается при развертывании.</span><span class="sxs-lookup"><span data-stu-id="9275f-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="9275f-181">Добавить *файл web.config* приложение локально с параметрами.</span><span class="sxs-lookup"><span data-stu-id="9275f-181">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="9275f-182">Дополнительные сведения см. в разделе [Настройка на стороне разработки](#development-side-configuration-with-a-local-webconfig-file) раздел.</span><span class="sxs-lookup"><span data-stu-id="9275f-182">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="9275f-183">Публикация и развертывание проекта в папку сайта IIS</span><span class="sxs-lookup"><span data-stu-id="9275f-183">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="9275f-184">С помощью Visual Studio или .NET Core CLI, публикация и развертывание приложения в папке назначения.</span><span class="sxs-lookup"><span data-stu-id="9275f-184">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="9275f-185">Дополнительные сведения о размещении в службах IIS публикации и развертывания, см. в разделах:</span><span class="sxs-lookup"><span data-stu-id="9275f-185">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="9275f-186">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="9275f-186">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="9275f-187">Запустите приложение, чтобы убедиться, что проверка подлинности Windows работает.</span><span class="sxs-lookup"><span data-stu-id="9275f-187">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="9275f-188">Включить проверку подлинности Windows с использованием HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="9275f-188">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="9275f-189">Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [HTTP.sys](xref:fundamentals/servers/httpsys) для поддержки сценариев резидентных в Windows.</span><span class="sxs-lookup"><span data-stu-id="9275f-189">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="9275f-190">В следующем примере настраивается узел веб-приложения для использования HTTP.sys с проверкой подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="9275f-190">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="9275f-191">HTTP.sys делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos.</span><span class="sxs-lookup"><span data-stu-id="9275f-191">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="9275f-192">Проверка подлинности в режиме пользователя не поддерживается с Kerberos и HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="9275f-192">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="9275f-193">Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="9275f-193">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="9275f-194">Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.</span><span class="sxs-lookup"><span data-stu-id="9275f-194">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="9275f-195">HTTP.sys не поддерживается на сервере Nano Server версии 1709 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="9275f-195">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="9275f-196">Чтобы использовать проверку подлинности Windows и HTTP.sys с сервером Nano Server, используйте [контейнера Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="9275f-196">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="9275f-197">Дополнительные сведения о Server Core см. в разделе [Какова вариант установки Server Core в Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="9275f-197">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="9275f-198">Работа с проверкой подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="9275f-198">Work with Windows Authentication</span></span>

<span data-ttu-id="9275f-199">Состояние конфигурации анонимный доступ определяет способ, которым `[Authorize]` и `[AllowAnonymous]` атрибуты используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="9275f-199">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="9275f-200">Следующих двух разделах объясняется, как обрабатывать запрещенные и разрешенные конфигурации состояния анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="9275f-200">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="9275f-201">Запретить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="9275f-201">Disallow anonymous access</span></span>

<span data-ttu-id="9275f-202">Если включена проверка подлинности Windows и анонимный доступ отключен, `[Authorize]` и `[AllowAnonymous]` атрибуты никак не влияют.</span><span class="sxs-lookup"><span data-stu-id="9275f-202">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="9275f-203">Если IIS сайта (или HTTP.sys) настроен на запрет анонимного доступа, никогда не достигнут приложения.</span><span class="sxs-lookup"><span data-stu-id="9275f-203">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="9275f-204">По этой причине `[AllowAnonymous]` атрибут не применяется.</span><span class="sxs-lookup"><span data-stu-id="9275f-204">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="9275f-205">Разрешить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="9275f-205">Allow anonymous access</span></span>

<span data-ttu-id="9275f-206">Если включены как проверка подлинности Windows, так и анонимный доступ, используйте `[Authorize]` и `[AllowAnonymous]` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="9275f-206">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="9275f-207">`[Authorize]` Атрибут позволяет защищать частей приложения, которые действительно требует проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="9275f-207">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="9275f-208">`[AllowAnonymous]` Атрибут переопределения `[Authorize]` атрибут использования в приложениях, в которых разрешен анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="9275f-208">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="9275f-209">См. в разделе [простой авторизации](xref:security/authorization/simple) сведения об использовании атрибута.</span><span class="sxs-lookup"><span data-stu-id="9275f-209">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="9275f-210">В ASP.NET Core 2.x, `[Authorize]` атрибута требуется дополнительная конфигурация *Startup.cs* бросить вызов анонимные запросы для проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="9275f-210">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="9275f-211">Рекомендуемая конфигурация зависит от немного используется веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="9275f-211">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="9275f-212">По умолчанию пользователи, у которых отсутствует разрешение на доступ к странице, откроется пустой ответ HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="9275f-212">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="9275f-213">[StatusCodePages по промежуточного слоя](xref:fundamentals/error-handling#usestatuscodepages) можно настроить для предоставления пользователям более комфортные «Отказано в доступе».</span><span class="sxs-lookup"><span data-stu-id="9275f-213">The [StatusCodePages middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="9275f-214">IIS</span><span class="sxs-lookup"><span data-stu-id="9275f-214">IIS</span></span>

<span data-ttu-id="9275f-215">Если используется IIS, добавьте следующий код в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="9275f-215">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="9275f-216">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="9275f-216">HTTP.sys</span></span>

<span data-ttu-id="9275f-217">Если в файле HTTP.sys, добавьте следующий код в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="9275f-217">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="9275f-218">Олицетворение</span><span class="sxs-lookup"><span data-stu-id="9275f-218">Impersonation</span></span>

<span data-ttu-id="9275f-219">ASP.NET Core не реализует олицетворения.</span><span class="sxs-lookup"><span data-stu-id="9275f-219">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="9275f-220">Приложения запускаются с удостоверение приложения для всех запросов, с помощью удостоверения пула или процесс приложения.</span><span class="sxs-lookup"><span data-stu-id="9275f-220">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="9275f-221">Если вам нужно явно выполнять действия от имени пользователя, используйте [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) в [терминалов встроенного ПО промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9275f-221">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="9275f-222">Запустить одно действие в этом контексте, а затем закройте контекста.</span><span class="sxs-lookup"><span data-stu-id="9275f-222">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` <span data-ttu-id="9275f-223">не поддерживает асинхронные операции и не должны использоваться для сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="9275f-223">doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="9275f-224">Например упаковки всего запросов или по промежуточного слоя цепочек не поддерживается и не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="9275f-224">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

### <a name="claims-transformations"></a><span data-ttu-id="9275f-225">Преобразования утверждений</span><span class="sxs-lookup"><span data-stu-id="9275f-225">Claims transformations</span></span>

<span data-ttu-id="9275f-226">При размещении в режиме в процессе IIS <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя.</span><span class="sxs-lookup"><span data-stu-id="9275f-226">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="9275f-227">Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9275f-227">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="9275f-228">Дополнительные сведения и пример кода, который активирует преобразования утверждений, при размещении в процессе, см. в разделе <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="9275f-228">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>
