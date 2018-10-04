---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Настройка веб-API 2 ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 57066b8ce3254caf59cf927d16d96f8bc22a8acd
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795255"
---
<a name="configuring-aspnet-web-api-2"></a>Настройка веб-API 2 ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

В этом разделе описываются способы настройки веб-API ASP.NET.

- [Параметры конфигурации](#settings)
- [Настройка веб-API с размещением в ASP.NET](#webhost)
- [Настройка веб-API с Саморазмещением OWIN](#selfhost)
- [Глобальный веб-API службы](#services)
- [Конфигурации-контроллер](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Параметры конфигурации

Принудительный конфигурации веб-API определяются в [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) класса.

| Член | Описание: |
| --- | --- |
| **DependencyResolver** | Включает возможность встраивания зависимостей для контроллеров. См. в разделе [с помощью сопоставителя зависимостей веб-API](dependency-injection.md). |
| **Фильтры** | Фильтры действий. |
| **Модули форматирования** | [Модули форматирования типа мультимедиа](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Указывает, должен ли сервер включать сведения об ошибке, например сообщения исключений и трассировки стека, в сообщениях ответа HTTP. См. в разделе [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Инициализатор** | Функция, которая выполняет окончательной инициализации **HttpConfiguration**. |
| **MessageHandlers** | [Обработчики сообщений HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Коллекция правил для привязки параметров действия контроллера. |
| **Свойства** | Универсальный контейнер свойств. |
| **Маршруты** | Коллекция маршрутов. См. в разделе [маршрутизации в веб-API ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Службы** | Коллекция служб. См. в разделе [служб](#services). |


## <a name="prerequisites"></a>Предварительные требования

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional или Enterprise edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Настройка веб-API с размещением в ASP.NET

В приложении ASP.NET настройки веб-API путем вызова [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) в **приложения\_запустить** метод. **Настройка** метод принимает делегат с одним параметром типа **HttpConfiguration**. Выполните все ваши настройки внутри делегата.

Ниже приведен пример использования анонимного делегата:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

В Visual Studio 2017, шаблон проекта «Веб-приложение ASP.NET» автоматически настраивает код конфигурации, если выбрать «Веб-API» в **новый проект ASP.NET** диалоговое окно.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Шаблон проекта создает файл с именем WebApiConfig.cs в приложении\_Начальная папка. Этот файл код определяет делегат, где следует поместить код настройки веб-API.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Шаблон проекта также добавляет код, который вызывает делегат из **приложения\_запустить**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Настройка веб-API с Саморазмещением OWIN

В случае резидентного размещения с OWIN, создайте новый **HttpConfiguration** экземпляра. Выполнить настройки на этом экземпляре, а затем передайте экземпляр для **Owin.UseWebApi** метода расширения.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Руководства [использование OWIN для Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) показан полный набор действий.

<a id="services"></a>
## <a name="global-web-api-services"></a>Глобальный веб-API службы

**HttpConfiguration.Services** коллекция содержит набор глобальных служб, используемых веб-API для выполнения различных задач, таких как контроллер выделения и согласующего содержимое.

> [!NOTE]
> **Служб** коллекция не является механизм общего назначения для внесения обнаружения или зависимостей службы. Она только хранит типов служб, которые заведомо платформа веб-API.


**Служб** коллекция инициализирована со стандартным набором служб, и можно задать пользовательские реализации. Некоторые службы поддерживают несколько экземпляров, а другие могут иметь только один экземпляр. (Тем не менее, можно также предоставить службы на уровне контроллера; см. в разделе [конфигурации-контроллер](#percontrollerconfig).

Одного экземпляра службы


| Служба | Описание: |
| --- | --- |
| **IActionValueBinder** | Получает привязку для параметра. |
| **IApiExplorer** | Получает описания интерфейсов API, предоставляемые приложением. См. в разделе [Создание страницы справки для веб-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Возвращает список сборок для приложения. См. в разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Проверяет модель, которая считывается из текста запроса модулем форматирования типа мультимедиа. |
| **IContentNegotiator** | Выполняет согласование содержимого. |
| **IDocumentationProvider** | Содержит документацию по API-интерфейсы. По умолчанию используется **null**. См. в разделе [Создание страницы справки для веб-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Указывает, является ли узел буферизовать текст сущности HTTP-сообщений. |
| **IHttpActionInvoker** | Вызывает действие контроллера. См. в разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Выбирает действие контроллера. См. в разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Активирует контроллера. См. в разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Выбирает контроллер. См. в разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Предоставляет список типов контроллера веб-API в приложении. См. в разделе [Маршрутизация и выбор действий](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Инициализирует платформу трассировки. См. в разделе [трассировка в веб-API ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Предоставляет модуль записи трассировки. По умолчанию используется модуль записи трассировки «холостыми». См. в разделе [трассировка в веб-API ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Предоставляет кэш средств проверки модели. |

Несколько экземпляров служб


|                 Служба                 |                                                                                                              Описание:                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Возвращает список фильтров для действия контроллера.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Возвращает связыватель модели для данного типа.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Предоставляет метаданные для модели.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Предоставляет проверяющий элемент управления для модели.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Создает поставщик значений. Дополнительные сведения см. в разделе блога Майк стол [Создание поставщика пользовательских значений в веб-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Чтобы добавить пользовательскую реализацию службы несколькими экземплярами, вызовите **добавить** или **вставить** на **служб** коллекции:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Чтобы заменить одного экземпляра службы с пользовательской реализацией, вызовите **замените** на **служб** коллекции:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Конфигурации-контроллер

Можно переопределить следующие параметры на основе конкретного контроллера:

- Модули форматирования типа мультимедиа
- Параметр привязки правил
- Службы

Для этого нужно определить настраиваемый атрибут, реализующий **IControllerConfiguration** интерфейс. Затем примените атрибут к контроллеру.

В следующем примере заменяется модули форматирования типа мультимедиа по умолчанию с помощью пользовательского модуля форматирования.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize** метод принимает два параметра:

- **HttpControllerSettings** объекта
- **HttpControllerDescriptor** объекта

**HttpControllerDescriptor** содержит описание контроллера, который можно проверить в информационных целях (скажем, для различения двух контроллеров).

Используйте **HttpControllerSettings** объекта, чтобы настроить контроллер. Этот объект содержит подмножество параметров конфигурации, которые можно переопределить на основе-контроллер. По умолчанию все параметры, которые не изменяются на глобальную **HttpConfiguration** объекта.
