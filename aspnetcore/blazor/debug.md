---
title: Отладка ASP.NET Core Блазор
author: guardrex
description: Узнайте, как выполнять отладку приложений Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/debug
ms.openlocfilehash: 3519479d8058f013de23cc9cfa0f5574cd158053
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207212"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="a47eb-103">Отладка ASP.NET Core Блазор</span><span class="sxs-lookup"><span data-stu-id="a47eb-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="a47eb-104">Даниэль Roth)</span><span class="sxs-lookup"><span data-stu-id="a47eb-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a47eb-105">Для отладки Блазор приложений веб сборки, выполняющихся на веб – сборке в Chrome, существует *ранняя* поддержка.</span><span class="sxs-lookup"><span data-stu-id="a47eb-105">*Early* support exists for debugging Blazor WebAssembly apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="a47eb-106">Возможности отладчика ограничены.</span><span class="sxs-lookup"><span data-stu-id="a47eb-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="a47eb-107">Доступные сценарии включают в себя:</span><span class="sxs-lookup"><span data-stu-id="a47eb-107">Available scenarios include:</span></span>

* <span data-ttu-id="a47eb-108">Установка и удаление точек останова.</span><span class="sxs-lookup"><span data-stu-id="a47eb-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="a47eb-109">Выполнение одного шага (`F10`) с помощью кода или возобновления`F8`() кода.</span><span class="sxs-lookup"><span data-stu-id="a47eb-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="a47eb-110">В окне *локальные* переменные просмотрите значения всех локальных переменных типа `int`, `string`и `bool`.</span><span class="sxs-lookup"><span data-stu-id="a47eb-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="a47eb-111">См. стек вызовов, включая цепочки вызовов, которые поступают из JavaScript в .NET и из .NET в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a47eb-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="a47eb-112">Вы *не можете*:</span><span class="sxs-lookup"><span data-stu-id="a47eb-112">You *can't*:</span></span>

* <span data-ttu-id="a47eb-113">Обратите внимание на значения всех локальных переменных, `int`которые `string`не являются `bool`, или.</span><span class="sxs-lookup"><span data-stu-id="a47eb-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="a47eb-114">Обратите внимание на значения всех свойств или полей класса.</span><span class="sxs-lookup"><span data-stu-id="a47eb-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="a47eb-115">Наведите указатель мыши на переменные, чтобы увидеть их значения.</span><span class="sxs-lookup"><span data-stu-id="a47eb-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="a47eb-116">Вычисление выражений в консоли.</span><span class="sxs-lookup"><span data-stu-id="a47eb-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="a47eb-117">Шаг с заходом в асинхронные вызовы.</span><span class="sxs-lookup"><span data-stu-id="a47eb-117">Step across async calls.</span></span>
* <span data-ttu-id="a47eb-118">Выполнение большинства других обычных сценариев отладки.</span><span class="sxs-lookup"><span data-stu-id="a47eb-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="a47eb-119">Разработка дальнейших сценариев отладки — это непрерывная работа группы инженеров.</span><span class="sxs-lookup"><span data-stu-id="a47eb-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a47eb-120">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a47eb-120">Prerequisites</span></span>

<span data-ttu-id="a47eb-121">Для отладки требуется один из следующих браузеров:</span><span class="sxs-lookup"><span data-stu-id="a47eb-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="a47eb-122">Google Chrome (версия 70 или более поздняя)</span><span class="sxs-lookup"><span data-stu-id="a47eb-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="a47eb-123">Microsoft ребра Preview ([канал пограничной разработки](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="a47eb-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="a47eb-124">Процедура</span><span class="sxs-lookup"><span data-stu-id="a47eb-124">Procedure</span></span>

1. <span data-ttu-id="a47eb-125">Запустите приложение блазор сборки в `Debug` конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a47eb-125">Run a Blazor WebAssembly app in `Debug` configuration.</span></span> <span data-ttu-id="a47eb-126">Передайте `dotnet run --configuration Debug`параметр в команду [DotNet Run:.](/dotnet/core/tools/dotnet-run) `--configuration Debug`</span><span class="sxs-lookup"><span data-stu-id="a47eb-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="a47eb-127">Доступ к приложению в браузере.</span><span class="sxs-lookup"><span data-stu-id="a47eb-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="a47eb-128">Поместите фокус клавиатуры на приложение, а не на панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="a47eb-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="a47eb-129">Панель средств разработчика можно закрыть при инициации отладки.</span><span class="sxs-lookup"><span data-stu-id="a47eb-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="a47eb-130">Выберите следующее сочетание клавиш, определяемое Блазор:</span><span class="sxs-lookup"><span data-stu-id="a47eb-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="a47eb-131">`Shift+Alt+D`в Windows и Linux</span><span class="sxs-lookup"><span data-stu-id="a47eb-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="a47eb-132">`Shift+Cmd+D`на macOS</span><span class="sxs-lookup"><span data-stu-id="a47eb-132">`Shift+Cmd+D` on macOS</span></span>
1. <span data-ttu-id="a47eb-133">Выполните действия, перечисленные на экране, чтобы перезапустить браузер с включенной удаленной отладкой.</span><span class="sxs-lookup"><span data-stu-id="a47eb-133">Follow the steps listed on the screen to restart the browser with remote debugging enabled.</span></span>
1. <span data-ttu-id="a47eb-134">Чтобы запустить сеанс отладки, снова выберите сочетание клавиш, определенное Блазор.</span><span class="sxs-lookup"><span data-stu-id="a47eb-134">Select the following Blazor-specific keyboard shortcut once again to start the debug session:</span></span>
   * <span data-ttu-id="a47eb-135">`Shift+Alt+D`в Windows и Linux</span><span class="sxs-lookup"><span data-stu-id="a47eb-135">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="a47eb-136">`Shift+Cmd+D`на macOS</span><span class="sxs-lookup"><span data-stu-id="a47eb-136">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="a47eb-137">Включить удаленную отладку</span><span class="sxs-lookup"><span data-stu-id="a47eb-137">Enable remote debugging</span></span>

<span data-ttu-id="a47eb-138">Если удаленная отладка отключена, то **не удается найти отладочную страницу ошибки вкладки браузера** создается Chrome.</span><span class="sxs-lookup"><span data-stu-id="a47eb-138">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="a47eb-139">На странице ошибки содержатся инструкции по запуску Chrome с открытым портом отладки, чтобы прокси-сервер отладки Блазор мог подключиться к приложению.</span><span class="sxs-lookup"><span data-stu-id="a47eb-139">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="a47eb-140">*Закройте все экземпляры Chrome* и перезапустите Chrome с указанием.</span><span class="sxs-lookup"><span data-stu-id="a47eb-140">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="a47eb-141">Отладка приложения</span><span class="sxs-lookup"><span data-stu-id="a47eb-141">Debug the app</span></span>

<span data-ttu-id="a47eb-142">После того как Chrome будет запущен с включенной удаленной отладкой, откроется новая вкладка отладчика. Через некоторое время на вкладке **источники** отображается список сборок .NET в приложении.</span><span class="sxs-lookup"><span data-stu-id="a47eb-142">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="a47eb-143">Разверните каждую сборку и найдите исходные файлы *. CS*/ *. Razor* , доступные для отладки.</span><span class="sxs-lookup"><span data-stu-id="a47eb-143">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="a47eb-144">Установите точки останова, переключитесь обратно на вкладку приложения, и точки останова будут достигнуты при выполнении кода.</span><span class="sxs-lookup"><span data-stu-id="a47eb-144">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="a47eb-145">После попадания в точку останова одношаговое`F10`() через код или Resume (`F8`) выполнение кода обычно.</span><span class="sxs-lookup"><span data-stu-id="a47eb-145">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

<span data-ttu-id="a47eb-146">Блазор предоставляет прокси-сервер отладки, реализующий [протокол Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) и дополненный протоколом. Сведения, относящиеся к сети.</span><span class="sxs-lookup"><span data-stu-id="a47eb-146">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="a47eb-147">Когда нажата клавиша Отладка, Блазор указывает Chrome DevTools на прокси-сервере.</span><span class="sxs-lookup"><span data-stu-id="a47eb-147">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="a47eb-148">Прокси-сервер подключается к окну браузера, для которого выполняется отладка (следовательно, необходимо включить удаленную отладку).</span><span class="sxs-lookup"><span data-stu-id="a47eb-148">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="a47eb-149">Карты исходного кода браузера</span><span class="sxs-lookup"><span data-stu-id="a47eb-149">Browser source maps</span></span>

<span data-ttu-id="a47eb-150">Карты источников браузера позволяют браузеру сопоставлять скомпилированные файлы с исходными файлами и обычно используются для отладки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a47eb-150">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="a47eb-151">Однако в настоящее время Блазор не C# сопоставляется непосредственно с JavaScript/васм.</span><span class="sxs-lookup"><span data-stu-id="a47eb-151">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="a47eb-152">Вместо этого Блазор выполняет интерпретацию IL в браузере, поэтому исходные карты не актуальны.</span><span class="sxs-lookup"><span data-stu-id="a47eb-152">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="a47eb-153">Советы по устранению неполадок</span><span class="sxs-lookup"><span data-stu-id="a47eb-153">Troubleshooting tip</span></span>

<span data-ttu-id="a47eb-154">Если вы используете ошибки, следующий совет может помочь:</span><span class="sxs-lookup"><span data-stu-id="a47eb-154">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="a47eb-155">На вкладке **отладчик** откройте Инструменты разработчика в браузере.</span><span class="sxs-lookup"><span data-stu-id="a47eb-155">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="a47eb-156">В консоли выполните `localStorage.clear()` процедуру, чтобы удалить все точки останова.</span><span class="sxs-lookup"><span data-stu-id="a47eb-156">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
