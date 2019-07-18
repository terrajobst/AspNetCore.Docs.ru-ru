---
title: Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core
author: guardrex
description: Узнайте о поддерживаемых возможностях для отладки приложений ASP.NET Core, выполняемых в службах IIS в Windows Server.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: f2d5dbbdc80eec035616ddea234ee5d3343eeae8
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815179"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="58894-103">Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58894-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="58894-104">Авторы: [Сурабх Ширхатти](https://twitter.com/sshirhatti) (Sourabh Shirhatti) и [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="58894-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="58894-105">В этой статье описаны поддерживаемые в [Visual Studio](https://visualstudio.microsoft.com) возможности для отладки приложений ASP.NET Core, выполняющихся в службах IIS в Windows Server.</span><span class="sxs-lookup"><span data-stu-id="58894-105">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="58894-106">Также здесь приведены пошаговые инструкции по включению этого сценария и настройке проекта.</span><span class="sxs-lookup"><span data-stu-id="58894-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58894-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="58894-107">Prerequisites</span></span>

* [<span data-ttu-id="58894-108">Visual Studio для Windows</span><span class="sxs-lookup"><span data-stu-id="58894-108">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="58894-109">Рабочая нагрузка **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="58894-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="58894-110">Рабочая нагрузка **Кроссплатформенная разработка .NET Core**</span><span class="sxs-lookup"><span data-stu-id="58894-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="58894-111">Сертификат безопасности X.509 (для поддержки HTTPS)</span><span class="sxs-lookup"><span data-stu-id="58894-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="58894-112">Активация IIS</span><span class="sxs-lookup"><span data-stu-id="58894-112">Enable IIS</span></span>

1. <span data-ttu-id="58894-113">В Windows последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="58894-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="58894-114">Установите флажок **Службы IIS**.</span><span class="sxs-lookup"><span data-stu-id="58894-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="58894-115">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="58894-115">Select **OK**.</span></span>

<span data-ttu-id="58894-116">Для установки служб IIS, возможно, потребуется перезагрузить компьютер.</span><span class="sxs-lookup"><span data-stu-id="58894-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="58894-117">Настройка IIS</span><span class="sxs-lookup"><span data-stu-id="58894-117">Configure IIS</span></span>

<span data-ttu-id="58894-118">В службах IIS нужно настроить веб-сайт со следующими характеристиками:</span><span class="sxs-lookup"><span data-stu-id="58894-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="58894-119">**Имя узла**. Обычно используется **Веб-сайт по умолчанию** со значением `localhost`, заданным параметру **Имя узла**.</span><span class="sxs-lookup"><span data-stu-id="58894-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="58894-120">Но можно также задать любой допустимый веб-сайт IIS с уникальным именем узла.</span><span class="sxs-lookup"><span data-stu-id="58894-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="58894-121">**Привязка сайта**</span><span class="sxs-lookup"><span data-stu-id="58894-121">**Site Binding**</span></span>
  * <span data-ttu-id="58894-122">Для приложений, которым требуется протокол HTTPS, создайте привязку к порту 443 с помощью сертификата.</span><span class="sxs-lookup"><span data-stu-id="58894-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="58894-123">Обычно используется **сертификат разработки IIS Express**, но подойдет и любой другой действительный сертификат.</span><span class="sxs-lookup"><span data-stu-id="58894-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="58894-124">Для приложений, использующих протокол HTTP, подтвердите привязку к порту 80 или создайте привязку к порту 80 для нового сайта.</span><span class="sxs-lookup"><span data-stu-id="58894-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="58894-125">Используйте одну привязку либо для HTTP, либо для HTTPS.</span><span class="sxs-lookup"><span data-stu-id="58894-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="58894-126">**Одновременная привязка к портам HTTP и HTTPS не поддерживается.**</span><span class="sxs-lookup"><span data-stu-id="58894-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="58894-127">Включение поддержки служб IIS в Visual Studio во время разработки</span><span class="sxs-lookup"><span data-stu-id="58894-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="58894-128">Запустите установщик Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58894-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="58894-129">Выберите **Изменить** в установщике программы Visual Studio, которую планируется использовать для поддержки IIS во время разработки.</span><span class="sxs-lookup"><span data-stu-id="58894-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="58894-130">Для рабочей нагрузки **ASP.NET и разработка веб-приложений** найдите и установите компонент **Поддержка времени разработки в IIS**.</span><span class="sxs-lookup"><span data-stu-id="58894-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="58894-131">Компонент находится в разделе **Дополнительно** под пунктом **Поддержка времени разработки в IIS** на панели **Сведения об установке** справа от рабочих нагрузок.</span><span class="sxs-lookup"><span data-stu-id="58894-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="58894-132">Этот компонент выполнит установку [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module), который является собственным модулем IIS, необходимым для запуска приложений ASP.NET Core со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="58894-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="58894-133">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="58894-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="58894-134">Перенаправление HTTPS</span><span class="sxs-lookup"><span data-stu-id="58894-134">HTTPS redirection</span></span>

<span data-ttu-id="58894-135">Если новому проекту требуется протокол HTTPS, установите флажок **Configure for HTTPS** (Настроить для HTTPS) в окне **Создать веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="58894-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="58894-136">Установка флажка добавляет [ПО промежуточного слоя перенаправления HTTPS и HSTS](xref:security/enforcing-ssl) в приложение при его создании.</span><span class="sxs-lookup"><span data-stu-id="58894-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="58894-137">Если существующему проекту требуется протокол HTTPS, используйте ПО промежуточного слоя перенаправления HTTPS и HSTS в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="58894-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="58894-138">Дополнительные сведения можно найти по адресу: <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="58894-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="58894-139">Для проекта, который использует протокол HTTP, поддержку [ПО промежуточного слоя перенаправления HTTPS и HSTS](xref:security/enforcing-ssl) задавать не нужно.</span><span class="sxs-lookup"><span data-stu-id="58894-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="58894-140">Настройка приложения не требуется.</span><span class="sxs-lookup"><span data-stu-id="58894-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="58894-141">Профиль запуска служб IIS</span><span class="sxs-lookup"><span data-stu-id="58894-141">IIS launch profile</span></span>

<span data-ttu-id="58894-142">Создайте новый профиль запуска, чтобы добавить поддержку IIS во время разработки.</span><span class="sxs-lookup"><span data-stu-id="58894-142">Create a new launch profile to add development-time IIS support:</span></span>

::: moniker range=">= aspnetcore-3.0"

1. <span data-ttu-id="58894-143">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="58894-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="58894-144">Выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="58894-144">Select **Properties**.</span></span> <span data-ttu-id="58894-145">Откройте вкладку **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="58894-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="58894-146">В разделе **Профиль** нажмите кнопку **Новый**.</span><span class="sxs-lookup"><span data-stu-id="58894-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="58894-147">Во всплывающем окне присвойте новому профилю имя IIS.</span><span class="sxs-lookup"><span data-stu-id="58894-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="58894-148">Нажмите кнопку **ОК**, чтобы создать проект.</span><span class="sxs-lookup"><span data-stu-id="58894-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="58894-149">В поле **Запуск** выберите из списка значение **IIS**.</span><span class="sxs-lookup"><span data-stu-id="58894-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="58894-150">Установите флажок **Запуск браузера** и укажите URL-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="58894-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="58894-151">Если приложению требуется протокол HTTPS, используйте конечную точку HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="58894-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="58894-152">Для протокола HTTP используйте конечную точку HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="58894-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="58894-153">Укажите то же имя узла и тот же порт, как в [выполненной ранее настройке IIS](#configure-iis). Обычно это `localhost`.</span><span class="sxs-lookup"><span data-stu-id="58894-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="58894-154">Укажите имя приложения в конце URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="58894-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="58894-155">Например `https://localhost/WebApplication1` (HTTPS) или `http://localhost/WebApplication1` (HTTP) являются действительными URL-адресами конечной точки.</span><span class="sxs-lookup"><span data-stu-id="58894-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="58894-156">В разделе **Переменные среды** нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="58894-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="58894-157">Для переменной среды задайте **имя** `ASPNETCORE_ENVIRONMENT` и **значение** `Development`.</span><span class="sxs-lookup"><span data-stu-id="58894-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="58894-158">В области **Параметры веб-сервера** в поле **URL-адрес приложения** задайте значение, соответствующее URL-адресу конечной точки в поле **Запуск браузера**.</span><span class="sxs-lookup"><span data-stu-id="58894-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="58894-159">В Visual Studio 2019 и последующих версиях параметру **Модель размещения** задайте значение **По умолчанию**, чтобы использовать модель размещения проекта.</span><span class="sxs-lookup"><span data-stu-id="58894-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="58894-160">Если для проекта задано свойство `<AspNetCoreHostingModel>` в файле проекта, используется значение свойства `InProcess` или `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="58894-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="58894-161">Если свойство не задано, используется модель размещения приложения по умолчанию In Process.</span><span class="sxs-lookup"><span data-stu-id="58894-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="58894-162">Если приложению требуется явно указать модель размещения, отличную от обычной модели размещения приложения, задайте параметру **Модель размещения** значение `In Process` или `Out Of Process` по необходимости.</span><span class="sxs-lookup"><span data-stu-id="58894-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="58894-163">Сохраните профиль.</span><span class="sxs-lookup"><span data-stu-id="58894-163">Save the profile.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. <span data-ttu-id="58894-164">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="58894-164">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="58894-165">Выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="58894-165">Select **Properties**.</span></span> <span data-ttu-id="58894-166">Откройте вкладку **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="58894-166">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="58894-167">В разделе **Профиль** нажмите кнопку **Новый**.</span><span class="sxs-lookup"><span data-stu-id="58894-167">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="58894-168">Во всплывающем окне присвойте новому профилю имя IIS.</span><span class="sxs-lookup"><span data-stu-id="58894-168">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="58894-169">Нажмите кнопку **ОК**, чтобы создать проект.</span><span class="sxs-lookup"><span data-stu-id="58894-169">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="58894-170">В поле **Запуск** выберите из списка значение **IIS**.</span><span class="sxs-lookup"><span data-stu-id="58894-170">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="58894-171">Установите флажок **Запуск браузера** и укажите URL-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="58894-171">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="58894-172">Если приложению требуется протокол HTTPS, используйте конечную точку HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="58894-172">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="58894-173">Для протокола HTTP используйте конечную точку HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="58894-173">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="58894-174">Укажите то же имя узла и тот же порт, как в [выполненной ранее настройке IIS](#configure-iis). Обычно это `localhost`.</span><span class="sxs-lookup"><span data-stu-id="58894-174">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="58894-175">Укажите имя приложения в конце URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="58894-175">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="58894-176">Например `https://localhost/WebApplication1` (HTTPS) или `http://localhost/WebApplication1` (HTTP) являются действительными URL-адресами конечной точки.</span><span class="sxs-lookup"><span data-stu-id="58894-176">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="58894-177">В разделе **Переменные среды** нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="58894-177">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="58894-178">Для переменной среды задайте **имя** `ASPNETCORE_ENVIRONMENT` и **значение** `Development`.</span><span class="sxs-lookup"><span data-stu-id="58894-178">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="58894-179">В области **Параметры веб-сервера** в поле **URL-адрес приложения** задайте значение, соответствующее URL-адресу конечной точки в поле **Запуск браузера**.</span><span class="sxs-lookup"><span data-stu-id="58894-179">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="58894-180">В Visual Studio 2019 и последующих версиях параметру **Модель размещения** задайте значение **По умолчанию**, чтобы использовать модель размещения проекта.</span><span class="sxs-lookup"><span data-stu-id="58894-180">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="58894-181">Если для проекта задано свойство `<AspNetCoreHostingModel>` в файле проекта, используется значение свойства `InProcess` или `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="58894-181">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="58894-182">Если свойство не задано, используется модель размещения приложения по умолчанию Out Of Process.</span><span class="sxs-lookup"><span data-stu-id="58894-182">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="58894-183">Если приложению требуется явно указать модель размещения, отличную от обычной модели размещения приложения, задайте параметру **Модель размещения** значение `In Process` или `Out Of Process` по необходимости.</span><span class="sxs-lookup"><span data-stu-id="58894-183">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="58894-184">Сохраните профиль.</span><span class="sxs-lookup"><span data-stu-id="58894-184">Save the profile.</span></span>

::: moniker-end

<span data-ttu-id="58894-185">Если Visual Studio не используется, можно вручную добавить профиль запуска в файл [launchSettings.json](https://json.schemastore.org/launchsettings) в папке *Properties*.</span><span class="sxs-lookup"><span data-stu-id="58894-185">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="58894-186">В следующем примере настраивается профиль для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="58894-186">The following example configures the profile to use the HTTPS protocol:</span></span>

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

<span data-ttu-id="58894-187">Убедитесь, что конечные точки `applicationUrl` и `launchUrl` совпадают и используют тот же протокол (HTTP или HTTPS), что и конфигурация привязки IIS.</span><span class="sxs-lookup"><span data-stu-id="58894-187">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="58894-188">Запуск проекта</span><span class="sxs-lookup"><span data-stu-id="58894-188">Run the project</span></span>

<span data-ttu-id="58894-189">Запустите Visual Studio от имени администратора.</span><span class="sxs-lookup"><span data-stu-id="58894-189">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="58894-190">Убедитесь, что для раскрывающегося списка с конфигурацией сборки построения выбрано значение **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="58894-190">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="58894-191">Выберите для кнопки "Выполнить" профиль **IIS** и нажмите ее запуска приложения:</span><span class="sxs-lookup"><span data-stu-id="58894-191">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="58894-192">Если вы вошли в Visual Studio без прав администратора, возможно, потребуется перезапуск.</span><span class="sxs-lookup"><span data-stu-id="58894-192">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="58894-193">Перезапустите Visual Studio при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="58894-193">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="58894-194">Если используется сертификат разработки без доверия, возможно, потребуется создать исключение для этого ненадежного сертификата по запросу в браузере.</span><span class="sxs-lookup"><span data-stu-id="58894-194">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="58894-195">Отладка конфигурации сборки выпуска с использованием функции [Только мой код](/visualstudio/debugger/just-my-code) и оптимизации компилятора приводит к ограничению возможностей.</span><span class="sxs-lookup"><span data-stu-id="58894-195">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="58894-196">Например, точки останова не будут достигнуты.</span><span class="sxs-lookup"><span data-stu-id="58894-196">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58894-197">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="58894-197">Additional resources</span></span>

* [<span data-ttu-id="58894-198">Начало работы с диспетчером IIS в IIS</span><span class="sxs-lookup"><span data-stu-id="58894-198">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="58894-199">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="58894-199">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="58894-200">Введение в модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58894-200">Introduction to ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="58894-201">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58894-201">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="58894-202">Принудительное использование HTTPS</span><span class="sxs-lookup"><span data-stu-id="58894-202">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
