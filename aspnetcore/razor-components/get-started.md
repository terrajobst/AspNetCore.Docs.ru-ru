---
title: Начало работы с Razor компонентов
author: guardrex
description: Узнайте, как приступить к работе с компонентами Razor путем создания и изменения проекта Razor компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515653"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="311a3-103">Начало работы с Razor компонентов</span><span class="sxs-lookup"><span data-stu-id="311a3-103">Get started with Razor Components</span></span>

<span data-ttu-id="311a3-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="311a3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="311a3-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="311a3-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="311a3-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="311a3-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="311a3-107">Создание первого проекта Razor компонентов в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="311a3-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="311a3-108">Установите последнюю версию [Предварительная версия 3.0 SDK для .NET Core](https://dotnet.microsoft.com/download/dotnet-core/3.0) выпуска.</span><span class="sxs-lookup"><span data-stu-id="311a3-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="311a3-109">Включение Visual Studio для использования предварительной версии пакетов SDK:</span><span class="sxs-lookup"><span data-stu-id="311a3-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="311a3-110">Откройте **средства** > **параметры** в строке меню.</span><span class="sxs-lookup"><span data-stu-id="311a3-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="311a3-111">Откройте **проекты и решения** узла.</span><span class="sxs-lookup"><span data-stu-id="311a3-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="311a3-112">Откройте **.NET Core** вкладки.</span><span class="sxs-lookup"><span data-stu-id="311a3-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="311a3-113">Установите флажок для **использовать предварительные версии пакета SDK для .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="311a3-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="311a3-114">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="311a3-114">Select **OK**.</span></span>
1. <span data-ttu-id="311a3-115">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="311a3-115">Create a new project.</span></span>
1. <span data-ttu-id="311a3-116">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="311a3-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="311a3-117">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="311a3-117">Select **Next**.</span></span>
1. <span data-ttu-id="311a3-118">Введите имя в **имя_проекта** поля.</span><span class="sxs-lookup"><span data-stu-id="311a3-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="311a3-119">Подтвердите **расположение** запись правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="311a3-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="311a3-120">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="311a3-120">Select **Create**.</span></span>
1. <span data-ttu-id="311a3-121">Убедитесь, что **.NET Core** и **ASP.NET Core 3.0** выбраны вверху.</span><span class="sxs-lookup"><span data-stu-id="311a3-121">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="311a3-122">Выберите **компоненты Razor** шаблона и выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="311a3-122">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="311a3-123">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="311a3-123">Press **F5** to run the app.</span></span>

<span data-ttu-id="311a3-124">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="311a3-124">Congratulations!</span></span> <span data-ttu-id="311a3-125">Вы только что выполнили свое первое приложение Razor компоненты!</span><span class="sxs-lookup"><span data-stu-id="311a3-125">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="311a3-126">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="311a3-126">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="311a3-127">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="311a3-127">Prerequisites:</span></span>

* [<span data-ttu-id="311a3-128">Предварительная версия .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="311a3-128">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="311a3-129">Создание первого проекта Razor компонентов из командной оболочки:</span><span class="sxs-lookup"><span data-stu-id="311a3-129">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="311a3-130">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="311a3-130">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="311a3-131">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="311a3-131">Congratulations!</span></span> <span data-ttu-id="311a3-132">Вы только что выполнили свое первое приложение Razor компоненты!</span><span class="sxs-lookup"><span data-stu-id="311a3-132">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="311a3-133">Компоненты проекта Razor</span><span class="sxs-lookup"><span data-stu-id="311a3-133">Razor Components project</span></span>

<span data-ttu-id="311a3-134">Компоненты Razor создаются с использованием синтаксиса Razor, но компилируются иначе, чем представлений Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="311a3-134">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="311a3-135">*.Razor* расширение файла используется для указания компонента Razor.</span><span class="sxs-lookup"><span data-stu-id="311a3-135">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="311a3-136">Razor Pages и MVC представления по-прежнему использовать *.cshtml* расширение файла.</span><span class="sxs-lookup"><span data-stu-id="311a3-136">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="311a3-137">Razor компоненты могут быть созданы с помощью *.cshtml* расширение файла, до тех пор, пока эти файлы идентифицируются как файлы Razor компонента, с помощью `_RazorComponentInclude` свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="311a3-137">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="311a3-138">Например, приложение, созданное с помощью шаблона Razor компонент указывает, что все *.cshtml* файлы в разделе *компоненты* папки, которые должны рассматриваться как компоненты Razor:</span><span class="sxs-lookup"><span data-stu-id="311a3-138">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="311a3-139">При запуске приложения, доступны несколько страницы из вкладок на боковой панели.</span><span class="sxs-lookup"><span data-stu-id="311a3-139">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="311a3-140">Главная страница</span><span class="sxs-lookup"><span data-stu-id="311a3-140">Home</span></span>
* <span data-ttu-id="311a3-141">Счетчик</span><span class="sxs-lookup"><span data-stu-id="311a3-141">Counter</span></span>
* <span data-ttu-id="311a3-142">Выборки данных</span><span class="sxs-lookup"><span data-stu-id="311a3-142">Fetch data</span></span>

<span data-ttu-id="311a3-143">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="311a3-143">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="311a3-144">Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, но Razor Components реализует более правильный подход с использованием C#.</span><span class="sxs-lookup"><span data-stu-id="311a3-144">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="311a3-145">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="311a3-145">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="311a3-146">Запрос `/counter` в браузере, в соответствии с `@page` директива сверху, компонент счетчика для отображения его содержимого.</span><span class="sxs-lookup"><span data-stu-id="311a3-146">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="311a3-147">В выполняющейся в памяти представление дерева визуализации, можно использовать для обновления пользовательского интерфейса в гибкий и эффективный способ отображения компонентов.</span><span class="sxs-lookup"><span data-stu-id="311a3-147">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="311a3-148">Каждый раз **Click me** выбранной кнопки:</span><span class="sxs-lookup"><span data-stu-id="311a3-148">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="311a3-149">`onclick` События.</span><span class="sxs-lookup"><span data-stu-id="311a3-149">The `onclick` event is fired.</span></span>
* <span data-ttu-id="311a3-150">вызывается метод `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="311a3-150">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="311a3-151">`currentCount` Увеличивается.</span><span class="sxs-lookup"><span data-stu-id="311a3-151">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="311a3-152">Компонент отображается снова.</span><span class="sxs-lookup"><span data-stu-id="311a3-152">The component is rendered again.</span></span>

<span data-ttu-id="311a3-153">Среда выполнения сравнивает новое содержимое для предыдущее содержимое и применяется измененное содержимое только для модели объектов документов (DOM).</span><span class="sxs-lookup"><span data-stu-id="311a3-153">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="311a3-154">Добавление компонента в другой компонент, используя синтаксис типа HTML.</span><span class="sxs-lookup"><span data-stu-id="311a3-154">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="311a3-155">Параметры компонента указываются с помощью атрибутов или дочернего содержимого.</span><span class="sxs-lookup"><span data-stu-id="311a3-155">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="311a3-156">Например, компонент счетчика можно добавить на домашнюю страницу приложения, добавив `<Counter />` элемент компонент индекса.</span><span class="sxs-lookup"><span data-stu-id="311a3-156">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="311a3-157">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="311a3-157">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="311a3-158">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="311a3-158">Run the app.</span></span> <span data-ttu-id="311a3-159">Домашняя страница имеет свой собственный счетчика.</span><span class="sxs-lookup"><span data-stu-id="311a3-159">The homepage has its own counter.</span></span>

<span data-ttu-id="311a3-160">Чтобы добавить параметр к компоненту счетчика, обновите компонента `@functions` блок:</span><span class="sxs-lookup"><span data-stu-id="311a3-160">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="311a3-161">Добавьте свойство для `IncrementAmount` с `[Parameter]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="311a3-161">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="311a3-162">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="311a3-162">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="311a3-163">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="311a3-163">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="311a3-164">Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента домашней страницы, используя атрибут.</span><span class="sxs-lookup"><span data-stu-id="311a3-164">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="311a3-165">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="311a3-165">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="311a3-166">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="311a3-166">Run the app.</span></span> <span data-ttu-id="311a3-167">Домашняя страница имеет свой собственный счетчик, который увеличивается каждый раз, десять **Click me** выбранной кнопки.</span><span class="sxs-lookup"><span data-stu-id="311a3-167">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="311a3-168">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="311a3-168">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
