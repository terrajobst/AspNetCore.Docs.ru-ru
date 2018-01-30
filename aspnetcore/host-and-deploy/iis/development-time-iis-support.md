---
title: "Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core"
author: shirhatti
description: "Узнайте о поддерживаемых возможностях для отладки приложений ASP.NET Core при запуске в IIS на Windows Server."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a5f727dd21ac0c6702691df2215c42f4adc0ec27
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="62d3c-103">Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62d3c-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="62d3c-104">Автор: [Sourabh Shirhatti](https://twitter.com/sshirhatti) (Сурабх Ширхатти)</span><span class="sxs-lookup"><span data-stu-id="62d3c-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="62d3c-105">В этой статье описываются поддерживаемые в [Visual Studio](https://www.visualstudio.com/vs/) возможности для отладки приложений ASP.NET Core, запускаемых в IIS на Windows Server.</span><span class="sxs-lookup"><span data-stu-id="62d3c-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="62d3c-106">В этом разделе описывается включение этой функции и настройка проекта.</span><span class="sxs-lookup"><span data-stu-id="62d3c-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62d3c-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="62d3c-107">Prerequisites</span></span>

* <span data-ttu-id="62d3c-108">Visual Studio (2017/версия 15.3 или более поздняя)</span><span class="sxs-lookup"><span data-stu-id="62d3c-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="62d3c-109">ASP.NET и рабочая нагрузка веб-разработки *ИЛИ* рабочая нагрузка кроссплатформенной разработки .NET Core</span><span class="sxs-lookup"><span data-stu-id="62d3c-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="62d3c-110">Активация IIS</span><span class="sxs-lookup"><span data-stu-id="62d3c-110">Enable IIS</span></span>

<span data-ttu-id="62d3c-111">Включите службы IIS.</span><span class="sxs-lookup"><span data-stu-id="62d3c-111">Enable IIS.</span></span> <span data-ttu-id="62d3c-112">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="62d3c-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="62d3c-113">Установите флажок **Службы IIS**.</span><span class="sxs-lookup"><span data-stu-id="62d3c-113">Select the **Internet Information Services** checkbox.</span></span>

![Показаны компоненты Windows, где флажок "Службы IIS" отображен в виде черного квадрата (не галочки), что означает, что некоторые функции IIS включены](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="62d3c-115">При установке IIS требует перезагрузки, перезагрузите систему.</span><span class="sxs-lookup"><span data-stu-id="62d3c-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="62d3c-116">Включение поддержки IIS во время разработки</span><span class="sxs-lookup"><span data-stu-id="62d3c-116">Enable development-time IIS support</span></span>

<span data-ttu-id="62d3c-117">После установки IIS, запустите установщик Visual Studio, чтобы изменить существующую установку Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="62d3c-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="62d3c-118">В установщике выберите компонент **Поддержка IIS во время разработки**.</span><span class="sxs-lookup"><span data-stu-id="62d3c-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="62d3c-119">Компонент находится в списке дополнительных компонентов на панели **Сводка** для рабочей нагрузки **ASP.NET и веб-разработка**.</span><span class="sxs-lookup"><span data-stu-id="62d3c-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="62d3c-120">Будет выполнена установка [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), который является собственным модулем IIS, необходимым для запуска приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62d3c-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Изменение функций Visual Studio: выбрана вкладка "Рабочие нагрузки".](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="62d3c-124">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="62d3c-124">Configure the project</span></span>

<span data-ttu-id="62d3c-125">Создайте новый профиль запуска, чтобы добавить поддержку IIS во время разработки.</span><span class="sxs-lookup"><span data-stu-id="62d3c-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="62d3c-126">В **Обозревателе решений** Visual Studio щелкните проект правой кнопкой мыши и выберите **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="62d3c-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="62d3c-127">Выберите вкладку **Отладка**. Выберите **IIS** в раскрывающемся списке **Запуск**.</span><span class="sxs-lookup"><span data-stu-id="62d3c-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="62d3c-128">Проверьте, что функция **Запуск браузера** включена и для нее указан корректный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="62d3c-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Окно свойств проекта с выбранной вкладкой "Отладка".](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="62d3c-133">Можно также вручную добавить профиль запуска для [launchSettings.json](http://json.schemastore.org/launchsettings) файл в приложении:</span><span class="sxs-lookup"><span data-stu-id="62d3c-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

<span data-ttu-id="62d3c-134">Visual Studio могут потребовать перезапуска, если не запущена с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="62d3c-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="62d3c-135">Перезапустите Visual Studio при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="62d3c-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="62d3c-136">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="62d3c-136">Congratulations!</span></span> <span data-ttu-id="62d3c-137">На этом этапе проект настроен для поддержки IIS во время разработки.</span><span class="sxs-lookup"><span data-stu-id="62d3c-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="62d3c-138">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="62d3c-138">Additional resources</span></span>

* [<span data-ttu-id="62d3c-139">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="62d3c-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="62d3c-140">Введение в модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62d3c-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="62d3c-141">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62d3c-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
