---
title: "Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core"
author: shirhatti
description: "Узнайте о поддерживаемых возможностях для отладки приложений ASP.NET Core при запуске в IIS на Windows Server."
keywords: "ASP.NET Core,internet information services,службы IIS,iis,windows server,модуль asp.net core,отладка"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="9bf3a-104">Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9bf3a-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="9bf3a-105">Автор: [Sourabh Shirhatti](https://twitter.com/sshirhatti) (Сурабх Ширхатти)</span><span class="sxs-lookup"><span data-stu-id="9bf3a-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="9bf3a-106">В этой статье описываются поддерживаемые в [Visual Studio](https://www.visualstudio.com/vs/) возможности для отладки приложений ASP.NET Core, запускаемых в IIS на Windows Server.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="9bf3a-107">Раздел показывает, как активировать эту функциональность и настроить ваш проект.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9bf3a-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="9bf3a-108">Prerequisites</span></span>

* <span data-ttu-id="9bf3a-109">Visual Studio (2017/версия 15.3 или более поздняя)</span><span class="sxs-lookup"><span data-stu-id="9bf3a-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="9bf3a-110">ASP.NET и рабочая нагрузка веб-разработки *ИЛИ* рабочая нагрузка кроссплатформенной разработки .NET Core</span><span class="sxs-lookup"><span data-stu-id="9bf3a-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="9bf3a-111">Активация IIS</span><span class="sxs-lookup"><span data-stu-id="9bf3a-111">Enable IIS</span></span>

<span data-ttu-id="9bf3a-112">Активируйте IIS в вашей системе.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-112">Enable IIS on your system.</span></span> <span data-ttu-id="9bf3a-113">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="9bf3a-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="9bf3a-114">Установите флажок **Службы IIS**.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-114">Select the **Internet Information Services** checkbox.</span></span>

![Показаны компоненты Windows, где флажок "Службы IIS" отображен в виде черного квадрата (не галочки), что означает, что некоторые функции IIS включены](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="9bf3a-116">Если для установки IIS требуется перезагрузка системы, выполните ее.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="9bf3a-117">Включение поддержки IIS во время разработки</span><span class="sxs-lookup"><span data-stu-id="9bf3a-117">Enable development-time IIS support</span></span>

<span data-ttu-id="9bf3a-118">После установки IIS запустите установщик Visual Studio, чтобы изменить существующую установку Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="9bf3a-119">В установщике выберите компонент **Поддержка IIS во время разработки**.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="9bf3a-120">Компонент находится в списке дополнительных компонентов на панели **Сводка** для рабочей нагрузки **ASP.NET и веб-разработка**.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="9bf3a-121">Будет выполнена установка [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), который является собственным модулем IIS, необходимым для запуска приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Изменение функций Visual Studio: выбрана вкладка "Рабочие нагрузки".](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="9bf3a-125">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="9bf3a-125">Configure the project</span></span>

<span data-ttu-id="9bf3a-126">Создайте новый профиль запуска, чтобы добавить поддержку IIS во время разработки.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="9bf3a-127">В **Обозревателе решений** Visual Studio щелкните проект правой кнопкой мыши и выберите **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="9bf3a-128">Выберите вкладку **Отладка**. Выберите **IIS** в раскрывающемся списке **Запуск**.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="9bf3a-129">Проверьте, что функция **Запуск браузера** включена и для нее указан корректный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Окно свойств проекта с выбранной вкладкой "Отладка".](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="9bf3a-134">Вы также можете вручную добавить профиль запуска в файл [launchSettings.json](http://json.schemastore.org/launchsettings) в приложении:</span><span class="sxs-lookup"><span data-stu-id="9bf3a-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="9bf3a-135">Возможно, от вас потребуется перезапустить Visual Studio, если вы запустили эту среду не от имени администратора.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="9bf3a-136">Перезапустите Visual Studio при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="9bf3a-137">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="9bf3a-137">Congratulations!</span></span> <span data-ttu-id="9bf3a-138">Теперь в вашем проекте настроена поддержка IIS во время разработки.</span><span class="sxs-lookup"><span data-stu-id="9bf3a-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9bf3a-139">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9bf3a-139">Additional resources</span></span>

* [<span data-ttu-id="9bf3a-140">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="9bf3a-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="9bf3a-141">Введение в модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9bf3a-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="9bf3a-142">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9bf3a-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
