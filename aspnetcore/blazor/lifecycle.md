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
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>Жизненный цикл Blazor ASP.NET Core

[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)

Платформа Blazor содержит синхронные и асинхронные методы жизненного цикла. Переопределяйте методы жизненного цикла для выполнения дополнительных операций с компонентами во время инициализации и отрисовки компонента.

## <a name="lifecycle-methods"></a>Методы жизненного цикла

### <a name="component-initialization-methods"></a>Методы инициализации компонентов

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> и <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> вызываются, когда компонент инициализируется после получения начальных параметров от родительского компонента. Используйте `OnInitializedAsync`, когда компонент выполняет асинхронную операцию и должен обновляться после завершения операции.

Для синхронной операции Переопределите `OnInitialized`:

```csharp
protected override void OnInitialized()
{
    ...
}
```

Чтобы выполнить асинхронную операцию, переопределите `OnInitializedAsync` и используйте в операции ключевое слово `await`:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor серверных приложений, которые [выправляют их вызов содержимого](xref:blazor/hosting-model-configuration#render-mode) `OnInitializedAsync` **_дважды_** :

* Один раз, когда компонент изначально отрисовывается статически как часть страницы.
* Второй раз, когда браузер устанавливает соединение с сервером.

Чтобы предотвратить выполнение кода разработчика в `OnInitializedAsync` дважды, см. раздел повторное [подключение с отслеживанием состояния после подготовки к просмотру](#stateful-reconnection-after-prerendering) .

Во время предварительной отрисовки Blazor серверного приложения некоторые действия, такие как вызов JavaScript, не возможны, так как соединение с браузером не установлено. При предварительной отрисовке компонентов может потребоваться прорисовка по-разному. Дополнительные сведения см. в разделе [Обнаружение времени подготовки приложения к просмотру](#detect-when-the-app-is-prerendering) .

### <a name="before-parameters-are-set"></a>Перед установкой параметров

<xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> задает параметры, предоставляемые родительским компонентом компонента в дереве визуализации:

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<xref:Microsoft.AspNetCore.Components.ParameterView> содержит весь набор значений параметров при каждом вызове `SetParametersAsync`.

Реализация `SetParametersAsync` по умолчанию задает значение каждого свойства с атрибутом `[Parameter]` или `[CascadingParameter]`, имеющим соответствующее значение в `ParameterView`. Параметры, у которых нет соответствующего значения в `ParameterView`, остаются неизменными.

Если `base.SetParametersAync` не вызывается, Пользовательский код может интерпретировать значение входящих параметров любым необходимым образом. Например, нет необходимости назначать входящие параметры свойствам класса.

### <a name="after-parameters-are-set"></a>После установки параметров

вызываются <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> и <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>:

* Когда компонент инициализирован и получил первый набор параметров от родительского компонента.
* Когда родительский компонент повторно готовится к просмотру и предоставляет:
  * Только известные примитивные неизменяемые типы, которые изменились по крайней мере с одним параметром.
  * Любые параметры со сложной типизацией. Платформа не может определить, изменяются ли значения сложного типа, поэтому он рассматривает набор параметров как измененный.

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> Асинхронная работа при применении параметров и значений свойств должна происходить во время события жизненного цикла `OnParametersSetAsync`.

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a>После подготовки к просмотру компонента

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> и <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> вызываются после завершения подготовки компонента к просмотру. В этот момент заполнены ссылки на элементы и компоненты. Используйте этот этап для выполнения дополнительных шагов инициализации с помощью готового к просмотру содержимого, например для активации сторонних библиотек JavaScript, которые работают с визуализированными элементами DOM.

Параметр `firstRender` для `OnAfterRenderAsync` и `OnAfterRender`:

* Устанавливается в значение `true` при первом отображении экземпляра компонента.
* Можно использовать, чтобы гарантировать, что работа по инициализации выполняется только один раз.

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
> Асинхронная работа сразу после отрисовки должна произойти во время события жизненного цикла `OnAfterRenderAsync`.
>
> Даже если вы возвращаете <xref:System.Threading.Tasks.Task> из `OnAfterRenderAsync`, платформа не планирует дальнейший цикл рендеринга компонента после завершения задачи. Это позволяет избежать бесконечного цикла отрисовки. Он отличается от других методов жизненного цикла, которые запланируют дальнейший цикл рендеринга после завершения возвращаемой задачи.

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

`OnAfterRender` и `OnAfterRenderAsync` *не вызываются при предварительной отрисовке на сервере.*

### <a name="suppress-ui-refreshing"></a>Подавлять обновление пользовательского интерфейса

Переопределите <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*>, чтобы отключить обновление пользовательского интерфейса. Если реализация возвращает `true`, Пользовательский интерфейс обновляется:

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

`ShouldRender` вызывается каждый раз при подготовке к просмотру компонента.

Даже при переопределении `ShouldRender` компонент всегда первоначально готовится к просмотру.

## <a name="state-changes"></a>Изменения состояний

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> уведомляет компонент о том, что его состояние изменилось. Если применимо, вызов `StateHasChanged` вызывает переподготовку компонента.

## <a name="handle-incomplete-async-actions-at-render"></a>Обработка незавершенных асинхронных действий при отрисовке

Асинхронные действия, выполняемые в событиях жизненного цикла, могут не завершиться до подготовки компонента к просмотру. Во время выполнения метода жизненного цикла объекты могут быть `null`ы или заполнены незаполненными данными. Предоставьте логику отрисовки для подтверждения инициализации объектов. Отрисовывает элементы пользовательского интерфейса заполнителя (например, сообщение загрузки), пока объекты `null`.

В компоненте `FetchData` шаблонов Blazor `OnInitializedAsync` переопределяется в асичронаусли получения данных прогноза (`forecasts`). Если `forecasts` `null`, пользователю выводится сообщение о загрузке. После завершения `Task`, возвращенного `OnInitializedAsync`, компонент перерисовывается с обновленным состоянием.

*Pages/FetchData. Razor* в шаблоне сервера Blazor:

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>Освобождение компонентов с помощью IDisposable

Если компонент реализует <xref:System.IDisposable>, [метод Dispose](/dotnet/standard/garbage-collection/implementing-dispose) вызывается при удалении компонента из пользовательского интерфейса. Следующий компонент использует `@implements IDisposable` и метод `Dispose`:

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
> Вызов <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> в `Dispose` не поддерживается. `StateHasChanged` могут быть вызваны при разрыве модуля подготовки отчетов, поэтому запрос обновлений пользовательского интерфейса на этом этапе не поддерживается.

## <a name="handle-errors"></a>Обработка ошибок

Сведения об обработке ошибок во время выполнения метода жизненного цикла см. в разделе <xref:blazor/handle-errors#lifecycle-methods>.

## <a name="stateful-reconnection-after-prerendering"></a>Повторное подключение с отслеживанием состояния после предварительной подготовки к просмотру

В приложении Blazor Server, когда `RenderMode` `ServerPrerendered`, компонент изначально отрисовывается статически как часть страницы. После того, как браузер установит соединение с сервером, компонент *снова*готовится к просмотру, и теперь компонент является интерактивным. Если имеется метод жизненно инициализированного метода жизненный цикл [{Async}](xref:blazor/lifecycle#component-initialization-methods) для инициализации компонента, метод выполняется *дважды*:

* Когда компонент предварительно отображается статически.
* После установки соединения с сервером.

Это может привести к заметному изменению данных, отображаемых в пользовательском интерфейсе, когда компонент в итоге готовится к просмотру.

Чтобы избежать сценария двойной визуализации в приложении Blazor Server, выполните следующие действия.

* Передайте идентификатор, который можно использовать для кэширования состояния во время подготовки к просмотру, и для получения состояния после перезапуска приложения.
* Используйте идентификатор во время подготовки к просмотру для сохранения состояния компонента.
* Используйте идентификатор после предварительной подготовки для получения кэшированного состояния.

В следующем коде показано обновленное `WeatherForecastService` в серверном приложении Blazor на основе шаблонов, которое позволяет избежать двойной отрисовки:

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

Дополнительные сведения о `RenderMode`см. в разделе <xref:blazor/hosting-model-configuration#render-mode>.

## <a name="detect-when-the-app-is-prerendering"></a>Обнаруживать время подготовки приложения к просмотру

[!INCLUDE[](~/includes/blazor-prerendering.md)]
