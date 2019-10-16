---
title: ASP.NET Core взаимодействие JavaScript Блазор
author: guardrex
description: Узнайте, как вызывать функции JavaScript из методов .NET и .NET из JavaScript в приложениях Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/javascript-interop
ms.openlocfilehash: b4776a20c6da6c722d2c057d19863c570f530a21
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391068"
---
# <a name="aspnet-core-blazor-javascript-interop"></a>ASP.NET Core взаимодействие JavaScript Блазор

[Хавьер Калварро Воронков](https://github.com/javiercn), [Даниэль Roth)](https://github.com/danroth27)и [Люк ЛаСаМ](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Приложение Блазор может вызывать функции JavaScript из кода JavaScript в методах .NET и .NET.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Вызов функций JavaScript из методов .NET

В некоторых случаях для вызова функции JavaScript требуется код .NET. Например, вызов JavaScript может предоставлять приложению возможности браузера или функциональные возможности из библиотеки JavaScript.

Чтобы вызвать JavaScript из .NET, используйте абстракцию `IJSRuntime`. Метод `InvokeAsync<T>` принимает идентификатор для функции JavaScript, которую нужно вызвать вместе с любым числом аргументов, сериализуемых в формат JSON. Идентификатор функции является относительным к глобальной области (`window`). Если вы хотите вызвать `window.someScope.someFunction`, идентификатор будет `someScope.someFunction`. Нет необходимости регистрировать функцию перед ее вызовом. Возвращаемый тип `T` также должен быть сериализуемым в формат JSON.

Для серверных приложений Блазор:

* Приложение Блазор Server обрабатывает несколько запросов пользователей. Не вызывайте `JSRuntime.Current` в компоненте для вызова функций JavaScript.
* Вставьте абстракцию `IJSRuntime` и используйте внедренный объект для выдаче вызовов взаимодействия JavaScript.
* Во время предварительной отрисовки приложения Блазор вызов JavaScript невозможен, поскольку не было установлено соединение с браузером. Дополнительные сведения см. в разделе [Обнаружение времени подготовки приложения блазор](#detect-when-a-blazor-app-is-prerendering) .

Следующий пример основан на [текстдекодер](https://developer.mozilla.org/docs/Web/API/TextDecoder), экспериментальном декодере на основе JavaScript. В примере показано, как вызвать функцию JavaScript из C# метода. Функция JavaScript принимает массив байтов из C# метода, декодирует массив и возвращает текст компоненту для вывода.

В элементе `<head>` элемента *wwwroot/index.HTML* (блазор Assembly) или *pages/_Host. cshtml* (блазор Server) Укажите функцию, которая использует `TextDecoder` для декодирования переданного массива:

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

Код JavaScript, например код, показанный в предыдущем примере, можно также загрузить из файла JavaScript (*. js*) со ссылкой на файл скрипта:

```html
<script src="exampleJsInterop.js"></script>
```

Следующий компонент:

* Вызывает функцию JavaScript `ConvertArray`, используя `JsRuntime` при выборе кнопки компонента (**преобразование массива**).
* После вызова функции JavaScript переданный массив преобразуется в строку. Строка возвращается компоненту для вывода.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

Чтобы использовать абстракцию `IJSRuntime`, следует принять любой из следующих подходов:

* Вставьте абстракцию `IJSRuntime` в компонент Razor (*. Razor*):

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* Вставьте абстракцию `IJSRuntime` в класс (*CS*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* Для создания динамического содержимого с помощью [буилдрендертри](xref:blazor/components#manual-rendertreebuilder-logic)используйте атрибут `[Inject]`:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

В примере приложения на стороне клиента, прилагаемом к этой теме, для приложения, которое взаимодействует с моделью DOM для получения вводимых пользователем данных и вывода приветственного сообщения, доступны две функции JavaScript.

* `showPrompt` &ndash; выдает запрос на ввод данных пользователем (имя пользователя) и возвращает имя вызывающему объекту.
* `displayWelcome` &ndash; назначает приветственное сообщение объекту модели DOM с `id` из `welcome`.

*wwwroot/ексамплежсинтероп. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Поместите тег `<script>`, который ссылается на файл JavaScript, в файл *wwwroot/index.HTML* (блазор) или *pages/_Host. cshtml* (блазор Server).

*wwwroot/index.HTML* (Блазор-сборка):

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (сервер блазор):

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

Не размещайте тег `<script>` в файле компонента, так как тег `<script>` не может быть обновлен динамически.

Методы .NET взаимодействует с функциями JavaScript в файле *ексамплежсинтероп. js* путем вызова `IJSRuntime.InvokeAsync<T>`.

Абстракция `IJSRuntime` является асинхронной, чтобы разрешить сценарии Блазор Server. Если приложение является Блазор приложением для сборки и требуется вызвать функцию JavaScript синхронно, произведение производных от `IJSInProcessRuntime` и вызов `Invoke<T>`. Рекомендуется, чтобы большинство библиотек взаимодействия JavaScript использовали асинхронные интерфейсы API, чтобы обеспечить доступность библиотек во всех сценариях.

Пример приложения содержит компонент для демонстрации взаимодействия JavaScript. Компонент:

* Получает входные данные пользователя с помощью командной строки JavaScript.
* Возвращает текст компонента для обработки.
* Вызывает вторую функцию JavaScript, которая взаимодействует с моделью DOM для вывода приветственного сообщения.

*Pages/жсинтероп. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. При выполнении `TriggerJsPrompt` путем выбора кнопки **запроса JavaScript триггера** компонента вызывается функция JavaScript `showPrompt`, предоставленная в файле *wwwroot/ексамплежсинтероп. js* .
1. Функция `showPrompt` принимает ввод пользователя (имя пользователя), который кодируется в формате HTML и возвращается компоненту. Компонент сохраняет имя пользователя в локальной переменной `name`.
1. Строка, хранящаяся в `name`, встроена в приветственное сообщение, которое передается в функцию JavaScript, `displayWelcome`, что позволяет отобразить приветственное сообщение в тег заголовка.

## <a name="call-a-void-javascript-function"></a>Вызов функции JavaScript void

Функции JavaScript, возвращающие [значение void (0)/воид 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) или [undefine](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) , вызываются с `IJSRuntime.InvokeVoidAsync`.

## <a name="detect-when-a-blazor-app-is-prerendering"></a>Обнаружение времени предварительной отрисовки приложения Блазор
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Запись ссылок на элементы

Для некоторых сценариев [взаимодействия JavaScript](xref:blazor/javascript-interop) требуются ссылки на элементы HTML. Например, библиотеке пользовательского интерфейса может потребоваться ссылка на элемент для инициализации, или же может потребоваться вызов API-интерфейсов, аналогичных Command, для элемента, например `focus` или `play`.

Запишите ссылки на элементы HTML в компоненте, используя следующий подход:

* Добавьте атрибут `@ref` к элементу HTML.
* Определите поле типа `ElementReference`, имя которого соответствует значению атрибута `@ref`.

В следующем примере показана запись ссылки на элемент `username` `<input>`:

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> **Не** используйте захваченные ссылки на элементы в качестве способа заполнения модели DOM. Это может повлиять на модель декларативной отрисовки.

Что касается кода .NET, `ElementReference` является непрозрачным маркером. *Единственное* , что можно сделать с `ElementReference`, — передать его в код JavaScript через взаимодействие JavaScript. При этом код на стороне JavaScript получает экземпляр `HTMLElement`, который можно использовать с обычными API DOM.

Например, следующий код определяет метод расширения .NET, который позволяет установить фокус на элемент:

*ексамплежсинтероп. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Используйте `IJSRuntime.InvokeAsync<T>` и вызовите `exampleJsFunctions.focusElement` с помощью `ElementReference`, чтобы сосредоточиться на элементе:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Чтобы использовать метод расширения для выделения элемента, создайте статический метод расширения, который получает экземпляр `IJSRuntime`:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Метод вызывается непосредственно для объекта. В следующем примере предполагается, что статический метод `Focus` доступен из пространства имен `JsInteropClasses`:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> Переменная `username` заполняется только после подготовки компонента к просмотру. Если незаполненное `ElementReference` передается в код JavaScript, код JavaScript получает значение `null`. Для управления ссылками на элементы после завершения отрисовки компонента (для установки начального фокуса на элемент) используйте [методы жизненного цикла компонента](xref:blazor/components#lifecycle-methods)`OnAfterRenderAsync` или `OnAfterRender`.

## <a name="invoke-net-methods-from-javascript-functions"></a>Вызов методов .NET из функций JavaScript

### <a name="static-net-method-call"></a>Статический вызов метода .NET

Чтобы вызвать статический метод .NET из JavaScript, используйте функции `DotNet.invokeMethod` или `DotNet.invokeMethodAsync`. Передайте идентификатор статического метода, который необходимо вызвать, имя сборки, содержащей функцию, и любые аргументы. Асинхронная версия является предпочтительной для поддержки сценариев сервера Блазор. Чтобы вызвать метод .NET из JavaScript, метод .NET должен быть открытым, статическим и иметь атрибут `[JSInvokable]`. По умолчанию идентификатором метода является имя метода, но можно указать другой идентификатор с помощью конструктора `JSInvokableAttribute`. Вызов открытых универсальных методов в настоящее время не поддерживается.

Пример приложения включает C# метод, возвращающий массив `int`S. К методу применяется атрибут `JSInvokable`.

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

Четвертое значение массива помещается в массив (`data.push(4);`), возвращаемый `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Вызов метода экземпляра

Кроме того, можно вызывать методы экземпляра .NET из JavaScript. Вызов метода экземпляра .NET из JavaScript:

* Передайте экземпляр .NET в JavaScript, заключив его в экземпляр `DotNetObjectReference`. Экземпляр .NET передается по ссылке на JavaScript.
* Вызывайте методы экземпляра .NET для экземпляра с помощью функций `invokeMethod` или `invokeMethodAsync`. Экземпляр .NET можно также передать в качестве аргумента при вызове других методов .NET из JavaScript.

> [!NOTE]
> Пример приложения записывает сообщения в консоль на стороне клиента. В следующих примерах, продемонстрированных в примере приложения, изучите выходные данные консоли браузера в средствах разработчика браузера.

Когда выбран **метод триггера экземпляра .NET хеллохелпер. SayHello** , вызывается `ExampleJsInterop.CallHelloHelperSayHello` и в метод передается имя, `Blazor`.

*Pages/жсинтероп. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` вызывает функцию JavaScript `sayHello` с новым экземпляром `HelloHelper`.

*Жсинтеропклассес/ексамплежсинтероп. CS*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/ексамплежсинтероп. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Имя передается в конструктор `HelloHelper`, который задает свойство `HelloHelper.Name`. При выполнении функции JavaScript `sayHello` `HelloHelper.SayHello` возвращает сообщение `Hello, {Name}!`, которое записывается в консоль функцией JavaScript.

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

Для получения дополнительной информации см. <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Вызовы взаимодействия с зафиксированным JS

Взаимодействие с JS может завершиться сбоем из-за ошибок сети и должно рассматриваться как ненадежное. По умолчанию серверное приложение Блазор время ожидания вызовов взаимодействия JS на сервере через одну минуту. Если приложение может допускать более агрессивное время ожидания, например 10 секунд, задайте время ожидания одним из следующих способов.

* Глобально в `Startup.ConfigureServices` укажите время ожидания:

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
