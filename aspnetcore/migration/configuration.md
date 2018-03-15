---
title: "Миграция конфигурации для ASP.NET Core"
author: ardalis
description: "Дополнительные сведения о миграции конфигурации из проекта ASP.NET MVC в проект ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: 6c72b324de49a03a3b2c4e96ba8886d1ed249103
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="migrating-configuration-to-aspnet-core"></a>Миграция конфигурации для ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Скотт Эдди](https://scottaddie.com) (Scott Addie)

В предыдущей статье мы начали [миграции проекта ASP.NET MVC в ASP.NET Core MVC](mvc.md). В этой статье мы миграцию конфигурации.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Настройка установки

Больше не используется ASP.NET Core *Global.asax* и *web.config* файлы, загруженные в предыдущих версиях ASP.NET. В более ранних версиях ASP.NET логику запуска приложения был помещен в `Application_StartUp` метода в *Global.asax*. Далее, в ASP.NET MVC *файла Startup.cs* файл был включен в корне проекта; и он был вызван при запуске приложения. ASP.NET Core полностью приняла этот подход, поместив вся логика запуска в *файла Startup.cs* файл.

*Web.config* также завершает в ASP.NET Core. Сама конфигурация теперь можно, как часть процедуры при запуске приложения, описанной в *файла Startup.cs*. Конфигурация по-прежнему можно использовать XML-файлов, но обычно проекты ASP.NET Core будет помещать значения конфигурации в файл в формате JSON, такие как *appsettings.json*. Система конфигурации ASP.NET Core можно также легко получить переменные среды, которые содержат информацию о [более безопасным и надежным расположение](xref:security/app-secrets) для конкретных значений. Это особенно верно для секретов, такие как строки подключения и ключи API, которые не должны проверяться в системе управления версиями. В разделе [конфигурации](xref:fundamentals/configuration/index) для получения дополнительных сведений о конфигурации в ASP.NET Core.

Для данной статьи мы приступаете к работе с частично миграции проекта ASP.NET Core из [предыдущей статьи](mvc.md). Для настройки конфигурации, добавьте следующий конструктор и свойства *файла Startup.cs* файл, расположенный в корневой папке проекта:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

Обратите внимание, на этом этапе *файла Startup.cs* файла не может быть скомпилирован, как все равно нужно добавить следующие `using` инструкции:

```csharp
using Microsoft.Extensions.Configuration;
```

Добавить *appsettings.json* файла в корневую папку проекта, с помощью шаблона соответствующий элемент:

![Добавить AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Перенос параметров конфигурации из файла web.config

Наш проект ASP.NET MVC включены в строку подключения базы данных, необходимых в *web.config*в `<connectionStrings>` элемент. В данном проекте ASP.NET Core мы будем хранить эти сведения в *appsettings.json* файла. Откройте *appsettings.json*и обратите внимание, что он уже включает в себя следующее:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


В выделенной строке, описанные выше, измените имя базы данных из **_CHANGE_ME** к имени базы данных.

## <a name="summary"></a>Сводка

ASP.NET Core заключает всю логику запуска приложения в одном файле, в котором необходимые службы и зависимости может быть определения и настройка. Он заменяет *web.config* файла с помощью функции гибкая настройка, который можно использовать в разнообразных форматах, например JSON, а также переменные среды.
