---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Новые возможности в ASP.NET Web API 2.2 | Документация Майкрософт
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 0f0794988da712897092ab808a08fca5eeebb6d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813282"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="e6cd8-102">Новые возможности в ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="e6cd8-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="e6cd8-103">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e6cd8-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="e6cd8-104">В этом разделе описываются новые возможности ASP.NET веб-API 2.2.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="e6cd8-105">Скачать</span><span class="sxs-lookup"><span data-stu-id="e6cd8-105">Download</span></span>](#download)
- [<span data-ttu-id="e6cd8-106">Документация</span><span class="sxs-lookup"><span data-stu-id="e6cd8-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="e6cd8-107">Новые возможности в ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="e6cd8-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="e6cd8-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="e6cd8-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="e6cd8-109">Атрибут улучшения маршрутизации</span><span class="sxs-lookup"><span data-stu-id="e6cd8-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="e6cd8-110">Поддержка веб-API клиента Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="e6cd8-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="e6cd8-111">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="e6cd8-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="e6cd8-112">Исправления ошибок</span><span class="sxs-lookup"><span data-stu-id="e6cd8-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="e6cd8-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="e6cd8-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="e6cd8-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="e6cd8-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="e6cd8-115">Бета-версии Microsoft.AspNet.WebAPI 5.2.3</span><span class="sxs-lookup"><span data-stu-id="e6cd8-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="e6cd8-116">Скачать</span><span class="sxs-lookup"><span data-stu-id="e6cd8-116">Download</span></span>

<span data-ttu-id="e6cd8-117">Компоненты среды выполнения выпускаются в виде пакетов NuGet из коллекции NuGet.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="e6cd8-118">Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="e6cd8-119">Последнюю версию пакета ASP.NET веб-API 2.2 имеет следующую версию: «5.2.0».</span><span class="sxs-lookup"><span data-stu-id="e6cd8-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="e6cd8-120">Вы можете установить или обновить эти пакеты с помощью [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="e6cd8-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="e6cd8-121">Этот выпуск также включает соответствующие локализованные пакеты в NuGet.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="e6cd8-122">Можно установить или обновить на выпущенных пакетов NuGet с помощью консоли диспетчера пакетов NuGet:</span><span class="sxs-lookup"><span data-stu-id="e6cd8-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="e6cd8-123">Документация</span><span class="sxs-lookup"><span data-stu-id="e6cd8-123">Documentation</span></span>

<span data-ttu-id="e6cd8-124">Учебники и другие сведения об ASP.NET Web API 2.2 доступны на веб-сайте ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="e6cd8-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="e6cd8-125">Новые возможности в ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="e6cd8-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="e6cd8-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="e6cd8-126">OData v4</span></span>

<span data-ttu-id="e6cd8-127">Этот выпуск добавляет поддержку для протокола OData версии 4.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="e6cd8-128">Дополнительные сведения см. в разделе [документации веб-API OData v4.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="e6cd8-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="e6cd8-129">Ниже приведены некоторые основные возможности и изменения в OData v4:</span><span class="sxs-lookup"><span data-stu-id="e6cd8-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="e6cd8-130">Поддержка псевдонимов свойств в модели OData</span><span class="sxs-lookup"><span data-stu-id="e6cd8-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="e6cd8-131">Поддержка ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute и ConcurrencyCheckAttribute в ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="e6cd8-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="e6cd8-132">Предоставляют возможность указать понятное название для действий</span><span class="sxs-lookup"><span data-stu-id="e6cd8-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="e6cd8-133">Интеграция с ODL UriParser</span><span class="sxs-lookup"><span data-stu-id="e6cd8-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="e6cd8-134">Поддержка [перечисления](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [вложения](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) и [одноэлементный](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="e6cd8-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="e6cd8-135">Поддержка приведения для типов-примитивов</span><span class="sxs-lookup"><span data-stu-id="e6cd8-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="e6cd8-136">Добавлена поддержка функции OData</span><span class="sxs-lookup"><span data-stu-id="e6cd8-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="e6cd8-137">Поддерживает псевдонимы параметра для вызовов функций</span><span class="sxs-lookup"><span data-stu-id="e6cd8-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="e6cd8-138">Поддерживает camel case соглашение об именовании в модели</span><span class="sxs-lookup"><span data-stu-id="e6cd8-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="e6cd8-139">Поддержка cast() в $filter</span><span class="sxs-lookup"><span data-stu-id="e6cd8-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="e6cd8-140">Поддержка открытых сложного типа</span><span class="sxs-lookup"><span data-stu-id="e6cd8-140">Support for open complex type</span></span>
- <span data-ttu-id="e6cd8-141">Удален EntitySetController и AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="e6cd8-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="e6cd8-142">Измененные $link для $ref</span><span class="sxs-lookup"><span data-stu-id="e6cd8-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="e6cd8-143">Добавлена поддержка маршрутизации атрибутов</span><span class="sxs-lookup"><span data-stu-id="e6cd8-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="e6cd8-144">Использует библиотеки OData Core 6.4.0</span><span class="sxs-lookup"><span data-stu-id="e6cd8-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="e6cd8-145">Атрибут улучшения маршрутизации</span><span class="sxs-lookup"><span data-stu-id="e6cd8-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="e6cd8-146">Маршрутизация с помощью атрибутов теперь предоставляет точку расширения, вызывается IDirectRouteProvider, что позволяет полностью контролировать принципы обнаружения и настроить маршруты с атрибутами.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="e6cd8-147">IDirectRouteProvider отвечает за предоставление списка действия и контроллеры вместе с информацией о связанного маршрута для указания точности для этих действий требуется какие конфигурации маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="e6cd8-148">Реализация IDirectRouteProvider могут быть указаны при вызове MapAttributes/MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="e6cd8-149">Настройка IDirectRouteProvider будет простым путем расширения наших реализация по умолчанию, DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="e6cd8-150">Этот класс предоставляет отдельный переопределяемый виртуальные методы, чтобы изменить логику для обнаружения атрибутов, создание записи маршрута и обнаружения префикс маршрута и префикс области.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="e6cd8-151">Ниже приведены некоторые примеры, на что можно сделать с помощью этой новой точке расширения.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="e6cd8-152">Поддержка наследования атрибутов маршрута</span><span class="sxs-lookup"><span data-stu-id="e6cd8-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="e6cd8-153">Пример</span><span class="sxs-lookup"><span data-stu-id="e6cd8-153">Example:</span></span>

    <span data-ttu-id="e6cd8-154">Здесь запрос like «/ 10/api/значений» успешно возвратит «Успех: 10»</span><span class="sxs-lookup"><span data-stu-id="e6cd8-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="e6cd8-155">Предоставить имя маршрута по умолчанию для атрибутов маршрутов, следуя некоторые соглашение, которое вам нравится.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="e6cd8-156">По умолчанию маршрутизации с помощью атрибутов не создает автоматически имена для маршрутов на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="e6cd8-157">Измените шаблон маршрута атрибут маршрутов в одном месте, прежде чем они заключаются в таблице маршрутов.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="e6cd8-158">Поддержка веб-API клиента Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="e6cd8-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="e6cd8-159">Теперь можно использовать пакет NuGet клиента Web API для реализации логики клиента веб-API, при разработке для Windows Phone 8.1 или из в универсальное приложение.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="e6cd8-160">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="e6cd8-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="e6cd8-161">В этом разделе описываются известные проблемы и критические изменения в веб-API ASP.NET 2.2.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="e6cd8-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="e6cd8-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="e6cd8-163">Построитель модели</span><span class="sxs-lookup"><span data-stu-id="e6cd8-163">Model builder</span></span>

<span data-ttu-id="e6cd8-164">Проблема: Перегруженные функции не может быть представлено как FunctionImport</span><span class="sxs-lookup"><span data-stu-id="e6cd8-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="e6cd8-165">При наличии 2 перегруженные функции, и они также FunctionImport, как показано ниже, затем запросив результаты ~/GetAllConventionCustomers(CustomerName={customerName}) в System.InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="e6cd8-166">Инструкции по решению: Возможным решением этой проблемы является добавление перегрузок функций как FunctionImports.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="e6cd8-167">Маршрутизация OData</span><span class="sxs-lookup"><span data-stu-id="e6cd8-167">OData Routing</span></span>

<span data-ttu-id="e6cd8-168">Строковые литералы, которые включают URL-адрес в кодировке косой черты (% 2F), а backslash(%5C) вызвать ошибку 404, если они используются в пути к ресурсам OData.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="e6cd8-169">Например строковые литералы можно использовать в пути к ресурсам OData как параметры функций или значения ключа наборов сущностей.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="e6cd8-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="e6cd8-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="e6cd8-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="e6cd8-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="e6cd8-172">Когда службы получают такие запросы узлов будет un-escape те escape-последовательность перед их передачей в среду выполнения веб-API.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="e6cd8-173">Это обеспечивает защиту от атак, таких как следующие:</span><span class="sxs-lookup"><span data-stu-id="e6cd8-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="e6cd8-174">В результате в стеке веб-API OData в сообщение об ошибке 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="e6cd8-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="e6cd8-175">Чтобы предотвратить эту ошибку, ваш клиент должен использовать двойные escape-последовательности для косой черты (% 252F) и обратную косую черту (% 255C).</span><span class="sxs-lookup"><span data-stu-id="e6cd8-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="e6cd8-176">Этого не происходит для строк запросов, например /Employees? $filter = имя eq «% 2F имя»</span><span class="sxs-lookup"><span data-stu-id="e6cd8-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="e6cd8-177">**Обратите внимание, без escape-символы косой черты (/) и символов обратной косой черты ("") не допустимы в OData ресурс путь строковых литералах. Черты должны отображаться только как разделители путей и символов обратной косой черты не должен появляться в пути к ресурсу OData вообще. (Оба могут использоваться в некоторые части строку запроса OData).**</span><span class="sxs-lookup"><span data-stu-id="e6cd8-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="e6cd8-178">Инструкции по решению: Можно переопределить метод Parse DefaultODataPathHandler для экранирования перед анализом фактически косой черты и обратной косой черты в строковых литералах.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="e6cd8-179">Вы найдете пример по решению этой задачи.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="e6cd8-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="e6cd8-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="e6cd8-181">[Поддерживает запросы]</span><span class="sxs-lookup"><span data-stu-id="e6cd8-181">[Queryable]</span></span>

<span data-ttu-id="e6cd8-182">Атрибут [Queryable] является устаревшим.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="e6cd8-183">Новые приложения OData v3 должны использовать **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="e6cd8-184">**ODataHttpConfigurationExtensions.EnableQuerySupport** теперь добавляет метод расширения **EnableQueryAttribute** глобальную коллекцию фильтров.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="e6cd8-185">Если какие-либо контроллеры **[Queryable]** атрибут, вызвав `config.EnableQuerySupport()` вызовет **[Queryable]** атрибут переход на другой</span><span class="sxs-lookup"><span data-stu-id="e6cd8-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="e6cd8-186">Для устранения этой проблемы рекомендуется для замены всех экземпляров **QueryableAttribute** с **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="e6cd8-187">Второе решение — использовать следующий код в конфигурации веб-API:</span><span class="sxs-lookup"><span data-stu-id="e6cd8-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="e6cd8-188">Маршрутизация с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="e6cd8-188">Attribute Routing</span></span>

<span data-ttu-id="e6cd8-189">Проблема: Привязки модели сложного типа, который дополняется атрибутом FromUri ведет себя по-разному при использовании маршрутизации с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="e6cd8-190">Следующую ссылку отслеживает проблемы, а также содержит подробные сведения по решению этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="e6cd8-191">Проблема: Формирование шаблонов MVC и веб-API в проект без 5.2.0 пакетов приводит 5.1.2 пакеты для тех, которые еще не существуют в проекте</span><span class="sxs-lookup"><span data-stu-id="e6cd8-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="e6cd8-192">Обновление пакетов NuGet для ASP.NET MVC 5.2 не обновить инструменты Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="e6cd8-193">Они используют предыдущую версию пакетов среды выполнения ASP.NET (например 5.1.2 в обновлении 2).</span><span class="sxs-lookup"><span data-stu-id="e6cd8-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="e6cd8-194">Таким образом формирование шаблонов ASP.NET установит предыдущую версию (например 5.1.2 в обновлении 2) необходимые пакеты, если они еще не доступны в проектах.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="e6cd8-195">Тем не менее формирование шаблонов ASP.NET в Visual Studio 2013 RTM или с обновлением 1 не перезаписывает последних пакетов в проектах.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="e6cd8-196">Если вы используете формирование шаблонов ASP.NET после обновления пакеты проектов веб-API 2.2 или ASP.NET MVC 5.2, убедитесь, что в версиях веб-API и ASP.NET MVC являются согласованными.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="e6cd8-197">Исправления ошибок и незначительные обновления</span><span class="sxs-lookup"><span data-stu-id="e6cd8-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="e6cd8-198">Этот выпуск также включает несколько исправлений ошибок, а также дополнительный компонент обновления.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="e6cd8-199">Можно найти полный список здесь:</span><span class="sxs-lookup"><span data-stu-id="e6cd8-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="e6cd8-200">5.2 пакета</span><span class="sxs-lookup"><span data-stu-id="e6cd8-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="e6cd8-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="e6cd8-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="e6cd8-202">Microsoft.AspNet.OData 5.2.1 пакет содержит обновление зависимостей NuGet, но не исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="e6cd8-203">Это обновление больше не существует строгая зависимость на Microsoft.OData.Core 6.4.0, но один можно обновить до любой версии между 6.4.0 и 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="e6cd8-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="e6cd8-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="e6cd8-205">В этом выпуске мы изменили зависимость для `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="e6cd8-206">Дополнительные сведения о новинках в этом выпуске `Json.NET`, см. в разделе [Json.NET 6.0 выпуск 4 — слияние JSON, внедрение зависимостей](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e6cd8-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="e6cd8-207">Этот выпуск не содержит других новых функций или исправления ошибок в веб-API.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="e6cd8-208">Впоследствии мы обновили все наши связанные пакеты, которые зависят от этой новой версии веб-API.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="e6cd8-209">Бета-версии Microsoft.AspNet.WebAPI 5.2.3</span><span class="sxs-lookup"><span data-stu-id="e6cd8-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="e6cd8-210">Читайте о выпуске [здесь](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6cd8-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="e6cd8-211">Этот выпуск содержит только исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-211">This release contains only bug fixes.</span></span> <span data-ttu-id="e6cd8-212">Можно использовать [этот запрос](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) для просмотра списка проблем, исправленных в этом выпуске.</span><span class="sxs-lookup"><span data-stu-id="e6cd8-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
