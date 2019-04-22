---
title: Общие сведения об использовании Blazor в ASP.NET Core
author: guardrex
description: Узнайте больше об использовании Blazor в ASP.NET Core для создания интерактивного клиентского веб-интерфейса с помощью .NET в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614600"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="9b197-103">Введение в Blazor</span><span class="sxs-lookup"><span data-stu-id="9b197-103">Introduction to Blazor</span></span>

<span data-ttu-id="9b197-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9b197-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="razor-components"></a><span data-ttu-id="9b197-105">Компоненты Razor</span><span class="sxs-lookup"><span data-stu-id="9b197-105">Razor Components</span></span>

<span data-ttu-id="9b197-106">*Компоненты Razor* (модель размещения на стороне сервера Blazor) — это средства для создания интерактивного клиентского веб-интерфейса с помощью .NET, которые предоставляют такие возможности:</span><span class="sxs-lookup"><span data-stu-id="9b197-106">The server-side hosting model of Blazor, *Razor Components*, is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="9b197-107">создание многофункциональных интерактивных пользовательских интерфейсов с помощью C# (а не JavaScript);</span><span class="sxs-lookup"><span data-stu-id="9b197-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="9b197-108">совместное использование серверной и клиентской логик приложений, написанных с помощью .NET;</span><span class="sxs-lookup"><span data-stu-id="9b197-108">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="9b197-109">отображение пользовательского интерфейса в виде HTML-страницы с CSS для широкой поддержки браузеров, в том числе для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="9b197-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="9b197-110">Компоненты Razor поддерживают базовые функции, которые требуются большинству приложений, в том числе такие:</span><span class="sxs-lookup"><span data-stu-id="9b197-110">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="9b197-111">Параметры</span><span class="sxs-lookup"><span data-stu-id="9b197-111">Parameters</span></span>
* <span data-ttu-id="9b197-112">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="9b197-112">Event handling</span></span>
* <span data-ttu-id="9b197-113">привязка данных,</span><span class="sxs-lookup"><span data-stu-id="9b197-113">Data binding</span></span>
* <span data-ttu-id="9b197-114">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="9b197-114">Routing</span></span>
* <span data-ttu-id="9b197-115">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="9b197-115">Dependency injection</span></span>
* <span data-ttu-id="9b197-116">Макеты</span><span class="sxs-lookup"><span data-stu-id="9b197-116">Layouts</span></span>
* <span data-ttu-id="9b197-117">Использование шаблонов</span><span class="sxs-lookup"><span data-stu-id="9b197-117">Templating</span></span>
* <span data-ttu-id="9b197-118">Каскадные значения</span><span class="sxs-lookup"><span data-stu-id="9b197-118">Cascading values</span></span>

<span data-ttu-id="9b197-119">Компоненты Razor отделяют логику отображения компонентов от способов применения обновлений пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9b197-119">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="9b197-120">Компоненты Razor в ASP.NET Core в .NET Core 3.0 реализуют поддержку размещения компонентов Razor на сервере в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b197-120">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="9b197-121">Обновления пользовательского интерфейса обрабатываются через подключение SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b197-121">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="9b197-122">Среда выполнения:</span><span class="sxs-lookup"><span data-stu-id="9b197-122">The runtime:</span></span>

* <span data-ttu-id="9b197-123">обрабатывает отправку событий пользовательского интерфейса из браузера на сервер;</span><span class="sxs-lookup"><span data-stu-id="9b197-123">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="9b197-124">применяет обновления пользовательского интерфейса, отправленные сервером браузеру после выполнения компонентов.</span><span class="sxs-lookup"><span data-stu-id="9b197-124">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="9b197-125">Подключение, используемое компонентами Razor для обмена данными с браузером, также применяется для обработки вызовов взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b197-125">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Компоненты Razor выполняют код .NET на сервере и взаимодействуют с моделью DOM на стороне клиента через подключение SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="9b197-127">Для получения дополнительной информации см. <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="9b197-127">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="9b197-128">Компоненты</span><span class="sxs-lookup"><span data-stu-id="9b197-128">Components</span></span>

<span data-ttu-id="9b197-129">*Компонент Razor* — это часть пользовательского интерфейса, например страница, диалоговое окно или форма ввода данных.</span><span class="sxs-lookup"><span data-stu-id="9b197-129">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="9b197-130">Компоненты обрабатывают для пользователей события и определяют гибкую логику отображения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9b197-130">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="9b197-131">Их можно вкладывать друг в друга и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="9b197-131">Components can be nested and reused.</span></span>

<span data-ttu-id="9b197-132">Компоненты — это классы .NET, встроенные в сборки .NET, которые можно совместно использовать и распространять в виде пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="9b197-132">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="9b197-133">Этот класс обычно записывается в виде страницы разметки Razor с расширением файла *.razor* (Компоненты Razor) или страницы разметки Razor с расширением файла *.cshtml* (Blazor).</span><span class="sxs-lookup"><span data-stu-id="9b197-133">The class is normally written in the form of a Razor markup page with a *.razor* file extension (Razor Components) or Razor markup page with a *.cshtml* file extension (Blazor).</span></span>

<span data-ttu-id="9b197-134">[Razor](xref:mvc/views/razor) — это синтаксис для объединения разметки HTML с кодом C#.</span><span class="sxs-lookup"><span data-stu-id="9b197-134">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="9b197-135">Razor предназначен для повышения производительности разработчика и позволяет разработчику переключаться между разметкой и C# в одном файле с поддержкой [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="9b197-135">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="9b197-136">Razor Pages и представления MVC также используют Razor.</span><span class="sxs-lookup"><span data-stu-id="9b197-136">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="9b197-137">В отличие от Razor Pages и представлений MVC, которые созданы на базе модели "запрос — ответ", компоненты используются исключительно для обработки композиции пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9b197-137">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="9b197-138">Компоненты Razor можно использовать исключительно для клиентской логики и композиции пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9b197-138">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="9b197-139">Следующая разметка является примером настраиваемого диалогового компонента:</span><span class="sxs-lookup"><span data-stu-id="9b197-139">The following markup is an example of a custom dialog component:</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="9b197-140">Когда этот компонент используется в другом месте в приложении, IntelliSense ускоряет разработку с помощью выполнения синтаксиса и параметров.</span><span class="sxs-lookup"><span data-stu-id="9b197-140">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="9b197-141">Компоненты преобразуются в представление DOM браузера в памяти, которое называется *деревом визуализации*. Его затем можно использовать для гибкого и эффективного обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9b197-141">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="9b197-142">Взаимодействие с JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b197-142">JavaScript interop</span></span>

<span data-ttu-id="9b197-143">Для приложений, требующих сторонние библиотеки JavaScript и API браузера, компоненты взаимодействуют с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b197-143">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="9b197-144">Компоненты могут использовать любую библиотеку или API, которые может использовать JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b197-144">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="9b197-145">Код C# может вызывать код JavaScript, а код JavaScript — код C#.</span><span class="sxs-lookup"><span data-stu-id="9b197-145">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="9b197-146">Дополнительные сведения см. в разделе [Взаимодействие с JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="9b197-146">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="9b197-147">Совместное использование кода и .NET Standard</span><span class="sxs-lookup"><span data-stu-id="9b197-147">Code sharing and .NET Standard</span></span>

<span data-ttu-id="9b197-148">Приложения могут ссылаться и использовать существующие библиотеки [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="9b197-148">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="9b197-149">.NET Standard — это формальная спецификация API-интерфейсов .NET, которые доступны во всех реализациях .NET.</span><span class="sxs-lookup"><span data-stu-id="9b197-149">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="9b197-150">Blazor реализует .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="9b197-150">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="9b197-151">API, которые нельзя использовать в веб-браузере (например, для доступа к файловой системе, открытия сокета, работы с потоками и других операций), выдают исключение <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="9b197-151">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="9b197-152">Библиотеки классов .NET Standard можно использовать на разных платформах .NET, включая Blazor, .NET Framework, .NET Core, Mono, Xamarin и Unity.</span><span class="sxs-lookup"><span data-stu-id="9b197-152">.NET Standard class libraries can be shared across different .NET platforms, like Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="blazor"></a><span data-ttu-id="9b197-153">Blazor</span><span class="sxs-lookup"><span data-stu-id="9b197-153">Blazor</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="9b197-154">Blazor — это платформа одностраничных приложений для создания интерактивных клиентских веб-приложений с использованием .NET.</span><span class="sxs-lookup"><span data-stu-id="9b197-154">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="9b197-155">Blazor использует открытые веб-стандарты без подключаемых модулей или транспиляции кода.</span><span class="sxs-lookup"><span data-stu-id="9b197-155">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="9b197-156">Blazor работает во всех современных веб-браузерах, включая браузеры мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="9b197-156">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="9b197-157">Использование .NET в браузере для веб-разработки на стороне клиента дает множество преимуществ:</span><span class="sxs-lookup"><span data-stu-id="9b197-157">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="9b197-158">**Язык C#**: создавайте код на C#, а не на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b197-158">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="9b197-159">**Экосистема .NET**: используйте существующую экосистему библиотек .NET.</span><span class="sxs-lookup"><span data-stu-id="9b197-159">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="9b197-160">**Полностековая разработка**: совместно используйте серверную и клиентскую логики.</span><span class="sxs-lookup"><span data-stu-id="9b197-160">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="9b197-161">**Скорость и масштабируемость**: NET обеспечивает производительность, надежность и безопасность.</span><span class="sxs-lookup"><span data-stu-id="9b197-161">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="9b197-162">**Ведущее в отрасли средства**: сохраняйте высокий уровень продуктивности с помощью Visual Studio в Windows, Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="9b197-162">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="9b197-163">**Стабильность и согласованность**:  в основе лежит общий набор языков, платформ и инструментов, которые отличаются стабильностью, многофункциональностью и простотой использования.</span><span class="sxs-lookup"><span data-stu-id="9b197-163">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="9b197-164">Выполнение кода .NET в веб-браузерах становится возможным благодаря технологии [WebAssembly](http://webassembly.org) (сокращенно *wasm*).</span><span class="sxs-lookup"><span data-stu-id="9b197-164">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="9b197-165">WebAssembly — это открытый веб-стандарт, который поддерживается в веб-браузерах без подключаемых модулей.</span><span class="sxs-lookup"><span data-stu-id="9b197-165">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="9b197-166">WebAssembly — это компактный формат байт-кода, оптимизированный для быстрой загрузки и максимального быстродействия.</span><span class="sxs-lookup"><span data-stu-id="9b197-166">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="9b197-167">Код WebAssembly может использовать все возможности браузера с помощью взаимодействия с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b197-167">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="9b197-168">В то же время код .NET, запускаемый с использованием WebAssembly, выполняется в той же доверенной песочнице, что и код JavaScript, для предотвращения вредоносных действий на клиентском компьютере.</span><span class="sxs-lookup"><span data-stu-id="9b197-168">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor выполняет код .NET в браузере с WebAssembly.](index/_static/blazor.png)

<span data-ttu-id="9b197-170">Когда приложение Blazor создано и запущено в браузере:</span><span class="sxs-lookup"><span data-stu-id="9b197-170">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="9b197-171">Файлы кода C# и файлы Razor компилируются в сборки .NET.</span><span class="sxs-lookup"><span data-stu-id="9b197-171">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="9b197-172">Сборки и среда выполнения .NET загружаются в браузер.</span><span class="sxs-lookup"><span data-stu-id="9b197-172">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="9b197-173">Blazor осуществляет начальную загрузку среды выполнения .NET и настраивает ее для загрузки сборок для приложения.</span><span class="sxs-lookup"><span data-stu-id="9b197-173">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="9b197-174">Операции с моделью DOM и вызовы API браузера обрабатываются средой выполнения Blazor через взаимодействие с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b197-174">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="9b197-175">Blazor поддерживает базовые функции, в которых нуждаются большинство приложений, в том числе такие:</span><span class="sxs-lookup"><span data-stu-id="9b197-175">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="9b197-176">Параметры</span><span class="sxs-lookup"><span data-stu-id="9b197-176">Parameters</span></span>
* <span data-ttu-id="9b197-177">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="9b197-177">Event handling</span></span>
* <span data-ttu-id="9b197-178">привязка данных,</span><span class="sxs-lookup"><span data-stu-id="9b197-178">Data binding</span></span>
* <span data-ttu-id="9b197-179">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="9b197-179">Routing</span></span>
* <span data-ttu-id="9b197-180">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="9b197-180">Dependency injection</span></span>
* <span data-ttu-id="9b197-181">Макеты</span><span class="sxs-lookup"><span data-stu-id="9b197-181">Layouts</span></span>
* <span data-ttu-id="9b197-182">Использование шаблонов</span><span class="sxs-lookup"><span data-stu-id="9b197-182">Templating</span></span>
* <span data-ttu-id="9b197-183">Каскадные значения</span><span class="sxs-lookup"><span data-stu-id="9b197-183">Cascading values</span></span>

<span data-ttu-id="9b197-184">Чтобы уменьшить размер скачиваемого приложения, [компоновщик промежуточного языка (IL)](xref:host-and-deploy/blazor/configure-linker) отсекает неиспользуемый код от приложения при его публикации.</span><span class="sxs-lookup"><span data-stu-id="9b197-184">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="9b197-185">Blazor предполагает размещение на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="9b197-185">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="9b197-186">Так как Blazor отделяет логику отображения компонента от способов применения обновлений пользовательского интерфейса, Blazor можно размещать разными способами.</span><span class="sxs-lookup"><span data-stu-id="9b197-186">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="9b197-187">Используйте [Компоненты Razor](#razor-components) в ASP.NET Core, чтобы разместить их на сервере в приложении ASP.NET Core с возможностью обработки обновлений пользовательского интерфейса через подключение SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b197-187">Use ASP.NET Core [Razor Components](#razor-components) to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="9b197-188">Для получения дополнительной информации см. <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="9b197-188">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span> 

<span data-ttu-id="9b197-189">Размер полезных данных критически важен для производительности и, как следствие, удобства использования приложения.</span><span class="sxs-lookup"><span data-stu-id="9b197-189">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="9b197-190">Blazor оптимизирует размер полезных данных, чтобы сократить время скачивания:</span><span class="sxs-lookup"><span data-stu-id="9b197-190">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="9b197-191">неиспользуемые части сборок .NET удаляются в ходе компиляции;</span><span class="sxs-lookup"><span data-stu-id="9b197-191">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="9b197-192">HTTP-ответы сжимаются;</span><span class="sxs-lookup"><span data-stu-id="9b197-192">HTTP responses are compressed.</span></span>
* <span data-ttu-id="9b197-193">среда выполнения и сборки .NET кэшируются в браузере.</span><span class="sxs-lookup"><span data-stu-id="9b197-193">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="9b197-194">[Компоненты Razor](#razor-components) предоставляют меньший размер полезных данных, чем Blazor, поддерживая сборки .NET, сборку приложения и среду выполнения на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="9b197-194">[Razor Components](#razor-components) provides a smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="9b197-195">Приложения компонентов Razor передают клиентам только файлы разметки и статическое содержимое.</span><span class="sxs-lookup"><span data-stu-id="9b197-195">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b197-196">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9b197-196">Additional resources</span></span>

* [<span data-ttu-id="9b197-197">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="9b197-197">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="9b197-198">Руководство по языку C#</span><span class="sxs-lookup"><span data-stu-id="9b197-198">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="9b197-199">HTML</span><span class="sxs-lookup"><span data-stu-id="9b197-199">HTML</span></span>](https://www.w3.org/html/)
