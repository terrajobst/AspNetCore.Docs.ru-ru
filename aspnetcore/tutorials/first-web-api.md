---
title: "Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows"
author: rick-anderson
description: "Сборка веб-API с помощью MVC ASP.NET Core и Visual Studio для Windows"
keywords: "ASP.NET Core,вебAPI,веб-API,REST,HTTP,служба,служба HTTP"
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 4aab61c7ee4498b33a4ea8bbec6033ce9828e2af
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)

В этом учебнике вы создадите веб-API для управления списком дел. Вы не будете создавать пользовательский интерфейс.

Существует 3 версии этого учебника.

* Windows: создание веб-API с помощью Visual Studio для Windows (этот учебник)
* macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE[install 2.0](../includes/install2.0.md)]

См. [этот PDF-файл](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) для версии ASP.NET Core 1.1.

## <a name="create-the-project"></a>Создание проекта

В Visual Studio откройте меню **Файл** > **Создать** > **Проект**.

Выберите шаблон проекта **Веб-приложение ASP.NET Core (.NET Core)**. Задайте для проекта имя `TodoApi` и нажмите кнопку **ОК**.

![Диалоговое окно создания проекта](first-web-api/_static/new-project.png)

В диалоговом окне **Новое веб-приложение ASP.NET Core — TodoApi** выберите шаблон **Веб-API**. Нажмите кнопку **ОК**. **Не** выбирайте **Включение поддержки Docker**.

![Диалоговое окно создания веб-приложения ASP.NET с помощью шаблона проекта веб-API, выбранное из базовых шаблонов ASP.NET](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение. Visual Studio запустит браузер и перейдет к `http://localhost:port/api/values`, где *порт* — это номер порта, выбранный случайным образом. В Chrome, Edge и Firefox отобразится следующая информация.

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a>Добавление класса модели

Модель — это объект, представляющий данные в приложении. В этом случае единственной моделью является задача.

Добавьте папку с именем "Models". В обозревателе решений щелкните проект правой кнопкой мыши. Выберите **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

Примечание. Классы моделей можно размещать в любом месте проекта, но обычно используется папка *Models*.

Добавьте класс `TodoItem`. Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**. Присвойте классу имя `TodoItem` и выберите **Добавить**.

Замените сгенерированный код следующим кодом:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

После создания `TodoItem` база данных формирует `Id`.

### <a name="create-the-database-context"></a>Создание контекста базы данных

*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных. Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.

Добавьте класс `TodoContext`. Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**. Присвойте классу имя `TodoContext` и выберите **Добавить**.

Замените сгенерированный код следующим кодом:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Добавление контроллера

В обозревателе решений щелкните папку *Контроллеры* правой кнопкой мыши. Выберите **Добавить** > **Новый объект**. В диалоговом окне **Добавление нового элемента** выберите шаблон **Класс контроллера веб-API**. Присвойте классу имя `TodoController`.

![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

Замените сгенерированный код следующим кодом:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение. Visual Studio запустит браузер и перейдет к `http://localhost:port/api/values`, где *порт* — это номер порта, выбранный случайным образом. Если вы используете Chrome, Edge или Firefox, отобразятся данные. Если вы используете IE, он предложит вам открыть или сохранить файл *values.json*. Перейдите к только что созданному контроллеру `Todo` по адресу `http://localhost:port/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

