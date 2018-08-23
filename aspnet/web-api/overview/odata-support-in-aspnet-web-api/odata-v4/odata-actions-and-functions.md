---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Действия и функции в OData v4, с помощью ASP.NET Web API 2.2 | Документация Майкрософт
author: MikeWasson
description: В OData действия и функции — это способ для добавления поведений на стороне сервера, которые легко не определены как операций CRUD в объектах. В этом руководстве показано как...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 4e61c6b4bf59792b6570e32e6d24635d4f5e5ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838263"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Действия и функции в OData v4, с помощью ASP.NET Web API 2.2
====================
по [Майк Уоссон](https://github.com/MikeWasson)

> В OData действия и функции — это способ для добавления поведений на стороне сервера, которые легко не определены как операций CRUD в объектах. Этом руководстве показано, как добавить действия и функции для конечной точки OData v4, с помощью веб-API 2.2. Учебном курсе руководство [создания OData v4 конечной точки с помощью ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-API 2.2
> - OData v4
> - [Visual Studio 2013 с обновлением 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Учебника по версии
> 
> OData версии 3, см. в разделе [действия OData в веб-API ASP.NET 2](../odata-v3/odata-actions.md).


Разница между *действия* и *функции* действия может иметь побочные эффекты, а функции — нет. Действия и функции могут возвращать данные. Некоторые способы для действий:

- Сложных транзакциях.
- Управление несколько сущностей за один раз.
- Разрешение обновлений только на определенные свойства сущности.
- Отправка данных, который не является сущностью.

Функции полезны для извлечения информации, не относящиеся непосредственно к сущности или коллекции.

Действие (или функции) могут быть предназначены единственную сущность или коллекцию. В терминологии OData, это *привязки*. Вы также можете &quot;несвязанного&quot; действий и функций, которые вызываются как статические операции службы.

## <a name="example-adding-an-action"></a>Пример: Добавление действия

Давайте определим действие, чтобы дать оценку продукта.

> [!NOTE]
> Этот учебник основан на учебнике [создания OData v4 конечной точки с помощью ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


Во-первых, добавьте `ProductRating` модели для представления оценки.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Кроме того, добавить **DbSet** для `ProductsContext` класса, позволяя EF создаст таблицу оценок в базе данных.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Добавление действия к модели EDM

В файле WebApiConfig.cs добавьте следующий код:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action** метод добавляет действие в модель данных сущности (модель EDM). **Параметр** метод задает типизированный параметр для действия.

Этот код также задает пространство имен для модели EDM. Пространство имен имеет значение, так как URI для действия содержит действие, полное доменное имя:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> В типичной конфигурации IIS точка, в этот URL-адрес приведет к IIS, чтобы вернуть ошибку 404. Вы можете решить эту проблему, добавив следующий раздел в файл Web.Config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Добавьте метод контроллера для действия

Чтобы включить &quot;скорость&quot; действие, добавьте следующий метод в `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Обратите внимание на то, что имя метода совпадает с именем действия. **[HttpPost]** атрибут задает метод является методом HTTP POST.

Для вызова действия, клиент отправляет запрос HTTP POST, следующим образом:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;Скорость&quot; действие привязано к экземпляров продукта, поэтому URI для действия является имя полного действия, добавляемый в конец URI сущности. (Помните, что мы значение в пространстве имен EDM &quot;ProductService&quot;, поэтому действия полное доменное имя &quot;ProductService.Rate&quot;.)

Текст запроса содержит параметры действий как полезные данные JSON. Веб-API автоматически преобразует полезные данные JSON для **ODataActionParameters** объект, который является просто словарь значений параметров. Этот словарь можно используйте для доступа к параметрам метода контроллера.

Если клиент отправляет параметры действий в неправильном форматирования, значения **ModelState.IsValid** имеет значение false. Установите этот флажок, в методе контроллера и возвращает ошибку, если **IsValid** имеет значение false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Пример: Добавление функции

Теперь давайте добавим функцию OData, который возвращает самых дорогих продуктов. Как ранее, первым шагом является добавление функции модели EDM. В файле WebApiConfig.cs добавьте следующий код.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

В этом случае функция привязана к коллекции продуктов, а не отдельных экземпляров продукта. Клиенты вызывать функцию, отправив запрос GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Ниже приведен метод контроллера для этой функции.

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Обратите внимание на то, что имя метода соответствует имени функции. **[HttpGet]** атрибут задает метод является методом HTTP GET.

Ниже приведен HTTP-ответа.

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Пример: Добавление несвязанную функцию

Предыдущий пример был привязан к коллекции функции. В этом примере мы создадим *несвязанного* функции. Свободные функции вызываются как статические операции службы. В этом примере функция возвращает налог для данного почтового индекса.

Добавьте функцию модели EDM в файле WebApiConfig:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Обратите внимание, что мы называем **функция** непосредственно на **ODataModelBuilder**, а не типом сущности или коллекцией. Это сообщает построитель модели, что функция является свободным.

Ниже приведен метод контроллера, реализующий функцию.

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Неважно, какой контроллер веб-API, поместите этот метод в. Вы можете задать для него `ProductsController`, или определить отдельный контроллер. **[ODataRoute]** атрибут определяет шаблон URI для функции.

Ниже приведен пример запроса клиента.

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP-ответа:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
