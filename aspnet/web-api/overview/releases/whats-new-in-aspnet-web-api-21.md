---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Новые возможности в ASP.NET Web API 2.1 | Документация Майкрософт
author: microsoft
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 6c5a73b95955663d76238e88009ca700f9ace010
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836302"
---
<a name="whats-new-in-aspnet-web-api-21"></a>Новые возможности в ASP.NET Web API 2.1
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описываются новые возможности ASP.NET Web API 2.1.

- [Скачать](#download)
- [Документация](#documentation)
- [Новые возможности в ASP.NET Web API 2.1](#new-features)

    - [Обработки глобальных ошибок](#global-error)
    - [Атрибут улучшения маршрутизации](#attribute-routing)
    - [Усовершенствования на странице справки](#help-page)
    - [Поддержка IgnoreRoute](#ignoreroute)
    - [Модуль форматирования типа мультимедиа BSON](#bson)
    - [Улучшенная поддержка Async фильтры](#async-filters)
    - [Запрос, синтаксический анализ для форматирования библиотеки клиента](#query-parsing)
- [Известные проблемы и критические изменения](#known-issues)
- [Исправления ошибок](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Скачать

Компоненты среды выполнения выпускаются в виде пакетов NuGet из коллекции NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Последнюю версию пакета ASP.NET Web API 2.1 RTM имеет следующую версию: «5.1.2». Вы можете установить или обновить эти пакеты с помощью [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить на выпущенных пакетов NuGet с помощью консоли диспетчера пакетов NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения об ASP.NET Web API 2.1 RTM доступны на веб-сайте ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Новые возможности в ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Обработки глобальных ошибок

Через один центральный механизм теперь могут регистрироваться все необработанные исключения, и их можно настроить поведение для необработанных исключений.

Платформа поддерживает несколько средств ведения журнала исключений, которые все необработанные исключения и сведения о контексте, в котором она произошла, таких как запрос, обработка которого во время см. в разделе.

Например следующий код использует System.Diagnostics.TraceSource все необработанные исключения в журнал:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Также можно заменить обработчик исключений по умолчанию, таким образом, можно полностью настроить сообщение HTTP-ответа, который отправляется, когда необработанное исключение происходит.

Мы предоставили [пример](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , записывает в журнал все необработанные исключения через популярная платформа ELMAH.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Атрибут улучшения маршрутизации

Маршрутизация с помощью атрибутов теперь поддерживает ограничения, обеспечивая управление версиями и выбор маршрута на основе заголовков. Кроме того, многие аспекты маршруты с атрибутами, теперь можно настраивать с помощью **IDirectRouteFactory** интерфейс и **RouteFactoryAttribute** класса. Префикс маршрута теперь можно расширять посредством **IRoutePrefix** интерфейс и **RoutePrefixAttribute** класса.

Мы предоставили [пример](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) , использующий ограничения для динамической фильтрации контроллеров, заголовок HTTP «api-version».

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Усовершенствования на странице справки

Веб-API 2.1 включает следующие улучшения для [страницы справки по API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Документация отдельные свойства параметров или возвращаемых типов действий.
- Документация по модели заметок к данным.

Дизайн пользовательского интерфейса страницы справки также был изменен, в соответствии с этими изменениями.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Поддержка IgnoreRoute

Веб-API 2.1 поддерживают пропуск шаблоны URL-адресов, маршрутизации веб-API с помощью набора **IgnoreRoute** методов расширения в **HttpRouteCollection**. Эти методы вызывают веб-API игнорировать все URL-адреса, которые соответствуют указанным шаблоном и разрешить основному приложению применять дополнительную обработку при необходимости.

Следующий пример игнорирует URI, которые начинаются с &quot;содержимого&quot; сегмент:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Модуль форматирования типа мультимедиа BSON

Веб-API теперь поддерживает [BSON](http://bsonspec.org/) формат подключения, как на клиенте, так и на сервере.

Чтобы включить BSON на стороне сервера, добавьте **BsonMediaTypeFormatter** в коллекцию модулей форматирования:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Вот, как клиент .NET может использовать формат BSON.

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Мы предоставили [пример](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) , в котором показано на стороне сервера и клиента.

Дополнительные сведения см. в разделе [поддержка BSON в Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Улучшенная поддержка Async фильтры

Веб-API теперь поддерживает простой способ создать фильтры, которые выполняются асинхронно. Эта функция полезна фильтра нужно выполнить действие async, таких как доступ в базе данных. Ранее для создания асинхронного фильтра, приходилось реализуют интерфейс filter, самостоятельно, так как базовые классы фильтра доступны только для синхронных методов. Теперь вы можете переопределить виртуальный `On*Async` методы фильтра базового класса.

Пример:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**, **ActionFilterAttribute**, и **ExceptionFilterAttribute** все классы поддерживают async в Web API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Запрос, синтаксический анализ для форматирования библиотеки клиента

Ранее **System.Net.Http.Formatting** поддерживается синтаксический анализ и обновление запросы URI для кода на стороне сервера, но эквивалентные переносимой библиотеки отсутствует эта функция. В Web API 2.1 клиентское приложение теперь можно легко проанализировать и обновить строку запроса.

Следующие примеры показывают, как для синтаксического анализа, изменять и создавать запросы URI. (В примерах консольное приложение для простоты.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

В этом разделе описываются известные проблемы и критические изменения в ASP.NET Web API 2.1 RTM.

### <a name="attribute-routing"></a>Маршрутизация с помощью атрибутов

Неоднозначности в маршрутизации соответствует атрибута теперь сообщают ошибку вместо выбора первого совпадения.

Маршруты с атрибутами, запрещается использовать *{controller}* параметра и обратно с помощью *{action}* параметр на маршрутах уделено действия. Эти параметры большой вероятностью приведет к неоднозначности.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Формирование шаблонов MVC и веб-API в проект с помощью 5.1 результатов пакеты в 5.0 пакеты для тех, которые еще не существуют в проекте

Обновление пакетов NuGet для ASP.NET Web API 2.1 RTM не обновляет средств Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET. Они используют предыдущую версию пакетов среды выполнения ASP.NET (5.0.0.0). Таким образом формирование шаблонов ASP.NET установит предыдущую версию (5.0.0.0) необходимые пакеты, если они еще не доступны в проектах. Тем не менее формирование шаблонов ASP.NET в Visual Studio 2013 RTM или с обновлением 1 не перезаписывает последних пакетов в проектах.

Если вы используете формирование шаблонов ASP.NET после обновления пакетов Web API 2.1 или ASP.NET MVC 5.1, убедитесь, что в версиях веб-API и MVC являются согласованными.

### <a name="type-renames"></a>Переименование типа

Некоторые типы, используемые для расширения маршрутизации атрибут были переименованы в версии-Кандидата до 2.1 RTM.

| Старое имя типа (2.1 версия-Кандидат) | Имя нового типа (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Фильтры исключений не unwrap агрегатные исключения в асинхронных действиях

Ранее Если действие async создала **AggregateException**, фильтр исключений бы unwrap исключение, и **OnException** получит базовое исключение. В 2.1, фильтр исключений не unwrap, и **OnException** возвращает исходный **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Исправления ошибок

Этот выпуск также включает несколько исправлений ошибок. Можно найти полный список здесь:

- [5.1.0 пакет](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 пакет](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 пакет содержит обновления IntelliSense, но не исправления ошибок.
