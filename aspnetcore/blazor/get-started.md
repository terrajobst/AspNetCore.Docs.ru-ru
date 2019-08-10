---
title: Начало работы с ASP.NET Core Блазор
author: guardrex
description: Приступите к работе с Блазор, создав приложение Блазор с любыми инструментами по своему усмотрению.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/23/2019
uid: blazor/get-started
ms.openlocfilehash: b4609858be43acf9d1b2d8be5eff4879fd56f49f
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68948334"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="e1449-103">Начало работы с ASP.NET Core Блазор</span><span class="sxs-lookup"><span data-stu-id="e1449-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="e1449-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e1449-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e1449-105">Начало работы с Блазор:</span><span class="sxs-lookup"><span data-stu-id="e1449-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="e1449-106">Установите последнюю версию [пакета SDK для предварительной версии .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="e1449-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="e1449-107">Установите шаблоны Блазор, выполнив следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e1449-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview7.19365.7
   ```

1. <span data-ttu-id="e1449-108">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="e1449-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1449-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1449-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="e1449-110">1 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-110">1\.</span></span> <span data-ttu-id="e1449-111">Установите последнюю предварительную версию [Visual Studio](https://visualstudio.com/vs/preview) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="e1449-111">Install the latest [Visual Studio preview](https://visualstudio.com/vs/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="e1449-112">2 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-112">2\.</span></span> <span data-ttu-id="e1449-113">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="e1449-113">Create a new project.</span></span>

   <span data-ttu-id="e1449-114">3 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-114">3\.</span></span> <span data-ttu-id="e1449-115">Выберите **приложение блазор**.</span><span class="sxs-lookup"><span data-stu-id="e1449-115">Select **Blazor App**.</span></span> <span data-ttu-id="e1449-116">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e1449-116">Select **Next**.</span></span>

   <span data-ttu-id="e1449-117">4 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-117">4\.</span></span> <span data-ttu-id="e1449-118">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e1449-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="e1449-119">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="e1449-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="e1449-120">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e1449-120">Select **Create**.</span></span>

   <span data-ttu-id="e1449-121">5 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-121">5\.</span></span> <span data-ttu-id="e1449-122">Для интерфейса Блазор на стороне клиента выберите шаблон **блазор (на стороне клиента)** .</span><span class="sxs-lookup"><span data-stu-id="e1449-122">For a Blazor client-side experience, choose the **Blazor (client-side)** template.</span></span> <span data-ttu-id="e1449-123">Для интерфейса Блазор на стороне сервера выберите шаблон серверное **приложение блазор** .</span><span class="sxs-lookup"><span data-stu-id="e1449-123">For a Blazor server-side experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="e1449-124">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e1449-124">Select **Create**.</span></span> <span data-ttu-id="e1449-125">Сведения о двух моделях размещения Блазор: на стороне сервера и на клиенте см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e1449-125">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e1449-126">6 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-126">6\.</span></span> <span data-ttu-id="e1449-127">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="e1449-127">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e1449-128">Если вы установили расширение Visual Studio Блазор для предыдущей предварительной версии ASP.NET Core Блазор (Предварительная версия 6 или более ранняя версия), вы можете удалить расширение в предварительной версии 7.</span><span class="sxs-lookup"><span data-stu-id="e1449-128">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension at Preview 7.</span></span> <span data-ttu-id="e1449-129">Установка шаблонов Блазор в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e1449-129">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1449-130">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1449-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="e1449-131">1 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-131">1\.</span></span> <span data-ttu-id="e1449-132">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="e1449-132">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="e1449-133">2 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-133">2\.</span></span> <span data-ttu-id="e1449-134">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="e1449-134">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="e1449-135">3 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-135">3\.</span></span> <span data-ttu-id="e1449-136">Для интерфейса Блазор на стороне клиента выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e1449-136">For a Blazor client-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazor -o WebApplication1
      ```

      <span data-ttu-id="e1449-137">Для интерфейса Блазор на стороне сервера выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e1449-137">For a Blazor server-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      <span data-ttu-id="e1449-138">Сведения о двух моделях размещения Блазор: на стороне сервера и на клиенте см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e1449-138">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e1449-139">4 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-139">4\.</span></span> <span data-ttu-id="e1449-140">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1449-140">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="e1449-141">5 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-141">5\.</span></span> <span data-ttu-id="e1449-142">Для проекта на стороне сервера Блазор среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="e1449-142">For a Blazor server-side project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="e1449-143">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="e1449-143">Select **Yes**.</span></span>

   <span data-ttu-id="e1449-144">6 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-144">6\.</span></span> <span data-ttu-id="e1449-145">При использовании приложения Блазор на стороне сервера запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1449-145">If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="e1449-146">Если используется клиентское приложение блазор, выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="e1449-146">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="e1449-147">7 \.</span><span class="sxs-lookup"><span data-stu-id="e1449-147">7\.</span></span> <span data-ttu-id="e1449-148">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e1449-148">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **ASP.NET Core Blazor Server App** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e1449-149">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1449-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="e1449-150">Для интерфейса Блазор на стороне клиента выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e1449-150">For a Blazor client-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="e1449-151">Для интерфейса Блазор на стороне сервера выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e1449-151">For a Blazor server-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="e1449-152">Сведения о двух моделях размещения Блазор: на стороне сервера и на клиенте см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e1449-152">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e1449-153">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e1449-153">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="e1449-154">Из вкладок боковой панели доступны несколько страниц:</span><span class="sxs-lookup"><span data-stu-id="e1449-154">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="e1449-155">Корневая</span><span class="sxs-lookup"><span data-stu-id="e1449-155">Home</span></span>
* <span data-ttu-id="e1449-156">Счетчик</span><span class="sxs-lookup"><span data-stu-id="e1449-156">Counter</span></span>
* <span data-ttu-id="e1449-157">Выборка данных</span><span class="sxs-lookup"><span data-stu-id="e1449-157">Fetch data</span></span>

<span data-ttu-id="e1449-158">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="e1449-158">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="e1449-159">Увеличение счетчика на веб-странице обычно требует написания JavaScript, но компоненты Razor предоставляют более эффективный подход с помощью C#.</span><span class="sxs-lookup"><span data-stu-id="e1449-159">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="e1449-160">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="e1449-160">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="e1449-161">Запрос `/counter` в браузере, как указано `@page` в директиве вверху, приводит к тому, что `Counter` компонент визуализирует свое содержимое.</span><span class="sxs-lookup"><span data-stu-id="e1449-161">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="e1449-162">Компоненты подготавливаются к просмотру в памяти представления дерева подготовки, которое затем может использоваться для обновления пользовательского интерфейса в гибком и удобном виде.</span><span class="sxs-lookup"><span data-stu-id="e1449-162">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="e1449-163">При каждом выборе кнопки « **Click Me** »:</span><span class="sxs-lookup"><span data-stu-id="e1449-163">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="e1449-164">`onclick` Событие срабатывает.</span><span class="sxs-lookup"><span data-stu-id="e1449-164">The `onclick` event is fired.</span></span>
* <span data-ttu-id="e1449-165">вызывается метод `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="e1449-165">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="e1449-166">`currentCount` Значение увеличивается.</span><span class="sxs-lookup"><span data-stu-id="e1449-166">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="e1449-167">Компонент снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="e1449-167">The component is rendered again.</span></span>

<span data-ttu-id="e1449-168">Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет только измененное содержимое к модель DOM (DOM).</span><span class="sxs-lookup"><span data-stu-id="e1449-168">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="e1449-169">Добавьте компонент в другой компонент с помощью синтаксиса HTML.</span><span class="sxs-lookup"><span data-stu-id="e1449-169">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="e1449-170">Например, добавьте `Counter` компонент в домашнюю страницу приложения, `<Counter />` добавив элемент в `Index` компонент.</span><span class="sxs-lookup"><span data-stu-id="e1449-170">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="e1449-171">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="e1449-171">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="e1449-172">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e1449-172">Run the app.</span></span> <span data-ttu-id="e1449-173">Домашняя страница имеет собственный счетчик, `Counter` предоставляемый компонентом.</span><span class="sxs-lookup"><span data-stu-id="e1449-173">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="e1449-174">Параметры компонента указываются с помощью атрибутов или дочернего [содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="e1449-174">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="e1449-175">Чтобы добавить параметр в `Counter` компонент, обновите `@code` блок компонента:</span><span class="sxs-lookup"><span data-stu-id="e1449-175">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="e1449-176">Добавьте свойство для `IncrementAmount` `[Parameter]` с атрибутом.</span><span class="sxs-lookup"><span data-stu-id="e1449-176">Add a property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="e1449-177">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="e1449-177">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="e1449-178">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="e1449-178">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="e1449-179">`IncrementAmount` Укажите вэлементе`<Counter>`компонента,используяатрибут. `Index`</span><span class="sxs-lookup"><span data-stu-id="e1449-179">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="e1449-180">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="e1449-180">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="e1449-181">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e1449-181">Run the app.</span></span> <span data-ttu-id="e1449-182">Компонент имеет собственный счетчик, увеличивающийся на десять при каждом нажатии кнопки « **Click Me** ». `Index`</span><span class="sxs-lookup"><span data-stu-id="e1449-182">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="e1449-183">Компонент `Counter` (*Counter. Razor*) по- `/counter` своему по-в то же время увеличивается на единицу.</span><span class="sxs-lookup"><span data-stu-id="e1449-183">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1449-184">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e1449-184">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="e1449-185">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e1449-185">Additional resources</span></span>

* <xref:signalr/introduction>
