---
title: Создание веб-API с помощью ASP.NET Core и Visual Studio Code
author: rick-anderson
description: Создание веб-API на платформах macOS, Linux или Windows с помощью ASP.NET Core MVC и Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: b8e5c8b7d3dc04513997997d903295853dd1ff46
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348433"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Создание веб-API с помощью ASP.NET Core и Visual Studio Code

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)

В этом руководстве создается веб-API для управления элементами списка дел. При этом пользовательский интерфейс не создается.

Существуют три версии этого руководства:

* macOS, Linux, Windows: создание веб-API с помощью Visual Studio Code (этот учебник)
* macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)
* Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Необходимые компоненты

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

Советы по использованию VS Code см. в [справке по Visual Studio Code](#visual-studio-code-help).

## <a name="create-the-project"></a>Создание проекта

Из консоли выполните следующие команды:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

В Visual Studio Code (VS Code) откроется папка *TodoApi*. Выберите файл *Startup.cs*.

* В **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?) нажмите кнопку **Да** .
* В **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости) щелкните **Восстановить**.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code с предупреждением "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их? Больше не спрашивать, не сейчас, да](web-api-vsc/_static/vsc_restore.png)

Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее. В браузере перейдите на адрес http://localhost:5000/api/values. Выводится следующий результат.

```json
["value1","value2"]
```



## <a name="add-support-for-entity-framework-core"></a>Добавление поддержки для Entity Framework Core

:::moniker range=">= aspnetcore-2.1"

При создании проекта в ASP.NET Core 2.1 или более поздней версии в файл *TodoApi.csproj* добавляется ссылка на пакет [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App):

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

При создании проекта в ASP.NET Core 2.0 в файл *TodoApi.csproj* добавляется ссылка на пакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All):

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

Устанавливать поставщик базы данных [Entity Framework Core InMemory](/ef/core/providers/in-memory/) отдельно не требуется. Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.

## <a name="add-a-model-class"></a>Добавление класса модели

Модель — это объект, представляющий данные в приложении. В этом случае единственной моделью является задача.

Добавьте папку с именем *Models*. Классы моделей можно размещать в любом месте в проекте, но по соглашению используется папка *Models*.

Добавьте класс `TodoItem` с помощью следующего кода:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

После создания `TodoItem` база данных сформирует `Id`.

## <a name="create-the-database-context"></a>Создание контекста базы данных

*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных. Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.

Добавьте класс `TodoContext` в папку *Models*:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Добавление контроллера

В папке *Controllers* создайте класс с именем `TodoController`. Замените его содержимое следующим кодом:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Запуск приложения

В VS Code нажмите клавишу F5, чтобы запустить приложение. Перейдите к http://localhost:5000/api/todo (созданному контроллеру `Todo`).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Справка по Visual Studio Code

* [Начало работы](https://code.visualstudio.com/docs)
* [Отладка](https://code.visualstudio.com/docs/editor/debugging)
* [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Сочетания клавиш](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [Сочетания клавиш для macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Сочетания клавиш для Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Сочетания клавиш для Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
