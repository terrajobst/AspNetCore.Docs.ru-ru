---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Новые возможности в ASP.NET Web API 2.1 | Документы Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>Новые возможности в ASP.NET Web API 2.1
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описываются новые возможности в ASP.NET Web API 2.1.

- [Скачать](#download)
- [Документация](#documentation)
- [Новые возможности в ASP.NET Web API 2.1](#new-features)

    - [Обработка ошибок, глобальные](#global-error)
    - [Атрибут улучшения маршрутизации](#attribute-routing)
    - [Усовершенствования страницы справки](#help-page)
    - [Поддержка IgnoreRoute](#ignoreroute)
    - [BSON форматирования типа мультимедиа](#bson)
    - [Улучшенная поддержка асинхронного фильтры](#async-filters)
    - [Запрос анализа для форматирования библиотеки клиента](#query-parsing)
- [Известные проблемы и критические изменения](#known-issues)
- [Исправления ошибок](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Скачать

Компоненты среды выполнения выпущены в виде пакетов NuGet при галереи NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Последнюю версию пакета ASP.NET Web API 2.1 RTM имеет следующую версию: «5.1.2». Можно установить или обновить эти пакеты через [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить выпущенных пакетов NuGet, используя консоль диспетчера пакетов NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения о ASP.NET Web API 2.1 RTM доступны из веб-сайт ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Новые возможности в ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Обработка ошибок, глобальные

Все необработанные исключения, сейчас будет выполнен через один механизм центра, и можно настроить поведение для необработанных исключений.

Платформа поддерживает несколько средств ведения журнала исключений, которые необработанное исключение и сведения о контексте, в котором она произошла, например при обработке запроса.

Например следующий код использует System.Diagnostics.TraceSource в журнал все необработанные исключения:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Также можно заменить обработчику исключений по умолчанию, так что можно полностью настроить сообщения HTTP-ответа, который отправляется, когда необработанное исключение возникает.

Мы предоставили [пример](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , регистрирует все необработанные исключения через популярные ELMAH framework.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Атрибут улучшения маршрутизации

Атрибут маршрутизации теперь поддерживает ограничения, Включение управления версиями и выбора маршрутов на основе заголовка. Кроме того, многие аспекты маршруты атрибута теперь настраиваются через **IDirectRouteFactory** интерфейс и **RouteFactoryAttribute** класса. Префикс маршрута теперь можно расширять посредством **IRoutePrefix** интерфейс и **RoutePrefixAttribute** класса.

Мы предоставили [пример](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) , использующий ограничения для динамической фильтрации контроллеров с заголовком HTTP «api-version».

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Усовершенствования страницы справки

Веб-API 2.1 включает следующие усовершенствования для [страницах справки API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Документация отдельные свойства параметров или возвращаемых типов действий.
- Документация заметки модели данных.

Разработки пользовательского интерфейса страниц справки также был изменен, в соответствии с этими изменениями.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Поддержка IgnoreRoute

Веб-API 2.1 поддерживает игнорирование шаблонов URL-адресов в веб-API маршрутизации через набор **IgnoreRoute** методы расширения в **HttpRouteCollection**. Эти методы вызывают веб-API игнорировать все URL-адреса, которые соответствуют указанным шаблоном и благодаря этому узел может применить дополнительную обработку при необходимости.

Следующий пример игнорирует идентификаторы URI, начинающихся с &quot;содержимого&quot; сегмента:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON форматирования типа мультимедиа

Веб-API теперь поддерживает [BSON](http://bsonspec.org/) формат для передачи как на клиенте, так и на сервере.

Чтобы включить BSON на стороне сервера, добавить **BsonMediaTypeFormatter** в коллекцию модулей форматирования:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Вот, как клиент .NET могут использовать формат BSON.

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Мы предоставили [пример](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) , показывающий клиента и на сервере.

Дополнительные сведения см. в разделе [поддержки BSON в Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Улучшенная поддержка асинхронного фильтры

Веб-API теперь поддерживает простой способ создать фильтры, которые выполняются асинхронно. Эта возможность полезна является фильтра необходимо для выполнения асинхронного действия, например доступа базы данных. Ранее Чтобы создать фильтр async, приходилось реализовать интерфейс фильтра, так как базовые классы фильтра доступными только для синхронных методов. Теперь можно переопределить виртуальный `On*Async` методы фильтра базового класса.

Пример:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**, **ActionFilterAttribute**, и **ExceptionFilterAttribute** все классы поддержки async в Web API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Запрос анализа для форматирования библиотеки клиента

Ранее **System.Net.Http.Formatting** поддерживается синтаксический анализ и обновление запросов URI для кода на стороне сервера, но эквивалентные переносимой библиотеки отсутствует эта функция. В Web API 2.1 клиентское приложение может теперь легко проанализировать и измените строку запроса.

Следующие примеры показывают, как анализировать, изменять и создавать запросы URI. (В примерах консольное приложение для простоты.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

В этом разделе описываются известные проблемы и критические изменения в ASP.NET Web API 2.1 RTM.

### <a name="attribute-routing"></a>Атрибут маршрутизации

Неоднозначность в маршрутизации соответствует атрибута теперь отчетов ошибку вместо выбора первого совпадения.

Маршруты атрибутов не могут использовать *{controller}* параметра и с помощью *{action}* параметр маршрутов разместить на действия. Эти параметры большой вероятностью приведет к неоднозначности.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>API MVC или веб-формирования шаблонов в проект 5.1 пакетов приводит к получению 5.0 пакетов для те, которые уже не существуют в проекте

Обновление пакетов NuGet для ASP.NET Web API 2.1 RTM не обновляет набора средств Visual Studio, например формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET. Они используют предыдущую версию пакета среды выполнения ASP.NET (5.0.0.0). В результате формирование шаблонов ASP.NET установит предыдущую версию (5.0.0.0) необходимые пакеты, если они отсутствуют доступные в проектах. Формирование шаблонов ASP.NET в Visual Studio 2013 RTM или обновлением 1 не перезаписывает последние пакеты в проекте.

Если вы используете формирование шаблонов ASP.NET после обновления пакетов Web API 2.1 или ASP.NET MVC 5.1, убедитесь, что версии веб-API и MVC согласованы.

### <a name="type-renames"></a>Переименование типа

Некоторые типы, используемые для расширения маршрутизации атрибута были переименованы версий-Кандидатов 2.1 RTM.

| Старое имя типа (2.1 версия-Кандидат) | Введите новое имя (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Фильтры исключений не распаковки агрегатные исключения, создаваемые асинхронные действия

Ранее Если задача создала асинхронного действия **AggregateException**, фильтр исключений будет разворачивать исключение, и **OnException** бы получить базовое исключение. В версии 2.1, фильтра исключений не разворачивать ее, и **OnException** возвращает исходный **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Исправления ошибок

Этот выпуск также включает несколько исправлений ошибок. Можно найти полный список здесь:

- [5.1.0 пакет](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 пакета](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 пакет содержит обновления IntelliSense, но без исправления ошибок.
