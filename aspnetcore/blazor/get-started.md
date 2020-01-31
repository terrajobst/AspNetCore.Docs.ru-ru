---
title: Начало работы с Blazor ASP.NET Core
author: guardrex
description: Приступайте к работе с Blazor, создав Blazor приложение с любыми инструментами по своему усмотрению.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: bd33d874b3d6122f2ab820e9b147b0e62ba03a58
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869584"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="56e76-103">Начало работы с ASP.NET Core Блазор</span><span class="sxs-lookup"><span data-stu-id="56e76-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="56e76-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="56e76-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="56e76-105">Начало работы с Блазор:</span><span class="sxs-lookup"><span data-stu-id="56e76-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="56e76-106">Установите [пакет SDK для .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="56e76-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="56e76-107">При необходимости установите шаблон [блазор для сборки](xref:blazor/hosting-models#blazor-webassembly) .</span><span class="sxs-lookup"><span data-stu-id="56e76-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="56e76-108">Установите [пакет SDK для .NET Core 3,1 или более поздней версии (Предварительная версия)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="56e76-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="56e76-109">Выполните следующую команду в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="56e76-109">Run the following command in a command shell.</span></span> <span data-ttu-id="56e76-110">Пакет [Microsoft. AspNetCore. блазор. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) имеет предварительную версию, а блазор уже находится в предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="56e76-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
   ```

1. <span data-ttu-id="56e76-111">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="56e76-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="56e76-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56e76-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="56e76-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-113">1\.</span></span> <span data-ttu-id="56e76-114">Установите [Visual Studio 2019 версии 16,4 или более поздней](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="56e76-114">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="56e76-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-115">2\.</span></span> <span data-ttu-id="56e76-116">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="56e76-116">Create a new project.</span></span>

   <span data-ttu-id="56e76-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-117">3\.</span></span> <span data-ttu-id="56e76-118">Выберите **приложение блазор**.</span><span class="sxs-lookup"><span data-stu-id="56e76-118">Select **Blazor App**.</span></span> <span data-ttu-id="56e76-119">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="56e76-119">Select **Next**.</span></span>

   <span data-ttu-id="56e76-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-120">4\.</span></span> <span data-ttu-id="56e76-121">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="56e76-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="56e76-122">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="56e76-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="56e76-123">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="56e76-123">Select **Create**.</span></span>

   <span data-ttu-id="56e76-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-124">5\.</span></span> <span data-ttu-id="56e76-125">Для интерфейса Блазор можно выбрать шаблон **приложения блазор для сборки** .</span><span class="sxs-lookup"><span data-stu-id="56e76-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="56e76-126">Для удобства работы с сервером Блазор выберите шаблон **серверное приложение блазор** .</span><span class="sxs-lookup"><span data-stu-id="56e76-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="56e76-127">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="56e76-127">Select **Create**.</span></span> <span data-ttu-id="56e76-128">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="56e76-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="56e76-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-129">6\.</span></span> <span data-ttu-id="56e76-130">Нажмите клавишу **Ctrl**+**F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="56e76-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="56e76-131">Если вы установили расширение Visual Studio Блазор для предыдущей предварительной версии ASP.NET Core Блазор (Предварительная версия 6 или более ранняя версия), вы можете удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="56e76-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="56e76-132">Установка шаблонов Блазор в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56e76-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="56e76-133">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="56e76-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="56e76-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-134">1\.</span></span> <span data-ttu-id="56e76-135">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="56e76-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="56e76-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-136">2\.</span></span> <span data-ttu-id="56e76-137">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="56e76-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="56e76-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-138">3\.</span></span> <span data-ttu-id="56e76-139">Для работы с Блазор-сборками выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="56e76-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="56e76-140">Для работы с сервером Блазор выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="56e76-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="56e76-141">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="56e76-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="56e76-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-142">4\.</span></span> <span data-ttu-id="56e76-143">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="56e76-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="56e76-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-144">5\.</span></span> <span data-ttu-id="56e76-145">Для проекта сервера Блазор среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="56e76-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="56e76-146">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="56e76-146">Select **Yes**.</span></span>

   <span data-ttu-id="56e76-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-147">6\.</span></span> <span data-ttu-id="56e76-148">При использовании серверного приложения Блазор запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="56e76-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="56e76-149">Если используется Блазор приложение сборки, выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="56e76-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="56e76-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-150">7\.</span></span> <span data-ttu-id="56e76-151">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="56e76-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="56e76-152">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="56e76-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="56e76-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-153">1\.</span></span> <span data-ttu-id="56e76-154">Установите [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="56e76-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="56e76-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-155">2\.</span></span> <span data-ttu-id="56e76-156">Выберите **файл** > **создать решение** или создать **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="56e76-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="56e76-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-157">3\.</span></span> <span data-ttu-id="56e76-158">На боковой панели выберите > **приложение** **.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="56e76-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="56e76-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-159">4\.</span></span> <span data-ttu-id="56e76-160">Выберите шаблон **серверное приложение блазор** .</span><span class="sxs-lookup"><span data-stu-id="56e76-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="56e76-161">В Visual Studio для Mac в данный момент доступен только шаблон сервера Блазор.</span><span class="sxs-lookup"><span data-stu-id="56e76-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="56e76-162">Для работы с Блазор-сборками следуйте инструкциям на вкладке **.NET Core CLI** . Выбрав шаблон сервера Блазор, нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="56e76-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="56e76-163">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="56e76-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="56e76-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-164">5\.</span></span> <span data-ttu-id="56e76-165">Задайте для параметра **Целевая платформа** значение **.NET Core 3,1** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="56e76-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="56e76-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-166">6\.</span></span> <span data-ttu-id="56e76-167">В поле **имя проекта** Назовите приложение `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="56e76-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="56e76-168">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="56e76-168">Select **Create**.</span></span>

   <span data-ttu-id="56e76-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="56e76-169">7\.</span></span> <span data-ttu-id="56e76-170">Выберите **выполнить** > **выполнить без отладки** , чтобы запустить приложение *без отладчика*.</span><span class="sxs-lookup"><span data-stu-id="56e76-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="56e76-171">Запустите приложение с помощью программы " **начать отладку** ", чтобы запустить приложение *с помощью отладчика*.</span><span class="sxs-lookup"><span data-stu-id="56e76-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="56e76-172">Если появится запрос на доверие к сертификату разработки, то следует доверять сертификату и продолжить.</span><span class="sxs-lookup"><span data-stu-id="56e76-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="56e76-173">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="56e76-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="56e76-174">Для работы с Блазор-сборками выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="56e76-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="56e76-175">Для работы с сервером Блазор выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="56e76-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="56e76-176">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="56e76-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="56e76-177">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="56e76-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="56e76-178">Из вкладок боковой панели доступны несколько страниц:</span><span class="sxs-lookup"><span data-stu-id="56e76-178">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="56e76-179">Домашняя страница</span><span class="sxs-lookup"><span data-stu-id="56e76-179">Home</span></span>
* <span data-ttu-id="56e76-180">Счетчик</span><span class="sxs-lookup"><span data-stu-id="56e76-180">Counter</span></span>
* <span data-ttu-id="56e76-181">Выборка данных</span><span class="sxs-lookup"><span data-stu-id="56e76-181">Fetch data</span></span>

<span data-ttu-id="56e76-182">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="56e76-182">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="56e76-183">Увеличение счетчика на веб-странице обычно требует написания JavaScript, но с Blazor можно использовать C#.</span><span class="sxs-lookup"><span data-stu-id="56e76-183">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="56e76-184">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="56e76-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="56e76-185">Запрос `/counter` в браузере, как указано в директиве `@page` в верхней части, заставляет компонент `Counter` визуализировать свое содержимое.</span><span class="sxs-lookup"><span data-stu-id="56e76-185">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="56e76-186">Компоненты подготавливаются к просмотру в памяти представления дерева подготовки, которое затем может использоваться для обновления пользовательского интерфейса в гибком и удобном виде.</span><span class="sxs-lookup"><span data-stu-id="56e76-186">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="56e76-187">При каждом выборе кнопки « **Click Me** »:</span><span class="sxs-lookup"><span data-stu-id="56e76-187">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="56e76-188">Запускается событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="56e76-188">The `onclick` event is fired.</span></span>
* <span data-ttu-id="56e76-189">Вызывается метод `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="56e76-189">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="56e76-190">`currentCount` увеличивается.</span><span class="sxs-lookup"><span data-stu-id="56e76-190">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="56e76-191">Компонент снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="56e76-191">The component is rendered again.</span></span>

<span data-ttu-id="56e76-192">Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет только измененное содержимое к модель DOM (DOM).</span><span class="sxs-lookup"><span data-stu-id="56e76-192">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="56e76-193">Добавьте компонент в другой компонент с помощью синтаксиса HTML.</span><span class="sxs-lookup"><span data-stu-id="56e76-193">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="56e76-194">Например, добавьте `Counter` компонент на домашнюю страницу приложения, добавив элемент `<Counter />` в компонент `Index`.</span><span class="sxs-lookup"><span data-stu-id="56e76-194">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="56e76-195">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="56e76-195">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="56e76-196">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="56e76-196">Run the app.</span></span> <span data-ttu-id="56e76-197">Домашняя страница имеет собственный счетчик, предоставляемый компонентом `Counter`.</span><span class="sxs-lookup"><span data-stu-id="56e76-197">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="56e76-198">Параметры компонента указываются с помощью атрибутов или [дочернего содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="56e76-198">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="56e76-199">Чтобы добавить параметр в компонент `Counter`, обновите блок `@code` компонента:</span><span class="sxs-lookup"><span data-stu-id="56e76-199">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="56e76-200">Добавьте открытое свойство для `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="56e76-200">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="56e76-201">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="56e76-201">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="56e76-202">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="56e76-202">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="56e76-203">Укажите `IncrementAmount` в элементе `<Counter>` компонента `Index` с помощью атрибута.</span><span class="sxs-lookup"><span data-stu-id="56e76-203">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="56e76-204">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="56e76-204">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="56e76-205">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="56e76-205">Run the app.</span></span> <span data-ttu-id="56e76-206">Компонент `Index` имеет собственный счетчик, который увеличивается на десять каждый раз при выборе кнопки " **нажать** ".</span><span class="sxs-lookup"><span data-stu-id="56e76-206">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="56e76-207">Компонент `Counter` (*Counter. Razor*) в `/counter` по-своему увеличивается на единицу.</span><span class="sxs-lookup"><span data-stu-id="56e76-207">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="56e76-208">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="56e76-208">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="56e76-209">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="56e76-209">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
