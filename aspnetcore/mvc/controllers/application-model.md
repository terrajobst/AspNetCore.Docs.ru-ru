---
title: "Работа с моделью приложения"
author: ardalis
description: 
keywords: "Модель приложения ASP.NET Core, ASP.NET Core MVC"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4eb7e52f-5665-41a4-a3e3-e348d07337f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: 18046389becd17135ff831e71e700244d48552d3
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-the-application-model"></a>Работа с моделью приложения

По [Стив Смит](http://ardalis.com)

Определяет основные ASP.NET MVC *модель приложения* представляющие компоненты приложения MVC. Можно читать и управлять этой модели, чтобы изменить поведение элементов MVC. По умолчанию MVC соблюдаются определенные соглашения для определения классов, которые считаются контроллеров, какие методы этих классов являются действия и поведение маршрутизации и параметры. Вы можете настроить это поведение в соответствии с потребностями приложения путем создания собственного правила и применения их глобально или как атрибуты.

## <a name="models-and-providers"></a>Модели и поставщики

Модель приложения ASP.NET Core MVC включают абстрактные интерфейсы и конкретные реализации классов, описывающих приложения MVC. Эта модель является результатом обнаружения контроллеров, действия, параметров действия, маршруты и фильтров в соответствии с соглашениями по умолчанию приложения MVC. Работая с моделью приложения, можно изменить приложения для разных соглашениям от поведения MVC по умолчанию. Параметры, имена, маршрутов и фильтры всех используются как данные конфигурации для контроллеров и действий.

Модель приложения MVC ASP.NET Core имеет следующую структуру:

* ApplicationModel
    * Контроллеры (ControllerModel)
        * Действия (ActionModel)
            * Параметры (ParameterModel)

Каждый уровень модели имеет доступ к обычное `Properties` коллекции, а более низкие уровни можно получить доступ к и перезаписать значения свойств, заданные на более высоких уровнях иерархии. Свойства, сохраняются в `ActionDescriptor.Properties` при создании действия. Затем при обработке запроса какие-либо свойства соглашение о добавлении или изменении может осуществляться через `ActionContext.ActionDescriptor.Properties`. С помощью свойств является хорошим способом настроить фильтры, связыватели моделей, т. д., отдельно для каждого действия.

> [!NOTE]
> `ActionDescriptor.Properties` Коллекции не является потокобезопасным (для операций записи), после завершения процесса запуска приложения. Соглашения — это лучший способ, чтобы добавить данные в эту коллекцию.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

ASP.NET Core MVC загружает модели приложения, с помощью шаблона поставщика, определяется [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) интерфейса. В этом разделе освещаются некоторые сведения о внутренней реализации этой функции поставщика. Это довольно сложная тема - большинства приложений, использующих модель приложения следует это делать, работая с соглашения.

Реализации `IApplicationModelProvider` интерфейс «wrap» друг с другом, с каждого вызова реализации `OnProvidersExecuting` по возрастанию на основе его `Order` свойство. `OnProvidersExecuted` Затем вызывается метод в обратном порядке. Платформа framework определяет несколько поставщиков:

Первый (`Order=-1000`):

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

Затем (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> Порядок, в которой два поставщика с тем же значением для `Order` называются не определено и поэтому не следует полагаться на.

> [!NOTE]
> `IApplicationModelProvider`является расширенные концепции для авторов framework для расширения. В общем случае приложения должны использовать соглашения и платформ следует использовать поставщиков. Ключевое различие — всегда поставщики выполняются перед соглашения.

`DefaultApplicationModelProvider` Устанавливает множество вариантов поведения по умолчанию, используемые ядра ASP.NET MVC. Его обязанности входит:

* Добавление глобальных фильтров контекста
* Добавление контроллеров в контексте
* Добавление контроллера открытых методов в качестве действия
* Добавление параметров методов действий в контексте
* Применение маршрута и другие атрибуты

Некоторые встроенные поведения реализуются `DefaultApplicationModelProvider`. Этот поставщик отвечает за создание [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), который в свою очередь ссылается на [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), и [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) экземпляров. `DefaultApplicationModelProvider` Класса является элементом реализации внутреннюю структуру, можно и приведет к изменению в будущем. 

`AuthorizationApplicationModelProvider` Отвечает за применение поведение, связанное с `AuthorizeFilter` и `AllowAnonymousFilter` атрибуты. [Дополнительные сведения об этих атрибутов](https://docs.microsoft.com/aspnet/core/security/authorization/simple).

`CorsApplicationModelProvider` Реализует поведение, связанное с `IEnableCorsAttribute` и `IDisableCorsAttribute`и `DisableCorsAuthorizationFilter`. [Дополнительные сведения о CORS](https://docs.microsoft.com/aspnet/core/security/cors).

## <a name="conventions"></a>Соглашения

Модель приложения определяет соглашение о абстрактные классы, которые предоставляют простой способ настроить поведение модели, чем переопределение всей модели или поставщика. Эти абстракции являются рекомендуемый способ изменения поведения приложения. Соглашения позволяют писать код, который будет динамически применения настроек. Хотя [фильтры](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters) позволяют изменить поведение платформы, настройки позволяют пользователю контролировать, как совместно проводной всего приложения.

Доступны следующие соглашения:

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

Соглашения применяются, добавив их в MVC параметры либо путем реализации `Attribute`s и применении этих параметров действия, контроллеров и действий (аналогично [ `Filters` ](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters)). В отличие от фильтров соглашения о выполняются только при запуске приложения, не являющийся частью каждого запроса.

### <a name="sample-modifying-the-applicationmodel"></a>Пример: Изменение ApplicationModel

Чтобы добавить свойство в модели приложения используется следующее соглашение. 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Соглашения о модели приложения применяются как параметры, при добавлении в MVC `ConfigureServices` в `Startup`.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Свойства доступны из `ActionDescriptor` свойства коллекции в течение действия контроллера:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Пример: Изменение описания ControllerModel

Как в предыдущем примере контроллера модели можно изменить, чтобы включить настраиваемые свойства. Они переопределят существующие свойства с именем, указанным в модели приложения. Следующий атрибут соглашение добавляет описание на уровне контроллера:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Это соглашение будет применяться в качестве атрибута на контроллере.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

Свойство «описание» осуществляется так же, как и в предыдущих примерах.

### <a name="sample-modifying-the-actionmodel-description"></a>Пример: Изменение описания ActionModel

Соглашение отдельный атрибут может применяться к отдельных действий, переопределение поведения, уже примененные на уровне приложения или контроллера.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Применение этого действия в предыдущем примере контроллера показано, как оно переопределяет соглашение уровня контроллера:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Пример: Изменение ParameterModel

Следующее соглашение может применяться к параметрам действия, чтобы изменить их `BindingInfo`. Следующее соглашение требует, чтобы параметр был параметр маршрута; Другие возможные источники привязки (например, значения строки запроса) учитываются.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

Атрибут может применяться к любой параметр действия:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Пример: Изменение имени ActionModel

Изменяет следующее соглашение `ActionModel` обновление *имя* действия, к которому он применяется. Новое имя предоставляется как параметр атрибута. Это новое имя используется по маршрутизация, поэтому влияет на маршрута, используемого для доступа данным методом действия.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Этот атрибут применяется к методу действия в `HomeController`:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

Несмотря на то, что имя метода `SomeName`, атрибут MVC соглашение о том, используя имя метода, заменяет имя действия с `MyCoolAction`. Таким образом, включающий это действие маршрута — `/Home/MyCoolAction`.

> [!NOTE]
> В этом примере в основном одинаков применения встроенного [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) атрибута.

### <a name="sample-custom-routing-convention"></a>Образец: Соглашение о маршрутизации пользовательский

Можно использовать `IApplicationModelConvention` для настройки, как работает маршрутизация. Например, следующее соглашение будет включать в себя пространств имен контроллеров в маршруты, заменив `.` в пространстве имен с `/` маршрута:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

Соглашение о добавляется в качестве альтернативы при запуске.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> Можно добавить соглашения в вашей [по промежуточного слоя](https://docs.microsoft.com/aspnet/core/fundamentals/middleware) , обратившись к `MvcOptions` с помощью`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

В этом примере применяется это соглашение маршруты, не использующие атрибут маршрутизации, где контроллер имеет «Пространство имен» в имени. Следующий контроллер демонстрирует это соглашение:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Использование модели приложения в WebApiCompatShim

ASP.NET Core MVC использует другой набор соглашений из веб-API ASP.NET 2. С помощью пользовательского соглашения, можно изменить поведение приложения ASP.NET Core MVC для соответствия, приложения веб-API. Поставляется с Microsoft [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) специально для этой цели.

> [!NOTE]
> Дополнительные сведения о [миграции с веб-API ASP.NET](https://docs.microsoft.com/aspnet/core/migration/webapi)).

Чтобы использовать оболочку совместимости Web API, необходимо добавить пакет в проект и добавить условные обозначения MVC с помощью `AddWebApiConventions` в `Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

Соглашения, предоставляемые прокладка применяются только к части приложения, для которых определенные атрибуты, примененные к ним. Следующие четыре атрибуты, используемые для элемента управления, имеющих контроллеры следует их соглашения, была изменена соглашения оболочки.

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Действия соглашения

`UseWebApiActionConventionsAttribute` Используется для сопоставления метода HTTP для действия в зависимости от их имени (например, `Get` будут сопоставлены `HttpGet`). Он применяется только к действиям, которые не используют атрибут маршрутизации.

### <a name="overloading"></a>Перегрузка

`UseWebApiOverloadingAttribute` Используется для применения `WebApiOverloadingApplicationModelConvention` соглашение. Это соглашение добавляет `OverloadActionConstraint` для завершения процесса выбора действия, что ограничивает действия кандидатом для тех, для которых запрос удовлетворяет всех обязательных параметров.

### <a name="parameter-conventions"></a>Параметр соглашения

`UseWebApiParameterConventionsAttribute` Используется для применения `WebApiParameterConventionsApplicationModelConvention` действия соглашения. Это соглашение указывает, что простые типы, используемые в качестве параметров действия привязаны из URI по умолчанию во время сложных типов привязаны в тексте запроса.

### <a name="routes"></a>Маршруты

`UseWebApiRoutesAttribute` Элементы управления ли `WebApiApplicationModelConvention` применяется соглашение контроллера. Если включено, это обозначение используется для добавления поддержки [областей](https://docs.microsoft.com/aspnet/core/mvc/controllers/areas) для маршрута.

В дополнение к набор соглашений, включает пакет совместимости `System.Web.Http.ApiController` базового класса, который заменяет, предоставляемых веб-API. Это позволяет контроллеров, написанные для веб-API и наследование из его `ApiController` будут работать, как они были разработаны, при выполнении с ASP.NET Core MVC. Этот базовый класс, отмеченный со всеми `UseWebApi*` атрибуты, приведенные выше. `ApiController` Предоставляет свойства, методы и типы результата, совместимые с имеющимся в веб-API.

## <a name="using-apiexplorer-to-document-your-app"></a>С помощью ApiExplorer для документирования приложения

Модель приложения предоставляет [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) свойства на каждом уровне, который может использоваться для обхода структуры приложения. Это может быть использована для [создания страницы справки для вашего веб-API, с помощью таких средств, как Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger). `ApiExplorer` Предоставляет свойство `IsVisible` свойство, которое можно задать, чтобы указать, какие части модель приложения должен быть предоставлен. Можно настроить этот параметр, используя соглашение:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Используя этот подход (и дополнительные соглашения, если это необходимо), можно включить или отключить видимость API на любом уровне в приложении. 
