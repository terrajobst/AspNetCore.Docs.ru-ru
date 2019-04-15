---
title: Взаимодействие компонентов JavaScript в Razor
author: guardrex
description: Узнайте, как вызывать функции JavaScript, .NET, .NET методы из JavaScript в приложениях Blazor и компоненты Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: f2588f4ed1ec2f01218283625fae4632d0a8ae58
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515680"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="3a03b-103">Взаимодействие компонентов JavaScript в Razor</span><span class="sxs-lookup"><span data-stu-id="3a03b-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="3a03b-104">По [Нельсон Calvarro Хавьер](https://github.com/javiercn), [Дэниэл рот](https://github.com/danroth27), и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3a03b-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3a03b-105">В приложение Razor компоненты могут вызывать функции JavaScript, .NET, .NET методы из кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="3a03b-106">Вызова функций JavaScript из методов .NET</span><span class="sxs-lookup"><span data-stu-id="3a03b-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="3a03b-107">Бывают случаи, когда код .NET не требуется для вызова функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="3a03b-108">Например вызов JavaScript может предоставлять возможности браузера или функции из библиотеки JavaScript в приложение.</span><span class="sxs-lookup"><span data-stu-id="3a03b-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="3a03b-109">Чтобы вызывать JavaScript с помощью .NET, используйте `IJSRuntime` абстракции.</span><span class="sxs-lookup"><span data-stu-id="3a03b-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="3a03b-110">`InvokeAsync<T>` Метод принимает идентификатор для функции JavaScript, которую нужно вызвать, наряду с любое количество аргументов, сериализуемые в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="3a03b-110">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="3a03b-111">Идентификатор функции задается относительно глобальной области (`window`).</span><span class="sxs-lookup"><span data-stu-id="3a03b-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="3a03b-112">Если вы хотите вызвать `window.someScope.someFunction`, идентификатор является `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="3a03b-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="3a03b-113">Нет необходимости зарегистрировать функцию перед его вызовом.</span><span class="sxs-lookup"><span data-stu-id="3a03b-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="3a03b-114">Тип возвращаемого значения `T` также должны быть JSON сериализуемыми.</span><span class="sxs-lookup"><span data-stu-id="3a03b-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="3a03b-115">Для приложений ASP.NET Core Razor компоненты на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="3a03b-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="3a03b-116">Нескольких пользовательских запросов обрабатываются приложением на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="3a03b-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="3a03b-117">Не вызывайте `JSRuntime.Current` в компоненте для вызова функций JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="3a03b-118">Внедрить `IJSRuntime` абстракции и использование внедренного объекта взаимодействия вызовов JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="3a03b-119">Следующий пример построен на основе [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), экспериментальный декодера, на основе JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="3a03b-120">В примере для вызова функции JavaScript из C# метод.</span><span class="sxs-lookup"><span data-stu-id="3a03b-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="3a03b-121">Функция JavaScript, которая принимает массив байтов из C# метод, декодирует массива и возвращает текст в компонент для отображения.</span><span class="sxs-lookup"><span data-stu-id="3a03b-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="3a03b-122">Внутри `<head>` элемент *wwwroot/index.html*, предоставляют функции, которая использует `TextDecoder` для декодирования переданный массив:</span><span class="sxs-lookup"><span data-stu-id="3a03b-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

<span data-ttu-id="3a03b-123">Код JavaScript, например код, показанный в предыдущем примере, также может быть загружено из файла JavaScript (*.js*) со ссылкой на файл скрипта в *wwwroot/index.html* файла:</span><span class="sxs-lookup"><span data-stu-id="3a03b-123">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file in the *wwwroot/index.html* file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="3a03b-124">Следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="3a03b-124">The following component:</span></span>

* <span data-ttu-id="3a03b-125">Вызывает `ConvertArray` функцию JavaScript с помощью `JsRuntime` при кнопки компонент (**преобразовать массив**) выбран.</span><span class="sxs-lookup"><span data-stu-id="3a03b-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="3a03b-126">После вызова функции JavaScript, переданный массив преобразуется в строку.</span><span class="sxs-lookup"><span data-stu-id="3a03b-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="3a03b-127">Строка возвращается к компоненту для отображения.</span><span class="sxs-lookup"><span data-stu-id="3a03b-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    private async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

<span data-ttu-id="3a03b-128">Чтобы использовать `IJSRuntime` абстракции, применять любой из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="3a03b-128">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="3a03b-129">Внедрить `IJSRuntime` абстракции в файле Razor (*.razor*, *.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="3a03b-129">Inject the `IJSRuntime` abstraction into the Razor file (*.razor*, *.cshtml*):</span></span>

  ```cshtml
  @inject IJSRuntime JSRuntime

  @functions {
      public override void OnInit()
      {
          StocksService.OnStockTickerUpdated += stockUpdate =>
          {
              JSRuntime.InvokeAsync<object>(
                  "handleTickerChanged",
                  stockUpdate.symbol,
                  stockUpdate.price);
          };
      }
  }
  ```

* <span data-ttu-id="3a03b-130">Внедрить `IJSRuntime` абстракции в класс (*.cs*):</span><span class="sxs-lookup"><span data-stu-id="3a03b-130">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  ```csharp
  public class JsInteropClasses
  {
      private readonly IJSRuntime _jsRuntime;

      public JsInteropClasses(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public Task<string> TickerChanged(string data)
      {
          // The handleTickerChanged JavaScript method is implemented
          // in a JavaScript file, such as 'wwwroot/tickerJsInterop.js'.
          return _jsRuntime.InvokeAsync<object>(
              "handleTickerChanged",
              stockUpdate.symbol,
              stockUpdate.price);
      }
  }
  ```

* <span data-ttu-id="3a03b-131">Для динамического создания содержимого с `BuildRenderTree`, использовать `[Inject]` атрибут:</span><span class="sxs-lookup"><span data-stu-id="3a03b-131">For dynamic content generation with `BuildRenderTree`, use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject] IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="3a03b-132">В примере клиентского приложения, используемый в этом разделе доступны две функции JavaScript для клиентского приложения, взаимодействия с DOM для ввода данных пользователем и отображения приветственное сообщение:</span><span class="sxs-lookup"><span data-stu-id="3a03b-132">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* `showPrompt` <span data-ttu-id="3a03b-133">&ndash; Выводит приглашение на ввод пользователя (имя пользователя) и возвращает имя вызывающей стороне.</span><span class="sxs-lookup"><span data-stu-id="3a03b-133">&ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* `displayWelcome` <span data-ttu-id="3a03b-134">&ndash; Назначает приветственное сообщение от вызывающего объекта DOM с `id` из `welcome`.</span><span class="sxs-lookup"><span data-stu-id="3a03b-134">&ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="3a03b-135">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="3a03b-135">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="3a03b-136">Место `<script>` тег, который ссылается на файл JavaScript в *wwwroot/index.html* файла:</span><span class="sxs-lookup"><span data-stu-id="3a03b-136">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="3a03b-137">Не размещайте `<script>` тег в файле компонента, так как `<script>` тега не может обновляться динамически.</span><span class="sxs-lookup"><span data-stu-id="3a03b-137">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="3a03b-138">Функции взаимодействия методов .NET с помощью JavaScript в *exampleJsInterop.js* файл путем вызова `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="3a03b-138">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="3a03b-139">`IJSRuntime` Абстракции является асинхронным, разрешающие сценарии на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="3a03b-139">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="3a03b-140">Если приложение выполняется на стороне клиента и нужно вызвать функцию JavaScript синхронно, получено нисходящим `IJSInProcessRuntime` и вызвать `Invoke<T>` вместо этого.</span><span class="sxs-lookup"><span data-stu-id="3a03b-140">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="3a03b-141">Рекомендуется использовать большинство взаимодействия библиотек JavaScript асинхронные интерфейсы API, чтобы убедиться, что эти библиотеки доступны во всех сценариях, клиентские или на сервере.</span><span class="sxs-lookup"><span data-stu-id="3a03b-141">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="3a03b-142">Пример приложения включает компонент для демонстрации взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-142">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="3a03b-143">Компонент:</span><span class="sxs-lookup"><span data-stu-id="3a03b-143">The component:</span></span>

* <span data-ttu-id="3a03b-144">Получает входные данные пользователя с помощью строки JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-144">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="3a03b-145">Возвращает текст к компоненту для обработки.</span><span class="sxs-lookup"><span data-stu-id="3a03b-145">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="3a03b-146">Вызывает второй функции JavaScript, которая взаимодействует с моделью DOM для отображения приветственного сообщения.</span><span class="sxs-lookup"><span data-stu-id="3a03b-146">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="3a03b-147">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3a03b-147">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="3a03b-148">При `TriggerJsPrompt` выполняется путем выбора компонента **триггера JavaScript Prompt** кнопку JavaScript `showPrompt` функция, предоставленная в *wwwroot/exampleJsInterop.js* файл вызывается.</span><span class="sxs-lookup"><span data-stu-id="3a03b-148">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="3a03b-149">`showPrompt` Функция принимает вводимые пользователем данные (имя пользователя), являющееся HTML-кодирование и возвращаются к компоненту.</span><span class="sxs-lookup"><span data-stu-id="3a03b-149">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="3a03b-150">Компонент сохраняет имя пользователя в локальной переменной, `name`.</span><span class="sxs-lookup"><span data-stu-id="3a03b-150">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="3a03b-151">Строки хранятся в `name` является частью приветственное сообщение, которая передается в функцию JavaScript, `displayWelcome`, который отображает приветственное сообщение в тег заголовка.</span><span class="sxs-lookup"><span data-stu-id="3a03b-151">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="3a03b-152">Захват ссылок на элементы</span><span class="sxs-lookup"><span data-stu-id="3a03b-152">Capture references to elements</span></span>

<span data-ttu-id="3a03b-153">Некоторые [взаимодействия JavaScript](xref:razor-components/javascript-interop) сценарии требуют ссылки на элементы HTML.</span><span class="sxs-lookup"><span data-stu-id="3a03b-153">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="3a03b-154">Например, библиотека пользовательского интерфейса может потребоваться ссылки на элемент для инициализации, или может потребоваться вызвать подобные API-интерфейсы на элементе, например `focus` или `play`.</span><span class="sxs-lookup"><span data-stu-id="3a03b-154">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="3a03b-155">Ссылки на элементы HTML в компоненте можно записать, добавив `ref` атрибут к элементу HTML, а затем определив поле типа `ElementRef` , имя которого совпадает со значением `ref` атрибута.</span><span class="sxs-lookup"><span data-stu-id="3a03b-155">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="3a03b-156">В следующем примере показано, захват ссылку на элемент ввода имени пользователя:</span><span class="sxs-lookup"><span data-stu-id="3a03b-156">The following example shows capturing a reference to the username input element:</span></span>

```cshtml
<input ref="username" ...>

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="3a03b-157">Сделать **не** использовать ссылки на элементы, записанный как способ заполнения модели DOM.</span><span class="sxs-lookup"><span data-stu-id="3a03b-157">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="3a03b-158">Это может помешать модели для декларативной подготовки отчетов.</span><span class="sxs-lookup"><span data-stu-id="3a03b-158">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="3a03b-159">Что касается кода .NET является, `ElementRef` – это прозрачный дескриптор.</span><span class="sxs-lookup"><span data-stu-id="3a03b-159">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="3a03b-160">*Только* , что можно сделать с помощью `ElementRef` является его передачи в код JavaScript с помощью взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-160">The *only* thing you can do with `ElementRef` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="3a03b-161">При этом код JavaScript на стороне получает `HTMLElement` экземпляр, который его можно использовать с обычной интерфейсы API модели DOM.</span><span class="sxs-lookup"><span data-stu-id="3a03b-161">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="3a03b-162">Например следующий код определяет метод расширения .NET, который позволяет установить фокус на элементе:</span><span class="sxs-lookup"><span data-stu-id="3a03b-162">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="3a03b-163">*exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="3a03b-163">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="3a03b-164">Используйте `IJSRuntime.InvokeAsync<T>` и вызвать `exampleJsFunctions.focusElement` с `ElementRef` сосредоточиться элемент:</span><span class="sxs-lookup"><span data-stu-id="3a03b-164">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementRef` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.cshtml?highlight=1,3,7,11-12)]

<span data-ttu-id="3a03b-165">Чтобы использовать метод расширения, чтобы сосредоточиться на элемент, создайте статический метод расширения, получающий `IJSRuntime` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="3a03b-165">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="3a03b-166">Метод вызывается непосредственно в объекте.</span><span class="sxs-lookup"><span data-stu-id="3a03b-166">The method is called directly on the object.</span></span> <span data-ttu-id="3a03b-167">В следующем примере предполагается, что статический `Focus` метод доступен из `JsInteropClasses` пространство имен:</span><span class="sxs-lookup"><span data-stu-id="3a03b-167">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.cshtml?highlight=1,4,8,12)]

> [!IMPORTANT]
> <span data-ttu-id="3a03b-168">`username` Переменной заполняется только после того как компонент осуществляет визуализацию и его выходные данные содержат `>` элемент.</span><span class="sxs-lookup"><span data-stu-id="3a03b-168">The `username` variable is only populated after the component renders and its output includes the `>` element.</span></span> <span data-ttu-id="3a03b-169">Если при попытке передать другой незаполненной `ElementRef` в код JavaScript код JavaScript получает `null`.</span><span class="sxs-lookup"><span data-stu-id="3a03b-169">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="3a03b-170">Для управления ссылки на элементы, после завершения отрисовки (чтобы устанавливать исходный фокус в элементе) используйте компонент `OnAfterRenderAsync` или `OnAfterRender` [методы жизненного цикла компонент](xref:razor-components/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="3a03b-170">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="3a03b-171">Вызов методов .NET из функции JavaScript</span><span class="sxs-lookup"><span data-stu-id="3a03b-171">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="3a03b-172">Статический вызов метода .NET</span><span class="sxs-lookup"><span data-stu-id="3a03b-172">Static .NET method call</span></span>

<span data-ttu-id="3a03b-173">Чтобы вызвать статический метод .NET из JavaScript, используйте `DotNet.invokeMethod` или `DotNet.invokeMethodAsync` функции.</span><span class="sxs-lookup"><span data-stu-id="3a03b-173">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="3a03b-174">Необходимо передать идентификатор статического метода, который вы хотите вызвать, имя сборки, содержащей функцию и все аргументы.</span><span class="sxs-lookup"><span data-stu-id="3a03b-174">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="3a03b-175">Асинхронная версия является предпочтительным для поддержки сценариев на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="3a03b-175">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="3a03b-176">Чтобы иметь могут вызываться из JavaScript, метод .NET должен быть общедоступный статический и декорированные с `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="3a03b-176">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="3a03b-177">По умолчанию идентификатор метода — это имя метода, но можно указать другой идентификатор с помощью `JSInvokableAttribute` конструктор.</span><span class="sxs-lookup"><span data-stu-id="3a03b-177">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="3a03b-178">Вызов открытые универсальные методы сейчас не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="3a03b-178">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="3a03b-179">Образец приложения включает C# метод, чтобы вернуть массив `int`s.</span><span class="sxs-lookup"><span data-stu-id="3a03b-179">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="3a03b-180">Метод дополняется `JSInvokable` атрибута.</span><span class="sxs-lookup"><span data-stu-id="3a03b-180">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="3a03b-181">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3a03b-181">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="3a03b-182">Клиенту JavaScript вызывает C# метода .NET.</span><span class="sxs-lookup"><span data-stu-id="3a03b-182">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="3a03b-183">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="3a03b-183">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="3a03b-184">Когда **статический метод .NET триггера ReturnArrayAsync** выбрана кнопка, изучите выходные данные консоли в веб-инструменты разработчиков обозревателя.</span><span class="sxs-lookup"><span data-stu-id="3a03b-184">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="3a03b-185">Консоль будет:</span><span class="sxs-lookup"><span data-stu-id="3a03b-185">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="3a03b-186">Четвертое значение массива передается в массив (`data.push(4);`) возвращаемый `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="3a03b-186">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="3a03b-187">Вызов метода экземпляра</span><span class="sxs-lookup"><span data-stu-id="3a03b-187">Instance method call</span></span>

<span data-ttu-id="3a03b-188">Также можно вызывать методы экземпляра .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-188">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="3a03b-189">Для вызова экземпляра метода .NET из JavaScript, сначала пройти экземпляр .NET в JavaScript, если поместить его в `DotNetObjectRef` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="3a03b-189">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="3a03b-190">Экземпляр .NET передается по ссылке на JavaScript, и можно вызывать методами экземпляра .NET для экземпляра с помощью `invokeMethod` или `invokeMethodAsync` функции.</span><span class="sxs-lookup"><span data-stu-id="3a03b-190">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="3a03b-191">Экземпляр .NET также можно передать в качестве аргумента, при вызове других методов .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-191">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="3a03b-192">Пример приложения регистрирует сообщения в консоль на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="3a03b-192">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="3a03b-193">Для приведенных ниже примерах демонстрируется в примере приложения проверьте выходные данные консоли браузера в средствах разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="3a03b-193">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="3a03b-194">При **метод экземпляра триггера .NET HelloHelper.SayHello** выбран переключатель, `ExampleJsInterop.CallHelloHelperSayHello` вызывается и передает имя, `Blazor`, в метод.</span><span class="sxs-lookup"><span data-stu-id="3a03b-194">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="3a03b-195">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3a03b-195">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` <span data-ttu-id="3a03b-196">вызывает функцию JavaScript `sayHello` для нового экземпляра `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="3a03b-196">invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="3a03b-197">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="3a03b-197">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="3a03b-198">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="3a03b-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="3a03b-199">Передается имя `HelloHelper`в конструктор, который задает `HelloHelper.Name` свойства.</span><span class="sxs-lookup"><span data-stu-id="3a03b-199">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="3a03b-200">Когда функция JavaScript, которая `sayHello` выполняется, `HelloHelper.SayHello` возвращает `Hello, {Name}!` сообщения, которое записывается в консоль с помощью функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a03b-200">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="3a03b-201">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="3a03b-201">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="3a03b-202">Выходные данные в браузере веб-инструменты разработчиков консоли:</span><span class="sxs-lookup"><span data-stu-id="3a03b-202">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="3a03b-203">Совместное использование взаимодействия кода в библиотеку классов для компонента Razor</span><span class="sxs-lookup"><span data-stu-id="3a03b-203">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="3a03b-204">Взаимодействия кода JavaScript, которые могут быть включены в библиотеку классов для компонента Razor (`dotnet new razorclasslib`), который позволяет совместно использовать код в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="3a03b-204">JavaScript interop code can be included in a Razor Component class library (`dotnet new razorclasslib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="3a03b-205">Библиотеки классов Razor компонент обрабатывает внедрения ресурсов JavaScript в построенной сборке.</span><span class="sxs-lookup"><span data-stu-id="3a03b-205">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="3a03b-206">Файлы JavaScript, помещаются в *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="3a03b-206">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="3a03b-207">Инструментарий отвечает за внедрение ресурсов при построении библиотеки.</span><span class="sxs-lookup"><span data-stu-id="3a03b-207">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="3a03b-208">Встроенный пакет NuGet есть ссылки в файле проекта приложения, так же, как используется ссылка на любой обычный пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="3a03b-208">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="3a03b-209">После восстановления приложения, код приложения может вызывать в JavaScript, как если бы он был C#.</span><span class="sxs-lookup"><span data-stu-id="3a03b-209">After the app is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="3a03b-210">Дополнительные сведения см. в разделе <xref:razor-components/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="3a03b-210">For more information, see <xref:razor-components/class-libraries>.</span></span>
