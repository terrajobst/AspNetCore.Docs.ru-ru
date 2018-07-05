---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Часть 2: Создание моделей домена | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f379c92f7795c9fe10f7860b72188a8cfc1b6d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379729"
---
<a name="part-2-creating-the-domain-models"></a>Часть 2: Создание модели домена
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Добавление модели

Существует три способа подхода Entity Framework:

- Сначала база данных: запустите с базой данных и Entity Framework создает код.
- Основанная на модели: Начните с модели visual и Entity Framework создает код и базы данных.
- Code first: Начнем с кода, и платформа Entity Framework создает базу данных.

Мы используем подход code-first, поэтому начнем с определения объектов домена как POCO (обычные старые объекты CLR). Используя подход code-first объекты домена не обязательно дополнительного кода для поддержки на уровне базы данных, таких как транзакции или сохраняемость. (В частности, они не обязательно должен наследовать от [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) класса.) Заметки к данным по-прежнему можно использовать для управления как Entity Framework создает схему базы данных.

Поскольку POCO без выполнения любые дополнительные свойства, которые описывают [состояние базы данных](https://msdn.microsoft.com/library/system.data.entitystate.aspx), они легко могут быть сериализованы в JSON или XML. Тем не менее, это означает, что следует всегда предоставлять модели Entity Framework напрямую к клиентам, как мы увидим далее в этом руководстве.

Мы создадим следующие POCO.

- Продукт
- Номер
- OrderDetail

Для создания каждого класса, щелкните правой кнопкой мыши папку Models в обозревателе решений. В контекстном меню выберите **добавить** , а затем выберите **класса.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Добавление `Product` класс следующей реализацией:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

По соглашению Entity Framework использует `Id` свойство в качестве первичного ключа и сопоставляет его со столбцом идентификаторов в таблице базы данных. При создании нового `Product` экземпляра, не будет установлено значение для `Id`, так как база данных формирует значение.

**ScaffoldColumn** атрибут сообщает ASP.NET MVC, чтобы пропустить `Id` свойства при создании форму редактора. **Требуется** атрибут используется для проверки модели. Указывает, что `Name` свойство должно быть непустой строкой.

Добавление `Order` класса:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Добавление `OrderDetail` класса:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Отношения внешнего ключа

Заказ содержит многие подробности заказа и подробности каждого заказа ссылается на один продукт. Для представления этих отношений `OrderDetail` класс определяет свойства с именем `OrderId` и `ProductId`. Entity Framework определяет, что эти свойства представляют внешние ключи и добавит ограничения внешнего ключа к базе данных.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order` И `OrderDetail` классы также содержат свойства «навигация», которые содержат ссылки на связанные объекты. Учитывая заказа, можно перейти к продуктов, в том порядке, выполнив следующие свойства навигации.

Теперь скомпилируйте проект. Платформа Entity Framework использует отражение для обнаружения свойства моделей, поэтому для него требуется скомпилированной сборки для создания схемы базы данных.

## <a name="configure-the-media-type-formatters"></a>Настройте модули форматирования типа мультимедиа

Объект [форматирования типа мультимедиа](../../formats-and-model-binding/media-formatters.md) — это объект, который выполняет сериализацию данных, когда веб-API записывает в тексте ответа HTTP. Встроенные модули форматирования поддерживает вывод данных JSON и XML. По умолчанию оба этих модулей форматирования сериализации все объекты по значению.

Сериализации по значению возникает проблема, если граф объектов содержит циклические ссылки. Это именно так с `Order` и `OrderDetail` классов, поскольку каждый содержит ссылку на другой. Модуль форматирования используйте ссылки, записи каждого объекта по значению и перейдите по кругу. Таким образом нам нужно изменить поведение по умолчанию.

В обозревателе решений, разверните приложение\_запустите папку и откройте файл WebApiConfig.cs. Добавьте следующий код в класс `WebApiConfig` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Этот код задает модуль форматирования JSON для сохранения ссылок на объекты и полностью удаляет модуль форматирования XML из конвейера. (Можно настроить модуль форматирования XML для сохранения ссылок на объекты, но это немного больше работы, и нам требуется только JSON для этого приложения. Дополнительные сведения см. в разделе [обработка циклические ссылки объекта](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-1.md)
> [Вперед](using-web-api-with-entity-framework-part-3.md)
