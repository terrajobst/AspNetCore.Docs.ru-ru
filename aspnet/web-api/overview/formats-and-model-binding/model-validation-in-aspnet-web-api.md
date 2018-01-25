---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: "Проверка в ASP.NET Web API модели | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 45b519af4073b62c8be1ca8951e44d6cf3cbe075
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="model-validation-in-aspnet-web-api"></a>Проверка модели в веб-API ASP.NET
====================
по [Mike Wasson](https://github.com/MikeWasson)

Когда клиент отправляет данные на веб-API, часто требуется проверить данные перед выполнением какой-либо обработки. В этой статье показано, как для заметок к моделям, использование примечаний для проверки данных и обработки ошибок проверки в веб-API.

## <a name="data-annotations"></a>Заметки к данным

В веб-API ASP.NET, можно использовать атрибуты из [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространства имен, чтобы задать правила проверки для свойства в модели. Рассмотрим следующую модель:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Если используется проверка модели в ASP.NET MVC, должна быть знакома. **Необходимые** атрибут говорится, что `Name` свойство не должно иметь значение null. **Диапазон** говорится, что атрибут `Weight` должен находиться в диапазоне от 0 до 999.

Предположим, что клиент отправляет запрос POST с следующее представление JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Вы увидите, что клиент не включил `Name` свойство, которое отмечено как требуется. Если веб-API преобразует JSON в `Product` экземпляр, он проверяет `Product` атрибутов проверки. В действии контроллера можно проверить, допустима ли модель:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Проверка модели не гарантирует безопасные данные клиента. В других слоях приложения может потребоваться дополнительная проверка. (Например, на уровне данных может применять ограничения внешних ключей). Учебник [с помощью веб-API с Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) рассматриваются некоторые из этих проблем.

**«Снизу учета»**: недостаточное учета происходит, когда клиент опущены некоторые свойства. Например предположим, что клиент отправляет следующее:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Здесь, клиент не указывает значения для `Price` или `Weight`. Модуль форматирования JSON назначает нулевое значение по умолчанию для отсутствующие свойства.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Состояние модели является допустимым, поскольку ноль является допустимым значением для этих свойств. Является ли это проблемой зависит от сценария. Например операция обновления может потребоваться для различения «ноль» и «не установлено». Чтобы заставить клиентов, чтобы задать значение, сделайте свойство допускает значения NULL и задать **необходимые** атрибута:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**«Чрезмерно учета»**: клиент также может отправлять *дополнительные* данных, чем ожидалось. Пример:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Здесь JSON включает свойство («цвет»), которые отсутствуют в `Product` модели. В этом случае модуль форматирования JSON просто игнорирует это значение. (Модуль форматирования XML делает то же самое.) Чрезмерное учета вызывает проблемы, если модель содержит свойства, которые предназначены только для чтения. Пример:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Требуется запретить пользователям обновлять `IsAdmin` свойства и может повышать уровень администраторам! Наиболее безопасным стратегия состоит в использовании класс модели, который точно соответствует, что клиент может отправлять:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Сообщение в блоге Брэда Уилсона «[vs проверки входных данных. Проверка в ASP.NET MVC модели](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)«содержит подробное обсуждение перевыборки учета и чрезмерное учет. Хотя сообщение о ASP.NET MVC 2, проблемы, по-прежнему актуальны для веб-API.


## <a name="handling-validation-errors"></a>Обработка ошибок проверки

Веб-API не возвращает автоматически ошибку клиенту при сбое проверки. Именно действия контроллера, чтобы проверить состояние модели и реагировать соответствующим образом.

Также можно создать фильтр действий, чтобы проверить состояние модели до вызова действия контроллера. Ниже приведен пример:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

При сбое проверки модели, этот фильтр возвращает HTTP-ответа, содержащего ошибки проверки. В этом случае действия контроллера не вызывается.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Чтобы применить фильтр ко всем контроллерам веб-API, нужно добавить экземпляр фильтра **HttpConfiguration.Filters** коллекции во время настройки:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Другой вариант — Чтобы установить фильтр как атрибут на отдельных контроллерах или действия контроллера:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
