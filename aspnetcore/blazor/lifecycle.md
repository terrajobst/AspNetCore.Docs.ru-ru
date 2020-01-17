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
ms.openlocfilehash: df5bb676df59b538179a69978040521c4ee78ed1
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146372"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="1fc8a-103">Жизненный цикл Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1fc8a-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="1fc8a-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1fc8a-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="1fc8a-105">Платформа Blazor содержит синхронные и асинхронные методы жизненного цикла.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="1fc8a-106">Переопределяйте методы жизненного цикла для выполнения дополнительных операций с компонентами во время инициализации и отрисовки компонента.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="1fc8a-107">Методы жизненного цикла</span><span class="sxs-lookup"><span data-stu-id="1fc8a-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="1fc8a-108">Методы инициализации компонентов</span><span class="sxs-lookup"><span data-stu-id="1fc8a-108">Component initialization methods</span></span>

<span data-ttu-id="1fc8a-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> и <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> вызываются, когда компонент инициализируется после получения начальных параметров от родительского компонента.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="1fc8a-110">Используйте `OnInitializedAsync`, когда компонент выполняет асинхронную операцию и должен обновляться после завершения операции.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span> <span data-ttu-id="1fc8a-111">Эти методы вызываются только один раз при первом создании экземпляра компонента.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-111">These methods are only called one time when the component is first instantiated.</span></span>

<span data-ttu-id="1fc8a-112">Для синхронной операции Переопределите `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="1fc8a-112">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="1fc8a-113">Чтобы выполнить асинхронную операцию, переопределите `OnInitializedAsync` и используйте в операции ключевое слово `await`:</span><span class="sxs-lookup"><span data-stu-id="1fc8a-113">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

### <a name="before-parameters-are-set"></a><span data-ttu-id="1fc8a-114">Перед установкой параметров</span><span class="sxs-lookup"><span data-stu-id="1fc8a-114">Before parameters are set</span></span>

<span data-ttu-id="1fc8a-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> задает параметры, предоставляемые родительским компонентом компонента в дереве визуализации:</span><span class="sxs-lookup"><span data-stu-id="1fc8a-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="1fc8a-116"><xref:Microsoft.AspNetCore.Components.ParameterView> содержит весь набор значений параметров при каждом вызове `SetParametersAsync`.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="1fc8a-117">Реализация `SetParametersAsync` по умолчанию задает значение каждого свойства с атрибутом `[Parameter]` или `[CascadingParameter]`, имеющим соответствующее значение в `ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-117">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="1fc8a-118">Параметры, у которых нет соответствующего значения в `ParameterView`, остаются неизменными.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-118">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="1fc8a-119">Если `base.SetParametersAync` не вызывается, Пользовательский код может интерпретировать значение входящих параметров любым необходимым образом.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-119">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="1fc8a-120">Например, нет необходимости назначать входящие параметры свойствам класса.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-120">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="1fc8a-121">После установки параметров</span><span class="sxs-lookup"><span data-stu-id="1fc8a-121">After parameters are set</span></span>

<span data-ttu-id="1fc8a-122">вызываются <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> и <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>:</span><span class="sxs-lookup"><span data-stu-id="1fc8a-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="1fc8a-123">Когда компонент инициализирован и получил первый набор параметров от родительского компонента.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-123">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="1fc8a-124">Когда родительский компонент повторно готовится к просмотру и предоставляет:</span><span class="sxs-lookup"><span data-stu-id="1fc8a-124">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="1fc8a-125">Только известные примитивные неизменяемые типы, которые изменились по крайней мере с одним параметром.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-125">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="1fc8a-126">Любые параметры со сложной типизацией.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-126">Any complex-typed parameters.</span></span> <span data-ttu-id="1fc8a-127">Платформа не может определить, изменяются ли значения сложного типа, поэтому он рассматривает набор параметров как измененный.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-127">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="1fc8a-128">Асинхронная работа при применении параметров и значений свойств должна происходить во время события жизненного цикла `OnParametersSetAsync`.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-128">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="1fc8a-129">После подготовки к просмотру компонента</span><span class="sxs-lookup"><span data-stu-id="1fc8a-129">After component render</span></span>

<span data-ttu-id="1fc8a-130"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> и <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> вызываются после завершения подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-130"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="1fc8a-131">В этот момент заполнены ссылки на элементы и компоненты.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-131">Element and component references are populated at this point.</span></span> <span data-ttu-id="1fc8a-132">Используйте этот этап для выполнения дополнительных шагов инициализации с помощью готового к просмотру содержимого, например для активации сторонних библиотек JavaScript, которые работают с визуализированными элементами DOM.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-132">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="1fc8a-133">Параметр `firstRender` для `OnAfterRenderAsync` и `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="1fc8a-133">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="1fc8a-134">Устанавливается в значение `true` при первом отображении экземпляра компонента.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-134">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="1fc8a-135">Можно использовать, чтобы гарантировать, что работа по инициализации выполняется только один раз.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-135">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="1fc8a-136">Асинхронная работа сразу после отрисовки должна произойти во время события жизненного цикла `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-136">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="1fc8a-137">Даже если вы возвращаете <xref:System.Threading.Tasks.Task> из `OnAfterRenderAsync`, платформа не планирует дальнейший цикл рендеринга компонента после завершения задачи.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-137">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="1fc8a-138">Это позволяет избежать бесконечного цикла отрисовки.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-138">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="1fc8a-139">Он отличается от других методов жизненного цикла, которые запланируют дальнейший цикл рендеринга после завершения возвращаемой задачи.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-139">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="1fc8a-140">`OnAfterRender` и `OnAfterRenderAsync` *не вызываются при предварительной отрисовке на сервере.*</span><span class="sxs-lookup"><span data-stu-id="1fc8a-140">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="1fc8a-141">Подавлять обновление пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="1fc8a-141">Suppress UI refreshing</span></span>

<span data-ttu-id="1fc8a-142">Переопределите <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*>, чтобы отключить обновление пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-142">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="1fc8a-143">Если реализация возвращает `true`, Пользовательский интерфейс обновляется:</span><span class="sxs-lookup"><span data-stu-id="1fc8a-143">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="1fc8a-144">`ShouldRender` вызывается каждый раз при подготовке к просмотру компонента.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-144">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="1fc8a-145">Даже при переопределении `ShouldRender` компонент всегда первоначально готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-145">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="1fc8a-146">Изменения состояния</span><span class="sxs-lookup"><span data-stu-id="1fc8a-146">State changes</span></span>

<span data-ttu-id="1fc8a-147"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> уведомляет компонент о том, что его состояние изменилось.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-147"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="1fc8a-148">Если применимо, вызов `StateHasChanged` вызывает переподготовку компонента.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-148">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="1fc8a-149">Обработка незавершенных асинхронных действий при отрисовке</span><span class="sxs-lookup"><span data-stu-id="1fc8a-149">Handle incomplete async actions at render</span></span>

<span data-ttu-id="1fc8a-150">Асинхронные действия, выполняемые в событиях жизненного цикла, могут не завершиться до подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-150">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="1fc8a-151">Во время выполнения метода жизненного цикла объекты могут быть `null`ы или заполнены незаполненными данными.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-151">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="1fc8a-152">Предоставьте логику отрисовки для подтверждения инициализации объектов.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-152">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="1fc8a-153">Отрисовывает элементы пользовательского интерфейса заполнителя (например, сообщение загрузки), пока объекты `null`.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-153">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="1fc8a-154">В компоненте `FetchData` шаблонов Blazor `OnInitializedAsync` переопределяется в асичронаусли получения данных прогноза (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="1fc8a-154">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="1fc8a-155">Если `forecasts` `null`, пользователю выводится сообщение о загрузке.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-155">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="1fc8a-156">После завершения `Task`, возвращенного `OnInitializedAsync`, компонент перерисовывается с обновленным состоянием.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-156">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="1fc8a-157">*Pages/FetchData. Razor* в шаблоне сервера Blazor:</span><span class="sxs-lookup"><span data-stu-id="1fc8a-157">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="1fc8a-158">Освобождение компонентов с помощью IDisposable</span><span class="sxs-lookup"><span data-stu-id="1fc8a-158">Component disposal with IDisposable</span></span>

<span data-ttu-id="1fc8a-159">Если компонент реализует <xref:System.IDisposable>, [метод Dispose](/dotnet/standard/garbage-collection/implementing-dispose) вызывается при удалении компонента из пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-159">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="1fc8a-160">Следующий компонент использует `@implements IDisposable` и метод `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="1fc8a-160">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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
> <span data-ttu-id="1fc8a-161">Вызов <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> в `Dispose` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-161">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="1fc8a-162">`StateHasChanged` могут быть вызваны при разрыве модуля подготовки отчетов, поэтому запрос обновлений пользовательского интерфейса на этом этапе не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-162">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="1fc8a-163">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="1fc8a-163">Handle errors</span></span>

<span data-ttu-id="1fc8a-164">Сведения об обработке ошибок во время выполнения метода жизненного цикла см. в разделе <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="1fc8a-164">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>
