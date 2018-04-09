---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Часть 3: Создание контроллера Admin | Документы Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a>Часть 3: Создание контроллера администратора
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Добавить контроллер администратора

В этом разделе будет добавлен контроллер веб-API, который поддерживает CRUD (Создание, чтение, обновление и удаление) операций по продуктам. Контроллер будет использовать Entity Framework для взаимодействия с уровня базы данных. Только администраторы будут иметь возможность использовать данный контроллер. Клиенты будут получать доступ продукты по другому контроллеру.

В обозревателе решений щелкните правой кнопкой мыши папку Controllers. Выберите **добавить** и затем **контроллера**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

В **добавления контроллера** диалога, имя контроллера `AdminController`. В разделе **шаблона**выберите &quot;контроллер API с действиями чтения и записи, использующий Entity Framework&quot;. В разделе **класс модели**, выберите «Product (ProductStore.Models)». В разделе **контекст данных**, выберите «&lt;новый контекст данных&gt;».

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Если **класс модели** раскрывающийся список не содержит всех классов модели, убедитесь, что при компиляции проекта. Entity Framework использует отражение, поэтому он должен скомпилированную сборку.


При выборе «&lt;новый контекст данных&gt;» будет открыт **новый контекст данных** диалогового окна. Имя контекста данных `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Нажмите кнопку **ОК** закрыть **новый контекст данных** диалогового окна. В **добавления контроллера** диалоговое окно, нажмите кнопку **добавить**.

Вот, что был добавлен в проект.

- Класс с именем `OrdersContext` , производный от **DbContext**. Этот класс обеспечивает связь между моделями POCO и базы данных.
- Контроллер веб-API с именем `AdminController`. Этот контроллер поддерживает операций CRUD в `Product` экземпляров. Она использует `OrdersContext` класс для взаимодействия с Entity Framework.
- Новая строка подключения базы данных в файле Web.config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Откройте файл OrdersContext.cs. Обратите внимание, что конструктор указывает имя строки подключения базы данных. Это имя относится к строке соединения, который был добавлен в файл Web.config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Добавьте в класс `OrdersContext` следующие свойства:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Объект **DbSet** представляет набор сущностей, которые могут запрашиваться. Ниже приведен полный листинг для `OrdersContext` класса:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController` Класс определяет пять методов, которые реализуют базовые функциональные возможности CRUD. Каждый метод соответствует URI, который может вызывать клиент:

| Метод контроллера | Описание | URI | Метод HTTP |
| --- | --- | --- | --- |
| GetProducts | Получает все продукты. | API и продуктов | GET |
| GetProduct | Выполняет поиск продуктов по идентификатору. | api/products/*id* | GET |
| PutProduct | Обновления продукта. | api/products/*id* | PUT |
| PostProduct | Создает новый продукт. | API и продуктов | ПОМЕСТИТЬ |
| DeleteProduct | Удаление продукта. | api/products/*id* | DELETE |

Вызывает каждый метод `OrdersContext` запросов к базе данных. Вызовите методы, позволяющие изменять коллекцию (PUT, POST и DELETE) `db.SaveChanges` для сохранения изменений в базу данных. Контроллеры создаются на HTTP-запрос и затем ликвидирован, поэтому это необходимо для сохранения изменений перед возвратом метода.

## <a name="add-a-database-initializer"></a>Добавление инициализатора базы данных

Платформа Entity Framework должна удобная функция, которая позволяет заполнить базу данных во время запуска и автоматически повторно создать базу данных при каждом изменении модели. Эта возможность полезна во время разработки, если у вас всегда некоторые тестовые данные, даже при изменении модели.

В обозревателе решений щелкните правой кнопкой мыши папку модели и создать новый класс с именем `OrdersContextInitializer`. Вставьте следующие реализации:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Путем наследования от **DropCreateDatabaseIfModelChanges** класса, мы указываем удалить базу данных, каждый раз, когда мы изменить классы модели Entity Framework. Если Entity Framework создает (или повторно создает) базы данных, он вызывает **начальное значение** метод для заполнения таблиц. Мы используем **начальное значение** метод, чтобы добавить некоторые из примера, а также пример заказа.

Эта функция отлично подходит для тестирования, но не по **DropCreateDatabaseIfModelChanges** класса в рабочей среде, так как вы можете потерять данные при изменении модели.

Затем откройте Global.asax и добавьте следующий код в **приложения\_запустить** метод:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Отправка запроса на контроллер

На этом этапе мы еще не записаны кода клиента, но можно вызвать веб-сайте, такие как средства API с помощью веб-браузера или отладка HTTP [Fiddler](http://www.fiddler2.com/fiddler2/). В Visual Studio нажмите клавишу F5 для запуска отладки. Веб-браузере будет открыта `http://localhost:*portnum*/`, где *portnum* — некоторые номер порта.

Отправить запрос HTTP»`http://localhost:*portnum*/api/admin`. Первый запрос может быть медленное, потому что Entify Framework необходимо создать и инициализировать базу данных. Ответ должен нечто похожее на следующее:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-2.md)
> [Вперед](using-web-api-with-entity-framework-part-4.md)
