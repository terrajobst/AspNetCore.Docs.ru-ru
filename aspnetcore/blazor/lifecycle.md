---
title: Жизненный цикл Blazor ASP.NET Core
author: guardrex
description: Узнайте, как использовать методы жизненного цикла компонента Razor в ASP.NET Core Blazor приложениях.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: ecacd0a9728cbefd716e9dc7cd8a8c62f3df6e0d
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213393"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="4e421-103">Жизненный цикл Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e421-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="4e421-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="4e421-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="4e421-105">Платформа Blazor содержит синхронные и асинхронные методы жизненного цикла.</span><span class="sxs-lookup"><span data-stu-id="4e421-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="4e421-106">Переопределяйте методы жизненного цикла для выполнения дополнительных операций с компонентами во время инициализации и отрисовки компонента.</span><span class="sxs-lookup"><span data-stu-id="4e421-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="4e421-107">Методы жизненного цикла</span><span class="sxs-lookup"><span data-stu-id="4e421-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="4e421-108">Методы инициализации компонентов</span><span class="sxs-lookup"><span data-stu-id="4e421-108">Component initialization methods</span></span>

<span data-ttu-id="4e421-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> и <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> вызываются, когда компонент инициализируется после получения начальных параметров от родительского компонента.</span><span class="sxs-lookup"><span data-stu-id="4e421-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="4e421-110">Используйте `OnInitializedAsync`, когда компонент выполняет асинхронную операцию и должен обновляться после завершения операции.</span><span class="sxs-lookup"><span data-stu-id="4e421-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span>

<span data-ttu-id="4e421-111">Для синхронной операции Переопределите `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="4e421-111">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="4e421-112">Чтобы выполнить асинхронную операцию, переопределите `OnInitializedAsync` и используйте в операции ключевое слово `await`:</span><span class="sxs-lookup"><span data-stu-id="4e421-112">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor<span data-ttu-id="4e421-113"> серверных приложений, которые [выправляют их вызов содержимого](xref:blazor/hosting-model-configuration#render-mode) `OnInitializedAsync` **_дважды_** :</span><span class="sxs-lookup"><span data-stu-id="4e421-113"> Server apps that [prerender their content](xref:blazor/hosting-model-configuration#render-mode) call `OnInitializedAsync` **_twice_**:</span></span>

* <span data-ttu-id="4e421-114">Один раз, когда компонент изначально отрисовывается статически как часть страницы.</span><span class="sxs-lookup"><span data-stu-id="4e421-114">Once when the component is initially rendered statically as part of the page.</span></span>
* <span data-ttu-id="4e421-115">Второй раз, когда браузер устанавливает соединение с сервером.</span><span class="sxs-lookup"><span data-stu-id="4e421-115">A second time when the browser establishes a connection back to the server.</span></span>

<span data-ttu-id="4e421-116">Чтобы предотвратить выполнение кода разработчика в `OnInitializedAsync` дважды, см. раздел повторное [подключение с отслеживанием состояния после подготовки к просмотру](#stateful-reconnection-after-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="4e421-116">To prevent developer code in `OnInitializedAsync` from running twice, see the [Stateful reconnection after prerendering](#stateful-reconnection-after-prerendering) section.</span></span>

<span data-ttu-id="4e421-117">Во время предварительной отрисовки Blazor серверного приложения некоторые действия, такие как вызов JavaScript, не возможны, так как соединение с браузером не установлено.</span><span class="sxs-lookup"><span data-stu-id="4e421-117">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="4e421-118">При предварительной отрисовке компонентов может потребоваться прорисовка по-разному.</span><span class="sxs-lookup"><span data-stu-id="4e421-118">Components may need to render differently when prerendered.</span></span> <span data-ttu-id="4e421-119">Дополнительные сведения см. в разделе [Обнаружение времени подготовки приложения к просмотру](#detect-when-the-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="4e421-119">For more information, see the [Detect when the app is prerendering](#detect-when-the-app-is-prerendering) section.</span></span>

### <a name="before-parameters-are-set"></a><span data-ttu-id="4e421-120">Перед установкой параметров</span><span class="sxs-lookup"><span data-stu-id="4e421-120">Before parameters are set</span></span>

<span data-ttu-id="4e421-121"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> задает параметры, предоставляемые родительским компонентом компонента в дереве визуализации:</span><span class="sxs-lookup"><span data-stu-id="4e421-121"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="4e421-122"><xref:Microsoft.AspNetCore.Components.ParameterView> содержит весь набор значений параметров при каждом вызове `SetParametersAsync`.</span><span class="sxs-lookup"><span data-stu-id="4e421-122"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="4e421-123">Реализация `SetParametersAsync` по умолчанию задает значение каждого свойства с атрибутом `[Parameter]` или `[CascadingParameter]`, имеющим соответствующее значение в `ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="4e421-123">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="4e421-124">Параметры, у которых нет соответствующего значения в `ParameterView`, остаются неизменными.</span><span class="sxs-lookup"><span data-stu-id="4e421-124">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="4e421-125">Если `base.SetParametersAync` не вызывается, Пользовательский код может интерпретировать значение входящих параметров любым необходимым образом.</span><span class="sxs-lookup"><span data-stu-id="4e421-125">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="4e421-126">Например, нет необходимости назначать входящие параметры свойствам класса.</span><span class="sxs-lookup"><span data-stu-id="4e421-126">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="4e421-127">После установки параметров</span><span class="sxs-lookup"><span data-stu-id="4e421-127">After parameters are set</span></span>

<span data-ttu-id="4e421-128">вызываются <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> и <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>:</span><span class="sxs-lookup"><span data-stu-id="4e421-128"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="4e421-129">Когда компонент инициализирован и получил первый набор параметров от родительского компонента.</span><span class="sxs-lookup"><span data-stu-id="4e421-129">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="4e421-130">Когда родительский компонент повторно готовится к просмотру и предоставляет:</span><span class="sxs-lookup"><span data-stu-id="4e421-130">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="4e421-131">Только известные примитивные неизменяемые типы, которые изменились по крайней мере с одним параметром.</span><span class="sxs-lookup"><span data-stu-id="4e421-131">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="4e421-132">Любые параметры со сложной типизацией.</span><span class="sxs-lookup"><span data-stu-id="4e421-132">Any complex-typed parameters.</span></span> <span data-ttu-id="4e421-133">Платформа не может определить, изменяются ли значения сложного типа, поэтому он рассматривает набор параметров как измененный.</span><span class="sxs-lookup"><span data-stu-id="4e421-133">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="4e421-134">Асинхронная работа при применении параметров и значений свойств должна происходить во время события жизненного цикла `OnParametersSetAsync`.</span><span class="sxs-lookup"><span data-stu-id="4e421-134">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="4e421-135">После подготовки к просмотру компонента</span><span class="sxs-lookup"><span data-stu-id="4e421-135">After component render</span></span>

<span data-ttu-id="4e421-136"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> и <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> вызываются после завершения подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="4e421-136"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="4e421-137">В этот момент заполнены ссылки на элементы и компоненты.</span><span class="sxs-lookup"><span data-stu-id="4e421-137">Element and component references are populated at this point.</span></span> <span data-ttu-id="4e421-138">Используйте этот этап для выполнения дополнительных шагов инициализации с помощью готового к просмотру содержимого, например для активации сторонних библиотек JavaScript, которые работают с визуализированными элементами DOM.</span><span class="sxs-lookup"><span data-stu-id="4e421-138">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="4e421-139">Параметр `firstRender` для `OnAfterRenderAsync` и `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="4e421-139">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="4e421-140">Устанавливается в значение `true` при первом отображении экземпляра компонента.</span><span class="sxs-lookup"><span data-stu-id="4e421-140">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="4e421-141">Можно использовать, чтобы гарантировать, что работа по инициализации выполняется только один раз.</span><span class="sxs-lookup"><span data-stu-id="4e421-141">Can be used to ensure that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="4e421-142">Асинхронная работа сразу после отрисовки должна произойти во время события жизненного цикла `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="4e421-142">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="4e421-143">Даже если вы возвращаете <xref:System.Threading.Tasks.Task> из `OnAfterRenderAsync`, платформа не планирует дальнейший цикл рендеринга компонента после завершения задачи.</span><span class="sxs-lookup"><span data-stu-id="4e421-143">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="4e421-144">Это позволяет избежать бесконечного цикла отрисовки.</span><span class="sxs-lookup"><span data-stu-id="4e421-144">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="4e421-145">Он отличается от других методов жизненного цикла, которые запланируют дальнейший цикл рендеринга после завершения возвращаемой задачи.</span><span class="sxs-lookup"><span data-stu-id="4e421-145">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="4e421-146">`OnAfterRender` и `OnAfterRenderAsync` *не вызываются при предварительной отрисовке на сервере.*</span><span class="sxs-lookup"><span data-stu-id="4e421-146">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="4e421-147">Подавлять обновление пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="4e421-147">Suppress UI refreshing</span></span>

<span data-ttu-id="4e421-148">Переопределите <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*>, чтобы отключить обновление пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="4e421-148">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="4e421-149">Если реализация возвращает `true`, Пользовательский интерфейс обновляется:</span><span class="sxs-lookup"><span data-stu-id="4e421-149">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="4e421-150">`ShouldRender` вызывается каждый раз при подготовке к просмотру компонента.</span><span class="sxs-lookup"><span data-stu-id="4e421-150">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="4e421-151">Даже при переопределении `ShouldRender` компонент всегда первоначально готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="4e421-151">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="4e421-152">Изменения состояний</span><span class="sxs-lookup"><span data-stu-id="4e421-152">State changes</span></span>

<span data-ttu-id="4e421-153"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> уведомляет компонент о том, что его состояние изменилось.</span><span class="sxs-lookup"><span data-stu-id="4e421-153"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="4e421-154">Если применимо, вызов `StateHasChanged` вызывает переподготовку компонента.</span><span class="sxs-lookup"><span data-stu-id="4e421-154">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="4e421-155">Обработка незавершенных асинхронных действий при отрисовке</span><span class="sxs-lookup"><span data-stu-id="4e421-155">Handle incomplete async actions at render</span></span>

<span data-ttu-id="4e421-156">Асинхронные действия, выполняемые в событиях жизненного цикла, могут не завершиться до подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="4e421-156">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="4e421-157">Во время выполнения метода жизненного цикла объекты могут быть `null`ы или заполнены незаполненными данными.</span><span class="sxs-lookup"><span data-stu-id="4e421-157">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="4e421-158">Предоставьте логику отрисовки для подтверждения инициализации объектов.</span><span class="sxs-lookup"><span data-stu-id="4e421-158">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="4e421-159">Отрисовывает элементы пользовательского интерфейса заполнителя (например, сообщение загрузки), пока объекты `null`.</span><span class="sxs-lookup"><span data-stu-id="4e421-159">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="4e421-160">В компоненте `FetchData` шаблонов Blazor `OnInitializedAsync` переопределяется в асичронаусли получения данных прогноза (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="4e421-160">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="4e421-161">Если `forecasts` `null`, пользователю выводится сообщение о загрузке.</span><span class="sxs-lookup"><span data-stu-id="4e421-161">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="4e421-162">После завершения `Task`, возвращенного `OnInitializedAsync`, компонент перерисовывается с обновленным состоянием.</span><span class="sxs-lookup"><span data-stu-id="4e421-162">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="4e421-163">*Pages/FetchData. Razor* в шаблоне сервера Blazor:</span><span class="sxs-lookup"><span data-stu-id="4e421-163">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="4e421-164">Освобождение компонентов с помощью IDisposable</span><span class="sxs-lookup"><span data-stu-id="4e421-164">Component disposal with IDisposable</span></span>

<span data-ttu-id="4e421-165">Если компонент реализует <xref:System.IDisposable>, [метод Dispose](/dotnet/standard/garbage-collection/implementing-dispose) вызывается при удалении компонента из пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="4e421-165">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="4e421-166">Следующий компонент использует `@implements IDisposable` и метод `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="4e421-166">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```razor
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="4e421-167">Вызов <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> в `Dispose` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4e421-167">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="4e421-168">`StateHasChanged` могут быть вызваны при разрыве модуля подготовки отчетов, поэтому запрос обновлений пользовательского интерфейса на этом этапе не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4e421-168">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="4e421-169">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="4e421-169">Handle errors</span></span>

<span data-ttu-id="4e421-170">Сведения об обработке ошибок во время выполнения метода жизненного цикла см. в разделе <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="4e421-170">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>

## <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="4e421-171">Повторное подключение с отслеживанием состояния после предварительной подготовки к просмотру</span><span class="sxs-lookup"><span data-stu-id="4e421-171">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="4e421-172">В приложении Blazor Server, когда `RenderMode` `ServerPrerendered`, компонент изначально отрисовывается статически как часть страницы.</span><span class="sxs-lookup"><span data-stu-id="4e421-172">In a Blazor Server app when `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="4e421-173">После того, как браузер установит соединение с сервером, компонент *снова*готовится к просмотру, и теперь компонент является интерактивным.</span><span class="sxs-lookup"><span data-stu-id="4e421-173">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="4e421-174">Если имеется метод жизненно инициализированного метода жизненный цикл [{Async}](xref:blazor/lifecycle#component-initialization-methods) для инициализации компонента, метод выполняется *дважды*:</span><span class="sxs-lookup"><span data-stu-id="4e421-174">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="4e421-175">Когда компонент предварительно отображается статически.</span><span class="sxs-lookup"><span data-stu-id="4e421-175">When the component is prerendered statically.</span></span>
* <span data-ttu-id="4e421-176">После установки соединения с сервером.</span><span class="sxs-lookup"><span data-stu-id="4e421-176">After the server connection has been established.</span></span>

<span data-ttu-id="4e421-177">Это может привести к заметному изменению данных, отображаемых в пользовательском интерфейсе, когда компонент в итоге готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="4e421-177">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="4e421-178">Чтобы избежать сценария двойной визуализации в приложении Blazor Server, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4e421-178">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="4e421-179">Передайте идентификатор, который можно использовать для кэширования состояния во время подготовки к просмотру, и для получения состояния после перезапуска приложения.</span><span class="sxs-lookup"><span data-stu-id="4e421-179">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="4e421-180">Используйте идентификатор во время подготовки к просмотру для сохранения состояния компонента.</span><span class="sxs-lookup"><span data-stu-id="4e421-180">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="4e421-181">Используйте идентификатор после предварительной подготовки для получения кэшированного состояния.</span><span class="sxs-lookup"><span data-stu-id="4e421-181">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="4e421-182">В следующем коде показано обновленное `WeatherForecastService` в серверном приложении Blazor на основе шаблонов, которое позволяет избежать двойной отрисовки:</span><span class="sxs-lookup"><span data-stu-id="4e421-182">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

<span data-ttu-id="4e421-183">Дополнительные сведения о `RenderMode`см. в разделе <xref:blazor/hosting-model-configuration#render-mode>.</span><span class="sxs-lookup"><span data-stu-id="4e421-183">For more information on the `RenderMode`, see <xref:blazor/hosting-model-configuration#render-mode>.</span></span>

## <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="4e421-184">Обнаруживать время подготовки приложения к просмотру</span><span class="sxs-lookup"><span data-stu-id="4e421-184">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]
