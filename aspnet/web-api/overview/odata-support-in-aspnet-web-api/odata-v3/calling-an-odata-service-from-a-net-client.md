---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Вызов службы OData из клиента .NET (C#) | Документы Microsoft
author: MikeWasson
description: Этот учебник показывает, как для вызова службы OData из клиентского приложения C#. Версии программного обеспечения, используемые в учебник Visual Studio 2013 (работает с Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "28042398"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Вызов службы OData из клиента .NET (C#)
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Этот учебник показывает, как для вызова службы OData из клиентского приложения C#.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (работает с Visual Studio 2012)
> - [Библиотека клиентов служб данных WCF](https://msdn.microsoft.com/library/cc668772.aspx)
> - Веб-API 2. (Пример службы OData построена на основе веб-API 2, но клиентское приложение не зависит от веб-API).


В этом учебнике мы рассмотрим создание клиентского приложения, которая вызывает службу OData. Служба OData предоставляет следующие сущности:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Следующие статьи описывается, как реализовать службу OData в веб-API. (Не требуется читать их для понимания этого учебника, однако.)

- [Создание конечной точки OData в веб-API 2](creating-an-odata-endpoint.md)
- [Отношениями сущностей OData в веб-API 2](working-with-entity-relations.md)
- [Действия OData в веб-API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Создать прокси-службы

Первым шагом является создание прокси-службы. Прокси-службы представляет собой класс .NET, который определяет методы для доступа к службе OData. Прокси-сервер преобразует вызовы метода в HTTP-запросов.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Сначала откройте проект службы OData в Visual Studio. Нажмите клавиши CTRL + F5, чтобы запустить службу локально в IIS Express. Обратите внимание, локальный адрес, включая номер порта, присвоенный Visual Studio. Этот адрес потребуется при создании учетной записи-посредника.

Далее откройте другой экземпляр Visual Studio и создайте проект консольного приложения. Консольное приложение будет OData клиентского приложения. (Можно также добавить проект в решение службы.)

> [!NOTE]
> Оставшиеся шаги см. в проект.


В обозревателе решений щелкните правой кнопкой мыши **ссылки** и выберите **добавить ссылку на службу**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

В **добавить ссылку на службу** диалоговое окно, введите адрес службы OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

где *порт* — номер порта.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Для **имен**, введите «ProductService». Этот параметр определяет пространство имен класса-посредника.

Нажмите **Перейти**. Visual Studio считывает документ метаданных OData для обнаружения сущностей в службе.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Нажмите кнопку **ОК** для добавления класса-посредника в проект.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Создайте экземпляр класса прокси-сервера службы

Внутри вашей `Main` метод создания нового экземпляра класса-посредника, следующим образом:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Еще раз используйте действительный номер порта, где служба запущена. При развертывании службы используется URI интерактивную службу. Нет необходимости обновления учетной записи-посредника.

Следующий код добавляет обработчик событий, который выводит URI-адресов в окно консоли. Этот шаг не требуется, но интересно. в разделе URL-адреса для каждого запроса.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Запрос службы

Следующий код возвращает список продуктов из службы OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Обратите внимание, что не нужно писать код для отправки HTTP-запроса или проанализировать ответ. Класс прокси проводит при этом автоматически при перечислении `Container.Products` коллекции в **foreach** цикла.

При запуске приложение вывод должен выглядеть следующим образом:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Чтобы получить сущность по Идентификатору, используйте `where` предложения.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Для остальной части этого раздела, не будут отображаться всего `Main` функционировать только код, необходимый для вызова службы.

## <a name="apply-query-options"></a>Применить параметры запроса

OData определяет [параметры запроса](../supporting-odata-query-options.md) может использоваться для фильтрации, сортировки, страницы данных и т. д. В прокси-службы можно применить эти параметры с помощью разных выражений LINQ.

В этом разделе мы рассмотрим краткие примеры. Дополнительные сведения см. в разделе [рекомендации по LINQ (службы данных WCF)](https://msdn.microsoft.com/library/ee622463.aspx) на сайте MSDN.

### <a name="filtering-filter"></a>Фильтрации ($filter)

Чтобы отфильтровать, используйте `where` предложения. В следующем примере фильтрация по категории продукта.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Этот код соответствует следующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Обратите внимание, что прокси-сервер преобразует `where` предложение в OData `$filter` выражение.

### <a name="sorting-orderby"></a>Сортировка ($orderby)

Для сортировки используйте `orderby` предложения. Следующий пример Сортировка по цене от наибольшего к наименьшему.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Ниже приведен соответствующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Клиентские подкачки ($skip и $top)

Для больших наборов сущностей клиент может потребоваться ограничить количество результатов. Например клиент может показывать 10 записей за раз. Это называется *постраничный просмотр на стороне клиента*. (Также [постраничный просмотр на стороне сервера](../supporting-odata-query-options.md#server-paging), где сервер ограничивает количество результатов.) Чтобы выполнить постраничный просмотр на стороне клиента, используйте LINQ **пропустить** и **принимать** методы. В следующем примере пропускает первых 40 результаты и принимает следующие 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Ниже приведен соответствующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>SELECT ($select) и развернуть ($expand)

Чтобы включить связанные сущности, используйте `DataServiceQuery<t>.Expand` метод. Например, чтобы включить `Supplier` для каждого `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Ниже приведен соответствующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Чтобы изменить форму ответа, использовать LINQ **выберите** предложения. Следующий пример возвращает только имя каждого продукта, а не другие свойства.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Ниже приведен соответствующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Предложение select может включать связанные сущности. В этом случае не вызывать **развернуть**; прокси-сервера автоматически включает в себя расширение в этом случае. В следующем примере возвращается имя и поставщика каждого продукта.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Ниже приведен соответствующий запрос OData. Обратите внимание, что он включает **$expand** параметр.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Дополнительные сведения о $select и $expand разверните см. в разделе [развернуть с помощью $select, $ и $value в веб-API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Добавление новой сущности

Чтобы добавить новую сущность в наборе сущностей, вызовите `AddToEntitySet`, где *EntitySet* имя набора сущностей. Например `AddToProducts` добавляет новый `Product` для `Products` набора сущностей. При создании учетной записи-посредника служб данных WCF автоматически создает эти строго типизированных **AddTo** методы.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Чтобы добавить связь между двумя сущностями, используйте **AddLink** и **setlink будет работать** методы. Следующий код добавляет нового поставщика и новый продукт, а затем создает связи между ними.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Используйте **AddLink** при свойство навигации коллекции. В этом примере мы добавляем продукт `Products` коллекции на поставщика.

Используйте **setlink будет работать** при одной сущности с помощью свойства навигации. В этом примере рекомендуется задать `Supplier` свойства продукта.

## <a name="update--patch"></a>Обновления или исправления

Чтобы обновить сущность, вызовите **UpdateObject** метод.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Обновление выполняется при вызове **SaveChanges**. По умолчанию WCF отправляет запрос HTTP MERGE. **PatchOnUpdate** параметр предписывает WCF для отправки HTTP, PATCH, чтобы вместо этого.

> [!NOTE]
> Почему PATCH или MERGE? Исходной спецификации HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) не был определен любой метод HTTP в соответствии с семантикой «частичное обновление». Для поддержки частичных обновлений, спецификации OData определен метод MERGE. В 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) определен метод PATCH для частичного обновления. Некоторые из журнала в этом можно прочитать [блога](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) блоге WCF Data Services. В настоящее время ИСПРАВЛЕНИЯ предпочтительнее СЛИЯНИЯ. Контроллера OData, созданного посредством формирования шаблонов веб-API поддерживает оба метода.


Заменяет всю сущность (PUT семантика) укажите **ReplaceOnUpdate** параметр. В этом случае WCF для отправки запроса HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Удаление сущности

Чтобы удалить сущность, вызовите **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Вызвать действие OData

В OData [действия](odata-actions.md) позволяют добавить серверные поведения, которые легко не определены как операций CRUD в объектах.

Несмотря на то, что документ метаданных OData описывает действия, прокси-класса не создает никаких строго типизированных методов для них. По-прежнему можно вызвать действие OData с помощью универсального **Execute** метод. Тем не менее необходимо знать типы данных параметров и возвращаемого значения.

Например `RateProduct` действие имеет параметр с именем «Оценка» типа `Int32` и возвращает `double`. Ниже показано, как вызвать это действие.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Дополнительные сведения см. в разделе[действия и вызова операций службы](https://msdn.microsoft.com/library/hh230677.aspx).

Один из вариантов является расширение **контейнера** класса для предоставления строго типизированный метод, который вызывает действие:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
