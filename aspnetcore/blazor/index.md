---
title: Общие сведения об использовании Blazor в ASP.NET Core
author: guardrex
description: Узнайте больше об использовании Blazor в ASP.NET Core для создания интерактивного клиентского веб-интерфейса с помощью .NET в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 05/01/2019
uid: blazor/index
ms.openlocfilehash: d58115b17536cad0b3927e6d32b7dbe8db8e4b0f
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034419"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="80280-103">Введение в Blazor</span><span class="sxs-lookup"><span data-stu-id="80280-103">Introduction to Blazor</span></span>

<span data-ttu-id="80280-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="80280-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="80280-105">*Добро пожаловать в Blazor!*</span><span class="sxs-lookup"><span data-stu-id="80280-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="80280-106">Платформа Blazor предназначена для создания интерактивного веб-интерфейса на стороне клиента для .NET и предлагает следующие возможности:</span><span class="sxs-lookup"><span data-stu-id="80280-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="80280-107">создание многофункциональных интерактивных пользовательских интерфейсов на C# вместо JavaScript;</span><span class="sxs-lookup"><span data-stu-id="80280-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="80280-108">совместное использование серверной и клиентской логик приложений, написанных с помощью .NET;</span><span class="sxs-lookup"><span data-stu-id="80280-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="80280-109">отображение пользовательского интерфейса в виде HTML-страницы с CSS для широкой поддержки браузеров, в том числе для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="80280-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="80280-110">Использование .NET для разработки веб-приложений на стороне клиента предоставляет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="80280-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="80280-111">создавайте код на C#, а не на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="80280-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="80280-112">возможность использования существующей экосистемы .NET с библиотеками .NET;</span><span class="sxs-lookup"><span data-stu-id="80280-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="80280-113">сохранение единой логики приложений для сервера и клиента;</span><span class="sxs-lookup"><span data-stu-id="80280-113">Share app logic across the server and client.</span></span>
* <span data-ttu-id="80280-114">высокая производительность, надежность и безопасность платформы .NET;</span><span class="sxs-lookup"><span data-stu-id="80280-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="80280-115">сохраняйте высокий уровень продуктивности с помощью Visual Studio в Windows, Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="80280-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="80280-116">создавайте приложения на основе распространенных языков, платформ и инструментов, которые отличаются стабильностью, широким набором функций и простотой в использовании.</span><span class="sxs-lookup"><span data-stu-id="80280-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="80280-117">Компоненты</span><span class="sxs-lookup"><span data-stu-id="80280-117">Components</span></span>

<span data-ttu-id="80280-118">Приложения Blazor основаны на *компонентах*.</span><span class="sxs-lookup"><span data-stu-id="80280-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="80280-119">Компонентом Blazor называется любой элемент пользовательского интерфейса, например страница, диалоговое окно или форма ввода данных.</span><span class="sxs-lookup"><span data-stu-id="80280-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="80280-120">Компоненты обрабатывают для пользователей события и определяют гибкую логику отображения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="80280-120">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="80280-121">Их можно вкладывать друг в друга и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="80280-121">Components can be nested and reused.</span></span>

<span data-ttu-id="80280-122">Компоненты являются классами .NET, которые встроены в сборки .NET для совместного использования и распространения в виде [пакетов NuGet](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="80280-122">Components are .NET classes built into .NET assemblies that can be shared and distributed as [NuGet packages](/nuget/what-is-nuget).</span></span> <span data-ttu-id="80280-123">Класс компонента обычно записывается в виде страницы разметки Razor с расширением файла *.razor*.</span><span class="sxs-lookup"><span data-stu-id="80280-123">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="80280-124">Иногда компоненты в Blazor называются *компонентами Razor*.</span><span class="sxs-lookup"><span data-stu-id="80280-124">Components in Blazor are sometimes referred to as *Razor components*.</span></span> <span data-ttu-id="80280-125">Синтаксис [Razor](xref:mvc/views/razor) сочетает разметку HTML с кодом C#, что позволяет повысить производительность разработчиков.</span><span class="sxs-lookup"><span data-stu-id="80280-125">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="80280-126">Razor позволяет переключаться между разметкой HTML и кодом C# в одном файле с поддержкой [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="80280-126">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="80280-127">Синтаксис Razor применяется также для Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="80280-127">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="80280-128">В отличие от Razor Pages и MVC, которые построены по модели "запрос-ответ", компоненты используются специально для логики пользовательского интерфейса на стороне клиента и для компоновки.</span><span class="sxs-lookup"><span data-stu-id="80280-128">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="80280-129">Следующая разметка Razor определяет компонент *Dialog.razor*, который может быть вложен в другой компонент:</span><span class="sxs-lookup"><span data-stu-id="80280-129">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="@OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#!");
    }
}
```

<span data-ttu-id="80280-130">Содержимое текста (`ChildContent`) и заголовка (`Title`) диалогового окна предоставляются компонентом, который использует этот компонент в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="80280-130">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="80280-131">Метод C# `OnYes` инициируется событием кнопки `onclick`.</span><span class="sxs-lookup"><span data-stu-id="80280-131">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="80280-132">Blazor использует естественные теги HTML для компоновки пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="80280-132">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="80280-133">Элементы HTML определяют компоненты, а атрибуты тегов передают значения для свойств компонентов.</span><span class="sxs-lookup"><span data-stu-id="80280-133">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span> <span data-ttu-id="80280-134">`ChildContent` и `Title` устанавливаются тем компонентом, который использует компонент Dialog (*Index.razor*):</span><span class="sxs-lookup"><span data-stu-id="80280-134">`ChildContent` and `Title` are set by the component that uses the Dialog component (*Index.razor*):</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="80280-135">Диалоговое окно отображается, когда к родительскому компоненту (*Index.razor*) осуществляется доступ в браузере:</span><span class="sxs-lookup"><span data-stu-id="80280-135">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Компонент диалогового окна, отображаемый в браузере](index/_static/dialog.png)

<span data-ttu-id="80280-137">Если этот компонент используется в приложении, технология IntelliSense в [Visual Studio](/visualstudio/ide/using-intellisense) и [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) ускоряет разработку благодаря завершению синтаксиса и параметров.</span><span class="sxs-lookup"><span data-stu-id="80280-137">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="80280-138">Компоненты преобразуются в хранящееся в памяти представление модели DOM для браузера, которое называется *деревом отображения*. Оно позволяет гибко и эффективно обновлять пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="80280-138">Components render into an in-memory representation of the browser DOM called a *render tree* that's used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="80280-139">Blazor на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="80280-139">Blazor client-side</span></span>

<span data-ttu-id="80280-140">Blazor на стороне клиента — это платформа для одностраничных приложений, которая позволяет создавать интерактивные клиентские веб-приложения с помощью .NET.</span><span class="sxs-lookup"><span data-stu-id="80280-140">Blazor client-side is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="80280-141">Blazor на стороне клиента использует открытые веб-стандарты без подключаемых модулей или транскомпиляции кода. Она работает во всех современных веб-браузерах, в том числе на мобильных устройствах.</span><span class="sxs-lookup"><span data-stu-id="80280-141">Blazor client-side uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="80280-142">Выполнение кода .NET в веб-браузерах становится возможным благодаря технологии [WebAssembly](http://webassembly.org) (сокращенно *wasm*).</span><span class="sxs-lookup"><span data-stu-id="80280-142">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="80280-143">WebAssembly — это открытый веб-стандарт, который поддерживается в веб-браузерах без подключаемых модулей.</span><span class="sxs-lookup"><span data-stu-id="80280-143">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="80280-144">WebAssembly — это компактный формат байт-кода, оптимизированный для быстрой загрузки и максимального быстродействия.</span><span class="sxs-lookup"><span data-stu-id="80280-144">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="80280-145">Код WebAssembly может обращаться к любым функциям браузера через JavaScript благодаря поддержке *взаимодействия с JavaScript* \*\* .</span><span class="sxs-lookup"><span data-stu-id="80280-145">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="80280-146">Код .NET, который выполняется через WebAssembly в браузере, выполняется в той же доверенной песочнице, что и код JavaScript. Это практически устраняет любые возможности для выполнения вредоносных действий из приложения на клиентском компьютере.</span><span class="sxs-lookup"><span data-stu-id="80280-146">.NET code executed via WebAssembly in the browser runs in the same trusted sandbox as JavaScript, which virtually eliminates the opportunity for an app to perform malicious actions on the client machine.</span></span>

![Blazor на стороне клиента выполняет код .NET в браузере с WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="80280-148">Когда клиентское приложение Blazor создается и запускается в браузере:</span><span class="sxs-lookup"><span data-stu-id="80280-148">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="80280-149">Файлы кода C# и файлы Razor компилируются в сборки .NET.</span><span class="sxs-lookup"><span data-stu-id="80280-149">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="80280-150">Сборки и среда выполнения .NET загружаются в браузер.</span><span class="sxs-lookup"><span data-stu-id="80280-150">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="80280-151">Blazor на стороне клиента осуществляет начальную загрузку среды выполнения .NET и настраивает ее для загрузки сборок для приложения.</span><span class="sxs-lookup"><span data-stu-id="80280-151">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="80280-152">Среда выполнения Blazor на стороне клиента использует взаимодействие с JavaScript для обработки операций с моделью DOM и вызовов к API браузера.</span><span class="sxs-lookup"><span data-stu-id="80280-152">The Blazor client-side runtime uses JavaScript interop to handle Document Object Model (DOM) manipulation and browser API calls.</span></span>

<span data-ttu-id="80280-153">Размер опубликованного приложения, известный как *размер полезных данных*, является критически важным фактором производительности для удобства работы с приложением.</span><span class="sxs-lookup"><span data-stu-id="80280-153">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="80280-154">Крупное приложение скачивается в браузер довольно долго, что плохо влияет на взаимодействие с пользователем.</span><span class="sxs-lookup"><span data-stu-id="80280-154">A large app takes a relatively long time to download to a browser, which hurts the user experience.</span></span> <span data-ttu-id="80280-155">Blazor на стороне клиента оптимизирует размер полезных данных, чтобы сократить время скачивания:</span><span class="sxs-lookup"><span data-stu-id="80280-155">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="80280-156">При публикации неиспользуемый код исключается из приложения [компоновщиком промежуточного языка](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="80280-156">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="80280-157">HTTP-ответы сжимаются;</span><span class="sxs-lookup"><span data-stu-id="80280-157">HTTP responses are compressed.</span></span>
* <span data-ttu-id="80280-158">среда выполнения и сборки .NET кэшируются в браузере.</span><span class="sxs-lookup"><span data-stu-id="80280-158">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="80280-159">Подробные сведения и рекомендации по выбору модели размещения см. в статье <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="80280-159">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="80280-160">Blazor на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="80280-160">Blazor server-side</span></span>

<span data-ttu-id="80280-161">Blazor отделяет логику отображения компонентов от того, как применяются обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="80280-161">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="80280-162">Blazor на стороне сервера предоставляет поддержку размещения компонентов Razor на сервере в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="80280-162">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="80280-163">Обновления пользовательского интерфейса передаются через подключение [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="80280-163">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="80280-164">Среда выполнения обрабатывает отправку событий пользовательского интерфейса из браузера на сервер и применяет полученные от сервера обновления пользовательского интерфейса в браузере после выполнения компонентов.</span><span class="sxs-lookup"><span data-stu-id="80280-164">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="80280-165">Подключение, используемое Blazor на стороне сервера для обмена данными с браузером, также применяется для обработки вызовов взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="80280-165">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor на стороне сервера выполняет код .NET на сервере и взаимодействует с моделью DOM на стороне клиента через подключение SignalR.](index/_static/blazor-server-side.png)

<span data-ttu-id="80280-167">Подробные сведения и рекомендации по выбору модели размещения см. в статье <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="80280-167">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="80280-168">Взаимодействие с JavaScript</span><span class="sxs-lookup"><span data-stu-id="80280-168">JavaScript interop</span></span>

<span data-ttu-id="80280-169">Для приложений, требующих сторонние библиотеки JavaScript и API браузера, компоненты взаимодействуют с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="80280-169">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="80280-170">Компоненты могут использовать любую библиотеку или API, которые может использовать JavaScript.</span><span class="sxs-lookup"><span data-stu-id="80280-170">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="80280-171">Код C# может вызывать код JavaScript, а код JavaScript — код C#.</span><span class="sxs-lookup"><span data-stu-id="80280-171">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="80280-172">Для получения дополнительной информации см. <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="80280-172">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="80280-173">Совместное использование кода и .NET Standard</span><span class="sxs-lookup"><span data-stu-id="80280-173">Code sharing and .NET Standard</span></span>

<span data-ttu-id="80280-174">Blazor реализует [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="80280-174">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="80280-175">.NET Standard — это формальная спецификация API-интерфейсов .NET, которые доступны во всех реализациях .NET.</span><span class="sxs-lookup"><span data-stu-id="80280-175">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="80280-176">Библиотеки классов .NET Standard можно использовать на разных платформах .NET, таких как Blazor, .NET Framework, .NET Core, Xamarin, Mono и Unity.</span><span class="sxs-lookup"><span data-stu-id="80280-176">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="80280-177">Интерфейсы API, которые неприменимы в веб-браузере (например, доступ к файловой системе, открытие сокетов и работа с потоками), создают исключение <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="80280-177">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80280-178">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="80280-178">Additional resources</span></span>

* [<span data-ttu-id="80280-179">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="80280-179">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="80280-180">Руководство по языку C#</span><span class="sxs-lookup"><span data-stu-id="80280-180">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="80280-181">HTML</span><span class="sxs-lookup"><span data-stu-id="80280-181">HTML</span></span>](https://www.w3.org/html/)
