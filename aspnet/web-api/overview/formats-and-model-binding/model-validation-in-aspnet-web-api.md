---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Проверка модели в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 75474d5597ba11e09f788269b9a2a278559c63a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393100"
---
<a name="model-validation-in-aspnet-web-api"></a>Проверка модели в веб-API ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Когда клиент отправляет данные в веб-API, часто требуется проверять данные перед обработкой. В этой статье показано, как для создания заметок к моделям, использование заметок для проверки данных и обработки ошибок проверки в веб-API.

## <a name="data-annotations"></a>Заметки к данным

В веб-API ASP.NET, можно использовать атрибуты из [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) пространства имен, чтобы задать правила проверки для свойства в модели. Рассмотрим следующую модель:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Если вы использовали проверка модели в ASP.NET MVC, это должна быть знакома. **Требуется** атрибут говорится, что `Name` свойство не должно иметь значение null. **Диапазон** атрибут говорится, что `Weight` должен находиться в диапазоне от 0 до 999.

Предположим, что клиент отправляет запрос POST с следующее представление JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Вы увидите, что клиент не включил `Name` свойство, которое помечается как требуется. Когда веб-API преобразует JSON-файла в `Product` экземпляр, он проверяет `Product` от атрибутов проверки. В действии контроллера можно проверить, допустима ли модель:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Проверка модели не гарантирует, что данные клиента безопасен. Возможно, потребуется дополнительная проверка другим уровням приложения. (Например, уровень данных может принудительно применять ограничения внешних ключей.) Руководства [с помощью веб-API с Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) исследует некоторые из этих проблем.

**«Заниженных учета»**: недостаточное число ресурсов учета происходит, когда клиент оставляет out некоторые свойства. Например предположим, что клиент отправляет следующее:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Здесь, клиент не указал значения для `Price` или `Weight`. Модуль форматирования JSON назначается значение по умолчанию, от нуля до отсутствующие свойства.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Состояние модели является допустимым, так как нуль является допустимым значением для этих свойств. Является ли это проблемой зависит от сценария. Например в операции обновления, может потребоваться различать «ноль» и «не установлено». Чтобы заставить клиентов, чтобы задать значение, сделайте свойство, допускающее значение NULL и set **требуется** атрибут:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**«Чрезмерной передачи данных»**: клиент может также отправлять *дополнительные* данных, чем ожидалось. Пример:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Здесь JSON включает свойство («цвет»), которое не существует в `Product` модели. В этом случае модуль форматирования JSON просто игнорирует это значение. (Модуль форматирования XML делает то же самое.) От чрезмерной передачи данных вызывает проблемы, если модель содержит свойства, которые должны быть доступны только для чтения. Пример:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Вы не хотите пользователям обновлять `IsAdmin` свойства и повысить свои права до Администраторы! Наиболее безопасным при этом важно использовать класс модели, которая точно совпадает с допустимых клиента для отправки:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> В блоге Брэда уилсона: "[vs проверка входных данных. Проверка модели в ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)«содержит подробное обсуждение заниженных учета и от чрезмерной передачи данных. Несмотря на то, что, является запись об ASP.NET MVC 2, вопросы по-прежнему относятся к веб-API.


## <a name="handling-validation-errors"></a>Обработка ошибок проверки

Веб-API не возвращает автоматически ошибку клиенту при сбое проверки. Возлагается действие контроллера, чтобы проверить состояние модели и реагировать соответствующим образом.

Также можно создать фильтр действий для проверки состояния модели, перед вызовом действия контроллера. Ниже показан пример:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Если проверка модели не пройдена, этот фильтр возвращает ответ HTTP, который содержит ошибки проверки. В этом случае действие контроллера не вызывается.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Чтобы применить фильтр ко всем контроллерам веб-API, добавьте экземпляр фильтра, который **HttpConfiguration.Filters** коллекции во время настройки:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Другой вариант — установить фильтр в качестве атрибута на отдельным контроллерам или действиям контроллера:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
