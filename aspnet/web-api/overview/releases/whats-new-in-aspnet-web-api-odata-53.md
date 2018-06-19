---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Новые возможности в ASP.NET Web API OData 5.3 | Документы Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508113"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>Новые возможности в ASP.NET Web API OData 5.3
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описываются новые возможности в ASP.NET Web API OData 5.3.

- [Скачать](#download)
- [Документация](#documentation)
- [Основные OData библиотеки](#corelib)
- [Новые возможности](#newf)
- [Известные проблемы и критические изменения](#known-issues)
- [Исправления ошибок](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Скачать

Компоненты среды выполнения выпущены в виде пакетов NuGet при галереи NuGet. Можно установить или обновить выпущенных пакетов NuGet, используя консоль диспетчера пакетов NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Документация

Можно найти учебники и другую документацию по ASP.NET Web API OData в [веб-сайт ASP.NET](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>Основные OData библиотеки

Для OData v4 веб-API теперь использует ODataLib версии 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Новые возможности в ASP.NET веб-API OData 5.3

### <a name="support-for-levels-in-expand"></a>Разверните поддержка $levels $

Можно использовать $levels запроса в $разверните запросов. Пример:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Этот запрос эквивалентен:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Поддержка типов открытые сущности

*Открытый тип* — stuctured тип, который содержит динамические свойства, помимо любые свойства, объявленные в определении типа. Открытые типы позволяют повысить гибкость модели анализа данных. Дополнительные сведения см. в разделе xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Поддержка динамическую коллекцию свойств в открытых типах

Раньше динамических свойств должны были быть одно значение. В 5.3 динамические свойства могут иметь значения коллекции. Например, в следующем полезные данные JSON `Emails` свойства является динамическим и коллекции строкового типа:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Поддержка наследования для сложных типов

Теперь сложные типы могут наследовать от базового типа. Например службы OData определить следующих сложных типов:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Вот модели EDM для этого примера.

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Дополнительные сведения см. в разделе [OData сложный пример наследования типов](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

В этом разделе описываются известные проблемы и критические изменения в ASP.NET Web API OData 5.3.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Параметры запросов

Проблема: С помощью $ вложенного разверните с $levels = максимальное число результатов в глубину неверные расширения.

Например рассмотрим следующий запрос:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Если `MaxExpansionDepth` равно 5, этот запрос приведет к появлению расширения глубина 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Исправления и обновления вспомогательных компонентов

Этот выпуск включает несколько исправлений ошибок, а также дополнительный компонент обновления. Можно найти полный список здесь:

- [Исправления ошибок](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

В этом выпуске мы внесли [исправления ошибки](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) к некоторым AllowedFunctions перечислений. Этот выпуск не имеет других исправления или новые функции.
