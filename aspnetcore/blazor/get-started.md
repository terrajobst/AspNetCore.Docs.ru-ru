---
title: Начало работы с ASP.NET Core Блазор
author: guardrex
description: Приступите к работе с Блазор, создав приложение Блазор с любыми инструментами по своему усмотрению.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/21/2019
uid: blazor/get-started
ms.openlocfilehash: 80ff7b42a44e722dd27bc4fde53a066863448e10
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779130"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="d9a91-103">Начало работы с ASP.NET Core Блазор</span><span class="sxs-lookup"><span data-stu-id="d9a91-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="d9a91-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d9a91-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="d9a91-105">Начало работы с Блазор:</span><span class="sxs-lookup"><span data-stu-id="d9a91-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="d9a91-106">Установите [пакет SDK для предварительной версии .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d9a91-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="d9a91-107">Установите шаблон [блазор-Assembly](xref:blazor/hosting-models#blazor-webassembly) , выполнив следующую команду в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="d9a91-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="d9a91-108">Пакет [Microsoft. AspNetCore. блазор. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) имеет предварительную версию, а блазор уже находится в предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="d9a91-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="d9a91-109">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="d9a91-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d9a91-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9a91-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d9a91-111">1 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-111">1\.</span></span> <span data-ttu-id="d9a91-112">Установите [Visual Studio 16,4 Preview 2 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="d9a91-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d9a91-113">2 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-113">2\.</span></span> <span data-ttu-id="d9a91-114">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="d9a91-114">Create a new project.</span></span>

   <span data-ttu-id="d9a91-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-115">3\.</span></span> <span data-ttu-id="d9a91-116">Выберите **приложение блазор**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-116">Select **Blazor App**.</span></span> <span data-ttu-id="d9a91-117">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-117">Select **Next**.</span></span>

   <span data-ttu-id="d9a91-118">4 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-118">4\.</span></span> <span data-ttu-id="d9a91-119">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d9a91-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d9a91-120">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="d9a91-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d9a91-121">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-121">Select **Create**.</span></span>

   <span data-ttu-id="d9a91-122">5 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-122">5\.</span></span> <span data-ttu-id="d9a91-123">Для интерфейса Блазор можно выбрать шаблон **приложения блазор для сборки** .</span><span class="sxs-lookup"><span data-stu-id="d9a91-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d9a91-124">Для удобства работы с сервером Блазор выберите шаблон **серверное приложение блазор** .</span><span class="sxs-lookup"><span data-stu-id="d9a91-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d9a91-125">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-125">Select **Create**.</span></span> <span data-ttu-id="d9a91-126">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9a91-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9a91-127">6 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-127">6\.</span></span> <span data-ttu-id="d9a91-128">Нажмите клавишу **Ctrl**+**F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d9a91-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d9a91-129">Если вы установили расширение Visual Studio Блазор для предыдущей предварительной версии ASP.NET Core Блазор (Предварительная версия 6 или более ранняя версия), вы можете удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="d9a91-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="d9a91-130">Установка шаблонов Блазор в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9a91-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d9a91-131">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9a91-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d9a91-132">1 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-132">1\.</span></span> <span data-ttu-id="d9a91-133">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d9a91-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d9a91-134">2 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-134">2\.</span></span> <span data-ttu-id="d9a91-135">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="d9a91-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d9a91-136">3 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-136">3\.</span></span> <span data-ttu-id="d9a91-137">Для работы с Блазор-сборками выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d9a91-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="d9a91-138">Для работы с сервером Блазор выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d9a91-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="d9a91-139">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9a91-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9a91-140">4 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-140">4\.</span></span> <span data-ttu-id="d9a91-141">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9a91-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d9a91-142">5 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-142">5\.</span></span> <span data-ttu-id="d9a91-143">Для проекта сервера Блазор среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="d9a91-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d9a91-144">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-144">Select **Yes**.</span></span>

   <span data-ttu-id="d9a91-145">6 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-145">6\.</span></span> <span data-ttu-id="d9a91-146">При использовании серверного приложения Блазор запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9a91-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d9a91-147">Если используется Блазор приложение сборки, выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="d9a91-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d9a91-148">7 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-148">7\.</span></span> <span data-ttu-id="d9a91-149">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d9a91-149">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d9a91-150">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d9a91-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d9a91-151">Для работы с Блазор-сборками выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d9a91-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d9a91-152">Для работы с сервером Блазор выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d9a91-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d9a91-153">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9a91-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9a91-154">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d9a91-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="d9a91-155">Установите последнюю версию [пакета SDK для .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="d9a91-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="d9a91-156">При необходимости установите шаблон [блазор-Assembly](xref:blazor/hosting-models#blazor-webassembly) , установив [пакет SDK для .NET Core 3,1 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.1) , а затем выполнив следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d9a91-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="d9a91-157">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="d9a91-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d9a91-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9a91-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d9a91-159">1 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-159">1\.</span></span> <span data-ttu-id="d9a91-160">Установите последнюю версию [Visual Studio](https://visualstudio.com/vs/) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="d9a91-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d9a91-161">2 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-161">2\.</span></span> <span data-ttu-id="d9a91-162">При необходимости установите [Visual Studio 16,4 Preview 2 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** для разработки приложений блазор.</span><span class="sxs-lookup"><span data-stu-id="d9a91-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="d9a91-163">3 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-163">3\.</span></span> <span data-ttu-id="d9a91-164">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="d9a91-164">Create a new project.</span></span>

   <span data-ttu-id="d9a91-165">4 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-165">4\.</span></span> <span data-ttu-id="d9a91-166">Выберите **приложение блазор**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-166">Select **Blazor App**.</span></span> <span data-ttu-id="d9a91-167">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-167">Select **Next**.</span></span>

   <span data-ttu-id="d9a91-168">5 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-168">5\.</span></span> <span data-ttu-id="d9a91-169">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d9a91-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d9a91-170">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="d9a91-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d9a91-171">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-171">Select **Create**.</span></span>

   <span data-ttu-id="d9a91-172">6 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-172">6\.</span></span> <span data-ttu-id="d9a91-173">Для интерфейса Блазор можно выбрать шаблон **приложения блазор для сборки** .</span><span class="sxs-lookup"><span data-stu-id="d9a91-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d9a91-174">Для удобства работы с сервером Блазор выберите шаблон **серверное приложение блазор** .</span><span class="sxs-lookup"><span data-stu-id="d9a91-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d9a91-175">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-175">Select **Create**.</span></span> <span data-ttu-id="d9a91-176">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9a91-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9a91-177">7 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-177">7\.</span></span> <span data-ttu-id="d9a91-178">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d9a91-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d9a91-179">Если вы установили расширение Visual Studio Блазор для предыдущей предварительной версии ASP.NET Core Блазор (Предварительная версия 6 или более ранняя версия), вы можете удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="d9a91-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="d9a91-180">Установка шаблонов Блазор в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9a91-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d9a91-181">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9a91-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d9a91-182">1 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-182">1\.</span></span> <span data-ttu-id="d9a91-183">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d9a91-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d9a91-184">2 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-184">2\.</span></span> <span data-ttu-id="d9a91-185">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="d9a91-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d9a91-186">3 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-186">3\.</span></span> <span data-ttu-id="d9a91-187">Для работы с Блазор-сборками выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d9a91-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="d9a91-188">Для работы с сервером Блазор выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d9a91-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="d9a91-189">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9a91-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9a91-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-190">4\.</span></span> <span data-ttu-id="d9a91-191">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9a91-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d9a91-192">5 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-192">5\.</span></span> <span data-ttu-id="d9a91-193">Для проекта сервера Блазор среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="d9a91-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d9a91-194">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="d9a91-194">Select **Yes**.</span></span>

   <span data-ttu-id="d9a91-195">6 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-195">6\.</span></span> <span data-ttu-id="d9a91-196">При использовании серверного приложения Блазор запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9a91-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d9a91-197">Если используется Блазор приложение сборки, выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="d9a91-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d9a91-198">7 \.</span><span class="sxs-lookup"><span data-stu-id="d9a91-198">7\.</span></span> <span data-ttu-id="d9a91-199">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d9a91-199">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d9a91-200">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d9a91-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d9a91-201">Для работы с Блазор-сборками выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d9a91-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d9a91-202">Для работы с сервером Блазор выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="d9a91-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d9a91-203">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9a91-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9a91-204">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d9a91-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="d9a91-205">Из вкладок боковой панели доступны несколько страниц:</span><span class="sxs-lookup"><span data-stu-id="d9a91-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="d9a91-206">Главная страница</span><span class="sxs-lookup"><span data-stu-id="d9a91-206">Home</span></span>
* <span data-ttu-id="d9a91-207">Счетчик</span><span class="sxs-lookup"><span data-stu-id="d9a91-207">Counter</span></span>
* <span data-ttu-id="d9a91-208">Выборка данных</span><span class="sxs-lookup"><span data-stu-id="d9a91-208">Fetch data</span></span>

<span data-ttu-id="d9a91-209">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="d9a91-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d9a91-210">Для увеличения счетчика на веб-странице обычно требуется написание кода JavaScript, но с Блазор можно C#использовать.</span><span class="sxs-lookup"><span data-stu-id="d9a91-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="d9a91-211">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="d9a91-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="d9a91-212">Запрос `/counter` в браузере, как указано в директиве `@page` в верхней части, приводит к тому, что компонент `Counter` отображает его содержимое.</span><span class="sxs-lookup"><span data-stu-id="d9a91-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="d9a91-213">Компоненты подготавливаются к просмотру в памяти представления дерева подготовки, которое затем может использоваться для обновления пользовательского интерфейса в гибком и удобном виде.</span><span class="sxs-lookup"><span data-stu-id="d9a91-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="d9a91-214">При каждом выборе кнопки « **Click Me** »:</span><span class="sxs-lookup"><span data-stu-id="d9a91-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="d9a91-215">Будет вызвано событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="d9a91-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="d9a91-216">вызывается метод `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="d9a91-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="d9a91-217">Значение `currentCount` увеличивается.</span><span class="sxs-lookup"><span data-stu-id="d9a91-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="d9a91-218">Компонент снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="d9a91-218">The component is rendered again.</span></span>

<span data-ttu-id="d9a91-219">Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет только измененное содержимое к модель DOM (DOM).</span><span class="sxs-lookup"><span data-stu-id="d9a91-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="d9a91-220">Добавьте компонент в другой компонент с помощью синтаксиса HTML.</span><span class="sxs-lookup"><span data-stu-id="d9a91-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="d9a91-221">Например, добавьте компонент `Counter` на домашнюю страницу приложения, добавив элемент `<Counter />` в компонент `Index`.</span><span class="sxs-lookup"><span data-stu-id="d9a91-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="d9a91-222">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="d9a91-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="d9a91-223">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d9a91-223">Run the app.</span></span> <span data-ttu-id="d9a91-224">Домашняя страница имеет собственный счетчик, предоставляемый компонентом `Counter`.</span><span class="sxs-lookup"><span data-stu-id="d9a91-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="d9a91-225">Параметры компонента указываются с помощью атрибутов или [дочернего содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="d9a91-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="d9a91-226">Чтобы добавить параметр в компонент `Counter`, обновите блок `@code` компонента:</span><span class="sxs-lookup"><span data-stu-id="d9a91-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="d9a91-227">Добавьте открытое свойство для `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d9a91-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="d9a91-228">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d9a91-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="d9a91-229">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="d9a91-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="d9a91-230">Укажите `IncrementAmount` в элементе `<Counter>` компонента `Index` с помощью атрибута.</span><span class="sxs-lookup"><span data-stu-id="d9a91-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="d9a91-231">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="d9a91-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="d9a91-232">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d9a91-232">Run the app.</span></span> <span data-ttu-id="d9a91-233">Компонент `Index` имеет собственный счетчик, который увеличивается на десять каждый раз при выборе кнопки " **нажать** ".</span><span class="sxs-lookup"><span data-stu-id="d9a91-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d9a91-234">Компонент `Counter` (*Counter. Razor*) в `/counter` продолжит приращение на единицу.</span><span class="sxs-lookup"><span data-stu-id="d9a91-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9a91-235">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="d9a91-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="d9a91-236">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d9a91-236">Additional resources</span></span>

* <xref:signalr/introduction>
