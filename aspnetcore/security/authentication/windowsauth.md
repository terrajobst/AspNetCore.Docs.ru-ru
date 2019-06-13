---
title: Настройка проверки подлинности Windows в ASP.NET Core
author: scottaddie
description: Узнайте, как настроить проверку подлинности Windows в ASP.NET Core для IIS и HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/12/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 93f833adff95f25d570947cd1a9035d652f522c2
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034953"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="32f9b-103">Настройка проверки подлинности Windows в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32f9b-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="32f9b-104">По [Scott Addie](https://twitter.com/Scott_Addie) и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="32f9b-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32f9b-105">Проверка подлинности Windows (также называется Negotiate, Kerberos или NTLM проверка подлинности) можно настроить для приложений ASP.NET Core, размещенных с [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), или [HTTP.sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="32f9b-105">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="32f9b-106">Проверка подлинности Windows (также называется Negotiate, Kerberos или NTLM проверка подлинности) можно настроить для приложений ASP.NET Core, размещенных с [IIS](xref:host-and-deploy/iis/index) или [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="32f9b-106">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

<span data-ttu-id="32f9b-107">Проверка подлинности Windows зависит от операционной системы для проверки подлинности пользователей, приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32f9b-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="32f9b-108">Можно использовать проверку подлинности Windows, когда сервер работает в корпоративной сети с помощью удостоверения домена Active Directory или учетных записей Windows для идентификации пользователей.</span><span class="sxs-lookup"><span data-stu-id="32f9b-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="32f9b-109">Проверка подлинности Windows является наилучшим образом подходит для среды интрасети, где пользователи, клиентские приложения и веб-серверы принадлежат к тому же домену Windows.</span><span class="sxs-lookup"><span data-stu-id="32f9b-109">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

> [!NOTE]
> <span data-ttu-id="32f9b-110">Проверка подлинности Windows не поддерживается с HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="32f9b-110">Windows Authentication isn't supported with HTTP/2.</span></span> <span data-ttu-id="32f9b-111">Проблемы проверки подлинности могут отправляться на ответы HTTP/2, но клиент необходимо понизить HTTP/1.1 до проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="32f9b-111">Authentication challenges can be sent on HTTP/2 responses, but the client must downgrade to HTTP/1.1 before authenticating.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="32f9b-112">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="32f9b-112">IIS/IIS Express</span></span>

<span data-ttu-id="32f9b-113">Добавить службы проверки подлинности путем вызова <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> пространства имен) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="32f9b-113">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="32f9b-114">Параметры (отладчик) запуска</span><span class="sxs-lookup"><span data-stu-id="32f9b-114">Launch settings (debugger)</span></span>

<span data-ttu-id="32f9b-115">Конфигурация параметров запуска влияет только на *Properties/launchSettings.json* файл для IIS Express и не настраивает IIS для Windows проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="32f9b-115">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="32f9b-116">Конфигурация сервера описан в [IIS](#iis) раздел.</span><span class="sxs-lookup"><span data-stu-id="32f9b-116">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="32f9b-117">**Веб-приложение** шаблон, доступный через Visual Studio или .NET Core CLI можно настроить для поддержки проверки подлинности Windows, которая обновляет *Properties/launchSettings.json* файла автоматически.</span><span class="sxs-lookup"><span data-stu-id="32f9b-117">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32f9b-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32f9b-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="32f9b-119">**новый проект**</span><span class="sxs-lookup"><span data-stu-id="32f9b-119">**New project**</span></span>

1. <span data-ttu-id="32f9b-120">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="32f9b-120">Create a new project.</span></span>
1. <span data-ttu-id="32f9b-121">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-121">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="32f9b-122">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-122">Select **Next**.</span></span>
1. <span data-ttu-id="32f9b-123">Введите имя в **имя_проекта** поля.</span><span class="sxs-lookup"><span data-stu-id="32f9b-123">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="32f9b-124">Подтвердите **расположение** запись правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="32f9b-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="32f9b-125">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-125">Select **Create**.</span></span>
1. <span data-ttu-id="32f9b-126">Выберите **изменение** под **проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-126">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="32f9b-127">В **изменить способ проверки подлинности** выберите **проверки подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-127">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="32f9b-128">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-128">Select **OK**.</span></span>
1. <span data-ttu-id="32f9b-129">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-129">Select **Web Application**.</span></span>
1. <span data-ttu-id="32f9b-130">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-130">Select **Create**.</span></span>

<span data-ttu-id="32f9b-131">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="32f9b-131">Run the app.</span></span> <span data-ttu-id="32f9b-132">Имя пользователя отображается в готовом для просмотра приложения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="32f9b-132">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="32f9b-133">**Существующий проект**</span><span class="sxs-lookup"><span data-stu-id="32f9b-133">**Existing project**</span></span>

<span data-ttu-id="32f9b-134">Свойства проекта, включите проверку подлинности Windows и отключить анонимную проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="32f9b-134">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="32f9b-135">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-135">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="32f9b-136">Выберите вкладку **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-136">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="32f9b-137">Снимите флажок для **Включение анонимной проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-137">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="32f9b-138">Установите флажок для **включить проверку подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-138">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="32f9b-139">Сохранить и закрыть страницу его свойств.</span><span class="sxs-lookup"><span data-stu-id="32f9b-139">Save and close the property page.</span></span>

<span data-ttu-id="32f9b-140">Кроме того, можно настроить свойства в `iisSettings` узел *launchSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="32f9b-140">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="32f9b-141">Visual Studio Code или .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="32f9b-141">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="32f9b-142">**новый проект**</span><span class="sxs-lookup"><span data-stu-id="32f9b-142">**New project**</span></span>

<span data-ttu-id="32f9b-143">Выполнение [команды dotnet new](/dotnet/core/tools/dotnet-new) с `webapp` аргумент (веб-приложения ASP.NET Core) и `--auth Windows` переключения:</span><span class="sxs-lookup"><span data-stu-id="32f9b-143">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="32f9b-144">**Существующий проект**</span><span class="sxs-lookup"><span data-stu-id="32f9b-144">**Existing project**</span></span>

<span data-ttu-id="32f9b-145">Обновление `iisSettings` узел *launchSettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="32f9b-145">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="32f9b-146">При изменении существующего проекта, убедитесь, что файл проекта содержит ссылку на пакет для [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **или** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="32f9b-146">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="32f9b-147">IIS</span><span class="sxs-lookup"><span data-stu-id="32f9b-147">IIS</span></span>

<span data-ttu-id="32f9b-148">Службы IIS используют [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32f9b-148">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="32f9b-149">Проверка подлинности Windows настроена для IIS с помощью *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="32f9b-149">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="32f9b-150">В следующих разделах показано как:</span><span class="sxs-lookup"><span data-stu-id="32f9b-150">The following sections show how to:</span></span>

* <span data-ttu-id="32f9b-151">Укажите локальный *web.config* файл, который активирует проверку подлинности Windows на сервере, при развертывании приложения.</span><span class="sxs-lookup"><span data-stu-id="32f9b-151">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="32f9b-152">Использование диспетчера служб IIS для настройки *web.config* файл приложения ASP.NET Core, которое уже развернуто на сервер.</span><span class="sxs-lookup"><span data-stu-id="32f9b-152">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="32f9b-153">Если вы еще не сделали, включении служб IIS для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32f9b-153">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="32f9b-154">Дополнительные сведения см. в разделе <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="32f9b-154">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="32f9b-155">Включение службы роли IIS для проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="32f9b-155">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="32f9b-156">Дополнительные сведения см. в разделе [включить проверку подлинности Windows в службы роли IIS (см. шаг 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="32f9b-156">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="32f9b-157">[По промежуточного слоя для интеграции IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) настраивается для автоматической проверки подлинности запросов по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="32f9b-157">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="32f9b-158">Дополнительные сведения см. в разделе [размещение ASP.NET Core в Windows со службами IIS: Параметры служб IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="32f9b-158">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="32f9b-159">Модуль ASP.NET Core настроен на пересылку токена проверки подлинности Windows в приложение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="32f9b-159">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="32f9b-160">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core: Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="32f9b-160">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="32f9b-161">Используйте **либо** из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="32f9b-161">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="32f9b-162">**Перед публикацией и развертывание проекта,** добавьте следующий *web.config* файл в корневую папку проекта:</span><span class="sxs-lookup"><span data-stu-id="32f9b-162">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="32f9b-163">При публикации проекта с помощью пакета SDK .NET Core (без `<IsTransformWebConfigDisabled>` свойство значение `true` в файле проекта), опубликованной *web.config* файл включает `<location><system.webServer><security><authentication>` раздел.</span><span class="sxs-lookup"><span data-stu-id="32f9b-163">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="32f9b-164">Дополнительные сведения о `<IsTransformWebConfigDisabled>` свойство, см. в разделе <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="32f9b-164">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="32f9b-165">**После публикации и развертывании проекта,** выполнения конфигурации на стороне сервера с помощью диспетчера IIS:</span><span class="sxs-lookup"><span data-stu-id="32f9b-165">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="32f9b-166">В диспетчере IIS выберите сайт IIS, в разделе **сайты** узел **подключений** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="32f9b-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="32f9b-167">Дважды щелкните **проверки подлинности** в **IIS** области.</span><span class="sxs-lookup"><span data-stu-id="32f9b-167">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="32f9b-168">Выберите **анонимную проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="32f9b-169">Выберите **отключить** в **действия** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="32f9b-169">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="32f9b-170">Выберите **проверки подлинности Windows**.</span><span class="sxs-lookup"><span data-stu-id="32f9b-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="32f9b-171">Выберите **включить** в **действия** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="32f9b-171">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="32f9b-172">При этих действий, IIS Manager вносит изменения в приложения *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="32f9b-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="32f9b-173">Объект `<system.webServer><security><authentication>` добавляется узел с обновленными параметрами для `anonymousAuthentication` и `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="32f9b-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="32f9b-174">`<system.webServer>` Добавлен в раздел *web.config* файл диспетчером IIS находится за пределами приложения `<location>` добавлен с помощью пакета SDK .NET Core, когда приложение публикуется раздел.</span><span class="sxs-lookup"><span data-stu-id="32f9b-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="32f9b-175">Поскольку раздел добавляется вне `<location>` узел, параметры, наследуются [дочерние приложения](xref:host-and-deploy/iis/index#sub-applications) с текущим приложением.</span><span class="sxs-lookup"><span data-stu-id="32f9b-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="32f9b-176">Чтобы запретить наследование, переместите добавленного `<security>` разделе внутри `<location><system.webServer>` раздел, в котором указан пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="32f9b-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="32f9b-177">Когда диспетчер IIS используется для добавления конфигурации IIS, оно влияет только на приложения *web.config* файл на сервере.</span><span class="sxs-lookup"><span data-stu-id="32f9b-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="32f9b-178">Последующие развертывания приложения может перезаписать параметры на сервере, если копия сервера *web.config* заменяется проекта *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="32f9b-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="32f9b-179">Используйте **либо** из следующих методов для управления параметрами:</span><span class="sxs-lookup"><span data-stu-id="32f9b-179">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="32f9b-180">Используйте диспетчер служб IIS, чтобы присвоить параметрам в *web.config* файл после файл перезаписывается при развертывании.</span><span class="sxs-lookup"><span data-stu-id="32f9b-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="32f9b-181">Добавить *файл web.config* приложение локально с параметрами.</span><span class="sxs-lookup"><span data-stu-id="32f9b-181">Add a *web.config file* to the app locally with the settings.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a><span data-ttu-id="32f9b-182">Kestrel</span><span class="sxs-lookup"><span data-stu-id="32f9b-182">Kestrel</span></span>

 <span data-ttu-id="32f9b-183">[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) пакет NuGet может использоваться с [Kestrel](xref:fundamentals/servers/kestrel) для поддержки проверки подлинности Windows, с помощью Negotiate, Kerberos и NTLM в Windows, Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="32f9b-183">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet package can be used with [Kestrel](xref:fundamentals/servers/kestrel) to support Windows Authentication using Negotiate, Kerberos, and NTLM on Windows, Linux, and macOS.</span></span>

> [!WARNING]
> <span data-ttu-id="32f9b-184">Учетные данные могут сохраняться среди запросов на подключение.</span><span class="sxs-lookup"><span data-stu-id="32f9b-184">Credentials can be persisted across requests on a connection.</span></span> <span data-ttu-id="32f9b-185">*Согласование проверки подлинности не должны использоваться с прокси-серверы, если прокси-сервер поддерживает сходство соединения 1:1 (постоянное подключение), с Kestrel.*</span><span class="sxs-lookup"><span data-stu-id="32f9b-185">*Negotiate authentication must not be used with proxies unless the proxy maintains a 1:1 connection affinity (a persistent connection) with Kestrel.*</span></span> <span data-ttu-id="32f9b-186">Это означает, что не следует использовать проверку подлинности согласованием с Kestrel за службами IIS [модуль Core ASP.NET (ANCM) вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="32f9b-186">This means that Negotiate authentication must not be used with Kestrel behind the IIS [ASP.NET Core Module (ANCM) out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model).</span></span>

 <span data-ttu-id="32f9b-187">Добавить службы проверки подлинности путем вызова <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` пространства имен) и `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` пространства имен) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="32f9b-187">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) and `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) in `Startup.ConfigureServices`:</span></span>

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

<span data-ttu-id="32f9b-188">Добавьте по промежуточного слоя проверки подлинности путем вызова <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="32f9b-188">Add Authentication Middleware by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span></span>

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

<span data-ttu-id="32f9b-189">Дополнительные сведения о по промежуточного слоя, см. в разделе <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="32f9b-189">For more information on middleware, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="32f9b-190">Разрешены анонимные запросы.</span><span class="sxs-lookup"><span data-stu-id="32f9b-190">Anonymous requests are allowed.</span></span> <span data-ttu-id="32f9b-191">Используйте [авторизации ASP.NET Core](xref:security/authorization/introduction) бросить вызов анонимные запросы для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="32f9b-191">Use [ASP.NET Core Authorization](xref:security/authorization/introduction) to challenge anonymous requests for authentication.</span></span>

### <a name="windows-environment-configuration"></a><span data-ttu-id="32f9b-192">Конфигурация среды Windows</span><span class="sxs-lookup"><span data-stu-id="32f9b-192">Windows environment configuration</span></span>

<span data-ttu-id="32f9b-193">[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) компонент выполняет проверку подлинности в режиме пользователя.</span><span class="sxs-lookup"><span data-stu-id="32f9b-193">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) component performs User Mode authentication.</span></span> <span data-ttu-id="32f9b-194">Необходимо добавить имена участника-службы (SPN) с учетной записью пользователя, под управлением службы, а не учетной записи компьютера.</span><span class="sxs-lookup"><span data-stu-id="32f9b-194">Service Principal Names (SPNs) must be added to the user account running the service, not the machine account.</span></span> <span data-ttu-id="32f9b-195">Выполнение `setspn -S HTTP/mysrevername.mydomain.com myuser` в административной командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="32f9b-195">Execute `setspn -S HTTP/mysrevername.mydomain.com myuser` in an administrative command shell.</span></span>

### <a name="linux-and-macos-environment-configuration"></a><span data-ttu-id="32f9b-196">Конфигурации среды Linux и macOS</span><span class="sxs-lookup"><span data-stu-id="32f9b-196">Linux and macOS environment configuration</span></span>

<span data-ttu-id="32f9b-197">Инструкции по присоединению к домену Windows машину Linux или macOS [подключение к серверу SQL Server, использование проверки подлинности Windows — Kerberos Studio данных Azure](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) статьи.</span><span class="sxs-lookup"><span data-stu-id="32f9b-197">Instructions for joining a Linux or macOS machine to a Windows domain are available in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span></span> <span data-ttu-id="32f9b-198">Инструкции создания учетной записи компьютера для компьютера Linux в домене.</span><span class="sxs-lookup"><span data-stu-id="32f9b-198">The instructions create a machine account for the Linux machine on the domain.</span></span> <span data-ttu-id="32f9b-199">Имена участников-служб необходимо добавить в эту учетную запись компьютера.</span><span class="sxs-lookup"><span data-stu-id="32f9b-199">SPNs must be added to that machine account.</span></span>

> [!NOTE]
> <span data-ttu-id="32f9b-200">Если следовать инструкциям в [подключение к серверу SQL Server, использование проверки подлинности Windows — Kerberos Studio данных Azure](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) статьи, замените `python-software-properties` с `python3-software-properties` при необходимости.</span><span class="sxs-lookup"><span data-stu-id="32f9b-200">When following the guidance in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, replace `python-software-properties` with `python3-software-properties` if needed.</span></span>

<span data-ttu-id="32f9b-201">Как только Linux или macOS компьютер присоединен к домену, необходимы дополнительные действия для предоставления [файла keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) с имена SPN:</span><span class="sxs-lookup"><span data-stu-id="32f9b-201">Once the Linux or macOS machine is joined to the domain, additional steps are required to provide a [keytab file](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) with the SPNs:</span></span>

* <span data-ttu-id="32f9b-202">На контроллере домена добавьте учетную запись компьютера новой веб-службы SPN:</span><span class="sxs-lookup"><span data-stu-id="32f9b-202">On the domain controller, add new web service SPNs to the machine account:</span></span>
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* <span data-ttu-id="32f9b-203">Используйте [ktpass](/windows-server/administration/windows-commands/ktpass) для создания файла keytab:</span><span class="sxs-lookup"><span data-stu-id="32f9b-203">Use [ktpass](/windows-server/administration/windows-commands/ktpass) to generate a keytab file:</span></span>
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * <span data-ttu-id="32f9b-204">Некоторые поля должно быть указано в верхний регистр, как указано.</span><span class="sxs-lookup"><span data-stu-id="32f9b-204">Some fields must be specified in uppercase as indicated.</span></span>
* <span data-ttu-id="32f9b-205">Скопируйте файл keytab на компьютер Linux или macOS.</span><span class="sxs-lookup"><span data-stu-id="32f9b-205">Copy the keytab file to the Linux or macOS machine.</span></span>
* <span data-ttu-id="32f9b-206">Выберите файл keytab через переменную среды: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span><span class="sxs-lookup"><span data-stu-id="32f9b-206">Select the keytab file via an environment variable: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span></span>
* <span data-ttu-id="32f9b-207">Вызвать `klist` Показать имена SPN, доступные в настоящий момент.</span><span class="sxs-lookup"><span data-stu-id="32f9b-207">Invoke `klist` to show the SPNs currently available for use.</span></span>

> [!NOTE]
> <span data-ttu-id="32f9b-208">Файла keytab содержит учетные данные домена для доступа и должен быть защищен соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="32f9b-208">A keytab file contains domain access credentials and must be protected accordingly.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="32f9b-209">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="32f9b-209">HTTP.sys</span></span>

<span data-ttu-id="32f9b-210">[HTTP.sys](xref:fundamentals/servers/httpsys) поддерживает проверку подлинности Windows режима ядра с помощью Negotiate, NTLM или обычную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="32f9b-210">[HTTP.sys](xref:fundamentals/servers/httpsys) supports Kernel Mode Windows Authentication using Negotiate, NTLM, or Basic authentication.</span></span>

<span data-ttu-id="32f9b-211">Добавить службы проверки подлинности путем вызова <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> пространства имен) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="32f9b-211">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="32f9b-212">Настроить узел веб-приложения для использования HTTP.sys с проверкой подлинности Windows (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="32f9b-212">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="32f9b-213"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> находится в пространстве имен <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="32f9b-213"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="32f9b-214">HTTP.sys делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos.</span><span class="sxs-lookup"><span data-stu-id="32f9b-214">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="32f9b-215">Проверка подлинности в режиме пользователя не поддерживается с Kerberos и HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="32f9b-215">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="32f9b-216">Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="32f9b-216">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="32f9b-217">Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.</span><span class="sxs-lookup"><span data-stu-id="32f9b-217">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="32f9b-218">HTTP.sys не поддерживается на сервере Nano Server версии 1709 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="32f9b-218">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="32f9b-219">Чтобы использовать проверку подлинности Windows и HTTP.sys с сервером Nano Server, используйте [контейнера Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="32f9b-219">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="32f9b-220">Дополнительные сведения о Server Core см. в разделе [Какова вариант установки Server Core в Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="32f9b-220">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="32f9b-221">Авторизация пользователей</span><span class="sxs-lookup"><span data-stu-id="32f9b-221">Authorize users</span></span>

<span data-ttu-id="32f9b-222">Состояние конфигурации анонимный доступ определяет способ, которым `[Authorize]` и `[AllowAnonymous]` атрибуты используются в приложении.</span><span class="sxs-lookup"><span data-stu-id="32f9b-222">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="32f9b-223">Следующих двух разделах объясняется, как обрабатывать запрещенные и разрешенные конфигурации состояния анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="32f9b-223">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="32f9b-224">Запретить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="32f9b-224">Disallow anonymous access</span></span>

<span data-ttu-id="32f9b-225">Если включена проверка подлинности Windows и анонимный доступ отключен, `[Authorize]` и `[AllowAnonymous]` атрибуты никак не влияют.</span><span class="sxs-lookup"><span data-stu-id="32f9b-225">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="32f9b-226">Если узел IIS настроен на запрет анонимного доступа, никогда не достигнут приложения.</span><span class="sxs-lookup"><span data-stu-id="32f9b-226">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="32f9b-227">По этой причине `[AllowAnonymous]` атрибут не применяется.</span><span class="sxs-lookup"><span data-stu-id="32f9b-227">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="32f9b-228">Разрешить анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="32f9b-228">Allow anonymous access</span></span>

<span data-ttu-id="32f9b-229">Если включены как проверка подлинности Windows, так и анонимный доступ, используйте `[Authorize]` и `[AllowAnonymous]` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="32f9b-229">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="32f9b-230">`[Authorize]` Атрибут позволяет защищать конечные точки приложения, которые требуют проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="32f9b-230">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="32f9b-231">`[AllowAnonymous]` Атрибут переопределения `[Authorize]` атрибут в приложениях с возможностью анонимного доступа.</span><span class="sxs-lookup"><span data-stu-id="32f9b-231">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="32f9b-232">Дополнительные сведения об использовании атрибута см. в разделе <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="32f9b-232">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="32f9b-233">По умолчанию пользователи, у которых отсутствует разрешение на доступ к странице, откроется пустой ответ HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="32f9b-233">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="32f9b-234">[StatusCodePages по промежуточного слоя](xref:fundamentals/error-handling#usestatuscodepages) можно настроить для предоставления пользователям более комфортные «Отказано в доступе».</span><span class="sxs-lookup"><span data-stu-id="32f9b-234">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="32f9b-235">Олицетворение</span><span class="sxs-lookup"><span data-stu-id="32f9b-235">Impersonation</span></span>

<span data-ttu-id="32f9b-236">ASP.NET Core не реализует олицетворения.</span><span class="sxs-lookup"><span data-stu-id="32f9b-236">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="32f9b-237">Приложения запускаются с удостоверение приложения для всех запросов, с помощью удостоверения пула или процесс приложения.</span><span class="sxs-lookup"><span data-stu-id="32f9b-237">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="32f9b-238">Если приложение должно выполнить действие от имени пользователя, используйте [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) в [терминалов встроенного ПО промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="32f9b-238">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="32f9b-239">Запустить одно действие в этом контексте, а затем закройте контекста.</span><span class="sxs-lookup"><span data-stu-id="32f9b-239">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="32f9b-240">`RunImpersonated` не поддерживает асинхронные операции и не должны использоваться для сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="32f9b-240">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="32f9b-241">Например упаковки всего запросов или по промежуточного слоя цепочек не поддерживается и не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="32f9b-241">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32f9b-242">Хотя [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) пакет включает проверку подлинности в Windows, Linux и macOS, олицетворение поддерживается только в Windows.</span><span class="sxs-lookup"><span data-stu-id="32f9b-242">While the [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package enables authentication on Windows, Linux, and macOS, impersonation is only supported on Windows.</span></span>

::: moniker-end

## <a name="claims-transformations"></a><span data-ttu-id="32f9b-243">Преобразования утверждений</span><span class="sxs-lookup"><span data-stu-id="32f9b-243">Claims transformations</span></span>

<span data-ttu-id="32f9b-244">При размещении в режиме в процессе IIS <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя.</span><span class="sxs-lookup"><span data-stu-id="32f9b-244">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="32f9b-245">Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="32f9b-245">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="32f9b-246">Дополнительные сведения и пример кода, который активирует преобразования утверждений, при размещении в процессе, см. в разделе <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="32f9b-246">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="32f9b-247">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="32f9b-247">Additional resources</span></span>

* [<span data-ttu-id="32f9b-248">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="32f9b-248">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
