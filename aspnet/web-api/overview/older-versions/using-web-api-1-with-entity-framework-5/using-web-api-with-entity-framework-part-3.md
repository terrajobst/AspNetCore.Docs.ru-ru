---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Часть 3: Создание контроллера администрирования | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7ad0ec27021514b447e569e479a9e9127e3f75fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838131"
---
<a name="part-3-creating-an-admin-controller"></a>Часть 3: Создание контроллера администрирования
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Добавление контроллера администрирования

В этом разделе мы добавим контроллер веб-API, который поддерживает CRUD (Создание, чтение, обновление и удаление) операций по продуктам. Контроллер будет использовать Entity Framework для взаимодействия с уровня базы данных. Только администраторы будут иметь возможность использовать этот контроллер. Клиенты получат доступ к продуктов через другой контроллер.

В обозревателе решений щелкните правой кнопкой мыши папку Controllers. Выберите **добавить** и затем **контроллера**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

В **Добавление контроллера** диалоговое окно, назовите контроллер `AdminController`. В разделе **шаблона**выберите &quot;контроллер API с действиями чтения и записи, с помощью Entity Framework&quot;. В разделе **класс модели**, выберите «Product (ProductStore.Models)». В разделе **контекст данных**, выберите "&lt;новый контекст данных&gt;«.

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Если **класс модели** раскрывающегося списка не отображаются все классы модели компилировать проект. Платформа Entity Framework использует отражение, поэтому оно должно скомпилированной сборки.


Выбрав "&lt;новый контекст данных&gt;" откроется **новый контекст данных** диалоговое окно. Имя контекста данных `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Нажмите кнопку **ОК** Отклонить **новый контекст данных** диалоговое окно. В **Добавление контроллера** диалоговое окно, нажмите кнопку **добавить**.

Вот, что был добавлен в проект:

- Класс с именем `OrdersContext` , наследуемый от класса **DbContext**. Этот класс обеспечивает связь между моделями POCO и базе данных.
- Контроллер Web API с именем `AdminController`. Этот контроллер поддерживает операции CRUD в `Product` экземпляров. Она использует `OrdersContext` класс для взаимодействия с Entity Framework.
- Новая строка подключения базы данных в файле Web.config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Откройте файл OrdersContext.cs. Обратите внимание на то, что конструктор указывает имя строки подключения базы данных. Это имя относится к строке подключения, который был добавлен в файл Web.config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Добавьте в класс `OrdersContext` следующие свойства:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Объект **DbSet** представляет набор сущностей, которые могут запрашиваться. Ниже приведен полный список для `OrdersContext` класса:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController` Класс определяет пять методов, которые реализуют основные функции CRUD. Каждый метод соответствует URI, который может вызвать клиент:

| Метод контроллера | Описание: | URI | Метод HTTP |
| --- | --- | --- | --- |
| GetProducts | Получает все продукты. | API/products | GET |
| GetProduct | Находит продукта по идентификатору. | API/products/*идентификатор* | GET |
| PutProduct | Обновление продукта. | API/products/*идентификатор* | PUT |
| PostProduct | Создает новый продукт. | API/products | ПОМЕСТИТЬ |
| DeleteProduct | Удаление продукта. | API/products/*идентификатор* | DELETE |

Каждый метод вызывает `OrdersContext` для запроса к базе данных. Вызовите методы, которые изменить коллекцию (PUT, POST и DELETE) `db.SaveChanges` для сохранения изменений в базу данных. Контроллеры создаются на HTTP-запрос и затем удален, поэтому необходимо сохранить изменения перед возвратом метода.

## <a name="add-a-database-initializer"></a>Добавить инициализатор базы данных

Платформа Entity Framework есть замечательная функция, которая позволяет вам заполнить базу данных при запуске и автоматически воссоздать базу данных при каждом изменении модели. Эта функция полезна во время разработки, так как у вас всегда есть некоторые тестовые данные, даже при изменении модели.

В обозревателе решений щелкните правой кнопкой мыши папку Models и создайте новый класс с именем `OrdersContextInitializer`. Вставьте следующую реализацию:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Путем наследования от **DropCreateDatabaseIfModelChanges** класс, мы указываем Entity Framework, чтобы удалить базу данных, каждый раз, когда мы изменим классы моделей. Когда Entity Framework создает (или повторно создает) базы данных, он вызывает **начальное значение** метод для заполнения таблиц. Мы используем **начальное значение** метод, чтобы добавить некоторые продукты пример, а также пример заказа.

Эта функция очень удобно для тестирования, но не используйте **DropCreateDatabaseIfModelChanges** класса в рабочей среде, так как вы можете потерять данные при изменении модели.

Затем откройте Global.asax и добавьте следующий код, чтобы **приложения\_запустить** метод:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Отправьте запрос к контроллеру

На этом этапе мы еще не написали код клиента, но вы можете вызвать web API с помощью веб-браузер или отладки HTTP, такие как средство [Fiddler](http://www.fiddler2.com/fiddler2/). В Visual Studio нажмите клавишу F5, чтобы начать отладку. Веб-браузере будет открыта `http://localhost:*portnum*/`, где *portnum* является некоторое число портов.

Отправить запрос HTTP для "`http://localhost:*portnum*/api/admin`. Первый запрос может быть медленное, поскольку Entify Framework необходимо создать и заполнить базу данных. Ответ должен нечто похожее на следующее:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-2.md)
> [Вперед](using-web-api-with-entity-framework-part-4.md)
