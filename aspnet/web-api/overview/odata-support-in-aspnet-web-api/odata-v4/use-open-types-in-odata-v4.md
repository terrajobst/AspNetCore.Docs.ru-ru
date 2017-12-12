---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: "Открытые типы в OData v4 с веб-API ASP.NET | Документы Microsoft"
author: microsoft
description: "В OData v4 открытым типом является тип stuctured, который содержит динамические свойства, помимо любые свойства, объявленные в определении типа. Открыть..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: c2d7454534ff0e9e0a80365793800ab7c45d3b6e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Открытые типы в OData v4 с веб-API ASP.NET
====================
по [Microsoft](https://github.com/microsoft)

> В OData v4 *открытый тип* — stuctured тип, который содержит динамические свойства, помимо любые свойства, объявленные в определении типа. Открытые типы позволяют повысить гибкость модели анализа данных. Этого учебника показано, как использовать открытые типы в ASP.NET Web API OData.
> 
> Предполагается, что уже известно, как создать конечную точку OData в веб-API ASP.NET. В противном случае начните с просмотра [Создайте конечную точку OData v4](create-an-odata-v4-endpoint.md) первой.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - OData веб-API 5.3
> - OData v4


Во-первых, термины OData:

- Тип сущности: структурный тип с помощью ключа.
- Сложный тип: структурированного типа без ключа.
- Открытый тип: тип с динамическими свойствами. Типы сущностей и сложных типов может быть открыт.

Допускается значение динамического свойства, тип-примитив, сложный тип или тип перечисления; или коллекцию любого из этих типов. Дополнительные сведения об открытых типах см. в разделе [спецификации OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Установка библиотек OData Web

Используйте диспетчер пакетов NuGet для установки последних библиотек OData веб-API. В окне консоли диспетчера пакетов:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Определение типов CLR

Начинается с определения модели EDM как типы среды CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

При создании модели данных сущности (EDM)

- `Category`является типом перечисления.
- `Address`— Это сложный тип. (Он не имеет ключа, поэтому тип сущности не.)
- `Customer`Представляет тип сущности. (Он имеет ключ).
- `Press`представляет собой открытый сложный тип.
- `Book`представляет собой тип открытые сущности.

Чтобы создать открытый тип, тип среды CLR должен иметь свойство типа `IDictionary<string, object>`, который содержит динамические свойства.

## <a name="build-the-edm-model"></a>Построение модели EDM

Если вы используете **ODataConventionModelBuilder** для создания модели EDM, `Press` и `Book` автоматически добавляется как открытые типы, в зависимости от наличия `IDictionary<string, object>` свойство.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Можно также создать модель EDM явно, с помощью **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Добавить контроллер OData

Затем добавьте контроллер OData. В этом учебнике мы будем использовать упрощенный контроллер поддерживает только GET и POST запрашивает, которое использует список в памяти для хранения сущностей.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Обратите внимание, что первый `Book` экземпляр не имеет динамических свойств. Второй `Book` экземпляр имеет следующие динамические свойства:

- «Опубликованные»: тип-примитив
- «Авторы»: коллекции типов-примитивов
- «OtherCategories»: коллекцию типов перечисления.

Кроме того `Press` , свойство `Book` экземпляра имеет следующие динамические свойства:

- «Блог»: тип-примитив
- «Адрес»: сложный тип

## <a name="query-the-metadata"></a>Запрос метаданных

Чтобы получить документ метаданных OData, отправка запроса GET к `~/$metadata`. Текст ответа должен выглядеть следующим образом:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Из документа метаданных можно видеть, что:

- Для `Book` и `Press` типов, значение `OpenType` атрибут имеет значение true. `Customer` И `Address` типы не имеют этого атрибута.
- `Book` Тип сущности имеет три объявленных свойств: ISBN, название и нажмите клавишу. OData метаданные не содержат `Book.Properties` свойство из класса среды CLR.
- Аналогичным образом `Press` сложного типа есть только два объявленных свойств: имени и категории. Метаданные не содержат `Press.DynamicProperties` свойство из класса среды CLR.

## <a name="query-an-entity"></a>Запрос сущности

Для получения книги с ISBN равно «978-0-7356-7942-9» send отправьте запрос GET к `~/Books('978-0-7356-7942-9')`. Текст ответа должен выглядеть следующим образом. (Отступ сделать код более удобочитаемым.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Обратите внимание, что динамические свойства встроенные вместе с объявленные свойства.

## <a name="post-an-entity"></a>POST сущности

Чтобы добавить сущность книги, отправка запроса POST для `~/Books`. Клиент может задать динамические свойства в полезных данных запроса.

Ниже приведен пример запроса. Обратите внимание, свойства «Цена» и «Опубликовано».

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Если установить точку останова в методе контроллера, вы увидите, что веб-API добавлены для этих свойств `Properties` словаря.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

[Образец типа OData Open](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
