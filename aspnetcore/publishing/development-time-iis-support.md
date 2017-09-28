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
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core

Автор: [Sourabh Shirhatti](https://twitter.com/sshirhatti) (Сурабх Ширхатти)

В этой статье описываются поддерживаемые в [Visual Studio](https://www.visualstudio.com/vs/) возможности для отладки приложений ASP.NET Core, запускаемых в IIS на Windows Server. Раздел показывает, как активировать эту функциональность и настроить ваш проект.

## <a name="prerequisites"></a>Предварительные требования

* Visual Studio (2017/версия 15.3 или более поздняя)
* ASP.NET и рабочая нагрузка веб-разработки *ИЛИ* рабочая нагрузка кроссплатформенной разработки .NET Core

## <a name="enable-iis"></a>Активация IIS

Активируйте IIS в вашей системе. Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана). Установите флажок **Службы IIS**.

![Показаны компоненты Windows, где флажок "Службы IIS" отображен в виде черного квадрата (не галочки), что означает, что некоторые функции IIS включены](development-time-iis-support/_static/enable_iis.png)

Если для установки IIS требуется перезагрузка системы, выполните ее.

## <a name="enable-development-time-iis-support"></a>Включение поддержки IIS во время разработки

После установки IIS запустите установщик Visual Studio, чтобы изменить существующую установку Visual Studio. В установщике выберите компонент **Поддержка IIS во время разработки**. Компонент находится в списке дополнительных компонентов на панели **Сводка** для рабочей нагрузки **ASP.NET и веб-разработка**. Будет выполнена установка [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), который является собственным модулем IIS, необходимым для запуска приложений ASP.NET Core.

![Изменение функций Visual Studio: выбрана вкладка "Рабочие нагрузки". В разделе "Интернет и облако" выбрана панель "ASP.NET и веб-разработка". Справа, в панели "Сводка", в области "Дополнительно" находится флажок "Поддержка IIS во время разработки".](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Настройка проекта

Создайте новый профиль запуска, чтобы добавить поддержку IIS во время разработки. В **Обозревателе решений** Visual Studio щелкните проект правой кнопкой мыши и выберите **Свойства**. Выберите вкладку **Отладка**. Выберите **IIS** в раскрывающемся списке **Запуск**. Проверьте, что функция **Запуск браузера** включена и для нее указан корректный URL-адрес.

![Окно свойств проекта с выбранной вкладкой "Отладка". В качестве параметров профиля и запуска задано IIS. Функция "Запуск браузера" включена и имеет адрес http://localhost/WebApplication2. Этот же адрес указан в поле "URL-адрес приложения" области "Параметры веб-сервера". Там же активирован параметр "Включить анонимный доступ".](development-time-iis-support/_static/project_properties.png)

Вы также можете вручную добавить профиль запуска в файл [launchSettings.json](http://json.schemastore.org/launchsettings) в приложении:

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

Возможно, от вас потребуется перезапустить Visual Studio, если вы запустили эту среду не от имени администратора. Перезапустите Visual Studio при появлении соответствующего запроса.

Поздравляем! Теперь в вашем проекте настроена поддержка IIS во время разработки. 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение ASP.NET Core в Windows со службами IIS](xref:publishing/iis)
* [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Справочник по конфигурации модуля ASP.NET Core](xref:hosting/aspnet-core-module)
