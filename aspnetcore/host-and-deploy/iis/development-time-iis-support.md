---
title: Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core
author: shirhatti
description: Обнаружение поддержки для отладки приложений ASP.NET Core при запуске за IIS на сервере Windows.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="7ec78-103">Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ec78-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="7ec78-104">Автор [Сурабх Ширхатти](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="7ec78-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="7ec78-105">В этой статье описывается [Visual Studio](https://www.visualstudio.com/vs/) поддержка отладки приложений ASP.NET Core, работающие за IIS в Windows Server.</span><span class="sxs-lookup"><span data-stu-id="7ec78-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="7ec78-106">В этом разделе описывается включение этой функции и настройка проекта.</span><span class="sxs-lookup"><span data-stu-id="7ec78-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ec78-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="7ec78-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="7ec78-108">Активация IIS</span><span class="sxs-lookup"><span data-stu-id="7ec78-108">Enable IIS</span></span>

<span data-ttu-id="7ec78-109">Включите службы IIS.</span><span class="sxs-lookup"><span data-stu-id="7ec78-109">Enable IIS.</span></span> <span data-ttu-id="7ec78-110">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="7ec78-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="7ec78-111">Установите флажок **Службы IIS**.</span><span class="sxs-lookup"><span data-stu-id="7ec78-111">Select the **Internet Information Services** checkbox.</span></span>

![Показаны компоненты Windows, где флажок "Службы IIS" отображен в виде черного квадрата (не галочки), что означает, что некоторые функции IIS включены](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="7ec78-113">Если во время установки IIS потребуется перезагрузка, перезагрузите систему.</span><span class="sxs-lookup"><span data-stu-id="7ec78-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="7ec78-114">Включение поддержки IIS во время разработки</span><span class="sxs-lookup"><span data-stu-id="7ec78-114">Enable development-time IIS support</span></span>

<span data-ttu-id="7ec78-115">Запустите установщик Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ec78-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="7ec78-116">Выберите **времени разработки, IIS поддерживает** компонента.</span><span class="sxs-lookup"><span data-stu-id="7ec78-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="7ec78-117">Компонент указан как необязательные в **Сводка** панели для **ASP.NET и веб-разработки** рабочей нагрузки.</span><span class="sxs-lookup"><span data-stu-id="7ec78-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="7ec78-118">При этом устанавливаются [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), который является собственный модуль IIS, необходимые для запуска приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ec78-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Изменение функций Visual Studio: выбрана вкладка "Рабочие нагрузки".](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="7ec78-122">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="7ec78-122">Configure the project</span></span>

<span data-ttu-id="7ec78-123">Создайте новый профиль запуска, чтобы добавить поддержку IIS во время разработки.</span><span class="sxs-lookup"><span data-stu-id="7ec78-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="7ec78-124">В **Обозревателе решений** Visual Studio щелкните проект правой кнопкой мыши и выберите **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="7ec78-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="7ec78-125">Выберите вкладку **Отладка**. Выберите **IIS** в раскрывающемся списке **Запуск**.</span><span class="sxs-lookup"><span data-stu-id="7ec78-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="7ec78-126">Проверьте, что функция **Запуск браузера** включена и для нее указан корректный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="7ec78-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Окно свойств проекта с выбранной вкладкой "Отладка".](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="7ec78-131">Можно также вручную добавить профиль запуска для [launchSettings.json](http://json.schemastore.org/launchsettings) файл в приложении:</span><span class="sxs-lookup"><span data-stu-id="7ec78-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="7ec78-132">Visual Studio могут потребовать перезапуска, если не запущена с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="7ec78-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="7ec78-133">Перезапустите Visual Studio при появлении соответствующего запроса.</span><span class="sxs-lookup"><span data-stu-id="7ec78-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ec78-134">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7ec78-134">Additional resources</span></span>

* [<span data-ttu-id="7ec78-135">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="7ec78-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="7ec78-136">Введение в модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ec78-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="7ec78-137">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ec78-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
