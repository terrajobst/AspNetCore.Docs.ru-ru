---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Наследование сложного типа в OData v4 с веб-API ASP.NET | Документация Майкрософт
author: microsoft
description: Согласно спецификации OData v4 сложный тип может наследовать от другого сложного типа. (Сложный тип является структурированного типа без ключа). Веб-API...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 8dbf7dc4cfc70e1ea1ed6f72ffc0751a56809c09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839094"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Наследование сложного типа в OData v4 с веб-API ASP.NET
====================
по [Microsoft](https://github.com/microsoft)

> В соответствии с OData v4 [спецификации](http://www.odata.org/documentation/odata-version-4-0/), сложный тип может наследовать от другого сложного типа. (Объект *сложных* тип является типом структурированных без ключа.) Веб-API OData 5.3 поддерживает наследование сложного типа.
> 
> В этом разделе описывается создание entity data model (EDM) наследование сложных типов. Полный исходный код, см. в разделе [OData сложный тип наследования пример](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Иерархия модели

Чтобы продемонстрировать наследование сложного типа, мы будем использовать следующие иерархии классов.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` — Это абстрактный сложный тип. `Rectangle`, `Triangle`, и `Circle` сложные типы являются производными от `Shape`, и `RoundRectangle` является производным от `Rectangle`. `Window` является типом сущности и содержит `Shape` экземпляра.

Ниже приведены классы CLR, которые определяют эти типы.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Создание модели EDM

Чтобы создать модель EDM, можно использовать **ODataConventionModelBuilder**, который выводит связи наследования из типов среды CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Вы также можете создавать модели EDM явно, с помощью **ODataModelBuilder**. Это занимает больше кода, но дает больший контроль над модели EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Эти два примера создайте одну и ту же схему модели EDM.

## <a name="metadata-document"></a>Документ метаданных

Ниже приведен документ метаданных OData, показывающая наследование сложного типа.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Из документа метаданных можно увидеть, что:

- `Shape` Сложный тип является абстрактным.
- `Rectangle`, `Triangle`, И `Circle` сложный тип имеет базовый тип `Shape`.
- `RoundRectangle` Тип имеет базовый тип `Rectangle`.

## <a name="casting-complex-types"></a>Приведение сложные типы

Теперь поддерживается приведение для сложных типов. Например, следующий запрос приведения `Shape` для `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Ниже приведен полезных данных ответа.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
