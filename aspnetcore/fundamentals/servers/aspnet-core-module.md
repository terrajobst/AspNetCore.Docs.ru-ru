---
title: Модуль ASP.NET Core
author: rick-anderson
description: Сведения о том, как модуль ASP.NET Core позволяет веб-серверу Kestrel использовать IIS или IIS Express в качестве обратного прокси-сервера.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153709"
---
# <a name="aspnet-core-module"></a>Модуль ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Рик Штраль](https://github.com/RickStrahl) (Rick Strahl) и [Крис Росс](https://github.com/Tratcher) (Chris Ross) 

Модуль Core ASP.NET позволяет приложениям ASP.NET Core выполняться за IIS при использовании обратного прокси-сервера. Служба IIS предоставляет расширенные функции безопасности и управляемости веб-приложения.

Поддерживаемые версии Windows:

* Windows 7 и более поздние версии
* Windows Server 2008 R2 и более поздние версии

Модуль ASP.NET Core работает только с Kestrel. Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys) (ранее — [WebListener](xref:fundamentals/servers/weblistener)).

## <a name="aspnet-core-module-description"></a>Описание модуля ASP.NET Core

Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для перенаправления веб-запросов во внутренние приложения ASP.NET Core. Многие собственные модули, такие как проверка подлинности Windows, остаются активными. Дополнительные сведения о модулях IIS, активных в этом модуле, см. в разделе [Модули IIS](xref:host-and-deploy/iis/modules).

Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль также обрабатывает управление процессами. Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое. Это, по сути, совпадает с поведением приложений ASP.NET 4.x, выполняемых внутрипроцессно в IIS и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложениями ASP.NET Core:

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm.png)

Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра. Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS). Модуль перенаправляет запросы Kestrel на случайный порт для приложения, который не является портом 80 или 443.

Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{port}`. Выполняются дополнительные проверки, и запросы не из модуля отклоняются. Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.

После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core. Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения. Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.

Модуль ASP.NET Core имеет несколько других функций. Функции модуля:

* Задание переменных среды для рабочего процесса.
* Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.
* Переадресация токенов проверки подлинности Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Как установить и использовать модуль ASP.NET Core

Подробные инструкции о том, как установить и использовать модуль ASP.NET Core, см. в разделе [Размещение в Windows с помощью IIS](xref:host-and-deploy/iis/index). Дополнительные сведения о настройке модуля см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение в Windows с помощью IIS](xref:host-and-deploy/iis/index)
* [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Репозиторий GitHub для модуля ASP.NET Core (исходный код)](https://github.com/aspnet/AspNetCoreModule)
