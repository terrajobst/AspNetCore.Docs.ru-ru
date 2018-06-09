---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Действия и функции в OData v4, с помощью ASP.NET Web API 2.2 | Документы Microsoft
author: MikeWasson
description: В OData действия и функции — это способ добавить серверные поведения, которые легко не определены как операций CRUD в объектах. В этом учебнике показано как...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508233"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Действия и функции в OData v4, с помощью ASP.NET Web API 2.2
====================
по [Mike Wasson](https://github.com/MikeWasson)

> В OData действия и функции — это способ добавить серверные поведения, которые легко не определены как операций CRUD в объектах. Этот учебник демонстрирует добавление действия и функции для конечной точки OData v4, с помощью Web API 2.2. Учебник построен на учебника [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-API 2.2
> - OData v4
> - [Visual Studio 2013 с обновлением 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Версии учебника
> 
> OData версии 3, в разделе [действия OData в ASP.NET Web API 2](../odata-v3/odata-actions.md).


Разница между *действия* и *функции* что действий может иметь побочные эффекты, и функции — нет. Действия и функции могут возвращать данные. Некоторые варианты применения действия включают:

- Сложных транзакциях.
- Управление сразу несколько сущностей.
- Разрешение обновлений только на определенные свойства сущности.
- Отправка данных, который не является сущностью.

Функции полезны для извлечения информации, не относящиеся непосредственно к сущности или коллекции.

Действие (или функции) можно выбрать целевую единственную сущность или коллекцию. В терминологии OData это *привязки*. Вы также можете &quot;несвязанного&quot; действий и функций, которые вызываются как статические операции службы.

## <a name="example-adding-an-action"></a>Пример: Добавление действий

Давайте определить действие, чтобы оценить продукт.

> [!NOTE]
> Этот учебник построен на учебника [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md)


Сначала добавьте `ProductRating` модели для представления рейтинга.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Кроме того, добавить **DbSet** для `ProductsContext` класса, чтобы EF создаст таблицу оценок в базе данных.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Добавить действие к модели EDM

В WebApiConfig.cs добавьте следующий код:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action** метод добавляет действие в модель данных сущности (EDM). **Параметр** метод задает типизированный параметр для действия.

Этот код также задает пространство имен для модели EDM. Пространство имен имеет значение, так как полное имя включает в себя URI для действия:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> В типичной конфигурации IIS точка в этот URL-адрес заставит IIS возвращает ошибку 404. Решить эту проблему, добавив следующий раздел в файл Web.Config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Добавьте метод контроллера для действия

Чтобы включить &quot;скорость&quot; действия, добавьте следующий метод `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Обратите внимание, что имя метода совпадает с именем действия. **[HttpPost]** атрибут указывает, что метод вызывается методом HTTP POST.

Для вызова действия, клиент отправляет запрос HTTP POST, следующим образом:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;Скорость&quot; привязано действие экземпляров продукта, поэтому URI для действия имя действия полное, добавляемый в конец URI сущности. (Помните, что мы задании пространства имен EDM &quot;ProductService&quot;, поэтому полное действие названо &quot;ProductService.Rate&quot;.)

Текст запроса содержит параметры действия как полезные данные JSON. Веб-API автоматически преобразует полезные данные JSON для **ODataActionParameters** объект, используемый словарь значений параметров. Этот словарь можно используйте для доступа к параметрам метода контроллера.

Если клиент отправляет параметров действия в неверную форматирования, значения **ModelState.IsValid** имеет значение false. Установить этот флаг в методе контроллера и возвращает ошибку, если **IsValid** имеет значение false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Пример: Добавление функции

Теперь добавим функцию OData, возвращает самый дорогостоящий продукта. Как и прежде, первым шагом является добавление функция модели EDM. В WebApiConfig.cs добавьте следующий код.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

В этом случае функция привязан к коллекции продуктов, а не отдельных экземпляров продукта. Функция вызывается клиентов, отправив запрос GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Далее приводится метод контроллера для этой функции.

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Обратите внимание, что имя метода совпадает с именем функции. **[HttpGet]** атрибут указывает, что метод вызывается метод HTTP GET.

Ниже приведен в HTTP-ответе.

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Пример: Добавление несвязанную функцию

Предыдущий пример был функции связаны с коллекцией. В этом примере мы создадим *несвязанного* функции. Свободные функции вызываются как статические операции службы. В этом примере функция возвращает налог для данного почтового индекса.

В файле WebApiConfig добавьте функцию в модель EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Обратите внимание, что поступает вызов **функция** непосредственно на **ODataModelBuilder**, вместо типа сущности или коллекции. Это значение определяет построитель модели, что функция является свободной.

Ниже приведен метод контроллера, который реализует функцию:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Неважно, какой контроллер веб-API, поместите в этот метод. Вы можете поместить в `ProductsController`, или определить отдельные контроллера. **[ODataRoute]** атрибут определяет шаблон URI для функции.

Ниже приведен пример запроса клиента.

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP-ответа:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
