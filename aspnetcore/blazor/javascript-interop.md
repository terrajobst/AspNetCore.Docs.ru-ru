---
title: ASP.NET Core Blazor взаимодействия JavaScript
author: guardrex
description: Узнайте, как вызывать функции JavaScript из методов .NET и .NET из JavaScript в Blazor приложениях.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: c4f2444b60fc2d3a8af893df379cf62636a7bdd5
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213367"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>ASP.NET Core Blazor взаимодействия JavaScript

[Хавьер Калварро Воронков](https://github.com/javiercn), [Даниэль Roth)](https://github.com/danroth27)и [Люк ЛаСаМ](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor приложение может вызывать функции JavaScript из кода JavaScript в методах .NET и .NET.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Вызов функций JavaScript из методов .NET

В некоторых случаях для вызова функции JavaScript требуется код .NET. Например, вызов JavaScript может предоставлять приложению возможности браузера или функциональные возможности из библиотеки JavaScript. Этот сценарий называется *совместимостью JavaScript* (*JS Interop*).

Чтобы вызвать JavaScript из .NET, используйте абстракцию `IJSRuntime`. Чтобы выдать вызовы взаимодействия JS, вставьте в компонент абстракцию `IJSRuntime`. Метод `InvokeAsync<T>` принимает идентификатор для функции JavaScript, которую нужно вызвать вместе с любым числом аргументов, сериализуемых в формат JSON. Идентификатор функции задается относительно глобальной области (`window`). Если вы хотите вызвать `window.someScope.someFunction`, идентификатор будет `someScope.someFunction`. Нет необходимости регистрировать функцию перед ее вызовом. Тип возвращаемого значения `T` также должен быть сериализуемым в формат JSON. `T` должен соответствовать типу .NET, который лучше сопоставить с возвращаемым типом JSON.

Для Blazor серверных приложений с включенной предварительной отрисовкой вызов JavaScript невозможен во время первоначальной отрисовки. Вызовы взаимодействия JavaScript должны быть отложены до тех пор, пока не будет установлено соединение с браузером. Дополнительные сведения см. в разделе [Обнаружение времени подготовки Blazor приложения](#detect-when-a-blazor-app-is-prerendering) .

Следующий пример основан на [текстдекодер](https://developer.mozilla.org/docs/Web/API/TextDecoder), экспериментальном декодере на основе JavaScript. В примере показано, как вызвать функцию JavaScript из C# метода. Функция JavaScript принимает массив байтов из C# метода, декодирует массив и возвращает текст компоненту для вывода.

В `<head>` элементе *wwwroot/index.HTML* (Blazor Assembly) или *pages/_Host. cshtml* (Blazor Server) Укажите функцию JavaScript, которая использует `TextDecoder` для декодирования переданного массива и возврата декодированного значения:

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

Код JavaScript, например код, показанный в предыдущем примере, можно также загрузить из файла JavaScript ( *. js*) со ссылкой на файл скрипта:

```html
<script src="exampleJsInterop.js"></script>
```

Следующий компонент:

* Вызывает функцию JavaScript `convertArray`, используя `JSRuntime` при выборе кнопки компонента (**преобразование массива**).
* После вызова функции JavaScript переданный массив преобразуется в строку. Строка возвращается компоненту для вывода.

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a>Использование Ижсрунтиме

Чтобы использовать абстракцию `IJSRuntime`, следует принять любой из следующих подходов:

* Внедрить `IJSRuntime`ую абстракцию в компонент Razor ( *. Razor*):

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  В элементе `<head>` *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server) укажите `handleTickerChanged` функцию JavaScript. Функция вызывается с `IJSRuntime.InvokeVoidAsync` и не возвращает значение:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* Внедрить `IJSRuntime`ую абстракцию в класс (*CS*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  В элементе `<head>` *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server) укажите `handleTickerChanged` функцию JavaScript. Функция вызывается с `JSRuntime.InvokeAsync` и возвращает значение:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* Для динамического создания содержимого с помощью [буилдрендертри](xref:blazor/components#manual-rendertreebuilder-logic)используйте атрибут `[Inject]`:

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

В примере приложения на стороне клиента, прилагаемом к этой теме, для приложения, которое взаимодействует с моделью DOM для получения вводимых пользователем данных и вывода приветственного сообщения, доступны две функции JavaScript.

* `showPrompt` &ndash; выдает запрос на ввод пользователя (имя пользователя) и возвращает имя вызывающему объекту.
* `displayWelcome` &ndash; назначает приветственное сообщение объекту модели DOM с помощью `id` `welcome`.

*wwwroot/ексамплежсинтероп. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Поместите тег `<script>`, который ссылается на файл JavaScript, в файл *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server).

*wwwroot/index.HTML* (Blazorная сборка):

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

*Pages/_Host. cshtml* (Blazor Server):

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

Не размещайте тег `<script>` в файле компонента, так как тег `<script>` нельзя обновлять динамически.

Методы .NET взаимодействует с функциями JavaScript в файле *ексамплежсинтероп. js* путем вызова `IJSRuntime.InvokeAsync<T>`.

Абстракция `IJSRuntime` является асинхронной, что позволяет выполнять сценарии с Blazor Server. Если приложение является Blazor приложением сборки и необходимо синхронно вызвать функцию JavaScript, произведение производных `IJSInProcessRuntime` и вызвать `Invoke<T>`. Рекомендуется, чтобы большинство библиотек взаимодействия JS использовали асинхронные интерфейсы API, чтобы обеспечить доступность библиотек во всех сценариях.

Пример приложения включает компонент для демонстрации взаимодействия с JS. Компонент:

* Получает входные данные пользователя с помощью командной строки JavaScript.
* Возвращает текст компонента для обработки.
* Вызывает вторую функцию JavaScript, которая взаимодействует с моделью DOM для вывода приветственного сообщения.

*Pages/жсинтероп. Razor*:

```razor
@page "/JSInterop"
@using BlazorSample.JsInteropClasses
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<h2>Invoke JavaScript functions from .NET methods</h2>

<button type="button" class="btn btn-primary" @onclick="TriggerJsPrompt">
    Trigger JavaScript Prompt
</button>

<h3 id="welcome" style="color:green;font-style:italic"></h3>

@code {
    public async Task TriggerJsPrompt()
    {
        // showPrompt is implemented in wwwroot/exampleJsInterop.js
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");
        // displayWelcome is implemented in wwwroot/exampleJsInterop.js
        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. Когда `TriggerJsPrompt` выполняется при нажатии кнопки **запроса JavaScript триггера** компонента, вызывается функция JavaScript `showPrompt`, предоставленная в файле *wwwroot/ексамплежсинтероп. js* .
1. Функция `showPrompt` принимает ввод пользователя (имя пользователя), который кодируется в формате HTML и возвращается компоненту. Компонент сохраняет имя пользователя в локальной переменной `name`.
1. Строка, хранящаяся в `name`, включается в приветственное сообщение, которое передается в функцию JavaScript, `displayWelcome`, которая визуализирует приветственное сообщение в тег заголовка.

## <a name="call-a-void-javascript-function"></a>Вызов функции JavaScript void

Функции JavaScript, возвращающие [значение void (0)/воид 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) или [undefine](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) , вызываются с `IJSRuntime.InvokeVoidAsync`.

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a>Обнаружение времени предварительной отрисовки Blazor приложения
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Запись ссылок на элементы

В некоторых сценариях взаимодействия JS требуются ссылки на элементы HTML. Например, библиотеке пользовательского интерфейса может требоваться ссылка на элемент для инициализации, или же может потребоваться вызвать API-интерфейсы, подобные Command, в элементе, например `focus` или `play`.

Запишите ссылки на элементы HTML в компоненте, используя следующий подход:

* Добавьте атрибут `@ref` в элемент HTML.
* Определите поле типа `ElementReference`, имя которого соответствует значению атрибута `@ref`.

В следующем примере показана запись ссылки на элемент `<input>` `username`.

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> Используйте ссылку на элемент только для изменения содержимого пустого элемента, который не взаимодействует с Blazor. Этот сценарий полезен, если сторонний API передает содержимое элементу. Поскольку Blazor не взаимодействует с элементом, существует вероятность конфликта между представлением элемента и DOM в Blazor.
>
> В следующем примере *опасно* изменить содержимое неупорядоченного списка (`ul`), поскольку Blazor взаимодействует с моделью DOM для заполнения элементов списка этого элемента (`<li>`):
>
> ```razor
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> Если при взаимодействии JS изменяется содержимое элемента `MyList` и Blazor пытается применить diff к элементу, различия не будут соответствовать модели DOM.

Что касается кода .NET, `ElementReference` является непрозрачным маркером. *Единственное* , что можно сделать с `ElementReference`, — передать его в код JavaScript через JS-взаимодействие. При этом код на стороне JavaScript получает экземпляр `HTMLElement`, который можно использовать с обычными API DOM.

Например, следующий код определяет метод расширения .NET, который позволяет установить фокус на элемент:

*ексамплежсинтероп. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Чтобы вызвать функцию JavaScript, которая не возвращает значение, используйте `IJSRuntime.InvokeVoidAsync`. Следующий код задает фокус ввода имени пользователя, вызывая предыдущую функцию JavaScript с захваченным `ElementReference`:

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Чтобы использовать метод расширения, создайте статический метод расширения, который получает экземпляр `IJSRuntime`:

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Метод `Focus` вызывается непосредственно для объекта. В следующем примере предполагается, что метод `Focus` доступен из пространства имен `JsInteropClasses`:

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> `username` переменная заполняется только после подготовки компонента к просмотру. Если незаполненный `ElementReference` передается в код JavaScript, код JavaScript получает значение `null`. Для управления ссылками на элементы после завершения отрисовки компонента (для установки начального фокуса на элемент) используйте [методы жизненного цикла компонента онафтеррендерасинк или онафтеррендер](xref:blazor/lifecycle#after-component-render).

При работе с универсальными типами и возвратом значения используйте [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

`GenericMethod` вызывается непосредственно для объекта с типом. В следующем примере предполагается, что `GenericMethod` доступен из пространства имен `JsInteropClasses`:

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a>Ссылочные элементы в разных компонентах

`ElementReference` гарантируется только в методе `OnAfterRender` компонента (а ссылка на элемент является `struct`), поэтому ссылка на элемент не может передаваться между компонентами.

Чтобы родительский компонент мог сделать ссылку на элемент доступным для других компонентов, родительский компонент может:

* Разрешить дочерним компонентам регистрировать обратные вызовы.
* Вызов зарегистрированных обратных вызовов во время события `OnAfterRender` с переданной ссылкой на элемент. Косвенно этот подход позволяет дочерним компонентам взаимодействовать со ссылкой на элемент родителя.

В приведенном ниже примере Blazorной сборки показан этот подход.

В `<head>` *wwwroot/index.HTML*:

```html
<style>
    .red { color: red }
</style>
```

В `<body>` *wwwroot/index.HTML*:

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

*Pages/index. Razor* (родительский компонент):

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

*Pages/index. Razor. CS*:

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool _disposing;
        private IList<IObserver<ElementReference>> _subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference _title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnNext(_title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            _disposing = true;

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            _subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (_disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            _subscriptions.Add(observer);

            return new Subscription(observer, this);
        }

        private class Subscription : IDisposable
        {
            public Subscription(IObserver<ElementReference> observer, Index self)
            {
                Observer = observer;
                Self = self;
            }

            public IObserver<ElementReference> Observer { get; }
            public Index Self { get; }

            public void Dispose()
            {
                Self._subscriptions.Remove(Observer);
            }
        }
    }
}
```

*Shared/сурвэйпромпт. Razor* (дочерний компонент):

```razor
@inject IJSRuntime JS

<div class="alert alert-secondary mt-4" role="alert">
    <span class="oi oi-pencil mr-2" aria-hidden="true"></span>
    <strong>@Title</strong>

    <span class="text-nowrap">
        Please take our
        <a target="_blank" class="font-weight-bold" 
            href="https://go.microsoft.com/fwlink/?linkid=2109206">brief survey</a>
    </span>
    and tell us what you think.
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

*Shared/сурвэйпромпт. Razor. CS*:

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable _subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (_subscription != null)
            {
                _subscription.Dispose();
            }

            _subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            _subscription = null;
        }

        public void OnError(Exception error)
        {
            _subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            _subscription?.Dispose();
        }
    }
}
```

## <a name="invoke-net-methods-from-javascript-functions"></a>Вызов методов .NET из функций JavaScript

### <a name="static-net-method-call"></a>Статический вызов метода .NET

Чтобы вызвать статический метод .NET из JavaScript, используйте функции `DotNet.invokeMethod` или `DotNet.invokeMethodAsync`. Передайте идентификатор статического метода, который необходимо вызвать, имя сборки, содержащей функцию, и любые аргументы. Асинхронная версия является предпочтительной для поддержки сценариев Blazor Server. Метод .NET должен быть открытым, статическим и иметь атрибут `[JSInvokable]`. Вызов открытых универсальных методов в настоящее время не поддерживается.

Пример приложения включает C# метод для возврата массива `int`s. К методу применяется атрибут `JSInvokable`.

*Pages/жсинтероп. Razor*:

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

JavaScript, предоставляемый клиенту, вызывает метод C# .NET.

*wwwroot/ексамплежсинтероп. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

При выборе переключателя **статический метод ретурнаррайасинк .NET** изучите выходные данные консоли в средствах веб-разработчика браузера.

Выходные данные консоли:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Четвертое значение массива помещается в массив (`data.push(4);`), возвращаемый `ReturnArrayAsync`.

По умолчанию идентификатором метода является имя метода, но можно указать другой идентификатор с помощью конструктора `JSInvokableAttribute`:

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

В файле JavaScript на стороне клиента:

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a>Вызов метода экземпляра

Кроме того, можно вызывать методы экземпляра .NET из JavaScript. Вызов метода экземпляра .NET из JavaScript:

* Передайте экземпляр .NET в JavaScript, заключив его в экземпляр `DotNetObjectReference`. Экземпляр .NET передается по ссылке на JavaScript.
* Вызывайте методы экземпляра .NET в экземпляре с помощью функций `invokeMethod` или `invokeMethodAsync`. Экземпляр .NET можно также передать в качестве аргумента при вызове других методов .NET из JavaScript.

> [!NOTE]
> Пример приложения записывает сообщения в консоль на стороне клиента. В следующих примерах, продемонстрированных в примере приложения, изучите выходные данные консоли браузера в средствах разработчика браузера.

Когда выбран **метод триггера экземпляра .NET хеллохелпер. SayHello** , `ExampleJsInterop.CallHelloHelperSayHello` вызывается и передает имя, `Blazor`, в метод.

*Pages/жсинтероп. Razor*:

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

`CallHelloHelperSayHello` вызывает функцию JavaScript `sayHello` с новым экземпляром `HelloHelper`.

*Жсинтеропклассес/ексамплежсинтероп. CS*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/ексамплежсинтероп. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Имя передается в конструктор `HelloHelper`, который задает свойство `HelloHelper.Name`. При выполнении `sayHello` функции JavaScript `HelloHelper.SayHello` возвращает `Hello, {Name}!` сообщение, которое записывается в консоль функцией JavaScript.

*Жсинтеропклассес/хеллохелпер. CS*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Выходные данные консоли в средствах веб-разработчика браузера:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Совместное использование кода взаимодействия в библиотеке классов

Код взаимодействия JS может быть добавлен в библиотеку классов, которая позволяет совместно использовать код в пакете NuGet.

Библиотека классов обрабатывает внедрение ресурсов JavaScript в построенную сборку. Файлы JavaScript помещаются в папку *wwwroot* . Инструментарий выполняет внедрение ресурсов при построении библиотеки.

В файле проекта приложения имеется ссылка на созданный пакет NuGet так же, как и на любой пакет NuGet. После восстановления пакета код приложения может вызывать JavaScript, как если бы он был C#.

Дополнительные сведения см. в разделе <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Вызовы взаимодействия с зафиксированным JS

Взаимодействие с JS может завершиться сбоем из-за ошибок сети и должно рассматриваться как ненадежное. По умолчанию серверное приложение Blazor истекает из JS-вызовов взаимодействия на сервере через одну минуту. Если приложение может допускать более агрессивное время ожидания, например 10 секунд, задайте время ожидания одним из следующих способов.

* Глобально в `Startup.ConfigureServices`укажите время ожидания:

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* Для каждого вызова в коде компонента один вызов может указать время ожидания:

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

Дополнительные сведения об исчерпании ресурсов см. в разделе <xref:security/blazor/server>.

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a>Выполнение больших объемов передачи данных в приложениях Blazor Server

В некоторых сценариях необходимо передавать большие объемы данных между JavaScript и Blazor. Как правило, передача больших данных происходит в следующих случаях:

* API-интерфейсы файловой системы браузера используются для отправки или загрузки файла.
* Требуется взаимодействие с библиотекой стороннего разработчика.

В Blazor Server есть ограничение, предотвращающее передачу одного большого количества сообщений, что может привести к проблемам с производительностью.

При разработке кода, который передает данные между JavaScript и Blazor, учитывайте следующие рекомендации.

* Разбивает данные на небольшие части и отправляйте сегменты данных последовательно, пока все данные не будут получены сервером.
* Не выделяйте большие объекты в C# JavaScript и коде.
* Не блокируйте основной поток пользовательского интерфейса на длительные периоды при отправке или получении данных.
* Освободите память, потребляемую при завершении или отмене процесса.
* Применяйте следующие дополнительные требования в целях безопасности:
  * Объявите максимальный размер файла или данных, который может быть передан.
  * Объявите минимальную скорость передачи от клиента к серверу.
* После получения данных сервером данные могут быть:
  * Временно хранится в буфере памяти до тех пор, пока не будут собраны все сегменты.
  * Используется немедленно. Например, данные могут храниться сразу в базе данных или записываться на диск по мере получения каждого сегмента.

Следующий класс загрузчика файлов обрабатывает взаимодействие JS с клиентом. Класс Loader использует взаимодействие JS для:

* Опросить клиент, чтобы отправить сегмент данных.
* Прервать транзакцию при истечении времени опроса.

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

В предшествующем примере:

* Для `_maxBase64SegmentSize` задано значение `8192`, которое вычисляется на основе `_maxBase64SegmentSize = _segmentSize * 4 / 3`.
* Низкоуровневые API управления памятью .NET Core используются для хранения сегментов памяти на сервере в `_uploadedSegments`.
* Для выполнения передачи через JS-взаимодействие используется метод `ReceiveFile`:
  * Размер файла определяется с помощью взаимодействия в байтах через JS с `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.
  * Количество принимаемых сегментов вычисляется и сохраняется в `numberOfSegments`.
  * Сегменты запрашиваются в цикле `for` через JS-взаимодействие с `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`. Все сегменты, кроме последнего, должны быть 8 192 байт перед декодированием. Клиент вынужден отправить данные эффективным способом.
  * Для каждого полученного сегмента проверки выполняются перед декодированием с помощью <xref:System.Convert.TryFromBase64String*>.
  * Поток с данными возвращается в виде нового <xref:System.IO.Stream> (`SegmentedStream`) после завершения передачи.

Класс сегментированного потока предоставляет список сегментов как недоступный для поиска <xref:System.IO.Stream>:

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

Следующий код реализует функции JavaScript для получения данных:

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Пример Интеропкомпонент. Razor (репозиторий DotNet/AspNetCore GitHub, ветвь выпуска 3,1)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
