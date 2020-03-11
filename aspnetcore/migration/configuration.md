---
title: Перенос конфигурации в ASP.NET Core
author: ardalis
description: Узнайте, как перенести конфигурацию из проекта ASP.NET MVC в проект ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 2c50ea768a42aa38d14c55d8c403fea4176b3650
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651886"
---
# <a name="migrate-configuration-to-aspnet-core"></a>Перенос конфигурации в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Скотт Эдди](https://scottaddie.com) (Scott Addie)

В предыдущей статье мы начали [Перенос проекта ASP.NET MVC на ASP.NET Core MVC](xref:migration/mvc). В этой статье мы выполним миграцию конфигурации.

[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/configuration/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Настройка конфигурации

ASP.NET Core больше не использует файлы *Global. asax* и *Web. config* , которые использовались в предыдущих версиях ASP.NET. В более ранних версиях ASP.NET логика запуска приложения была помещена в метод `Application_StartUp` в *Global. asax*. Позже в ASP.NET MVC файл *Startup.CS* был добавлен в корень проекта. и был вызван при запуске приложения. ASP.NET Core полностью приняла этот подход, поместив всю логику запуска в файл *Startup.CS* .

Файл *Web. config* также был заменен в ASP.NET Core. Теперь конфигурацию можно настроить в рамках процедуры запуска приложения, описанной в *Startup.CS*. Конфигурация по-прежнему может использовать XML-файлы, но обычно ASP.NET Core проекты помещают значения конфигурации в файл в формате JSON, например *appSettings. JSON*. Система настройки ASP.NET Core также может легко получить доступ к переменным среды, что может обеспечить [более безопасное и надежное расположение](xref:security/app-secrets) для значений, зависящих от среды. Это особенно справедливо для таких секретов, как строки подключения и ключи API, которые не должны быть возвращены в систему управления версиями. Дополнительные сведения о настройке в ASP.NET Core см. в разделе [Конфигурация](xref:fundamentals/configuration/index) .

В этой статье мы начинаем с частично перенесенного ASP.NET Core проекта из [предыдущей статьи](xref:migration/mvc). Чтобы настроить конфигурацию, добавьте следующий конструктор и свойство в файл *Startup.CS* , расположенный в корне проекта:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

Обратите внимание на то, что на этом этапе файл *Startup.CS* не будет компилироваться, так как по-прежнему нужно добавить следующую инструкцию `using`:

```csharp
using Microsoft.Extensions.Configuration;
```

Добавьте файл *appSettings. JSON* в корень проекта, используя соответствующий шаблон элемента:

![Добавление формата AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Перенос параметров конфигурации из файла Web. config

Наш проект ASP.NET MVC включал в себя необходимую строку подключения к базе данных в *файле Web. config*в элементе `<connectionStrings>`. В нашем проекте ASP.NET Core эти сведения будут храниться в файле *appSettings. JSON* . Откройте *appSettings. JSON*и обратите внимание, что он уже включает следующее:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

В выделенной выше строке измените имя базы данных с **_CHANGE_ME** на имя базы данных.

## <a name="summary"></a>Сводка

ASP.NET Core помещает всю логику запуска приложения в один файл, в котором можно определить и настроить необходимые службы и зависимости. Он заменяет файл *Web. config* гибкой функцией настройки, которая может использовать различные форматы файлов, такие как JSON, а также переменные среды.
