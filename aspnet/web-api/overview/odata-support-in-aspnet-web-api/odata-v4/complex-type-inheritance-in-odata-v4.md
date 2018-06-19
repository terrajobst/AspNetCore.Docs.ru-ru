---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Сложный тип наследования в OData v4 с веб-API ASP.NET | Документы Microsoft
author: microsoft
description: Согласно спецификации OData v4 сложный тип может наследовать от другого сложного типа. (Сложного типа является структурированного типа без ключа). Веб-API...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508423"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Сложный тип наследования в OData v4 с веб-API ASP.NET
====================
по [Microsoft](https://github.com/microsoft)

> В соответствии с OData v4 [спецификации](http://www.odata.org/documentation/odata-version-4-0/), сложный тип может наследовать от другого сложного типа. (A *сложных* тип — структурированного типа без ключа.) Веб-API OData 5.3 поддерживает наследование сложного типа.
> 
> В этом разделе показано, как построить модель EDM (модель EDM) с наследование, сложные типы. Полный исходный код в разделе [OData сложный пример наследования типов](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - OData веб-API 5.3
> - OData v4


## <a name="model-hierarchy"></a>Иерархия модели

Чтобы продемонстрировать наследование сложных типов, мы будем использовать следующие иерархии классов.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`является абстрактным сложным типом. `Rectangle`, `Triangle`, и `Circle` являются сложными типами, производными от `Shape`, и `RoundRectangle` является производным от `Rectangle`. `Window`Тип сущности и содержит `Shape` экземпляра.

Ниже приведены классы среды CLR, которые определяют эти типы.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Построение модели EDM

Создание модели EDM, можно использовать **ODataConventionModelBuilder**, котором выводит отношения наследования из типов среды CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Можно также создать модель EDM явно, с помощью **ODataModelBuilder**. Принимает дополнительный код, но обеспечивает больший контроль над модели EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Эти два примера создания одной и той же схемы модели EDM.

## <a name="metadata-document"></a>Документ метаданных

Ниже приведен документ метаданных OData, показывающая наследование сложного типа.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Из документа метаданных можно видеть, что:

- `Shape` Сложный тип является абстрактным.
- `Rectangle`, `Triangle`, И `Circle` сложный тип имеет базовый тип `Shape`.
- `RoundRectangle` Тип имеет базовый тип `Rectangle`.

## <a name="casting-complex-types"></a>Приведение сложных типов

Теперь поддерживается приведение в сложных типах. Например, следующий запрос приведения `Shape` для `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Вот полезные данные ответа.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
