---
title: "Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core"
author: shirhatti
description: "Обнаружение поддержки для отладки приложений ASP.NET Core при запуске за IIS на сервере Windows."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core

Автор: [Sourabh Shirhatti](https://twitter.com/sshirhatti) (Сурабх Ширхатти)

В этой статье описывается [Visual Studio](https://www.visualstudio.com/vs/) поддержка отладки приложений ASP.NET Core, работающие за IIS в Windows Server. В этом разделе описывается включение этой функции и настройка проекта.

## <a name="prerequisites"></a>Предварительные требования

* Visual Studio (2017/версия 15.3 или более поздняя)
* ASP.NET и рабочая нагрузка веб-разработки *ИЛИ* рабочая нагрузка кроссплатформенной разработки .NET Core

## <a name="enable-iis"></a>Активация IIS

Включите службы IIS. Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана). Установите флажок **Службы IIS**.

![Показаны компоненты Windows, где флажок "Службы IIS" отображен в виде черного квадрата (не галочки), что означает, что некоторые функции IIS включены](development-time-iis-support/_static/enable_iis.png)

При установке IIS требует перезагрузки, перезагрузите систему.

## <a name="enable-development-time-iis-support"></a>Включение поддержки IIS во время разработки

Запустите установщик Visual Studio. Выберите **времени разработки, IIS поддерживает** компонента. Компонент указан как необязательные в **Сводка** панели для **ASP.NET и веб-разработки** рабочей нагрузки. При этом устанавливаются [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), который является собственный модуль IIS, необходимые для запуска приложений ASP.NET Core.

![Изменение функций Visual Studio: выбрана вкладка "Рабочие нагрузки". В разделе "Интернет и облако" выбрана панель "ASP.NET и веб-разработка". В правой области необязательно панель «Сводка» есть флажок для времени разработки, поддерживаемых IIS.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Настройка проекта

Создайте новый профиль запуска, чтобы добавить поддержку IIS во время разработки. В **Обозревателе решений** Visual Studio щелкните проект правой кнопкой мыши и выберите **Свойства**. Выберите вкладку **Отладка**. Выберите **IIS** в раскрывающемся списке **Запуск**. Проверьте, что функция **Запуск браузера** включена и для нее указан корректный URL-адрес.

![Окно свойств проекта с выбранной вкладкой "Отладка". В качестве параметров профиля и запуска задано IIS. Функция "Запуск браузера" включена и имеет адрес http://localhost/WebApplication2. Этот же адрес указан в поле "URL-адрес приложения" области "Параметры веб-сервера". Там же активирован параметр "Включить анонимный доступ".](development-time-iis-support/_static/project_properties.png)

Можно также вручную добавить профиль запуска для [launchSettings.json](http://json.schemastore.org/launchsettings) файл в приложении:

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

Visual Studio могут потребовать перезапуска, если не запущена с правами администратора. Перезапустите Visual Studio при появлении соответствующего запроса.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index)
* [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
