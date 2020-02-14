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
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="d3ba5-103">ASP.NET Core Blazor взаимодействия JavaScript</span><span class="sxs-lookup"><span data-stu-id="d3ba5-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="d3ba5-104">[Хавьер Калварро Воронков](https://github.com/javiercn), [Даниэль Roth)](https://github.com/danroth27)и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d3ba5-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="d3ba5-105">Blazor приложение может вызывать функции JavaScript из кода JavaScript в методах .NET и .NET.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="d3ba5-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3ba5-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="d3ba5-107">Вызов функций JavaScript из методов .NET</span><span class="sxs-lookup"><span data-stu-id="d3ba5-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="d3ba5-108">В некоторых случаях для вызова функции JavaScript требуется код .NET.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="d3ba5-109">Например, вызов JavaScript может предоставлять приложению возможности браузера или функциональные возможности из библиотеки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="d3ba5-110">Этот сценарий называется *совместимостью JavaScript* (*JS Interop*).</span><span class="sxs-lookup"><span data-stu-id="d3ba5-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="d3ba5-111">Чтобы вызвать JavaScript из .NET, используйте абстракцию `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="d3ba5-112">Чтобы выдать вызовы взаимодействия JS, вставьте в компонент абстракцию `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="d3ba5-113">Метод `InvokeAsync<T>` принимает идентификатор для функции JavaScript, которую нужно вызвать вместе с любым числом аргументов, сериализуемых в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="d3ba5-114">Идентификатор функции задается относительно глобальной области (`window`).</span><span class="sxs-lookup"><span data-stu-id="d3ba5-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="d3ba5-115">Если вы хотите вызвать `window.someScope.someFunction`, идентификатор будет `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="d3ba5-116">Нет необходимости регистрировать функцию перед ее вызовом.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="d3ba5-117">Тип возвращаемого значения `T` также должен быть сериализуемым в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="d3ba5-118">`T` должен соответствовать типу .NET, который лучше сопоставить с возвращаемым типом JSON.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="d3ba5-119">Для Blazor серверных приложений с включенной предварительной отрисовкой вызов JavaScript невозможен во время первоначальной отрисовки.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="d3ba5-120">Вызовы взаимодействия JavaScript должны быть отложены до тех пор, пока не будет установлено соединение с браузером.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="d3ba5-121">Дополнительные сведения см. в разделе [Обнаружение времени подготовки Blazor приложения](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="d3ba5-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="d3ba5-122">Следующий пример основан на [текстдекодер](https://developer.mozilla.org/docs/Web/API/TextDecoder), экспериментальном декодере на основе JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="d3ba5-123">В примере показано, как вызвать функцию JavaScript из C# метода.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="d3ba5-124">Функция JavaScript принимает массив байтов из C# метода, декодирует массив и возвращает текст компоненту для вывода.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="d3ba5-125">В `<head>` элементе *wwwroot/index.HTML* (Blazor Assembly) или *pages/_Host. cshtml* (Blazor Server) Укажите функцию JavaScript, которая использует `TextDecoder` для декодирования переданного массива и возврата декодированного значения:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="d3ba5-126">Код JavaScript, например код, показанный в предыдущем примере, можно также загрузить из файла JavaScript ( *. js*) со ссылкой на файл скрипта:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="d3ba5-127">Следующий компонент:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-127">The following component:</span></span>

* <span data-ttu-id="d3ba5-128">Вызывает функцию JavaScript `convertArray`, используя `JSRuntime` при выборе кнопки компонента (**преобразование массива**).</span><span class="sxs-lookup"><span data-stu-id="d3ba5-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="d3ba5-129">После вызова функции JavaScript переданный массив преобразуется в строку.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="d3ba5-130">Строка возвращается компоненту для вывода.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="d3ba5-131">Использование Ижсрунтиме</span><span class="sxs-lookup"><span data-stu-id="d3ba5-131">Use of IJSRuntime</span></span>

<span data-ttu-id="d3ba5-132">Чтобы использовать абстракцию `IJSRuntime`, следует принять любой из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="d3ba5-133">Внедрить `IJSRuntime`ую абстракцию в компонент Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="d3ba5-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="d3ba5-134">В элементе `<head>` *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server) укажите `handleTickerChanged` функцию JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="d3ba5-135">Функция вызывается с `IJSRuntime.InvokeVoidAsync` и не возвращает значение:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="d3ba5-136">Внедрить `IJSRuntime`ую абстракцию в класс (*CS*):</span><span class="sxs-lookup"><span data-stu-id="d3ba5-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="d3ba5-137">В элементе `<head>` *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server) укажите `handleTickerChanged` функцию JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="d3ba5-138">Функция вызывается с `JSRuntime.InvokeAsync` и возвращает значение:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="d3ba5-139">Для динамического создания содержимого с помощью [буилдрендертри](xref:blazor/components#manual-rendertreebuilder-logic)используйте атрибут `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="d3ba5-140">В примере приложения на стороне клиента, прилагаемом к этой теме, для приложения, которое взаимодействует с моделью DOM для получения вводимых пользователем данных и вывода приветственного сообщения, доступны две функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="d3ba5-141">`showPrompt` &ndash; выдает запрос на ввод пользователя (имя пользователя) и возвращает имя вызывающему объекту.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="d3ba5-142">`displayWelcome` &ndash; назначает приветственное сообщение объекту модели DOM с помощью `id` `welcome`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="d3ba5-143">*wwwroot/ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="d3ba5-144">Поместите тег `<script>`, который ссылается на файл JavaScript, в файл *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="d3ba5-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="d3ba5-145">*wwwroot/index.HTML* (Blazorная сборка):</span><span class="sxs-lookup"><span data-stu-id="d3ba5-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="d3ba5-146">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="d3ba5-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="d3ba5-147">Не размещайте тег `<script>` в файле компонента, так как тег `<script>` нельзя обновлять динамически.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="d3ba5-148">Методы .NET взаимодействует с функциями JavaScript в файле *ексамплежсинтероп. js* путем вызова `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="d3ba5-149">Абстракция `IJSRuntime` является асинхронной, что позволяет выполнять сценарии с Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="d3ba5-150">Если приложение является Blazor приложением сборки и необходимо синхронно вызвать функцию JavaScript, произведение производных `IJSInProcessRuntime` и вызвать `Invoke<T>`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="d3ba5-151">Рекомендуется, чтобы большинство библиотек взаимодействия JS использовали асинхронные интерфейсы API, чтобы обеспечить доступность библиотек во всех сценариях.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="d3ba5-152">Пример приложения включает компонент для демонстрации взаимодействия с JS.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="d3ba5-153">Компонент:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-153">The component:</span></span>

* <span data-ttu-id="d3ba5-154">Получает входные данные пользователя с помощью командной строки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="d3ba5-155">Возвращает текст компонента для обработки.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="d3ba5-156">Вызывает вторую функцию JavaScript, которая взаимодействует с моделью DOM для вывода приветственного сообщения.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="d3ba5-157">*Pages/жсинтероп. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-157">*Pages/JSInterop.razor*:</span></span>

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

1. <span data-ttu-id="d3ba5-158">Когда `TriggerJsPrompt` выполняется при нажатии кнопки **запроса JavaScript триггера** компонента, вызывается функция JavaScript `showPrompt`, предоставленная в файле *wwwroot/ексамплежсинтероп. js* .</span><span class="sxs-lookup"><span data-stu-id="d3ba5-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="d3ba5-159">Функция `showPrompt` принимает ввод пользователя (имя пользователя), который кодируется в формате HTML и возвращается компоненту.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="d3ba5-160">Компонент сохраняет имя пользователя в локальной переменной `name`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="d3ba5-161">Строка, хранящаяся в `name`, включается в приветственное сообщение, которое передается в функцию JavaScript, `displayWelcome`, которая визуализирует приветственное сообщение в тег заголовка.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="d3ba5-162">Вызов функции JavaScript void</span><span class="sxs-lookup"><span data-stu-id="d3ba5-162">Call a void JavaScript function</span></span>

<span data-ttu-id="d3ba5-163">Функции JavaScript, возвращающие [значение void (0)/воид 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) или [undefine](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) , вызываются с `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="d3ba5-164">Обнаружение времени предварительной отрисовки Blazor приложения</span><span class="sxs-lookup"><span data-stu-id="d3ba5-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="d3ba5-165">Запись ссылок на элементы</span><span class="sxs-lookup"><span data-stu-id="d3ba5-165">Capture references to elements</span></span>

<span data-ttu-id="d3ba5-166">В некоторых сценариях взаимодействия JS требуются ссылки на элементы HTML.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="d3ba5-167">Например, библиотеке пользовательского интерфейса может требоваться ссылка на элемент для инициализации, или же может потребоваться вызвать API-интерфейсы, подобные Command, в элементе, например `focus` или `play`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="d3ba5-168">Запишите ссылки на элементы HTML в компоненте, используя следующий подход:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="d3ba5-169">Добавьте атрибут `@ref` в элемент HTML.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="d3ba5-170">Определите поле типа `ElementReference`, имя которого соответствует значению атрибута `@ref`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="d3ba5-171">В следующем примере показана запись ссылки на элемент `<input>` `username`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="d3ba5-172">Используйте ссылку на элемент только для изменения содержимого пустого элемента, который не взаимодействует с Blazor.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="d3ba5-173">Этот сценарий полезен, если сторонний API передает содержимое элементу.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="d3ba5-174">Поскольку Blazor не взаимодействует с элементом, существует вероятность конфликта между представлением элемента и DOM в Blazor.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="d3ba5-175">В следующем примере *опасно* изменить содержимое неупорядоченного списка (`ul`), поскольку Blazor взаимодействует с моделью DOM для заполнения элементов списка этого элемента (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="d3ba5-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="d3ba5-176">Если при взаимодействии JS изменяется содержимое элемента `MyList` и Blazor пытается применить diff к элементу, различия не будут соответствовать модели DOM.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="d3ba5-177">Что касается кода .NET, `ElementReference` является непрозрачным маркером.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="d3ba5-178">*Единственное* , что можно сделать с `ElementReference`, — передать его в код JavaScript через JS-взаимодействие.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="d3ba5-179">При этом код на стороне JavaScript получает экземпляр `HTMLElement`, который можно использовать с обычными API DOM.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="d3ba5-180">Например, следующий код определяет метод расширения .NET, который позволяет установить фокус на элемент:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="d3ba5-181">*ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="d3ba5-182">Чтобы вызвать функцию JavaScript, которая не возвращает значение, используйте `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="d3ba5-183">Следующий код задает фокус ввода имени пользователя, вызывая предыдущую функцию JavaScript с захваченным `ElementReference`:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="d3ba5-184">Чтобы использовать метод расширения, создайте статический метод расширения, который получает экземпляр `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="d3ba5-185">Метод `Focus` вызывается непосредственно для объекта.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="d3ba5-186">В следующем примере предполагается, что метод `Focus` доступен из пространства имен `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="d3ba5-187">`username` переменная заполняется только после подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="d3ba5-188">Если незаполненный `ElementReference` передается в код JavaScript, код JavaScript получает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="d3ba5-189">Для управления ссылками на элементы после завершения отрисовки компонента (для установки начального фокуса на элемент) используйте [методы жизненного цикла компонента онафтеррендерасинк или онафтеррендер](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="d3ba5-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="d3ba5-190">При работе с универсальными типами и возвратом значения используйте [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="d3ba5-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="d3ba5-191">`GenericMethod` вызывается непосредственно для объекта с типом.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="d3ba5-192">В следующем примере предполагается, что `GenericMethod` доступен из пространства имен `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="d3ba5-193">Ссылочные элементы в разных компонентах</span><span class="sxs-lookup"><span data-stu-id="d3ba5-193">Reference elements across components</span></span>

<span data-ttu-id="d3ba5-194">`ElementReference` гарантируется только в методе `OnAfterRender` компонента (а ссылка на элемент является `struct`), поэтому ссылка на элемент не может передаваться между компонентами.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-194">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="d3ba5-195">Чтобы родительский компонент мог сделать ссылку на элемент доступным для других компонентов, родительский компонент может:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-195">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="d3ba5-196">Разрешить дочерним компонентам регистрировать обратные вызовы.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-196">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="d3ba5-197">Вызов зарегистрированных обратных вызовов во время события `OnAfterRender` с переданной ссылкой на элемент.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-197">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="d3ba5-198">Косвенно этот подход позволяет дочерним компонентам взаимодействовать со ссылкой на элемент родителя.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-198">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="d3ba5-199">В приведенном ниже примере Blazorной сборки показан этот подход.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-199">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="d3ba5-200">В `<head>` *wwwroot/index.HTML*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-200">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="d3ba5-201">В `<body>` *wwwroot/index.HTML*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-201">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="d3ba5-202">*Pages/index. Razor* (родительский компонент):</span><span class="sxs-lookup"><span data-stu-id="d3ba5-202">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="d3ba5-203">*Pages/index. Razor. CS*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-203">*Pages/Index.razor.cs*:</span></span>

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

<span data-ttu-id="d3ba5-204">*Shared/сурвэйпромпт. Razor* (дочерний компонент):</span><span class="sxs-lookup"><span data-stu-id="d3ba5-204">*Shared/SurveyPrompt.razor* (child component):</span></span>

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

<span data-ttu-id="d3ba5-205">*Shared/сурвэйпромпт. Razor. CS*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-205">*Shared/SurveyPrompt.razor.cs*:</span></span>

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

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="d3ba5-206">Вызов методов .NET из функций JavaScript</span><span class="sxs-lookup"><span data-stu-id="d3ba5-206">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="d3ba5-207">Статический вызов метода .NET</span><span class="sxs-lookup"><span data-stu-id="d3ba5-207">Static .NET method call</span></span>

<span data-ttu-id="d3ba5-208">Чтобы вызвать статический метод .NET из JavaScript, используйте функции `DotNet.invokeMethod` или `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-208">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="d3ba5-209">Передайте идентификатор статического метода, который необходимо вызвать, имя сборки, содержащей функцию, и любые аргументы.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-209">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="d3ba5-210">Асинхронная версия является предпочтительной для поддержки сценариев Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-210">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="d3ba5-211">Метод .NET должен быть открытым, статическим и иметь атрибут `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-211">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="d3ba5-212">Вызов открытых универсальных методов в настоящее время не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-212">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="d3ba5-213">Пример приложения включает C# метод для возврата массива `int`s.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-213">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="d3ba5-214">К методу применяется атрибут `JSInvokable`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-214">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="d3ba5-215">*Pages/жсинтероп. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-215">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="d3ba5-216">JavaScript, предоставляемый клиенту, вызывает метод C# .NET.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-216">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="d3ba5-217">*wwwroot/ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-217">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="d3ba5-218">При выборе переключателя **статический метод ретурнаррайасинк .NET** изучите выходные данные консоли в средствах веб-разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-218">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="d3ba5-219">Выходные данные консоли:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-219">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="d3ba5-220">Четвертое значение массива помещается в массив (`data.push(4);`), возвращаемый `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-220">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="d3ba5-221">По умолчанию идентификатором метода является имя метода, но можно указать другой идентификатор с помощью конструктора `JSInvokableAttribute`:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-221">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="d3ba5-222">В файле JavaScript на стороне клиента:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-222">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a><span data-ttu-id="d3ba5-223">Вызов метода экземпляра</span><span class="sxs-lookup"><span data-stu-id="d3ba5-223">Instance method call</span></span>

<span data-ttu-id="d3ba5-224">Кроме того, можно вызывать методы экземпляра .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-224">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="d3ba5-225">Вызов метода экземпляра .NET из JavaScript:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-225">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="d3ba5-226">Передайте экземпляр .NET в JavaScript, заключив его в экземпляр `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-226">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="d3ba5-227">Экземпляр .NET передается по ссылке на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-227">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="d3ba5-228">Вызывайте методы экземпляра .NET в экземпляре с помощью функций `invokeMethod` или `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-228">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="d3ba5-229">Экземпляр .NET можно также передать в качестве аргумента при вызове других методов .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-229">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="d3ba5-230">Пример приложения записывает сообщения в консоль на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-230">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="d3ba5-231">В следующих примерах, продемонстрированных в примере приложения, изучите выходные данные консоли браузера в средствах разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-231">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="d3ba5-232">Когда выбран **метод триггера экземпляра .NET хеллохелпер. SayHello** , `ExampleJsInterop.CallHelloHelperSayHello` вызывается и передает имя, `Blazor`, в метод.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-232">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="d3ba5-233">*Pages/жсинтероп. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-233">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="d3ba5-234">`CallHelloHelperSayHello` вызывает функцию JavaScript `sayHello` с новым экземпляром `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-234">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="d3ba5-235">*Жсинтеропклассес/ексамплежсинтероп. CS*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-235">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="d3ba5-236">*wwwroot/ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-236">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="d3ba5-237">Имя передается в конструктор `HelloHelper`, который задает свойство `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-237">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="d3ba5-238">При выполнении `sayHello` функции JavaScript `HelloHelper.SayHello` возвращает `Hello, {Name}!` сообщение, которое записывается в консоль функцией JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-238">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="d3ba5-239">*Жсинтеропклассес/хеллохелпер. CS*:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-239">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="d3ba5-240">Выходные данные консоли в средствах веб-разработчика браузера:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-240">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="d3ba5-241">Совместное использование кода взаимодействия в библиотеке классов</span><span class="sxs-lookup"><span data-stu-id="d3ba5-241">Share interop code in a class library</span></span>

<span data-ttu-id="d3ba5-242">Код взаимодействия JS может быть добавлен в библиотеку классов, которая позволяет совместно использовать код в пакете NuGet.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-242">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="d3ba5-243">Библиотека классов обрабатывает внедрение ресурсов JavaScript в построенную сборку.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-243">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="d3ba5-244">Файлы JavaScript помещаются в папку *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="d3ba5-244">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="d3ba5-245">Инструментарий выполняет внедрение ресурсов при построении библиотеки.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-245">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="d3ba5-246">В файле проекта приложения имеется ссылка на созданный пакет NuGet так же, как и на любой пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-246">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="d3ba5-247">После восстановления пакета код приложения может вызывать JavaScript, как если бы он был C#.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-247">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="d3ba5-248">Дополнительные сведения см. в разделе <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-248">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="d3ba5-249">Вызовы взаимодействия с зафиксированным JS</span><span class="sxs-lookup"><span data-stu-id="d3ba5-249">Harden JS interop calls</span></span>

<span data-ttu-id="d3ba5-250">Взаимодействие с JS может завершиться сбоем из-за ошибок сети и должно рассматриваться как ненадежное.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-250">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="d3ba5-251">По умолчанию серверное приложение Blazor истекает из JS-вызовов взаимодействия на сервере через одну минуту.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-251">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="d3ba5-252">Если приложение может допускать более агрессивное время ожидания, например 10 секунд, задайте время ожидания одним из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-252">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="d3ba5-253">Глобально в `Startup.ConfigureServices`укажите время ожидания:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-253">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="d3ba5-254">Для каждого вызова в коде компонента один вызов может указать время ожидания:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-254">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="d3ba5-255">Дополнительные сведения об исчерпании ресурсов см. в разделе <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-255">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a><span data-ttu-id="d3ba5-256">Выполнение больших объемов передачи данных в приложениях Blazor Server</span><span class="sxs-lookup"><span data-stu-id="d3ba5-256">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="d3ba5-257">В некоторых сценариях необходимо передавать большие объемы данных между JavaScript и Blazor.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-257">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="d3ba5-258">Как правило, передача больших данных происходит в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-258">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="d3ba5-259">API-интерфейсы файловой системы браузера используются для отправки или загрузки файла.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-259">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="d3ba5-260">Требуется взаимодействие с библиотекой стороннего разработчика.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-260">Interop with a third party library is required.</span></span>

<span data-ttu-id="d3ba5-261">В Blazor Server есть ограничение, предотвращающее передачу одного большого количества сообщений, что может привести к проблемам с производительностью.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-261">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="d3ba5-262">При разработке кода, который передает данные между JavaScript и Blazor, учитывайте следующие рекомендации.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-262">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="d3ba5-263">Разбивает данные на небольшие части и отправляйте сегменты данных последовательно, пока все данные не будут получены сервером.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-263">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="d3ba5-264">Не выделяйте большие объекты в C# JavaScript и коде.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-264">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="d3ba5-265">Не блокируйте основной поток пользовательского интерфейса на длительные периоды при отправке или получении данных.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-265">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="d3ba5-266">Освободите память, потребляемую при завершении или отмене процесса.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-266">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="d3ba5-267">Применяйте следующие дополнительные требования в целях безопасности:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-267">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="d3ba5-268">Объявите максимальный размер файла или данных, который может быть передан.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-268">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="d3ba5-269">Объявите минимальную скорость передачи от клиента к серверу.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-269">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="d3ba5-270">После получения данных сервером данные могут быть:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-270">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="d3ba5-271">Временно хранится в буфере памяти до тех пор, пока не будут собраны все сегменты.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-271">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="d3ba5-272">Используется немедленно.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-272">Consumed immediately.</span></span> <span data-ttu-id="d3ba5-273">Например, данные могут храниться сразу в базе данных или записываться на диск по мере получения каждого сегмента.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-273">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="d3ba5-274">Следующий класс загрузчика файлов обрабатывает взаимодействие JS с клиентом.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-274">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="d3ba5-275">Класс Loader использует взаимодействие JS для:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-275">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="d3ba5-276">Опросить клиент, чтобы отправить сегмент данных.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-276">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="d3ba5-277">Прервать транзакцию при истечении времени опроса.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-277">Abort the transaction if polling times out.</span></span>

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

<span data-ttu-id="d3ba5-278">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-278">In the preceding example:</span></span>

* <span data-ttu-id="d3ba5-279">Для `_maxBase64SegmentSize` задано значение `8192`, которое вычисляется на основе `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-279">The `_maxBase64SegmentSize` is set to `8192`, which is calculated from `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="d3ba5-280">Низкоуровневые API управления памятью .NET Core используются для хранения сегментов памяти на сервере в `_uploadedSegments`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-280">Low level .NET Core memory management APIs are used to store the memory segments on the server in `_uploadedSegments`.</span></span>
* <span data-ttu-id="d3ba5-281">Для выполнения передачи через JS-взаимодействие используется метод `ReceiveFile`:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-281">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="d3ba5-282">Размер файла определяется с помощью взаимодействия в байтах через JS с `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-282">The file size is determined in bytes through JS interop with `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="d3ba5-283">Количество принимаемых сегментов вычисляется и сохраняется в `numberOfSegments`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-283">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="d3ba5-284">Сегменты запрашиваются в цикле `for` через JS-взаимодействие с `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-284">The segments are requested in a `for` loop through JS interop with `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="d3ba5-285">Все сегменты, кроме последнего, должны быть 8 192 байт перед декодированием.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-285">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="d3ba5-286">Клиент вынужден отправить данные эффективным способом.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-286">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="d3ba5-287">Для каждого полученного сегмента проверки выполняются перед декодированием с помощью <xref:System.Convert.TryFromBase64String*>.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-287">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String*>.</span></span>
  * <span data-ttu-id="d3ba5-288">Поток с данными возвращается в виде нового <xref:System.IO.Stream> (`SegmentedStream`) после завершения передачи.</span><span class="sxs-lookup"><span data-stu-id="d3ba5-288">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="d3ba5-289">Класс сегментированного потока предоставляет список сегментов как недоступный для поиска <xref:System.IO.Stream>:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-289">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

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

<span data-ttu-id="d3ba5-290">Следующий код реализует функции JavaScript для получения данных:</span><span class="sxs-lookup"><span data-stu-id="d3ba5-290">The following code implements JavaScript functions to receive the data:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="d3ba5-291">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d3ba5-291">Additional resources</span></span>

* [<span data-ttu-id="d3ba5-292">Пример Интеропкомпонент. Razor (репозиторий DotNet/AspNetCore GitHub, ветвь выпуска 3,1)</span><span class="sxs-lookup"><span data-stu-id="d3ba5-292">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
