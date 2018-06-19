---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Настройка ASP.NET Web API 2 | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: de2396710fb9434c84bf14a2faa37b98154f34d8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874984"
---
<a name="configuring-aspnet-web-api-2"></a>Настройка ASP.NET Web API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

В этом разделе описываются способы настройки веб-API ASP.NET.

- [Параметры конфигурации](#settings)
- [Настройка веб-API с размещением ASP.NET](#webhost)
- [Настройка веб-API с резидентной OWIN](#selfhost)
- [Глобальные веб-API службы](#services)
- [Настройка контроллера](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Параметры конфигурации

Определенные параметры конфигурации веб-API в [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) класса.

| Член | Описание |
| --- | --- |
| **DependencyResolver** | Позволяет внедрения зависимостей для контроллеров. В разделе [с помощью сопоставителя зависимостей веб-API](dependency-injection.md). |
| **Фильтры** | Фильтры действий. |
| **Formatters** | [Модули форматирования типа мультимедиа](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Указывает, должен ли сервер включать сведения об ошибке, например сообщения исключений и трассировки стека в сообщений ответов HTTP. В разделе [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Инициализатор** | Функция, которая выполняет окончательной инициализации **HttpConfiguration**. |
| **MessageHandlers** | [Обработчики сообщений HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Коллекции правил привязки параметров действий контроллера. |
| **Свойства** | Универсальное хранилище свойств. |
| **Маршруты** | Коллекция маршрутов. В разделе [маршрутизации в ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Службы** | Коллекция служб. В разделе [службы](#services). |


## <a name="prerequisites"></a>Предварительные требования

[Visual Studio 2017 г](https://www.visualstudio.com/vs/) Community, Professional или Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Настройка веб-API с размещением ASP.NET

В приложении ASP.NET настроить веб-API путем вызова [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) в **приложения\_запустить** метод. **Настройка** метод принимает делегат с одним параметром типа **HttpConfiguration**. Выполните все вашей конфигурации внутри делегата.

Ниже приведен пример использования анонимного делегата:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

В Visual Studio 2017 г., шаблон проекта «Веб-приложение ASP.NET» автоматически настраивает код конфигурации, если выбрать «Веб-API» в **новый проект ASP.NET** диалогового окна.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Шаблон проекта создает файл с именем WebApiConfig.cs внутри приложения\_Начальная папка. Этот файл код определяет делегат, где следует поместить код конфигурации веб-API.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Шаблон проекта также добавляет код, который вызывает делегат из **приложения\_запустить**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Настройка веб-API с резидентной OWIN

Если резидентного размещения с OWIN, создайте новый **HttpConfiguration** экземпляра. Выполнить настройки в данном экземпляре, а затем передайте экземпляр для **Owin.UseWebApi** метода расширения.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Учебник [OWIN используйте Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) показывает все шаги.

<a id="services"></a>
## <a name="global-web-api-services"></a>Глобальные веб-API службы

**HttpConfiguration.Services** коллекция содержит набор глобальных служб, используемых для выполнения различных задач, таких как контроллер согласования выделения и содержимое веб-API.

> [!NOTE]
> **Служб** коллекции не является универсального механизма для внесения обнаружения или зависимостей службы. Она только хранит типы служб, которые заведомо платформа веб-API.


**Служб** коллекция инициализирована со стандартным набором служб, и можно задать собственные пользовательские реализации. Некоторые службы поддержки нескольких экземпляров, а другие могут иметь только один экземпляр. (Тем не менее, можно также предоставить службы на уровне контроллера; см. раздел [-контроллер конфигурации](#percontrollerconfig).

Один экземпляр службы


| Служба | Описание |
| --- | --- |
| **IActionValueBinder** | Возвращает привязку для параметра. |
| **IApiExplorer** | Получает описания API-интерфейсы, предоставляемые приложением. В разделе [Создание страницы справки для веб-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Возвращает список сборок для приложения. В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Проверяет модель, которая считывается в тексте запроса модуль форматирования типа мультимедиа. |
| **IContentNegotiator** | Выполняет согласование содержимого. |
| **IDocumentationProvider** | Предоставляет документацию для API-интерфейсов. Значение по умолчанию — **null**. В разделе [Создание страницы справки для веб-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Указывает, является ли узел буферизовать текст сущности сообщений HTTP. |
| **IHttpActionInvoker** | Вызывает действие контроллера. В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Выбирает действие контроллера. В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Активирует контроллера. В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Выбирает контроллер. В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Содержит список типов контроллера веб-API в приложении. В разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Инициализирует платформы трассировки. В разделе [трассировки в ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Предоставляет модуль записи трассировки. Значение по умолчанию — модуль записи трассировки «холостой». В разделе [трассировки в ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Предоставляет кэш средств проверки модели. |

Нескольких экземпляров служб


|                 Служба                 |                                                                                                              Описание                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Возвращает список фильтров для действия контроллера.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Возвращает связыватель модели для данного типа.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Предоставляет метаданные для модели.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Предоставляет проверяющий элемент управления для модели.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Создает поставщик значений. Дополнительные сведения см. в разделе блога Майк стол [Создание поставщика пользовательских значений в WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Чтобы добавить пользовательскую реализацию нескольких экземпляров службы, вызовите **добавить** или **вставить** на **служб** коллекции:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Чтобы заменить одного экземпляра службы с пользовательской реализацией, вызовите **заменить** на **служб** коллекции:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Настройка контроллера

Можно переопределить отдельно для каждого контроллера следующие параметры:

- Модули форматирования типа мультимедиа
- Параметр привязки правил
- Службы

Для этого нужно определить настраиваемый атрибут, реализующий **IControllerConfiguration** интерфейса. Затем примените атрибут к контроллеру.

Следующий пример заменяет пользовательский модуль форматирования по умолчанию модули форматирования типа мультимедиа.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize** метод принимает два параметра:

- **HttpControllerSettings** объекта
- **HttpControllerDescriptor** объекта

**HttpControllerDescriptor** содержит описание контроллера, который можно просмотреть в ознакомительных целях (скажем, для различения двух контроллеров).

Используйте **HttpControllerSettings** объекта, чтобы настроить контроллер. Этот объект содержит подмножество параметров конфигурации, которые можно переопределить отдельно для каждого контроллера. По умолчанию все параметры, которые не изменяются на глобальную **HttpConfiguration** объекта.
