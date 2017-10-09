---
title: "Создание веб-API с помощью ASP.NET Core и Visual Studio для Mac"
description: "Создание веб-API с помощью MVC ASP.NET Core и Visual Studio для Mac"
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, веб-API, REST, mac, macOS, HTTP, служба, служба HTTP"
manager: wpickett
ms.openlocfilehash: 6bbd5e332e395928d8f79888ecf190f7f59a4bbc
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Создание веб-API с помощью MVC ASP.NET Core и Visual Studio для Mac

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson) (Mike Wasson)

В этом учебнике вы создадите веб-API для управления списком дел. Вы не будете создавать пользовательский интерфейс.

Существует 3 версии этого учебника.

* macOS: создание веб-API с помощью Visual Studio для Mac (этот учебник)
* Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* Пример использования постоянной базы данных см. в статье [Введение в MVC ASP.NET Core на Mac или Linux](xref:tutorials/first-mvc-app-xplat/index).

## <a name="prerequisites"></a>Предварительные требования

Установите следующие компоненты:

- [пакет SDK для .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии;
- [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a>Создание проекта

В Visual Studio выберите **Файл > Новое решение**.

![Новое решение macOS](first-web-api-mac/_static/sln.png)

Выберите **Приложение .NET Core > Веб-API ASP.NET Core > Далее**.

![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)

Введите **TodoApi** в поле **Имя проекта** и выберите команду "Создать".

![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio откройте меню **Выполнить > Запуск без отладки**, чтобы запустить приложение. Visual Studio откроет браузер и перейдет к `http://localhost:5000`. Появится ошибка HTTP 404 (Не найдено).  Измените URL-адрес на `http://localhost:port/api/values`. Отобразятся данные `ValuesController`

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Добавление поддержки для Entity Framework Core

Установите поставщик базы данных [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/). Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.

* В меню **Проект** выберите пункт **Добавить пакеты NuGet**. 

  *  Также можно щелкнуть **Зависимости** правой кнопкой мыши и выбрать команду **Добавить пакеты**.

* Введите `EntityFrameworkCore.InMemory` в поле поиска.
* Выберите `Microsoft.EntityFrameworkCore.InMemory`, а затем **Добавить пакет**.

### <a name="add-a-model-class"></a>Добавление класса модели

Модель — это объект, представляющий данные в приложении. В этом случае единственной моделью является задача.

Добавьте папку с именем *Models*. В обозревателе решений щелкните проект правой кнопкой мыши. Выберите **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

![Новая папка](first-web-api-mac/_static/folder.png)

Примечание. Классы моделей можно размещать в любом месте проекта, но обычно используется папка *Models*.

Добавьте класс `TodoItem`. Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить > Новый файл > Общие > Пустой класс**. Присвойте классу имя `TodoItem` и выберите команду **Создать**.

Замените сгенерированный код на следующий:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

После создания `TodoItem` база данных формирует `Id`.

### <a name="create-the-database-context"></a>Создание контекста базы данных

*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных. Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.

Добавьте класс `TodoContext` в папку *Models*.

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Добавление контроллера

В обозревателе решений добавьте в папку *Controllers* класс `TodoController`.

Замените сгенерированный код на следующий (и добавьте закрывающие скобки).

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio откройте меню **Выполнить > Запуск без отладки**, чтобы запустить приложение. Visual Studio запустит браузер и перейдет к `http://localhost:port`, где *порт* — это номер порта, выбранный случайным образом. Появится ошибка HTTP 404 (Не найдено).  Измените URL-адрес на `http://localhost:port/api/values`. Отобразятся данные `ValuesController`

```
["value1","value2"]
```

Перейдите к контроллеру `Todo` по адресу `http://localhost:port/api/todo`.

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Реализация других операций CRUD

Добавим к контроллеру методы `Create`, `Update` и `Delete`. Это вариации темы, поэтому мы просто покажем код и выделим основные различия. После добавления или изменения кода выполните сборку проекта.

### <a name="create"></a>Создать

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Это метод HTTP POST, обозначенный атрибутом [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute). Атрибут [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.

Метод `CreatedAtRoute` возвращает ответ 201, который представляет собой стандартный ответ для метода HTTP POST, создающий новый ресурс на сервере. `CreatedAtRoute` также добавляет в ответ заголовок расположения. Заголовок расположения указывает URI вновь созданной задачи. См. раздел [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Отправка запроса Create с помощью коллекции Postman

* Запустите приложение (**Выполнить > Запуск с отладкой**).
* Запустите Postman.

![Консоль Postman](first-web-api/_static/pmc.png)

* Укажите HTTP-метод `POST`
* Установите переключатель **Текст запроса**
* Установите переключатель **без обработки**
* В качестве типа выберите JSON
* В редакторе пар ключей и значений введите элемент Todo, например

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Нажмите кнопку **Отправить**

* Откройте вкладку "Заголовки" в нижней области и скопируйте заголовок **расположения**:

![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmget.png)

URI заголовка расположения позволяет получить доступ к только что созданному ресурсу. Снова вызовите метод `GetById` для создания маршрута с именем `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a>Обновление

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT. Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только сведения о ней. Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Удаление

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a>Следующие шаги

* [Маршрутизация к действиям контроллера](xref:mvc/controllers/routing)
* Сведения о развертывании API см. в статье [Публикация и развертывание](../publishing/index.md).
* [Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))
* [Postman](https://www.getpostman.com/)
* [Fiddler](https://www.telerik.com/download/fiddler)
