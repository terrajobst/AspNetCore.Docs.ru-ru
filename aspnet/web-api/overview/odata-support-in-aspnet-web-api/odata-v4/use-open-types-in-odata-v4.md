---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Открытые типы в OData v4 с веб-API ASP.NET | Документация Майкрософт
author: microsoft
description: В OData v4 открытого типа является типом stuctured, который содержит динамические свойства, а также любые свойства, объявленные в определении типа. Открыть...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 77771d85532b8b622c2ad4ca219a38990e474c9c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828361"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Открытые типы в OData v4 с веб-API ASP.NET
====================
по [Microsoft](https://github.com/microsoft)

> В OData v4 *открытый тип* является типом stuctured, который содержит динамические свойства, а также любые свойства, объявленные в определении типа. Открытые типы позволяют добавлять гибкость к моделям данных. Этом руководстве показано, как использовать открытые типы в OData веб-API ASP.NET.
> 
> Предполагается, что вы уже знаете, как создать конечную точку OData в веб-API ASP.NET. Если это не так, обратитесь к статье [Создайте конечную точку OData v4](create-an-odata-v4-endpoint.md) первого.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-API OData 5.3
> - OData v4


Во-первых, термины OData:

- Тип сущности: структурный тип с ключом.
- Сложный тип: структурированного типа без ключа.
- Открытый тип: тип с динамические свойства. Типы сущностей и сложных типов может быть открыт.

Значение динамического свойства может быть тип-примитив, сложный тип или тип перечисления; или коллекцию любого из этих типов. Дополнительные сведения об открытых типах см. в разделе [спецификации OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Установка библиотеки OData Web

С помощью диспетчера пакетов NuGet для установки последних версий библиотек OData веб-API. В окне консоли диспетчера пакетов:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Определение типов среды CLR

Начинается с определения модели EDM как типы среды CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

При создании Entity Data Model (EDM)

- `Category` является типом перечисления.
- `Address` — Это сложный тип. (Он не имеет ключа, поэтому он не является типом сущности.)
- `Customer` является типом сущности. (Он имеет ключ).
- `Press` имеет открытый сложный тип.
- `Book` Это тип сущности открытым.

Чтобы создать открытый тип, тип среды CLR должен иметь свойство типа `IDictionary<string, object>`, который содержит динамические свойства.

## <a name="build-the-edm-model"></a>Создание модели EDM

Если вы используете **ODataConventionModelBuilder** для создания модели EDM `Press` и `Book` автоматически добавляется в качестве открытых типов, в зависимости от наличия `IDictionary<string, object>` свойство.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Вы также можете создавать модели EDM явно, с помощью **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Добавить контроллер OData

Добавьте контроллер OData. В этом учебнике мы будем использовать упрощенный контроллер поддерживает только GET и POST запрашивает что использует список в памяти для хранения сущностей.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Обратите внимание, что первый `Book` экземпляр не имеет динамических свойств. Второй `Book` экземпляр имеет следующие динамические свойства:

- «Опубликовано»: тип-примитив
- «Авторы»: коллекции типов-примитивов
- «OtherCategories»: коллекцию типов перечисления.

Кроме того `Press` , свойство `Book` экземпляра имеет следующие динамические свойства:

- «Блог»: тип-примитив
- «Address»: сложный тип

## <a name="query-the-metadata"></a>Запрос метаданных

Чтобы получить документ метаданных OData, отправьте запрос GET к `~/$metadata`. Текст ответа должен выглядеть примерно следующим образом:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Из документа метаданных можно увидеть, что:

- Для `Book` и `Press` типов — значение `OpenType` атрибут имеет значение true. `Customer` И `Address` типы не имеют этого атрибута.
- `Book` Тип сущности имеет три объявленных свойств: ISBN, заголовок и нажмите клавишу. OData метаданные не содержат `Book.Properties` свойство из класса CLR.
- Аналогичным образом `Press` сложный тип имеет только два объявленных свойств: имени и категории. Метаданные не содержат `Press.DynamicProperties` свойство из класса CLR.

## <a name="query-an-entity"></a>Запрос сущности

Чтобы получить в книге с ISBN, равным «978-0-7356-7942-9», отправьте запрос GET для `~/Books('978-0-7356-7942-9')`. Текст ответа должен выглядеть следующим образом. (Отступ сделать код более удобочитаемым.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Обратите внимание, что динамические свойства встраивается внутрь объявленные свойства.

## <a name="post-an-entity"></a>POST сущности

Чтобы добавить сущность книги, отправьте запрос POST к `~/Books`. Клиент может задать динамические свойства в полезных данных запроса.

Ниже приведен пример запроса. Обратите внимание, в свойствах «Price» и «Опубликовано».

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Если установить точку останова в методе контроллера, вы увидите, что веб-API добавлены эти свойства, чтобы `Properties` словаря.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

[Образец типа открытым OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
