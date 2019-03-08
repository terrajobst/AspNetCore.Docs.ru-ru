---
title: Начало работы с Blazor
author: guardrex
description: Узнайте, как приступить к работе с Blazor путем создания и изменения проекта Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: 667c57d536450fa2f8ae1cabc7c5a76a16d38a55
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665591"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="0ce70-103">Начало работы с Blazor</span><span class="sxs-lookup"><span data-stu-id="0ce70-103">Get started with Blazor</span></span>

<span data-ttu-id="0ce70-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0ce70-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ce70-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ce70-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0ce70-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0ce70-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="0ce70-107">Создание первого проекта Blazor в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0ce70-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="0ce70-108">Установите последнюю версию [расширение языковых служб Blazor](https://go.microsoft.com/fwlink/?linkid=870389) в Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0ce70-108">Install the latest [Blazor Language Services extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="0ce70-109">Этот шаг делает доступным Blazor шаблоны для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ce70-109">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="0ce70-110">Шаблоны Blazor стали доступны для использования с .NET Core CLI, выполнив следующую команду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="0ce70-110">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="0ce70-111">Выберите **файл** > **новый проект** > **Web** > **веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0ce70-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="0ce70-112">Убедитесь, что **.NET Core** и **ASP.NET Core 3.0** выбраны вверху.</span><span class="sxs-lookup"><span data-stu-id="0ce70-112">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="0ce70-113">Выберите шаблон **Blazor** и нажмите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="0ce70-113">Choose the **Blazor** template and select **OK**.</span></span>
1. <span data-ttu-id="0ce70-114">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="0ce70-114">Press **F5** to run the app.</span></span>

<span data-ttu-id="0ce70-115">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="0ce70-115">Congratulations!</span></span> <span data-ttu-id="0ce70-116">Вы только что выполнили свое первое приложение Blazor!</span><span class="sxs-lookup"><span data-stu-id="0ce70-116">You just ran your first Blazor app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0ce70-117">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0ce70-117">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="0ce70-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0ce70-118">Prerequisites:</span></span>

* [<span data-ttu-id="0ce70-119">Предварительная версия .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="0ce70-119">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="0ce70-120">Добавьте шаблоны Blazor, выполнив следующую команду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="0ce70-120">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="0ce70-121">Создайте свой первый проект Blazor в командной строке:</span><span class="sxs-lookup"><span data-stu-id="0ce70-121">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="0ce70-122">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0ce70-122">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="0ce70-123">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="0ce70-123">Congratulations!</span></span> <span data-ttu-id="0ce70-124">Вы только что выполнили свое первое приложение Blazor!</span><span class="sxs-lookup"><span data-stu-id="0ce70-124">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="0ce70-125">Blazor проекта</span><span class="sxs-lookup"><span data-stu-id="0ce70-125">Blazor project</span></span>

<span data-ttu-id="0ce70-126">При запуске приложения, доступны несколько страницы из вкладок на боковой панели.</span><span class="sxs-lookup"><span data-stu-id="0ce70-126">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="0ce70-127">Главная страница</span><span class="sxs-lookup"><span data-stu-id="0ce70-127">Home</span></span>
* <span data-ttu-id="0ce70-128">Счетчик</span><span class="sxs-lookup"><span data-stu-id="0ce70-128">Counter</span></span>
* <span data-ttu-id="0ce70-129">Выборки данных</span><span class="sxs-lookup"><span data-stu-id="0ce70-129">Fetch data</span></span>

<span data-ttu-id="0ce70-130">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="0ce70-130">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="0ce70-131">Увеличение счетчика в веб-страницы обычно требует написания JavaScript, но Blazor предоставляет лучшую подход с использованием C#.</span><span class="sxs-lookup"><span data-stu-id="0ce70-131">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="0ce70-132">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0ce70-132">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="0ce70-133">Запрос `/counter` в браузере, в соответствии с `@page` директива сверху, компонент счетчика для отображения его содержимого.</span><span class="sxs-lookup"><span data-stu-id="0ce70-133">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="0ce70-134">В выполняющейся в памяти представление дерева визуализации, можно использовать для обновления пользовательского интерфейса в гибкий и эффективный способ отображения компонентов.</span><span class="sxs-lookup"><span data-stu-id="0ce70-134">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="0ce70-135">Каждый раз **Click me** выбранной кнопки:</span><span class="sxs-lookup"><span data-stu-id="0ce70-135">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="0ce70-136">`onclick` События.</span><span class="sxs-lookup"><span data-stu-id="0ce70-136">The `onclick` event is fired.</span></span>
* <span data-ttu-id="0ce70-137">вызывается метод `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="0ce70-137">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="0ce70-138">`currentCount` Увеличивается.</span><span class="sxs-lookup"><span data-stu-id="0ce70-138">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="0ce70-139">Компонент отображается снова.</span><span class="sxs-lookup"><span data-stu-id="0ce70-139">The component is rendered again.</span></span>

<span data-ttu-id="0ce70-140">Среда выполнения сравнивает новое содержимое для предыдущее содержимое и применяется измененное содержимое только для модели объектов документов (DOM).</span><span class="sxs-lookup"><span data-stu-id="0ce70-140">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="0ce70-141">Добавление компонента в другой компонент, используя синтаксис типа HTML.</span><span class="sxs-lookup"><span data-stu-id="0ce70-141">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="0ce70-142">Параметры компонента указываются с помощью атрибутов или дочернего содержимого.</span><span class="sxs-lookup"><span data-stu-id="0ce70-142">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="0ce70-143">Например, компонент счетчика можно добавить на домашнюю страницу приложения, добавив `<Counter />` элемент компонент индекса.</span><span class="sxs-lookup"><span data-stu-id="0ce70-143">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="0ce70-144">В *Pages/Index.cshtml*, замените компонент счетчика опроса Prompt компонента:</span><span class="sxs-lookup"><span data-stu-id="0ce70-144">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="0ce70-145">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0ce70-145">Run the app.</span></span> <span data-ttu-id="0ce70-146">Домашняя страница имеет свой собственный счетчика.</span><span class="sxs-lookup"><span data-stu-id="0ce70-146">The homepage has its own counter.</span></span>

<span data-ttu-id="0ce70-147">Чтобы добавить параметр к компоненту счетчика, обновите компонента `@functions` блок:</span><span class="sxs-lookup"><span data-stu-id="0ce70-147">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="0ce70-148">Добавьте свойство для `IncrementAmount` с `[Parameter]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="0ce70-148">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="0ce70-149">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="0ce70-149">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="0ce70-150">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0ce70-150">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="0ce70-151">Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента домашней страницы, используя атрибут.</span><span class="sxs-lookup"><span data-stu-id="0ce70-151">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="0ce70-152">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0ce70-152">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="0ce70-153">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0ce70-153">Run the app.</span></span> <span data-ttu-id="0ce70-154">Домашняя страница имеет свой собственный счетчик, который увеличивается каждый раз, десять **Click me** выбранной кнопки.</span><span class="sxs-lookup"><span data-stu-id="0ce70-154">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ce70-155">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="0ce70-155">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
