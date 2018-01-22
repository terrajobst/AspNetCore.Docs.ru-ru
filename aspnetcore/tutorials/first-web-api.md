---
title: "Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows"
author: rick-anderson
description: "Сборка веб-API с помощью MVC ASP.NET Core и Visual Studio для Windows"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 234dbf73f9751ad4f995d6e7471d94f060fb15bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)

В этом руководстве вы создали веб-API для управления элементами списка дел. Пользовательский интерфейс (UI) не создан.

Существует 3 версии этого учебника:

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

В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение. Visual Studio запустит браузер и перейдет к `http://localhost:port/api/values`, где *порт* — это номер порта, выбранный случайным образом. В Chrome, Microsoft Edge и Firefox отобразится следующая информация.

```
["value1","value2"]
```

### <a name="add-a-model-class"></a>Добавление класса модели

Модель — это объект, представляющий данные в приложении. В этом случае единственной моделью является задача.

Добавьте папку с именем "Models". В обозревателе решений щелкните проект правой кнопкой мыши. Выберите **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

Примечание. Классы модели могут переходить в любое место в проекте. Папка *Модели* используется по соглашению о классах модели.

Добавьте класс `TodoItem`. Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**. Присвойте классу имя `TodoItem` и выберите **Добавить**.

Обновите класс `TodoItem` с помощью следующего кода:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

После создания `TodoItem` база данных формирует `Id`.

### <a name="create-the-database-context"></a>Создание контекста базы данных

*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных. Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.

Добавьте класс `TodoContext`. Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**. Присвойте классу имя `TodoContext` и выберите **Добавить**.

Замените класс на следующий код.

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Добавление контроллера

В обозревателе решений щелкните папку *Контроллеры* правой кнопкой мыши. Выберите **Добавить** > **Новый объект**. В диалоговом окне **Add New Item** (Добавление нового элемента) выберите шаблон **Класс контроллера веб-API**. Присвойте классу имя `TodoController`.

![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

Замените класс на следующий код.

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение. Visual Studio запустит браузер и перейдет к `http://localhost:port/api/values`, где *порт* — это номер порта, выбранный случайным образом. Перейдите к контроллеру `Todo` по адресу `http://localhost:port/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

