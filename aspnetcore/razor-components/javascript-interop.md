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
# <a name="razor-components-javascript-interop"></a>Взаимодействие компонентов JavaScript в Razor

По [Нельсон Calvarro Хавьер](https://github.com/javiercn), [Дэниэл рот](https://github.com/danroth27), и [Люк Лэтем](https://github.com/guardrex)

В приложение Razor компоненты могут вызывать функции JavaScript, .NET, .NET методы из кода JavaScript.

## <a name="invoke-javascript-functions-from-net-methods"></a>Вызова функций JavaScript из методов .NET

Бывают случаи, когда код .NET не требуется для вызова функции JavaScript. Например вызов JavaScript может предоставлять возможности браузера или функции из библиотеки JavaScript в приложение.

Чтобы вызывать JavaScript с помощью .NET, используйте `IJSRuntime` абстракции. `InvokeAsync<T>` Метод принимает идентификатор для функции JavaScript, которую нужно вызвать, наряду с любое количество аргументов, сериализуемые в формат JSON. Идентификатор функции задается относительно глобальной области (`window`). Если вы хотите вызвать `window.someScope.someFunction`, идентификатор является `someScope.someFunction`. Нет необходимости зарегистрировать функцию перед его вызовом. Тип возвращаемого значения `T` также должны быть JSON сериализуемыми.

Для приложений ASP.NET Core Razor компоненты на стороне сервера:

* Нескольких пользовательских запросов обрабатываются приложением на стороне сервера. Не вызывайте `JSRuntime.Current` в компоненте для вызова функций JavaScript.
* Внедрить `IJSRuntime` абстракции и использование внедренного объекта взаимодействия вызовов JavaScript.

Следующий пример построен на основе [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), экспериментальный декодера, на основе JavaScript. В примере для вызова функции JavaScript из C# метод. Функция JavaScript, которая принимает массив байтов из C# метод, декодирует массива и возвращает текст в компонент для отображения.

Внутри `<head>` элемент *wwwroot/index.html*, предоставляют функции, которая использует `TextDecoder` для декодирования переданный массив:

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

Код JavaScript, например код, показанный в предыдущем примере, также может быть загружено из файла JavaScript (*.js*) со ссылкой на файл скрипта в *wwwroot/index.html* файла:

```html
<script src="exampleJsInterop.js"></script>
```

Следующие компоненты:

* Вызывает `ConvertArray` функцию JavaScript с помощью `JsRuntime` при кнопки компонент (**преобразовать массив**) выбран.
* После вызова функции JavaScript, переданный массив преобразуется в строку. Строка возвращается к компоненту для отображения.

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

Чтобы использовать `IJSRuntime` абстракции, применять любой из следующих подходов:

* Внедрить `IJSRuntime` абстракции в файле Razor (*.razor*, *.cshtml*):

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

* Внедрить `IJSRuntime` абстракции в класс (*.cs*):

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

* Для динамического создания содержимого с `BuildRenderTree`, использовать `[Inject]` атрибут:

  ```csharp
  [Inject] IJSRuntime JSRuntime { get; set; }
  ```

В примере клиентского приложения, используемый в этом разделе доступны две функции JavaScript для клиентского приложения, взаимодействия с DOM для ввода данных пользователем и отображения приветственное сообщение:

* `showPrompt` &ndash; Выводит приглашение на ввод пользователя (имя пользователя) и возвращает имя вызывающей стороне.
* `displayWelcome` &ndash; Назначает приветственное сообщение от вызывающего объекта DOM с `id` из `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Место `<script>` тег, который ссылается на файл JavaScript в *wwwroot/index.html* файла:

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

Не размещайте `<script>` тег в файле компонента, так как `<script>` тега не может обновляться динамически.

Функции взаимодействия методов .NET с помощью JavaScript в *exampleJsInterop.js* файл путем вызова `IJSRuntime.InvokeAsync<T>`.

`IJSRuntime` Абстракции является асинхронным, разрешающие сценарии на стороне сервера. Если приложение выполняется на стороне клиента и нужно вызвать функцию JavaScript синхронно, получено нисходящим `IJSInProcessRuntime` и вызвать `Invoke<T>` вместо этого. Рекомендуется использовать большинство взаимодействия библиотек JavaScript асинхронные интерфейсы API, чтобы убедиться, что эти библиотеки доступны во всех сценариях, клиентские или на сервере.

Пример приложения включает компонент для демонстрации взаимодействия JavaScript. Компонент:

* Получает входные данные пользователя с помощью строки JavaScript.
* Возвращает текст к компоненту для обработки.
* Вызывает второй функции JavaScript, которая взаимодействует с моделью DOM для отображения приветственного сообщения.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. При `TriggerJsPrompt` выполняется путем выбора компонента **триггера JavaScript Prompt** кнопку JavaScript `showPrompt` функция, предоставленная в *wwwroot/exampleJsInterop.js* файл вызывается.
1. `showPrompt` Функция принимает вводимые пользователем данные (имя пользователя), являющееся HTML-кодирование и возвращаются к компоненту. Компонент сохраняет имя пользователя в локальной переменной, `name`.
1. Строки хранятся в `name` является частью приветственное сообщение, которая передается в функцию JavaScript, `displayWelcome`, который отображает приветственное сообщение в тег заголовка.

## <a name="capture-references-to-elements"></a>Захват ссылок на элементы

Некоторые [взаимодействия JavaScript](xref:razor-components/javascript-interop) сценарии требуют ссылки на элементы HTML. Например, библиотека пользовательского интерфейса может потребоваться ссылки на элемент для инициализации, или может потребоваться вызвать подобные API-интерфейсы на элементе, например `focus` или `play`.

Ссылки на элементы HTML в компоненте можно записать, добавив `ref` атрибут к элементу HTML, а затем определив поле типа `ElementRef` , имя которого совпадает со значением `ref` атрибута.

В следующем примере показано, захват ссылку на элемент ввода имени пользователя:

```cshtml
<input ref="username" ...>

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Сделать **не** использовать ссылки на элементы, записанный как способ заполнения модели DOM. Это может помешать модели для декларативной подготовки отчетов.

Что касается кода .NET является, `ElementRef` – это прозрачный дескриптор. *Только* , что можно сделать с помощью `ElementRef` является его передачи в код JavaScript с помощью взаимодействия JavaScript. При этом код JavaScript на стороне получает `HTMLElement` экземпляр, который его можно использовать с обычной интерфейсы API модели DOM.

Например следующий код определяет метод расширения .NET, который позволяет установить фокус на элементе:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Используйте `IJSRuntime.InvokeAsync<T>` и вызвать `exampleJsFunctions.focusElement` с `ElementRef` сосредоточиться элемент:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.cshtml?highlight=1,3,7,11-12)]

Чтобы использовать метод расширения, чтобы сосредоточиться на элемент, создайте статический метод расширения, получающий `IJSRuntime` экземпляр:

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Метод вызывается непосредственно в объекте. В следующем примере предполагается, что статический `Focus` метод доступен из `JsInteropClasses` пространство имен:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.cshtml?highlight=1,4,8,12)]

> [!IMPORTANT]
> `username` Переменной заполняется только после того как компонент осуществляет визуализацию и его выходные данные содержат `>` элемент. Если при попытке передать другой незаполненной `ElementRef` в код JavaScript код JavaScript получает `null`. Для управления ссылки на элементы, после завершения отрисовки (чтобы устанавливать исходный фокус в элементе) используйте компонент `OnAfterRenderAsync` или `OnAfterRender` [методы жизненного цикла компонент](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Вызов методов .NET из функции JavaScript

### <a name="static-net-method-call"></a>Статический вызов метода .NET

Чтобы вызвать статический метод .NET из JavaScript, используйте `DotNet.invokeMethod` или `DotNet.invokeMethodAsync` функции. Необходимо передать идентификатор статического метода, который вы хотите вызвать, имя сборки, содержащей функцию и все аргументы. Асинхронная версия является предпочтительным для поддержки сценариев на стороне сервера. Чтобы иметь могут вызываться из JavaScript, метод .NET должен быть общедоступный статический и декорированные с `[JSInvokable]`. По умолчанию идентификатор метода — это имя метода, но можно указать другой идентификатор с помощью `JSInvokableAttribute` конструктор. Вызов открытые универсальные методы сейчас не поддерживается.

Образец приложения включает C# метод, чтобы вернуть массив `int`s. Метод дополняется `JSInvokable` атрибута.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop2&highlight=7-11)]

Клиенту JavaScript вызывает C# метода .NET.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Когда **статический метод .NET триггера ReturnArrayAsync** выбрана кнопка, изучите выходные данные консоли в веб-инструменты разработчиков обозревателя.

Консоль будет:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Четвертое значение массива передается в массив (`data.push(4);`) возвращаемый `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Вызов метода экземпляра

Также можно вызывать методы экземпляра .NET из JavaScript. Для вызова экземпляра метода .NET из JavaScript, сначала пройти экземпляр .NET в JavaScript, если поместить его в `DotNetObjectRef` экземпляра. Экземпляр .NET передается по ссылке на JavaScript, и можно вызывать методами экземпляра .NET для экземпляра с помощью `invokeMethod` или `invokeMethodAsync` функции. Экземпляр .NET также можно передать в качестве аргумента, при вызове других методов .NET из JavaScript.

> [!NOTE]
> Пример приложения регистрирует сообщения в консоль на стороне клиента. Для приведенных ниже примерах демонстрируется в примере приложения проверьте выходные данные консоли браузера в средствах разработчика браузера.

При **метод экземпляра триггера .NET HelloHelper.SayHello** выбран переключатель, `ExampleJsInterop.CallHelloHelperSayHello` вызывается и передает имя, `Blazor`, в метод.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` вызывает функцию JavaScript `sayHello` для нового экземпляра `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Передается имя `HelloHelper`в конструктор, который задает `HelloHelper.Name` свойства. Когда функция JavaScript, которая `sayHello` выполняется, `HelloHelper.SayHello` возвращает `Hello, {Name}!` сообщения, которое записывается в консоль с помощью функции JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Выходные данные в браузере веб-инструменты разработчиков консоли:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Совместное использование взаимодействия кода в библиотеку классов для компонента Razor

Взаимодействия кода JavaScript, которые могут быть включены в библиотеку классов для компонента Razor (`dotnet new razorclasslib`), который позволяет совместно использовать код в виде пакета NuGet.

Библиотеки классов Razor компонент обрабатывает внедрения ресурсов JavaScript в построенной сборке. Файлы JavaScript, помещаются в *wwwroot* папки. Инструментарий отвечает за внедрение ресурсов при построении библиотеки.

Встроенный пакет NuGet есть ссылки в файле проекта приложения, так же, как используется ссылка на любой обычный пакет NuGet. После восстановления приложения, код приложения может вызывать в JavaScript, как если бы он был C#.

Дополнительные сведения см. в разделе <xref:razor-components/class-libraries>.
