---
title: ASP.NET Core взаимодействие JavaScript Блазор
author: guardrex
description: Узнайте, как вызывать функции JavaScript из методов .NET и .NET из JavaScript в приложениях Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/javascript-interop
ms.openlocfilehash: 4e2c979971f8f550af4aa9653880bfd1e5fae731
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800303"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="1242a-103">ASP.NET Core взаимодействие JavaScript Блазор</span><span class="sxs-lookup"><span data-stu-id="1242a-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="1242a-104">[Хавьер Калварро Воронков](https://github.com/javiercn), [Даниэль Roth)](https://github.com/danroth27)и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1242a-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1242a-105">Приложение Блазор может вызывать функции JavaScript из кода JavaScript в методах .NET и .NET.</span><span class="sxs-lookup"><span data-stu-id="1242a-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="1242a-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1242a-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="1242a-107">Вызов функций JavaScript из методов .NET</span><span class="sxs-lookup"><span data-stu-id="1242a-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="1242a-108">В некоторых случаях для вызова функции JavaScript требуется код .NET.</span><span class="sxs-lookup"><span data-stu-id="1242a-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="1242a-109">Например, вызов JavaScript может предоставлять приложению возможности браузера или функциональные возможности из библиотеки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="1242a-110">Чтобы вызвать JavaScript из .NET, используйте `IJSRuntime` абстракцию.</span><span class="sxs-lookup"><span data-stu-id="1242a-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="1242a-111">`InvokeAsync<T>` Метод принимает идентификатор для функции JavaScript, которую нужно вызвать вместе с любым числом аргументов, сериализуемых в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="1242a-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="1242a-112">Идентификатор функции является относительным к глобальной области (`window`).</span><span class="sxs-lookup"><span data-stu-id="1242a-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="1242a-113">Если вы хотите вызвать `window.someScope.someFunction`, идентификатор имеет `someScope.someFunction`значение.</span><span class="sxs-lookup"><span data-stu-id="1242a-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="1242a-114">Нет необходимости регистрировать функцию перед ее вызовом.</span><span class="sxs-lookup"><span data-stu-id="1242a-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="1242a-115">Возвращаемый тип `T` также должен быть сериализуемым в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="1242a-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="1242a-116">Для приложений на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="1242a-116">For server-side apps:</span></span>

* <span data-ttu-id="1242a-117">Приложение на стороне сервера обрабатывает несколько запросов пользователей.</span><span class="sxs-lookup"><span data-stu-id="1242a-117">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="1242a-118">Не вызывайте `JSRuntime.Current` в компоненте для вызова функций JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="1242a-119">`IJSRuntime` Внедрить абстракцию и использовать внедренный объект для выдаче вызовов взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="1242a-120">Во время предварительной отрисовки приложения Блазор вызов JavaScript невозможен, поскольку не было установлено соединение с браузером.</span><span class="sxs-lookup"><span data-stu-id="1242a-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="1242a-121">Дополнительные сведения см. в разделе [Обнаружение времени подготовки приложения блазор](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="1242a-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="1242a-122">Следующий пример основан на [текстдекодер](https://developer.mozilla.org/docs/Web/API/TextDecoder), экспериментальном декодере на основе JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="1242a-123">В примере показано, как вызвать функцию JavaScript из C# метода.</span><span class="sxs-lookup"><span data-stu-id="1242a-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="1242a-124">Функция JavaScript принимает массив байтов из C# метода, декодирует массив и возвращает текст компоненту для вывода.</span><span class="sxs-lookup"><span data-stu-id="1242a-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="1242a-125">Внутри элемента wwwroot/index.HTML (блазор на стороне клиента) или *pages/_Host. cshtml* (блазор на стороне сервера) Укажите функцию, которая использует `TextDecoder` для декодирования переданного массива: `<head>`</span><span class="sxs-lookup"><span data-stu-id="1242a-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="1242a-126">Код JavaScript, например код, показанный в предыдущем примере, можно также загрузить из файла JavaScript ( *. js*) со ссылкой на файл скрипта:</span><span class="sxs-lookup"><span data-stu-id="1242a-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="1242a-127">Следующий компонент:</span><span class="sxs-lookup"><span data-stu-id="1242a-127">The following component:</span></span>

* <span data-ttu-id="1242a-128">Вызывает функцию JavaScript `ConvertArray` , используя `JsRuntime` , когда выбрана кнопка компонента (**преобразование массива**).</span><span class="sxs-lookup"><span data-stu-id="1242a-128">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="1242a-129">После вызова функции JavaScript переданный массив преобразуется в строку.</span><span class="sxs-lookup"><span data-stu-id="1242a-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="1242a-130">Строка возвращается компоненту для вывода.</span><span class="sxs-lookup"><span data-stu-id="1242a-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="1242a-131">Чтобы использовать `IJSRuntime` абстракцию, следует принять любой из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="1242a-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="1242a-132">Внедрить абстракцию в компонент Razor ( *. Razor*): `IJSRuntime`</span><span class="sxs-lookup"><span data-stu-id="1242a-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="1242a-133">Внедрить абстракцию в класс (CS): `IJSRuntime`</span><span class="sxs-lookup"><span data-stu-id="1242a-133">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="1242a-134">Для создания динамического содержимого с помощью [буилдрендертри](xref:blazor/components#manual-rendertreebuilder-logic)используйте `[Inject]` атрибут:</span><span class="sxs-lookup"><span data-stu-id="1242a-134">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="1242a-135">В примере клиентского приложения, прилагаемого к этой теме, для клиентского приложения, которое взаимодействует с моделью DOM для получения вводимых пользователем данных и вывода приветственного сообщения, доступны две функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-135">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="1242a-136">`showPrompt`&ndash; Создает запрос на ввод пользователя (имя пользователя) и возвращает имя вызывающему объекту.</span><span class="sxs-lookup"><span data-stu-id="1242a-136">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="1242a-137">`displayWelcome`Назначает сообщение приветствия от вызывающего объекта модели DOM объекту `id` с `welcome`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="1242a-137">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="1242a-138">*wwwroot/ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="1242a-138">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="1242a-139">Поместите тег, который ссылается на файл JavaScript, в файл wwwroot/index.HTML (на стороне клиента блазор) или *pages/_Host. cshtml* (блазор на стороне сервера). `<script>`</span><span class="sxs-lookup"><span data-stu-id="1242a-139">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side).</span></span>

<span data-ttu-id="1242a-140">*wwwroot/index.HTML* (На стороне клиента блазор):</span><span class="sxs-lookup"><span data-stu-id="1242a-140">*wwwroot/index.html* (Blazor client-side):</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="1242a-141">*Pages/_Host. cshtml* (Блазор на стороне сервера):</span><span class="sxs-lookup"><span data-stu-id="1242a-141">*Pages/_Host.cshtml* (Blazor server-side):</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

<span data-ttu-id="1242a-142">Не размещайте `<script>` тег в файле компонента, `<script>` так как тег не может быть обновлен динамически.</span><span class="sxs-lookup"><span data-stu-id="1242a-142">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="1242a-143">Методы .NET взаимодействует с функциями JavaScript в файле *ексамплежсинтероп. js* путем вызова `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="1242a-143">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="1242a-144">`IJSRuntime` Абстракция является асинхронной, чтобы разрешить сценарии на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="1242a-144">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="1242a-145">Если приложение работает на стороне клиента и требуется вызвать функцию JavaScript синхронно, следует использовать приведение к `IJSInProcessRuntime` и вызов. `Invoke<T>`</span><span class="sxs-lookup"><span data-stu-id="1242a-145">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="1242a-146">Рекомендуется, чтобы большинство библиотек взаимодействия JavaScript использовали асинхронные интерфейсы API, чтобы обеспечить доступность библиотек во всех сценариях, на стороне клиента или на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="1242a-146">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="1242a-147">Пример приложения содержит компонент для демонстрации взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-147">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="1242a-148">Компонент:</span><span class="sxs-lookup"><span data-stu-id="1242a-148">The component:</span></span>

* <span data-ttu-id="1242a-149">Получает входные данные пользователя с помощью командной строки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-149">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="1242a-150">Возвращает текст компонента для обработки.</span><span class="sxs-lookup"><span data-stu-id="1242a-150">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="1242a-151">Вызывает вторую функцию JavaScript, которая взаимодействует с моделью DOM для вывода приветственного сообщения.</span><span class="sxs-lookup"><span data-stu-id="1242a-151">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="1242a-152">*Pages/жсинтероп. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1242a-152">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="1242a-153">Когда `TriggerJsPrompt` выполняется при нажатии кнопки **командной строки триггера JavaScript** компонента, вызывается `showPrompt` функция JavaScript, предоставленная в файле *wwwroot/ексамплежсинтероп. js* .</span><span class="sxs-lookup"><span data-stu-id="1242a-153">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="1242a-154">`showPrompt` Функция принимает ввод пользователя (имя пользователя), который кодируется в формате HTML и возвращается компоненту.</span><span class="sxs-lookup"><span data-stu-id="1242a-154">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="1242a-155">Компонент сохраняет имя пользователя в локальной переменной `name`.</span><span class="sxs-lookup"><span data-stu-id="1242a-155">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="1242a-156">Строка, хранимая `name` в, включается в приветственное сообщение, которое передается `displayWelcome`функции JavaScript, которая визуализирует приветственное сообщение в тег заголовка.</span><span class="sxs-lookup"><span data-stu-id="1242a-156">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="1242a-157">Вызов функции JavaScript void</span><span class="sxs-lookup"><span data-stu-id="1242a-157">Call a void JavaScript function</span></span>

<span data-ttu-id="1242a-158">Функции JavaScript, возвращающие [значение void (0)/воид 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) или [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined), `IJSRuntime.InvokeAsync<object>`вызываются с `null`параметром, который возвращает.</span><span class="sxs-lookup"><span data-stu-id="1242a-158">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeAsync<object>`, which returns `null`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="1242a-159">Обнаружение времени предварительной отрисовки приложения Блазор</span><span class="sxs-lookup"><span data-stu-id="1242a-159">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="1242a-160">Запись ссылок на элементы</span><span class="sxs-lookup"><span data-stu-id="1242a-160">Capture references to elements</span></span>

<span data-ttu-id="1242a-161">Для некоторых сценариев [взаимодействия JavaScript](xref:blazor/javascript-interop) требуются ссылки на элементы HTML.</span><span class="sxs-lookup"><span data-stu-id="1242a-161">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="1242a-162">Например, библиотеке пользовательского интерфейса может требоваться ссылка на элемент для инициализации, или же может потребоваться вызвать API-интерфейсы, подобные Command, в элементе, например `focus` или `play`.</span><span class="sxs-lookup"><span data-stu-id="1242a-162">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="1242a-163">Запишите ссылки на элементы HTML в компоненте, используя следующий подход:</span><span class="sxs-lookup"><span data-stu-id="1242a-163">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="1242a-164">`@ref` Добавьте атрибут к элементу HTML.</span><span class="sxs-lookup"><span data-stu-id="1242a-164">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="1242a-165">Определите поле типа `ElementReference` , имя которого соответствует значению `@ref` атрибута.</span><span class="sxs-lookup"><span data-stu-id="1242a-165">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="1242a-166">В следующем примере показана запись ссылки на `username` `<input>` элемент:</span><span class="sxs-lookup"><span data-stu-id="1242a-166">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="1242a-167">**Не** используйте захваченные ссылки на элементы в качестве способа заполнения или управления моделью DOM, когда блазор взаимодействует с элементами, на которые имеются ссылки.</span><span class="sxs-lookup"><span data-stu-id="1242a-167">Do **not** use captured element references as a way of populating or manipulating the DOM when Blazor interacts with the elements referenced.</span></span> <span data-ttu-id="1242a-168">Это может повлиять на модель декларативной отрисовки.</span><span class="sxs-lookup"><span data-stu-id="1242a-168">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="1242a-169">Что касается кода .NET, `ElementReference` то является непрозрачным маркером.</span><span class="sxs-lookup"><span data-stu-id="1242a-169">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="1242a-170">*Единственное* , что можно сделать с помощью `ElementReference` , — передать его в код JavaScript через взаимодействие JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-170">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="1242a-171">При этом код на стороне JavaScript получает `HTMLElement` экземпляр, который можно использовать с обычными API DOM.</span><span class="sxs-lookup"><span data-stu-id="1242a-171">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="1242a-172">Например, следующий код определяет метод расширения .NET, который позволяет установить фокус на элемент:</span><span class="sxs-lookup"><span data-stu-id="1242a-172">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="1242a-173">*ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="1242a-173">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="1242a-174">Используйте `IJSRuntime.InvokeAsync<T>` и вызовите `exampleJsFunctions.focusElement` с `ElementReference` помощью, чтобы сосредоточиться на элементе:</span><span class="sxs-lookup"><span data-stu-id="1242a-174">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="1242a-175">Чтобы использовать метод расширения для фокусировки элемента, создайте статический метод расширения, который получает `IJSRuntime` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="1242a-175">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="1242a-176">Метод вызывается непосредственно для объекта.</span><span class="sxs-lookup"><span data-stu-id="1242a-176">The method is called directly on the object.</span></span> <span data-ttu-id="1242a-177">В следующем примере предполагается, что `Focus` статический метод доступен `JsInteropClasses` из пространства имен:</span><span class="sxs-lookup"><span data-stu-id="1242a-177">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="1242a-178">`username` Переменная заполняется только после подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1242a-178">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="1242a-179">Если незаполненный `ElementReference` объект передается в код JavaScript, код JavaScript получает `null`значение.</span><span class="sxs-lookup"><span data-stu-id="1242a-179">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="1242a-180">Для управления ссылками на элементы после завершения отрисовки компонента (для установки начального фокуса на элемент) используйте `OnAfterRenderAsync` [методы жизненного цикла компонента](xref:blazor/components#lifecycle-methods)или. `OnAfterRender`</span><span class="sxs-lookup"><span data-stu-id="1242a-180">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="1242a-181">Вызов методов .NET из функций JavaScript</span><span class="sxs-lookup"><span data-stu-id="1242a-181">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="1242a-182">Статический вызов метода .NET</span><span class="sxs-lookup"><span data-stu-id="1242a-182">Static .NET method call</span></span>

<span data-ttu-id="1242a-183">Чтобы вызвать статический метод .NET из JavaScript, используйте `DotNet.invokeMethod` функции или. `DotNet.invokeMethodAsync`</span><span class="sxs-lookup"><span data-stu-id="1242a-183">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="1242a-184">Передайте идентификатор статического метода, который необходимо вызвать, имя сборки, содержащей функцию, и любые аргументы.</span><span class="sxs-lookup"><span data-stu-id="1242a-184">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="1242a-185">Асинхронная версия является предпочтительной для поддержки сценариев на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="1242a-185">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="1242a-186">Чтобы вызвать метод .NET из JavaScript, метод .NET должен быть открытым, статическим и иметь `[JSInvokable]` атрибут.</span><span class="sxs-lookup"><span data-stu-id="1242a-186">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="1242a-187">По умолчанию идентификатором метода является имя метода, но можно указать другой идентификатор с помощью `JSInvokableAttribute` конструктора.</span><span class="sxs-lookup"><span data-stu-id="1242a-187">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="1242a-188">Вызов открытых универсальных методов в настоящее время не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="1242a-188">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="1242a-189">Пример приложения включает C# метод, возвращающий массив типа `int`.</span><span class="sxs-lookup"><span data-stu-id="1242a-189">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="1242a-190">`JSInvokable` Атрибут применяется к методу.</span><span class="sxs-lookup"><span data-stu-id="1242a-190">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="1242a-191">*Pages/жсинтероп. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1242a-191">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="1242a-192">JavaScript, предоставляемый клиенту, вызывает метод C# .NET.</span><span class="sxs-lookup"><span data-stu-id="1242a-192">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="1242a-193">*wwwroot/ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="1242a-193">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="1242a-194">При выборе переключателя **статический метод ретурнаррайасинк .NET** изучите выходные данные консоли в средствах веб-разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="1242a-194">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="1242a-195">Выходные данные консоли:</span><span class="sxs-lookup"><span data-stu-id="1242a-195">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="1242a-196">Четвертое значение массива помещается в массив (`data.push(4);`), возвращаемый методом. `ReturnArrayAsync`</span><span class="sxs-lookup"><span data-stu-id="1242a-196">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="1242a-197">Вызов метода экземпляра</span><span class="sxs-lookup"><span data-stu-id="1242a-197">Instance method call</span></span>

<span data-ttu-id="1242a-198">Кроме того, можно вызывать методы экземпляра .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-198">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="1242a-199">Вызов метода экземпляра .NET из JavaScript:</span><span class="sxs-lookup"><span data-stu-id="1242a-199">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="1242a-200">Передайте экземпляр .NET в JavaScript, заключив его в `DotNetObjectReference` экземпляр.</span><span class="sxs-lookup"><span data-stu-id="1242a-200">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="1242a-201">Экземпляр .NET передается по ссылке на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-201">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="1242a-202">Вызов методов экземпляра .NET для экземпляра с помощью `invokeMethod` функций или. `invokeMethodAsync`</span><span class="sxs-lookup"><span data-stu-id="1242a-202">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="1242a-203">Экземпляр .NET можно также передать в качестве аргумента при вызове других методов .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1242a-203">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="1242a-204">Пример приложения записывает сообщения в консоль на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="1242a-204">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="1242a-205">В следующих примерах, продемонстрированных в примере приложения, изучите выходные данные консоли браузера в средствах разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="1242a-205">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="1242a-206">Когда выбран **метод триггера экземпляра .NET хеллохелпер. SayHello** , `ExampleJsInterop.CallHelloHelperSayHello` он вызывается и `Blazor`передает имя в метод.</span><span class="sxs-lookup"><span data-stu-id="1242a-206">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="1242a-207">*Pages/жсинтероп. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1242a-207">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="1242a-208">`CallHelloHelperSayHello`вызывает функцию `sayHello` JavaScript с новым `HelloHelper`экземпляром.</span><span class="sxs-lookup"><span data-stu-id="1242a-208">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="1242a-209">*Жсинтеропклассес/ексамплежсинтероп. CS*:</span><span class="sxs-lookup"><span data-stu-id="1242a-209">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="1242a-210">*wwwroot/ексамплежсинтероп. js*:</span><span class="sxs-lookup"><span data-stu-id="1242a-210">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="1242a-211">Имя передается `HelloHelper`конструктору, который `HelloHelper.Name` задает свойство.</span><span class="sxs-lookup"><span data-stu-id="1242a-211">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="1242a-212">При `sayHello` `Hello, {Name}!` выполнении функции JavaScript возвращаетсообщение,котороезаписываетсявконсольфункциейJavaScript.`HelloHelper.SayHello`</span><span class="sxs-lookup"><span data-stu-id="1242a-212">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="1242a-213">*Жсинтеропклассес/хеллохелпер. CS*:</span><span class="sxs-lookup"><span data-stu-id="1242a-213">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="1242a-214">Выходные данные консоли в средствах веб-разработчика браузера:</span><span class="sxs-lookup"><span data-stu-id="1242a-214">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="1242a-215">Совместное использование кода взаимодействия в библиотеке классов</span><span class="sxs-lookup"><span data-stu-id="1242a-215">Share interop code in a class library</span></span>

<span data-ttu-id="1242a-216">Код взаимодействия JavaScript может быть добавлен в библиотеку классов, которая позволяет совместно использовать код в пакете NuGet.</span><span class="sxs-lookup"><span data-stu-id="1242a-216">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="1242a-217">Библиотека классов обрабатывает внедрение ресурсов JavaScript в построенную сборку.</span><span class="sxs-lookup"><span data-stu-id="1242a-217">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="1242a-218">Файлы JavaScript помещаются в папку *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="1242a-218">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="1242a-219">Инструментарий выполняет внедрение ресурсов при построении библиотеки.</span><span class="sxs-lookup"><span data-stu-id="1242a-219">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="1242a-220">В файле проекта приложения имеется ссылка на созданный пакет NuGet так же, как и на любой пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="1242a-220">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="1242a-221">После восстановления пакета код приложения может вызывать JavaScript, как если бы он был C#.</span><span class="sxs-lookup"><span data-stu-id="1242a-221">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="1242a-222">Дополнительные сведения см. в разделе <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="1242a-222">For more information, see <xref:blazor/class-libraries>.</span></span>
