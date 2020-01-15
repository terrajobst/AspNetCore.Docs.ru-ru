---
title: Фильтры в ASP.NET Core
author: Rick-Anderson
description: Из этой статьи вы узнаете, как работают фильтры и как их использовать в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 1/1/2020
uid: mvc/controllers/filters
ms.openlocfilehash: 2300b14a6a89191d3d8c673311880fc144183da9
ms.sourcegitcommit: e7d4fe6727d423f905faaeaa312f6c25ef844047
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/02/2020
ms.locfileid: "75608133"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="b1b93-103">Фильтры в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1b93-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1b93-104">Авторы: [Кирк Ларкин](https://github.com/serpent5) (Kirk Larkin) [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Том Дайкстра](https://github.com/tdykstra/) (Tom Dykstra) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="b1b93-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b1b93-105">*Фильтры* в ASP.NET Core позволяют выполнять код до или после определенных этапов в конвейере обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="b1b93-106">Встроенные фильтры обрабатывают следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="b1b93-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="b1b93-107">Авторизация (предотвращение несанкционированного доступа к ресурсам).</span><span class="sxs-lookup"><span data-stu-id="b1b93-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="b1b93-108">Кэширование откликов (замыкание конвейера обработки запросов для возврата кэшированного ответа).</span><span class="sxs-lookup"><span data-stu-id="b1b93-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="b1b93-109">Для обработки сквозной функциональности можно создавать настраиваемые фильтры.</span><span class="sxs-lookup"><span data-stu-id="b1b93-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="b1b93-110">Примеры сквозной функциональности включают обработку ошибок, кэширование, конфигурирование, авторизацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="b1b93-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="b1b93-111">Фильтры предотвращают дублирование кода.</span><span class="sxs-lookup"><span data-stu-id="b1b93-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="b1b93-112">Например, можно объединить обработку ошибок с помощью фильтра исключений обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="b1b93-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="b1b93-113">Этот документ применим к Razor Pages, контроллерам API и контроллерам с представлениями.</span><span class="sxs-lookup"><span data-stu-id="b1b93-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="b1b93-114">[Просмотреть или скачать пример](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([как скачивать](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b1b93-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="b1b93-115">Как работают фильтры</span><span class="sxs-lookup"><span data-stu-id="b1b93-115">How filters work</span></span>

<span data-ttu-id="b1b93-116">Фильтры выполняются в *конвейере вызова действий ASP.NET Core*, который иногда называют *конвейером фильтров*.</span><span class="sxs-lookup"><span data-stu-id="b1b93-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="b1b93-117">Конвейер фильтров запускается после того, как платформа ASP.NET Core выбирает выполняемое действие.</span><span class="sxs-lookup"><span data-stu-id="b1b93-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Запрос обрабатывается с помощью прочего ПО промежуточного слоя, ПО промежуточного слоя маршрутизации, выбора действия и конвейера вызова действий.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="b1b93-120">Типы фильтров</span><span class="sxs-lookup"><span data-stu-id="b1b93-120">Filter types</span></span>

<span data-ttu-id="b1b93-121">Фильтр каждого типа выполняется на определенном этапе конвейера фильтров:</span><span class="sxs-lookup"><span data-stu-id="b1b93-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="b1b93-122">[Фильтры авторизации](#authorization-filters). Применяются в первую очередь и служат для определения того, авторизован ли пользователь для выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="b1b93-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="b1b93-123">Эти фильтры могут сократить выполнение конвейера, если запрос не авторизован.</span><span class="sxs-lookup"><span data-stu-id="b1b93-123">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="b1b93-124">[Фильтры ресурсов](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="b1b93-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="b1b93-125">Выполняются после авторизации.</span><span class="sxs-lookup"><span data-stu-id="b1b93-125">Run after authorization.</span></span>  
  * <span data-ttu-id="b1b93-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> выполняет код до остальной части конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="b1b93-127">Например, `OnResourceExecuting` выполняет код до привязки модели.</span><span class="sxs-lookup"><span data-stu-id="b1b93-127">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="b1b93-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> выполняет код после завершения остальной части конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="b1b93-129">[Фильтры действий](#action-filters):</span><span class="sxs-lookup"><span data-stu-id="b1b93-129">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="b1b93-130">выполняют код непосредственно до и после вызова метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-130">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="b1b93-131">Могут изменять аргументы, передаваемые в действие.</span><span class="sxs-lookup"><span data-stu-id="b1b93-131">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="b1b93-132">Могут изменять результат, возвращенный действием.</span><span class="sxs-lookup"><span data-stu-id="b1b93-132">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="b1b93-133">**Не** поддерживаются в Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b1b93-133">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="b1b93-134">[Фильтры исключений](#exception-filters) применяют глобальные политики к необработанным исключениям, которые происходят до записи данных в тело ответа.</span><span class="sxs-lookup"><span data-stu-id="b1b93-134">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="b1b93-135">[Фильтры результатов](#result-filters) выполняют код непосредственно до и после выполнения результатов действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-135">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="b1b93-136">Они выполняются только в том случае, если метод действия выполнен успешно.</span><span class="sxs-lookup"><span data-stu-id="b1b93-136">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="b1b93-137">Они полезны для логики, которая должна включать исключения для представлений или модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="b1b93-137">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="b1b93-138">На приведенной ниже схеме показано, как типы фильтров взаимодействуют друг с другом в конвейере фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-138">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Запрос обрабатывается посредством фильтров авторизации, фильтров ресурсов, привязки модели, фильтров действий, выполнения действия и преобразования результата действия, фильтров исключений, фильтров результатов и выполнения результатов.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="b1b93-141">Реализация</span><span class="sxs-lookup"><span data-stu-id="b1b93-141">Implementation</span></span>

<span data-ttu-id="b1b93-142">Фильтры поддерживают как синхронные, так и асинхронные реализации посредством различных определений интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-142">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="b1b93-143">Синхронные фильтры выполняют код до и после этапа конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-143">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="b1b93-144">Например, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> вызывается перед вызовом метода действия,</span><span class="sxs-lookup"><span data-stu-id="b1b93-144">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="b1b93-145">а <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> — после возврата метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="b1b93-146">Асинхронные фильтры определяют метод `On-Stage-ExecutionAsync`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-146">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="b1b93-147">Например, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span><span class="sxs-lookup"><span data-stu-id="b1b93-147">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="b1b93-148">В приведенном выше коде `SampleAsyncActionFilter` включает <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) для выполнения метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-148">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="b1b93-149">Несколько этапов фильтра</span><span class="sxs-lookup"><span data-stu-id="b1b93-149">Multiple filter stages</span></span>

<span data-ttu-id="b1b93-150">Реализовать интерфейсы для нескольких этапов фильтра можно в одном классе.</span><span class="sxs-lookup"><span data-stu-id="b1b93-150">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="b1b93-151">Например, класс <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> реализует следующие интерфейсы:</span><span class="sxs-lookup"><span data-stu-id="b1b93-151">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="b1b93-152">синхронные: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> и <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>;</span><span class="sxs-lookup"><span data-stu-id="b1b93-152">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="b1b93-153">асинхронные: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> и <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-153">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="b1b93-154">Реализуйте синхронный **или** асинхронный интерфейс фильтра, но **не** оба варианта.</span><span class="sxs-lookup"><span data-stu-id="b1b93-154">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="b1b93-155">Среда выполнения сначала проверяет, реализует ли фильтр асинхронный интерфейс. Если да, вызывается именно он.</span><span class="sxs-lookup"><span data-stu-id="b1b93-155">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="b1b93-156">В противном случае вызываются методы синхронного интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b1b93-156">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="b1b93-157">Если асинхронный и синхронный интерфейсы реализованы в одном классе, вызывается только асинхронный метод.</span><span class="sxs-lookup"><span data-stu-id="b1b93-157">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="b1b93-158">При использовании абстрактных классов, таких как <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, следует переопределять только синхронные методы или асинхронный метод каждого типа фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-158">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="b1b93-159">Встроенные атрибуты фильтров</span><span class="sxs-lookup"><span data-stu-id="b1b93-159">Built-in filter attributes</span></span>

<span data-ttu-id="b1b93-160">ASP.NET Core включает встроенные фильтры на основе атрибутов, с помощью которых можно создавать подклассы и которые можно настраивать.</span><span class="sxs-lookup"><span data-stu-id="b1b93-160">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="b1b93-161">Например, следующий фильтр результатов добавляет заголовок к ответу:</span><span class="sxs-lookup"><span data-stu-id="b1b93-161">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="b1b93-162">Атрибуты позволяют фильтрам принимать аргументы, как показано в примере выше.</span><span class="sxs-lookup"><span data-stu-id="b1b93-162">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="b1b93-163">Примените `AddHeaderAttribute` к методу контроллера или действия, а затем укажите имя и значение заголовка HTTP:</span><span class="sxs-lookup"><span data-stu-id="b1b93-163">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="b1b93-164">Проверьте заголовки с помощью [инструментов разработчика для браузера](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools).</span><span class="sxs-lookup"><span data-stu-id="b1b93-164">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="b1b93-165">В **заголовке ответа** отображается `author: Rick Anderson`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-165">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="b1b93-166">В следующем коде реализуется класс `ActionFilterAttribute`, который выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="b1b93-166">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="b1b93-167">Считывает заголовок и имя в системе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1b93-167">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="b1b93-168">В отличие от предыдущего примера, для указанного ниже кода не нужно добавлять параметры фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-168">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="b1b93-169">Добавляет заголовок и имя к заголовку ответа.</span><span class="sxs-lookup"><span data-stu-id="b1b93-169">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="b1b93-170">Параметры конфигурации предоставляются из [системы конфигурации](xref:fundamentals/configuration/index) с помощью [шаблона параметров](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="b1b93-170">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="b1b93-171">Например, из файла *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b1b93-171">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="b1b93-172">В `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-172">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="b1b93-173">Класс `PositionOptions` добавляется в контейнер службы с помощью области конфигурации `"Position"`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-173">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="b1b93-174">`MyActionFilterAttribute` добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="b1b93-174">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="b1b93-175">В следующем коде демонстрируется класс `PositionOptions`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-175">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="b1b93-176">В следующем коде применяется атрибут `MyActionFilterAttribute` к методу `Index2`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-176">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="b1b93-177">В **заголовке ответа** при вызове конечной точки `Sample/Index2` отображается `author: Rick Anderson` и `Editor: Joe Smith`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-177">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="b1b93-178">В следующем коде применяется атрибут `MyActionFilterAttribute` и `AddHeaderAttribute` к Razor Page:</span><span class="sxs-lookup"><span data-stu-id="b1b93-178">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="b1b93-179">К методам обработчика Razor Page нельзя применить фильтры.</span><span class="sxs-lookup"><span data-stu-id="b1b93-179">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="b1b93-180">Их можно применить глобально или к модели Razor Page.</span><span class="sxs-lookup"><span data-stu-id="b1b93-180">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="b1b93-181">Некоторые интерфейсы фильтров имеют соответствующие атрибуты, которые можно использовать как базовые классы для пользовательских реализаций.</span><span class="sxs-lookup"><span data-stu-id="b1b93-181">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="b1b93-182">Атрибуты фильтров:</span><span class="sxs-lookup"><span data-stu-id="b1b93-182">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="b1b93-183">Области и порядок выполнения фильтров</span><span class="sxs-lookup"><span data-stu-id="b1b93-183">Filter scopes and order of execution</span></span>

<span data-ttu-id="b1b93-184">Фильтр можно добавить в конвейер в одной из трех *областей*:</span><span class="sxs-lookup"><span data-stu-id="b1b93-184">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="b1b93-185">С помощью атрибута в действии контроллера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-185">Using an attribute on a controller action.</span></span> <span data-ttu-id="b1b93-186">Атрибуты фильтра нельзя применить к методам обработчика Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b1b93-186">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="b1b93-187">С помощью атрибута в контроллере или Razor Page.</span><span class="sxs-lookup"><span data-stu-id="b1b93-187">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="b1b93-188">Глобально для всех контроллеров, действий и Razor Pages, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="b1b93-188">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="b1b93-189">Порядок выполнения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="b1b93-189">Default order of execution</span></span>

<span data-ttu-id="b1b93-190">Если для определенного этапа конвейера имеется несколько фильтров, область определяет порядок их выполнения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b1b93-190">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="b1b93-191">Глобальные фильтры заключают в себя фильтры классов, которые, в свою очередь, заключают в себя фильтры методов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-191">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="b1b93-192">В результате такого вложения *последующий* код фильтров выполняется в порядке, обратном выполнению *предшествующего* кода.</span><span class="sxs-lookup"><span data-stu-id="b1b93-192">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="b1b93-193">Последовательность фильтров:</span><span class="sxs-lookup"><span data-stu-id="b1b93-193">The filter sequence:</span></span>

* <span data-ttu-id="b1b93-194">*Предшествующий* код глобальных фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-194">The *before* code of global filters.</span></span>
  * <span data-ttu-id="b1b93-195">*Предшествующий* код фильтров контроллера и Razor Page.</span><span class="sxs-lookup"><span data-stu-id="b1b93-195">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="b1b93-196">*Предшествующий* код фильтров методов действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-196">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="b1b93-197">*Последующий* код фильтров методов действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-197">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="b1b93-198">*Последующий* код фильтров контроллера и Razor Page.</span><span class="sxs-lookup"><span data-stu-id="b1b93-198">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="b1b93-199">*Последующий* код глобальных фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-199">The *after* code of global filters.</span></span>
  
<span data-ttu-id="b1b93-200">В следующем примере показан порядок вызова методов фильтров для синхронных фильтров действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-200">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="b1b93-201">Sequence</span><span class="sxs-lookup"><span data-stu-id="b1b93-201">Sequence</span></span> | <span data-ttu-id="b1b93-202">Область фильтра</span><span class="sxs-lookup"><span data-stu-id="b1b93-202">Filter scope</span></span> | <span data-ttu-id="b1b93-203">Метод фильтра</span><span class="sxs-lookup"><span data-stu-id="b1b93-203">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="b1b93-204">1</span><span class="sxs-lookup"><span data-stu-id="b1b93-204">1</span></span> | <span data-ttu-id="b1b93-205">Global</span><span class="sxs-lookup"><span data-stu-id="b1b93-205">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b1b93-206">2</span><span class="sxs-lookup"><span data-stu-id="b1b93-206">2</span></span> | <span data-ttu-id="b1b93-207">Контроллер или Razor Page</span><span class="sxs-lookup"><span data-stu-id="b1b93-207">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="b1b93-208">3</span><span class="sxs-lookup"><span data-stu-id="b1b93-208">3</span></span> | <span data-ttu-id="b1b93-209">Метод</span><span class="sxs-lookup"><span data-stu-id="b1b93-209">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b1b93-210">4</span><span class="sxs-lookup"><span data-stu-id="b1b93-210">4</span></span> | <span data-ttu-id="b1b93-211">Метод</span><span class="sxs-lookup"><span data-stu-id="b1b93-211">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="b1b93-212">5</span><span class="sxs-lookup"><span data-stu-id="b1b93-212">5</span></span> | <span data-ttu-id="b1b93-213">Контроллер или Razor Page</span><span class="sxs-lookup"><span data-stu-id="b1b93-213">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="b1b93-214">6</span><span class="sxs-lookup"><span data-stu-id="b1b93-214">6</span></span> | <span data-ttu-id="b1b93-215">Global</span><span class="sxs-lookup"><span data-stu-id="b1b93-215">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="b1b93-216">Фильтры на уровне контроллера</span><span class="sxs-lookup"><span data-stu-id="b1b93-216">Controller level filters</span></span>

<span data-ttu-id="b1b93-217">Каждый контроллер, наследуемый от базового класса <xref:Microsoft.AspNetCore.Mvc.Controller> включает методы [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) и [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-217">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="b1b93-218">Эти методы:</span><span class="sxs-lookup"><span data-stu-id="b1b93-218">These methods:</span></span>

* <span data-ttu-id="b1b93-219">Заключают фильтры, которые выполняются для указанного действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-219">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="b1b93-220">`OnActionExecuting` вызывается перед всеми фильтрами действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-220">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="b1b93-221">`OnActionExecuted` вызывается после всех фильтров действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-221">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="b1b93-222">`OnActionExecutionAsync` вызывается перед всеми фильтрами действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-222">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="b1b93-223">Код в фильтре после `next` выполняется после метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-223">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="b1b93-224">Например, в скачиваемом примере `MySampleActionFilter` применяется глобально при запуске.</span><span class="sxs-lookup"><span data-stu-id="b1b93-224">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="b1b93-225">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-225">The `TestController`:</span></span>

* <span data-ttu-id="b1b93-226">Применяет `SampleActionFilterAttribute` (`[SampleActionFilter]`) для действия `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-226">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="b1b93-227">Переопределяет `OnActionExecuting` и `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-227">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="b1b93-228">Переходит к `https://localhost:5001/Test2/FilterTest2` для выполнения следующего кода:</span><span class="sxs-lookup"><span data-stu-id="b1b93-228">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="b1b93-229">Фильтры на уровне контроллера задают для свойства [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) значение `int.MinValue`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-229">Controller level filters set the [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="b1b93-230">Их **нельзя** настроить, чтобы они запускались после применения фильтров к методам.</span><span class="sxs-lookup"><span data-stu-id="b1b93-230">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="b1b93-231">Свойство Order рассматривается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="b1b93-231">Order is explained in the next section.</span></span>

<span data-ttu-id="b1b93-232">См. подробнее о [реализации фильтров страницы Razor путем переопределения методов фильтра](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="b1b93-232">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="b1b93-233">Переопределение порядка по умолчанию</span><span class="sxs-lookup"><span data-stu-id="b1b93-233">Overriding the default order</span></span>

<span data-ttu-id="b1b93-234">Чтобы переопределить порядок выполнения по умолчанию, можно реализовать <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-234">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="b1b93-235">`IOrderedFilter` предоставляет свойство <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, которое имеет приоритет над областью и определяет порядок выполнения.</span><span class="sxs-lookup"><span data-stu-id="b1b93-235">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="b1b93-236">Фильтр со значением меньше `Order`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-236">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="b1b93-237">Выполняется *перед* кодом, выполняемым до фильтра с более высоким значением `Order`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-237">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="b1b93-238">Выполняется *после* кода, выполняемого после фильтра с более высоким значением `Order`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-238">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="b1b93-239">Свойство `Order` задается с помощью параметра конструктора:</span><span class="sxs-lookup"><span data-stu-id="b1b93-239">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="b1b93-240">Рассмотрим два фильтра действий в следующем контроллере:</span><span class="sxs-lookup"><span data-stu-id="b1b93-240">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="b1b93-241">Глобальный фильтр добавляется в `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-241">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="b1b93-242">3 фильтра выполняются в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="b1b93-242">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="b1b93-243">Свойство `Order` переопределяет область при определении порядка выполнения фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="b1b93-244">Фильтры сначала сортируются по порядку, а затем очередность окончательно определяется по области.</span><span class="sxs-lookup"><span data-stu-id="b1b93-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="b1b93-245">Все встроенные фильтры реализуют `IOrderedFilter` и задают значение по умолчанию `Order`, равное 0.</span><span class="sxs-lookup"><span data-stu-id="b1b93-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="b1b93-246">Как упоминалось ранее, фильтры на уровне контроллера задают для свойства [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) значение `int.MinValue`. Во встроенных фильтрах область определяет порядок, если для `Order` не задано ненулевое значение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-246">As mentioned previously, controller level filters set the [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="b1b93-247">В указанном выше коде `MySampleActionFilter` имеет глобальную область действия и запускается перед `MyAction2FilterAttribute`, у которого область действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-247">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="b1b93-248">Чтобы `MyAction2FilterAttribute` запускался первым, задайте для свойства Order значение `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-248">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="b1b93-249">Чтобы глобальный фильтр `MySampleActionFilter` запускался первым, задайте для свойства `Order` значение `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-249">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="b1b93-250">Отмена и сокращенное выполнение</span><span class="sxs-lookup"><span data-stu-id="b1b93-250">Cancellation and short-circuiting</span></span>

<span data-ttu-id="b1b93-251">Вы можете настроить сокращенное выполнение конвейера фильтров, задав свойство <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> для параметра <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext>, передаваемого в метод фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-251">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="b1b93-252">Например, приведенный ниже фильтр ресурсов предотвращает выполнение остальной части конвейера:</span><span class="sxs-lookup"><span data-stu-id="b1b93-252">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="b1b93-253">В приведенном ниже коде как фильтр `ShortCircuitingResourceFilter`, так и фильтр `AddHeader` нацелены на метод действия `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-253">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="b1b93-254">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-254">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="b1b93-255">Выполняется первым, поскольку это фильтр ресурсов, а `AddHeader` — фильтр действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-255">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="b1b93-256">Замыкает оставшуюся часть конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-256">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="b1b93-257">Поэтому фильтр `AddHeader` никогда не выполняется для действия `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-257">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="b1b93-258">Поведение будет аналогичным при применении обоих фильтров на уровне метода действия при условии, что фильтр `ShortCircuitingResourceFilter` выполняется первым.</span><span class="sxs-lookup"><span data-stu-id="b1b93-258">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="b1b93-259">Фильтр `ShortCircuitingResourceFilter` выполняется в первую очередь из-за его типа или в связи с явным указанием свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-259">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="b1b93-260">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="b1b93-260">Dependency injection</span></span>

<span data-ttu-id="b1b93-261">Фильтры можно добавлять по типу или экземпляру.</span><span class="sxs-lookup"><span data-stu-id="b1b93-261">Filters can be added by type or by instance.</span></span> <span data-ttu-id="b1b93-262">Добавляемый экземпляр используется для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="b1b93-262">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="b1b93-263">Если добавляется тип, он является активированным.</span><span class="sxs-lookup"><span data-stu-id="b1b93-263">If a type is added, it's type-activated.</span></span> <span data-ttu-id="b1b93-264">Активация фильтра означает, что:</span><span class="sxs-lookup"><span data-stu-id="b1b93-264">A type-activated filter means:</span></span>

* <span data-ttu-id="b1b93-265">Экземпляр создается для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="b1b93-265">An instance is created for each request.</span></span>
* <span data-ttu-id="b1b93-266">Зависимости конструктора заполняются путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b1b93-266">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="b1b93-267">Фильтры, которые реализуются как атрибуты и добавляются непосредственно в классы контроллеров или методы действий, не могут иметь зависимости конструктора, предоставленные посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b1b93-267">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="b1b93-268">Зависимости конструктора не могут предоставляться путем внедрения зависимостей, так как:</span><span class="sxs-lookup"><span data-stu-id="b1b93-268">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="b1b93-269">Параметры конструктора должны предоставляться для атрибутов в месте их применения.</span><span class="sxs-lookup"><span data-stu-id="b1b93-269">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="b1b93-270">Это ограничение, налагаемое на использование атрибутов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-270">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="b1b93-271">Следующие фильтры поддерживают зависимости конструктора, предоставленные путем внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="b1b93-271">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="b1b93-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> реализуется в атрибуте.</span><span class="sxs-lookup"><span data-stu-id="b1b93-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="b1b93-273">Предыдущие фильтры могут применяться к контроллеру или методу действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-273">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="b1b93-274">Средства ведения журнала доступны путем внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-274">Loggers are available from DI.</span></span> <span data-ttu-id="b1b93-275">Но следует избегать создания и использования фильтров исключительно для целей ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b1b93-275">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="b1b93-276">[Встроенное средство ведения журнала](xref:fundamentals/logging/index) обычно предоставляет все необходимое для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b1b93-276">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="b1b93-277">При добавлении ведения журналов в фильтры следует учитывать следующее:</span><span class="sxs-lookup"><span data-stu-id="b1b93-277">Logging added to filters:</span></span>

* <span data-ttu-id="b1b93-278">Следует сосредоточиться на бизнес-среде или поведении в привязке к фильтру.</span><span class="sxs-lookup"><span data-stu-id="b1b93-278">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="b1b93-279">**Не** следует регистрировать действия или другие события платформы,</span><span class="sxs-lookup"><span data-stu-id="b1b93-279">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="b1b93-280">так как это делают встроенные фильтры.</span><span class="sxs-lookup"><span data-stu-id="b1b93-280">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="b1b93-281">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="b1b93-281">ServiceFilterAttribute</span></span>

<span data-ttu-id="b1b93-282">Типы реализации фильтра службы регистрируются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-282">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="b1b93-283">Атрибут <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> извлекает экземпляр фильтра из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-283">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="b1b93-284">В следующем коде используется `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-284">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="b1b93-285">В следующем коде в контейнер внедрения зависимостей добавляется `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-285">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="b1b93-286">В следующем коде атрибут `ServiceFilter` извлекает экземпляр фильтра `AddHeaderResultServiceFilter` из внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="b1b93-286">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="b1b93-287">При использовании `ServiceFilterAttribute` задание [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="b1b93-287">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="b1b93-288">Указывает, что экземпляр фильтра *можно* многократно использовать за пределами области запроса, в которой он был создан.</span><span class="sxs-lookup"><span data-stu-id="b1b93-288">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="b1b93-289">Среда выполнения ASP.NET Core не гарантирует:</span><span class="sxs-lookup"><span data-stu-id="b1b93-289">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="b1b93-290">что будет создан хотя бы один экземпляр фильтра;</span><span class="sxs-lookup"><span data-stu-id="b1b93-290">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="b1b93-291">что фильтр не будет повторно запрошен из контейнера внедрения зависимостей на более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="b1b93-291">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="b1b93-292">Не следует использовать с фильтром, который зависит от служб со временем существования, кроме элементов singleton.</span><span class="sxs-lookup"><span data-stu-id="b1b93-292">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="b1b93-293">Объект <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> реализует интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="b1b93-294">`IFilterFactory` предоставляет метод <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> для создания экземпляра <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-294">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="b1b93-295">`CreateInstance` загружает указанный тип из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-295">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="b1b93-296">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="b1b93-296">TypeFilterAttribute</span></span>

<span data-ttu-id="b1b93-297">Атрибут <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> похож на <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, но его тип не разрешается напрямую из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="b1b93-298">Он создает экземпляр типа с помощью <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-298">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="b1b93-299">Так как типы `TypeFilterAttribute` не разрешаются напрямую из контейнера внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="b1b93-299">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="b1b93-300">Типы, на которые ссылаются с помощью `TypeFilterAttribute`, не нужно регистрировать в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-300">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="b1b93-301">Их зависимости выполняются самим контейнером.</span><span class="sxs-lookup"><span data-stu-id="b1b93-301">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="b1b93-302">Атрибут `TypeFilterAttribute` может также принимать аргументы конструктора для типа.</span><span class="sxs-lookup"><span data-stu-id="b1b93-302">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="b1b93-303">При использовании `TypeFilterAttribute` задание [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="b1b93-303">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="b1b93-304">Указывает, что экземпляр фильтра *можно* многократно использовать за пределами области запроса, в которой он был создан.</span><span class="sxs-lookup"><span data-stu-id="b1b93-304">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="b1b93-305">Среда выполнения ASP.NET Core не предоставляет никаких гарантий, что будет создан хотя бы один экземпляр фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-305">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="b1b93-306">Не следует использовать с фильтром, который зависит от служб со временем существования, кроме элементов singleton.</span><span class="sxs-lookup"><span data-stu-id="b1b93-306">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="b1b93-307">В следующем примере показано, как передавать аргументы в тип с помощью `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-307">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="b1b93-308">Фильтры авторизации</span><span class="sxs-lookup"><span data-stu-id="b1b93-308">Authorization filters</span></span>

<span data-ttu-id="b1b93-309">Фильтры авторизации:</span><span class="sxs-lookup"><span data-stu-id="b1b93-309">Authorization filters:</span></span>

* <span data-ttu-id="b1b93-310">Выполняются в первую очередь в конвейере фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-310">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="b1b93-311">Контролируют доступ к методам действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-311">Control access to action methods.</span></span>
* <span data-ttu-id="b1b93-312">Имеют предшествующий, но не последующий метод.</span><span class="sxs-lookup"><span data-stu-id="b1b93-312">Have a before method, but no after method.</span></span>

<span data-ttu-id="b1b93-313">Для работы пользовательских фильтров авторизации требуется настраиваемая платформа авторизации.</span><span class="sxs-lookup"><span data-stu-id="b1b93-313">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="b1b93-314">Настройка политик авторизации или определение пользовательской политики авторизации предпочтительнее создания пользовательского фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-314">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="b1b93-315">Встроенный фильтр авторизации:</span><span class="sxs-lookup"><span data-stu-id="b1b93-315">The built-in authorization filter:</span></span>

* <span data-ttu-id="b1b93-316">Вызывает систему авторизации.</span><span class="sxs-lookup"><span data-stu-id="b1b93-316">Calls the authorization system.</span></span>
* <span data-ttu-id="b1b93-317">Не выполняет авторизацию запросов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-317">Does not authorize requests.</span></span>

<span data-ttu-id="b1b93-318">**Не** вызывайте исключения в фильтрах авторизации:</span><span class="sxs-lookup"><span data-stu-id="b1b93-318">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="b1b93-319">Исключение не будет обработано.</span><span class="sxs-lookup"><span data-stu-id="b1b93-319">The exception will not be handled.</span></span>
* <span data-ttu-id="b1b93-320">Фильтры исключений не будут обрабатывать исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-320">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="b1b93-321">При возникновении исключения в фильтре авторизации попробуйте создать запрос.</span><span class="sxs-lookup"><span data-stu-id="b1b93-321">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="b1b93-322">Дополнительные сведения об [авторизации](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="b1b93-322">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="b1b93-323">Фильтры ресурсов</span><span class="sxs-lookup"><span data-stu-id="b1b93-323">Resource filters</span></span>

<span data-ttu-id="b1b93-324">Фильтры ресурсов:</span><span class="sxs-lookup"><span data-stu-id="b1b93-324">Resource filters:</span></span>

* <span data-ttu-id="b1b93-325">Реализуют либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter>, либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-325">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="b1b93-326">Выполнение охватывает большую часть конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-326">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="b1b93-327">До фильтров ресурсов применяются только [фильтры авторизации](#authorization-filters).</span><span class="sxs-lookup"><span data-stu-id="b1b93-327">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="b1b93-328">Фильтры ресурсов полезны для сокращенного выполнения большей части конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-328">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="b1b93-329">Например, фильтр кэширования предотвращает выполнение остальной части конвейера при попадании в кэше.</span><span class="sxs-lookup"><span data-stu-id="b1b93-329">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="b1b93-330">Примеры фильтров ресурсов:</span><span class="sxs-lookup"><span data-stu-id="b1b93-330">Resource filter examples:</span></span>

* <span data-ttu-id="b1b93-331">[Сокращенное выполнение фильтра ресурсов](#short-circuiting-resource-filter) показано ранее.</span><span class="sxs-lookup"><span data-stu-id="b1b93-331">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="b1b93-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="b1b93-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="b1b93-333">Предотвращает доступ привязки модели к данным формы.</span><span class="sxs-lookup"><span data-stu-id="b1b93-333">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="b1b93-334">Используется для загрузки больших файлов, если необходимо предотвратить считывание данных формы в память.</span><span class="sxs-lookup"><span data-stu-id="b1b93-334">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="b1b93-335">Фильтры действий</span><span class="sxs-lookup"><span data-stu-id="b1b93-335">Action filters</span></span>

<span data-ttu-id="b1b93-336">Фильтры действий **неприменимы** к Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b1b93-336">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="b1b93-337">Razor Pages поддерживает <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> и <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-337">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="b1b93-338">Дополнительные сведения см. в разделе [Методы фильтрации для Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="b1b93-338">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="b1b93-339">Фильтры действий:</span><span class="sxs-lookup"><span data-stu-id="b1b93-339">Action filters:</span></span>

* <span data-ttu-id="b1b93-340">Реализуют либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter>, либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-340">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="b1b93-341">Их выполнение охватывает выполнение методов действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-341">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="b1b93-342">В следующем фрагменте кода показан пример фильтра действий:</span><span class="sxs-lookup"><span data-stu-id="b1b93-342">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="b1b93-343"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> предоставляет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="b1b93-343">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="b1b93-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> позволяет считать входные данные в метод действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="b1b93-345"><xref:Microsoft.AspNetCore.Mvc.Controller> позволяет управлять экземпляром контроллера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-345"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="b1b93-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> позволяет при задании свойства `Result` сократить выполнение метода действия и последующих фильтров действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="b1b93-347">Вызов исключения в методе действия:</span><span class="sxs-lookup"><span data-stu-id="b1b93-347">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="b1b93-348">Предотвращает выполнение последующих фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-348">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="b1b93-349">В отличие от параметра `Result` рассматривается как сбой, а не успешный результат.</span><span class="sxs-lookup"><span data-stu-id="b1b93-349">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="b1b93-350"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> предоставляет `Controller` и `Result`, а также следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="b1b93-350">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="b1b93-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> будет иметь значение true, если выполнение действия было сокращено другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="b1b93-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="b1b93-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> будет иметь ненулевое значение, если предыдущее выполнение фильтра действий вызвало исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="b1b93-353">При присвоении этому свойству ненулевого значения:</span><span class="sxs-lookup"><span data-stu-id="b1b93-353">Setting this property to null:</span></span>

  * <span data-ttu-id="b1b93-354">Будет эффективно обрабатываться исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-354">Effectively handles the exception.</span></span>
  * <span data-ttu-id="b1b93-355">`Result` будет выполняться так, как при возвращении методом действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-355">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="b1b93-356">Для `IAsyncActionFilter` вызов <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="b1b93-356">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="b1b93-357">Приводит к выполнению последующих фильтров действий и метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-357">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="b1b93-358">Возвращает `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-358">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="b1b93-359">Для сокращенного выполнения присвойте <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> экземпляру результата и не вызывайте `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="b1b93-359">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="b1b93-360">Платформа предоставляет абстрактный класс <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, для которого можно создавать подклассы.</span><span class="sxs-lookup"><span data-stu-id="b1b93-360">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="b1b93-361">Фильтр действий `OnActionExecuting` можно использовать:</span><span class="sxs-lookup"><span data-stu-id="b1b93-361">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="b1b93-362">Для проверки состояния модели.</span><span class="sxs-lookup"><span data-stu-id="b1b93-362">Validate model state.</span></span>
* <span data-ttu-id="b1b93-363">Для получения ошибки, если состояние является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="b1b93-363">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="b1b93-364">Метод `OnActionExecuted` выполняется после метода действия:</span><span class="sxs-lookup"><span data-stu-id="b1b93-364">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="b1b93-365">Он имеет доступ к результатам действия и может управлять ими с помощью свойства <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-365">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="b1b93-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> будет иметь значение true, если выполнение действия было сокращено другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="b1b93-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="b1b93-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> будет иметь ненулевое значение, если действие или последующий фильтр действий вызвали исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="b1b93-368">Если установить для `Exception` значение NULL:</span><span class="sxs-lookup"><span data-stu-id="b1b93-368">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="b1b93-369">Будет эффективно обрабатываться исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-369">Effectively handles an exception.</span></span>
  * <span data-ttu-id="b1b93-370">`ActionExecutedContext.Result` будет выполняться так, как если бы метод действия вернул его обычным образом.</span><span class="sxs-lookup"><span data-stu-id="b1b93-370">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="b1b93-371">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="b1b93-371">Exception filters</span></span>

<span data-ttu-id="b1b93-372">Фильтры исключений:</span><span class="sxs-lookup"><span data-stu-id="b1b93-372">Exception filters:</span></span>

* <span data-ttu-id="b1b93-373">Реализуют <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-373">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="b1b93-374">Можно использовать для реализации политик обработки стандартных ошибок.</span><span class="sxs-lookup"><span data-stu-id="b1b93-374">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="b1b93-375">В следующем примере фильтра исключений используется пользовательское представление ошибок для отображения подробных сведений об исключениях, которые происходят во время разработки приложения:</span><span class="sxs-lookup"><span data-stu-id="b1b93-375">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="b1b93-376">В следующем коде проверяется фильтр исключений:</span><span class="sxs-lookup"><span data-stu-id="b1b93-376">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="b1b93-377">Фильтры исключений:</span><span class="sxs-lookup"><span data-stu-id="b1b93-377">Exception filters:</span></span>

* <span data-ttu-id="b1b93-378">Не имеют предшествующих и последующих событий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-378">Don't have before and after events.</span></span>
* <span data-ttu-id="b1b93-379">Реализуют <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-379">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="b1b93-380">Обрабатывают необработанные исключения, которые возникают при создании страницы Razor или контроллера, в [привязке модели](xref:mvc/models/model-binding), фильтрах действий или методах действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-380">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="b1b93-381">**Не** перехватывают исключения, которые возникают в фильтрах ресурсов, фильтрах результатов или при выполнении результата MVC.</span><span class="sxs-lookup"><span data-stu-id="b1b93-381">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="b1b93-382">Для обработки исключения присвойте свойству <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> значение `true` или напишите ответ.</span><span class="sxs-lookup"><span data-stu-id="b1b93-382">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="b1b93-383">Это предотвратит распространение исключения.</span><span class="sxs-lookup"><span data-stu-id="b1b93-383">This stops propagation of the exception.</span></span> <span data-ttu-id="b1b93-384">Фильтр исключений не может преобразовать исключение в успешное выполнение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-384">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="b1b93-385">Это может сделать только фильтр действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-385">Only an action filter can do that.</span></span>

<span data-ttu-id="b1b93-386">Фильтры исключений:</span><span class="sxs-lookup"><span data-stu-id="b1b93-386">Exception filters:</span></span>

* <span data-ttu-id="b1b93-387">Хорошо подходят для перехвата исключений, возникающих в действиях.</span><span class="sxs-lookup"><span data-stu-id="b1b93-387">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="b1b93-388">Не обладает такой гибкостью, как ПО промежуточного слоя обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="b1b93-388">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="b1b93-389">ПО промежуточного слоя хорошо подходит для обработки исключений.</span><span class="sxs-lookup"><span data-stu-id="b1b93-389">Prefer middleware for exception handling.</span></span> <span data-ttu-id="b1b93-390">Используйте фильтры исключений, только если обработка ошибок осуществляется *по-разному* с учетом вызванного метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-390">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="b1b93-391">Например, в приложении могут использоваться методы действий как для конечных точек API, так и для представлений или HTML.</span><span class="sxs-lookup"><span data-stu-id="b1b93-391">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="b1b93-392">Конечные точки API могут возвращать сведения об ошибках в формате JSON, в то время как действия на основе представлений могут возвращать страницу ошибки в формате HTML.</span><span class="sxs-lookup"><span data-stu-id="b1b93-392">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="b1b93-393">Фильтры результатов</span><span class="sxs-lookup"><span data-stu-id="b1b93-393">Result filters</span></span>

<span data-ttu-id="b1b93-394">Фильтры результатов:</span><span class="sxs-lookup"><span data-stu-id="b1b93-394">Result filters:</span></span>

* <span data-ttu-id="b1b93-395">Реализация интерфейса:</span><span class="sxs-lookup"><span data-stu-id="b1b93-395">Implement an interface:</span></span>
  * <span data-ttu-id="b1b93-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="b1b93-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="b1b93-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="b1b93-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="b1b93-398">Их выполнение охватывает выполнение результатов действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-398">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="b1b93-399">IResultFilter и IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="b1b93-399">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="b1b93-400">В следующем фрагменте кода показан фильтр результатов, который добавляет заголовок HTTP:</span><span class="sxs-lookup"><span data-stu-id="b1b93-400">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="b1b93-401">Тип выполняемого результата зависит от соответствующего действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-401">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="b1b93-402">Действие, возвращающее представление, включает всю обработку Razor в рамках выполняемого объекта <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-402">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="b1b93-403">В процессе выполнения результата метод API может производить сериализацию.</span><span class="sxs-lookup"><span data-stu-id="b1b93-403">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="b1b93-404">Дополнительные сведения о [результатах действий](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="b1b93-404">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="b1b93-405">Фильтры результатов выполняются только в том случае, когда действие или фильтры действий предоставляют результат действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-405">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="b1b93-406">Фильтры результатов не выполняются в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="b1b93-406">Result filters are not executed when:</span></span>

* <span data-ttu-id="b1b93-407">Фильтр авторизации или ресурсов выполняет сокращение конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-407">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="b1b93-408">Фильтр исключений обрабатывает исключение и выдает результат действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-408">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="b1b93-409">Метод <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> может сокращать выполнение результата действия и последующих фильтров результатов, присваивая свойству <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> значение `true`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-409">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="b1b93-410">При сокращении выполнения выполните запись в объект ответа, чтобы избежать формирования пустого ответа.</span><span class="sxs-lookup"><span data-stu-id="b1b93-410">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="b1b93-411">Создание исключения в `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-411">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="b1b93-412">предотвращает выполнение результата действия и последующих фильтров;</span><span class="sxs-lookup"><span data-stu-id="b1b93-412">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="b1b93-413">рассматривается как сбой, а не успешный результат.</span><span class="sxs-lookup"><span data-stu-id="b1b93-413">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="b1b93-414">Если запускается метод <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName>, ответ, скорее всего, уже был отправлен клиенту.</span><span class="sxs-lookup"><span data-stu-id="b1b93-414">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="b1b93-415">Если ответ уже был отправлен клиенту, его нельзя изменить.</span><span class="sxs-lookup"><span data-stu-id="b1b93-415">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="b1b93-416">`ResultExecutedContext.Canceled` будет иметь значение `true`, если выполнение результата действия было сокращено другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="b1b93-416">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="b1b93-417">`ResultExecutedContext.Exception` будет иметь ненулевое значение, если результат действия или последующий фильтр результатов вызвали исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-417">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="b1b93-418">Присвоение `Exception` значения NULL приводит к обработке исключения и предотвращает его последующий вызов на дальнейших этапах конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-418">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="b1b93-419">Надежный способ записи данных в ответ при обработке исключения в фильтре результатов отсутствует.</span><span class="sxs-lookup"><span data-stu-id="b1b93-419">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="b1b93-420">Если результат действия вызывает исключение в процессе выполнения и заголовки уже были переданы в клиент, надежного механизма отправки кода сбоя не существует.</span><span class="sxs-lookup"><span data-stu-id="b1b93-420">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="b1b93-421">Для <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> вызов к `await next` для <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> приводит к выполнению последующих фильтров результатов и результата действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-421">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="b1b93-422">Для сокращения выполнения задайте [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) для `true` и не вызывайте `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-422">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="b1b93-423">Платформа предоставляет абстрактный класс `ResultFilterAttribute`, для которого можно создавать подклассы.</span><span class="sxs-lookup"><span data-stu-id="b1b93-423">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="b1b93-424">Представленный ранее класс [AddHeaderAttribute](#add-header-attribute) — это пример атрибута фильтра результатов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-424">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="b1b93-425">IAlwaysRunResultFilter и IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="b1b93-425">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="b1b93-426">Интерфейсы <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> и <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> объявляют реализацию <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>, которая выполняется для всех результатов действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-426">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="b1b93-427">Сюда включены результаты действий, созданные:</span><span class="sxs-lookup"><span data-stu-id="b1b93-427">This includes action results produced by:</span></span>

* <span data-ttu-id="b1b93-428">фильтрами авторизации и фильтрами ресурсов, которые сокращают ответ;</span><span class="sxs-lookup"><span data-stu-id="b1b93-428">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="b1b93-429">фильтрами исключений.</span><span class="sxs-lookup"><span data-stu-id="b1b93-429">Exception filters.</span></span>

<span data-ttu-id="b1b93-430">Например, следующий фильтр всегда выполняется, задавая результат действия (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) с кодом состояния *422 Unprocessable Entity* при сбое согласования содержимого:</span><span class="sxs-lookup"><span data-stu-id="b1b93-430">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="b1b93-431">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="b1b93-431">IFilterFactory</span></span>

<span data-ttu-id="b1b93-432">Объект <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> реализует интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="b1b93-433">Поэтому экземпляр `IFilterFactory` можно использовать в качестве экземпляра `IFilterMetadata` в любом месте конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-433">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="b1b93-434">Когда среда выполнения готовится вызвать фильтр, она пытается привести его к `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-434">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="b1b93-435">Если приведение проходит успешно, вызывается метод <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> для создания вызываемого экземпляра `IFilterMetadata`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-435">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="b1b93-436">Это обеспечивает высокую степень гибкости, так как при запуске приложения нет необходимости в явном и точном определении конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-436">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="b1b93-437">Еще один подход к созданию фильтров заключается в реализации `IFilterFactory` с помощью настраиваемых атрибутов:</span><span class="sxs-lookup"><span data-stu-id="b1b93-437">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="b1b93-438">В следующем коде применяется фильтр:</span><span class="sxs-lookup"><span data-stu-id="b1b93-438">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="b1b93-439">Проверьте приведенный выше код, выполнив [скачиваемый пример](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span><span class="sxs-lookup"><span data-stu-id="b1b93-439">Test the preceding code by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="b1b93-440">Вызовите средства для разработчика (F12).</span><span class="sxs-lookup"><span data-stu-id="b1b93-440">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="b1b93-441">Перейдите к `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-441">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="b1b93-442">В средствах для разработчика (F12) отобразятся следующие заголовки ответа, добавленные примером кода:</span><span class="sxs-lookup"><span data-stu-id="b1b93-442">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="b1b93-443">**author:** `Rick Anderson`;</span><span class="sxs-lookup"><span data-stu-id="b1b93-443">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="b1b93-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`;</span><span class="sxs-lookup"><span data-stu-id="b1b93-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="b1b93-445">**internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-445">**internal:** `My header`</span></span>

<span data-ttu-id="b1b93-446">В приведенном выше коде создается заголовок ответа **internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-446">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="b1b93-447">Реализация IFilterFactory в атрибуте</span><span class="sxs-lookup"><span data-stu-id="b1b93-447">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="b1b93-448">Фильтры, реализующие `IFilterFactory`, используются для фильтров, которые:</span><span class="sxs-lookup"><span data-stu-id="b1b93-448">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="b1b93-449">Не требуют передачи параметров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-449">Don't require passing parameters.</span></span>
* <span data-ttu-id="b1b93-450">Содержат зависимости конструктора, которые должны заполняться путем внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-450">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="b1b93-451">Объект <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> реализует интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="b1b93-452">`IFilterFactory` предоставляет метод <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> для создания экземпляра <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-452">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="b1b93-453">`CreateInstance` загружает указанный тип из контейнера служб (внедрение зависимостей).</span><span class="sxs-lookup"><span data-stu-id="b1b93-453">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="b1b93-454">В коде ниже приведено три примера применения `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-454">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="b1b93-455">В приведенном выше коде декорирование метода с использованием `[SampleActionFilter]` является рекомендуемым способом применения `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-455">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="b1b93-456">Использование ПО промежуточного слоя в конвейере фильтров</span><span class="sxs-lookup"><span data-stu-id="b1b93-456">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="b1b93-457">Фильтры ресурсов по принципу работы похожи на [ПО промежуточного слоя](xref:fundamentals/middleware/index) тем, что они заключают в себя выполнение всех объектов, находящихся далее в конвейере.</span><span class="sxs-lookup"><span data-stu-id="b1b93-457">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="b1b93-458">Однако фильтры отличаются от ПО промежуточного слоя тем, что они являются частью среды выполнения, а значит имеют доступ к контексту и конструкциям.</span><span class="sxs-lookup"><span data-stu-id="b1b93-458">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="b1b93-459">Чтобы использовать ПО промежуточного слоя в качестве фильтра, создайте тип с методом `Configure`, определяющим ПО промежуточного слоя, которое нужно внедрить в конвейер фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-459">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="b1b93-460">В примере ниже ПО промежуточного слоя локализации применяется для определения текущих языка и региональных параметров для запроса:</span><span class="sxs-lookup"><span data-stu-id="b1b93-460">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="b1b93-461">Используйте <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> для выполнения ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="b1b93-461">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="b1b93-462">Фильтры ПО промежуточного слоя выполняются на том же этапе конвейера фильтров, что и фильтры ресурсов, перед привязкой модели и после остальной части конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-462">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="b1b93-463">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="b1b93-463">Next actions</span></span>

* <span data-ttu-id="b1b93-464">Дополнительные сведения см. в статье [Filter methods for Razor Pages in ASP.NET Core](xref:razor-pages/filter) (Методы фильтрации для Razor Pages в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="b1b93-464">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="b1b93-465">Чтобы поэкспериментировать с фильтрами, [скачайте, протестируйте и измените пример с GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span><span class="sxs-lookup"><span data-stu-id="b1b93-465">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b1b93-466">Авторы: [Кирк Ларкин](https://github.com/serpent5) (Kirk Larkin) [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Том Дайкстра](https://github.com/tdykstra/) (Tom Dykstra) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="b1b93-466">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b1b93-467">*Фильтры* в ASP.NET Core позволяют выполнять код до или после определенных этапов в конвейере обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-467">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="b1b93-468">Встроенные фильтры обрабатывают следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="b1b93-468">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="b1b93-469">Авторизация (предотвращение несанкционированного доступа к ресурсам).</span><span class="sxs-lookup"><span data-stu-id="b1b93-469">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="b1b93-470">Кэширование откликов (замыкание конвейера обработки запросов для возврата кэшированного ответа).</span><span class="sxs-lookup"><span data-stu-id="b1b93-470">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="b1b93-471">Для обработки сквозной функциональности можно создавать настраиваемые фильтры.</span><span class="sxs-lookup"><span data-stu-id="b1b93-471">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="b1b93-472">Примеры сквозной функциональности включают обработку ошибок, кэширование, конфигурирование, авторизацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="b1b93-472">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="b1b93-473">Фильтры предотвращают дублирование кода.</span><span class="sxs-lookup"><span data-stu-id="b1b93-473">Filters avoid duplicating code.</span></span> <span data-ttu-id="b1b93-474">Например, можно объединить обработку ошибок с помощью фильтра исключений обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="b1b93-474">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="b1b93-475">Этот документ применим к Razor Pages, контроллерам API и контроллерам с представлениями.</span><span class="sxs-lookup"><span data-stu-id="b1b93-475">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="b1b93-476">[Просмотреть или скачать пример](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([как скачивать](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b1b93-476">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="b1b93-477">Как работают фильтры</span><span class="sxs-lookup"><span data-stu-id="b1b93-477">How filters work</span></span>

<span data-ttu-id="b1b93-478">Фильтры выполняются в *конвейере вызова действий ASP.NET Core*, который иногда называют *конвейером фильтров*.</span><span class="sxs-lookup"><span data-stu-id="b1b93-478">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="b1b93-479">Конвейер фильтров запускается после того, как платформа ASP.NET Core выбирает выполняемое действие.</span><span class="sxs-lookup"><span data-stu-id="b1b93-479">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Запрос обрабатывается с использованием прочего ПО промежуточного слоя, ПО промежуточного слоя маршрутизации, выбора действия и конвейера вызова действий ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="b1b93-482">Типы фильтров</span><span class="sxs-lookup"><span data-stu-id="b1b93-482">Filter types</span></span>

<span data-ttu-id="b1b93-483">Фильтр каждого типа выполняется на определенном этапе конвейера фильтров:</span><span class="sxs-lookup"><span data-stu-id="b1b93-483">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="b1b93-484">[Фильтры авторизации](#authorization-filters). Применяются в первую очередь и служат для определения того, авторизован ли пользователь для выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="b1b93-484">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="b1b93-485">Эти фильтры могут сократить выполнение конвейера, если запрос не авторизован.</span><span class="sxs-lookup"><span data-stu-id="b1b93-485">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="b1b93-486">[Фильтры ресурсов](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="b1b93-486">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="b1b93-487">Выполняются после авторизации.</span><span class="sxs-lookup"><span data-stu-id="b1b93-487">Run after authorization.</span></span>  
  * <span data-ttu-id="b1b93-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> может выполнять код до остальной части конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="b1b93-489">Например, `OnResourceExecuting` может выполнять код до привязки модели.</span><span class="sxs-lookup"><span data-stu-id="b1b93-489">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="b1b93-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> может выполнять код после завершения остальной части конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="b1b93-491">[Фильтры действий](#action-filters) могут выполнять код непосредственно до и после вызова отдельного метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-491">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="b1b93-492">С их помощью можно управлять аргументами, передаваемыми в действие, и возвращаемым из него результатом.</span><span class="sxs-lookup"><span data-stu-id="b1b93-492">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="b1b93-493">Фильтры действий **не** поддерживаются в Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b1b93-493">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="b1b93-494">[Фильтры исключений](#exception-filters) служат для применения глобальных политик к необработанным исключениям, которые происходят до записи данных в тело ответа.</span><span class="sxs-lookup"><span data-stu-id="b1b93-494">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="b1b93-495">[Фильтры результатов](#result-filters) могут выполнять код непосредственно до и после выполнения результатов отдельного действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-495">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="b1b93-496">Они выполняются только в том случае, если метод действия выполнен успешно.</span><span class="sxs-lookup"><span data-stu-id="b1b93-496">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="b1b93-497">Они полезны для логики, которая должна включать исключения для представлений или модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="b1b93-497">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="b1b93-498">На приведенной ниже схеме показано, как типы фильтров взаимодействуют друг с другом в конвейере фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-498">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Запрос обрабатывается посредством фильтров авторизации, фильтров ресурсов, привязки модели, фильтров действий, выполнения действия и преобразования результата действия, фильтров исключений, фильтров результатов и выполнения результатов.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="b1b93-501">Реализация</span><span class="sxs-lookup"><span data-stu-id="b1b93-501">Implementation</span></span>

<span data-ttu-id="b1b93-502">Фильтры поддерживают как синхронные, так и асинхронные реализации посредством различных определений интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-502">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="b1b93-503">Синхронные фильтры могут выполнять код до (`On-Stage-Executing`) и после (`On-Stage-Executed`) этапа конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-503">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="b1b93-504">Например, `OnActionExecuting` вызывается перед вызовом метода действия,</span><span class="sxs-lookup"><span data-stu-id="b1b93-504">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="b1b93-505">а `OnActionExecuted` — после возврата метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-505">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="b1b93-506">Асинхронные фильтры определяют метод `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-506">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="b1b93-507">В приведенном выше коде `SampleAsyncActionFilter` включает <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) для выполнения метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-507">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="b1b93-508">Каждый из методов `On-Stage-ExecutionAsync` принимает `FilterType-ExecutionDelegate` для выполнения этапа конвейера фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-508">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="b1b93-509">Несколько этапов фильтра</span><span class="sxs-lookup"><span data-stu-id="b1b93-509">Multiple filter stages</span></span>

<span data-ttu-id="b1b93-510">Реализовать интерфейсы для нескольких этапов фильтра можно в одном классе.</span><span class="sxs-lookup"><span data-stu-id="b1b93-510">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="b1b93-511">Например, класс <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> реализует интерфейсы `IActionFilter` и `IResultFilter`, а также их асинхронные эквиваленты.</span><span class="sxs-lookup"><span data-stu-id="b1b93-511">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="b1b93-512">Реализуйте синхронный **или** асинхронный интерфейс фильтра, но **не** оба варианта.</span><span class="sxs-lookup"><span data-stu-id="b1b93-512">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="b1b93-513">Среда выполнения сначала проверяет, реализует ли фильтр асинхронный интерфейс. Если да, вызывается именно он.</span><span class="sxs-lookup"><span data-stu-id="b1b93-513">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="b1b93-514">В противном случае вызываются методы синхронного интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b1b93-514">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="b1b93-515">Если асинхронный и синхронный интерфейсы реализованы в одном классе, вызывается только асинхронный метод.</span><span class="sxs-lookup"><span data-stu-id="b1b93-515">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="b1b93-516">При использовании абстрактных классов, таких как <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, следует переопределять только синхронные методы или асинхронный метод каждого типа фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-516">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="b1b93-517">Встроенные атрибуты фильтров</span><span class="sxs-lookup"><span data-stu-id="b1b93-517">Built-in filter attributes</span></span>

<span data-ttu-id="b1b93-518">ASP.NET Core включает встроенные фильтры на основе атрибутов, с помощью которых можно создавать подклассы и которые можно настраивать.</span><span class="sxs-lookup"><span data-stu-id="b1b93-518">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="b1b93-519">Например, следующий фильтр результатов добавляет заголовок к ответу:</span><span class="sxs-lookup"><span data-stu-id="b1b93-519">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="b1b93-520">Атрибуты позволяют фильтрам принимать аргументы, как показано в примере выше.</span><span class="sxs-lookup"><span data-stu-id="b1b93-520">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="b1b93-521">Примените `AddHeaderAttribute` к методу контроллера или действия, а затем укажите имя и значение заголовка HTTP:</span><span class="sxs-lookup"><span data-stu-id="b1b93-521">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="b1b93-522">Некоторые интерфейсы фильтров имеют соответствующие атрибуты, которые можно использовать как базовые классы для пользовательских реализаций.</span><span class="sxs-lookup"><span data-stu-id="b1b93-522">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="b1b93-523">Атрибуты фильтров:</span><span class="sxs-lookup"><span data-stu-id="b1b93-523">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="b1b93-524">Области и порядок выполнения фильтров</span><span class="sxs-lookup"><span data-stu-id="b1b93-524">Filter scopes and order of execution</span></span>

<span data-ttu-id="b1b93-525">Фильтр можно добавить в конвейер в одной из трех *областей*:</span><span class="sxs-lookup"><span data-stu-id="b1b93-525">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="b1b93-526">С помощью атрибута в действии.</span><span class="sxs-lookup"><span data-stu-id="b1b93-526">Using an attribute on an action.</span></span>
* <span data-ttu-id="b1b93-527">С помощью атрибута в контроллере.</span><span class="sxs-lookup"><span data-stu-id="b1b93-527">Using an attribute on a controller.</span></span>
* <span data-ttu-id="b1b93-528">Глобально для всех контроллеров и действий, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="b1b93-528">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="b1b93-529">Предыдущий код глобально добавляет три фильтра с помощью коллекции [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="b1b93-529">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="b1b93-530">Порядок выполнения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="b1b93-530">Default order of execution</span></span>

<span data-ttu-id="b1b93-531">Если есть несколько *одинаковых* фильтров, область определяет порядок их выполнения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b1b93-531">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="b1b93-532">Глобальные фильтры заключают в себя фильтры классов,</span><span class="sxs-lookup"><span data-stu-id="b1b93-532">Global filters surround class filters.</span></span> <span data-ttu-id="b1b93-533">которые, в свою очередь, заключают в себя фильтры методов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-533">Class filters surround method filters.</span></span>

<span data-ttu-id="b1b93-534">В результате такого вложения *последующий* код фильтров выполняется в порядке, обратном выполнению *предшествующего* кода.</span><span class="sxs-lookup"><span data-stu-id="b1b93-534">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="b1b93-535">Последовательность фильтров:</span><span class="sxs-lookup"><span data-stu-id="b1b93-535">The filter sequence:</span></span>

* <span data-ttu-id="b1b93-536">*Предшествующий* код глобальных фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-536">The *before* code of global filters.</span></span>
  * <span data-ttu-id="b1b93-537">*Предшествующий* код фильтров контроллера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-537">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="b1b93-538">*Предшествующий* код фильтров методов действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-538">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="b1b93-539">*Последующий* код фильтров методов действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-539">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="b1b93-540">*Последующий* код фильтров контроллера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-540">The *after* code of controller filters.</span></span>
* <span data-ttu-id="b1b93-541">*Последующий* код глобальных фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-541">The *after* code of global filters.</span></span>
  
<span data-ttu-id="b1b93-542">В следующем примере показан порядок вызова методов фильтров для синхронных фильтров действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-542">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="b1b93-543">Sequence</span><span class="sxs-lookup"><span data-stu-id="b1b93-543">Sequence</span></span> | <span data-ttu-id="b1b93-544">Область фильтра</span><span class="sxs-lookup"><span data-stu-id="b1b93-544">Filter scope</span></span> | <span data-ttu-id="b1b93-545">Метод фильтра</span><span class="sxs-lookup"><span data-stu-id="b1b93-545">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="b1b93-546">1</span><span class="sxs-lookup"><span data-stu-id="b1b93-546">1</span></span> | <span data-ttu-id="b1b93-547">Global</span><span class="sxs-lookup"><span data-stu-id="b1b93-547">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b1b93-548">2</span><span class="sxs-lookup"><span data-stu-id="b1b93-548">2</span></span> | <span data-ttu-id="b1b93-549">Контроллер</span><span class="sxs-lookup"><span data-stu-id="b1b93-549">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b1b93-550">3</span><span class="sxs-lookup"><span data-stu-id="b1b93-550">3</span></span> | <span data-ttu-id="b1b93-551">Метод</span><span class="sxs-lookup"><span data-stu-id="b1b93-551">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b1b93-552">4</span><span class="sxs-lookup"><span data-stu-id="b1b93-552">4</span></span> | <span data-ttu-id="b1b93-553">Метод</span><span class="sxs-lookup"><span data-stu-id="b1b93-553">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="b1b93-554">5</span><span class="sxs-lookup"><span data-stu-id="b1b93-554">5</span></span> | <span data-ttu-id="b1b93-555">Контроллер</span><span class="sxs-lookup"><span data-stu-id="b1b93-555">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="b1b93-556">6</span><span class="sxs-lookup"><span data-stu-id="b1b93-556">6</span></span> | <span data-ttu-id="b1b93-557">Global</span><span class="sxs-lookup"><span data-stu-id="b1b93-557">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="b1b93-558">Эта последовательность показывает:</span><span class="sxs-lookup"><span data-stu-id="b1b93-558">This sequence shows:</span></span>

* <span data-ttu-id="b1b93-559">Фильтр метода вкладывается в фильтр контроллера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-559">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="b1b93-560">Фильтр контроллера вкладывается в глобальный фильтр.</span><span class="sxs-lookup"><span data-stu-id="b1b93-560">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="b1b93-561">Фильтры уровня контроллера и страницы Razor</span><span class="sxs-lookup"><span data-stu-id="b1b93-561">Controller and Razor Page level filters</span></span>

<span data-ttu-id="b1b93-562">Каждый контроллер, наследуемый от базового класса <xref:Microsoft.AspNetCore.Mvc.Controller> включает методы [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) и [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-562">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="b1b93-563">Эти методы:</span><span class="sxs-lookup"><span data-stu-id="b1b93-563">These methods:</span></span>

* <span data-ttu-id="b1b93-564">Заключают фильтры, которые выполняются для указанного действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-564">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="b1b93-565">`OnActionExecuting` вызывается перед всеми фильтрами действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-565">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="b1b93-566">`OnActionExecuted` вызывается после всех фильтров действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-566">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="b1b93-567">`OnActionExecutionAsync` вызывается перед всеми фильтрами действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-567">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="b1b93-568">Код в фильтре после `next` выполняется после метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-568">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="b1b93-569">Например, в скачиваемом примере `MySampleActionFilter` применяется глобально при запуске.</span><span class="sxs-lookup"><span data-stu-id="b1b93-569">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="b1b93-570">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-570">The `TestController`:</span></span>

* <span data-ttu-id="b1b93-571">Применяет `SampleActionFilterAttribute` (`[SampleActionFilter]`) для действия `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-571">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="b1b93-572">Переопределяет `OnActionExecuting` и `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-572">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="b1b93-573">Переходит к `https://localhost:5001/Test/FilterTest2` для выполнения следующего кода:</span><span class="sxs-lookup"><span data-stu-id="b1b93-573">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="b1b93-574">См. подробнее о [реализации фильтров страницы Razor путем переопределения методов фильтра](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="b1b93-574">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="b1b93-575">Переопределение порядка по умолчанию</span><span class="sxs-lookup"><span data-stu-id="b1b93-575">Overriding the default order</span></span>

<span data-ttu-id="b1b93-576">Чтобы переопределить порядок выполнения по умолчанию, можно реализовать <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-576">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="b1b93-577">`IOrderedFilter` предоставляет свойство <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, которое имеет приоритет над областью и определяет порядок выполнения.</span><span class="sxs-lookup"><span data-stu-id="b1b93-577">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="b1b93-578">Фильтр со значением меньше `Order`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-578">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="b1b93-579">Выполняется *перед* кодом, выполняемым до фильтра с более высоким значением `Order`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-579">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="b1b93-580">Выполняется *после* кода, выполняемого после фильтра с более высоким значением `Order`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-580">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="b1b93-581">Свойство `Order` можно задать с помощью параметра конструктора:</span><span class="sxs-lookup"><span data-stu-id="b1b93-581">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="b1b93-582">Рассмотрим три фильтра действий, показанные в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="b1b93-582">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="b1b93-583">Если свойство `Order` контроллера и глобальные фильтры имеют значения 1 и 2 соответственно, порядок выполнения инвертируется.</span><span class="sxs-lookup"><span data-stu-id="b1b93-583">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="b1b93-584">Sequence</span><span class="sxs-lookup"><span data-stu-id="b1b93-584">Sequence</span></span> | <span data-ttu-id="b1b93-585">Область фильтра</span><span class="sxs-lookup"><span data-stu-id="b1b93-585">Filter scope</span></span> | <span data-ttu-id="b1b93-586">Свойство`Order`</span><span class="sxs-lookup"><span data-stu-id="b1b93-586">`Order` property</span></span> | <span data-ttu-id="b1b93-587">Метод фильтра</span><span class="sxs-lookup"><span data-stu-id="b1b93-587">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="b1b93-588">1</span><span class="sxs-lookup"><span data-stu-id="b1b93-588">1</span></span> | <span data-ttu-id="b1b93-589">Метод</span><span class="sxs-lookup"><span data-stu-id="b1b93-589">Method</span></span> | <span data-ttu-id="b1b93-590">0</span><span class="sxs-lookup"><span data-stu-id="b1b93-590">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b1b93-591">2</span><span class="sxs-lookup"><span data-stu-id="b1b93-591">2</span></span> | <span data-ttu-id="b1b93-592">Контроллер</span><span class="sxs-lookup"><span data-stu-id="b1b93-592">Controller</span></span> | <span data-ttu-id="b1b93-593">1</span><span class="sxs-lookup"><span data-stu-id="b1b93-593">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="b1b93-594">3</span><span class="sxs-lookup"><span data-stu-id="b1b93-594">3</span></span> | <span data-ttu-id="b1b93-595">Global</span><span class="sxs-lookup"><span data-stu-id="b1b93-595">Global</span></span> | <span data-ttu-id="b1b93-596">2</span><span class="sxs-lookup"><span data-stu-id="b1b93-596">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="b1b93-597">4</span><span class="sxs-lookup"><span data-stu-id="b1b93-597">4</span></span> | <span data-ttu-id="b1b93-598">Global</span><span class="sxs-lookup"><span data-stu-id="b1b93-598">Global</span></span> | <span data-ttu-id="b1b93-599">2</span><span class="sxs-lookup"><span data-stu-id="b1b93-599">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="b1b93-600">5</span><span class="sxs-lookup"><span data-stu-id="b1b93-600">5</span></span> | <span data-ttu-id="b1b93-601">Контроллер</span><span class="sxs-lookup"><span data-stu-id="b1b93-601">Controller</span></span> | <span data-ttu-id="b1b93-602">1</span><span class="sxs-lookup"><span data-stu-id="b1b93-602">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="b1b93-603">6</span><span class="sxs-lookup"><span data-stu-id="b1b93-603">6</span></span> | <span data-ttu-id="b1b93-604">Метод</span><span class="sxs-lookup"><span data-stu-id="b1b93-604">Method</span></span> | <span data-ttu-id="b1b93-605">0</span><span class="sxs-lookup"><span data-stu-id="b1b93-605">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="b1b93-606">Свойство `Order` переопределяет область при определении порядка выполнения фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-606">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="b1b93-607">Фильтры сначала сортируются по порядку, а затем очередность окончательно определяется по области.</span><span class="sxs-lookup"><span data-stu-id="b1b93-607">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="b1b93-608">Все встроенные фильтры реализуют `IOrderedFilter` и задают значение по умолчанию `Order`, равное 0.</span><span class="sxs-lookup"><span data-stu-id="b1b93-608">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="b1b93-609">Во встроенных фильтрах область определяет порядок, если для `Order` не задано ненулевое значение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-609">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="b1b93-610">Отмена и сокращенное выполнение</span><span class="sxs-lookup"><span data-stu-id="b1b93-610">Cancellation and short-circuiting</span></span>

<span data-ttu-id="b1b93-611">Вы можете настроить сокращенное выполнение конвейера фильтров, задав свойство <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> для параметра <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext>, передаваемого в метод фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-611">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="b1b93-612">Например, приведенный ниже фильтр ресурсов предотвращает выполнение остальной части конвейера:</span><span class="sxs-lookup"><span data-stu-id="b1b93-612">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="b1b93-613">В приведенном ниже коде как фильтр `ShortCircuitingResourceFilter`, так и фильтр `AddHeader` нацелены на метод действия `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-613">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="b1b93-614">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-614">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="b1b93-615">Выполняется первым, поскольку это фильтр ресурсов, а `AddHeader` — фильтр действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-615">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="b1b93-616">Замыкает оставшуюся часть конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-616">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="b1b93-617">Поэтому фильтр `AddHeader` никогда не выполняется для действия `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-617">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="b1b93-618">Поведение будет аналогичным при применении обоих фильтров на уровне метода действия при условии, что фильтр `ShortCircuitingResourceFilter` выполняется первым.</span><span class="sxs-lookup"><span data-stu-id="b1b93-618">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="b1b93-619">Фильтр `ShortCircuitingResourceFilter` выполняется в первую очередь из-за его типа или в связи с явным указанием свойства `Order`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-619">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="b1b93-620">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="b1b93-620">Dependency injection</span></span>

<span data-ttu-id="b1b93-621">Фильтры можно добавлять по типу или экземпляру.</span><span class="sxs-lookup"><span data-stu-id="b1b93-621">Filters can be added by type or by instance.</span></span> <span data-ttu-id="b1b93-622">Добавляемый экземпляр используется для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="b1b93-622">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="b1b93-623">Если добавляется тип, он является активированным.</span><span class="sxs-lookup"><span data-stu-id="b1b93-623">If a type is added, it's type-activated.</span></span> <span data-ttu-id="b1b93-624">Активация фильтра означает, что:</span><span class="sxs-lookup"><span data-stu-id="b1b93-624">A type-activated filter means:</span></span>

* <span data-ttu-id="b1b93-625">Экземпляр создается для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="b1b93-625">An instance is created for each request.</span></span>
* <span data-ttu-id="b1b93-626">Зависимости конструктора заполняются путем [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b1b93-626">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="b1b93-627">Фильтры, которые реализуются как атрибуты и добавляются непосредственно в классы контроллеров или методы действий, не могут иметь зависимости конструктора, предоставленные посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b1b93-627">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="b1b93-628">Зависимости конструктора не могут предоставляться путем внедрения зависимостей, так как:</span><span class="sxs-lookup"><span data-stu-id="b1b93-628">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="b1b93-629">Параметры конструктора должны предоставляться для атрибутов в месте их применения.</span><span class="sxs-lookup"><span data-stu-id="b1b93-629">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="b1b93-630">Это ограничение, налагаемое на использование атрибутов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-630">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="b1b93-631">Следующие фильтры поддерживают зависимости конструктора, предоставленные путем внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="b1b93-631">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="b1b93-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> реализуется в атрибуте.</span><span class="sxs-lookup"><span data-stu-id="b1b93-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="b1b93-633">Предыдущие фильтры могут применяться к контроллеру или методу действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-633">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="b1b93-634">Средства ведения журнала доступны путем внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-634">Loggers are available from DI.</span></span> <span data-ttu-id="b1b93-635">Но следует избегать создания и использования фильтров исключительно для целей ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b1b93-635">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="b1b93-636">[Встроенное средство ведения журнала](xref:fundamentals/logging/index) обычно предоставляет все необходимое для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b1b93-636">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="b1b93-637">При добавлении ведения журналов в фильтры следует учитывать следующее:</span><span class="sxs-lookup"><span data-stu-id="b1b93-637">Logging added to filters:</span></span>

* <span data-ttu-id="b1b93-638">Следует сосредоточиться на бизнес-среде или поведении в привязке к фильтру.</span><span class="sxs-lookup"><span data-stu-id="b1b93-638">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="b1b93-639">**Не** следует регистрировать действия или другие события платформы,</span><span class="sxs-lookup"><span data-stu-id="b1b93-639">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="b1b93-640">так как это делают встроенные фильтры.</span><span class="sxs-lookup"><span data-stu-id="b1b93-640">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="b1b93-641">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="b1b93-641">ServiceFilterAttribute</span></span>

<span data-ttu-id="b1b93-642">Типы реализации фильтра службы регистрируются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-642">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="b1b93-643">Атрибут <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> извлекает экземпляр фильтра из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-643">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="b1b93-644">В следующем коде используется `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-644">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="b1b93-645">В следующем коде в контейнер внедрения зависимостей добавляется `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-645">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="b1b93-646">В следующем коде атрибут `ServiceFilter` извлекает экземпляр фильтра `AddHeaderResultServiceFilter` из внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="b1b93-646">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="b1b93-647">При использовании `ServiceFilterAttribute` задание [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="b1b93-647">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="b1b93-648">Указывает, что экземпляр фильтра *можно* многократно использовать за пределами области запроса, в которой он был создан.</span><span class="sxs-lookup"><span data-stu-id="b1b93-648">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="b1b93-649">Среда выполнения ASP.NET Core не гарантирует:</span><span class="sxs-lookup"><span data-stu-id="b1b93-649">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="b1b93-650">что будет создан хотя бы один экземпляр фильтра;</span><span class="sxs-lookup"><span data-stu-id="b1b93-650">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="b1b93-651">что фильтр не будет повторно запрошен из контейнера внедрения зависимостей на более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="b1b93-651">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="b1b93-652">Не следует использовать с фильтром, который зависит от служб со временем существования, кроме элементов singleton.</span><span class="sxs-lookup"><span data-stu-id="b1b93-652">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="b1b93-653">Объект <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> реализует интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="b1b93-654">`IFilterFactory` предоставляет метод <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> для создания экземпляра <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-654">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="b1b93-655">`CreateInstance` загружает указанный тип из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-655">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="b1b93-656">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="b1b93-656">TypeFilterAttribute</span></span>

<span data-ttu-id="b1b93-657">Атрибут <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> похож на <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, но его тип не разрешается напрямую из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="b1b93-658">Он создает экземпляр типа с помощью <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-658">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="b1b93-659">Так как типы `TypeFilterAttribute` не разрешаются напрямую из контейнера внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="b1b93-659">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="b1b93-660">Типы, на которые ссылаются с помощью `TypeFilterAttribute`, не нужно регистрировать в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-660">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="b1b93-661">Их зависимости выполняются самим контейнером.</span><span class="sxs-lookup"><span data-stu-id="b1b93-661">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="b1b93-662">Атрибут `TypeFilterAttribute` может также принимать аргументы конструктора для типа.</span><span class="sxs-lookup"><span data-stu-id="b1b93-662">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="b1b93-663">При использовании `TypeFilterAttribute` задание [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="b1b93-663">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="b1b93-664">Указывает, что экземпляр фильтра *можно* многократно использовать за пределами области запроса, в которой он был создан.</span><span class="sxs-lookup"><span data-stu-id="b1b93-664">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="b1b93-665">Среда выполнения ASP.NET Core не предоставляет никаких гарантий, что будет создан хотя бы один экземпляр фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-665">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="b1b93-666">Не следует использовать с фильтром, который зависит от служб со временем существования, кроме элементов singleton.</span><span class="sxs-lookup"><span data-stu-id="b1b93-666">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="b1b93-667">В следующем примере показано, как передавать аргументы в тип с помощью `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-667">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="b1b93-668">Фильтры авторизации</span><span class="sxs-lookup"><span data-stu-id="b1b93-668">Authorization filters</span></span>

<span data-ttu-id="b1b93-669">Фильтры авторизации:</span><span class="sxs-lookup"><span data-stu-id="b1b93-669">Authorization filters:</span></span>

* <span data-ttu-id="b1b93-670">Выполняются в первую очередь в конвейере фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-670">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="b1b93-671">Контролируют доступ к методам действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-671">Control access to action methods.</span></span>
* <span data-ttu-id="b1b93-672">Имеют предшествующий, но не последующий метод.</span><span class="sxs-lookup"><span data-stu-id="b1b93-672">Have a before method, but no after method.</span></span>

<span data-ttu-id="b1b93-673">Для работы пользовательских фильтров авторизации требуется настраиваемая платформа авторизации.</span><span class="sxs-lookup"><span data-stu-id="b1b93-673">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="b1b93-674">Настройка политик авторизации или определение пользовательской политики авторизации предпочтительнее создания пользовательского фильтра.</span><span class="sxs-lookup"><span data-stu-id="b1b93-674">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="b1b93-675">Встроенный фильтр авторизации:</span><span class="sxs-lookup"><span data-stu-id="b1b93-675">The built-in authorization filter:</span></span>

* <span data-ttu-id="b1b93-676">Вызывает систему авторизации.</span><span class="sxs-lookup"><span data-stu-id="b1b93-676">Calls the authorization system.</span></span>
* <span data-ttu-id="b1b93-677">Не выполняет авторизацию запросов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-677">Does not authorize requests.</span></span>

<span data-ttu-id="b1b93-678">**Не** вызывайте исключения в фильтрах авторизации:</span><span class="sxs-lookup"><span data-stu-id="b1b93-678">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="b1b93-679">Исключение не будет обработано.</span><span class="sxs-lookup"><span data-stu-id="b1b93-679">The exception will not be handled.</span></span>
* <span data-ttu-id="b1b93-680">Фильтры исключений не будут обрабатывать исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-680">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="b1b93-681">При возникновении исключения в фильтре авторизации попробуйте создать запрос.</span><span class="sxs-lookup"><span data-stu-id="b1b93-681">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="b1b93-682">Дополнительные сведения об [авторизации](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="b1b93-682">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="b1b93-683">Фильтры ресурсов</span><span class="sxs-lookup"><span data-stu-id="b1b93-683">Resource filters</span></span>

<span data-ttu-id="b1b93-684">Фильтры ресурсов:</span><span class="sxs-lookup"><span data-stu-id="b1b93-684">Resource filters:</span></span>

* <span data-ttu-id="b1b93-685">Реализуют либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter>, либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-685">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="b1b93-686">Выполнение охватывает большую часть конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-686">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="b1b93-687">До фильтров ресурсов применяются только [фильтры авторизации](#authorization-filters).</span><span class="sxs-lookup"><span data-stu-id="b1b93-687">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="b1b93-688">Фильтры ресурсов полезны для сокращенного выполнения большей части конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-688">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="b1b93-689">Например, фильтр кэширования предотвращает выполнение остальной части конвейера при попадании в кэше.</span><span class="sxs-lookup"><span data-stu-id="b1b93-689">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="b1b93-690">Примеры фильтров ресурсов:</span><span class="sxs-lookup"><span data-stu-id="b1b93-690">Resource filter examples:</span></span>

* <span data-ttu-id="b1b93-691">[Сокращенное выполнение фильтра ресурсов](#short-circuiting-resource-filter) показано ранее.</span><span class="sxs-lookup"><span data-stu-id="b1b93-691">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="b1b93-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="b1b93-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="b1b93-693">Предотвращает доступ привязки модели к данным формы.</span><span class="sxs-lookup"><span data-stu-id="b1b93-693">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="b1b93-694">Используется для загрузки больших файлов, если необходимо предотвратить считывание данных формы в память.</span><span class="sxs-lookup"><span data-stu-id="b1b93-694">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="b1b93-695">Фильтры действий</span><span class="sxs-lookup"><span data-stu-id="b1b93-695">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b1b93-696">Фильтры действий **неприменимы** к Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b1b93-696">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="b1b93-697">Razor Pages поддерживает <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> и <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-697">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="b1b93-698">Дополнительные сведения см. в разделе [Методы фильтрации для Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="b1b93-698">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="b1b93-699">Фильтры действий:</span><span class="sxs-lookup"><span data-stu-id="b1b93-699">Action filters:</span></span>

* <span data-ttu-id="b1b93-700">Реализуют либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter>, либо интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-700">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="b1b93-701">Их выполнение охватывает выполнение методов действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-701">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="b1b93-702">В следующем фрагменте кода показан пример фильтра действий:</span><span class="sxs-lookup"><span data-stu-id="b1b93-702">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="b1b93-703"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> предоставляет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="b1b93-703">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="b1b93-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> позволяет считать входные данные в метод действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="b1b93-705"><xref:Microsoft.AspNetCore.Mvc.Controller> позволяет управлять экземпляром контроллера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-705"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="b1b93-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> позволяет при задании свойства `Result` сократить выполнение метода действия и последующих фильтров действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="b1b93-707">Вызов исключения в методе действия:</span><span class="sxs-lookup"><span data-stu-id="b1b93-707">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="b1b93-708">Предотвращает выполнение последующих фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-708">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="b1b93-709">В отличие от параметра `Result` рассматривается как сбой, а не успешный результат.</span><span class="sxs-lookup"><span data-stu-id="b1b93-709">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="b1b93-710"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> предоставляет `Controller` и `Result`, а также следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="b1b93-710">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="b1b93-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> будет иметь значение true, если выполнение действия было сокращено другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="b1b93-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="b1b93-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> будет иметь ненулевое значение, если предыдущее выполнение фильтра действий вызвало исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="b1b93-713">При присвоении этому свойству ненулевого значения:</span><span class="sxs-lookup"><span data-stu-id="b1b93-713">Setting this property to null:</span></span>

  * <span data-ttu-id="b1b93-714">Будет эффективно обрабатываться исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-714">Effectively handles the exception.</span></span>
  * <span data-ttu-id="b1b93-715">`Result` будет выполняться так, как при возвращении методом действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-715">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="b1b93-716">Для `IAsyncActionFilter` вызов <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="b1b93-716">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="b1b93-717">Приводит к выполнению последующих фильтров действий и метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-717">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="b1b93-718">Возвращает `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-718">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="b1b93-719">Для сокращенного выполнения присвойте <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> экземпляру результата и не вызывайте `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="b1b93-719">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="b1b93-720">Платформа предоставляет абстрактный класс <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, для которого можно создавать подклассы.</span><span class="sxs-lookup"><span data-stu-id="b1b93-720">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="b1b93-721">Фильтр действий `OnActionExecuting` можно использовать:</span><span class="sxs-lookup"><span data-stu-id="b1b93-721">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="b1b93-722">Для проверки состояния модели.</span><span class="sxs-lookup"><span data-stu-id="b1b93-722">Validate model state.</span></span>
* <span data-ttu-id="b1b93-723">Для получения ошибки, если состояние является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="b1b93-723">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="b1b93-724">Метод `OnActionExecuted` выполняется после метода действия:</span><span class="sxs-lookup"><span data-stu-id="b1b93-724">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="b1b93-725">Он имеет доступ к результатам действия и может управлять ими с помощью свойства <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-725">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="b1b93-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> будет иметь значение true, если выполнение действия было сокращено другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="b1b93-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="b1b93-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> будет иметь ненулевое значение, если действие или последующий фильтр действий вызвали исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="b1b93-728">Если установить для `Exception` значение NULL:</span><span class="sxs-lookup"><span data-stu-id="b1b93-728">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="b1b93-729">Будет эффективно обрабатываться исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-729">Effectively handles an exception.</span></span>
  * <span data-ttu-id="b1b93-730">`ActionExecutedContext.Result` будет выполняться так, как если бы метод действия вернул его обычным образом.</span><span class="sxs-lookup"><span data-stu-id="b1b93-730">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="b1b93-731">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="b1b93-731">Exception filters</span></span>

<span data-ttu-id="b1b93-732">Фильтры исключений:</span><span class="sxs-lookup"><span data-stu-id="b1b93-732">Exception filters:</span></span>

* <span data-ttu-id="b1b93-733">Реализуют <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-733">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="b1b93-734">Можно использовать для реализации политик обработки стандартных ошибок.</span><span class="sxs-lookup"><span data-stu-id="b1b93-734">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="b1b93-735">В следующем примере фильтра исключений используется пользовательское представление ошибок для отображения подробных сведений об исключениях, которые происходят во время разработки приложения:</span><span class="sxs-lookup"><span data-stu-id="b1b93-735">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="b1b93-736">Фильтры исключений:</span><span class="sxs-lookup"><span data-stu-id="b1b93-736">Exception filters:</span></span>

* <span data-ttu-id="b1b93-737">Не имеют предшествующих и последующих событий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-737">Don't have before and after events.</span></span>
* <span data-ttu-id="b1b93-738">Реализуют <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-738">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="b1b93-739">Обрабатывают необработанные исключения, которые возникают при создании страницы Razor или контроллера, в [привязке модели](xref:mvc/models/model-binding), фильтрах действий или методах действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-739">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="b1b93-740">**Не** перехватывают исключения, которые возникают в фильтрах ресурсов, фильтрах результатов или при выполнении результата MVC.</span><span class="sxs-lookup"><span data-stu-id="b1b93-740">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="b1b93-741">Для обработки исключения присвойте свойству <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> значение `true` или напишите ответ.</span><span class="sxs-lookup"><span data-stu-id="b1b93-741">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="b1b93-742">Это предотвратит распространение исключения.</span><span class="sxs-lookup"><span data-stu-id="b1b93-742">This stops propagation of the exception.</span></span> <span data-ttu-id="b1b93-743">Фильтр исключений не может преобразовать исключение в успешное выполнение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-743">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="b1b93-744">Это может сделать только фильтр действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-744">Only an action filter can do that.</span></span>

<span data-ttu-id="b1b93-745">Фильтры исключений:</span><span class="sxs-lookup"><span data-stu-id="b1b93-745">Exception filters:</span></span>

* <span data-ttu-id="b1b93-746">Хорошо подходят для перехвата исключений, возникающих в действиях.</span><span class="sxs-lookup"><span data-stu-id="b1b93-746">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="b1b93-747">Не обладает такой гибкостью, как ПО промежуточного слоя обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="b1b93-747">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="b1b93-748">ПО промежуточного слоя хорошо подходит для обработки исключений.</span><span class="sxs-lookup"><span data-stu-id="b1b93-748">Prefer middleware for exception handling.</span></span> <span data-ttu-id="b1b93-749">Используйте фильтры исключений, только если обработка ошибок осуществляется *по-разному* с учетом вызванного метода действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-749">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="b1b93-750">Например, в приложении могут использоваться методы действий как для конечных точек API, так и для представлений или HTML.</span><span class="sxs-lookup"><span data-stu-id="b1b93-750">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="b1b93-751">Конечные точки API могут возвращать сведения об ошибках в формате JSON, в то время как действия на основе представлений могут возвращать страницу ошибки в формате HTML.</span><span class="sxs-lookup"><span data-stu-id="b1b93-751">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="b1b93-752">Фильтры результатов</span><span class="sxs-lookup"><span data-stu-id="b1b93-752">Result filters</span></span>

<span data-ttu-id="b1b93-753">Фильтры результатов:</span><span class="sxs-lookup"><span data-stu-id="b1b93-753">Result filters:</span></span>

* <span data-ttu-id="b1b93-754">Реализация интерфейса:</span><span class="sxs-lookup"><span data-stu-id="b1b93-754">Implement an interface:</span></span>
  * <span data-ttu-id="b1b93-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="b1b93-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="b1b93-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> или <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="b1b93-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="b1b93-757">Их выполнение охватывает выполнение результатов действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-757">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="b1b93-758">IResultFilter и IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="b1b93-758">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="b1b93-759">В следующем фрагменте кода показан фильтр результатов, который добавляет заголовок HTTP:</span><span class="sxs-lookup"><span data-stu-id="b1b93-759">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="b1b93-760">Тип выполняемого результата зависит от соответствующего действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-760">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="b1b93-761">Действие, возвращающее представление, будет включать всю обработку Razor в рамках выполняемого объекта <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-761">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="b1b93-762">В процессе выполнения результата метод API может производить сериализацию.</span><span class="sxs-lookup"><span data-stu-id="b1b93-762">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="b1b93-763">Дополнительные сведения о [результатах действий](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="b1b93-763">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="b1b93-764">Фильтры результатов выполняются только в том случае, когда действие или фильтры действий предоставляют результат действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-764">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="b1b93-765">Фильтры результатов не выполняются в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="b1b93-765">Result filters are not executed when:</span></span>

* <span data-ttu-id="b1b93-766">Фильтр авторизации или ресурсов выполняет сокращение конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-766">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="b1b93-767">Фильтр исключений обрабатывает исключение и выдает результат действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-767">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="b1b93-768">Метод <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> может сокращать выполнение результата действия и последующих фильтров результатов, присваивая свойству <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> значение `true`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-768">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="b1b93-769">При сокращении выполнения выполните запись в объект ответа, чтобы избежать формирования пустого ответа.</span><span class="sxs-lookup"><span data-stu-id="b1b93-769">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="b1b93-770">Вызов исключения в `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-770">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="b1b93-771">Предотвращает выполнение результата действия и последующих фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-771">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="b1b93-772">Рассматривается как сбой, а не успешный результат.</span><span class="sxs-lookup"><span data-stu-id="b1b93-772">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="b1b93-773">Если запускается метод <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName>, ответ, скорее всего, уже был отправлен клиенту.</span><span class="sxs-lookup"><span data-stu-id="b1b93-773">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="b1b93-774">Если ответ уже был отправлен клиенту его больше нельзя изменить.</span><span class="sxs-lookup"><span data-stu-id="b1b93-774">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="b1b93-775">`ResultExecutedContext.Canceled` будет иметь значение `true`, если выполнение результата действия было сокращено другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="b1b93-775">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="b1b93-776">`ResultExecutedContext.Exception` будет иметь ненулевое значение, если результат действия или последующий фильтр результатов вызвали исключение.</span><span class="sxs-lookup"><span data-stu-id="b1b93-776">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="b1b93-777">Присвоение `Exception` значения NULL приводит к обработке исключения и предотвращает его последующий вызов ASP.NET Core на дальнейших этапах конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-777">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="b1b93-778">Надежный способ записи данных в ответ при обработке исключения в фильтре результатов отсутствует.</span><span class="sxs-lookup"><span data-stu-id="b1b93-778">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="b1b93-779">Если результат действия вызывает исключение в процессе выполнения и заголовки уже были переданы в клиент, надежного механизма отправки кода сбоя не существует.</span><span class="sxs-lookup"><span data-stu-id="b1b93-779">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="b1b93-780">Для <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> вызов к `await next` для <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> приводит к выполнению последующих фильтров результатов и результата действия.</span><span class="sxs-lookup"><span data-stu-id="b1b93-780">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="b1b93-781">Для сокращения выполнения задайте [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) для `true` и не вызывайте `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-781">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="b1b93-782">Платформа предоставляет абстрактный класс `ResultFilterAttribute`, для которого можно создавать подклассы.</span><span class="sxs-lookup"><span data-stu-id="b1b93-782">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="b1b93-783">Представленный ранее класс [AddHeaderAttribute](#add-header-attribute) — это пример атрибута фильтра результатов.</span><span class="sxs-lookup"><span data-stu-id="b1b93-783">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="b1b93-784">IAlwaysRunResultFilter и IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="b1b93-784">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="b1b93-785">Интерфейсы <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> и <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> объявляют реализацию <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>, которая выполняется для всех результатов действий.</span><span class="sxs-lookup"><span data-stu-id="b1b93-785">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="b1b93-786">Сюда включены результаты действий, созданные:</span><span class="sxs-lookup"><span data-stu-id="b1b93-786">This includes action results produced by:</span></span>

* <span data-ttu-id="b1b93-787">фильтрами авторизации и фильтрами ресурсов, которые сокращают ответ;</span><span class="sxs-lookup"><span data-stu-id="b1b93-787">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="b1b93-788">фильтрами исключений.</span><span class="sxs-lookup"><span data-stu-id="b1b93-788">Exception filters.</span></span>

<span data-ttu-id="b1b93-789">Например, следующий фильтр всегда выполняется, задавая результат действия (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) с кодом состояния *422 Unprocessable Entity* при сбое согласования содержимого:</span><span class="sxs-lookup"><span data-stu-id="b1b93-789">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="b1b93-790">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="b1b93-790">IFilterFactory</span></span>

<span data-ttu-id="b1b93-791">Объект <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> реализует интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="b1b93-792">Поэтому экземпляр `IFilterFactory` можно использовать в качестве экземпляра `IFilterMetadata` в любом месте конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-792">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="b1b93-793">Когда среда выполнения готовится вызвать фильтр, она пытается привести его к `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-793">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="b1b93-794">Если приведение проходит успешно, вызывается метод <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> для создания вызываемого экземпляра `IFilterMetadata`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-794">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="b1b93-795">Это обеспечивает высокую степень гибкости, так как при запуске приложения нет необходимости в явном и точном определении конвейера фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-795">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="b1b93-796">Еще один подход к созданию фильтров заключается в реализации `IFilterFactory` с помощью настраиваемых атрибутов:</span><span class="sxs-lookup"><span data-stu-id="b1b93-796">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="b1b93-797">Приведенный выше код можно проверить, выполнив [скачиваемый пример](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="b1b93-797">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="b1b93-798">Вызовите средства для разработчика (F12).</span><span class="sxs-lookup"><span data-stu-id="b1b93-798">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="b1b93-799">Перейдите к `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-799">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="b1b93-800">В средствах для разработчика (F12) отобразятся следующие заголовки ответа, добавленные примером кода:</span><span class="sxs-lookup"><span data-stu-id="b1b93-800">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="b1b93-801">**author:** `Joe Smith`;</span><span class="sxs-lookup"><span data-stu-id="b1b93-801">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="b1b93-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`;</span><span class="sxs-lookup"><span data-stu-id="b1b93-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="b1b93-803">**internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-803">**internal:** `My header`</span></span>

<span data-ttu-id="b1b93-804">В приведенном выше коде создается заголовок ответа **internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-804">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="b1b93-805">Реализация IFilterFactory в атрибуте</span><span class="sxs-lookup"><span data-stu-id="b1b93-805">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="b1b93-806">Фильтры, реализующие `IFilterFactory`, используются для фильтров, которые:</span><span class="sxs-lookup"><span data-stu-id="b1b93-806">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="b1b93-807">Не требуют передачи параметров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-807">Don't require passing parameters.</span></span>
* <span data-ttu-id="b1b93-808">Содержат зависимости конструктора, которые должны заполняться путем внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b1b93-808">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="b1b93-809">Объект <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> реализует интерфейс <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="b1b93-810">`IFilterFactory` предоставляет метод <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> для создания экземпляра <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="b1b93-810">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="b1b93-811">`CreateInstance` загружает указанный тип из контейнера служб (внедрение зависимостей).</span><span class="sxs-lookup"><span data-stu-id="b1b93-811">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="b1b93-812">В коде ниже приведено три примера применения `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="b1b93-812">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="b1b93-813">В приведенном выше коде декорирование метода с использованием `[SampleActionFilter]` является рекомендуемым способом применения `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="b1b93-813">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="b1b93-814">Использование ПО промежуточного слоя в конвейере фильтров</span><span class="sxs-lookup"><span data-stu-id="b1b93-814">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="b1b93-815">Фильтры ресурсов по принципу работы похожи на [ПО промежуточного слоя](xref:fundamentals/middleware/index) тем, что они заключают в себя выполнение всех объектов, находящихся далее в конвейере.</span><span class="sxs-lookup"><span data-stu-id="b1b93-815">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="b1b93-816">При этом фильтры отличаются от ПО промежуточного слоя тем, что они являются частью среды выполнения ASP.NET Core, то есть они имеют доступ к контексту и конструкциям ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1b93-816">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="b1b93-817">Чтобы использовать ПО промежуточного слоя в качестве фильтра, создайте тип с методом `Configure`, определяющим ПО промежуточного слоя, которое нужно внедрить в конвейер фильтров.</span><span class="sxs-lookup"><span data-stu-id="b1b93-817">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="b1b93-818">В примере ниже ПО промежуточного слоя локализации применяется для определения текущих языка и региональных параметров для запроса:</span><span class="sxs-lookup"><span data-stu-id="b1b93-818">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="b1b93-819">Используйте <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> для выполнения ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="b1b93-819">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="b1b93-820">Фильтры ПО промежуточного слоя выполняются на том же этапе конвейера фильтров, что и фильтры ресурсов, перед привязкой модели и после остальной части конвейера.</span><span class="sxs-lookup"><span data-stu-id="b1b93-820">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="b1b93-821">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="b1b93-821">Next actions</span></span>

* <span data-ttu-id="b1b93-822">Дополнительные сведения см. в статье [Filter methods for Razor Pages in ASP.NET Core](xref:razor-pages/filter) (Методы фильтрации для Razor Pages в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="b1b93-822">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="b1b93-823">Чтобы поэкспериментировать с фильтрами, [скачайте, протестируйте и измените пример с GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="b1b93-823">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
