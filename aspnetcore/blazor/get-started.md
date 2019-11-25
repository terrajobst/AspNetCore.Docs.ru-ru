---
title: Get started with ASP.NET Core Blazor
author: guardrex
description: Get started with Blazor by building a Blazor app with the tooling of your choice.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 198093b37cb4f440eb7b520d18004304aea570a5
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239715"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="9a0a2-103">Get started with ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="9a0a2-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="9a0a2-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9a0a2-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="9a0a2-105">Get started with Blazor:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="9a0a2-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="9a0a2-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="9a0a2-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="9a0a2-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview3.19555.2
   ```

1. <span data-ttu-id="9a0a2-109">Follow the guidance for your choice of tooling:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9a0a2-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9a0a2-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="9a0a2-111">1\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-111">1\.</span></span> <span data-ttu-id="9a0a2-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="9a0a2-113">2\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-113">2\.</span></span> <span data-ttu-id="9a0a2-114">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-114">Create a new project.</span></span>

   <span data-ttu-id="9a0a2-115">3\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-115">3\.</span></span> <span data-ttu-id="9a0a2-116">Select **Blazor App**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-116">Select **Blazor App**.</span></span> <span data-ttu-id="9a0a2-117">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-117">Select **Next**.</span></span>

   <span data-ttu-id="9a0a2-118">4\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-118">4\.</span></span> <span data-ttu-id="9a0a2-119">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="9a0a2-120">Confirm the **Location** entry is correct or provide a location for the project.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="9a0a2-121">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-121">Select **Create**.</span></span>

   <span data-ttu-id="9a0a2-122">5\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-122">5\.</span></span> <span data-ttu-id="9a0a2-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="9a0a2-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="9a0a2-125">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-125">Select **Create**.</span></span> <span data-ttu-id="9a0a2-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="9a0a2-127">6\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-127">6\.</span></span> <span data-ttu-id="9a0a2-128">Нажмите клавишу **Ctrl**+**F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9a0a2-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="9a0a2-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9a0a2-131">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="9a0a2-132">1\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-132">1\.</span></span> <span data-ttu-id="9a0a2-133">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="9a0a2-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="9a0a2-134">2\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-134">2\.</span></span> <span data-ttu-id="9a0a2-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="9a0a2-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="9a0a2-136">3\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-136">3\.</span></span> <span data-ttu-id="9a0a2-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="9a0a2-138">For a Blazor Server experience, execute the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="9a0a2-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="9a0a2-140">4\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-140">4\.</span></span> <span data-ttu-id="9a0a2-141">Open the *WebApplication1* folder in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="9a0a2-142">5\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-142">5\.</span></span> <span data-ttu-id="9a0a2-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="9a0a2-144">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-144">Select **Yes**.</span></span>

   <span data-ttu-id="9a0a2-145">6\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-145">6\.</span></span> <span data-ttu-id="9a0a2-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="9a0a2-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="9a0a2-148">7\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-148">7\.</span></span> <span data-ttu-id="9a0a2-149">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-149">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9a0a2-150">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="9a0a2-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="9a0a2-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="9a0a2-152">For a Blazor Server experience, execute the following commands in a command shell:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="9a0a2-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="9a0a2-154">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="9a0a2-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="9a0a2-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview3.19555.2
   ```

1. <span data-ttu-id="9a0a2-157">Follow the guidance for your choice of tooling:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9a0a2-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9a0a2-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="9a0a2-159">1\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-159">1\.</span></span> <span data-ttu-id="9a0a2-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="9a0a2-161">2\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-161">2\.</span></span> <span data-ttu-id="9a0a2-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="9a0a2-163">3\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-163">3\.</span></span> <span data-ttu-id="9a0a2-164">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-164">Create a new project.</span></span>

   <span data-ttu-id="9a0a2-165">4\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-165">4\.</span></span> <span data-ttu-id="9a0a2-166">Select **Blazor App**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-166">Select **Blazor App**.</span></span> <span data-ttu-id="9a0a2-167">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-167">Select **Next**.</span></span>

   <span data-ttu-id="9a0a2-168">5\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-168">5\.</span></span> <span data-ttu-id="9a0a2-169">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="9a0a2-170">Confirm the **Location** entry is correct or provide a location for the project.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="9a0a2-171">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-171">Select **Create**.</span></span>

   <span data-ttu-id="9a0a2-172">6\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-172">6\.</span></span> <span data-ttu-id="9a0a2-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="9a0a2-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="9a0a2-175">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-175">Select **Create**.</span></span> <span data-ttu-id="9a0a2-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="9a0a2-177">7\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-177">7\.</span></span> <span data-ttu-id="9a0a2-178">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9a0a2-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="9a0a2-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9a0a2-181">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="9a0a2-182">1\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-182">1\.</span></span> <span data-ttu-id="9a0a2-183">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="9a0a2-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="9a0a2-184">2\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-184">2\.</span></span> <span data-ttu-id="9a0a2-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="9a0a2-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="9a0a2-186">3\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-186">3\.</span></span> <span data-ttu-id="9a0a2-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="9a0a2-188">For a Blazor Server experience, execute the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="9a0a2-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="9a0a2-190">4\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-190">4\.</span></span> <span data-ttu-id="9a0a2-191">Open the *WebApplication1* folder in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="9a0a2-192">5\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-192">5\.</span></span> <span data-ttu-id="9a0a2-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="9a0a2-194">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-194">Select **Yes**.</span></span>

   <span data-ttu-id="9a0a2-195">6\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-195">6\.</span></span> <span data-ttu-id="9a0a2-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="9a0a2-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="9a0a2-198">7\.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-198">7\.</span></span> <span data-ttu-id="9a0a2-199">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-199">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9a0a2-200">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="9a0a2-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="9a0a2-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="9a0a2-202">For a Blazor Server experience, execute the following commands in a command shell:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="9a0a2-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="9a0a2-204">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="9a0a2-205">Multiple pages are available from tabs in the sidebar:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="9a0a2-206">Главная страница</span><span class="sxs-lookup"><span data-stu-id="9a0a2-206">Home</span></span>
* <span data-ttu-id="9a0a2-207">Счетчик</span><span class="sxs-lookup"><span data-stu-id="9a0a2-207">Counter</span></span>
* <span data-ttu-id="9a0a2-208">Fetch data</span><span class="sxs-lookup"><span data-stu-id="9a0a2-208">Fetch data</span></span>

<span data-ttu-id="9a0a2-209">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="9a0a2-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="9a0a2-211">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="9a0a2-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="9a0a2-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="9a0a2-214">Each time the **Click me** button is selected:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="9a0a2-215">The `onclick` event is fired.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="9a0a2-216">вызывается метод `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="9a0a2-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="9a0a2-217">The `currentCount` is incremented.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="9a0a2-218">The component is rendered again.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-218">The component is rendered again.</span></span>

<span data-ttu-id="9a0a2-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="9a0a2-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="9a0a2-220">Add a component to another component using HTML syntax.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="9a0a2-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="9a0a2-222">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="9a0a2-223">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-223">Run the app.</span></span> <span data-ttu-id="9a0a2-224">The homepage has its own counter provided by the `Counter` component.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="9a0a2-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="9a0a2-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="9a0a2-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="9a0a2-228">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="9a0a2-229">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="9a0a2-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="9a0a2-231">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="9a0a2-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="9a0a2-232">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-232">Run the app.</span></span> <span data-ttu-id="9a0a2-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="9a0a2-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span><span class="sxs-lookup"><span data-stu-id="9a0a2-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a0a2-235">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="9a0a2-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="9a0a2-236">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9a0a2-236">Additional resources</span></span>

* <xref:signalr/introduction>
