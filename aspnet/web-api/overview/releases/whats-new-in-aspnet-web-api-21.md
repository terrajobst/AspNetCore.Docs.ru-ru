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
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="3df60-102">Новые возможности в ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="3df60-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="3df60-103">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3df60-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="3df60-104">В этом разделе описываются новые возможности в ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="3df60-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="3df60-105">Скачать</span><span class="sxs-lookup"><span data-stu-id="3df60-105">Download</span></span>](#download)
- [<span data-ttu-id="3df60-106">Документация</span><span class="sxs-lookup"><span data-stu-id="3df60-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="3df60-107">Новые возможности в ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="3df60-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="3df60-108">Обработка ошибок, глобальные</span><span class="sxs-lookup"><span data-stu-id="3df60-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="3df60-109">Атрибут улучшения маршрутизации</span><span class="sxs-lookup"><span data-stu-id="3df60-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="3df60-110">Усовершенствования страницы справки</span><span class="sxs-lookup"><span data-stu-id="3df60-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="3df60-111">Поддержка IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="3df60-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="3df60-112">BSON форматирования типа мультимедиа</span><span class="sxs-lookup"><span data-stu-id="3df60-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="3df60-113">Улучшенная поддержка асинхронного фильтры</span><span class="sxs-lookup"><span data-stu-id="3df60-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="3df60-114">Запрос анализа для форматирования библиотеки клиента</span><span class="sxs-lookup"><span data-stu-id="3df60-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="3df60-115">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="3df60-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="3df60-116">Исправления ошибок</span><span class="sxs-lookup"><span data-stu-id="3df60-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="3df60-117">Скачать</span><span class="sxs-lookup"><span data-stu-id="3df60-117">Download</span></span>

<span data-ttu-id="3df60-118">Компоненты среды выполнения выпущены в виде пакетов NuGet при галереи NuGet.</span><span class="sxs-lookup"><span data-stu-id="3df60-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="3df60-119">Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации.</span><span class="sxs-lookup"><span data-stu-id="3df60-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="3df60-120">Последнюю версию пакета ASP.NET Web API 2.1 RTM имеет следующую версию: «5.1.2».</span><span class="sxs-lookup"><span data-stu-id="3df60-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="3df60-121">Можно установить или обновить эти пакеты через [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="3df60-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="3df60-122">Этот выпуск также включает соответствующие локализованные пакеты в NuGet.</span><span class="sxs-lookup"><span data-stu-id="3df60-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="3df60-123">Можно установить или обновить выпущенных пакетов NuGet, используя консоль диспетчера пакетов NuGet:</span><span class="sxs-lookup"><span data-stu-id="3df60-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="3df60-124">Документация</span><span class="sxs-lookup"><span data-stu-id="3df60-124">Documentation</span></span>

<span data-ttu-id="3df60-125">Учебники и другие сведения о ASP.NET Web API 2.1 RTM доступны из веб-сайт ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="3df60-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="3df60-126">Новые возможности в ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="3df60-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="3df60-127">Обработка ошибок, глобальные</span><span class="sxs-lookup"><span data-stu-id="3df60-127">Global Error Handling</span></span>

<span data-ttu-id="3df60-128">Все необработанные исключения, сейчас будет выполнен через один механизм центра, и можно настроить поведение для необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="3df60-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="3df60-129">Платформа поддерживает несколько средств ведения журнала исключений, которые необработанное исключение и сведения о контексте, в котором она произошла, например при обработке запроса.</span><span class="sxs-lookup"><span data-stu-id="3df60-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="3df60-130">Например следующий код использует System.Diagnostics.TraceSource в журнал все необработанные исключения:</span><span class="sxs-lookup"><span data-stu-id="3df60-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="3df60-131">Также можно заменить обработчику исключений по умолчанию, так что можно полностью настроить сообщения HTTP-ответа, который отправляется, когда необработанное исключение возникает.</span><span class="sxs-lookup"><span data-stu-id="3df60-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="3df60-132">Мы предоставили [пример](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , регистрирует все необработанные исключения через популярные ELMAH framework.</span><span class="sxs-lookup"><span data-stu-id="3df60-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="3df60-133">Атрибут улучшения маршрутизации</span><span class="sxs-lookup"><span data-stu-id="3df60-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="3df60-134">Атрибут маршрутизации теперь поддерживает ограничения, Включение управления версиями и выбора маршрутов на основе заголовка.</span><span class="sxs-lookup"><span data-stu-id="3df60-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="3df60-135">Кроме того, многие аспекты маршруты атрибута теперь настраиваются через **IDirectRouteFactory** интерфейс и **RouteFactoryAttribute** класса.</span><span class="sxs-lookup"><span data-stu-id="3df60-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="3df60-136">Префикс маршрута теперь можно расширять посредством **IRoutePrefix** интерфейс и **RoutePrefixAttribute** класса.</span><span class="sxs-lookup"><span data-stu-id="3df60-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="3df60-137">Мы предоставили [пример](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) , использующий ограничения для динамической фильтрации контроллеров с заголовком HTTP «api-version».</span><span class="sxs-lookup"><span data-stu-id="3df60-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="3df60-138">Усовершенствования страницы справки</span><span class="sxs-lookup"><span data-stu-id="3df60-138">Help Page Improvements</span></span>

<span data-ttu-id="3df60-139">Веб-API 2.1 включает следующие усовершенствования для [страницах справки API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="3df60-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="3df60-140">Документация отдельные свойства параметров или возвращаемых типов действий.</span><span class="sxs-lookup"><span data-stu-id="3df60-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="3df60-141">Документация заметки модели данных.</span><span class="sxs-lookup"><span data-stu-id="3df60-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="3df60-142">Разработки пользовательского интерфейса страниц справки также был изменен, в соответствии с этими изменениями.</span><span class="sxs-lookup"><span data-stu-id="3df60-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="3df60-143">Поддержка IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="3df60-143">IgnoreRoute Support</span></span>

<span data-ttu-id="3df60-144">Веб-API 2.1 поддерживает игнорирование шаблонов URL-адресов в веб-API маршрутизации через набор **IgnoreRoute** методы расширения в **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="3df60-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="3df60-145">Эти методы вызывают веб-API игнорировать все URL-адреса, которые соответствуют указанным шаблоном и благодаря этому узел может применить дополнительную обработку при необходимости.</span><span class="sxs-lookup"><span data-stu-id="3df60-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="3df60-146">Следующий пример игнорирует идентификаторы URI, начинающихся с &quot;содержимого&quot; сегмента:</span><span class="sxs-lookup"><span data-stu-id="3df60-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="3df60-147">BSON форматирования типа мультимедиа</span><span class="sxs-lookup"><span data-stu-id="3df60-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="3df60-148">Веб-API теперь поддерживает [BSON](http://bsonspec.org/) формат для передачи как на клиенте, так и на сервере.</span><span class="sxs-lookup"><span data-stu-id="3df60-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="3df60-149">Чтобы включить BSON на стороне сервера, добавить **BsonMediaTypeFormatter** в коллекцию модулей форматирования:</span><span class="sxs-lookup"><span data-stu-id="3df60-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="3df60-150">Вот, как клиент .NET могут использовать формат BSON.</span><span class="sxs-lookup"><span data-stu-id="3df60-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="3df60-151">Мы предоставили [пример](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) , показывающий клиента и на сервере.</span><span class="sxs-lookup"><span data-stu-id="3df60-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="3df60-152">Дополнительные сведения см. в разделе [поддержки BSON в Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="3df60-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="3df60-153">Улучшенная поддержка асинхронного фильтры</span><span class="sxs-lookup"><span data-stu-id="3df60-153">Better Support for Async Filters</span></span>

<span data-ttu-id="3df60-154">Веб-API теперь поддерживает простой способ создать фильтры, которые выполняются асинхронно.</span><span class="sxs-lookup"><span data-stu-id="3df60-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="3df60-155">Эта возможность полезна является фильтра необходимо для выполнения асинхронного действия, например доступа базы данных.</span><span class="sxs-lookup"><span data-stu-id="3df60-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="3df60-156">Ранее Чтобы создать фильтр async, приходилось реализовать интерфейс фильтра, так как базовые классы фильтра доступными только для синхронных методов.</span><span class="sxs-lookup"><span data-stu-id="3df60-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="3df60-157">Теперь можно переопределить виртуальный `On*Async` методы фильтра базового класса.</span><span class="sxs-lookup"><span data-stu-id="3df60-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="3df60-158">Пример:</span><span class="sxs-lookup"><span data-stu-id="3df60-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="3df60-159">**AuthorizationFilterAttribute**, **ActionFilterAttribute**, и **ExceptionFilterAttribute** все классы поддержки async в Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="3df60-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="3df60-160">Запрос анализа для форматирования библиотеки клиента</span><span class="sxs-lookup"><span data-stu-id="3df60-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="3df60-161">Ранее **System.Net.Http.Formatting** поддерживается синтаксический анализ и обновление запросов URI для кода на стороне сервера, но эквивалентные переносимой библиотеки отсутствует эта функция.</span><span class="sxs-lookup"><span data-stu-id="3df60-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="3df60-162">В Web API 2.1 клиентское приложение может теперь легко проанализировать и измените строку запроса.</span><span class="sxs-lookup"><span data-stu-id="3df60-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="3df60-163">Следующие примеры показывают, как анализировать, изменять и создавать запросы URI.</span><span class="sxs-lookup"><span data-stu-id="3df60-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="3df60-164">(В примерах консольное приложение для простоты.)</span><span class="sxs-lookup"><span data-stu-id="3df60-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="3df60-165">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="3df60-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="3df60-166">В этом разделе описываются известные проблемы и критические изменения в ASP.NET Web API 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="3df60-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="3df60-167">Атрибут маршрутизации</span><span class="sxs-lookup"><span data-stu-id="3df60-167">Attribute Routing</span></span>

<span data-ttu-id="3df60-168">Неоднозначность в маршрутизации соответствует атрибута теперь отчетов ошибку вместо выбора первого совпадения.</span><span class="sxs-lookup"><span data-stu-id="3df60-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="3df60-169">Маршруты атрибутов не могут использовать *{controller}* параметра и с помощью *{action}* параметр маршрутов разместить на действия.</span><span class="sxs-lookup"><span data-stu-id="3df60-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="3df60-170">Эти параметры большой вероятностью приведет к неоднозначности.</span><span class="sxs-lookup"><span data-stu-id="3df60-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="3df60-171">API MVC или веб-формирования шаблонов в проект 5.1 пакетов приводит к получению 5.0 пакетов для те, которые уже не существуют в проекте</span><span class="sxs-lookup"><span data-stu-id="3df60-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="3df60-172">Обновление пакетов NuGet для ASP.NET Web API 2.1 RTM не обновляет набора средств Visual Studio, например формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3df60-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="3df60-173">Они используют предыдущую версию пакета среды выполнения ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="3df60-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="3df60-174">В результате формирование шаблонов ASP.NET установит предыдущую версию (5.0.0.0) необходимые пакеты, если они отсутствуют доступные в проектах.</span><span class="sxs-lookup"><span data-stu-id="3df60-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="3df60-175">Формирование шаблонов ASP.NET в Visual Studio 2013 RTM или обновлением 1 не перезаписывает последние пакеты в проекте.</span><span class="sxs-lookup"><span data-stu-id="3df60-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="3df60-176">Если вы используете формирование шаблонов ASP.NET после обновления пакетов Web API 2.1 или ASP.NET MVC 5.1, убедитесь, что версии веб-API и MVC согласованы.</span><span class="sxs-lookup"><span data-stu-id="3df60-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="3df60-177">Переименование типа</span><span class="sxs-lookup"><span data-stu-id="3df60-177">Type Renames</span></span>

<span data-ttu-id="3df60-178">Некоторые типы, используемые для расширения маршрутизации атрибута были переименованы версий-Кандидатов 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="3df60-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="3df60-179">Старое имя типа (2.1 версия-Кандидат)</span><span class="sxs-lookup"><span data-stu-id="3df60-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="3df60-180">Введите новое имя (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="3df60-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="3df60-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="3df60-181">IDirectRouteProvider</span></span> | <span data-ttu-id="3df60-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="3df60-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="3df60-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="3df60-183">RouteProviderAttribute</span></span> | <span data-ttu-id="3df60-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="3df60-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="3df60-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="3df60-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="3df60-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="3df60-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="3df60-187">Фильтры исключений не распаковки агрегатные исключения, создаваемые асинхронные действия</span><span class="sxs-lookup"><span data-stu-id="3df60-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="3df60-188">Ранее Если задача создала асинхронного действия **AggregateException**, фильтр исключений будет разворачивать исключение, и **OnException** бы получить базовое исключение.</span><span class="sxs-lookup"><span data-stu-id="3df60-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="3df60-189">В версии 2.1, фильтра исключений не разворачивать ее, и **OnException** возвращает исходный **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="3df60-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="3df60-190">Исправления ошибок</span><span class="sxs-lookup"><span data-stu-id="3df60-190">Bug Fixes</span></span>

<span data-ttu-id="3df60-191">Этот выпуск также включает несколько исправлений ошибок.</span><span class="sxs-lookup"><span data-stu-id="3df60-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="3df60-192">Можно найти полный список здесь:</span><span class="sxs-lookup"><span data-stu-id="3df60-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="3df60-193">5.1.0 пакет</span><span class="sxs-lookup"><span data-stu-id="3df60-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="3df60-194">5.1.1 пакета</span><span class="sxs-lookup"><span data-stu-id="3df60-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="3df60-195">5.1.2 пакет содержит обновления IntelliSense, но без исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="3df60-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
