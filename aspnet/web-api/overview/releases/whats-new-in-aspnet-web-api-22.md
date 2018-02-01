---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: "Новые возможности в ASP.NET Web API 2.2 | Документы Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-22"></a>Новые возможности в ASP.NET Web API 2.2
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описаны новые для ASP.NET Web API 2.2.

- [Скачать](#download)
- [Документация](#documentation)
- [Новые возможности в ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Атрибут улучшения маршрутизации](#ARI)
    - [Поддержка веб-API клиента Windows Phone 8.1](#phone)
- [Известные проблемы и критические изменения](#known-issues)
- [Исправления ошибок](#bug-fixes)
- [5.2.1 Microsoft.AspNet.OData](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 бета-версии](#523)

<a id="download"></a>
## <a name="download"></a>Скачать

Компоненты среды выполнения выпущены в виде пакетов NuGet при галереи NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Последнюю версию пакета ASP.NET Web API 2.2 имеет следующую версию: «5.2.0». Можно установить или обновить эти пакеты через [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить выпущенных пакетов NuGet, используя консоль диспетчера пакетов NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения о ASP.NET Web API 2.2 доступны из веб-сайт ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Новые возможности в ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Этот выпуск добавляет поддержку для протокола OData версии 4. Дополнительные сведения см. в разделе [документации v4 OData веб-API.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Ниже приведены некоторые основные возможности и изменения для OData v4.

- [Поддержка Совмещение имен свойств в модели OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Поддержка ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute и ConcurrencyCheckAttribute в ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Предоставляют возможность указать понятное название для действий](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Интеграция с ODL UriParser
- Поддержка [перечисления](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [вложения](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) и [одноэлементный](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Поддержка приведения типов-примитивов
- [Добавлена поддержка OData-функция](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Поддержка псевдонимы параметра вызовы функций](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Поддерживает camel варианта именования в модели](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Поддержка cast() в $filter
- Поддержка открытых сложный тип
- Удален EntitySetController и AsyncEntitySetController
- [Измененные $link для $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Добавлена поддержка маршрутизации атрибута](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Использует основные OData библиотеки 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Атрибут улучшения маршрутизации

Атрибут маршрутизации теперь предоставляет точку расширения, вызывается IDirectRouteProvider, что позволяет полностью контролировать способа обнаружения и настройки маршрутов с атрибутами. IDirectRouteProvider отвечает за предоставление списка контроллеров вместе с информацией о связанного маршрута для указания точно конфигурации маршрутизации, которые является полезными для тех действий, и действия. Реализация IDirectRouteProvider может быть указан при вызове MapAttributes/MapHttpAttributeRoutes.

Настройка IDirectRouteProvider будет простым, расширяя реализацию по умолчанию DefaultDirectRouteProvider. Этот класс предоставляет отдельный overridable виртуальные методы для изменения логики для обнаружения атрибутов, создание записи маршрута и обнаружение префикс маршрута и префикс области.

Ниже приведены некоторые примеры на что можно сделать с помощью этой новой точке расширения.

1. Поддержка наследования атрибутов маршрута

    Пример.

    Здесь запрос like «/ 10/api/значений» успешно вернет «Успех: 10»

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Укажите имя маршрута по умолчанию для маршрутов с атрибутами, выполнив некоторые соглашение, как вам удобно. По умолчанию атрибут маршрутизации не создает автоматически имена маршрутов с атрибутами.
3. Измените шаблон маршрута маршруты атрибутов в одном централизованном месте, прежде чем они заключаются в таблице маршрутов.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Поддержка веб-API клиента Windows Phone 8.1

Теперь можно использовать пакет NuGet клиента Web API для реализации логики клиента веб-API при разработке для Windows Phone 8.1 или из внутри универсального приложения.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

В этом разделе описываются известные проблемы и критические изменения в ASP.NET Web API 2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Построитель модели

Проблема: Перегруженные функции не может быть представлено как FunctionImport

При наличии 2 перегруженные функции, которые также FunctionImport, как показано ниже, затем запрос приведет к ~/GetAllConventionCustomers(CustomerName={customerName}) System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Обходной путь: Решение для этой проблемы является добавление перегрузок функций как FunctionImports.

#### <a name="odata-routing"></a>Маршрутизация OData

Строковые литералы, которые включают URL-адрес закодированные косая черта (% 2F) и backslash(%5C) вызывают ошибку 404, при их использовании в пути к ресурсам OData.

Строковые литералы могут использоваться в пути к ресурсам OData как параметры функций или ключевых значений наборов сущностей.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Когда службы получают такие запросы узлов будет отменить escape-символ те escape-последовательность перед их передачей в среду выполнения веб-API. Это обеспечивает защиту от атак на систему следующим образом:  
  
 http://www.contoso.com/..%2f..%2f/Windows/System32/cmd.exe?/c+dir+c:

В этом случае в стек OData веб-API, чтобы сообщение об ошибке 404 (не найдено). Чтобы предотвратить эту ошибку, ваш клиент должен использовать двойные escape-последовательности для косой черты (% 252F) и обратную косую черту (% 255C). Этого не происходит для строк запросов, например /Employees? $filter = имя eq «% 2F имя»

**Обратите внимание, без escape-символы косой черты (/) и символы обратной косой черты ("") не допустимы в строковые литералы в пути ресурса OData. Символы косой черты появится только в качестве разделителей пути и символы обратной косой черты не должен появляться в пути к ресурсу OData вообще. (Оба могут использоваться в некоторые части строки запроса OData).**

Обходной путь: Можно переопределить метод Parse DefaultODataPathHandler для escape-знаком косой черты и обратной косой черты в строковых литералах перед началом фактически их обработки. Пример такого подхода, здесь можно найти.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Запрашиваемым]

Атрибут [Queryable] является устаревшим. Новые OData v3 приложения должны использовать **System.Web.Http.OData.EnableQueryAttribute**.

**ODataHttpConfigurationExtensions.EnableQuerySupport** теперь добавляет метод расширения **EnableQueryAttribute** глобальную коллекцию фильтров. Если все контроллеры имеют **[Queryable]** атрибут вызова `config.EnableQuerySupport()` приведет к **[Queryable]** атрибут к сбою

Для устранения этой проблемы рекомендуется для замены всех экземпляров **QueryableAttribute** с **System.Web.Http.OData.EnableQueryAttribute**.

Второе решение — использовать следующий код в настройку веб-API:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Атрибут маршрутизации

Проблема: Привязка модели сложного типа, который является помеченной атрибутом FromUri ведет себя по-разному при использовании атрибута маршрутизации.

Используйте следующую ссылку отслеживает проблемы, а также сведения о временном решении.  
[http://aspnetwebstack.CodePlex.com/WorkItem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Проблема: API MVC или веб-формирования шаблонов в проект с 5.2.0 результаты пакеты в 5.1.2 пакеты для те, которые уже не существуют в проекте

Обновление пакетов NuGet для ASP.NET MVC 5.2 не обновляет средства Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET. Они используют предыдущую версию пакета среды выполнения ASP.NET (например 5.1.2 в обновлении 2). В результате формирование шаблонов ASP.NET установит предыдущую версию (например 5.1.2 в обновлении 2) необходимые пакеты, если они отсутствуют доступные в проектах. Формирование шаблонов ASP.NET в Visual Studio 2013 RTM или обновлением 1 не перезаписывает последние пакеты в проекте. Если вы используете формирование шаблонов ASP.NET после обновления пакеты проектов Web API 2.2 или ASP.NET MVC 5.2, убедитесь, что версии веб-API и ASP.NET MVC согласованы.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Исправления и обновления вспомогательных компонентов

Этот выпуск включает несколько исправлений ошибок, а также дополнительный компонент обновления. Можно найти полный список здесь:

- [5.2 пакета](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>5.2.1 Microsoft.AspNet.OData

Пакета Microsoft.AspNet.OData 5.2.1 содержит обновления зависимостей NuGet, но без исправления ошибок. Это обновление больше не strict зависимостей на Microsoft.OData.Core 6.4.0, но один можно обновить до любой версии между 6.4.0 и 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

В этом выпуске были внесены изменения для зависимости `Json.Net 6.0.4`. Дополнительные сведения о новых возможностях этого выпуска `Json.NET`, в разделе [Json.NET 6.0 Release 4 - слияния JSON, внедрение зависимостей](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Этот выпуск не имеет других новых возможностей или исправления ошибок в веб-API. Впоследствии мы обновили все зависимые пакеты мы собственные зависят от этой новой версии веб-API.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 бета-версии

Можно прочитать о выпуске [здесь](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Этот выпуск содержит только исправления ошибок. Можно использовать [этот запрос](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) Чтобы просмотреть список проблем, исправленных в этом выпуске.
