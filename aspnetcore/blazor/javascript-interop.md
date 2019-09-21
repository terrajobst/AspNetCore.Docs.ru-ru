---
title: ASP.NET Core взаимодействие JavaScript Блазор
author: guardrex
description: Узнайте, как вызывать функции JavaScript из методов .NET и .NET из JavaScript в приложениях Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: blazor/javascript-interop
ms.openlocfilehash: aee9b981349e62dcc7ccf352dd5bab520969ed3b
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168198"
---
# <a name="aspnet-core-blazor-javascript-interop"></a>ASP.NET Core взаимодействие JavaScript Блазор

[Хавьер Калварро Воронков](https://github.com/javiercn), [Даниэль Roth)](https://github.com/danroth27)и [Люк ЛаСаМ](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Приложение Блазор может вызывать функции JavaScript из кода JavaScript в методах .NET и .NET.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Вызов функций JavaScript из методов .NET

В некоторых случаях для вызова функции JavaScript требуется код .NET. Например, вызов JavaScript может предоставлять приложению возможности браузера или функциональные возможности из библиотеки JavaScript.

Чтобы вызвать JavaScript из .NET, используйте `IJSRuntime` абстракцию. `InvokeAsync<T>` Метод принимает идентификатор для функции JavaScript, которую нужно вызвать вместе с любым числом аргументов, сериализуемых в формат JSON. Идентификатор функции является относительным к глобальной области (`window`). Если вы хотите вызвать `window.someScope.someFunction`, идентификатор имеет `someScope.someFunction`значение. Нет необходимости регистрировать функцию перед ее вызовом. Возвращаемый тип `T` также должен быть сериализуемым в формат JSON.

Для серверных приложений Блазор:

* Приложение Блазор Server обрабатывает несколько запросов пользователей. Не вызывайте `JSRuntime.Current` в компоненте для вызова функций JavaScript.
* `IJSRuntime` Внедрить абстракцию и использовать внедренный объект для выдаче вызовов взаимодействия JavaScript.
* Во время предварительной отрисовки приложения Блазор вызов JavaScript невозможен, поскольку не было установлено соединение с браузером. Дополнительные сведения см. в разделе [Обнаружение времени подготовки приложения блазор](#detect-when-a-blazor-app-is-prerendering) .

Следующий пример основан на [текстдекодер](https://developer.mozilla.org/docs/Web/API/TextDecoder), экспериментальном декодере на основе JavaScript. В примере показано, как вызвать функцию JavaScript из C# метода. Функция JavaScript принимает массив байтов из C# метода, декодирует массив и возвращает текст компоненту для вывода.

Внутри элемента wwwroot/index.HTML (блазор Assembly) или *pages/_Host. cshtml* (блазор Server) Укажите функцию, которая использует `TextDecoder` для декодирования переданного массива: `<head>`

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

Код JavaScript, например код, показанный в предыдущем примере, можно также загрузить из файла JavaScript ( *. js*) со ссылкой на файл скрипта:

```html
<script src="exampleJsInterop.js"></script>
```

Следующий компонент:

* Вызывает функцию JavaScript `ConvertArray` , используя `JsRuntime` , когда выбрана кнопка компонента (**преобразование массива**).
* После вызова функции JavaScript переданный массив преобразуется в строку. Строка возвращается компоненту для вывода.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

Чтобы использовать `IJSRuntime` абстракцию, следует принять любой из следующих подходов:

* Внедрить абстракцию в компонент Razor ( *. Razor*): `IJSRuntime`

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* Внедрить абстракцию в класс (CS): `IJSRuntime`

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* Для создания динамического содержимого с помощью [буилдрендертри](xref:blazor/components#manual-rendertreebuilder-logic)используйте `[Inject]` атрибут:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

В примере приложения на стороне клиента, прилагаемом к этой теме, для приложения, которое взаимодействует с моделью DOM для получения вводимых пользователем данных и вывода приветственного сообщения, доступны две функции JavaScript.

* `showPrompt`&ndash; Создает запрос на ввод пользователя (имя пользователя) и возвращает имя вызывающему объекту.
* `displayWelcome`Назначает сообщение приветствия от вызывающего объекта модели DOM объекту `id` с `welcome`. &ndash;

*wwwroot/ексамплежсинтероп. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Поместите тег, который ссылается на файл JavaScript, в файл *wwwroot/index.HTML* (блазор) или *pages/_Host. cshtml* (блазор Server). `<script>`

*wwwroot/index.HTML* (Блазорная сборка):

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (Блазор сервер):

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

Не размещайте `<script>` тег в файле компонента, `<script>` так как тег не может быть обновлен динамически.

Методы .NET взаимодействует с функциями JavaScript в файле *ексамплежсинтероп. js* путем вызова `IJSRuntime.InvokeAsync<T>`.

`IJSRuntime` Абстракция является асинхронной, чтобы разрешить сценарии сервера блазор. Если приложение является блазор приложением для сборки и необходимо вызвать функцию JavaScript синхронно, то приведение к `IJSInProcessRuntime` методу и вызов `Invoke<T>` вместо него. Рекомендуется, чтобы большинство библиотек взаимодействия JavaScript использовали асинхронные интерфейсы API, чтобы обеспечить доступность библиотек во всех сценариях.

Пример приложения содержит компонент для демонстрации взаимодействия JavaScript. Компонент:

* Получает входные данные пользователя с помощью командной строки JavaScript.
* Возвращает текст компонента для обработки.
* Вызывает вторую функцию JavaScript, которая взаимодействует с моделью DOM для вывода приветственного сообщения.

*Pages/жсинтероп. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Когда `TriggerJsPrompt` выполняется при нажатии кнопки **командной строки триггера JavaScript** компонента, вызывается `showPrompt` функция JavaScript, предоставленная в файле *wwwroot/ексамплежсинтероп. js* .
1. `showPrompt` Функция принимает ввод пользователя (имя пользователя), который кодируется в формате HTML и возвращается компоненту. Компонент сохраняет имя пользователя в локальной переменной `name`.
1. Строка, хранимая `name` в, включается в приветственное сообщение, которое передается `displayWelcome`функции JavaScript, которая визуализирует приветственное сообщение в тег заголовка.

## <a name="call-a-void-javascript-function"></a>Вызов функции JavaScript void

Функции JavaScript, возвращающие [значение void (0)/воид 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) или [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined), `IJSRuntime.InvokeAsync<object>`вызываются с `null`параметром, который возвращает.

## <a name="detect-when-a-blazor-app-is-prerendering"></a>Обнаружение времени предварительной отрисовки приложения Блазор
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Запись ссылок на элементы

Для некоторых сценариев [взаимодействия JavaScript](xref:blazor/javascript-interop) требуются ссылки на элементы HTML. Например, библиотеке пользовательского интерфейса может требоваться ссылка на элемент для инициализации, или же может потребоваться вызвать API-интерфейсы, подобные Command, в элементе, например `focus` или `play`.

Запишите ссылки на элементы HTML в компоненте, используя следующий подход:

* `@ref` Добавьте атрибут к элементу HTML.
* Определите поле типа `ElementReference` , имя которого соответствует значению `@ref` атрибута.

В следующем примере показана запись ссылки на `username` `<input>` элемент:

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> **Не** используйте захваченные ссылки на элементы в качестве способа заполнения или управления моделью DOM, когда блазор взаимодействует с элементами, на которые имеются ссылки. Это может повлиять на модель декларативной отрисовки.

Что касается кода .NET, `ElementReference` то является непрозрачным маркером. *Единственное* , что можно сделать с помощью `ElementReference` , — передать его в код JavaScript через взаимодействие JavaScript. При этом код на стороне JavaScript получает `HTMLElement` экземпляр, который можно использовать с обычными API DOM.

Например, следующий код определяет метод расширения .NET, который позволяет установить фокус на элемент:

*ексамплежсинтероп. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Используйте `IJSRuntime.InvokeAsync<T>` и вызовите `exampleJsFunctions.focusElement` с `ElementReference` помощью, чтобы сосредоточиться на элементе:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Чтобы использовать метод расширения для фокусировки элемента, создайте статический метод расширения, который получает `IJSRuntime` экземпляр:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Метод вызывается непосредственно для объекта. В следующем примере предполагается, что `Focus` статический метод доступен `JsInteropClasses` из пространства имен:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> `username` Переменная заполняется только после подготовки компонента к просмотру. Если незаполненный `ElementReference` объект передается в код JavaScript, код JavaScript получает `null`значение. Для управления ссылками на элементы после завершения отрисовки компонента (для установки начального фокуса на элемент) используйте `OnAfterRenderAsync` [методы жизненного цикла компонента](xref:blazor/components#lifecycle-methods)или. `OnAfterRender`

## <a name="invoke-net-methods-from-javascript-functions"></a>Вызов методов .NET из функций JavaScript

### <a name="static-net-method-call"></a>Статический вызов метода .NET

Чтобы вызвать статический метод .NET из JavaScript, используйте `DotNet.invokeMethod` функции или. `DotNet.invokeMethodAsync` Передайте идентификатор статического метода, который необходимо вызвать, имя сборки, содержащей функцию, и любые аргументы. Асинхронная версия является предпочтительной для поддержки сценариев сервера Блазор. Чтобы вызвать метод .NET из JavaScript, метод .NET должен быть открытым, статическим и иметь `[JSInvokable]` атрибут. По умолчанию идентификатором метода является имя метода, но можно указать другой идентификатор с помощью `JSInvokableAttribute` конструктора. Вызов открытых универсальных методов в настоящее время не поддерживается.

Пример приложения включает C# метод, возвращающий массив типа `int`. `JSInvokable` Атрибут применяется к методу.

*Pages/жсинтероп. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

JavaScript, предоставляемый клиенту, вызывает метод C# .NET.

*wwwroot/ексамплежсинтероп. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

При выборе переключателя **статический метод ретурнаррайасинк .NET** изучите выходные данные консоли в средствах веб-разработчика браузера.

Выходные данные консоли:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Четвертое значение массива помещается в массив (`data.push(4);`), возвращаемый методом. `ReturnArrayAsync`

### <a name="instance-method-call"></a>Вызов метода экземпляра

Кроме того, можно вызывать методы экземпляра .NET из JavaScript. Вызов метода экземпляра .NET из JavaScript:

* Передайте экземпляр .NET в JavaScript, заключив его в `DotNetObjectReference` экземпляр. Экземпляр .NET передается по ссылке на JavaScript.
* Вызов методов экземпляра .NET для экземпляра с помощью `invokeMethod` функций или. `invokeMethodAsync` Экземпляр .NET можно также передать в качестве аргумента при вызове других методов .NET из JavaScript.

> [!NOTE]
> Пример приложения записывает сообщения в консоль на стороне клиента. В следующих примерах, продемонстрированных в примере приложения, изучите выходные данные консоли браузера в средствах разработчика браузера.

Когда выбран **метод триггера экземпляра .NET хеллохелпер. SayHello** , `ExampleJsInterop.CallHelloHelperSayHello` он вызывается и `Blazor`передает имя в метод.

*Pages/жсинтероп. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello`вызывает функцию `sayHello` JavaScript с новым `HelloHelper`экземпляром.

*Жсинтеропклассес/ексамплежсинтероп. CS*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/ексамплежсинтероп. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Имя передается `HelloHelper`конструктору, который `HelloHelper.Name` задает свойство. При `sayHello` `Hello, {Name}!` выполнении функции JavaScript возвращаетсообщение,котороезаписываетсявконсольфункциейJavaScript.`HelloHelper.SayHello`

*Жсинтеропклассес/хеллохелпер. CS*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Выходные данные консоли в средствах веб-разработчика браузера:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Совместное использование кода взаимодействия в библиотеке классов

Код взаимодействия JavaScript может быть добавлен в библиотеку классов, которая позволяет совместно использовать код в пакете NuGet.

Библиотека классов обрабатывает внедрение ресурсов JavaScript в построенную сборку. Файлы JavaScript помещаются в папку *wwwroot* . Инструментарий выполняет внедрение ресурсов при построении библиотеки.

В файле проекта приложения имеется ссылка на созданный пакет NuGet так же, как и на любой пакет NuGet. После восстановления пакета код приложения может вызывать JavaScript, как если бы он был C#.

Дополнительные сведения см. в разделе <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Вызовы взаимодействия с зафиксированным JS

Взаимодействие с JS может завершиться сбоем из-за ошибок сети и должно рассматриваться как ненадежное. По умолчанию серверное приложение Блазор время ожидания вызовов взаимодействия JS на сервере через одну минуту. Если приложение может допускать более агрессивное время ожидания, например 10 секунд, задайте время ожидания одним из следующих способов.

* В `Startup.ConfigureServices`глобальном масштабе укажите время ожидания:

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* Для каждого вызова в коде компонента один вызов может указать время ожидания:

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

Дополнительные сведения об исчерпании ресурсов см. в <xref:security/blazor/server>разделе.
