---
title: Фильтры в ASP.NET Core
author: ardalis
description: Из этой статьи вы узнаете, как работают фильтры и как их использовать в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 50b199744f32ad19335080da406db69665ec1ae9
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856161"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="a798d-103">Фильтры в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a798d-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="a798d-104">Авторы: [Кирк Ларкин](https://github.com/serpent5) (Kirk Larkin) [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Том Дайкстра](https://github.com/tdykstra/) (Tom Dykstra) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="a798d-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a798d-105">*Фильтры* в ASP.NET Core позволяют выполнять код до или после определенных этапов в конвейере обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="a798d-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="a798d-106">Встроенные фильтры обрабатывают следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="a798d-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="a798d-107">Авторизация (предотвращение несанкционированного доступа к ресурсам).</span><span class="sxs-lookup"><span data-stu-id="a798d-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="a798d-108">Кэширование откликов (замыкание конвейера обработки запросов для возврата кэшированного ответа).</span><span class="sxs-lookup"><span data-stu-id="a798d-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="a798d-109">Для обработки сквозной функциональности можно создавать настраиваемые фильтры.</span><span class="sxs-lookup"><span data-stu-id="a798d-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="a798d-110">Примеры сквозной функциональности включают обработку ошибок, кэширование, конфигурирование, авторизацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="a798d-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="a798d-111">Фильтры предотвращают дублирование кода.</span><span class="sxs-lookup"><span data-stu-id="a798d-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="a798d-112">Например, можно объединить обработку ошибок с помощью фильтра исключений обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="a798d-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="a798d-113">Этот документ применим к Razor Pages, контроллерам API и контроллерам с представлениями.</span><span class="sxs-lookup"><span data-stu-id="a798d-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="a798d-114">[Просмотреть или скачать пример](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([как скачивать](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a798d-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="a798d-115">Как работают фильтры</span><span class="sxs-lookup"><span data-stu-id="a798d-115">How filters work</span></span>

<span data-ttu-id="a798d-116">Фильтры выполняются в *конвейере вызова действий ASP.NET Core*, который иногда называют *конвейером фильтров*.</span><span class="sxs-lookup"><span data-stu-id="a798d-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="a798d-117">Конвейер фильтров запускается после того, как платформа ASP.NET Core выбирает выполняемое действие.</span><span class="sxs-lookup"><span data-stu-id="a798d-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Запрос обрабатывается с использованием прочего ПО промежуточного слоя, ПО промежуточного слоя маршрутизации, выбора действия и конвейера вызова действий ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="a798d-120">Типы фильтров</span><span class="sxs-lookup"><span data-stu-id="a798d-120">Filter types</span></span>

<span data-ttu-id="a798d-121">Фильтр каждого типа выполняется на определенном этапе конвейера фильтров:</span><span class="sxs-lookup"><span data-stu-id="a798d-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="a798d-122">[Фильтры авторизации](#authorization-filters). Применяются в первую очередь и служат для определения того, авторизован ли пользователь для выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="a798d-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="a798d-123">Эти фильтры не могут сократить выполнение конвейера, если запрос не авторизован.</span><span class="sxs-lookup"><span data-stu-id="a798d-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="a798d-124">[Фильтры ресурсов](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="a798d-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="a798d-125">Выполняются после авторизации.</span><span class="sxs-lookup"><span data-stu-id="a798d-125">Run after authorization.</span></span>  
  * <span data-ttu-id="a798d-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> может выполнять код до остальной части конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="a798d-127">Например, `OnResourceExecuting` может выполнять код до привязки модели.</span><span class="sxs-lookup"><span data-stu-id="a798d-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="a798d-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> может выполнять код после завершения остальной части конвейера.</span><span class="sxs-lookup"><span data-stu-id="a798d-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="a798d-129">[Фильтры действий](#action-filters) могут выполнять код непосредственно до и после вызова отдельного метода действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="a798d-130">С их помощью можно управлять аргументами, передаваемыми в действие, и возвращаемым из него результатом.</span><span class="sxs-lookup"><span data-stu-id="a798d-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="a798d-131">Фильтры действий **не** поддерживаются в Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a798d-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="a798d-132">[Фильтры исключений](#exception-filters) служат для применения глобальных политик к необработанным исключениям, которые происходят до записи данных в тело ответа.</span><span class="sxs-lookup"><span data-stu-id="a798d-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="a798d-133">[Фильтры результатов](#result-filters) могут выполнять код непосредственно до и после выполнения результатов отдельного действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="a798d-134">Они выполняются только в том случае, если метод действия выполнен успешно.</span><span class="sxs-lookup"><span data-stu-id="a798d-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="a798d-135">Они полезны для логики, которая должна включать исключения для представлений или модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="a798d-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="a798d-136">На приведенной ниже схеме показано, как типы фильтров взаимодействуют друг с другом в конвейере фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Запрос обрабатывается посредством фильтров авторизации, фильтров ресурсов, привязки модели, фильтров действий, выполнения действия и преобразования результата действия, фильтров исключений, фильтров результатов и выполнения результатов.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="a798d-139">Реализация</span><span class="sxs-lookup"><span data-stu-id="a798d-139">Implementation</span></span>

<span data-ttu-id="a798d-140">Фильтры поддерживают как синхронные, так и асинхронные реализации посредством различных определений интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="a798d-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="a798d-141">Синхронные фильтры могут выполнять код до (`On-Stage-Executing`) и после (`On-Stage-Executed`) этапа конвейера.</span><span class="sxs-lookup"><span data-stu-id="a798d-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="a798d-142">Например, `OnActionExecuting` вызывается перед вызовом метода действия,</span><span class="sxs-lookup"><span data-stu-id="a798d-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="a798d-143">а `OnActionExecuted` — после возврата метода действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="a798d-144">Асинхронные фильтры определяют метод `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="a798d-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="a798d-145">В приведенном выше коде `SampleAsyncActionFilter` включает <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) для выполнения метода действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="a798d-146">Каждый из методов `On-Stage-ExecutionAsync` принимает `FilterType-ExecutionDelegate` для выполнения этапа конвейера фильтра.</span><span class="sxs-lookup"><span data-stu-id="a798d-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="a798d-147">Несколько этапов фильтра</span><span class="sxs-lookup"><span data-stu-id="a798d-147">Multiple filter stages</span></span>

<span data-ttu-id="a798d-148">Реализовать интерфейсы для нескольких этапов фильтра можно в одном классе.</span><span class="sxs-lookup"><span data-stu-id="a798d-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="a798d-149">Например, класс <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> реализует интерфейсы `IActionFilter` и `IResultFilter`, а также их асинхронные эквиваленты.</span><span class="sxs-lookup"><span data-stu-id="a798d-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="a798d-150">Реализуйте синхронный **или** асинхронный интерфейс фильтра, но **не** оба варианта.</span><span class="sxs-lookup"><span data-stu-id="a798d-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="a798d-151">Среда выполнения сначала проверяет, реализует ли фильтр асинхронный интерфейс. Если да, вызывается именно он.</span><span class="sxs-lookup"><span data-stu-id="a798d-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="a798d-152">В противном случае вызываются методы синхронного интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a798d-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="a798d-153">Если асинхронный и синхронный интерфейсы реализованы в одном классе, вызывается только асинхронный метод.</span><span class="sxs-lookup"><span data-stu-id="a798d-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="a798d-154">При использовании абстрактных классов, таких как <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, следует переопределять только синхронные методы или асинхронный метод каждого типа фильтра.</span><span class="sxs-lookup"><span data-stu-id="a798d-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="a798d-155">Встроенные атрибуты фильтров</span><span class="sxs-lookup"><span data-stu-id="a798d-155">Built-in filter attributes</span></span>

<span data-ttu-id="a798d-156">ASP.NET Core включает встроенные фильтры на основе атрибутов, с помощью которых можно создавать подклассы и которые можно настраивать.</span><span class="sxs-lookup"><span data-stu-id="a798d-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="a798d-157">Например, следующий фильтр результатов добавляет заголовок к ответу:</span><span class="sxs-lookup"><span data-stu-id="a798d-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="a798d-158">Атрибуты позволяют фильтрам принимать аргументы, как показано в примере выше.</span><span class="sxs-lookup"><span data-stu-id="a798d-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="a798d-159">Примените `AddHeaderAttribute` к методу контроллера или действия, а затем укажите имя и значение заголовка HTTP:</span><span class="sxs-lookup"><span data-stu-id="a798d-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="a798d-160">Некоторые интерфейсы фильтров имеют соответствующие атрибуты, которые можно использовать как базовые классы для пользовательских реализаций.</span><span class="sxs-lookup"><span data-stu-id="a798d-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="a798d-161">Атрибуты фильтров:</span><span class="sxs-lookup"><span data-stu-id="a798d-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="a798d-162">Области и порядок выполнения фильтров</span><span class="sxs-lookup"><span data-stu-id="a798d-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="a798d-163">Фильтр можно добавить в конвейер в одной из трех *областей*:</span><span class="sxs-lookup"><span data-stu-id="a798d-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="a798d-164">С помощью атрибута в действии.</span><span class="sxs-lookup"><span data-stu-id="a798d-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="a798d-165">С помощью атрибута в контроллере.</span><span class="sxs-lookup"><span data-stu-id="a798d-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="a798d-166">Глобально для всех контроллеров и действий, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="a798d-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="a798d-167">Предыдущий код глобально добавляет три фильтра с помощью коллекции [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="a798d-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="a798d-168">Порядок выполнения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="a798d-168">Default order of execution</span></span>

<span data-ttu-id="a798d-169">Если для определенного этапа конвейера имеется несколько фильтров, область определяет порядок их выполнения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a798d-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="a798d-170">Глобальные фильтры заключают в себя фильтры классов, которые, в свою очередь, заключают в себя фильтры методов.</span><span class="sxs-lookup"><span data-stu-id="a798d-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="a798d-171">В результате такого вложения *последующий* код фильтров выполняется в порядке, обратном выполнению *предшествующего* кода.</span><span class="sxs-lookup"><span data-stu-id="a798d-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="a798d-172">Последовательность фильтров:</span><span class="sxs-lookup"><span data-stu-id="a798d-172">The filter sequence:</span></span>

* <span data-ttu-id="a798d-173">*Предшествующий* код глобальных фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="a798d-174">*Предшествующий* код фильтров контроллера.</span><span class="sxs-lookup"><span data-stu-id="a798d-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="a798d-175">*Предшествующий* код фильтров методов действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="a798d-176">*Последующий* код фильтров методов действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="a798d-177">*Последующий* код фильтров контроллера.</span><span class="sxs-lookup"><span data-stu-id="a798d-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="a798d-178">*Последующий* код глобальных фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="a798d-179">В следующем примере показан порядок вызова методов фильтров для синхронных фильтров действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="a798d-180">Sequence</span><span class="sxs-lookup"><span data-stu-id="a798d-180">Sequence</span></span> | <span data-ttu-id="a798d-181">Область фильтра</span><span class="sxs-lookup"><span data-stu-id="a798d-181">Filter scope</span></span> | <span data-ttu-id="a798d-182">Метод фильтра</span><span class="sxs-lookup"><span data-stu-id="a798d-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="a798d-183">1</span><span class="sxs-lookup"><span data-stu-id="a798d-183">1</span></span> | <span data-ttu-id="a798d-184">Global</span><span class="sxs-lookup"><span data-stu-id="a798d-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="a798d-185">2</span><span class="sxs-lookup"><span data-stu-id="a798d-185">2</span></span> | <span data-ttu-id="a798d-186">Контроллер</span><span class="sxs-lookup"><span data-stu-id="a798d-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="a798d-187">3</span><span class="sxs-lookup"><span data-stu-id="a798d-187">3</span></span> | <span data-ttu-id="a798d-188">Метод</span><span class="sxs-lookup"><span data-stu-id="a798d-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="a798d-189">4</span><span class="sxs-lookup"><span data-stu-id="a798d-189">4</span></span> | <span data-ttu-id="a798d-190">Метод</span><span class="sxs-lookup"><span data-stu-id="a798d-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="a798d-191">5</span><span class="sxs-lookup"><span data-stu-id="a798d-191">5</span></span> | <span data-ttu-id="a798d-192">Контроллер</span><span class="sxs-lookup"><span data-stu-id="a798d-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="a798d-193">6</span><span class="sxs-lookup"><span data-stu-id="a798d-193">6</span></span> | <span data-ttu-id="a798d-194">Global</span><span class="sxs-lookup"><span data-stu-id="a798d-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="a798d-195">Эта последовательность показывает:</span><span class="sxs-lookup"><span data-stu-id="a798d-195">This sequence shows:</span></span>

* <span data-ttu-id="a798d-196">Метод фильтра вкладывается в фильтр контроллера.</span><span class="sxs-lookup"><span data-stu-id="a798d-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="a798d-197">Фильтр контроллера вкладывается в глобальный фильтр.</span><span class="sxs-lookup"><span data-stu-id="a798d-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="a798d-198">Фильтры уровня контроллера и страницы Razor</span><span class="sxs-lookup"><span data-stu-id="a798d-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="a798d-199">Каждый контроллер, наследуемый от базового класса <xref:Microsoft.AspNetCore.Mvc.Controller> включает методы [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) и [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="a798d-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="a798d-200">Эти методы:</span><span class="sxs-lookup"><span data-stu-id="a798d-200">These methods:</span></span>

* <span data-ttu-id="a798d-201">Заключают фильтры, которые выполняются для указанного действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="a798d-202">`OnActionExecuting` вызывается перед всеми фильтрами действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="a798d-203">`OnActionExecuted` вызывается после всех фильтров действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="a798d-204">`OnActionExecutionAsync` вызывается перед всеми фильтрами действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="a798d-205">Код в фильтре после `next` выполняется после метода действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="a798d-206">Например, в скачиваемом примере `MySampleActionFilter` применяется глобально при запуске.</span><span class="sxs-lookup"><span data-stu-id="a798d-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="a798d-207">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="a798d-207">The `TestController`:</span></span>

* <span data-ttu-id="a798d-208">Применяет `SampleActionFilterAttribute` (`[SampleActionFilter]`) для действия `FilterTest2`:</span><span class="sxs-lookup"><span data-stu-id="a798d-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action:</span></span>
* <span data-ttu-id="a798d-209">Переопределяет `OnActionExecuting` и `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="a798d-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="a798d-210">Переходит к `https://localhost:5001/Test/FilterTest2` для выполнения следующего кода:</span><span class="sxs-lookup"><span data-stu-id="a798d-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="a798d-211">См. подробнее о [реализации фильтров страницы Razor путем переопределения методов фильтра](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="a798d-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="a798d-212">Переопределение порядка по умолчанию</span><span class="sxs-lookup"><span data-stu-id="a798d-212">Overriding the default order</span></span>

<span data-ttu-id="a798d-213">Чтобы переопределить порядок выполнения по умолчанию, можно реализовать <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="a798d-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="a798d-214">`IOrderedFilter` предоставляет свойство <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, которое имеет приоритет над областью и определяет порядок выполнения.</span><span class="sxs-lookup"><span data-stu-id="a798d-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="a798d-215">Фильтр со значением меньше `Order`:</span><span class="sxs-lookup"><span data-stu-id="a798d-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="a798d-216">Выполняется *перед* кодом, выполняемым до фильтра с более высоким значением `Order`.</span><span class="sxs-lookup"><span data-stu-id="a798d-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="a798d-217">Выполняется *после* кода, выполняемого после фильтра с более высоким значением `Order`.</span><span class="sxs-lookup"><span data-stu-id="a798d-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="a798d-218">Свойство `Order` можно задать с помощью параметра конструктора:</span><span class="sxs-lookup"><span data-stu-id="a798d-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="a798d-219">Рассмотрим три фильтра действий, показанные в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="a798d-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="a798d-220">Если свойство `Order` контроллера и глобальные фильтры имеют значения 1 и 2 соответственно, порядок выполнения инвертируется.</span><span class="sxs-lookup"><span data-stu-id="a798d-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="a798d-221">Sequence</span><span class="sxs-lookup"><span data-stu-id="a798d-221">Sequence</span></span> | <span data-ttu-id="a798d-222">Область фильтра</span><span class="sxs-lookup"><span data-stu-id="a798d-222">Filter scope</span></span> | <span data-ttu-id="a798d-223">Свойство`Order`</span><span class="sxs-lookup"><span data-stu-id="a798d-223">`Order` property</span></span> | <span data-ttu-id="a798d-224">Метод фильтра</span><span class="sxs-lookup"><span data-stu-id="a798d-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="a798d-225">1</span><span class="sxs-lookup"><span data-stu-id="a798d-225">1</span></span> | <span data-ttu-id="a798d-226">Метод</span><span class="sxs-lookup"><span data-stu-id="a798d-226">Method</span></span> | <span data-ttu-id="a798d-227">0</span><span class="sxs-lookup"><span data-stu-id="a798d-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="a798d-228">2</span><span class="sxs-lookup"><span data-stu-id="a798d-228">2</span></span> | <span data-ttu-id="a798d-229">Контроллер</span><span class="sxs-lookup"><span data-stu-id="a798d-229">Controller</span></span> | <span data-ttu-id="a798d-230">1</span><span class="sxs-lookup"><span data-stu-id="a798d-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="a798d-231">3</span><span class="sxs-lookup"><span data-stu-id="a798d-231">3</span></span> | <span data-ttu-id="a798d-232">Global</span><span class="sxs-lookup"><span data-stu-id="a798d-232">Global</span></span> | <span data-ttu-id="a798d-233">2</span><span class="sxs-lookup"><span data-stu-id="a798d-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="a798d-234">4</span><span class="sxs-lookup"><span data-stu-id="a798d-234">4</span></span> | <span data-ttu-id="a798d-235">Global</span><span class="sxs-lookup"><span data-stu-id="a798d-235">Global</span></span> | <span data-ttu-id="a798d-236">2</span><span class="sxs-lookup"><span data-stu-id="a798d-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="a798d-237">5</span><span class="sxs-lookup"><span data-stu-id="a798d-237">5</span></span> | <span data-ttu-id="a798d-238">Контроллер</span><span class="sxs-lookup"><span data-stu-id="a798d-238">Controller</span></span> | <span data-ttu-id="a798d-239">1</span><span class="sxs-lookup"><span data-stu-id="a798d-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="a798d-240">6</span><span class="sxs-lookup"><span data-stu-id="a798d-240">6</span></span> | <span data-ttu-id="a798d-241">Метод</span><span class="sxs-lookup"><span data-stu-id="a798d-241">Method</span></span> | <span data-ttu-id="a798d-242">0</span><span class="sxs-lookup"><span data-stu-id="a798d-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="a798d-243">Свойство `Order` переопределяет область при определении порядка выполнения фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="a798d-244">Фильтры сначала сортируются по порядку, а затем очередность окончательно определяется по области.</span><span class="sxs-lookup"><span data-stu-id="a798d-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="a798d-245">Все встроенные фильтры реализуют `IOrderedFilter` и задают значение по умолчанию `Order`, равное 0.</span><span class="sxs-lookup"><span data-stu-id="a798d-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="a798d-246">Во встроенных фильтрах область определяет порядок, если для `Order` не задано ненулевое значение.</span><span class="sxs-lookup"><span data-stu-id="a798d-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="a798d-247">Отмена и сокращенное выполнение</span><span class="sxs-lookup"><span data-stu-id="a798d-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="a798d-248">Вы можете настроить сокращенное выполнение конвейера фильтров, задав свойство <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> для параметра <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext>, передаваемого в метод фильтра.</span><span class="sxs-lookup"><span data-stu-id="a798d-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="a798d-249">Например, приведенный ниже фильтр ресурсов предотвращает выполнение остальной части конвейера:</span><span class="sxs-lookup"><span data-stu-id="a798d-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="a798d-250">В приведенном ниже коде как фильтр `ShortCircuitingResourceFilter`, так и фильтр `AddHeader` нацелены на метод действия `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="a798d-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="a798d-251">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="a798d-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="a798d-252">Выполняется первым, поскольку это фильтр ресурсов, а `AddHeader` — фильтр действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="a798d-253">Замыкает оставшуюся часть конвейера.</span><span class="sxs-lookup"><span data-stu-id="a798d-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="a798d-254">Поэтому фильтр `AddHeader` никогда не выполняется для действия `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="a798d-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="a798d-255">Поведение будет аналогичным при применении обоих фильтров на уровне метода действия при условии, что фильтр `ShortCircuitingResourceFilter` выполняется первым.</span><span class="sxs-lookup"><span data-stu-id="a798d-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="a798d-256">Фильтр `ShortCircuitingResourceFilter` выполняется в первую очередь из-за его типа или в связи с явным указанием свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="a798d-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="a798d-257">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="a798d-257">Dependency injection</span></span>

<span data-ttu-id="a798d-258">Фильтры можно добавлять по типу или экземпляру.</span><span class="sxs-lookup"><span data-stu-id="a798d-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="a798d-259">Добавляемый экземпляр используется для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="a798d-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="a798d-260">Если добавляется тип, он является активированным.</span><span class="sxs-lookup"><span data-stu-id="a798d-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="a798d-261">Активация фильтра означает, что:</span><span class="sxs-lookup"><span data-stu-id="a798d-261">A type-activated filter means:</span></span>

* <span data-ttu-id="a798d-262">Экземпляр создается для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="a798d-262">An instance is created for each request.</span></span>
* <span data-ttu-id="a798d-263">Зависимости конструктора заполняются путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a798d-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="a798d-264">Фильтры, которые реализуются как атрибуты и добавляются непосредственно в классы контроллеров или методы действий, не могут иметь зависимости конструктора, предоставленные посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a798d-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="a798d-265">Зависимости конструктора не могут предоставляться путем внедрения зависимостей, так как:</span><span class="sxs-lookup"><span data-stu-id="a798d-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="a798d-266">Параметры конструктора должны предоставляться для атрибутов в месте их применения.</span><span class="sxs-lookup"><span data-stu-id="a798d-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="a798d-267">Это ограничение, налагаемое на использование атрибутов.</span><span class="sxs-lookup"><span data-stu-id="a798d-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="a798d-268">Следующие фильтры поддерживают зависимости конструктора, предоставленные путем внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="a798d-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="a798d-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> реализуется в атрибуте.</span><span class="sxs-lookup"><span data-stu-id="a798d-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="a798d-270">Предыдущие фильтры могут применяться к контроллеру или методу действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="a798d-271">Средства ведения журнала доступны путем внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a798d-271">Loggers are available from DI.</span></span> <span data-ttu-id="a798d-272">Но следует избегать создания и использования фильтров исключительно для целей ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="a798d-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="a798d-273">[Встроенное средство ведения журнала](xref:fundamentals/logging/index) обычно предоставляет все необходимое для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="a798d-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="a798d-274">При добавлении ведения журналов в фильтры следует учитывать следующее:</span><span class="sxs-lookup"><span data-stu-id="a798d-274">Logging added to filters:</span></span>

* <span data-ttu-id="a798d-275">Следует сосредоточиться на бизнес-среде или поведении в привязке к фильтру.</span><span class="sxs-lookup"><span data-stu-id="a798d-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="a798d-276">**Не** следует регистрировать действия или другие события платформы,</span><span class="sxs-lookup"><span data-stu-id="a798d-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="a798d-277">так как это делают встроенные фильтры.</span><span class="sxs-lookup"><span data-stu-id="a798d-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="a798d-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="a798d-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="a798d-279">Типы реализации фильтра службы регистрируются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a798d-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="a798d-280">Атрибут <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> извлекает экземпляр фильтра из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a798d-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="a798d-281">В следующем коде используется `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="a798d-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="a798d-282">В следующем коде в контейнер внедрения зависимостей добавляется `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="a798d-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="a798d-283">В следующем коде атрибут `ServiceFilter` извлекает экземпляр фильтра `AddHeaderResultServiceFilter` из внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="a798d-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="a798d-284">При использовании `ServiceFilterAttribute` задание [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="a798d-284">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="a798d-285">Указывает, что экземпляр фильтра *можно* многократно использовать за пределами области запроса, в которой он был создан.</span><span class="sxs-lookup"><span data-stu-id="a798d-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="a798d-286">Среда выполнения ASP.NET Core не гарантирует:</span><span class="sxs-lookup"><span data-stu-id="a798d-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="a798d-287">что будет создан хотя бы один экземпляр фильтра;</span><span class="sxs-lookup"><span data-stu-id="a798d-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="a798d-288">что фильтр не будет повторно запрошен из контейнера внедрения зависимостей на более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="a798d-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="a798d-289">Не следует использовать с фильтром, который зависит от служб со временем существования, кроме элементов singleton.</span><span class="sxs-lookup"><span data-stu-id="a798d-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="a798d-290">Объект <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> реализует интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="a798d-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="a798d-291">`IFilterFactory` предоставляет метод <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> для создания экземпляра <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="a798d-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="a798d-292">`CreateInstance` загружает указанный тип из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a798d-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="a798d-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="a798d-293">TypeFilterAttribute</span></span>

<span data-ttu-id="a798d-294">Атрибут <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> похож на <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, но его тип не разрешается напрямую из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a798d-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="a798d-295">Он создает экземпляр типа с помощью <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="a798d-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="a798d-296">Так как типы `TypeFilterAttribute` не разрешаются напрямую из контейнера внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="a798d-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="a798d-297">Типы, на которые ссылаются с помощью `TypeFilterAttribute`, не нужно регистрировать в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a798d-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="a798d-298">Их зависимости выполняются самим контейнером.</span><span class="sxs-lookup"><span data-stu-id="a798d-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="a798d-299">Атрибут `TypeFilterAttribute` может также принимать аргументы конструктора для типа.</span><span class="sxs-lookup"><span data-stu-id="a798d-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="a798d-300">При использовании `TypeFilterAttribute` задание [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="a798d-300">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="a798d-301">Указывает, что экземпляр фильтра *можно* многократно использовать за пределами области запроса, в которой он был создан.</span><span class="sxs-lookup"><span data-stu-id="a798d-301">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="a798d-302">Среда выполнения ASP.NET Core не предоставляет никаких гарантий, что будет создан хотя бы один экземпляр фильтра.</span><span class="sxs-lookup"><span data-stu-id="a798d-302">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="a798d-303">Не следует использовать с фильтром, который зависит от служб со временем существования, кроме элементов singleton.</span><span class="sxs-lookup"><span data-stu-id="a798d-303">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="a798d-304">В следующем примере показано, как передавать аргументы в тип с помощью `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="a798d-304">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="a798d-305">Фильтры авторизации</span><span class="sxs-lookup"><span data-stu-id="a798d-305">Authorization filters</span></span>

<span data-ttu-id="a798d-306">Фильтры авторизации:</span><span class="sxs-lookup"><span data-stu-id="a798d-306">Authorization filters:</span></span>

* <span data-ttu-id="a798d-307">Выполняются в первую очередь в конвейере фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-307">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="a798d-308">Контролируют доступ к методам действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-308">Control access to action methods.</span></span>
* <span data-ttu-id="a798d-309">Имеют предшествующий, но не последующий метод.</span><span class="sxs-lookup"><span data-stu-id="a798d-309">Have a before method, but no after method.</span></span>

<span data-ttu-id="a798d-310">Для работы пользовательских фильтров авторизации требуется настраиваемая платформа авторизации.</span><span class="sxs-lookup"><span data-stu-id="a798d-310">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="a798d-311">Настройка политик авторизации или определение пользовательской политики авторизации предпочтительнее создания пользовательского фильтра.</span><span class="sxs-lookup"><span data-stu-id="a798d-311">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="a798d-312">Встроенный фильтр авторизации:</span><span class="sxs-lookup"><span data-stu-id="a798d-312">The built-in authorization filter:</span></span>

* <span data-ttu-id="a798d-313">Вызывает систему авторизации.</span><span class="sxs-lookup"><span data-stu-id="a798d-313">Calls the authorization system.</span></span>
* <span data-ttu-id="a798d-314">Не выполняет авторизацию запросов.</span><span class="sxs-lookup"><span data-stu-id="a798d-314">Does not authorize requests.</span></span>

<span data-ttu-id="a798d-315">**Не** вызывайте исключения в фильтрах авторизации:</span><span class="sxs-lookup"><span data-stu-id="a798d-315">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="a798d-316">Исключение не будет обработано.</span><span class="sxs-lookup"><span data-stu-id="a798d-316">The exception will not be handled.</span></span>
* <span data-ttu-id="a798d-317">Фильтры исключений не будут обрабатывать исключение.</span><span class="sxs-lookup"><span data-stu-id="a798d-317">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="a798d-318">При возникновении исключения в фильтре авторизации попробуйте создать запрос.</span><span class="sxs-lookup"><span data-stu-id="a798d-318">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="a798d-319">Дополнительные сведения об [авторизации](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="a798d-319">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="a798d-320">Фильтры ресурсов</span><span class="sxs-lookup"><span data-stu-id="a798d-320">Resource filters</span></span>

<span data-ttu-id="a798d-321">Фильтры ресурсов:</span><span class="sxs-lookup"><span data-stu-id="a798d-321">Resource filters:</span></span>

* <span data-ttu-id="a798d-322">Реализуют либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter>, либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="a798d-322">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="a798d-323">Выполнение охватывает большую часть конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-323">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="a798d-324">До фильтров ресурсов применяются только [фильтры авторизации](#authorization-filters).</span><span class="sxs-lookup"><span data-stu-id="a798d-324">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="a798d-325">Фильтры ресурсов полезны для сокращенного выполнения большей части конвейера.</span><span class="sxs-lookup"><span data-stu-id="a798d-325">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="a798d-326">Например, фильтр кэширования предотвращает выполнение остальной части конвейера при попадании в кэше.</span><span class="sxs-lookup"><span data-stu-id="a798d-326">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="a798d-327">Примеры фильтров ресурсов:</span><span class="sxs-lookup"><span data-stu-id="a798d-327">Resource filter examples:</span></span>

* <span data-ttu-id="a798d-328">[Сокращенное выполнение фильтра ресурсов](#short-circuiting-resource-filter) показано ранее.</span><span class="sxs-lookup"><span data-stu-id="a798d-328">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="a798d-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="a798d-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="a798d-330">Предотвращает доступ привязки модели к данным формы.</span><span class="sxs-lookup"><span data-stu-id="a798d-330">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="a798d-331">Используется для загрузки больших файлов, если необходимо предотвратить считывание данных формы в память.</span><span class="sxs-lookup"><span data-stu-id="a798d-331">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="a798d-332">Фильтры действий</span><span class="sxs-lookup"><span data-stu-id="a798d-332">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a798d-333">Фильтры действий **неприменимы** к Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a798d-333">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="a798d-334">Razor Pages поддерживает <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> и <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="a798d-334">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="a798d-335">Дополнительные сведения см. в разделе [Методы фильтрации для Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="a798d-335">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="a798d-336">Фильтры действий:</span><span class="sxs-lookup"><span data-stu-id="a798d-336">Action filters:</span></span>

* <span data-ttu-id="a798d-337">Реализуют либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter>, либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="a798d-337">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="a798d-338">Их выполнение охватывает выполнение методов действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-338">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="a798d-339">В следующем фрагменте кода показан пример фильтра действий:</span><span class="sxs-lookup"><span data-stu-id="a798d-339">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="a798d-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> предоставляет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="a798d-340">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="a798d-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> позволяет считать входные данные в метод действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="a798d-342"><xref:Microsoft.AspNetCore.Mvc.Controller> позволяет управлять экземпляром контроллера.</span><span class="sxs-lookup"><span data-stu-id="a798d-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="a798d-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> позволяет при задании свойства `Result` сократить выполнение метода действия и последующих фильтров действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="a798d-344">Вызов исключения в методе действия:</span><span class="sxs-lookup"><span data-stu-id="a798d-344">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="a798d-345">Предотвращает выполнение последующих фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-345">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="a798d-346">В отличие от параметра `Result` рассматривается как сбой, а не успешный результат.</span><span class="sxs-lookup"><span data-stu-id="a798d-346">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="a798d-347"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> предоставляет `Controller` и `Result`, а также следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="a798d-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="a798d-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> будет иметь значение true, если выполнение действия было сокращено другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="a798d-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="a798d-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> будет иметь ненулевое значение, если предыдущее выполнение фильтра действий вызвало исключение.</span><span class="sxs-lookup"><span data-stu-id="a798d-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="a798d-350">При присвоении этому свойству ненулевого значения:</span><span class="sxs-lookup"><span data-stu-id="a798d-350">Setting this property to null:</span></span>

  * <span data-ttu-id="a798d-351">Будет эффективно обрабатываться исключение.</span><span class="sxs-lookup"><span data-stu-id="a798d-351">Effectively handles the exception.</span></span>
  * <span data-ttu-id="a798d-352">`Result` будет выполняться так, как при возвращении методом действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-352">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="a798d-353">Для `IAsyncActionFilter` вызов <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="a798d-353">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="a798d-354">Приводит к выполнению последующих фильтров действий и метода действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-354">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="a798d-355">Возвращает `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="a798d-355">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="a798d-356">Для сокращенного выполнения присвойте <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> экземпляру результата и не вызывайте `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="a798d-356">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="a798d-357">Платформа предоставляет абстрактный класс <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, для которого можно создавать подклассы.</span><span class="sxs-lookup"><span data-stu-id="a798d-357">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="a798d-358">Фильтр действий `OnActionExecuting` можно использовать:</span><span class="sxs-lookup"><span data-stu-id="a798d-358">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="a798d-359">Для проверки состояния модели.</span><span class="sxs-lookup"><span data-stu-id="a798d-359">Validate model state.</span></span>
* <span data-ttu-id="a798d-360">Для получения ошибки, если состояние является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="a798d-360">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="a798d-361">Метод `OnActionExecuted` выполняется после метода действия:</span><span class="sxs-lookup"><span data-stu-id="a798d-361">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="a798d-362">Он имеет доступ к результатам действия и может управлять ими с помощью свойства <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="a798d-362">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="a798d-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> будет иметь значение true, если выполнение действия было сокращено другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="a798d-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="a798d-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> будет иметь ненулевое значение, если действие или последующий фильтр действий вызвали исключение.</span><span class="sxs-lookup"><span data-stu-id="a798d-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="a798d-365">Если установить для `Exception` значение NULL:</span><span class="sxs-lookup"><span data-stu-id="a798d-365">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="a798d-366">Будет эффективно обрабатываться исключение.</span><span class="sxs-lookup"><span data-stu-id="a798d-366">Effectively handles an exception.</span></span>
  * <span data-ttu-id="a798d-367">`ActionExecutedContext.Result` будет выполняться так, как если бы метод действия вернул его обычным образом.</span><span class="sxs-lookup"><span data-stu-id="a798d-367">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="a798d-368">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="a798d-368">Exception filters</span></span>

<span data-ttu-id="a798d-369">Фильтры исключений:</span><span class="sxs-lookup"><span data-stu-id="a798d-369">Exception filters:</span></span>

* <span data-ttu-id="a798d-370">Реализуют <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="a798d-370">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="a798d-371">Можно использовать для реализации политик обработки стандартных ошибок.</span><span class="sxs-lookup"><span data-stu-id="a798d-371">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="a798d-372">В следующем примере фильтра исключений используется пользовательское представление ошибок для отображения подробных сведений об исключениях, которые происходят во время разработки приложения:</span><span class="sxs-lookup"><span data-stu-id="a798d-372">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="a798d-373">Фильтры исключений:</span><span class="sxs-lookup"><span data-stu-id="a798d-373">Exception filters:</span></span>

* <span data-ttu-id="a798d-374">Не имеют предшествующих и последующих событий.</span><span class="sxs-lookup"><span data-stu-id="a798d-374">Don't have before and after events.</span></span>
* <span data-ttu-id="a798d-375">Реализуют <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="a798d-375">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="a798d-376">Обрабатывают необработанные исключения, которые возникают при создании страницы Razor или контроллера, в [привязке модели](xref:mvc/models/model-binding), фильтрах действий или методах действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-376">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="a798d-377">**Не** перехватывают исключения, которые возникают в фильтрах ресурсов, фильтрах результатов или при выполнении результата MVC.</span><span class="sxs-lookup"><span data-stu-id="a798d-377">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="a798d-378">Для обработки исключения присвойте свойству <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> значение `true` или напишите ответ.</span><span class="sxs-lookup"><span data-stu-id="a798d-378">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="a798d-379">Это предотвратит распространение исключения.</span><span class="sxs-lookup"><span data-stu-id="a798d-379">This stops propagation of the exception.</span></span> <span data-ttu-id="a798d-380">Фильтр исключений не может преобразовать исключение в успешное выполнение.</span><span class="sxs-lookup"><span data-stu-id="a798d-380">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="a798d-381">Это может сделать только фильтр действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-381">Only an action filter can do that.</span></span>

<span data-ttu-id="a798d-382">Фильтры исключений:</span><span class="sxs-lookup"><span data-stu-id="a798d-382">Exception filters:</span></span>

* <span data-ttu-id="a798d-383">Хорошо подходят для перехвата исключений, возникающих в действиях.</span><span class="sxs-lookup"><span data-stu-id="a798d-383">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="a798d-384">Не обладает такой гибкостью, как ПО промежуточного слоя обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="a798d-384">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="a798d-385">ПО промежуточного слоя хорошо подходит для обработки исключений.</span><span class="sxs-lookup"><span data-stu-id="a798d-385">Prefer middleware for exception handling.</span></span> <span data-ttu-id="a798d-386">Используйте фильтры исключений, только если обработка ошибок осуществляется *по-разному* с учетом вызванного метода действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-386">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="a798d-387">Например, в приложении могут использоваться методы действий как для конечных точек API, так и для представлений или HTML.</span><span class="sxs-lookup"><span data-stu-id="a798d-387">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="a798d-388">Конечные точки API могут возвращать сведения об ошибках в формате JSON, в то время как действия на основе представлений могут возвращать страницу ошибки в формате HTML.</span><span class="sxs-lookup"><span data-stu-id="a798d-388">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="a798d-389">Фильтры результатов</span><span class="sxs-lookup"><span data-stu-id="a798d-389">Result filters</span></span>

<span data-ttu-id="a798d-390">Фильтры результатов:</span><span class="sxs-lookup"><span data-stu-id="a798d-390">Result filters:</span></span>

* <span data-ttu-id="a798d-391">Реализация интерфейса:</span><span class="sxs-lookup"><span data-stu-id="a798d-391">Implement an interface:</span></span>
  * <span data-ttu-id="a798d-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="a798d-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="a798d-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="a798d-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="a798d-394">Их выполнение охватывает выполнение результатов действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-394">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="a798d-395">IResultFilter и IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="a798d-395">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="a798d-396">В следующем фрагменте кода показан фильтр результатов, который добавляет заголовок HTTP:</span><span class="sxs-lookup"><span data-stu-id="a798d-396">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="a798d-397">Тип выполняемого результата зависит от соответствующего действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-397">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="a798d-398">Действие, возвращающее представление, будет включать всю обработку Razor в рамках выполняемого объекта <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="a798d-398">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="a798d-399">В процессе выполнения результата метод API может производить сериализацию.</span><span class="sxs-lookup"><span data-stu-id="a798d-399">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="a798d-400">Дополнительные сведения о [результатах действий](xref:mvc/controllers/actions)</span><span class="sxs-lookup"><span data-stu-id="a798d-400">Learn more about [action results](xref:mvc/controllers/actions)</span></span>

<span data-ttu-id="a798d-401">Фильтры результатов выполняются только в случае успешных результатов — когда действие или фильтры действий предоставляют результат действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-401">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="a798d-402">Фильтры результатов не выполняются, когда фильтры исключений обрабатывают исключение.</span><span class="sxs-lookup"><span data-stu-id="a798d-402">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="a798d-403">Метод <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> может сокращать выполнение результата действия и последующих фильтров результатов, присваивая свойству <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> значение `true`.</span><span class="sxs-lookup"><span data-stu-id="a798d-403">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="a798d-404">При сокращении выполнения выполните запись в объект ответа, чтобы избежать формирования пустого ответа.</span><span class="sxs-lookup"><span data-stu-id="a798d-404">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="a798d-405">Вызов исключения в `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="a798d-405">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="a798d-406">Предотвращает выполнение результата действия и последующих фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-406">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="a798d-407">Рассматривается как сбой, а не успешный результат.</span><span class="sxs-lookup"><span data-stu-id="a798d-407">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="a798d-408">При выполнении метода <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="a798d-408">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs:</span></span>

* <span data-ttu-id="a798d-409">Ответ, скорее всего, был отправлен клиенту и его нельзя изменить.</span><span class="sxs-lookup"><span data-stu-id="a798d-409">The response has likely been sent to the client and cannot be changed.</span></span>
* <span data-ttu-id="a798d-410">Если возникло исключение, текст ответа не отправляется.</span><span class="sxs-lookup"><span data-stu-id="a798d-410">If an exception was thrown, the response body is not sent.</span></span>

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

<span data-ttu-id="a798d-411">`ResultExecutedContext.Canceled` будет иметь значение `true`, если выполнение результата действия было сокращено другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="a798d-411">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="a798d-412">`ResultExecutedContext.Exception` будет иметь ненулевое значение, если результат действия или последующий фильтр результатов вызвали исключение.</span><span class="sxs-lookup"><span data-stu-id="a798d-412">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="a798d-413">Присвоение `Exception` значения NULL приводит к обработке исключения и предотвращает его последующий вызов ASP.NET Core на дальнейших этапах конвейера.</span><span class="sxs-lookup"><span data-stu-id="a798d-413">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="a798d-414">Надежный способ записи данных в ответ при обработке исключения в фильтре результатов отсутствует.</span><span class="sxs-lookup"><span data-stu-id="a798d-414">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="a798d-415">Если результат действия вызывает исключение в процессе выполнения и заголовки уже были переданы в клиент, надежного механизма отправки кода сбоя не существует.</span><span class="sxs-lookup"><span data-stu-id="a798d-415">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="a798d-416">Для <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> вызов к `await next` для <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> приводит к выполнению последующих фильтров результатов и результата действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-416">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="a798d-417">Для сокращения выполнения задайте [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) для `true` и не вызывайте `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="a798d-417">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="a798d-418">Платформа предоставляет абстрактный класс `ResultFilterAttribute`, для которого можно создавать подклассы.</span><span class="sxs-lookup"><span data-stu-id="a798d-418">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="a798d-419">Представленный ранее класс [AddHeaderAttribute](#add-header-attribute) — это пример атрибута фильтра результатов.</span><span class="sxs-lookup"><span data-stu-id="a798d-419">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="a798d-420">IAlwaysRunResultFilter и IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="a798d-420">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="a798d-421">Интерфейсы <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> и <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> объявляют реализацию <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>, которая выполняется для всех результатов действий.</span><span class="sxs-lookup"><span data-stu-id="a798d-421">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="a798d-422">Фильтр применяется для всех результатов действий, если:</span><span class="sxs-lookup"><span data-stu-id="a798d-422">The filter is applied to all action results unless:</span></span>

* <span data-ttu-id="a798d-423"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> применяется и сокращает ответ.</span><span class="sxs-lookup"><span data-stu-id="a798d-423">An <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> applies and short-circuits the response.</span></span>
* <span data-ttu-id="a798d-424">Фильтр исключений обрабатывает исключение и выдает результат действия.</span><span class="sxs-lookup"><span data-stu-id="a798d-424">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="a798d-425">Фильтры, отличающиеся от `IExceptionFilter` и `IAuthorizationFilter`, не сокращают `IAlwaysRunResultFilter` и `IAsyncAlwaysRunResultFilter`.</span><span class="sxs-lookup"><span data-stu-id="a798d-425">Filters other than `IExceptionFilter` and `IAuthorizationFilter` don't short-circuit `IAlwaysRunResultFilter` and `IAsyncAlwaysRunResultFilter`.</span></span>

<span data-ttu-id="a798d-426">Например, следующий фильтр всегда выполняется, задавая результат действия (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) с кодом состояния *422 Unprocessable Entity* при сбое согласования содержимого:</span><span class="sxs-lookup"><span data-stu-id="a798d-426">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="a798d-427">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="a798d-427">IFilterFactory</span></span>

<span data-ttu-id="a798d-428">Объект <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> реализует интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="a798d-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="a798d-429">Поэтому экземпляр `IFilterFactory` можно использовать в качестве экземпляра `IFilterMetadata` в любом месте конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-429">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="a798d-430">Когда среда выполнения готовится вызвать фильтр, она пытается привести его к `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="a798d-430">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="a798d-431">Если приведение проходит успешно, вызывается метод <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> для создания вызываемого экземпляра `IFilterMetadata`.</span><span class="sxs-lookup"><span data-stu-id="a798d-431">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="a798d-432">Это обеспечивает высокую степень гибкости, так как при запуске приложения нет необходимости в явном и точном определении конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-432">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="a798d-433">Еще один подход к созданию фильтров заключается в реализации `IFilterFactory` с помощью настраиваемых атрибутов:</span><span class="sxs-lookup"><span data-stu-id="a798d-433">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="a798d-434">Приведенный выше код можно проверить, выполнив [скачиваемый пример](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="a798d-434">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="a798d-435">Вызовите средства для разработчика (F12).</span><span class="sxs-lookup"><span data-stu-id="a798d-435">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="a798d-436">Перейдите к папке `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="a798d-436">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`</span></span>

<span data-ttu-id="a798d-437">В средствах для разработчика (F12) отобразятся следующие заголовки ответа, добавленные примером кода:</span><span class="sxs-lookup"><span data-stu-id="a798d-437">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="a798d-438">**author:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="a798d-438">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="a798d-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="a798d-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="a798d-440">**internal:** `My header`</span><span class="sxs-lookup"><span data-stu-id="a798d-440">**internal:** `My header`</span></span>

<span data-ttu-id="a798d-441">Приведенный выше код создает заголовок ответа **internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="a798d-441">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="a798d-442">Реализация IFilterFactory в атрибуте</span><span class="sxs-lookup"><span data-stu-id="a798d-442">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="a798d-443">Фильтры, реализующие `IFilterFactory`, используются для фильтров, которые:</span><span class="sxs-lookup"><span data-stu-id="a798d-443">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="a798d-444">Не требуют передачи параметров.</span><span class="sxs-lookup"><span data-stu-id="a798d-444">Don't require passing parameters.</span></span>
* <span data-ttu-id="a798d-445">Содержат зависимости конструктора, которые должны заполняться путем внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a798d-445">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="a798d-446">Объект <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> реализует интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="a798d-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="a798d-447">`IFilterFactory` предоставляет метод <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> для создания экземпляра <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="a798d-447">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="a798d-448">`CreateInstance` загружает указанный тип из контейнера служб (внедрение зависимостей).</span><span class="sxs-lookup"><span data-stu-id="a798d-448">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="a798d-449">В коде ниже приведено три примера применения `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="a798d-449">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="a798d-450">В приведенном выше коде декорирование метода с использованием `[SampleActionFilter]` является рекомендуемым способом применения `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="a798d-450">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="a798d-451">Использование ПО промежуточного слоя в конвейере фильтров</span><span class="sxs-lookup"><span data-stu-id="a798d-451">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="a798d-452">Фильтры ресурсов по принципу работы похожи на [ПО промежуточного слоя](xref:fundamentals/middleware/index) тем, что они заключают в себя выполнение всех объектов, находящихся далее в конвейере.</span><span class="sxs-lookup"><span data-stu-id="a798d-452">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="a798d-453">При этом фильтры отличаются от ПО промежуточного слоя тем, что они являются частью среды выполнения ASP.NET Core, то есть они имеют доступ к контексту и конструкциям ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a798d-453">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="a798d-454">Чтобы использовать ПО промежуточного слоя в качестве фильтра, создайте тип с методом `Configure`, определяющим ПО промежуточного слоя, которое нужно внедрить в конвейер фильтров.</span><span class="sxs-lookup"><span data-stu-id="a798d-454">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="a798d-455">В примере ниже ПО промежуточного слоя локализации применяется для определения текущих языка и региональных параметров для запроса:</span><span class="sxs-lookup"><span data-stu-id="a798d-455">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="a798d-456">Используйте <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> для выполнения ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="a798d-456">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="a798d-457">Фильтры ПО промежуточного слоя выполняются на том же этапе конвейера фильтров, что и фильтры ресурсов, перед привязкой модели и после остальной части конвейера.</span><span class="sxs-lookup"><span data-stu-id="a798d-457">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="a798d-458">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="a798d-458">Next actions</span></span>

* <span data-ttu-id="a798d-459">Дополнительные сведения см. в статье [Filter methods for Razor Pages in ASP.NET Core](xref:razor-pages/filter) (Методы фильтрации для Razor Pages в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="a798d-459">See [Filter methods for Razor Pages](xref:razor-pages/filter)</span></span>
* <span data-ttu-id="a798d-460">Чтобы поэкспериментировать с фильтрами, [скачайте, протестируйте и измените пример с GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="a798d-460">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
