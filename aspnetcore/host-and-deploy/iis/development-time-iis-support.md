---
title: Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core
author: shirhatti
description: Обнаружение поддержки для отладки приложений ASP.NET Core при запуске за IIS на сервере Windows.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="f233d-103">Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f233d-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="f233d-104">По [Sourabh Shirhatti](https://twitter.com/sshirhatti) и [Latham Люк](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f233d-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f233d-105">В этой статье описывается [Visual Studio](https://www.visualstudio.com/vs/) поддержка отладки приложений ASP.NET Core, работающие за IIS в Windows Server.</span><span class="sxs-lookup"><span data-stu-id="f233d-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="f233d-106">В этом разделе описывается включение этой функции и настройка проекта.</span><span class="sxs-lookup"><span data-stu-id="f233d-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f233d-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f233d-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="f233d-108">Активация IIS</span><span class="sxs-lookup"><span data-stu-id="f233d-108">Enable IIS</span></span>

1. <span data-ttu-id="f233d-109">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="f233d-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="f233d-110">Выберите **Internet Information Services** флажок.</span><span class="sxs-lookup"><span data-stu-id="f233d-110">Select the **Internet Information Services** check box.</span></span>

![Отображение компонентов Windows Internet Information Services флажков как черный квадрат (не в виде галочки), указывающее на то, что некоторые функции IIS включены](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="f233d-112">Для установки служб IIS может потребоваться перезагрузка компьютера.</span><span class="sxs-lookup"><span data-stu-id="f233d-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="f233d-113">Настройка IIS</span><span class="sxs-lookup"><span data-stu-id="f233d-113">Configure IIS</span></span>

<span data-ttu-id="f233d-114">Службы IIS необходимо иметь следующие настройки веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="f233d-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="f233d-115">Имя узла для имени узла, соответствующий URL-адрес профиля запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="f233d-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="f233d-116">Привязка для порта 443, назначенный сертификатом.</span><span class="sxs-lookup"><span data-stu-id="f233d-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="f233d-117">Например **имя узла** для добавленных веб-сайт имеет значение «localhost» (профиль запуска будет также использовать «localhost» далее в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="f233d-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="f233d-118">Порт имеет значение «443» (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f233d-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="f233d-119">**IIS Express разработки сертификат** назначается веб-сайт, но любой действительный сертификат works:</span><span class="sxs-lookup"><span data-stu-id="f233d-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Добавьте окно веб-сайта в IIS, отображение привязка, указанная для localhost на порте 443 с назначенного сертификата.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="f233d-121">Если установка IIS уже имеет **веб-сайт по умолчанию** с именем узла, который соответствует имя узла URL-адрес профиля запуска приложения:</span><span class="sxs-lookup"><span data-stu-id="f233d-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="f233d-122">Добавьте привязку порта для порта 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f233d-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="f233d-123">Назначьте действительный сертификат для веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="f233d-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="f233d-124">Включить поддержку IIS во время разработки в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f233d-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="f233d-125">Запустите установщик Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f233d-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="f233d-126">Выберите **времени разработки, IIS поддерживает** компонента.</span><span class="sxs-lookup"><span data-stu-id="f233d-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="f233d-127">Компонент указан как необязательные в **Сводка** панели для **ASP.NET и веб-разработки** рабочей нагрузки.</span><span class="sxs-lookup"><span data-stu-id="f233d-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="f233d-128">Устанавливает компонент [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), который является собственный модуль IIS, необходимые для запуска приложения под IIS ASP.NET Core в конфигурации обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="f233d-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Изменение функций Visual Studio: выбрана вкладка "Рабочие нагрузки".](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="f233d-132">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="f233d-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="f233d-133">Перенаправление на HTTPS</span><span class="sxs-lookup"><span data-stu-id="f233d-133">HTTPS redirection</span></span>

<span data-ttu-id="f233d-134">Для нового проекта, установите флажок для **настроить для использования протокола HTTPS** в **нового веб-приложения ASP.NET Core** окна:</span><span class="sxs-lookup"><span data-stu-id="f233d-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Новое окно веб-приложения ASP.NET Core, настройка для HTTPS флажок установлен.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="f233d-136">В существующий проект, с помощью по промежуточного слоя перенаправления HTTPS в `Startup.Configure` путем вызова [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) метода расширения:</span><span class="sxs-lookup"><span data-stu-id="f233d-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="f233d-137">Профиль запуска служб IIS</span><span class="sxs-lookup"><span data-stu-id="f233d-137">IIS launch profile</span></span>

<span data-ttu-id="f233d-138">Создание профиля запуска для добавления поддержки IIS во время разработки:</span><span class="sxs-lookup"><span data-stu-id="f233d-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="f233d-139">Для **профиль**выберите **New** кнопки.</span><span class="sxs-lookup"><span data-stu-id="f233d-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="f233d-140">Имя профиля «IIS» во всплывающем окне.</span><span class="sxs-lookup"><span data-stu-id="f233d-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="f233d-141">Выберите **ОК** для создания профиля.</span><span class="sxs-lookup"><span data-stu-id="f233d-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="f233d-142">Для **запуска** выберите **IIS** из списка.</span><span class="sxs-lookup"><span data-stu-id="f233d-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="f233d-143">Установите флажок для **браузер** и укажите URL-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="f233d-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="f233d-144">Использование протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f233d-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="f233d-145">В этом примере используется `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="f233d-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="f233d-146">В **переменных среды** выберите **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="f233d-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="f233d-147">Укажите переменную среды с ключом `ASPNETCORE_ENVIRONMENT` и значение `Development`.</span><span class="sxs-lookup"><span data-stu-id="f233d-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="f233d-148">В **параметров веб-сервера** задайте **URL-адрес приложения**.</span><span class="sxs-lookup"><span data-stu-id="f233d-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="f233d-149">В этом примере используется `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="f233d-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="f233d-150">Сохранение профиля.</span><span class="sxs-lookup"><span data-stu-id="f233d-150">Save the profile.</span></span>

![Окно свойств проекта с выбранной вкладкой "Отладка".](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="f233d-155">Можно также вручную добавить профиль запуска для [launchSettings.json](http://json.schemastore.org/launchsettings) файл в приложении:</span><span class="sxs-lookup"><span data-stu-id="f233d-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="f233d-156">Запуск проекта</span><span class="sxs-lookup"><span data-stu-id="f233d-156">Run the project</span></span>

<span data-ttu-id="f233d-157">В пользовательском Интерфейсе VS присвоено кнопку Выполнить **IIS** профиль и нажмите кнопку, чтобы запустить приложение:</span><span class="sxs-lookup"><span data-stu-id="f233d-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![На панели инструментов VS, равным «IIS» профиль запуска.](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="f233d-159">Visual Studio могут потребовать перезапуска, если не запущена с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="f233d-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="f233d-160">Перезапустите Visual Studio при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="f233d-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="f233d-161">Если используется сертификат ненадежных разработки, может потребоваться обозревателя позволяет создавать исключения для ненадежный сертификат.</span><span class="sxs-lookup"><span data-stu-id="f233d-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f233d-162">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f233d-162">Additional resources</span></span>

* [<span data-ttu-id="f233d-163">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="f233d-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="f233d-164">Введение в модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f233d-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="f233d-165">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f233d-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="f233d-166">Принудительное использование HTTPS</span><span class="sxs-lookup"><span data-stu-id="f233d-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
