---
title: Отладка в ASP.NET Core Blazor
author: guardrex
description: Узнайте, как выполнять отладку приложений Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78648316"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="3c19f-103">Отладка в ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="3c19f-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="3c19f-104">Дэниэл Рот (Daniel Roth)</span><span class="sxs-lookup"><span data-stu-id="3c19f-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3c19f-105">Предоставляется *ранняя* поддержка для отладки Blazor WebAssembly с помощью средств разработки браузера в браузерах на основе Chromium (Chrome/Edge).</span><span class="sxs-lookup"><span data-stu-id="3c19f-105">*Early* support exists for debugging Blazor WebAssembly using the browser dev tools in Chromium-based browsers (Chrome/Edge).</span></span> <span data-ttu-id="3c19f-106">Ведется работа над следующими задачами:</span><span class="sxs-lookup"><span data-stu-id="3c19f-106">Work is in progress to:</span></span>

* <span data-ttu-id="3c19f-107">Полная поддержка отладки в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c19f-107">Fully enable debugging in Visual Studio.</span></span>
* <span data-ttu-id="3c19f-108">Поддержка отладки в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3c19f-108">Enable debugging in Visual Studio Code.</span></span>

<span data-ttu-id="3c19f-109">Возможности отладчика ограничены.</span><span class="sxs-lookup"><span data-stu-id="3c19f-109">Debugger capabilities are limited.</span></span> <span data-ttu-id="3c19f-110">Поддерживаются следующие сценарии:</span><span class="sxs-lookup"><span data-stu-id="3c19f-110">Available scenarios include:</span></span>

* <span data-ttu-id="3c19f-111">Установка и удаление точек останова.</span><span class="sxs-lookup"><span data-stu-id="3c19f-111">Set and remove breakpoints.</span></span>
* <span data-ttu-id="3c19f-112">Пошаговое выполнение кода (`F10`) или возобновление выполнения кода (`F8`).</span><span class="sxs-lookup"><span data-stu-id="3c19f-112">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="3c19f-113">На экране *Локальные переменные* просмотрите значения локальных переменных типа `int`, `string` и `bool`.</span><span class="sxs-lookup"><span data-stu-id="3c19f-113">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="3c19f-114">См. стек вызовов, включая цепочки вызовов, которые поступают из JavaScript в .NET и из .NET в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3c19f-114">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="3c19f-115">Вы *не можете*:</span><span class="sxs-lookup"><span data-stu-id="3c19f-115">You *can't*:</span></span>

* <span data-ttu-id="3c19f-116">Просматривать значения локальных переменных, кроме `int`, `string` и `bool`.</span><span class="sxs-lookup"><span data-stu-id="3c19f-116">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="3c19f-117">Просматривать значения свойств или полей класса.</span><span class="sxs-lookup"><span data-stu-id="3c19f-117">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="3c19f-118">Просматривать значения, наводя на переменные указатель мыши.</span><span class="sxs-lookup"><span data-stu-id="3c19f-118">Hover over variables to see their values.</span></span>
* <span data-ttu-id="3c19f-119">Вычислять выражения в консоли.</span><span class="sxs-lookup"><span data-stu-id="3c19f-119">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="3c19f-120">Задействовать асинхронные вызовы.</span><span class="sxs-lookup"><span data-stu-id="3c19f-120">Step across async calls.</span></span>
* <span data-ttu-id="3c19f-121">Выполнять большинство других обычных сценариев отладки.</span><span class="sxs-lookup"><span data-stu-id="3c19f-121">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="3c19f-122">Группа инженеров занимается разработкой дальнейших сценариев отладки.</span><span class="sxs-lookup"><span data-stu-id="3c19f-122">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c19f-123">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3c19f-123">Prerequisites</span></span>

<span data-ttu-id="3c19f-124">Для отладки требуется один из следующих браузеров:</span><span class="sxs-lookup"><span data-stu-id="3c19f-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="3c19f-125">Google Chrome (версии 70 и более поздних)</span><span class="sxs-lookup"><span data-stu-id="3c19f-125">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="3c19f-126">Предварительная версия Microsoft Edge ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="3c19f-126">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="3c19f-127">Процедура</span><span class="sxs-lookup"><span data-stu-id="3c19f-127">Procedure</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="3c19f-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c19f-128">Visual Studio</span></span>](#tab/visual-studio)

> [!WARNING]
> <span data-ttu-id="3c19f-129">Поддержка отладки в Visual Studio находится на раннем этапе разработки.</span><span class="sxs-lookup"><span data-stu-id="3c19f-129">Debugging support in Visual Studio is at an early stage of development.</span></span> <span data-ttu-id="3c19f-130">Отладка с помощью **F5** в настоящее время не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="3c19f-130">**F5** debugging isn't currently supported.</span></span>

1. <span data-ttu-id="3c19f-131">Запустите приложение Blazor WebAssembly в конфигурации `Debug` без отладки (**CTRL**+**F5** вместо **F5**).</span><span class="sxs-lookup"><span data-stu-id="3c19f-131">Run a Blazor WebAssembly app in `Debug` configuration without debugging (**Ctrl**+**F5** instead of **F5**).</span></span>
1. <span data-ttu-id="3c19f-132">Откройте свойства отладки приложения (последняя запись в меню **Отладка**) и скопируйте **URL-адрес приложения** по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c19f-132">Open the Debug properties of the app (last entry in the **Debug** menu) and copy the HTTP **App URL**.</span></span> <span data-ttu-id="3c19f-133">Перейдите по адресу HTTP (а не HTTPS) приложения в браузере на основе Chromium (бета-версия Edge или Chrome).</span><span class="sxs-lookup"><span data-stu-id="3c19f-133">Browse to the HTTP address (not the HTTPS address) of the app using a Chromium-based browser (Edge Beta or Chrome).</span></span>
1. <span data-ttu-id="3c19f-134">Поместите фокус клавиатуры на приложение в окне браузера, а не на панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="3c19f-134">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span> <span data-ttu-id="3c19f-135">Для этой процедуры лучше закрыть панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="3c19f-135">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="3c19f-136">После запуска отладки можно повторно открыть панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="3c19f-136">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="3c19f-137">Нажмите следующее сочетание клавиш для Blazor:</span><span class="sxs-lookup"><span data-stu-id="3c19f-137">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="3c19f-138">`Shift+Alt+D` в Windows</span><span class="sxs-lookup"><span data-stu-id="3c19f-138">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="3c19f-139">`Shift+Cmd+D` в macOS</span><span class="sxs-lookup"><span data-stu-id="3c19f-139">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="3c19f-140">Если отобразится сообщение **Не удается найти вкладку браузера для отладки**, см. раздел [Включение удаленной отладки](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="3c19f-140">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="3c19f-141">После включения удаленной отладки:</span><span class="sxs-lookup"><span data-stu-id="3c19f-141">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="3c19f-142">1\.</span><span class="sxs-lookup"><span data-stu-id="3c19f-142">1\.</span></span> <span data-ttu-id="3c19f-143">Откроется новое окно браузера.</span><span class="sxs-lookup"><span data-stu-id="3c19f-143">A new browser window opens.</span></span> <span data-ttu-id="3c19f-144">Закройте прежнее окно.</span><span class="sxs-lookup"><span data-stu-id="3c19f-144">Close the prior window.</span></span>

   <span data-ttu-id="3c19f-145">2\.</span><span class="sxs-lookup"><span data-stu-id="3c19f-145">2\.</span></span> <span data-ttu-id="3c19f-146">Поместите фокус клавиатуры на приложение в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="3c19f-146">Place the keyboard focus on the app in the browser window.</span></span>

   <span data-ttu-id="3c19f-147">3\.</span><span class="sxs-lookup"><span data-stu-id="3c19f-147">3\.</span></span> <span data-ttu-id="3c19f-148">Нажмите сочетание клавиш для Blazor в новом окне браузера: `Shift+Alt+D` в Windows или `Shift+Cmd+D` в macOS.</span><span class="sxs-lookup"><span data-stu-id="3c19f-148">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

   <span data-ttu-id="3c19f-149">4\.</span><span class="sxs-lookup"><span data-stu-id="3c19f-149">4\.</span></span> <span data-ttu-id="3c19f-150">В браузере откроется вкладка **DevTools**.</span><span class="sxs-lookup"><span data-stu-id="3c19f-150">The **DevTools** tab opens in the browser.</span></span> <span data-ttu-id="3c19f-151">**Снова выберите вкладку приложения в окне браузера.**</span><span class="sxs-lookup"><span data-stu-id="3c19f-151">**Reselect the app's tab in the browser window.**</span></span>

   <span data-ttu-id="3c19f-152">Чтобы подключить приложение к Visual Studio, см. раздел [Присоединение к процессу в Visual Studio](#attach-to-process-in-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="3c19f-152">To attach the app to Visual Studio, see the [Attach to process in Visual Studio](#attach-to-process-in-visual-studio) section.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="3c19f-153">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="3c19f-153">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="3c19f-154">Запустите приложение Blazor WebAssembly в конфигурации `Debug`, передав параметр `--configuration Debug` в команду [dotnet run](/dotnet/core/tools/dotnet-run): `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="3c19f-154">Run a Blazor WebAssembly app in `Debug` configuration by passing the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="3c19f-155">Перейдите к приложению по URL-адресу HTTP, показанному в окне оболочки.</span><span class="sxs-lookup"><span data-stu-id="3c19f-155">Navigate to the app at the HTTP URL shown in the shell's window.</span></span>
1. <span data-ttu-id="3c19f-156">Поместите фокус клавиатуры на приложение, а не на панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="3c19f-156">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="3c19f-157">Для этой процедуры лучше закрыть панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="3c19f-157">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="3c19f-158">После запуска отладки можно повторно открыть панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="3c19f-158">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="3c19f-159">Нажмите следующее сочетание клавиш для Blazor:</span><span class="sxs-lookup"><span data-stu-id="3c19f-159">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="3c19f-160">`Shift+Alt+D` в Windows</span><span class="sxs-lookup"><span data-stu-id="3c19f-160">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="3c19f-161">`Shift+Cmd+D` в macOS</span><span class="sxs-lookup"><span data-stu-id="3c19f-161">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="3c19f-162">Если отобразится сообщение **Не удается найти вкладку браузера для отладки**, см. раздел [Включение удаленной отладки](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="3c19f-162">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="3c19f-163">После включения удаленной отладки:</span><span class="sxs-lookup"><span data-stu-id="3c19f-163">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="3c19f-164">1\.</span><span class="sxs-lookup"><span data-stu-id="3c19f-164">1\.</span></span> <span data-ttu-id="3c19f-165">Откроется новое окно браузера.</span><span class="sxs-lookup"><span data-stu-id="3c19f-165">A new browser window opens.</span></span> <span data-ttu-id="3c19f-166">Закройте прежнее окно.</span><span class="sxs-lookup"><span data-stu-id="3c19f-166">Close the prior window.</span></span>

   <span data-ttu-id="3c19f-167">2\.</span><span class="sxs-lookup"><span data-stu-id="3c19f-167">2\.</span></span> <span data-ttu-id="3c19f-168">Поместите фокус клавиатуры на приложение в окне браузера, а не на панель инструментов разработчика.</span><span class="sxs-lookup"><span data-stu-id="3c19f-168">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span>

   <span data-ttu-id="3c19f-169">3\.</span><span class="sxs-lookup"><span data-stu-id="3c19f-169">3\.</span></span> <span data-ttu-id="3c19f-170">Нажмите сочетание клавиш для Blazor в новом окне браузера: `Shift+Alt+D` в Windows или `Shift+Cmd+D` в macOS.</span><span class="sxs-lookup"><span data-stu-id="3c19f-170">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

---

## <a name="enable-remote-debugging"></a><span data-ttu-id="3c19f-171">Включение удаленной отладки</span><span class="sxs-lookup"><span data-stu-id="3c19f-171">Enable remote debugging</span></span>

<span data-ttu-id="3c19f-172">Если удаленная отладка отключена, в Chrome открывается страница ошибки **Не удается найти вкладку браузера для отладки**.</span><span class="sxs-lookup"><span data-stu-id="3c19f-172">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="3c19f-173">На странице ошибки содержатся инструкции для запуска Chrome с открытым портом отладки, чтобы прокси-сервер отладки Blazor мог подключиться к приложению.</span><span class="sxs-lookup"><span data-stu-id="3c19f-173">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="3c19f-174">*Закройте все экземпляры Chrome* и перезапустите Chrome.</span><span class="sxs-lookup"><span data-stu-id="3c19f-174">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="3c19f-175">Отладка приложения</span><span class="sxs-lookup"><span data-stu-id="3c19f-175">Debug the app</span></span>

<span data-ttu-id="3c19f-176">Когда Chrome будет запущен с включенной удаленной отладкой, откройте новую вкладку отладчика с помощью сочетания клавиш. Через некоторое время на вкладке **Источники** отобразится список сборок .NET в приложении.</span><span class="sxs-lookup"><span data-stu-id="3c19f-176">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="3c19f-177">Разверните каждую сборку и найдите исходные файлы *CS*/*RAZOR*, доступные для отладки.</span><span class="sxs-lookup"><span data-stu-id="3c19f-177">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="3c19f-178">Установите точки останова, переключитесь обратно на вкладку приложения, и точки останова будут достигнуты при выполнении кода.</span><span class="sxs-lookup"><span data-stu-id="3c19f-178">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="3c19f-179">После попадания в точку останова выполняйте код пошагово (`F10`) или возобновите его обычное выполнение (`F8`).</span><span class="sxs-lookup"><span data-stu-id="3c19f-179">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="3c19f-180"> предоставляет прокси-сервер отладки, который реализует протокол [Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) и дополняет его сведениями, относящимися к .NET.</span><span class="sxs-lookup"><span data-stu-id="3c19f-180"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="3c19f-181">При нажатии сочетания клавиш для отладки Blazor указывает Chrome DevTools на прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="3c19f-181">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="3c19f-182">Прокси-сервер подключается к окну браузера, для которого выполняется отладка (поэтому и надо было включить удаленную отладку).</span><span class="sxs-lookup"><span data-stu-id="3c19f-182">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="attach-to-process-in-visual-studio"></a><span data-ttu-id="3c19f-183">Присоединение к процессу в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c19f-183">Attach to process in Visual Studio</span></span>

<span data-ttu-id="3c19f-184">Присоединение к процессу приложения в Visual Studio — это *временный* сценарий отладки для WebAssembly Blazor, пока отладка с помощью **F5** находится в разработке.</span><span class="sxs-lookup"><span data-stu-id="3c19f-184">Attaching to the app's process in Visual Studio is a *temporary* debugging scenario for Blazor WebAssembly while **F5** debugging is in development.</span></span>

<span data-ttu-id="3c19f-185">Чтобы присоединить процесс выполняющегося приложения к Visual Studio, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="3c19f-185">To attach the running app's process to Visual Studio:</span></span>

1. <span data-ttu-id="3c19f-186">В Visual Studio выберите пункт **Отладка** > **Присоединить к процессу**.</span><span class="sxs-lookup"><span data-stu-id="3c19f-186">In Visual Studio, select **Debug** > **Attach to Process**.</span></span>
1. <span data-ttu-id="3c19f-187">Для параметра **Тип соединения** выберите **WebSocket для протокола Chrome devtools (без проверки подлинности)** .</span><span class="sxs-lookup"><span data-stu-id="3c19f-187">For the **Connection type**, select **Chrome devtools protocol websocket (no authentication)**.</span></span>
1. <span data-ttu-id="3c19f-188">Для параметра **Целевой объект подключения** вставьте адрес HTTP (не HTTPS) приложения.</span><span class="sxs-lookup"><span data-stu-id="3c19f-188">For the **Connection target**, paste in the HTTP address (not the HTTPS address) of the app.</span></span>
1. <span data-ttu-id="3c19f-189">Выберите **Обновить**, чтобы обновить записи в разделе **Доступные процессы**.</span><span class="sxs-lookup"><span data-stu-id="3c19f-189">Select **Refresh** to refresh the entries under **Available processes**.</span></span>
1. <span data-ttu-id="3c19f-190">Выберите процесс браузера для отладки и нажмите **Присоединить**.</span><span class="sxs-lookup"><span data-stu-id="3c19f-190">Select the browser process to debug and select **Attach**.</span></span>
1. <span data-ttu-id="3c19f-191">В диалоговом окне **Выбрать тип кода** выберите тип кода для конкретного браузера, к которому вы подключаетесь (Edge или Chrome), а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3c19f-191">In the **Select Code Type** dialog, select the code type for the specific browser you're attaching to (Edge or Chrome) and then select **OK**.</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="3c19f-192">Сопоставители с исходным кодом в браузере</span><span class="sxs-lookup"><span data-stu-id="3c19f-192">Browser source maps</span></span>

<span data-ttu-id="3c19f-193">Сопоставители с исходным кодом в браузере позволяют браузеру сопоставлять скомпилированные файлы с исходными файлами и обычно используются для отладки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="3c19f-193">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="3c19f-194">Однако Blazor в настоящее время не сопоставляет C# напрямую с JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="3c19f-194">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="3c19f-195">Вместо этого Blazor выполняет интерпретацию IL в браузере, поэтому сопоставление с исходным кодом не актуально.</span><span class="sxs-lookup"><span data-stu-id="3c19f-195">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="3c19f-196">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="3c19f-196">Troubleshoot</span></span>

<span data-ttu-id="3c19f-197">При возникновении ошибок вам поможет следующий совет:</span><span class="sxs-lookup"><span data-stu-id="3c19f-197">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="3c19f-198">На вкладке **Отладчик** откройте средства разработчика в браузере.</span><span class="sxs-lookup"><span data-stu-id="3c19f-198">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="3c19f-199">В консоли выполните `localStorage.clear()`, чтобы удалить все точки останова.</span><span class="sxs-lookup"><span data-stu-id="3c19f-199">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
