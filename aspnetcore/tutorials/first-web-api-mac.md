---
title: Создание веб-API с помощью ASP.NET Core и Visual Studio для Mac
author: rick-anderson
description: Создание веб-API с помощью MVC ASP.NET Core и Visual Studio для Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 699fbbf54abf1dc5c4156c559761110cdb375558
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Создание веб-API с помощью ASP.NET Core и Visual Studio для Mac

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)

В этом руководстве создается веб-API для управления элементами списка дел. При этом пользовательский интерфейс не создается.

Существуют три версии этого руководства:

* macOS: создание веб-API с помощью Visual Studio для Mac (этот учебник)
* Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Пример использования постоянной базы данных см. в статье [Введение в MVC ASP.NET Core в macOS](xref:tutorials/first-mvc-app-xplat/index).

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Создание проекта

В Visual Studio выберите **Файл** > **Новое решение**.

![Новое решение macOS](first-web-api-mac/_static/sln.png)

Выберите **Приложение .NET Core** > **Веб-API ASP.NET Core** > **Далее**.

![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)

Введите *TodoApi* в поле **Имя проекта** и нажмите **Создать**.

![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio откройте меню **Выполнить** > **Запуск без отладки**, чтобы запустить приложение. Visual Studio откроет браузер и перейдет к `http://localhost:5000`. Появится ошибка HTTP 404 (Не найдено). Измените URL-адрес на `http://localhost:<port>/api/values`. Отображаются данные `ValuesController`:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Добавление поддержки для Entity Framework Core

Установите поставщик базы данных [Entity Framework Core InMemory](/ef/core/providers/in-memory/). Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.

* В меню **Проект** выберите пункт **Добавить пакеты NuGet**.

  * Также можно щелкнуть **Зависимости** правой кнопкой мыши и выбрать команду **Добавить пакеты**.

* Введите `EntityFrameworkCore.InMemory` в поле поиска.
* Выберите `Microsoft.EntityFrameworkCore.InMemory`, а затем **Добавить пакет**.

### <a name="add-a-model-class"></a>Добавление класса модели

Модель — это объект, представляющий данные в приложении. В этом случае единственной моделью является задача.

В обозревателе решений щелкните проект правой кнопкой мыши. Выберите **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

![Новая папка](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Классы моделей можно размещать в любом месте в проекте, но по соглашению используется папка *Models*.

Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**. Назовите класс *TodoItem* и нажмите **Создать**.

Замените сгенерированный код на следующий:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

После создания `TodoItem` база данных формирует `Id`.

### <a name="create-the-database-context"></a>Создание контекста базы данных

*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных. Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.

Добавьте класс `TodoContext` в папку *Models*.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Добавление контроллера

В обозревателе решений добавьте в папку *Controllers* класс `TodoController`.

Замените сгенерированный код следующим кодом:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio откройте меню **Выполнить** > **Запуск без отладки**, чтобы запустить приложение. Visual Studio запустит браузер и перейдет к `http://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом. Появится ошибка HTTP 404 (Не найдено). Измените URL-адрес на `http://localhost:<port>/api/values`. Отображаются данные `ValuesController`:

```json
["value1","value2"]
```

Перейдите к контроллеру `Todo` по адресу `http://localhost:<port>/api/todo`. Возвращается следующий файл JSON:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Реализация других операций CRUD

Добавим к контроллеру методы `Create`, `Update` и `Delete`. Эти методы являются вариациями темы, поэтому мы просто покажем код и выделим основные различия. После добавления или изменения кода выполните сборку проекта.

### <a name="create"></a>Создать

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Приведенный выше метод отвечает на запрос HTTP POST, как указано атрибутом [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). Атрибут [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Приведенный выше метод отвечает на запрос HTTP POST, как указано атрибутом [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). MVC получает значение элемента задачи из текста HTTP-запроса.
::: moniker-end

Метод `CreatedAtRoute` возвращает ответ 201. Это стандартный ответ для метода HTTP POST, который создает ресурс на сервере. `CreatedAtRoute` также добавляет в ответ заголовок расположения. Заголовок расположения указывает URI вновь созданной задачи. См. раздел [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Отправка запроса Create с помощью Postman

* Запустите приложение (**Выполнить** > **Запуск с отладкой**).
* Откройте Postman.

![Консоль Postman](first-web-api/_static/pmc.png)

* Измените номера порта в localhost URL-адреса.
* Укажите метод HTTP *POST*.
* Откройте вкладку **Текст**.
* Установите переключатель **без обработки**.
* Задайте тип *JSON (приложение/json)*.
* Введите текст запроса с элементом задачи наподобие следующего JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Нажмите кнопку **Отправить**.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Если ответ не отображается после нажатия кнопки **Отправить**, отключите параметр **Проверка сертификации SSL**. Он находится в разделе **Файл** > **Параметры**. Нажмите кнопку **Отправить** еще раз после отключения параметра.
::: moniker-end

Откройте вкладку **Заголовки** на панели **Ответ** и скопируйте значение заголовка **Расположение**:

![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmc2.png)

URI заголовка расположения позволяет получить доступ к созданному ресурсу. Метод `Create` возвращает [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). Первый параметр, передаваемый `CreatedAtRoute`, представляет именованный маршрут для создания URL-адреса. Помните, что метод `GetById` создал именованный маршрут `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>Обновление

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT. Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только сведения о ней. Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Удаление

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
