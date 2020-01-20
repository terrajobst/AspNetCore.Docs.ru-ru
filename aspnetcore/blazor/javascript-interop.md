---
title: ASP.NET Core Blazor взаимодействия JavaScript
author: guardrex
description: Узнайте, как вызывать функции JavaScript из методов .NET и .NET из JavaScript в Blazor приложениях.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: e1b9c84dace193768c6f3fbb5636ef675d65a20d
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159911"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="5a91d-103">ASP.NET Core Blazor взаимодействия JavaScript</span><span class="sxs-lookup"><span data-stu-id="5a91d-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="5a91d-104">[Хавьер Калварро Воронков](https://github.com/javiercn), [Даниэль Roth)](https://github.com/danroth27)и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5a91d-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="5a91d-105">Blazor приложение может вызывать функции JavaScript из кода JavaScript в методах .NET и .NET.</span><span class="sxs-lookup"><span data-stu-id="5a91d-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="5a91d-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a91d-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="5a91d-107">Вызов функций JavaScript из методов .NET</span><span class="sxs-lookup"><span data-stu-id="5a91d-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="5a91d-108">В некоторых случаях для вызова функции JavaScript требуется код .NET.</span><span class="sxs-lookup"><span data-stu-id="5a91d-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="5a91d-109">Например, вызов JavaScript может предоставлять приложению возможности браузера или функциональные возможности из библиотеки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="5a91d-110">Этот сценарий называется *совместимостью JavaScript* (*JS Interop*).</span><span class="sxs-lookup"><span data-stu-id="5a91d-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="5a91d-111">Чтобы вызвать JavaScript из .NET, используйте абстракцию `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="5a91d-112">Чтобы выдать вызовы взаимодействия JS, вставьте в компонент абстракцию `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="5a91d-113">Метод `InvokeAsync<T>` принимает идентификатор для функции JavaScript, которую нужно вызвать вместе с любым числом аргументов, сериализуемых в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="5a91d-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="5a91d-114">Идентификатор функции задается относительно глобальной области (`window`).</span><span class="sxs-lookup"><span data-stu-id="5a91d-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="5a91d-115">Если вы хотите вызвать `window.someScope.someFunction`, идентификатор будет `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="5a91d-116">Нет необходимости регистрировать функцию перед ее вызовом.</span><span class="sxs-lookup"><span data-stu-id="5a91d-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="5a91d-117">Тип возвращаемого значения `T` также должен быть сериализуемым в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="5a91d-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="5a91d-118">`T` должен соответствовать типу .NET, который лучше сопоставить с возвращаемым типом JSON.</span><span class="sxs-lookup"><span data-stu-id="5a91d-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="5a91d-119">Для Blazor серверных приложений с включенной предварительной отрисовкой вызов JavaScript невозможен во время первоначальной отрисовки.</span><span class="sxs-lookup"><span data-stu-id="5a91d-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="5a91d-120">Вызовы взаимодействия JavaScript должны быть отложены до тех пор, пока не будет установлено соединение с браузером.</span><span class="sxs-lookup"><span data-stu-id="5a91d-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="5a91d-121">Дополнительные сведения см. в разделе [Обнаружение времени подготовки Blazor приложения](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="5a91d-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="5a91d-122">Следующий пример основан на [текстдекодер](https://developer.mozilla.org/docs/Web/API/TextDecoder), экспериментальном декодере на основе JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="5a91d-123">В примере показано, как вызвать функцию JavaScript из C# метода.</span><span class="sxs-lookup"><span data-stu-id="5a91d-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="5a91d-124">Функция JavaScript принимает массив байтов из C# метода, декодирует массив и возвращает текст компоненту для вывода.</span><span class="sxs-lookup"><span data-stu-id="5a91d-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="5a91d-125">В `<head>` элементе *wwwroot/index.HTML* (Blazor Assembly) или *pages/_Host. cshtml* (Blazor Server) Укажите функцию JavaScript, которая использует `TextDecoder` для декодирования переданного массива и возврата декодированного значения:</span><span class="sxs-lookup"><span data-stu-id="5a91d-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="5a91d-126">Код JavaScript, например код, показанный в предыдущем примере, можно также загрузить из файла JavaScript ( *. js*) со ссылкой на файл скрипта:</span><span class="sxs-lookup"><span data-stu-id="5a91d-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="5a91d-127">Следующий компонент:</span><span class="sxs-lookup"><span data-stu-id="5a91d-127">The following component:</span></span>

* <span data-ttu-id="5a91d-128">Вызывает функцию JavaScript `convertArray`, используя `JSRuntime` при выборе кнопки компонента (**преобразование массива**).</span><span class="sxs-lookup"><span data-stu-id="5a91d-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="5a91d-129">После вызова функции JavaScript переданный массив преобразуется в строку.</span><span class="sxs-lookup"><span data-stu-id="5a91d-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="5a91d-130">Строка возвращается компоненту для вывода.</span><span class="sxs-lookup"><span data-stu-id="5a91d-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="5a91d-131">Использование Ижсрунтиме</span><span class="sxs-lookup"><span data-stu-id="5a91d-131">Use of IJSRuntime</span></span>

<span data-ttu-id="5a91d-132">Чтобы использовать абстракцию `IJSRuntime`, следует принять любой из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="5a91d-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="5a91d-133">Внедрить `IJSRuntime`ую абстракцию в компонент Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="5a91d-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="5a91d-134">В элементе `<head>` *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server) укажите `handleTickerChanged` функцию JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="5a91d-135">Функция вызывается с `IJSRuntime.InvokeVoidAsync` и не возвращает значение:</span><span class="sxs-lookup"><span data-stu-id="5a91d-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="5a91d-136">Внедрить `IJSRuntime`ую абстракцию в класс (*CS*):</span><span class="sxs-lookup"><span data-stu-id="5a91d-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="5a91d-137">В элементе `<head>` *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server) укажите `handleTickerChanged` функцию JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="5a91d-138">Функция вызывается с `JSRuntime.InvokeAsync` и возвращает значение:</span><span class="sxs-lookup"><span data-stu-id="5a91d-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="5a91d-139">Для динамического создания содержимого с помощью [буилдрендертри](xref:blazor/components#manual-rendertreebuilder-logic)используйте атрибут `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="5a91d-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="5a91d-140">В примере приложения на стороне клиента, прилагаемом к этой теме, для приложения, которое взаимодействует с моделью DOM для получения вводимых пользователем данных и вывода приветственного сообщения, доступны две функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="5a91d-141">`showPrompt` &ndash; выдает запрос на ввод пользователя (имя пользователя) и возвращает имя вызывающему объекту.</span><span class="sxs-lookup"><span data-stu-id="5a91d-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="5a91d-142">`displayWelcome` &ndash; назначает приветственное сообщение объекту модели DOM с помощью `id` `welcome`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="5a91d-143">*wwwroot/ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="5a91d-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="5a91d-144">Поместите тег `<script>`, который ссылается на файл JavaScript, в файл *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="5a91d-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="5a91d-145">*wwwroot/index.HTML* (Blazorная сборка):</span><span class="sxs-lookup"><span data-stu-id="5a91d-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="5a91d-146">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="5a91d-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="5a91d-147">Не размещайте тег `<script>` в файле компонента, так как тег `<script>` нельзя обновлять динамически.</span><span class="sxs-lookup"><span data-stu-id="5a91d-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="5a91d-148">Методы .NET взаимодействует с функциями JavaScript в файле *ексамплежсинтероп. js* путем вызова `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="5a91d-149">Абстракция `IJSRuntime` является асинхронной, что позволяет выполнять сценарии с Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="5a91d-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="5a91d-150">Если приложение является Blazor приложением сборки и необходимо синхронно вызвать функцию JavaScript, произведение производных `IJSInProcessRuntime` и вызвать `Invoke<T>`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="5a91d-151">Рекомендуется, чтобы большинство библиотек взаимодействия JS использовали асинхронные интерфейсы API, чтобы обеспечить доступность библиотек во всех сценариях.</span><span class="sxs-lookup"><span data-stu-id="5a91d-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="5a91d-152">Пример приложения включает компонент для демонстрации взаимодействия с JS.</span><span class="sxs-lookup"><span data-stu-id="5a91d-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="5a91d-153">Компонент:</span><span class="sxs-lookup"><span data-stu-id="5a91d-153">The component:</span></span>

* <span data-ttu-id="5a91d-154">Получает входные данные пользователя с помощью командной строки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="5a91d-155">Возвращает текст компонента для обработки.</span><span class="sxs-lookup"><span data-stu-id="5a91d-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="5a91d-156">Вызывает вторую функцию JavaScript, которая взаимодействует с моделью DOM для вывода приветственного сообщения.</span><span class="sxs-lookup"><span data-stu-id="5a91d-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="5a91d-157">*Pages/жсинтероп. Razor*:</span><span class="sxs-lookup"><span data-stu-id="5a91d-157">*Pages/JSInterop.razor*:</span></span>

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

1. <span data-ttu-id="5a91d-158">Когда `TriggerJsPrompt` выполняется при нажатии кнопки **запроса JavaScript триггера** компонента, вызывается функция JavaScript `showPrompt`, предоставленная в файле *wwwroot/ексамплежсинтероп. js* .</span><span class="sxs-lookup"><span data-stu-id="5a91d-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="5a91d-159">Функция `showPrompt` принимает ввод пользователя (имя пользователя), который кодируется в формате HTML и возвращается компоненту.</span><span class="sxs-lookup"><span data-stu-id="5a91d-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="5a91d-160">Компонент сохраняет имя пользователя в локальной переменной `name`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="5a91d-161">Строка, хранящаяся в `name`, включается в приветственное сообщение, которое передается в функцию JavaScript, `displayWelcome`, которая визуализирует приветственное сообщение в тег заголовка.</span><span class="sxs-lookup"><span data-stu-id="5a91d-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="5a91d-162">Вызов функции JavaScript void</span><span class="sxs-lookup"><span data-stu-id="5a91d-162">Call a void JavaScript function</span></span>

<span data-ttu-id="5a91d-163">Функции JavaScript, возвращающие [значение void (0)/воид 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) или [undefine](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) , вызываются с `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="5a91d-164">Обнаружение времени предварительной отрисовки Blazor приложения</span><span class="sxs-lookup"><span data-stu-id="5a91d-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="5a91d-165">Запись ссылок на элементы</span><span class="sxs-lookup"><span data-stu-id="5a91d-165">Capture references to elements</span></span>

<span data-ttu-id="5a91d-166">В некоторых сценариях взаимодействия JS требуются ссылки на элементы HTML.</span><span class="sxs-lookup"><span data-stu-id="5a91d-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="5a91d-167">Например, библиотеке пользовательского интерфейса может требоваться ссылка на элемент для инициализации, или же может потребоваться вызвать API-интерфейсы, подобные Command, в элементе, например `focus` или `play`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="5a91d-168">Запишите ссылки на элементы HTML в компоненте, используя следующий подход:</span><span class="sxs-lookup"><span data-stu-id="5a91d-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="5a91d-169">Добавьте атрибут `@ref` в элемент HTML.</span><span class="sxs-lookup"><span data-stu-id="5a91d-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="5a91d-170">Определите поле типа `ElementReference`, имя которого соответствует значению атрибута `@ref`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="5a91d-171">В следующем примере показана запись ссылки на элемент `<input>` `username`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="5a91d-172">Используйте ссылку на элемент только для изменения содержимого пустого элемента, который не взаимодействует с Blazor.</span><span class="sxs-lookup"><span data-stu-id="5a91d-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="5a91d-173">Этот сценарий полезен, если сторонний API передает содержимое элементу.</span><span class="sxs-lookup"><span data-stu-id="5a91d-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="5a91d-174">Поскольку Blazor не взаимодействует с элементом, существует вероятность конфликта между представлением элемента и DOM в Blazor.</span><span class="sxs-lookup"><span data-stu-id="5a91d-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="5a91d-175">В следующем примере *опасно* изменить содержимое неупорядоченного списка (`ul`), поскольку Blazor взаимодействует с моделью DOM для заполнения элементов списка этого элемента (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="5a91d-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="5a91d-176">Если при взаимодействии JS изменяется содержимое элемента `MyList` и Blazor пытается применить diff к элементу, различия не будут соответствовать модели DOM.</span><span class="sxs-lookup"><span data-stu-id="5a91d-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="5a91d-177">Что касается кода .NET, `ElementReference` является непрозрачным маркером.</span><span class="sxs-lookup"><span data-stu-id="5a91d-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="5a91d-178">*Единственное* , что можно сделать с `ElementReference`, — передать его в код JavaScript через JS-взаимодействие.</span><span class="sxs-lookup"><span data-stu-id="5a91d-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="5a91d-179">При этом код на стороне JavaScript получает экземпляр `HTMLElement`, который можно использовать с обычными API DOM.</span><span class="sxs-lookup"><span data-stu-id="5a91d-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="5a91d-180">Например, следующий код определяет метод расширения .NET, который позволяет установить фокус на элемент:</span><span class="sxs-lookup"><span data-stu-id="5a91d-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="5a91d-181">*ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="5a91d-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="5a91d-182">Чтобы вызвать функцию JavaScript, которая не возвращает значение, используйте `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="5a91d-183">Следующий код задает фокус ввода имени пользователя, вызывая предыдущую функцию JavaScript с захваченным `ElementReference`:</span><span class="sxs-lookup"><span data-stu-id="5a91d-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="5a91d-184">Чтобы использовать метод расширения, создайте статический метод расширения, который получает экземпляр `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="5a91d-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="5a91d-185">Метод `Focus` вызывается непосредственно для объекта.</span><span class="sxs-lookup"><span data-stu-id="5a91d-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="5a91d-186">В следующем примере предполагается, что метод `Focus` доступен из пространства имен `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="5a91d-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="5a91d-187">`username` переменная заполняется только после подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="5a91d-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="5a91d-188">Если незаполненный `ElementReference` передается в код JavaScript, код JavaScript получает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="5a91d-189">Для управления ссылками на элементы после завершения отрисовки компонента (для установки начального фокуса на элемент) используйте [методы жизненного цикла компонента онафтеррендерасинк или онафтеррендер](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="5a91d-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="5a91d-190">При работе с универсальными типами и возвратом значения используйте [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="5a91d-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="5a91d-191">`GenericMethod` вызывается непосредственно для объекта с типом.</span><span class="sxs-lookup"><span data-stu-id="5a91d-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="5a91d-192">В следующем примере предполагается, что `GenericMethod` доступен из пространства имен `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="5a91d-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="5a91d-193">Вызов методов .NET из функций JavaScript</span><span class="sxs-lookup"><span data-stu-id="5a91d-193">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="5a91d-194">Статический вызов метода .NET</span><span class="sxs-lookup"><span data-stu-id="5a91d-194">Static .NET method call</span></span>

<span data-ttu-id="5a91d-195">Чтобы вызвать статический метод .NET из JavaScript, используйте функции `DotNet.invokeMethod` или `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-195">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="5a91d-196">Передайте идентификатор статического метода, который необходимо вызвать, имя сборки, содержащей функцию, и любые аргументы.</span><span class="sxs-lookup"><span data-stu-id="5a91d-196">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="5a91d-197">Асинхронная версия является предпочтительной для поддержки сценариев Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="5a91d-197">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="5a91d-198">Чтобы вызвать метод .NET из JavaScript, метод .NET должен быть открытым, статическим и иметь атрибут `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-198">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="5a91d-199">По умолчанию идентификатором метода является имя метода, но можно указать другой идентификатор с помощью конструктора `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-199">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="5a91d-200">Вызов открытых универсальных методов в настоящее время не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="5a91d-200">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="5a91d-201">Пример приложения включает C# метод для возврата массива `int`s.</span><span class="sxs-lookup"><span data-stu-id="5a91d-201">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="5a91d-202">К методу применяется атрибут `JSInvokable`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-202">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="5a91d-203">*Pages/жсинтероп. Razor*:</span><span class="sxs-lookup"><span data-stu-id="5a91d-203">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="5a91d-204">JavaScript, предоставляемый клиенту, вызывает метод C# .NET.</span><span class="sxs-lookup"><span data-stu-id="5a91d-204">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="5a91d-205">*wwwroot/ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="5a91d-205">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="5a91d-206">При выборе переключателя **статический метод ретурнаррайасинк .NET** изучите выходные данные консоли в средствах веб-разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="5a91d-206">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="5a91d-207">Выходные данные консоли:</span><span class="sxs-lookup"><span data-stu-id="5a91d-207">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="5a91d-208">Четвертое значение массива помещается в массив (`data.push(4);`), возвращаемый `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-208">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="5a91d-209">Вызов метода экземпляра</span><span class="sxs-lookup"><span data-stu-id="5a91d-209">Instance method call</span></span>

<span data-ttu-id="5a91d-210">Кроме того, можно вызывать методы экземпляра .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-210">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="5a91d-211">Вызов метода экземпляра .NET из JavaScript:</span><span class="sxs-lookup"><span data-stu-id="5a91d-211">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="5a91d-212">Передайте экземпляр .NET в JavaScript, заключив его в экземпляр `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-212">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="5a91d-213">Экземпляр .NET передается по ссылке на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-213">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="5a91d-214">Вызывайте методы экземпляра .NET в экземпляре с помощью функций `invokeMethod` или `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-214">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="5a91d-215">Экземпляр .NET можно также передать в качестве аргумента при вызове других методов .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-215">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="5a91d-216">Пример приложения записывает сообщения в консоль на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="5a91d-216">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="5a91d-217">В следующих примерах, продемонстрированных в примере приложения, изучите выходные данные консоли браузера в средствах разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="5a91d-217">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="5a91d-218">Когда выбран **метод триггера экземпляра .NET хеллохелпер. SayHello** , `ExampleJsInterop.CallHelloHelperSayHello` вызывается и передает имя, `Blazor`, в метод.</span><span class="sxs-lookup"><span data-stu-id="5a91d-218">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="5a91d-219">*Pages/жсинтероп. Razor*:</span><span class="sxs-lookup"><span data-stu-id="5a91d-219">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="5a91d-220">`CallHelloHelperSayHello` вызывает функцию JavaScript `sayHello` с новым экземпляром `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-220">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="5a91d-221">*Жсинтеропклассес/ексамплежсинтероп. CS*:</span><span class="sxs-lookup"><span data-stu-id="5a91d-221">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="5a91d-222">*wwwroot/ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="5a91d-222">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="5a91d-223">Имя передается в конструктор `HelloHelper`, который задает свойство `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="5a91d-223">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="5a91d-224">При выполнении `sayHello` функции JavaScript `HelloHelper.SayHello` возвращает `Hello, {Name}!` сообщение, которое записывается в консоль функцией JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a91d-224">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="5a91d-225">*Жсинтеропклассес/хеллохелпер. CS*:</span><span class="sxs-lookup"><span data-stu-id="5a91d-225">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="5a91d-226">Выходные данные консоли в средствах веб-разработчика браузера:</span><span class="sxs-lookup"><span data-stu-id="5a91d-226">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="5a91d-227">Совместное использование кода взаимодействия в библиотеке классов</span><span class="sxs-lookup"><span data-stu-id="5a91d-227">Share interop code in a class library</span></span>

<span data-ttu-id="5a91d-228">Код взаимодействия JS может быть добавлен в библиотеку классов, которая позволяет совместно использовать код в пакете NuGet.</span><span class="sxs-lookup"><span data-stu-id="5a91d-228">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="5a91d-229">Библиотека классов обрабатывает внедрение ресурсов JavaScript в построенную сборку.</span><span class="sxs-lookup"><span data-stu-id="5a91d-229">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="5a91d-230">Файлы JavaScript помещаются в папку *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="5a91d-230">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="5a91d-231">Инструментарий выполняет внедрение ресурсов при построении библиотеки.</span><span class="sxs-lookup"><span data-stu-id="5a91d-231">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="5a91d-232">В файле проекта приложения имеется ссылка на созданный пакет NuGet так же, как и на любой пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="5a91d-232">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="5a91d-233">После восстановления пакета код приложения может вызывать JavaScript, как если бы он был C#.</span><span class="sxs-lookup"><span data-stu-id="5a91d-233">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="5a91d-234">Для получения дополнительной информации см. <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="5a91d-234">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="5a91d-235">Вызовы взаимодействия с зафиксированным JS</span><span class="sxs-lookup"><span data-stu-id="5a91d-235">Harden JS interop calls</span></span>

<span data-ttu-id="5a91d-236">Взаимодействие с JS может завершиться сбоем из-за ошибок сети и должно рассматриваться как ненадежное.</span><span class="sxs-lookup"><span data-stu-id="5a91d-236">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="5a91d-237">По умолчанию серверное приложение Blazor истекает из JS-вызовов взаимодействия на сервере через одну минуту.</span><span class="sxs-lookup"><span data-stu-id="5a91d-237">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="5a91d-238">Если приложение может допускать более агрессивное время ожидания, например 10 секунд, задайте время ожидания одним из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="5a91d-238">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="5a91d-239">Глобально в `Startup.ConfigureServices`укажите время ожидания:</span><span class="sxs-lookup"><span data-stu-id="5a91d-239">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="5a91d-240">Для каждого вызова в коде компонента один вызов может указать время ожидания:</span><span class="sxs-lookup"><span data-stu-id="5a91d-240">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="5a91d-241">Дополнительные сведения об исчерпании ресурсов см. в разделе <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="5a91d-241">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5a91d-242">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5a91d-242">Additional resources</span></span>

* [<span data-ttu-id="5a91d-243">Пример Интеропкомпонент. Razor (репозиторий DotNet/AspNetCore GitHub, ветвь выпуска 3,1)</span><span class="sxs-lookup"><span data-stu-id="5a91d-243">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
