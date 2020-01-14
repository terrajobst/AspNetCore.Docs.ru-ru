---
title: Начало работы с Blazor ASP.NET Core
author: guardrex
description: Приступайте к работе с Blazor, создав Blazor приложение с любыми инструментами по своему усмотрению.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/09/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 554f4daff92a0839ee7679287a4618e9b51e0fe5
ms.sourcegitcommit: 925cdbd94613243f33bc7613a62ea34006219931
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921305"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="e24f7-103">Начало работы с Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e24f7-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="e24f7-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e24f7-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="e24f7-105">Начало работы с Blazor:</span><span class="sxs-lookup"><span data-stu-id="e24f7-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="e24f7-106">Установите [пакет SDK для .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="e24f7-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="e24f7-107">При необходимости установите шаблон [Blazor сборки](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="e24f7-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="e24f7-108">Установите [пакет SDK для .NET Core 3,1 или более поздней версии (Предварительная версия)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="e24f7-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="e24f7-109">Выполните следующую команду в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="e24f7-109">Run the following command in a command shell.</span></span> <span data-ttu-id="e24f7-110">Объект [Microsoft.AspNetCore.Blazor.Пакет шаблонов](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) имеет предварительную версию, тогда как Blazor сборка доступна в предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="e24f7-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="e24f7-111">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="e24f7-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e24f7-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e24f7-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="e24f7-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-113">1\.</span></span> <span data-ttu-id="e24f7-114">Установите [Visual Studio 16,4 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="e24f7-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-115">2\.</span></span> <span data-ttu-id="e24f7-116">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="e24f7-116">Create a new project.</span></span>

   <span data-ttu-id="e24f7-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-117">3\.</span></span> <span data-ttu-id="e24f7-118">Выберите **Blazor приложение**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-118">Select **Blazor App**.</span></span> <span data-ttu-id="e24f7-119">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-119">Select **Next**.</span></span>

   <span data-ttu-id="e24f7-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-120">4\.</span></span> <span data-ttu-id="e24f7-121">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e24f7-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="e24f7-122">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="e24f7-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="e24f7-123">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-123">Select **Create**.</span></span>

   <span data-ttu-id="e24f7-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-124">5\.</span></span> <span data-ttu-id="e24f7-125">Для Blazor процесса сборки выберите шаблон **приложенияBlazor сборки** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="e24f7-126">Для Blazor работы с сервером выберите шаблон **серверное приложениеBlazor** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="e24f7-127">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-127">Select **Create**.</span></span> <span data-ttu-id="e24f7-128">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e24f7-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e24f7-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-129">6\.</span></span> <span data-ttu-id="e24f7-130">Нажмите клавишу **Ctrl**+**F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="e24f7-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e24f7-131">Если вы установили расширение Blazor Visual Studio для предыдущей предварительной версии ASP.NET Core Blazor (Предварительная версия 6 или более ранней версии), вы можете удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="e24f7-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="e24f7-132">Установка шаблонов Blazor в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e24f7-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e24f7-133">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e24f7-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="e24f7-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-134">1\.</span></span> <span data-ttu-id="e24f7-135">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="e24f7-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="e24f7-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-136">2\.</span></span> <span data-ttu-id="e24f7-137">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="e24f7-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="e24f7-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-138">3\.</span></span> <span data-ttu-id="e24f7-139">Для Blazor процесса сборки выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e24f7-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="e24f7-140">Для работы с Blazor Server выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e24f7-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="e24f7-141">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e24f7-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e24f7-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-142">4\.</span></span> <span data-ttu-id="e24f7-143">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e24f7-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="e24f7-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-144">5\.</span></span> <span data-ttu-id="e24f7-145">Для проекта Blazor Server Среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="e24f7-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="e24f7-146">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-146">Select **Yes**.</span></span>

   <span data-ttu-id="e24f7-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-147">6\.</span></span> <span data-ttu-id="e24f7-148">При использовании серверного приложения Blazor запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e24f7-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="e24f7-149">При использовании Blazor приложения сборки выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="e24f7-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="e24f7-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-150">7\.</span></span> <span data-ttu-id="e24f7-151">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e24f7-152">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e24f7-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="e24f7-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-153">1\.</span></span> <span data-ttu-id="e24f7-154">Установите [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="e24f7-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="e24f7-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-155">2\.</span></span> <span data-ttu-id="e24f7-156">Выберите **файл** > **создать решение** или создать **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="e24f7-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-157">3\.</span></span> <span data-ttu-id="e24f7-158">На боковой панели выберите > **приложение** **.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="e24f7-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-159">4\.</span></span> <span data-ttu-id="e24f7-160">Выберите шаблон **серверное приложениеBlazor** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="e24f7-161">В настоящее время в Visual Studio для Mac доступен только шаблон Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="e24f7-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="e24f7-162">Для Blazor процесса сборки следуйте инструкциям на вкладке **.NET Core CLI** . Выбрав шаблон Blazor Server, нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="e24f7-163">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e24f7-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="e24f7-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-164">5\.</span></span> <span data-ttu-id="e24f7-165">Задайте для параметра **Целевая платформа** значение **.NET Core 3,1** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="e24f7-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-166">6\.</span></span> <span data-ttu-id="e24f7-167">В поле **имя проекта** Назовите приложение `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="e24f7-168">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-168">Select **Create**.</span></span>

   <span data-ttu-id="e24f7-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-169">7\.</span></span> <span data-ttu-id="e24f7-170">Выберите **выполнить** > **выполнить без отладки** , чтобы запустить приложение *без отладчика*.</span><span class="sxs-lookup"><span data-stu-id="e24f7-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="e24f7-171">Запустите приложение с помощью программы " **начать отладку** ", чтобы запустить приложение *с помощью отладчика*.</span><span class="sxs-lookup"><span data-stu-id="e24f7-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e24f7-172">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e24f7-172">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="e24f7-173">Для Blazor процесса сборки выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e24f7-173">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="e24f7-174">Для работы с Blazor Server выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e24f7-174">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="e24f7-175">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e24f7-175">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e24f7-176">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-176">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="e24f7-177">Установите последнюю версию [пакета SDK для .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="e24f7-177">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="e24f7-178">При необходимости установите шаблон [Blazor сборки](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="e24f7-178">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="e24f7-179">Установите [пакет SDK для .NET Core 3,1 или более поздней версии (Предварительная версия)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="e24f7-179">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="e24f7-180">Выполните следующую команду в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="e24f7-180">Run the following command in a command shell.</span></span> <span data-ttu-id="e24f7-181">Объект [Microsoft.AspNetCore.Blazor.Пакет шаблонов](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) имеет предварительную версию, тогда как Blazor сборка доступна в предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="e24f7-181">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="e24f7-182">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="e24f7-182">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e24f7-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e24f7-183">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="e24f7-184">1 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-184">1\.</span></span> <span data-ttu-id="e24f7-185">Установите последнюю версию [Visual Studio](https://visualstudio.com/vs/) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-185">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="e24f7-186">2 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-186">2\.</span></span> <span data-ttu-id="e24f7-187">При необходимости установите [Visual Studio 16,4 Preview 2 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и web Development** для Blazor разработки приложений веб-сборки.</span><span class="sxs-lookup"><span data-stu-id="e24f7-187">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="e24f7-188">3 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-188">3\.</span></span> <span data-ttu-id="e24f7-189">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="e24f7-189">Create a new project.</span></span>

   <span data-ttu-id="e24f7-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-190">4\.</span></span> <span data-ttu-id="e24f7-191">Выберите **Blazor приложение**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-191">Select **Blazor App**.</span></span> <span data-ttu-id="e24f7-192">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-192">Select **Next**.</span></span>

   <span data-ttu-id="e24f7-193">5 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-193">5\.</span></span> <span data-ttu-id="e24f7-194">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e24f7-194">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="e24f7-195">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="e24f7-195">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="e24f7-196">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-196">Select **Create**.</span></span>

   <span data-ttu-id="e24f7-197">6 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-197">6\.</span></span> <span data-ttu-id="e24f7-198">Для Blazor процесса сборки выберите шаблон **приложенияBlazor сборки** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-198">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="e24f7-199">Для Blazor работы с сервером выберите шаблон **серверное приложениеBlazor** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-199">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="e24f7-200">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-200">Select **Create**.</span></span> <span data-ttu-id="e24f7-201">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e24f7-201">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e24f7-202">7 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-202">7\.</span></span> <span data-ttu-id="e24f7-203">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="e24f7-203">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e24f7-204">Если вы установили расширение Blazor Visual Studio для предыдущей предварительной версии ASP.NET Core Blazor (Предварительная версия 6 или более ранней версии), вы можете удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="e24f7-204">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="e24f7-205">Установка шаблонов Blazor в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e24f7-205">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e24f7-206">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e24f7-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="e24f7-207">1 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-207">1\.</span></span> <span data-ttu-id="e24f7-208">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="e24f7-208">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="e24f7-209">2 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-209">2\.</span></span> <span data-ttu-id="e24f7-210">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="e24f7-210">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="e24f7-211">3 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-211">3\.</span></span> <span data-ttu-id="e24f7-212">Для Blazor процесса сборки выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e24f7-212">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="e24f7-213">Для работы с Blazor Server выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e24f7-213">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="e24f7-214">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e24f7-214">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e24f7-215">4 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-215">4\.</span></span> <span data-ttu-id="e24f7-216">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e24f7-216">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="e24f7-217">5 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-217">5\.</span></span> <span data-ttu-id="e24f7-218">Для проекта Blazor Server Среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="e24f7-218">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="e24f7-219">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-219">Select **Yes**.</span></span>

   <span data-ttu-id="e24f7-220">6 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-220">6\.</span></span> <span data-ttu-id="e24f7-221">При использовании серверного приложения Blazor запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e24f7-221">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="e24f7-222">При использовании Blazor приложения сборки выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="e24f7-222">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="e24f7-223">7 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-223">7\.</span></span> <span data-ttu-id="e24f7-224">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-224">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e24f7-225">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e24f7-225">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="e24f7-226">1 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-226">1\.</span></span> <span data-ttu-id="e24f7-227">Установите [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="e24f7-227">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="e24f7-228">Переключите [канал обновления в режим предварительного просмотра](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="e24f7-228">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="e24f7-229">2 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-229">2\.</span></span> <span data-ttu-id="e24f7-230">Выберите **файл** > **создать решение** или создать **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-230">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="e24f7-231">3 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-231">3\.</span></span> <span data-ttu-id="e24f7-232">На боковой панели выберите > **приложение** **.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-232">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="e24f7-233">4 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-233">4\.</span></span> <span data-ttu-id="e24f7-234">Выберите шаблон **серверное приложениеBlazor** .</span><span class="sxs-lookup"><span data-stu-id="e24f7-234">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="e24f7-235">В настоящее время в Visual Studio для Mac доступен только шаблон Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="e24f7-235">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="e24f7-236">Для Blazor процесса сборки следуйте инструкциям на вкладке **.NET Core CLI** . Выбрав шаблон Blazor Server, нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-236">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="e24f7-237">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e24f7-237">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="e24f7-238">5 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-238">5\.</span></span> <span data-ttu-id="e24f7-239">Задайте для параметра **Целевая платформа** значение **.NET Core 3,0** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-239">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="e24f7-240">6 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-240">6\.</span></span> <span data-ttu-id="e24f7-241">В поле **имя проекта** Назовите приложение `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-241">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="e24f7-242">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e24f7-242">Select **Create**.</span></span>

   <span data-ttu-id="e24f7-243">7 \.</span><span class="sxs-lookup"><span data-stu-id="e24f7-243">7\.</span></span> <span data-ttu-id="e24f7-244">Выберите **выполнить** > **выполнить без отладки** , чтобы запустить приложение *без отладчика*.</span><span class="sxs-lookup"><span data-stu-id="e24f7-244">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="e24f7-245">Запустите приложение с помощью программы " **начать отладку** ", чтобы запустить приложение *с помощью отладчика*.</span><span class="sxs-lookup"><span data-stu-id="e24f7-245">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e24f7-246">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e24f7-246">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="e24f7-247">Для Blazor процесса сборки выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e24f7-247">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="e24f7-248">Для работы с Blazor Server выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="e24f7-248">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="e24f7-249">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="e24f7-249">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="e24f7-250">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-250">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="e24f7-251">Из вкладок боковой панели доступны несколько страниц:</span><span class="sxs-lookup"><span data-stu-id="e24f7-251">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="e24f7-252">Домашняя страница</span><span class="sxs-lookup"><span data-stu-id="e24f7-252">Home</span></span>
* <span data-ttu-id="e24f7-253">Счетчик</span><span class="sxs-lookup"><span data-stu-id="e24f7-253">Counter</span></span>
* <span data-ttu-id="e24f7-254">Выборка данных</span><span class="sxs-lookup"><span data-stu-id="e24f7-254">Fetch data</span></span>

<span data-ttu-id="e24f7-255">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="e24f7-255">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="e24f7-256">Увеличение счетчика на веб-странице обычно требует написания JavaScript, но с Blazor можно использовать C#.</span><span class="sxs-lookup"><span data-stu-id="e24f7-256">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="e24f7-257">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="e24f7-257">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="e24f7-258">Запрос `/counter` в браузере, как указано в директиве `@page` в верхней части, заставляет компонент `Counter` визуализировать свое содержимое.</span><span class="sxs-lookup"><span data-stu-id="e24f7-258">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="e24f7-259">Компоненты подготавливаются к просмотру в памяти представления дерева подготовки, которое затем может использоваться для обновления пользовательского интерфейса в гибком и удобном виде.</span><span class="sxs-lookup"><span data-stu-id="e24f7-259">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="e24f7-260">При каждом выборе кнопки « **Click Me** »:</span><span class="sxs-lookup"><span data-stu-id="e24f7-260">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="e24f7-261">Запускается событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-261">The `onclick` event is fired.</span></span>
* <span data-ttu-id="e24f7-262">Вызывается метод `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-262">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="e24f7-263">`currentCount` увеличивается.</span><span class="sxs-lookup"><span data-stu-id="e24f7-263">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="e24f7-264">Компонент снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="e24f7-264">The component is rendered again.</span></span>

<span data-ttu-id="e24f7-265">Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет только измененное содержимое к модель DOM (DOM).</span><span class="sxs-lookup"><span data-stu-id="e24f7-265">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="e24f7-266">Добавьте компонент в другой компонент с помощью синтаксиса HTML.</span><span class="sxs-lookup"><span data-stu-id="e24f7-266">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="e24f7-267">Например, добавьте `Counter` компонент на домашнюю страницу приложения, добавив элемент `<Counter />` в компонент `Index`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-267">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="e24f7-268">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="e24f7-268">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="e24f7-269">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e24f7-269">Run the app.</span></span> <span data-ttu-id="e24f7-270">Домашняя страница имеет собственный счетчик, предоставляемый компонентом `Counter`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-270">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="e24f7-271">Параметры компонента указываются с помощью атрибутов или [дочернего содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="e24f7-271">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="e24f7-272">Чтобы добавить параметр в компонент `Counter`, обновите блок `@code` компонента:</span><span class="sxs-lookup"><span data-stu-id="e24f7-272">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="e24f7-273">Добавьте открытое свойство для `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-273">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="e24f7-274">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="e24f7-274">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="e24f7-275">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="e24f7-275">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="e24f7-276">Укажите `IncrementAmount` в элементе `<Counter>` компонента `Index` с помощью атрибута.</span><span class="sxs-lookup"><span data-stu-id="e24f7-276">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="e24f7-277">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="e24f7-277">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="e24f7-278">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e24f7-278">Run the app.</span></span> <span data-ttu-id="e24f7-279">Компонент `Index` имеет собственный счетчик, который увеличивается на десять каждый раз при выборе кнопки " **нажать** ".</span><span class="sxs-lookup"><span data-stu-id="e24f7-279">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="e24f7-280">Компонент `Counter` (*Counter. Razor*) в `/counter` по-своему увеличивается на единицу.</span><span class="sxs-lookup"><span data-stu-id="e24f7-280">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e24f7-281">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e24f7-281">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="e24f7-282">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e24f7-282">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
