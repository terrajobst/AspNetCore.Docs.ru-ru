---
title: "Миграция конфигурации"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 8468d859-ff32-4a92-9e62-08c4a9e36594
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: f19a8389bd8c4035292306216522994b1dbdc506
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="migrating-configuration"></a>Миграция конфигурации

По [Стив Смит](http://ardalis.com) и [Скотт Addie](https://scottaddie.com)

В предыдущей статье мы начали [миграции проекта ASP.NET MVC в ASP.NET Core MVC](mvc.md). В этой статье мы миграцию конфигурации.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)

## <a name="setup-configuration"></a>Настройка установки

Больше не используется ASP.NET Core *Global.asax* и *web.config* файлы, загруженные в предыдущих версиях ASP.NET. В более ранних версиях ASP.NET логику запуска приложения был помещен в `Application_StartUp` метода в *Global.asax*. Далее, в ASP.NET MVC *файла Startup.cs* файл был включен в корне проекта; и он был вызван при запуске приложения. ASP.NET Core полностью приняла этот подход, поместив вся логика запуска в *файла Startup.cs* файл.

*Web.config* также завершает в ASP.NET Core. Сама конфигурация теперь можно, как часть процедуры при запуске приложения, описанной в *файла Startup.cs*. Конфигурация по-прежнему можно использовать XML-файлов, но обычно проекты ASP.NET Core будет помещать значения конфигурации в файл в формате JSON, такие как *appsettings.json*. Система конфигурации ASP.NET Core можно также легко получить переменные среды, в которых может предоставлять более безопасным и надежным для конкретных значений. Это особенно верно для секретов, такие как строки подключения и ключи API, которые не должны быть проверены в систему управления версиями. В разделе [конфигурации](../fundamentals/configuration.md) для получения дополнительных сведений о конфигурации в ASP.NET Core.

Для данной статьи мы приступаете к работе с частично миграции проекта ASP.NET Core из [предыдущей статьи](mvc.md). Для настройки конфигурации, добавьте следующий конструктор и свойства *файла Startup.cs* файл, расположенный в корневой папке проекта:

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

Обратите внимание, на этом этапе *файла Startup.cs* файл не будет компилироваться, как все равно нужно добавить следующие `using` инструкции:

```csharp
using Microsoft.Extensions.Configuration;
```

Добавить *appsettings.json* файла в корневую папку проекта, с помощью шаблона соответствующий элемент:

![Добавить AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Перенос параметров конфигурации из файла web.config

Наш проект ASP.NET MVC включены в строку подключения базы данных, необходимых в *web.config*в `<connectionStrings>` элемент. В данном проекте ASP.NET Core мы будем хранить эти сведения в *appsettings.json* файла. Откройте *appsettings.json*и обратите внимание, что он уже включает в себя следующее:

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


В выделенной строке, описанные выше, измените имя базы данных из **_CHANGE_ME** к имени базы данных.

## <a name="summary"></a>Сводка

ASP.NET Core заключает всю логику запуска приложения в одном файле, в котором необходимые службы и зависимости может быть определения и настройка. Он заменяет *web.config* файла с помощью функции гибкая настройка, который можно использовать в разнообразных форматах, например JSON, а также переменные среды.
