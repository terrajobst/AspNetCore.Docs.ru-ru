---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "Настройка ASP.NET Web API 2 | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a><span data-ttu-id="4de61-102">Настройка ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="4de61-102">Configuring ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="4de61-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4de61-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4de61-104">В этом разделе описываются способы настройки веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4de61-104">This topic describes how to configure ASP.NET Web API.</span></span>

- [<span data-ttu-id="4de61-105">Параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="4de61-105">Configuration Settings</span></span>](#settings)
- [<span data-ttu-id="4de61-106">Настройка веб-API с размещением ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4de61-106">Configuring Web API with ASP.NET Hosting</span></span>](#webhost)
- [<span data-ttu-id="4de61-107">Настройка веб-API с резидентной OWIN</span><span class="sxs-lookup"><span data-stu-id="4de61-107">Configuring Web API with OWIN Self-Hosting</span></span>](#selfhost)
- [<span data-ttu-id="4de61-108">Глобальные веб-API службы</span><span class="sxs-lookup"><span data-stu-id="4de61-108">Global Web API Services</span></span>](#services)
- [<span data-ttu-id="4de61-109">Настройка контроллера</span><span class="sxs-lookup"><span data-stu-id="4de61-109">Per-Controller Configuration</span></span>](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a><span data-ttu-id="4de61-110">Параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="4de61-110">Configuration Settings</span></span>

<span data-ttu-id="4de61-111">Определенные параметры конфигурации веб-API в [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) класса.</span><span class="sxs-lookup"><span data-stu-id="4de61-111">Web API configuration setttings are defined in the [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) class.</span></span>

| <span data-ttu-id="4de61-112">Член</span><span class="sxs-lookup"><span data-stu-id="4de61-112">Member</span></span> | <span data-ttu-id="4de61-113">Описание:</span><span class="sxs-lookup"><span data-stu-id="4de61-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4de61-114">**DependencyResolver**</span><span class="sxs-lookup"><span data-stu-id="4de61-114">**DependencyResolver**</span></span> | <span data-ttu-id="4de61-115">Позволяет внедрения зависимостей для контроллеров.</span><span class="sxs-lookup"><span data-stu-id="4de61-115">Enables dependency injection for controllers.</span></span> <span data-ttu-id="4de61-116">В разделе [с помощью сопоставителя зависимостей веб-API](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-116">See [Using the Web API Dependency Resolver](dependency-injection.md).</span></span> |
| <span data-ttu-id="4de61-117">**Фильтры**</span><span class="sxs-lookup"><span data-stu-id="4de61-117">**Filters**</span></span> | <span data-ttu-id="4de61-118">Фильтры действий.</span><span class="sxs-lookup"><span data-stu-id="4de61-118">Action filters.</span></span> |
| <span data-ttu-id="4de61-119">**Formatters**</span><span class="sxs-lookup"><span data-stu-id="4de61-119">**Formatters**</span></span> | <span data-ttu-id="4de61-120">[Модули форматирования типа мультимедиа](../formats-and-model-binding/media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-120">[Media-type formatters](../formats-and-model-binding/media-formatters.md).</span></span> |
| <span data-ttu-id="4de61-121">**IncludeErrorDetailPolicy**</span><span class="sxs-lookup"><span data-stu-id="4de61-121">**IncludeErrorDetailPolicy**</span></span> | <span data-ttu-id="4de61-122">Указывает, должен ли сервер включать сведения об ошибке, например сообщения исключений и трассировки стека в сообщений ответов HTTP.</span><span class="sxs-lookup"><span data-stu-id="4de61-122">Specifies whether the server should include error details, such as exception messages and stack traces, in HTTP response messages.</span></span> <span data-ttu-id="4de61-123">В разделе [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span><span class="sxs-lookup"><span data-stu-id="4de61-123">See [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span></span> |
| <span data-ttu-id="4de61-124">**Инициализатор**</span><span class="sxs-lookup"><span data-stu-id="4de61-124">**Initializer**</span></span> | <span data-ttu-id="4de61-125">Функция, которая выполняет окончательной инициализации **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="4de61-125">A function that performs final initialization of the **HttpConfiguration**.</span></span> |
| <span data-ttu-id="4de61-126">**MessageHandlers**</span><span class="sxs-lookup"><span data-stu-id="4de61-126">**MessageHandlers**</span></span> | <span data-ttu-id="4de61-127">[Обработчики сообщений HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-127">[HTTP message handlers](http-message-handlers.md).</span></span> |
| <span data-ttu-id="4de61-128">**ParameterBindingRules**</span><span class="sxs-lookup"><span data-stu-id="4de61-128">**ParameterBindingRules**</span></span> | <span data-ttu-id="4de61-129">Коллекции правил привязки параметров действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="4de61-129">A collection of rules for binding parameters on controller actions.</span></span> |
| <span data-ttu-id="4de61-130">**Свойства**</span><span class="sxs-lookup"><span data-stu-id="4de61-130">**Properties**</span></span> | <span data-ttu-id="4de61-131">Универсальное хранилище свойств.</span><span class="sxs-lookup"><span data-stu-id="4de61-131">A generic property bag.</span></span> |
| <span data-ttu-id="4de61-132">**Маршруты**</span><span class="sxs-lookup"><span data-stu-id="4de61-132">**Routes**</span></span> | <span data-ttu-id="4de61-133">Коллекция маршрутов.</span><span class="sxs-lookup"><span data-stu-id="4de61-133">The collection of routes.</span></span> <span data-ttu-id="4de61-134">В разделе [маршрутизации в ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-134">See [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="4de61-135">**Службы**</span><span class="sxs-lookup"><span data-stu-id="4de61-135">**Services**</span></span> | <span data-ttu-id="4de61-136">Коллекция служб.</span><span class="sxs-lookup"><span data-stu-id="4de61-136">The collection of services.</span></span> <span data-ttu-id="4de61-137">В разделе [службы](#services).</span><span class="sxs-lookup"><span data-stu-id="4de61-137">See [Services](#services).</span></span> |


## <a name="prerequisites"></a><span data-ttu-id="4de61-138">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4de61-138">Prerequisites</span></span>

<span data-ttu-id="4de61-139">[Visual Studio 2017 г](https://www.visualstudio.com/vs/) Community, Professional или Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="4de61-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition.</span></span>

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a><span data-ttu-id="4de61-140">Настройка веб-API с размещением ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4de61-140">Configuring Web API with ASP.NET Hosting</span></span>

<span data-ttu-id="4de61-141">В приложении ASP.NET настроить веб-API путем вызова [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) в **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="4de61-141">In an ASP.NET application, configure Web API by calling [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in the **Application\_Start** method.</span></span> <span data-ttu-id="4de61-142">**Настройка** метод принимает делегат с одним параметром типа **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="4de61-142">The **Configure** method takes a delegate with a single parameter of type **HttpConfiguration**.</span></span> <span data-ttu-id="4de61-143">Выполните все вашей конфигурации внутри делегата.</span><span class="sxs-lookup"><span data-stu-id="4de61-143">Perform all of your configuation inside the delegate.</span></span>

<span data-ttu-id="4de61-144">Ниже приведен пример использования анонимного делегата:</span><span class="sxs-lookup"><span data-stu-id="4de61-144">Here is an example using an anonymous delegate:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="4de61-145">В Visual Studio 2017 г., шаблон проекта «Веб-приложение ASP.NET» автоматически настраивает код конфигурации, если выбрать «Веб-API» в **новый проект ASP.NET** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="4de61-145">In Visual Studio 2017, the "ASP.NET Web Application" project template automatically sets up the configuration code, if you select "Web API" in the **New ASP.NET Project** dialog.</span></span>

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

<span data-ttu-id="4de61-146">Шаблон проекта создает файл с именем WebApiConfig.cs внутри приложения\_Начальная папка.</span><span class="sxs-lookup"><span data-stu-id="4de61-146">The project template creates a file named WebApiConfig.cs inside the App\_Start folder.</span></span> <span data-ttu-id="4de61-147">Этот файл код определяет делегат, где следует поместить код конфигурации веб-API.</span><span class="sxs-lookup"><span data-stu-id="4de61-147">This code file defines the delegate where you should put your Web API configuration code.</span></span>

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

<span data-ttu-id="4de61-148">Шаблон проекта также добавляет код, который вызывает делегат из **приложения\_запустить**.</span><span class="sxs-lookup"><span data-stu-id="4de61-148">The project template also adds the code that calls the delegate from **Application\_Start**.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a><span data-ttu-id="4de61-149">Настройка веб-API с резидентной OWIN</span><span class="sxs-lookup"><span data-stu-id="4de61-149">Configuring Web API with OWIN Self-Hosting</span></span>

<span data-ttu-id="4de61-150">Если резидентного размещения с OWIN, создайте новый **HttpConfiguration** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="4de61-150">If you are self-hosting with OWIN, create a new **HttpConfiguration** instance.</span></span> <span data-ttu-id="4de61-151">Выполнить настройки в данном экземпляре, а затем передайте экземпляр для **Owin.UseWebApi** метода расширения.</span><span class="sxs-lookup"><span data-stu-id="4de61-151">Perform any configuration on this instance, and then pass the instance to the **Owin.UseWebApi** extension method.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="4de61-152">Учебник [OWIN используйте Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) показывает все шаги.</span><span class="sxs-lookup"><span data-stu-id="4de61-152">The tutorial [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) shows the complete steps.</span></span>

<a id="services"></a>
## <a name="global-web-api-services"></a><span data-ttu-id="4de61-153">Глобальные веб-API службы</span><span class="sxs-lookup"><span data-stu-id="4de61-153">Global Web API Services</span></span>

<span data-ttu-id="4de61-154">**HttpConfiguration.Services** коллекция содержит набор глобальных служб, используемых для выполнения различных задач, таких как контроллер согласования выделения и содержимое веб-API.</span><span class="sxs-lookup"><span data-stu-id="4de61-154">The **HttpConfiguration.Services** collection contains a set of global services that Web API uses to perform various tasks, such as controller selection and content negotiation.</span></span>

> [!NOTE]
> <span data-ttu-id="4de61-155">**Служб** коллекции не является универсального механизма для внесения обнаружения или зависимостей службы.</span><span class="sxs-lookup"><span data-stu-id="4de61-155">The **Services** collection is not a general-purpose mechanism for service discovery or dependency injection.</span></span> <span data-ttu-id="4de61-156">Она только хранит типы служб, которые заведомо платформа веб-API.</span><span class="sxs-lookup"><span data-stu-id="4de61-156">It only stores service types that are known to the Web API framework.</span></span>


<span data-ttu-id="4de61-157">**Служб** коллекция инициализирована со стандартным набором служб, и можно задать собственные пользовательские реализации.</span><span class="sxs-lookup"><span data-stu-id="4de61-157">The **Services** collection is initialized with a default set of services, and you can provide your own custom implementations.</span></span> <span data-ttu-id="4de61-158">Некоторые службы поддержки нескольких экземпляров, а другие могут иметь только один экземпляр.</span><span class="sxs-lookup"><span data-stu-id="4de61-158">Some services support multiple instances, while others can have only one instance.</span></span> <span data-ttu-id="4de61-159">(Тем не менее, можно также предоставить службы на уровне контроллера; см. раздел [-контроллер конфигурации](#percontrollerconfig).</span><span class="sxs-lookup"><span data-stu-id="4de61-159">(However, you can also provide services at the controller level; see [Per-Controller Configuration](#percontrollerconfig).</span></span>

<span data-ttu-id="4de61-160">Один экземпляр службы</span><span class="sxs-lookup"><span data-stu-id="4de61-160">Single-Instance Services</span></span>


| <span data-ttu-id="4de61-161">Служба</span><span class="sxs-lookup"><span data-stu-id="4de61-161">Service</span></span> | <span data-ttu-id="4de61-162">Описание:</span><span class="sxs-lookup"><span data-stu-id="4de61-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4de61-163">**IActionValueBinder**</span><span class="sxs-lookup"><span data-stu-id="4de61-163">**IActionValueBinder**</span></span> | <span data-ttu-id="4de61-164">Возвращает привязку для параметра.</span><span class="sxs-lookup"><span data-stu-id="4de61-164">Gets a binding for a parameter.</span></span> |
| <span data-ttu-id="4de61-165">**IApiExplorer**</span><span class="sxs-lookup"><span data-stu-id="4de61-165">**IApiExplorer**</span></span> | <span data-ttu-id="4de61-166">Получает описания API-интерфейсы, предоставляемые приложением.</span><span class="sxs-lookup"><span data-stu-id="4de61-166">Gets descriptions of the APIs exposed by the application.</span></span> <span data-ttu-id="4de61-167">В разделе [Создание страницы справки для веб-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-167">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="4de61-168">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="4de61-168">**IAssembliesResolver**</span></span> | <span data-ttu-id="4de61-169">Возвращает список сборок для приложения.</span><span class="sxs-lookup"><span data-stu-id="4de61-169">Gets a list of the assemblies for the application.</span></span> <span data-ttu-id="4de61-170">В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-170">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="4de61-171">**IBodyModelValidator**</span><span class="sxs-lookup"><span data-stu-id="4de61-171">**IBodyModelValidator**</span></span> | <span data-ttu-id="4de61-172">Проверяет модель, которая считывается в тексте запроса модуль форматирования типа мультимедиа.</span><span class="sxs-lookup"><span data-stu-id="4de61-172">Validates a model that is read from the request body by a media-type formatter.</span></span> |
| <span data-ttu-id="4de61-173">**IContentNegotiator**</span><span class="sxs-lookup"><span data-stu-id="4de61-173">**IContentNegotiator**</span></span> | <span data-ttu-id="4de61-174">Выполняет согласование содержимого.</span><span class="sxs-lookup"><span data-stu-id="4de61-174">Performs content negotiation.</span></span> |
| <span data-ttu-id="4de61-175">**IDocumentationProvider**</span><span class="sxs-lookup"><span data-stu-id="4de61-175">**IDocumentationProvider**</span></span> | <span data-ttu-id="4de61-176">Предоставляет документацию для API-интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="4de61-176">Provides documentation for APIs.</span></span> <span data-ttu-id="4de61-177">Значение по умолчанию — **null**.</span><span class="sxs-lookup"><span data-stu-id="4de61-177">The default is **null**.</span></span> <span data-ttu-id="4de61-178">В разделе [Создание страницы справки для веб-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-178">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="4de61-179">**IHostBufferPolicySelector**</span><span class="sxs-lookup"><span data-stu-id="4de61-179">**IHostBufferPolicySelector**</span></span> | <span data-ttu-id="4de61-180">Указывает, является ли узел буферизовать текст сущности сообщений HTTP.</span><span class="sxs-lookup"><span data-stu-id="4de61-180">Indicates whether the host should buffer HTTP message entity bodies.</span></span> |
| <span data-ttu-id="4de61-181">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="4de61-181">**IHttpActionInvoker**</span></span> | <span data-ttu-id="4de61-182">Вызывает действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="4de61-182">Invokes a controller action.</span></span> <span data-ttu-id="4de61-183">В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-183">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="4de61-184">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="4de61-184">**IHttpActionSelector**</span></span> | <span data-ttu-id="4de61-185">Выбирает действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="4de61-185">Selects a controller action.</span></span> <span data-ttu-id="4de61-186">В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-186">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="4de61-187">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="4de61-187">**IHttpControllerActivator**</span></span> | <span data-ttu-id="4de61-188">Активирует контроллера.</span><span class="sxs-lookup"><span data-stu-id="4de61-188">Activates a controller.</span></span> <span data-ttu-id="4de61-189">В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-189">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="4de61-190">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="4de61-190">**IHttpControllerSelector**</span></span> | <span data-ttu-id="4de61-191">Выбирает контроллер.</span><span class="sxs-lookup"><span data-stu-id="4de61-191">Selects a controller.</span></span> <span data-ttu-id="4de61-192">В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-192">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="4de61-193">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="4de61-193">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="4de61-194">Содержит список типов контроллера веб-API в приложении.</span><span class="sxs-lookup"><span data-stu-id="4de61-194">Provides a list of the Web API controller types in the application.</span></span> <span data-ttu-id="4de61-195">В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-195">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="4de61-196">**ITraceManager**</span><span class="sxs-lookup"><span data-stu-id="4de61-196">**ITraceManager**</span></span> | <span data-ttu-id="4de61-197">Инициализирует платформы трассировки.</span><span class="sxs-lookup"><span data-stu-id="4de61-197">Initializes the tracing framework.</span></span> <span data-ttu-id="4de61-198">В разделе [трассировки в ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-198">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="4de61-199">**ITraceWriter**</span><span class="sxs-lookup"><span data-stu-id="4de61-199">**ITraceWriter**</span></span> | <span data-ttu-id="4de61-200">Предоставляет модуль записи трассировки.</span><span class="sxs-lookup"><span data-stu-id="4de61-200">Provides a trace writer.</span></span> <span data-ttu-id="4de61-201">Значение по умолчанию — модуль записи трассировки «холостой».</span><span class="sxs-lookup"><span data-stu-id="4de61-201">The default is a "no-op" trace writer.</span></span> <span data-ttu-id="4de61-202">В разделе [трассировки в ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4de61-202">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="4de61-203">**IModelValidatorCache**</span><span class="sxs-lookup"><span data-stu-id="4de61-203">**IModelValidatorCache**</span></span> | <span data-ttu-id="4de61-204">Предоставляет кэш средств проверки модели.</span><span class="sxs-lookup"><span data-stu-id="4de61-204">Provides a cache of model validators.</span></span> |

<span data-ttu-id="4de61-205">Нескольких экземпляров служб</span><span class="sxs-lookup"><span data-stu-id="4de61-205">Multiple-Instance Services</span></span>


| <span data-ttu-id="4de61-206">Служба</span><span class="sxs-lookup"><span data-stu-id="4de61-206">Service</span></span> | <span data-ttu-id="4de61-207">Описание:</span><span class="sxs-lookup"><span data-stu-id="4de61-207">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4de61-208">**IFilterProvider**</span><span class="sxs-lookup"><span data-stu-id="4de61-208">**IFilterProvider**</span></span> | <span data-ttu-id="4de61-209">Возвращает список фильтров для действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="4de61-209">Returns a list of filters for a controller action.</span></span> |
| <span data-ttu-id="4de61-210">**ModelBinderProvider**</span><span class="sxs-lookup"><span data-stu-id="4de61-210">**ModelBinderProvider**</span></span> | <span data-ttu-id="4de61-211">Возвращает связыватель модели для данного типа.</span><span class="sxs-lookup"><span data-stu-id="4de61-211">Returns a model binder for a given type.</span></span> |
| <span data-ttu-id="4de61-212">**ModelMetadataProvider**</span><span class="sxs-lookup"><span data-stu-id="4de61-212">**ModelMetadataProvider**</span></span> | <span data-ttu-id="4de61-213">Предоставляет метаданные для модели.</span><span class="sxs-lookup"><span data-stu-id="4de61-213">Provides metadata for a model.</span></span> |
| <span data-ttu-id="4de61-214">**ModelValidatorProvider**</span><span class="sxs-lookup"><span data-stu-id="4de61-214">**ModelValidatorProvider**</span></span> | <span data-ttu-id="4de61-215">Предоставляет проверяющий элемент управления для модели.</span><span class="sxs-lookup"><span data-stu-id="4de61-215">Provides a validator for a model.</span></span> |
| <span data-ttu-id="4de61-216">**ValueProviderFactory**</span><span class="sxs-lookup"><span data-stu-id="4de61-216">**ValueProviderFactory**</span></span> | <span data-ttu-id="4de61-217">Создает поставщик значений.</span><span class="sxs-lookup"><span data-stu-id="4de61-217">Creates a value provider.</span></span> <span data-ttu-id="4de61-218">Дополнительные сведения см. в разделе блога Майк стол [Создание поставщика пользовательских значений в WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span><span class="sxs-lookup"><span data-stu-id="4de61-218">For more information, see Mike Stall's blog post [How to create a custom value provider in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span></span> |<span data-ttu-id="4de61-219">.</span><span class="sxs-lookup"><span data-stu-id="4de61-219">.</span></span>

<span data-ttu-id="4de61-220">Чтобы добавить пользовательскую реализацию нескольких экземпляров службы, вызовите **добавить** или **вставить** на **служб** коллекции:</span><span class="sxs-lookup"><span data-stu-id="4de61-220">To add a custom implementation to a multi-instance service, call **Add** or **Insert** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="4de61-221">Чтобы заменить одного экземпляра службы с пользовательской реализацией, вызовите **заменить** на **служб** коллекции:</span><span class="sxs-lookup"><span data-stu-id="4de61-221">To replace a single-instance service with a custom implementation, call **Replace** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a><span data-ttu-id="4de61-222">Настройка контроллера</span><span class="sxs-lookup"><span data-stu-id="4de61-222">Per-Controller Configuration</span></span>

<span data-ttu-id="4de61-223">Можно переопределить отдельно для каждого контроллера следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="4de61-223">You can override the following settings on a per-controller basis:</span></span>

- <span data-ttu-id="4de61-224">Модули форматирования типа мультимедиа</span><span class="sxs-lookup"><span data-stu-id="4de61-224">Media-type formatters</span></span>
- <span data-ttu-id="4de61-225">Параметр привязки правил</span><span class="sxs-lookup"><span data-stu-id="4de61-225">Parameter binding rules</span></span>
- <span data-ttu-id="4de61-226">Службы</span><span class="sxs-lookup"><span data-stu-id="4de61-226">Services</span></span>

<span data-ttu-id="4de61-227">Для этого нужно определить настраиваемый атрибут, реализующий **IControllerConfiguration** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="4de61-227">To do so, define a custom attribute that implements the **IControllerConfiguration** interface.</span></span> <span data-ttu-id="4de61-228">Затем примените атрибут к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="4de61-228">Then apply the attribute to the controller.</span></span>

<span data-ttu-id="4de61-229">Следующий пример заменяет пользовательский модуль форматирования по умолчанию модули форматирования типа мультимедиа.</span><span class="sxs-lookup"><span data-stu-id="4de61-229">The following example replaces the default media-type formatters with a custom formatter.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="4de61-230">**IControllerConfiguration.Initialize** метод принимает два параметра:</span><span class="sxs-lookup"><span data-stu-id="4de61-230">The **IControllerConfiguration.Initialize** method takes two parameters:</span></span>

- <span data-ttu-id="4de61-231">**HttpControllerSettings** объекта</span><span class="sxs-lookup"><span data-stu-id="4de61-231">An **HttpControllerSettings** object</span></span>
- <span data-ttu-id="4de61-232">**HttpControllerDescriptor** объекта</span><span class="sxs-lookup"><span data-stu-id="4de61-232">An **HttpControllerDescriptor** object</span></span>

<span data-ttu-id="4de61-233">**HttpControllerDescriptor** содержит описание контроллера, который можно просмотреть в ознакомительных целях (скажем, для различения двух контроллеров).</span><span class="sxs-lookup"><span data-stu-id="4de61-233">The **HttpControllerDescriptor** contains a description of the controller, which you can examine for informational purposes (say, to distinguish between two controllers).</span></span>

<span data-ttu-id="4de61-234">Используйте **HttpControllerSettings** объекта, чтобы настроить контроллер.</span><span class="sxs-lookup"><span data-stu-id="4de61-234">Use the **HttpControllerSettings** object to configure the controller.</span></span> <span data-ttu-id="4de61-235">Этот объект содержит подмножество параметров конфигурации, которые можно переопределить отдельно для каждого контроллера.</span><span class="sxs-lookup"><span data-stu-id="4de61-235">This object contains the subset of configuration parameters that you can override on a per-controller basis.</span></span> <span data-ttu-id="4de61-236">По умолчанию все параметры, которые не изменяются на глобальную **HttpConfiguration** объекта.</span><span class="sxs-lookup"><span data-stu-id="4de61-236">Any settings that you don't change default to the global **HttpConfiguration** object.</span></span>
