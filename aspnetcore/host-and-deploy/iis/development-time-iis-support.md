---
title: Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core
author: shirhatti
description: Узнайте о поддерживаемых возможностях для отладки приложений ASP.NET Core, выполняемых в службах IIS на Windows Server.
ms.author: riande
ms.custom: mvc
ms.date: 11/30/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 51375e6a6bb25a469d467ca97a151abd305c1ece
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862386"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="e4a66-103">Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4a66-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="e4a66-104">Авторы: [Сурабх Ширхатти](https://twitter.com/sshirhatti) (Sourabh Shirhatti) и [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="e4a66-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e4a66-105">В этой статье описаны поддерживаемые в [Visual Studio](https://www.visualstudio.com/vs/) возможности для отладки приложений ASP.NET Core, выполняющихся в службах IIS в Windows Server.</span><span class="sxs-lookup"><span data-stu-id="e4a66-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="e4a66-106">Также здесь приведены пошаговые инструкции по активации этой функциональности и настройке проекта.</span><span class="sxs-lookup"><span data-stu-id="e4a66-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4a66-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e4a66-107">Prerequisites</span></span>

* [<span data-ttu-id="e4a66-108">Visual Studio для Windows</span><span class="sxs-lookup"><span data-stu-id="e4a66-108">Visual Studio for Windows</span></span>](https://www.microsoft.com/net/download/windows)
* <span data-ttu-id="e4a66-109">Рабочая нагрузка **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="e4a66-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="e4a66-110">Рабочая нагрузка **Кроссплатформенная разработка .NET Core**</span><span class="sxs-lookup"><span data-stu-id="e4a66-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="e4a66-111">Сертификат безопасности X.509</span><span class="sxs-lookup"><span data-stu-id="e4a66-111">X.509 security certificate</span></span>

## <a name="enable-iis"></a><span data-ttu-id="e4a66-112">Активация IIS</span><span class="sxs-lookup"><span data-stu-id="e4a66-112">Enable IIS</span></span>

1. <span data-ttu-id="e4a66-113">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="e4a66-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="e4a66-114">Установите флажок **Службы IIS**.</span><span class="sxs-lookup"><span data-stu-id="e4a66-114">Select the **Internet Information Services** check box.</span></span>

![Показаны компоненты Windows, где флажок "Службы IIS" отображен в виде черного квадрата (не галочки), что означает, что некоторые функции IIS включены](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="e4a66-116">Для установки служб IIS, возможно, потребуется перезагрузить компьютер.</span><span class="sxs-lookup"><span data-stu-id="e4a66-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="e4a66-117">Настройка IIS</span><span class="sxs-lookup"><span data-stu-id="e4a66-117">Configure IIS</span></span>

<span data-ttu-id="e4a66-118">В службах IIS нужно настроить веб-сайт со следующими характеристиками:</span><span class="sxs-lookup"><span data-stu-id="e4a66-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="e4a66-119">имя узла, которое соответствует URL-адресу узла для профиля запуска приложения;</span><span class="sxs-lookup"><span data-stu-id="e4a66-119">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="e4a66-120">привязка для порта 443 с назначенным сертификатом.</span><span class="sxs-lookup"><span data-stu-id="e4a66-120">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="e4a66-121">Например, **Имя узла** для добавленного веб-сайта имеет значение localhost (это же значение будет использоваться в профиле запуска далее в этой статье).</span><span class="sxs-lookup"><span data-stu-id="e4a66-121">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="e4a66-122">Порт имеет значение 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="e4a66-122">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="e4a66-123">Нашему веб-сайту назначен **сертификат разработки IIS Express**, но подойдет и любой другой действительный сертификат:</span><span class="sxs-lookup"><span data-stu-id="e4a66-123">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Окно добавления веб-сайта в IIS с настроенной привязкой для localhost и порта 443, которой назначен сертификат.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="e4a66-125">Если для установки IIS уже настроен **веб-сайт по умолчанию** с именем узла, которое совпадает с именем узла URL-адреса для профиля запуска приложения, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="e4a66-125">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="e4a66-126">Добавьте привязку для порта 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="e4a66-126">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="e4a66-127">Назначьте веб-сайту действительный сертификат.</span><span class="sxs-lookup"><span data-stu-id="e4a66-127">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="e4a66-128">Включение поддержки служб IIS в Visual Studio во время разработки</span><span class="sxs-lookup"><span data-stu-id="e4a66-128">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="e4a66-129">Запустите установщик Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e4a66-129">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="e4a66-130">Выберите компонент **поддержки IIS во время разработки**.</span><span class="sxs-lookup"><span data-stu-id="e4a66-130">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="e4a66-131">Этот компонент указан как дополнительный на панели **Сводка** для рабочей нагрузки **ASP.NET и веб-разработки**.</span><span class="sxs-lookup"><span data-stu-id="e4a66-131">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="e4a66-132">Этот компонент выполнит установку [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), который является собственным модулем IIS, необходимым для запуска приложений ASP.NET Core со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="e4a66-132">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

![Изменение функций Visual Studio: выбрана вкладка "Рабочие нагрузки".](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="e4a66-136">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="e4a66-136">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="e4a66-137">Перенаправление HTTPS</span><span class="sxs-lookup"><span data-stu-id="e4a66-137">HTTPS redirection</span></span>

<span data-ttu-id="e4a66-138">Если это новый проект, установите флажок **Configure for HTTPS** (Настроить для HTTPS) в окне **создания веб-приложения ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="e4a66-138">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Окно создания веб-приложения ASP.NET Core с установленным флажком настройки для HTTPS.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="e4a66-140">Если это существующий проект, используйте ПО промежуточного слоя для перенаправления HTTPS из `Startup.Configure`, вызвав метод расширения [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection):</span><span class="sxs-lookup"><span data-stu-id="e4a66-140">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a><span data-ttu-id="e4a66-141">Профиль запуска служб IIS</span><span class="sxs-lookup"><span data-stu-id="e4a66-141">IIS launch profile</span></span>

<span data-ttu-id="e4a66-142">Создайте новый профиль запуска, чтобы добавить поддержку IIS во время разработки.</span><span class="sxs-lookup"><span data-stu-id="e4a66-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="e4a66-143">В разделе **Профиль** нажмите кнопку **Новый**.</span><span class="sxs-lookup"><span data-stu-id="e4a66-143">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="e4a66-144">Во всплывающем окне присвойте новому профилю имя IIS.</span><span class="sxs-lookup"><span data-stu-id="e4a66-144">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="e4a66-145">Нажмите кнопку **ОК**, чтобы создать проект.</span><span class="sxs-lookup"><span data-stu-id="e4a66-145">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="e4a66-146">В поле **Запуск** выберите из списка значение **IIS**.</span><span class="sxs-lookup"><span data-stu-id="e4a66-146">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="e4a66-147">Установите флажок **Запуск браузера** и укажите URL-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="e4a66-147">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="e4a66-148">Выберите протокол HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e4a66-148">Use the HTTPS protocol.</span></span> <span data-ttu-id="e4a66-149">В этом примере используется `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="e4a66-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="e4a66-150">В разделе **Переменные среды** нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="e4a66-150">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="e4a66-151">Введите переменную среды с ключом `ASPNETCORE_ENVIRONMENT` и значением `Development`.</span><span class="sxs-lookup"><span data-stu-id="e4a66-151">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="e4a66-152">В области **Параметры веб-сервера** задайте **URL-адрес приложения**.</span><span class="sxs-lookup"><span data-stu-id="e4a66-152">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="e4a66-153">В этом примере используется `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="e4a66-153">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="e4a66-154">Сохраните профиль.</span><span class="sxs-lookup"><span data-stu-id="e4a66-154">Save the profile.</span></span>

![Окно свойств проекта с выбранной вкладкой "Отладка".](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="e4a66-159">Вы также можете вручную добавить профиль запуска в файл [launchSettings.json](http://json.schemastore.org/launchsettings) для вашего приложения:</span><span class="sxs-lookup"><span data-stu-id="e4a66-159">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a><span data-ttu-id="e4a66-160">Запуск проекта</span><span class="sxs-lookup"><span data-stu-id="e4a66-160">Run the project</span></span>

<span data-ttu-id="e4a66-161">В Visual Studio сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="e4a66-161">In Visual Studio:</span></span>

* <span data-ttu-id="e4a66-162">Убедитесь, что для раскрывающегося списка с конфигурацией сборки построения выбрано значение **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="e4a66-162">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="e4a66-163">Выберите для кнопки "Выполнить" профиль **IIS** и нажмите ее запуска приложения:</span><span class="sxs-lookup"><span data-stu-id="e4a66-163">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

![Для кнопки "Выполнить" на панели инструментов VS выбран профиль IIS. Для раскрывающегося списка конфигурации сборки выбрано значение "Выпуск".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="e4a66-165">Если вы вошли в Visual Studio без прав администратора, возможно, потребуется перезапуск.</span><span class="sxs-lookup"><span data-stu-id="e4a66-165">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="e4a66-166">Перезапустите Visual Studio при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="e4a66-166">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="e4a66-167">Если используется сертификат разработки без доверия, возможно, потребуется создать исключение для этого ненадежного сертификата по запросу в браузере.</span><span class="sxs-lookup"><span data-stu-id="e4a66-167">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="e4a66-168">Отладка конфигурации сборки выпуска с использованием функции [Только мой код](/visualstudio/debugger/just-my-code) и оптимизации компилятора приводит к ограничению возможностей.</span><span class="sxs-lookup"><span data-stu-id="e4a66-168">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="e4a66-169">Например, точки останова не будут достигнуты.</span><span class="sxs-lookup"><span data-stu-id="e4a66-169">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4a66-170">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e4a66-170">Additional resources</span></span>

* [<span data-ttu-id="e4a66-171">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="e4a66-171">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="e4a66-172">Введение в модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4a66-172">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="e4a66-173">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4a66-173">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="e4a66-174">Принудительное использование HTTPS</span><span class="sxs-lookup"><span data-stu-id="e4a66-174">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
