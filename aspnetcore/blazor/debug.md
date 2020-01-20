---
title: Blazor ASP.NET Core отладки
author: guardrex
description: Узнайте, как выполнять отладку Blazor приложений.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159993"
---
# <a name="debug-aspnet-core-opno-locblazor"></a><span data-ttu-id="c6e97-103">Blazor ASP.NET Core отладки</span><span class="sxs-lookup"><span data-stu-id="c6e97-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="c6e97-104">Даниэль Roth)</span><span class="sxs-lookup"><span data-stu-id="c6e97-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="c6e97-105">*Ранняя* поддержка для отладки Blazor сборки с помощью средств разработчика браузера в браузерах на основе Chromium (Chrome/ребр).</span><span class="sxs-lookup"><span data-stu-id="c6e97-105">*Early* support exists for debugging Blazor WebAssembly using the browser dev tools in Chromium-based browsers (Chrome/Edge).</span></span> <span data-ttu-id="c6e97-106">Выполняется работа:</span><span class="sxs-lookup"><span data-stu-id="c6e97-106">Work is in progress to:</span></span>

* <span data-ttu-id="c6e97-107">Полностью Включите отладку в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6e97-107">Fully enable debugging in Visual Studio.</span></span>
* <span data-ttu-id="c6e97-108">Включить отладку в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c6e97-108">Enable debugging in Visual Studio Code.</span></span>

<span data-ttu-id="c6e97-109">Возможности отладчика ограничены.</span><span class="sxs-lookup"><span data-stu-id="c6e97-109">Debugger capabilities are limited.</span></span> <span data-ttu-id="c6e97-110">Доступные сценарии включают в себя:</span><span class="sxs-lookup"><span data-stu-id="c6e97-110">Available scenarios include:</span></span>

* <span data-ttu-id="c6e97-111">Установка и удаление точек останова.</span><span class="sxs-lookup"><span data-stu-id="c6e97-111">Set and remove breakpoints.</span></span>
* <span data-ttu-id="c6e97-112">Выполнение одного шага (`F10`) с помощью кода или возобновления (`F8`) кода.</span><span class="sxs-lookup"><span data-stu-id="c6e97-112">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="c6e97-113">В окне *локальные* переменные просмотрите значения всех локальных переменных типа `int`, `string`и `bool`.</span><span class="sxs-lookup"><span data-stu-id="c6e97-113">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="c6e97-114">См. стек вызовов, включая цепочки вызовов, которые поступают из JavaScript в .NET и из .NET в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c6e97-114">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="c6e97-115">Вы *не можете*:</span><span class="sxs-lookup"><span data-stu-id="c6e97-115">You *can't*:</span></span>

* <span data-ttu-id="c6e97-116">Обратите внимание на значения всех локальных переменных, которые не являются `int`, `string`или `bool`.</span><span class="sxs-lookup"><span data-stu-id="c6e97-116">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="c6e97-117">Обратите внимание на значения всех свойств или полей класса.</span><span class="sxs-lookup"><span data-stu-id="c6e97-117">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="c6e97-118">Наведите указатель мыши на переменные, чтобы увидеть их значения.</span><span class="sxs-lookup"><span data-stu-id="c6e97-118">Hover over variables to see their values.</span></span>
* <span data-ttu-id="c6e97-119">Вычисление выражений в консоли.</span><span class="sxs-lookup"><span data-stu-id="c6e97-119">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="c6e97-120">Шаг с заходом в асинхронные вызовы.</span><span class="sxs-lookup"><span data-stu-id="c6e97-120">Step across async calls.</span></span>
* <span data-ttu-id="c6e97-121">Выполнение большинства других обычных сценариев отладки.</span><span class="sxs-lookup"><span data-stu-id="c6e97-121">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="c6e97-122">Разработка дальнейших сценариев отладки — это непрерывная работа группы инженеров.</span><span class="sxs-lookup"><span data-stu-id="c6e97-122">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6e97-123">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="c6e97-123">Prerequisites</span></span>

<span data-ttu-id="c6e97-124">Для отладки требуется один из следующих браузеров:</span><span class="sxs-lookup"><span data-stu-id="c6e97-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="c6e97-125">Google Chrome (версия 70 или более поздняя)</span><span class="sxs-lookup"><span data-stu-id="c6e97-125">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="c6e97-126">Microsoft ребра Preview ([канал пограничной разработки](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="c6e97-126">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="c6e97-127">Процедура .</span><span class="sxs-lookup"><span data-stu-id="c6e97-127">Procedure</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6e97-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6e97-128">Visual Studio</span></span>](#tab/visual-studio)

> [!WARNING]
> <span data-ttu-id="c6e97-129">Поддержка отладки в Visual Studio находится на раннем этапе разработки.</span><span class="sxs-lookup"><span data-stu-id="c6e97-129">Debugging support in Visual Studio is at an early stage of development.</span></span> <span data-ttu-id="c6e97-130">Отладка **F5** в настоящее время не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="c6e97-130">**F5** debugging isn't currently supported.</span></span>

1. <span data-ttu-id="c6e97-131">Запустите Blazor приложение сборки в `Debug` конфигурации без отладки (**Ctrl**+**F5** вместо **F5**).</span><span class="sxs-lookup"><span data-stu-id="c6e97-131">Run a Blazor WebAssembly app in `Debug` configuration without debugging (**Ctrl**+**F5** instead of **F5**).</span></span>
1. <span data-ttu-id="c6e97-132">Откройте свойства отладки приложения (последняя запись в меню **Отладка** ) и скопируйте **URL-адрес приложения**HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6e97-132">Open the Debug properties of the app (last entry in the **Debug** menu) and copy the HTTP **App URL**.</span></span> <span data-ttu-id="c6e97-133">Перейдите по адресу HTTP (а не к HTTPS-адресу) приложения с помощью браузера на основе Chromium (пограничной бета-версии или Chrome).</span><span class="sxs-lookup"><span data-stu-id="c6e97-133">Browse to the HTTP address (not the HTTPS address) of the app using a Chromium-based browser (Edge Beta or Chrome).</span></span>
1. <span data-ttu-id="c6e97-134">Поместите фокус клавиатуры на приложение в окне браузера, а не на панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="c6e97-134">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span> <span data-ttu-id="c6e97-135">Для этой процедуры лучше не закрывать панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="c6e97-135">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="c6e97-136">После запуска отладки можно повторно открыть панель средств разработчика.</span><span class="sxs-lookup"><span data-stu-id="c6e97-136">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="c6e97-137">Выберите следующие Blazorсочетания клавиш:</span><span class="sxs-lookup"><span data-stu-id="c6e97-137">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="c6e97-138">`Shift+Alt+D` в Windows</span><span class="sxs-lookup"><span data-stu-id="c6e97-138">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="c6e97-139">`Shift+Cmd+D` на macOS</span><span class="sxs-lookup"><span data-stu-id="c6e97-139">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="c6e97-140">Если вы получаете **вкладку "не удается найти браузер для отладки"** , см. раздел [Включение удаленной отладки](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="c6e97-140">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="c6e97-141">После включения удаленной отладки:</span><span class="sxs-lookup"><span data-stu-id="c6e97-141">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="c6e97-142">1 \.</span><span class="sxs-lookup"><span data-stu-id="c6e97-142">1\.</span></span> <span data-ttu-id="c6e97-143">Будет открыто новое окно браузера.</span><span class="sxs-lookup"><span data-stu-id="c6e97-143">A new browser window opens.</span></span> <span data-ttu-id="c6e97-144">Закройте прежнее окно.</span><span class="sxs-lookup"><span data-stu-id="c6e97-144">Close the prior window.</span></span>

   <span data-ttu-id="c6e97-145">2 \.</span><span class="sxs-lookup"><span data-stu-id="c6e97-145">2\.</span></span> <span data-ttu-id="c6e97-146">Поместите фокус клавиатуры в приложение в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="c6e97-146">Place the keyboard focus on the app in the browser window.</span></span>

   <span data-ttu-id="c6e97-147">3 \.</span><span class="sxs-lookup"><span data-stu-id="c6e97-147">3\.</span></span> <span data-ttu-id="c6e97-148">Выберите сочетание клавиш Blazorв новом окне браузера: `Shift+Alt+D` в Windows или `Shift+Cmd+D` в macOS.</span><span class="sxs-lookup"><span data-stu-id="c6e97-148">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

   <span data-ttu-id="c6e97-149">4 \.</span><span class="sxs-lookup"><span data-stu-id="c6e97-149">4\.</span></span> <span data-ttu-id="c6e97-150">Вкладка **DevTools** открывается в браузере.</span><span class="sxs-lookup"><span data-stu-id="c6e97-150">The **DevTools** tab opens in the browser.</span></span> <span data-ttu-id="c6e97-151">**Снова выберите вкладку приложения в окне браузера.**</span><span class="sxs-lookup"><span data-stu-id="c6e97-151">**Reselect the app's tab in the browser window.**</span></span>

   <span data-ttu-id="c6e97-152">Чтобы подключить приложение к Visual Studio, см. раздел [Присоединение к процессу в Visual Studio](#attach-to-process-in-visual-studio) .</span><span class="sxs-lookup"><span data-stu-id="c6e97-152">To attach the app to Visual Studio, see the [Attach to process in Visual Studio](#attach-to-process-in-visual-studio) section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c6e97-153">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c6e97-153">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="c6e97-154">Запустите Blazor приложение сборки в `Debug` конфигурации, передав параметр `--configuration Debug` в команду [DotNet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="c6e97-154">Run a Blazor WebAssembly app in `Debug` configuration by passing the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="c6e97-155">Перейдите к приложению по URL-адресу HTTP, показанному в окне оболочки.</span><span class="sxs-lookup"><span data-stu-id="c6e97-155">Navigate to the app at the HTTP URL shown in the shell's window.</span></span>
1. <span data-ttu-id="c6e97-156">Поместите фокус клавиатуры на приложение, а не на панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="c6e97-156">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="c6e97-157">Для этой процедуры лучше не закрывать панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="c6e97-157">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="c6e97-158">После запуска отладки можно повторно открыть панель средств разработчика.</span><span class="sxs-lookup"><span data-stu-id="c6e97-158">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="c6e97-159">Выберите следующие Blazorсочетания клавиш:</span><span class="sxs-lookup"><span data-stu-id="c6e97-159">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="c6e97-160">`Shift+Alt+D` в Windows</span><span class="sxs-lookup"><span data-stu-id="c6e97-160">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="c6e97-161">`Shift+Cmd+D` на macOS</span><span class="sxs-lookup"><span data-stu-id="c6e97-161">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="c6e97-162">Если вы получаете **вкладку "не удается найти браузер для отладки"** , см. раздел [Включение удаленной отладки](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="c6e97-162">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="c6e97-163">После включения удаленной отладки:</span><span class="sxs-lookup"><span data-stu-id="c6e97-163">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="c6e97-164">1 \.</span><span class="sxs-lookup"><span data-stu-id="c6e97-164">1\.</span></span> <span data-ttu-id="c6e97-165">Будет открыто новое окно браузера.</span><span class="sxs-lookup"><span data-stu-id="c6e97-165">A new browser window opens.</span></span> <span data-ttu-id="c6e97-166">Закройте прежнее окно.</span><span class="sxs-lookup"><span data-stu-id="c6e97-166">Close the prior window.</span></span>

   <span data-ttu-id="c6e97-167">2 \.</span><span class="sxs-lookup"><span data-stu-id="c6e97-167">2\.</span></span> <span data-ttu-id="c6e97-168">Поместите фокус клавиатуры на приложение в окне браузера, а не на панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="c6e97-168">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span>

   <span data-ttu-id="c6e97-169">3 \.</span><span class="sxs-lookup"><span data-stu-id="c6e97-169">3\.</span></span> <span data-ttu-id="c6e97-170">Выберите сочетание клавиш Blazorв новом окне браузера: `Shift+Alt+D` в Windows или `Shift+Cmd+D` в macOS.</span><span class="sxs-lookup"><span data-stu-id="c6e97-170">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

---

## <a name="enable-remote-debugging"></a><span data-ttu-id="c6e97-171">Включение удаленной отладки</span><span class="sxs-lookup"><span data-stu-id="c6e97-171">Enable remote debugging</span></span>

<span data-ttu-id="c6e97-172">Если удаленная отладка отключена, то **не удается найти отладочную страницу ошибки вкладки браузера** создается Chrome.</span><span class="sxs-lookup"><span data-stu-id="c6e97-172">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="c6e97-173">На странице ошибки содержатся инструкции для запуска Chrome с открытым портом отладки, чтобы Blazor прокси-сервер отладки мог подключиться к приложению.</span><span class="sxs-lookup"><span data-stu-id="c6e97-173">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="c6e97-174">*Закройте все экземпляры Chrome* и перезапустите Chrome с указанием.</span><span class="sxs-lookup"><span data-stu-id="c6e97-174">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="c6e97-175">Отладка приложения</span><span class="sxs-lookup"><span data-stu-id="c6e97-175">Debug the app</span></span>

<span data-ttu-id="c6e97-176">После того как Chrome будет запущен с включенной удаленной отладкой, откроется новая вкладка отладчика. Через некоторое время на вкладке **источники** отображается список сборок .NET в приложении.</span><span class="sxs-lookup"><span data-stu-id="c6e97-176">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="c6e97-177">Разверните каждую сборку и найдите исходные файлы *cs*/ *. Razor* , доступные для отладки.</span><span class="sxs-lookup"><span data-stu-id="c6e97-177">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="c6e97-178">Установите точки останова, переключитесь обратно на вкладку приложения, и точки останова будут достигнуты при выполнении кода.</span><span class="sxs-lookup"><span data-stu-id="c6e97-178">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="c6e97-179">После попадания в точку останова один шаг (`F10`) через код или возобновление (`F8`) выполнение кода обычно.</span><span class="sxs-lookup"><span data-stu-id="c6e97-179">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="c6e97-180"> предоставляет прокси-сервер отладки, реализующий [протокол Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) и дополненный протоколом. Сведения, относящиеся к сети.</span><span class="sxs-lookup"><span data-stu-id="c6e97-180"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="c6e97-181">Когда нажата клавиша Отладка, Blazor указывает Chrome DevTools на прокси-сервере.</span><span class="sxs-lookup"><span data-stu-id="c6e97-181">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="c6e97-182">Прокси-сервер подключается к окну браузера, для которого выполняется отладка (следовательно, необходимо включить удаленную отладку).</span><span class="sxs-lookup"><span data-stu-id="c6e97-182">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="attach-to-process-in-visual-studio"></a><span data-ttu-id="c6e97-183">Присоединение к процессу в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6e97-183">Attach to process in Visual Studio</span></span>

<span data-ttu-id="c6e97-184">Присоединение к процессу приложения в Visual Studio — это *временный* сценарий отладки для Blazor сборки, а отладка **F5** — в разработке.</span><span class="sxs-lookup"><span data-stu-id="c6e97-184">Attaching to the app's process in Visual Studio is a *temporary* debugging scenario for Blazor WebAssembly while **F5** debugging is in development.</span></span>

<span data-ttu-id="c6e97-185">Чтобы подключить процесс выполняющегося приложения к Visual Studio, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="c6e97-185">To attach the running app's process to Visual Studio:</span></span>

1. <span data-ttu-id="c6e97-186">В Visual Studio выберите **отладка** > **присоединить к процессу**.</span><span class="sxs-lookup"><span data-stu-id="c6e97-186">In Visual Studio, select **Debug** > **Attach to Process**.</span></span>
1. <span data-ttu-id="c6e97-187">Для **типа подключения**выберите **Chrome Devtools протокол WebSocket (без проверки подлинности)** .</span><span class="sxs-lookup"><span data-stu-id="c6e97-187">For the **Connection type**, select **Chrome devtools protocol websocket (no authentication)**.</span></span>
1. <span data-ttu-id="c6e97-188">В качестве **цели подключения**вставьте адрес HTTP (а не HTTPS-адрес) приложения.</span><span class="sxs-lookup"><span data-stu-id="c6e97-188">For the **Connection target**, paste in the HTTP address (not the HTTPS address) of the app.</span></span>
1. <span data-ttu-id="c6e97-189">Выберите **Обновить** , чтобы обновить записи в разделе **Доступные процессы**.</span><span class="sxs-lookup"><span data-stu-id="c6e97-189">Select **Refresh** to refresh the entries under **Available processes**.</span></span>
1. <span data-ttu-id="c6e97-190">Выберите процесс браузера для отладки и щелкните **присоединить**.</span><span class="sxs-lookup"><span data-stu-id="c6e97-190">Select the browser process to debug and select **Attach**.</span></span>
1. <span data-ttu-id="c6e97-191">В диалоговом окне **Выбор типа кода** выберите тип кода для конкретного браузера, к которому выполняется подключение (ребро или Chrome), а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="c6e97-191">In the **Select Code Type** dialog, select the code type for the specific browser you're attaching to (Edge or Chrome) and then select **OK**.</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="c6e97-192">Карты исходного кода браузера</span><span class="sxs-lookup"><span data-stu-id="c6e97-192">Browser source maps</span></span>

<span data-ttu-id="c6e97-193">Карты источников браузера позволяют браузеру сопоставлять скомпилированные файлы с исходными файлами и обычно используются для отладки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="c6e97-193">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="c6e97-194">Однако Blazor в настоящее время не C# сопоставляется непосредственно с JavaScript/васм.</span><span class="sxs-lookup"><span data-stu-id="c6e97-194">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="c6e97-195">Вместо этого Blazor выполняет интерпретацию IL в браузере, поэтому исходные карты не актуальны.</span><span class="sxs-lookup"><span data-stu-id="c6e97-195">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="c6e97-196">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="c6e97-196">Troubleshoot</span></span>

<span data-ttu-id="c6e97-197">Если вы используете ошибки, следующий совет может помочь:</span><span class="sxs-lookup"><span data-stu-id="c6e97-197">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="c6e97-198">На вкладке **отладчик** откройте Инструменты разработчика в браузере.</span><span class="sxs-lookup"><span data-stu-id="c6e97-198">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="c6e97-199">В консоли выполните `localStorage.clear()`, чтобы удалить все точки останова.</span><span class="sxs-lookup"><span data-stu-id="c6e97-199">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
