---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Новые возможности в ASP.NET Web API 2.2 | Документация Майкрософт
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 51b01c4b9ee8d12e4e3925193e308d3ca6c5f2b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377445"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Новые возможности в ASP.NET Web API 2.2
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описываются новые возможности ASP.NET веб-API 2.2.

- [Скачать](#download)
- [Документация](#documentation)
- [Новые возможности в ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Атрибут улучшения маршрутизации](#ARI)
    - [Поддержка веб-API клиента Windows Phone 8.1](#phone)
- [Известные проблемы и критические изменения](#known-issues)
- [Исправления ошибок](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Бета-версии Microsoft.AspNet.WebAPI 5.2.3](#523)

<a id="download"></a>
## <a name="download"></a>Скачать

Компоненты среды выполнения выпускаются в виде пакетов NuGet из коллекции NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Последнюю версию пакета ASP.NET веб-API 2.2 имеет следующую версию: «5.2.0». Вы можете установить или обновить эти пакеты с помощью [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить на выпущенных пакетов NuGet с помощью консоли диспетчера пакетов NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения об ASP.NET Web API 2.2 доступны на веб-сайте ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Новые возможности в ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Этот выпуск добавляет поддержку для протокола OData версии 4. Дополнительные сведения см. в разделе [документации веб-API OData v4.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Ниже приведены некоторые основные возможности и изменения в OData v4:

- [Поддержка псевдонимов свойств в модели OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Поддержка ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute и ConcurrencyCheckAttribute в ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Предоставляют возможность указать понятное название для действий](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Интеграция с ODL UriParser
- Поддержка [перечисления](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [вложения](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) и [одноэлементный](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Поддержка приведения для типов-примитивов
- [Добавлена поддержка функции OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Поддерживает псевдонимы параметра для вызовов функций](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Поддерживает camel case соглашение об именовании в модели](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Поддержка cast() в $filter
- Поддержка открытых сложного типа
- Удален EntitySetController и AsyncEntitySetController
- [Измененные $link для $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Добавлена поддержка маршрутизации атрибутов](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Использует библиотеки OData Core 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Атрибут улучшения маршрутизации

Маршрутизация с помощью атрибутов теперь предоставляет точку расширения, вызывается IDirectRouteProvider, что позволяет полностью контролировать принципы обнаружения и настроить маршруты с атрибутами. IDirectRouteProvider отвечает за предоставление списка действия и контроллеры вместе с информацией о связанного маршрута для указания точности для этих действий требуется какие конфигурации маршрутизации. Реализация IDirectRouteProvider могут быть указаны при вызове MapAttributes/MapHttpAttributeRoutes.

Настройка IDirectRouteProvider будет простым путем расширения наших реализация по умолчанию, DefaultDirectRouteProvider. Этот класс предоставляет отдельный переопределяемый виртуальные методы, чтобы изменить логику для обнаружения атрибутов, создание записи маршрута и обнаружения префикс маршрута и префикс области.

Ниже приведены некоторые примеры, на что можно сделать с помощью этой новой точке расширения.

1. Поддержка наследования атрибутов маршрута

    Пример

    Здесь запрос like «/ 10/api/значений» успешно возвратит «Успех: 10»

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Предоставить имя маршрута по умолчанию для атрибутов маршрутов, следуя некоторые соглашение, которое вам нравится. По умолчанию маршрутизации с помощью атрибутов не создает автоматически имена для маршрутов на основе атрибутов.
3. Измените шаблон маршрута атрибут маршрутов в одном месте, прежде чем они заключаются в таблице маршрутов.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Поддержка веб-API клиента Windows Phone 8.1

Теперь можно использовать пакет NuGet клиента Web API для реализации логики клиента веб-API, при разработке для Windows Phone 8.1 или из в универсальное приложение.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

В этом разделе описываются известные проблемы и критические изменения в веб-API ASP.NET 2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Построитель модели

Проблема: Перегруженные функции не может быть представлено как FunctionImport

При наличии 2 перегруженные функции, и они также FunctionImport, как показано ниже, затем запросив результаты ~/GetAllConventionCustomers(CustomerName={customerName}) в System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Инструкции по решению: Возможным решением этой проблемы является добавление перегрузок функций как FunctionImports.

#### <a name="odata-routing"></a>Маршрутизация OData

Строковые литералы, которые включают URL-адрес в кодировке косой черты (% 2F), а backslash(%5C) вызвать ошибку 404, если они используются в пути к ресурсам OData.

Например строковые литералы можно использовать в пути к ресурсам OData как параметры функций или значения ключа наборов сущностей.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Когда службы получают такие запросы узлов будет un-escape те escape-последовательность перед их передачей в среду выполнения веб-API. Это обеспечивает защиту от атак, таких как следующие:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

В результате в стеке веб-API OData в сообщение об ошибке 404 (не найдено). Чтобы предотвратить эту ошибку, ваш клиент должен использовать двойные escape-последовательности для косой черты (% 252F) и обратную косую черту (% 255C). Этого не происходит для строк запросов, например /Employees? $filter = имя eq «% 2F имя»

**Обратите внимание, без escape-символы косой черты (/) и символов обратной косой черты ("") не допустимы в OData ресурс путь строковых литералах. Черты должны отображаться только как разделители путей и символов обратной косой черты не должен появляться в пути к ресурсу OData вообще. (Оба могут использоваться в некоторые части строку запроса OData).**

Инструкции по решению: Можно переопределить метод Parse DefaultODataPathHandler для экранирования перед анализом фактически косой черты и обратной косой черты в строковых литералах. Вы найдете пример по решению этой задачи.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Поддерживает запросы]

Атрибут [Queryable] является устаревшим. Новые приложения OData v3 должны использовать **System.Web.Http.OData.EnableQueryAttribute**.

**ODataHttpConfigurationExtensions.EnableQuerySupport** теперь добавляет метод расширения **EnableQueryAttribute** глобальную коллекцию фильтров. Если какие-либо контроллеры **[Queryable]** атрибут, вызвав `config.EnableQuerySupport()` вызовет **[Queryable]** атрибут переход на другой

Для устранения этой проблемы рекомендуется для замены всех экземпляров **QueryableAttribute** с **System.Web.Http.OData.EnableQueryAttribute**.

Второе решение — использовать следующий код в конфигурации веб-API:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Маршрутизация с помощью атрибутов

Проблема: Привязки модели сложного типа, который дополняется атрибутом FromUri ведет себя по-разному при использовании маршрутизации с помощью атрибутов.

Следующую ссылку отслеживает проблемы, а также содержит подробные сведения по решению этой проблемы.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Проблема: Формирование шаблонов MVC и веб-API в проект без 5.2.0 пакетов приводит 5.1.2 пакеты для тех, которые еще не существуют в проекте

Обновление пакетов NuGet для ASP.NET MVC 5.2 не обновить инструменты Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET. Они используют предыдущую версию пакетов среды выполнения ASP.NET (например 5.1.2 в обновлении 2). Таким образом формирование шаблонов ASP.NET установит предыдущую версию (например 5.1.2 в обновлении 2) необходимые пакеты, если они еще не доступны в проектах. Тем не менее формирование шаблонов ASP.NET в Visual Studio 2013 RTM или с обновлением 1 не перезаписывает последних пакетов в проектах. Если вы используете формирование шаблонов ASP.NET после обновления пакеты проектов веб-API 2.2 или ASP.NET MVC 5.2, убедитесь, что в версиях веб-API и ASP.NET MVC являются согласованными.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Исправления ошибок и незначительные обновления

Этот выпуск также включает несколько исправлений ошибок, а также дополнительный компонент обновления. Можно найти полный список здесь:

- [5.2 пакета](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Microsoft.AspNet.OData 5.2.1 пакет содержит обновление зависимостей NuGet, но не исправления ошибок. Это обновление больше не существует строгая зависимость на Microsoft.OData.Core 6.4.0, но один можно обновить до любой версии между 6.4.0 и 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

В этом выпуске мы изменили зависимость для `Json.Net 6.0.4`. Дополнительные сведения о новинках в этом выпуске `Json.NET`, см. в разделе [Json.NET 6.0 выпуск 4 — слияние JSON, внедрение зависимостей](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Этот выпуск не содержит других новых функций или исправления ошибок в веб-API. Впоследствии мы обновили все наши связанные пакеты, которые зависят от этой новой версии веб-API.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Бета-версии Microsoft.AspNet.WebAPI 5.2.3

Читайте о выпуске [здесь](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Этот выпуск содержит только исправления ошибок. Можно использовать [этот запрос](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) для просмотра списка проблем, исправленных в этом выпуске.
