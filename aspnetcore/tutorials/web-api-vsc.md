---
title: "Создание веб-API с помощью ASP.NET Core и VS Code"
description: "Создание веб-API на платформах macOS, Linux или Windows с помощью ASP.NET Core MVC и Visual Studio Code"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
manager: wpickett
uid: tutorials/web-api-vsc
ms.openlocfilehash: 9fec8904cf05fc486160c0641731c6336fe2766a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>Создание веб-API с помощью ASP.NET Core MVC и Visual Studio Code на платформах macOS, Windows и Linux

Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Майк Уоссон (Mike Wasson)](https://github.com/mikewasson)

В этом учебнике вы создадите веб-API для управления списком дел. Вы не будете создавать пользовательский интерфейс.

Существует 3 версии этого учебника:

* macOS, Linux, Windows: создание веб-API с помощью Visual Studio Code (этот учебник)
* macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)
* Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>Настройка среды разработки

Скачайте и установите следующие компоненты:
- [Пакет SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии.
- [Visual Studio Code.](https://code.visualstudio.com)
- [Расширение C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio Code

## <a name="create-the-project"></a>Создание проекта

Из консоли выполните следующие команды:

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

Откройте папку *TodoApi* в Visual Studio Code (VS Code) и выберите файл *Startup.cs*.

- В **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?) нажмите кнопку **Да** .
- В **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости) щелкните **Восстановить**.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code с предупреждением "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их? Больше не спрашивать, не сейчас, да](web-api-vsc/_static/vsc_restore.png)

Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее. В браузере перейдите по адресу http://localhost:5000/api/values. Отобразится следующее:

`["value1","value2"]`

Советы по использованию VS Code см. в [справке по Visual Studio Code](#visual-studio-code-help).

## <a name="add-support-for-entity-framework-core"></a>Добавление поддержки для Entity Framework Core

Создание проекта в .NET Core 2.0 добавляет поставщик "Microsoft.AspNetCore.All" в файл *TodoApi.csproj*. Устанавливать поставщик базы данных [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) отдельно не требуется. Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a>Добавление класса модели

Модель — это объект, представляющий данные в приложении. В этом случае единственной моделью является задача.

Добавьте папку с именем *Models*. Классы моделей можно размещать в любом месте в проекте, но по соглашению используется папка *Models*.

Добавьте класс `TodoItem` с помощью следующего кода:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

После создания `TodoItem` база данных сформирует `Id`.

## <a name="create-the-database-context"></a>Создание контекста базы данных

*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных. Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.

Добавьте класс `TodoContext` в папку *Models*:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Добавление контроллера

В папке *Controllers* создайте класс с именем `TodoController`. Добавьте следующий код:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Запуск приложения

В VS Code нажмите клавишу F5, чтобы запустить приложение. Перейдите по адресу http://localhost:5000/api/todo (только что созданный контроллер `Todo`).

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Справка по Visual Studio Code

- [Начало работы](https://code.visualstudio.com/docs)
- [Отладка](https://code.visualstudio.com/docs/editor/debugging)
- [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Сочетания клавиш](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Сочетания клавиш для Mac](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Сочетания клавиш для Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Сочетания клавиш для Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


