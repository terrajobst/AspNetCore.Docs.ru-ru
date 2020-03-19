---
title: Начало работы с Blazor ASP.NET Core
author: guardrex
description: Приступите к работе с Blazor, создав приложение Blazor с помощью предпочтительных средств.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 89c7529d2b8ec97db731f7c7268e19937c398115
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083239"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="e2308-103">Начало работы с MVC ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="e2308-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="e2308-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e2308-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="e2308-105">Начало работы с Blazor:</span><span class="sxs-lookup"><span data-stu-id="e2308-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="e2308-106">Установите [пакет SDK для .NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="e2308-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="e2308-107">Дополнительно установите шаблон [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly).</span><span class="sxs-lookup"><span data-stu-id="e2308-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="e2308-108">Установите [пакет SDK для .NET Core 3.1.102 или более поздней версии (предварительная версия)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="e2308-108">Install the [.NET Core 3.1.102 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="e2308-109">В командной оболочке выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="e2308-109">Run the following command in a command shell.</span></span> <span data-ttu-id="e2308-110">В период действия предварительной версии Blazor WebAssembly будет использоваться предварительная версия пакета [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/).</span><span class="sxs-lookup"><span data-stu-id="e2308-110">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
   ```

   > [!NOTE]
   > <span data-ttu-id="e2308-111">Пакет SDK для .NET Core версии 3.1.102 или более поздней **обязательно** должен использовать шаблон Blazor WebAssembly 3.2, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="e2308-111">.NET Core SDK version 3.1.102 or later is **required** to use the 3.2 Preview 2 Blazor WebAssembly template.</span></span> <span data-ttu-id="e2308-112">Подтвердите установленную версию пакет SDK для .NET Core, запустив `dotnet --version` в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="e2308-112">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="e2308-113">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="e2308-113">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studio"></a>[<span data-ttu-id="e2308-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2308-114">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="e2308-115">1\.</span><span class="sxs-lookup"><span data-stu-id="e2308-115">1\.</span></span> <span data-ttu-id="e2308-116">Установите [Visual Studio 2019 16.4 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка**.</span><span class="sxs-lookup"><span data-stu-id="e2308-116">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="e2308-117">2\.</span><span class="sxs-lookup"><span data-stu-id="e2308-117">2\.</span></span> <span data-ttu-id="e2308-118">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="e2308-118">Create a new project.</span></span>

   <span data-ttu-id="e2308-119">3\.</span><span class="sxs-lookup"><span data-stu-id="e2308-119">3\.</span></span> <span data-ttu-id="e2308-120">Выберите **Приложение Blazor**.</span><span class="sxs-lookup"><span data-stu-id="e2308-120">Select **Blazor App**.</span></span> <span data-ttu-id="e2308-121">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e2308-121">Select **Next**.</span></span>

   <span data-ttu-id="e2308-122">4\.</span><span class="sxs-lookup"><span data-stu-id="e2308-122">4\.</span></span> <span data-ttu-id="e2308-123">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e2308-123">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="e2308-124">Убедитесь, что для проекта правильно указано существующее **расположение** или укажите новое.</span><span class="sxs-lookup"><span data-stu-id="e2308-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="e2308-125">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e2308-125">Select **Create**.</span></span>

   <span data-ttu-id="e2308-126">5\.</span><span class="sxs-lookup"><span data-stu-id="e2308-126">5\.</span></span> <span data-ttu-id="e2308-127">Для работы с Blazor WebAssembly выберите шаблон **Приложение WebAssembly Blazor**.</span><span class="sxs-lookup"><span data-stu-id="e2308-127">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="e2308-128">Для работы с Blazor Server выберите шаблон **Серверное приложение Blazor**.</span><span class="sxs-lookup"><span data-stu-id="e2308-128">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="e2308-129">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e2308-129">Select **Create**.</span></span> <span data-ttu-id="e2308-130">Сведения о двух моделях размещения Blazor, *Blazor Server* и *Blazor WebAssembly*, см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e2308-130">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span> <span data-ttu-id="e2308-131">Если шаблон Blazor WebAssembly отсутствует, вернитесь к предыдущему шагу и переустановите шаблон.</span><span class="sxs-lookup"><span data-stu-id="e2308-131">If the Blazor WebAssembly template isn't present, return to the previous step and reinstall the template.</span></span>

   <span data-ttu-id="e2308-132">6\.</span><span class="sxs-lookup"><span data-stu-id="e2308-132">6\.</span></span> <span data-ttu-id="e2308-133">Нажмите клавишу **Ctrl**+**F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="e2308-133">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e2308-134">Если вы установили расширение Visual Studio Blazor для предыдущей предварительной версии ASP.NET Core Blazor (предварительная версия 6 или более ранняя версия), можно удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="e2308-134">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="e2308-135">Установки шаблонов Blazor в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2308-135">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-code"></a>[<span data-ttu-id="e2308-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e2308-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="e2308-137">1\.</span><span class="sxs-lookup"><span data-stu-id="e2308-137">1\.</span></span> <span data-ttu-id="e2308-138">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="e2308-138">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="e2308-139">2\.</span><span class="sxs-lookup"><span data-stu-id="e2308-139">2\.</span></span> <span data-ttu-id="e2308-140">Установите актуальную версию [расширения C# для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span><span class="sxs-lookup"><span data-stu-id="e2308-140">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span></span>

   <span data-ttu-id="e2308-141">3\.</span><span class="sxs-lookup"><span data-stu-id="e2308-141">3\.</span></span> <span data-ttu-id="e2308-142">Для работы с Blazor WebAssembly выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e2308-142">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="e2308-143">Для работы с Blazor Server выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e2308-143">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="e2308-144">Сведения о двух моделях размещения Blazor, *Blazor Server* и *Blazor WebAssembly*, см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e2308-144">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e2308-145">4\.</span><span class="sxs-lookup"><span data-stu-id="e2308-145">4\.</span></span> <span data-ttu-id="e2308-146">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e2308-146">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="e2308-147">5\.</span><span class="sxs-lookup"><span data-stu-id="e2308-147">5\.</span></span> <span data-ttu-id="e2308-148">Для проекта Blazor Server среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="e2308-148">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="e2308-149">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="e2308-149">Select **Yes**.</span></span>

   <span data-ttu-id="e2308-150">6\.</span><span class="sxs-lookup"><span data-stu-id="e2308-150">6\.</span></span> <span data-ttu-id="e2308-151">При использовании серверного приложения Blazor запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e2308-151">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="e2308-152">Если используется приложение WebAssembly Blazor, выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="e2308-152">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="e2308-153">7\.</span><span class="sxs-lookup"><span data-stu-id="e2308-153">7\.</span></span> <span data-ttu-id="e2308-154">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e2308-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e2308-155">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e2308-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="e2308-156">1\.</span><span class="sxs-lookup"><span data-stu-id="e2308-156">1\.</span></span> <span data-ttu-id="e2308-157">Установите [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="e2308-157">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="e2308-158">2\.</span><span class="sxs-lookup"><span data-stu-id="e2308-158">2\.</span></span> <span data-ttu-id="e2308-159">Выберите **Файл** > **Создать решение** или создайте **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="e2308-159">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="e2308-160">3\.</span><span class="sxs-lookup"><span data-stu-id="e2308-160">3\.</span></span> <span data-ttu-id="e2308-161">На боковой панели выберите **.NET Core** > **Приложение**.</span><span class="sxs-lookup"><span data-stu-id="e2308-161">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="e2308-162">4\.</span><span class="sxs-lookup"><span data-stu-id="e2308-162">4\.</span></span> <span data-ttu-id="e2308-163">Выберите шаблон **Серверное приложение Blazor**.</span><span class="sxs-lookup"><span data-stu-id="e2308-163">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="e2308-164">В Visual Studio для Mac в данный момент доступен только шаблон Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="e2308-164">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="e2308-165">Для работы с Blazor WebAssembly следуйте инструкциям на вкладке **.NET Core CLI**. После выбора шаблона Blazor Server нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e2308-165">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="e2308-166">Сведения о двух моделях размещения Blazor, *Blazor Server* и *Blazor WebAssembly*, см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e2308-166">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="e2308-167">5\.</span><span class="sxs-lookup"><span data-stu-id="e2308-167">5\.</span></span> <span data-ttu-id="e2308-168">Задайте для параметра **Целевая платформа** значение **.NET Core 3.1** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e2308-168">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="e2308-169">6\.</span><span class="sxs-lookup"><span data-stu-id="e2308-169">6\.</span></span> <span data-ttu-id="e2308-170">В поле **Имя проекта** присвойте имя приложению `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="e2308-170">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="e2308-171">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e2308-171">Select **Create**.</span></span>

   <span data-ttu-id="e2308-172">7\.</span><span class="sxs-lookup"><span data-stu-id="e2308-172">7\.</span></span> <span data-ttu-id="e2308-173">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение *без отладчика*.</span><span class="sxs-lookup"><span data-stu-id="e2308-173">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="e2308-174">Запустите приложение с помощью кнопки **Начать отладку**, чтобы запустить приложение *с отладчиком*.</span><span class="sxs-lookup"><span data-stu-id="e2308-174">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="e2308-175">При появлении подтверждения доверия к сертификату разработки подтвердите доверие, чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="e2308-175">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-cli"></a>[<span data-ttu-id="e2308-176">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2308-176">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="e2308-177">Для работы с Blazor WebAssembly выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e2308-177">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="e2308-178">Для работы с Blazor Server выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e2308-178">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="e2308-179">Сведения о двух моделях размещения Blazor, *Blazor Server* и *Blazor WebAssembly*, см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e2308-179">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e2308-180">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e2308-180">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="e2308-181">На вкладках на боковой панели доступно несколько страниц:</span><span class="sxs-lookup"><span data-stu-id="e2308-181">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="e2308-182">Главная страница</span><span class="sxs-lookup"><span data-stu-id="e2308-182">Home</span></span>
* <span data-ttu-id="e2308-183">Счетчик</span><span class="sxs-lookup"><span data-stu-id="e2308-183">Counter</span></span>
* <span data-ttu-id="e2308-184">Выборка данных</span><span class="sxs-lookup"><span data-stu-id="e2308-184">Fetch data</span></span>

<span data-ttu-id="e2308-185">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="e2308-185">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="e2308-186">Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, однако с Blazor можно использовать C#.</span><span class="sxs-lookup"><span data-stu-id="e2308-186">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="e2308-187">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="e2308-187">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="e2308-188">Запрос `/counter` в браузере, как указано в директиве `@page` в верхней части, заставляет компонент `Counter` визуализировать свое содержимое.</span><span class="sxs-lookup"><span data-stu-id="e2308-188">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="e2308-189">Компоненты преобразуются в представление дерева визуализации. Его затем можно использовать для гибкого и эффективного обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="e2308-189">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="e2308-190">Каждый раз при нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="e2308-190">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="e2308-191">Запускается событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="e2308-191">The `onclick` event is fired.</span></span>
* <span data-ttu-id="e2308-192">вызывается метод `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="e2308-192">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="e2308-193">Увеличивается значение `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="e2308-193">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="e2308-194">Компонент визуализируется снова.</span><span class="sxs-lookup"><span data-stu-id="e2308-194">The component is rendered again.</span></span>

<span data-ttu-id="e2308-195">Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет к модели DOM только измененное содержимое.</span><span class="sxs-lookup"><span data-stu-id="e2308-195">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="e2308-196">Добавьте компонент в другой компонент, используя синтаксис HTML.</span><span class="sxs-lookup"><span data-stu-id="e2308-196">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="e2308-197">Например, добавьте компонент `Counter` на домашнюю страницу приложения, разместив элемент `<Counter />` внутри компонента `Index`.</span><span class="sxs-lookup"><span data-stu-id="e2308-197">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="e2308-198">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="e2308-198">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="e2308-199">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e2308-199">Run the app.</span></span> <span data-ttu-id="e2308-200">На домашней странице имеется собственный счетчик, предоставленный компонентом `Counter`.</span><span class="sxs-lookup"><span data-stu-id="e2308-200">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="e2308-201">Параметры компонента указываются с помощью атрибутов или [дочернего содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="e2308-201">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="e2308-202">Чтобы добавить параметр в компонент `Counter`, обновите блок `@code`компонента:</span><span class="sxs-lookup"><span data-stu-id="e2308-202">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="e2308-203">Добавьте открытое свойство для `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="e2308-203">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="e2308-204">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="e2308-204">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="e2308-205">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="e2308-205">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="e2308-206">Укажите `IncrementAmount` в элементе `<Counter>` для компонента `Index`, используя атрибут.</span><span class="sxs-lookup"><span data-stu-id="e2308-206">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="e2308-207">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="e2308-207">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="e2308-208">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e2308-208">Run the app.</span></span> <span data-ttu-id="e2308-209">Теперь у компонента `Index` будет собственный счетчик, который будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь).</span><span class="sxs-lookup"><span data-stu-id="e2308-209">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="e2308-210">Компонент `Counter` (*Counter.razor*) в компоненте `/counter` продолжает увеличиваться на единицу.</span><span class="sxs-lookup"><span data-stu-id="e2308-210">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2308-211">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e2308-211">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="e2308-212">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e2308-212">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
