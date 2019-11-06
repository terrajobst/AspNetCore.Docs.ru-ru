---
title: Начало работы с ASP.NET Core Блазор
author: guardrex
description: Приступите к работе с Блазор, создав приложение Блазор с любыми инструментами по своему усмотрению.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/21/2019
uid: blazor/get-started
ms.openlocfilehash: 48d7ff4bf23273daf43128831aa46cfab3d982fe
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634032"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="60830-103">Начало работы с ASP.NET Core Блазор</span><span class="sxs-lookup"><span data-stu-id="60830-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="60830-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="60830-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="60830-105">Начало работы с Блазор:</span><span class="sxs-lookup"><span data-stu-id="60830-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="60830-106">Установите [пакет SDK для предварительной версии .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="60830-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="60830-107">Установите шаблон [блазор-Assembly](xref:blazor/hosting-models#blazor-webassembly) , выполнив следующую команду в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="60830-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="60830-108">Пакет [Microsoft. AspNetCore. блазор. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) имеет предварительную версию, а блазор уже находится в предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="60830-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. <span data-ttu-id="60830-109">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="60830-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60830-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60830-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="60830-111">1 \.</span><span class="sxs-lookup"><span data-stu-id="60830-111">1\.</span></span> <span data-ttu-id="60830-112">Установите [Visual Studio 16,4 Preview 2 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="60830-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="60830-113">2 \.</span><span class="sxs-lookup"><span data-stu-id="60830-113">2\.</span></span> <span data-ttu-id="60830-114">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="60830-114">Create a new project.</span></span>

   <span data-ttu-id="60830-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="60830-115">3\.</span></span> <span data-ttu-id="60830-116">Выберите **приложение блазор**.</span><span class="sxs-lookup"><span data-stu-id="60830-116">Select **Blazor App**.</span></span> <span data-ttu-id="60830-117">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="60830-117">Select **Next**.</span></span>

   <span data-ttu-id="60830-118">4 \.</span><span class="sxs-lookup"><span data-stu-id="60830-118">4\.</span></span> <span data-ttu-id="60830-119">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="60830-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="60830-120">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="60830-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="60830-121">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="60830-121">Select **Create**.</span></span>

   <span data-ttu-id="60830-122">5 \.</span><span class="sxs-lookup"><span data-stu-id="60830-122">5\.</span></span> <span data-ttu-id="60830-123">Для интерфейса Блазор можно выбрать шаблон **приложения блазор для сборки** .</span><span class="sxs-lookup"><span data-stu-id="60830-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="60830-124">Для удобства работы с сервером Блазор выберите шаблон **серверное приложение блазор** .</span><span class="sxs-lookup"><span data-stu-id="60830-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="60830-125">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="60830-125">Select **Create**.</span></span> <span data-ttu-id="60830-126">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="60830-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60830-127">6 \.</span><span class="sxs-lookup"><span data-stu-id="60830-127">6\.</span></span> <span data-ttu-id="60830-128">Нажмите клавишу **Ctrl**+**F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="60830-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="60830-129">Если вы установили расширение Visual Studio Блазор для предыдущей предварительной версии ASP.NET Core Блазор (Предварительная версия 6 или более ранняя версия), вы можете удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="60830-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="60830-130">Установка шаблонов Блазор в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="60830-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="60830-131">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="60830-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="60830-132">1 \.</span><span class="sxs-lookup"><span data-stu-id="60830-132">1\.</span></span> <span data-ttu-id="60830-133">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="60830-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="60830-134">2 \.</span><span class="sxs-lookup"><span data-stu-id="60830-134">2\.</span></span> <span data-ttu-id="60830-135">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="60830-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="60830-136">3 \.</span><span class="sxs-lookup"><span data-stu-id="60830-136">3\.</span></span> <span data-ttu-id="60830-137">Для работы с Блазор-сборками выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="60830-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="60830-138">Для работы с сервером Блазор выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="60830-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="60830-139">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="60830-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60830-140">4 \.</span><span class="sxs-lookup"><span data-stu-id="60830-140">4\.</span></span> <span data-ttu-id="60830-141">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="60830-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="60830-142">5 \.</span><span class="sxs-lookup"><span data-stu-id="60830-142">5\.</span></span> <span data-ttu-id="60830-143">Для проекта сервера Блазор среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="60830-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="60830-144">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="60830-144">Select **Yes**.</span></span>

   <span data-ttu-id="60830-145">6 \.</span><span class="sxs-lookup"><span data-stu-id="60830-145">6\.</span></span> <span data-ttu-id="60830-146">При использовании серверного приложения Блазор запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="60830-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="60830-147">Если используется Блазор приложение сборки, выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="60830-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="60830-148">7 \.</span><span class="sxs-lookup"><span data-stu-id="60830-148">7\.</span></span> <span data-ttu-id="60830-149">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="60830-149">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="60830-150">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="60830-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="60830-151">Для работы с Блазор-сборками выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="60830-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="60830-152">Для работы с сервером Блазор выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="60830-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="60830-153">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="60830-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60830-154">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="60830-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="60830-155">Установите последнюю версию [пакета SDK для .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="60830-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="60830-156">При необходимости установите шаблон [блазор-Assembly](xref:blazor/hosting-models#blazor-webassembly) , установив [пакет SDK для .NET Core 3,1 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.1) , а затем выполнив следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="60830-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="60830-157">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="60830-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60830-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60830-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="60830-159">1 \.</span><span class="sxs-lookup"><span data-stu-id="60830-159">1\.</span></span> <span data-ttu-id="60830-160">Установите последнюю версию [Visual Studio](https://visualstudio.com/vs/) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="60830-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="60830-161">2 \.</span><span class="sxs-lookup"><span data-stu-id="60830-161">2\.</span></span> <span data-ttu-id="60830-162">При необходимости установите [Visual Studio 16,4 Preview 2 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** для разработки приложений блазор.</span><span class="sxs-lookup"><span data-stu-id="60830-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="60830-163">3 \.</span><span class="sxs-lookup"><span data-stu-id="60830-163">3\.</span></span> <span data-ttu-id="60830-164">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="60830-164">Create a new project.</span></span>

   <span data-ttu-id="60830-165">4 \.</span><span class="sxs-lookup"><span data-stu-id="60830-165">4\.</span></span> <span data-ttu-id="60830-166">Выберите **приложение блазор**.</span><span class="sxs-lookup"><span data-stu-id="60830-166">Select **Blazor App**.</span></span> <span data-ttu-id="60830-167">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="60830-167">Select **Next**.</span></span>

   <span data-ttu-id="60830-168">5 \.</span><span class="sxs-lookup"><span data-stu-id="60830-168">5\.</span></span> <span data-ttu-id="60830-169">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="60830-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="60830-170">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="60830-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="60830-171">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="60830-171">Select **Create**.</span></span>

   <span data-ttu-id="60830-172">6 \.</span><span class="sxs-lookup"><span data-stu-id="60830-172">6\.</span></span> <span data-ttu-id="60830-173">Для интерфейса Блазор можно выбрать шаблон **приложения блазор для сборки** .</span><span class="sxs-lookup"><span data-stu-id="60830-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="60830-174">Для удобства работы с сервером Блазор выберите шаблон **серверное приложение блазор** .</span><span class="sxs-lookup"><span data-stu-id="60830-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="60830-175">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="60830-175">Select **Create**.</span></span> <span data-ttu-id="60830-176">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="60830-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60830-177">7 \.</span><span class="sxs-lookup"><span data-stu-id="60830-177">7\.</span></span> <span data-ttu-id="60830-178">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="60830-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="60830-179">Если вы установили расширение Visual Studio Блазор для предыдущей предварительной версии ASP.NET Core Блазор (Предварительная версия 6 или более ранняя версия), вы можете удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="60830-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="60830-180">Установка шаблонов Блазор в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="60830-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="60830-181">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="60830-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="60830-182">1 \.</span><span class="sxs-lookup"><span data-stu-id="60830-182">1\.</span></span> <span data-ttu-id="60830-183">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="60830-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="60830-184">2 \.</span><span class="sxs-lookup"><span data-stu-id="60830-184">2\.</span></span> <span data-ttu-id="60830-185">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="60830-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="60830-186">3 \.</span><span class="sxs-lookup"><span data-stu-id="60830-186">3\.</span></span> <span data-ttu-id="60830-187">Для работы с Блазор-сборками выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="60830-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="60830-188">Для работы с сервером Блазор выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="60830-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="60830-189">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="60830-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60830-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="60830-190">4\.</span></span> <span data-ttu-id="60830-191">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="60830-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="60830-192">5 \.</span><span class="sxs-lookup"><span data-stu-id="60830-192">5\.</span></span> <span data-ttu-id="60830-193">Для проекта сервера Блазор среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="60830-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="60830-194">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="60830-194">Select **Yes**.</span></span>

   <span data-ttu-id="60830-195">6 \.</span><span class="sxs-lookup"><span data-stu-id="60830-195">6\.</span></span> <span data-ttu-id="60830-196">При использовании серверного приложения Блазор запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="60830-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="60830-197">Если используется Блазор приложение сборки, выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="60830-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="60830-198">7 \.</span><span class="sxs-lookup"><span data-stu-id="60830-198">7\.</span></span> <span data-ttu-id="60830-199">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="60830-199">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="60830-200">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="60830-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="60830-201">Для работы с Блазор-сборками выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="60830-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="60830-202">Для работы с сервером Блазор выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="60830-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="60830-203">Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="60830-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60830-204">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="60830-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="60830-205">Из вкладок боковой панели доступны несколько страниц:</span><span class="sxs-lookup"><span data-stu-id="60830-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="60830-206">Главная страница</span><span class="sxs-lookup"><span data-stu-id="60830-206">Home</span></span>
* <span data-ttu-id="60830-207">Счетчик</span><span class="sxs-lookup"><span data-stu-id="60830-207">Counter</span></span>
* <span data-ttu-id="60830-208">Выборка данных</span><span class="sxs-lookup"><span data-stu-id="60830-208">Fetch data</span></span>

<span data-ttu-id="60830-209">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="60830-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="60830-210">Для увеличения счетчика на веб-странице обычно требуется написание кода JavaScript, но с Блазор можно C#использовать.</span><span class="sxs-lookup"><span data-stu-id="60830-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="60830-211">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="60830-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="60830-212">Запрос `/counter` в браузере, как указано в директиве `@page` в верхней части, приводит к тому, что компонент `Counter` отображает его содержимое.</span><span class="sxs-lookup"><span data-stu-id="60830-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="60830-213">Компоненты подготавливаются к просмотру в памяти представления дерева подготовки, которое затем может использоваться для обновления пользовательского интерфейса в гибком и удобном виде.</span><span class="sxs-lookup"><span data-stu-id="60830-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="60830-214">При каждом выборе кнопки « **Click Me** »:</span><span class="sxs-lookup"><span data-stu-id="60830-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="60830-215">Будет вызвано событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="60830-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="60830-216">вызывается метод `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="60830-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="60830-217">Значение `currentCount` увеличивается.</span><span class="sxs-lookup"><span data-stu-id="60830-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="60830-218">Компонент снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="60830-218">The component is rendered again.</span></span>

<span data-ttu-id="60830-219">Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет только измененное содержимое к модель DOM (DOM).</span><span class="sxs-lookup"><span data-stu-id="60830-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="60830-220">Добавьте компонент в другой компонент с помощью синтаксиса HTML.</span><span class="sxs-lookup"><span data-stu-id="60830-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="60830-221">Например, добавьте компонент `Counter` на домашнюю страницу приложения, добавив элемент `<Counter />` в компонент `Index`.</span><span class="sxs-lookup"><span data-stu-id="60830-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="60830-222">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="60830-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="60830-223">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="60830-223">Run the app.</span></span> <span data-ttu-id="60830-224">Домашняя страница имеет собственный счетчик, предоставляемый компонентом `Counter`.</span><span class="sxs-lookup"><span data-stu-id="60830-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="60830-225">Параметры компонента указываются с помощью атрибутов или [дочернего содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="60830-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="60830-226">Чтобы добавить параметр в компонент `Counter`, обновите блок `@code` компонента:</span><span class="sxs-lookup"><span data-stu-id="60830-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="60830-227">Добавьте открытое свойство для `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="60830-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="60830-228">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="60830-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="60830-229">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="60830-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="60830-230">Укажите `IncrementAmount` в элементе `<Counter>` компонента `Index` с помощью атрибута.</span><span class="sxs-lookup"><span data-stu-id="60830-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="60830-231">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="60830-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="60830-232">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="60830-232">Run the app.</span></span> <span data-ttu-id="60830-233">Компонент `Index` имеет собственный счетчик, который увеличивается на десять каждый раз при выборе кнопки " **нажать** ".</span><span class="sxs-lookup"><span data-stu-id="60830-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="60830-234">Компонент `Counter` (*Counter. Razor*) в `/counter` продолжит приращение на единицу.</span><span class="sxs-lookup"><span data-stu-id="60830-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60830-235">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="60830-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="60830-236">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="60830-236">Additional resources</span></span>

* <xref:signalr/introduction>
