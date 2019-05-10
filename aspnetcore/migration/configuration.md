---
title: Перенос конфигурации в ASP.NET Core
author: ardalis
description: Узнайте, как выполнять миграцию конфигурации из проекта ASP.NET MVC в проект ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: c379f1f64dc5ab8aeb48055124e86e4e60d93785
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894931"
---
# <a name="migrate-configuration-to-aspnet-core"></a>Перенос конфигурации в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Скотт Эдди](https://scottaddie.com) (Scott Addie)

В предыдущей статье, мы начали [миграции проекта ASP.NET MVC в ASP.NET Core MVC](xref:migration/mvc). В этой статье мы переносим конфигурации.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/configuration/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Настройка установки

ASP.NET Core больше не использует *Global.asax* и *web.config* файлы, которые используются в предыдущих версиях ASP.NET. В более ранних версиях ASP.NET, логика запуска приложения был помещен в `Application_StartUp` метода в *Global.asax*. Далее, в ASP.NET MVC, *Startup.cs* файл был включен в корне проекта; и он был вызван при запуске приложения. ASP.NET Core была внедрена такой подход полностью, поместив вся логика запуска в *Startup.cs* файла.

*Web.config* файла также был заменен в ASP.NET Core. Самой конфигурации теперь можно настроить, как часть процедуры запуска приложения, описанные в *Startup.cs*. Конфигурации по-прежнему могут использовать XML-файлы, но обычно проекты ASP.NET Core и помещает значения конфигурации в файл в формате JSON, такие как *appsettings.json*. Система конфигурации ASP.NET Core также упрощает доступ к переменные среды, которые может предоставить [более безопасные и надежные расположения](xref:security/app-secrets) для конкретных значений. Это особенно верно для секретные данные, например строки подключения и ключи API, которые не должны быть проверены в систему управления версиями. См. в разделе [конфигурации](xref:fundamentals/configuration/index) Дополнительные сведения о конфигурации в ASP.NET Core.

В этой статье мы начинаем с частично переноса проекта ASP.NET Core из [предыдущей статье](xref:migration/mvc). Чтобы настроить конфигурацию, добавьте следующий конструктор и свойства *Startup.cs* файл, расположенный в корневой папке проекта:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

Обратите внимание, на этом этапе *Startup.cs* файл не будет компилироваться, так как мы еще нужно добавить следующие `using` инструкции:

```csharp
using Microsoft.Extensions.Configuration;
```

Добавить *appsettings.json* файл в корень проекта, с помощью шаблона соответствующего элемента:

![Добавьте AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Перенос параметров конфигурации из файла web.config

Наш проект ASP.NET MVC включены строку подключения базы данных, необходимых в *web.config*в `<connectionStrings>` элемент. В нашем проекте ASP.NET Core, мы хотим сохранить эту информацию в *appsettings.json* файла. Откройте *appsettings.json*и обратите внимание, что он уже содержит следующее:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

В выделенной строке, описанному выше, измените имя базы данных из **_CHANGE_ME** имя базы данных.

## <a name="summary"></a>Сводка

ASP.NET Core помещает всю логику запуска для приложения в одном файле, в котором необходимые службы и зависимости могут быть определены и настроен. Он заменяет *web.config* файл с помощью функции гибкой конфигурации, который можно использовать разные форматы файлов, таких как JSON, а также переменные среды.
