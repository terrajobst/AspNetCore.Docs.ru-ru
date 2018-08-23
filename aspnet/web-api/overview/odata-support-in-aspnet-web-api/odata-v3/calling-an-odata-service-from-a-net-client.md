---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Вызов службы OData из клиента .NET (C#) | Документация Майкрософт
author: MikeWasson
description: Этом руководстве показано, как вызов службы OData из клиентского приложения C#. Версии программного обеспечения, используемые в руководства для Visual Studio 2013 (работает с Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 45d4d1304d2c628d2ab0d09925d2050bf6f58c60
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829332"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Вызов службы OData из клиента .NET (C#)
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Этом руководстве показано, как вызов службы OData из клиентского приложения C#.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (работает с Visual Studio 2012)
> - [Библиотека клиентов служб данных WCF](https://msdn.microsoft.com/library/cc668772.aspx)
> - Веб-API 2. (Пример службы OData создается с помощью веб-API 2, но клиентское приложение не зависит от веб-API).


В этом руководстве мы рассмотрим создание клиентского приложения, которая вызывает службу OData. Служба OData предоставляет следующие сущности:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

В следующих статьях описываются способы реализации службы OData в веб-API. (Не требуется читать их, чтобы понять этот учебник, однако.)

- [Создание конечной точки OData в веб-API 2](creating-an-odata-endpoint.md)
- [Отношения сущностей OData в веб-API 2](working-with-entity-relations.md)
- [Действия OData в веб-API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Создание прокси-службы

Первым шагом является создание прокси-службы. Прокси-службы представляет собой класс .NET, который определяет методы для доступа к службе OData. Прокси-сервер преобразует вызовы метода в HTTP-запросов.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Сначала откройте проект службы OData в Visual Studio. Нажмите клавиши CTRL + F5, чтобы запустить службу локально в IIS Express. Обратите внимание, локальный адрес, включая номер порта, который назначает Visual Studio. Этот адрес понадобится при создании прокси-сервер.

Далее откройте другой экземпляр Visual Studio и создайте проект консольного приложения. Консольное приложение будет OData в клиентском приложении. (Можно также добавить проект в решение как службу.)

> [!NOTE]
> Остальные шаги см. в проект.


В обозревателе решений щелкните правой кнопкой мыши **ссылки** и выберите **Add Service Reference**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

В **Add Service Reference** диалоговом окне введите адрес службы OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

где *порт* — номер порта.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Для **пространства имен**, введите «ProductService». Этот параметр определяет пространство имен класса-посредника.

Нажмите **Перейти**. Visual Studio считывает документ метаданных OData для обнаружения сущностей в службе.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Нажмите кнопку **ОК** для добавления прокси-класса в проект.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Создайте экземпляр прокси-класса службы

Внутри вашей `Main` метод, создайте новый экземпляр класса-посредника, следующим образом:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Опять же используйте действительный номер порта, где служба запущена. При развертывании службы используется URI службы в реальном времени. Не нужно обновить прокси-сервер.

Следующий код добавляет обработчик событий, который выводит запросы URI в окно консоли. Этот шаг не является обязательным, но это интересно посмотреть, URI для каждого запроса.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Запросы к службе

Следующий код возвращает список продуктов из службы OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Обратите внимание на то, что не нужно писать код для отправки HTTP-запроса или проанализировать ответ. Класс прокси проводит при этом автоматически при перечислении `Container.Products` коллекции в **foreach** цикла.

При запуске приложения результат должен выглядеть следующим образом:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Чтобы получить сущность по Идентификатору, используйте `where` предложение.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Для остальной части этого раздела, я не буду приводить всего `Main` функционировать только код, необходимый для вызова службы.

## <a name="apply-query-options"></a>Применить параметры запроса

OData определяет [параметры запроса](../supporting-odata-query-options.md) , можно использовать для фильтрации, сортировки, страницы данных и т. д. В случае прокси сервиса эти параметры можно применить с помощью различных выражений LINQ.

В этом разделе я покажу краткие примеры. Дополнительные сведения см. в разделе [рекомендации по LINQ (службы данных WCF)](https://msdn.microsoft.com/library/ee622463.aspx) на сайте MSDN.

### <a name="filtering-filter"></a>Фильтрации ($filter)

Для фильтрации используйте `where` предложение. В следующем примере отфильтровываются по категориям продуктов.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Этот код соответствует следующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Обратите внимание, что прокси-сервер преобразует `where` предложение в OData `$filter` выражение.

### <a name="sorting-orderby"></a>Сортировка ($orderby)

Чтобы отсортировать данные, используйте `orderby` предложение. Следующий пример сортирует по цене от самого высокого до самого низкого.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Ниже приведен соответствующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Разбиение по страницам на стороне клиента ($skip и $top)

Для больших наборов сущностей клиент может потребоваться ограничить количество результатов. Например клиент может показывать 10 записей за раз. Это называется *разбиение по страницам на стороне клиента*. (Также [серверное разбиение по страницам](../supporting-odata-query-options.md#server-paging), где сервер ограничивает число результатов.) Чтобы выполнить разбиение по страницам на стороне клиента, использовать LINQ **Skip** и **занять** методы. В следующем примере пропускает первых 40 результата и принимает следующие 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Ниже приведен соответствующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>SELECT ($select) и развернуть ($expand)

Чтобы включить связанные сущности, используйте `DataServiceQuery<t>.Expand` метод. Например, чтобы включить `Supplier` для каждого `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Ниже приведен соответствующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Чтобы изменить форму ответа, использовать LINQ **выберите** предложение. Следующий пример возвращает только имя каждого продукта, с помощью никакие другие свойства.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Ниже приведен соответствующий запрос OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Предложение select может включать связанные сущности. В этом случае не следует вызывать **Expand**; прокси-сервер автоматически включает в себя развертывание в этом случае. В следующем примере возвращается имя и поставщика каждого продукта.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Ниже приведен соответствующий запрос OData. Обратите внимание, что он включает **$expand** параметр.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Дополнительные сведения о $select и $expand развернуть, см. в разделе [использование $select, $expand и $value в веб-API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Добавить новую сущность

Чтобы добавить новую сущность в наборе сущностей, вызовите `AddToEntitySet`, где *EntitySet* имя набора сущностей. Например `AddToProducts` добавляет новый `Product` для `Products` набора сущностей. При создании прокси-сервера, службы данных WCF автоматически создает эти со строгой типизацией **AddTo** методы.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Чтобы добавить связь между двумя сущностями, используйте **AddLink** и **SetLink** методы. Следующий код добавляет нового поставщика и новый продукт, а затем создает связи между ними.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Используйте **AddLink** при свойство навигации является коллекцией. В этом примере мы добавляем продукт для `Products` коллекции поставщика.

Используйте **SetLink** Если это свойство навигации имеет одну сущность. В этом примере мы указываем `Supplier` свойство над продуктом.

## <a name="update--patch"></a>Обновления или исправления

Чтобы обновить сущность, вызовите **UpdateObject** метод.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Обновление выполняется при вызове **SaveChanges**. По умолчанию WCF отправляет запрос HTTP MERGE. **PatchOnUpdate** параметр предписывает WCF для отправки HTTP, PATCH, чтобы вместо этого.

> [!NOTE]
> Почему PATCH и MERGE? Исходная спецификация HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) не был определен любого метода HTTP с семантикой «частичное обновление». Чтобы обеспечить поддержку частичных обновлений, спецификации протокола OData определен метод MERGE. В 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) определенный метод PATCH для частичного обновления. Можно ознакомиться с некоторыми журнала в этом [блога](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) блоге WCF Data Services. В настоящее время ИСПРАВЛЕНИЙ предпочтительнее СЛИЯНИЯ. Контроллер OData, созданные путем формирования шаблонов веб-API поддерживает оба метода.


Если вы хотите заменить всю сущность (семантикой PUT), укажите **ReplaceOnUpdate** параметр. В результате WCF для отправки запроса HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Удаление сущности

Чтобы удалить сущность, вызовите **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Вызвать действие OData

В OData [действия](odata-actions.md) позволяют добавлять поведения на стороне сервера, которые легко не определены как операций CRUD в объектах.

Несмотря на то, что документ метаданных OData описывает действия, прокси-класса не создает никаких строго типизированных методов для них. По-прежнему можно вызвать действие OData с помощью универсального **Execute** метод. Тем не менее необходимо знать типы данных параметров и возвращаемого значения.

Например `RateProduct` действие имеет параметр с именем «Rating» типа `Int32` и возвращает `double`. Ниже показано, как вызывать это действие.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Дополнительные сведения см. в разделе[вызов операций служб и процессов](https://msdn.microsoft.com/library/hh230677.aspx).

Один из вариантов является расширение **контейнера** класс, чтобы обеспечить строго типизированный метод, который вызывает действие:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
