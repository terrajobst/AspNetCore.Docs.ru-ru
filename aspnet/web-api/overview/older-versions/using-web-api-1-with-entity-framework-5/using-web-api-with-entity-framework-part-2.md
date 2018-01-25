---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "Часть 2: Создание модели домена | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: a573b47d27767dc78d557cd2b6c73714eb9e94f4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="part-2-creating-the-domain-models"></a>Часть 2: Создание модели домена
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Добавление моделей

Существует три способа подхода Entity Framework:

- Сначала база данных: запустите с базой данных и Entity Framework создает код.
- «Сначала модель»: начать работу с моделью visual и Entity Framework создает базы данных и кода.
- Сначала код: Начните с кода и Entity Framework создает базу данных.

Метод сначала код используются, поэтому начнем с определения наши объекты домена как POCO (plain old CLR объекты). С данным подходом сначала код объектов домена не требуется любой дополнительный код для поддержки базы данных слоя, например транзакции или сохраняемость. (В частности, они не обязательно должны наследовать от [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) класса.) Заметок к данным по-прежнему можно использовать для управления как Entity Framework создает схему базы данных.

Поскольку POCO без выполнения все дополнительные свойства, описывающие [состояние базы данных](https://msdn.microsoft.com/library/system.data.entitystate.aspx), они могут легко быть сериализован в JSON или XML. Тем не менее, это означает, что следует всегда предоставлять модели Entity Framework непосредственно клиентам, как будет показано далее в этом учебнике.

Мы создадим POCO следующие:

- Продукт
- Номер
- OrderDetail

Для создания каждого класса, щелкните правой кнопкой мыши папку модели в обозревателе решений. В контекстном меню выберите **добавить** , а затем выберите **класса.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Добавить `Product` следующая реализация класса:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

По соглашению Entity Framework использует `Id` свойство в качестве первичного ключа и сопоставляет столбец идентификаторов в таблице базы данных. При создании нового `Product` экземпляра, не задать значение для `Id`, так как база данных формирует значение.

**ScaffoldColumn** атрибут сообщает ASP.NET MVC, чтобы пропустить `Id` свойства при создании формы редактора. **Необходимые** атрибут используется для проверки модели. Указывает, что `Name` свойство должно быть непустой строкой.

Добавить `Order` класса:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Добавить `OrderDetail` класса:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Отношения внешнего ключа

Заказ содержит многие подробности заказа и подробности каждого заказа ссылается на единицу продукта. Для представления таких отношений `OrderDetail` класс определяет свойства с именем `OrderId` и `ProductId`. Платформа Entity Framework будет построена, эти свойства представляют внешние ключи что будет добавить в базу данных ограничения foreign key.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order` И `OrderDetail` классы также включают свойства «навигация», которые содержат ссылки на связанные объекты. Если заказ, можно перейти к продуктов в порядке, следующие свойства навигации.

Теперь скомпилируйте проект. Платформа Entity Framework использует отражение для обнаружения свойств моделей, поэтому для него требуется скомпилированную сборку создать схему базы данных.

## <a name="configure-the-media-type-formatters"></a>Настройка модулей форматирования типа мультимедиа

Объект [форматирования типа мультимедиа](../../formats-and-model-binding/media-formatters.md) — это объект, который выполняет сериализацию данных, когда веб-API записывает текст ответа HTTP. Встроенные модули форматирования поддерживает выходных данных JSON и XML. По умолчанию оба этих модулей форматирования сериализации всех объектов по значению.

Сериализация по значению создает проблему, если граф объектов содержит циклические ссылки. Это только в случае с `Order` и `OrderDetail` классы, так как каждый содержит ссылку на другой. Модуль форматирования выполните ссылок на записи каждого объекта по значению и перейдите по кругу. Таким образом нам нужно изменить поведение по умолчанию.

В обозревателе решений разверните приложение\_запустите папку и откройте файл с именем WebApiConfig.cs. Добавьте следующий код в класс `WebApiConfig` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Этот код задает модуль форматирования JSON для сохранения ссылок на объекты и полностью удаляет модуль форматирования XML из конвейера. (Можно настроить модуль форматирования XML для сохранения ссылок на объект, но требует немного усилий и нам требуется только JSON для данного приложения. Дополнительные сведения см. в разделе [обработки циклических ссылок объекта](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

>[!div class="step-by-step"]
[Назад](using-web-api-with-entity-framework-part-1.md)
[Вперед](using-web-api-with-entity-framework-part-3.md)
