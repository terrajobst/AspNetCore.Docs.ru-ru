---
title: Создание веб-API с помощью ASP.NET Core и Visual Studio
author: rick-anderson
description: Создание веб-API с помощью MVC ASP.NET Core и Visual Studio в Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 2694388324cdbd246aad6c88d8439171704dfe89
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722520"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a>Создание веб-API с помощью ASP.NET Core и Visual Studio

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)

В этом руководстве вы создали веб-API для управления элементами списка дел. Пользовательский интерфейс не создан.

Существуют три версии этого руководства:

* Создание веб-API с помощью Visual Studio в Windows (это руководство)
* macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Создание проекта

Выполните следующие действия в Visual Studio:

* В меню **Файл** выберите пункт **Создать** > **Проект**.
* Выберите шаблон **Веб-приложение ASP.NET Core**. Назовите проект *TodoApi* и нажмите **OK**.
* В диалоговом окне **Новое веб-приложение ASP.NET Core — TodoApi** выберите версию ASP.NET Core. Выберите шаблон **API** и нажмите кнопку **ОК**. **Не** выбирайте **Включение поддержки Docker**.

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение. Visual Studio запустит браузер и перейдет к `http://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом. В Chrome, Microsoft Edge и Firefox отобразится следующая информация.

```json
["value1","value2"]
```

При использовании Internet Explorer будет предложено сохранить файл *values.json*.

### <a name="add-a-model-class"></a>Добавление класса модели

Модель — это объект, представляющий данные в приложении. В этом случае единственной моделью является задача.

В обозревателе решений щелкните проект правой кнопкой мыши. Выберите **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

> [!NOTE]
> Классы модели могут переходить в любое место в проекте. Папка *Модели* используется по соглашению о классах модели.

В обозревателе решений щелкните папку *Models* правой кнопкой мыши и выберите команду **Добавить** > **Класс**. Назовите класс *TodoItem* и нажмите **Добавить**.

Обновите класс `TodoItem` с помощью следующего кода:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

После создания `TodoItem` база данных формирует `Id`.

### <a name="create-the-database-context"></a>Создание контекста базы данных

*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных. Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.

В обозревателе решений щелкните папку *Models* правой кнопкой мыши и выберите команду **Добавить** > **Класс**. Назовите класс *TodoContext* и нажмите **Добавить**.

Замените класс на следующий код.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Добавление контроллера

В обозревателе решений щелкните папку *Контроллеры* правой кнопкой мыши. Выберите **Добавить** > **Новый объект**. В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**. Назовите класс *TodoController* и нажмите **Добавить**.

![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

Замените класс на следующий код.

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение. Visual Studio запустит браузер и перейдет к `http://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом. Перейдите к контроллеру `Todo` по адресу `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
