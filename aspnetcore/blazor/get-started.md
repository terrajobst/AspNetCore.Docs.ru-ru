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
ms.openlocfilehash: e368ecaf931d392de7e52ec2d5a2dfd171c2c86f
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943771"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="dca9e-103">Начало работы с Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dca9e-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="dca9e-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dca9e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="dca9e-105">Начало работы с Blazor:</span><span class="sxs-lookup"><span data-stu-id="dca9e-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="dca9e-106">Установите [пакет SDK для .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="dca9e-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="dca9e-107">При необходимости установите шаблон [Blazor сборки](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="dca9e-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="dca9e-108">Установите [пакет SDK для .NET Core 3,1 или более поздней версии (Предварительная версия)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="dca9e-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="dca9e-109">Выполните следующую команду в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="dca9e-109">Run the following command in a command shell.</span></span> <span data-ttu-id="dca9e-110">Объект [Microsoft. AspNetCore.Blazor. Пакет шаблонов](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) имеет предварительную версию, тогда как Blazor сборка доступна в предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="dca9e-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="dca9e-111">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="dca9e-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dca9e-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dca9e-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="dca9e-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-113">1\.</span></span> <span data-ttu-id="dca9e-114">Установите [Visual Studio 16,4 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="dca9e-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-115">2\.</span></span> <span data-ttu-id="dca9e-116">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="dca9e-116">Create a new project.</span></span>

   <span data-ttu-id="dca9e-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-117">3\.</span></span> <span data-ttu-id="dca9e-118">Выберите **Blazor приложение**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-118">Select **Blazor App**.</span></span> <span data-ttu-id="dca9e-119">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-119">Select **Next**.</span></span>

   <span data-ttu-id="dca9e-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-120">4\.</span></span> <span data-ttu-id="dca9e-121">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="dca9e-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="dca9e-122">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="dca9e-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="dca9e-123">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-123">Select **Create**.</span></span>

   <span data-ttu-id="dca9e-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-124">5\.</span></span> <span data-ttu-id="dca9e-125">Для Blazor процесса сборки выберите шаблон **приложенияBlazor сборки** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="dca9e-126">Для Blazor работы с сервером выберите шаблон **серверное приложениеBlazor** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="dca9e-127">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-127">Select **Create**.</span></span> <span data-ttu-id="dca9e-128">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="dca9e-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="dca9e-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-129">6\.</span></span> <span data-ttu-id="dca9e-130">Нажмите клавишу **Ctrl**+**F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="dca9e-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="dca9e-131">Если вы установили расширение Blazor Visual Studio для предыдущей предварительной версии ASP.NET Core Blazor (Предварительная версия 6 или более ранней версии), вы можете удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="dca9e-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="dca9e-132">Установка шаблонов Blazor в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dca9e-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dca9e-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dca9e-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="dca9e-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-134">1\.</span></span> <span data-ttu-id="dca9e-135">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="dca9e-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="dca9e-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-136">2\.</span></span> <span data-ttu-id="dca9e-137">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="dca9e-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="dca9e-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-138">3\.</span></span> <span data-ttu-id="dca9e-139">Для Blazor процесса сборки выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="dca9e-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="dca9e-140">Для работы с Blazor Server выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="dca9e-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="dca9e-141">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="dca9e-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="dca9e-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-142">4\.</span></span> <span data-ttu-id="dca9e-143">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dca9e-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="dca9e-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-144">5\.</span></span> <span data-ttu-id="dca9e-145">Для проекта Blazor Server Среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="dca9e-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="dca9e-146">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-146">Select **Yes**.</span></span>

   <span data-ttu-id="dca9e-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-147">6\.</span></span> <span data-ttu-id="dca9e-148">При использовании серверного приложения Blazor запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dca9e-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="dca9e-149">При использовании Blazor приложения сборки выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="dca9e-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="dca9e-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-150">7\.</span></span> <span data-ttu-id="dca9e-151">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dca9e-152">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="dca9e-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="dca9e-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-153">1\.</span></span> <span data-ttu-id="dca9e-154">Установите [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="dca9e-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="dca9e-155">Переключите [канал обновления в режим предварительного просмотра](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="dca9e-155">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="dca9e-156">2 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-156">2\.</span></span> <span data-ttu-id="dca9e-157">Выберите **файл** > **создать решение** или создать **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-157">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="dca9e-158">3 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-158">3\.</span></span> <span data-ttu-id="dca9e-159">На боковой панели выберите > **приложение** **.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-159">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="dca9e-160">4 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-160">4\.</span></span> <span data-ttu-id="dca9e-161">Выберите шаблон **серверное приложениеBlazor** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-161">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="dca9e-162">В настоящее время в Visual Studio для Mac доступен только шаблон Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="dca9e-162">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="dca9e-163">Для Blazor процесса сборки следуйте инструкциям на вкладке **.NET Core CLI** . Выбрав шаблон Blazor Server, нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-163">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="dca9e-164">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="dca9e-164">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="dca9e-165">5 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-165">5\.</span></span> <span data-ttu-id="dca9e-166">Задайте для параметра **Целевая платформа** значение **.NET Core 3,1** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-166">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="dca9e-167">6 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-167">6\.</span></span> <span data-ttu-id="dca9e-168">В поле **имя проекта** Назовите приложение `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-168">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="dca9e-169">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-169">Select **Create**.</span></span>

   <span data-ttu-id="dca9e-170">7 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-170">7\.</span></span> <span data-ttu-id="dca9e-171">Выберите **выполнить** > **выполнить без отладки** , чтобы запустить приложение *без отладчика*.</span><span class="sxs-lookup"><span data-stu-id="dca9e-171">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="dca9e-172">Запустите приложение с помощью программы " **начать отладку** ", чтобы запустить приложение *с помощью отладчика*.</span><span class="sxs-lookup"><span data-stu-id="dca9e-172">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dca9e-173">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="dca9e-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="dca9e-174">Для Blazor процесса сборки выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="dca9e-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="dca9e-175">Для работы с Blazor Server выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="dca9e-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="dca9e-176">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="dca9e-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="dca9e-177">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="dca9e-178">Установите последнюю версию [пакета SDK для .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="dca9e-178">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="dca9e-179">При необходимости установите шаблон [Blazor сборки](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="dca9e-179">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="dca9e-180">Установите [пакет SDK для .NET Core 3,1 или более поздней версии (Предварительная версия)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="dca9e-180">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="dca9e-181">Выполните следующую команду в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="dca9e-181">Run the following command in a command shell.</span></span> <span data-ttu-id="dca9e-182">Объект [Microsoft. AspNetCore.Blazor. Пакет шаблонов](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) имеет предварительную версию, тогда как Blazor сборка доступна в предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="dca9e-182">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="dca9e-183">Следуйте указаниям по выбору инструментов:</span><span class="sxs-lookup"><span data-stu-id="dca9e-183">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dca9e-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dca9e-184">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="dca9e-185">1 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-185">1\.</span></span> <span data-ttu-id="dca9e-186">Установите последнюю версию [Visual Studio](https://visualstudio.com/vs/) с рабочей нагрузкой **ASP.NET и веб-разработка** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-186">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="dca9e-187">2 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-187">2\.</span></span> <span data-ttu-id="dca9e-188">При необходимости установите [Visual Studio 16,4 Preview 2 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и web Development** для Blazor разработки приложений веб-сборки.</span><span class="sxs-lookup"><span data-stu-id="dca9e-188">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="dca9e-189">3 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-189">3\.</span></span> <span data-ttu-id="dca9e-190">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="dca9e-190">Create a new project.</span></span>

   <span data-ttu-id="dca9e-191">4 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-191">4\.</span></span> <span data-ttu-id="dca9e-192">Выберите **Blazor приложение**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-192">Select **Blazor App**.</span></span> <span data-ttu-id="dca9e-193">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-193">Select **Next**.</span></span>

   <span data-ttu-id="dca9e-194">5 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-194">5\.</span></span> <span data-ttu-id="dca9e-195">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="dca9e-195">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="dca9e-196">Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта.</span><span class="sxs-lookup"><span data-stu-id="dca9e-196">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="dca9e-197">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-197">Select **Create**.</span></span>

   <span data-ttu-id="dca9e-198">6 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-198">6\.</span></span> <span data-ttu-id="dca9e-199">Для Blazor процесса сборки выберите шаблон **приложенияBlazor сборки** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-199">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="dca9e-200">Для Blazor работы с сервером выберите шаблон **серверное приложениеBlazor** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-200">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="dca9e-201">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-201">Select **Create**.</span></span> <span data-ttu-id="dca9e-202">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="dca9e-202">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="dca9e-203">7 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-203">7\.</span></span> <span data-ttu-id="dca9e-204">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="dca9e-204">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="dca9e-205">Если вы установили расширение Blazor Visual Studio для предыдущей предварительной версии ASP.NET Core Blazor (Предварительная версия 6 или более ранней версии), вы можете удалить расширение.</span><span class="sxs-lookup"><span data-stu-id="dca9e-205">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="dca9e-206">Установка шаблонов Blazor в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dca9e-206">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dca9e-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dca9e-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="dca9e-208">1 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-208">1\.</span></span> <span data-ttu-id="dca9e-209">Установите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="dca9e-209">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="dca9e-210">2 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-210">2\.</span></span> <span data-ttu-id="dca9e-211">Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="dca9e-211">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="dca9e-212">3 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-212">3\.</span></span> <span data-ttu-id="dca9e-213">Для Blazor процесса сборки выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="dca9e-213">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="dca9e-214">Для работы с Blazor Server выполните следующую команду в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="dca9e-214">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="dca9e-215">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="dca9e-215">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="dca9e-216">4 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-216">4\.</span></span> <span data-ttu-id="dca9e-217">Откройте папку *WebApplication1* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dca9e-217">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="dca9e-218">5 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-218">5\.</span></span> <span data-ttu-id="dca9e-219">Для проекта Blazor Server Среда IDE запрашивает добавление ресурсов для сборки и отладки проекта.</span><span class="sxs-lookup"><span data-stu-id="dca9e-219">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="dca9e-220">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-220">Select **Yes**.</span></span>

   <span data-ttu-id="dca9e-221">6 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-221">6\.</span></span> <span data-ttu-id="dca9e-222">При использовании серверного приложения Blazor запустите приложение с помощью отладчика Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dca9e-222">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="dca9e-223">При использовании Blazor приложения сборки выполните `dotnet run` из папки проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="dca9e-223">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="dca9e-224">7 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-224">7\.</span></span> <span data-ttu-id="dca9e-225">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-225">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dca9e-226">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="dca9e-226">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="dca9e-227">1 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-227">1\.</span></span> <span data-ttu-id="dca9e-228">Установите [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="dca9e-228">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="dca9e-229">Переключите [канал обновления в режим предварительного просмотра](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="dca9e-229">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="dca9e-230">2 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-230">2\.</span></span> <span data-ttu-id="dca9e-231">Выберите **файл** > **создать решение** или создать **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-231">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="dca9e-232">3 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-232">3\.</span></span> <span data-ttu-id="dca9e-233">На боковой панели выберите > **приложение** **.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-233">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="dca9e-234">4 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-234">4\.</span></span> <span data-ttu-id="dca9e-235">Выберите шаблон **серверное приложениеBlazor** .</span><span class="sxs-lookup"><span data-stu-id="dca9e-235">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="dca9e-236">В настоящее время в Visual Studio для Mac доступен только шаблон Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="dca9e-236">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="dca9e-237">Для Blazor процесса сборки следуйте инструкциям на вкладке **.NET Core CLI** . Выбрав шаблон Blazor Server, нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-237">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="dca9e-238">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="dca9e-238">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="dca9e-239">5 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-239">5\.</span></span> <span data-ttu-id="dca9e-240">Задайте для параметра **Целевая платформа** значение **.NET Core 3,0** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-240">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="dca9e-241">6 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-241">6\.</span></span> <span data-ttu-id="dca9e-242">В поле **имя проекта** Назовите приложение `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-242">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="dca9e-243">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="dca9e-243">Select **Create**.</span></span>

   <span data-ttu-id="dca9e-244">7 \.</span><span class="sxs-lookup"><span data-stu-id="dca9e-244">7\.</span></span> <span data-ttu-id="dca9e-245">Выберите **выполнить** > **выполнить без отладки** , чтобы запустить приложение *без отладчика*.</span><span class="sxs-lookup"><span data-stu-id="dca9e-245">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="dca9e-246">Запустите приложение с помощью программы " **начать отладку** ", чтобы запустить приложение *с помощью отладчика*.</span><span class="sxs-lookup"><span data-stu-id="dca9e-246">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dca9e-247">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="dca9e-247">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="dca9e-248">Для Blazor процесса сборки выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="dca9e-248">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="dca9e-249">Для работы с Blazor Server выполните следующие команды в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="dca9e-249">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="dca9e-250">Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="dca9e-250">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="dca9e-251">В браузере перейдите на адрес `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-251">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="dca9e-252">Из вкладок боковой панели доступны несколько страниц:</span><span class="sxs-lookup"><span data-stu-id="dca9e-252">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="dca9e-253">Корневая</span><span class="sxs-lookup"><span data-stu-id="dca9e-253">Home</span></span>
* <span data-ttu-id="dca9e-254">Счетчик</span><span class="sxs-lookup"><span data-stu-id="dca9e-254">Counter</span></span>
* <span data-ttu-id="dca9e-255">Выборка данных</span><span class="sxs-lookup"><span data-stu-id="dca9e-255">Fetch data</span></span>

<span data-ttu-id="dca9e-256">На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="dca9e-256">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="dca9e-257">Увеличение счетчика на веб-странице обычно требует написания JavaScript, но с Blazor можно использовать C#.</span><span class="sxs-lookup"><span data-stu-id="dca9e-257">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="dca9e-258">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="dca9e-258">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="dca9e-259">Запрос `/counter` в браузере, как указано в директиве `@page` в верхней части, заставляет компонент `Counter` визуализировать свое содержимое.</span><span class="sxs-lookup"><span data-stu-id="dca9e-259">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="dca9e-260">Компоненты подготавливаются к просмотру в памяти представления дерева подготовки, которое затем может использоваться для обновления пользовательского интерфейса в гибком и удобном виде.</span><span class="sxs-lookup"><span data-stu-id="dca9e-260">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="dca9e-261">При каждом выборе кнопки « **Click Me** »:</span><span class="sxs-lookup"><span data-stu-id="dca9e-261">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="dca9e-262">Запускается событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-262">The `onclick` event is fired.</span></span>
* <span data-ttu-id="dca9e-263">вызывается метод `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="dca9e-263">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="dca9e-264">`currentCount` увеличивается.</span><span class="sxs-lookup"><span data-stu-id="dca9e-264">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="dca9e-265">Компонент снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="dca9e-265">The component is rendered again.</span></span>

<span data-ttu-id="dca9e-266">Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет только измененное содержимое к модель DOM (DOM).</span><span class="sxs-lookup"><span data-stu-id="dca9e-266">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="dca9e-267">Добавьте компонент в другой компонент с помощью синтаксиса HTML.</span><span class="sxs-lookup"><span data-stu-id="dca9e-267">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="dca9e-268">Например, добавьте `Counter` компонент на домашнюю страницу приложения, добавив элемент `<Counter />` в компонент `Index`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-268">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="dca9e-269">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="dca9e-269">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="dca9e-270">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="dca9e-270">Run the app.</span></span> <span data-ttu-id="dca9e-271">Домашняя страница имеет собственный счетчик, предоставляемый компонентом `Counter`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-271">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="dca9e-272">Параметры компонента указываются с помощью атрибутов или [дочернего содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="dca9e-272">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="dca9e-273">Чтобы добавить параметр в компонент `Counter`, обновите блок `@code` компонента:</span><span class="sxs-lookup"><span data-stu-id="dca9e-273">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="dca9e-274">Добавьте открытое свойство для `IncrementAmount` с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-274">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="dca9e-275">Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="dca9e-275">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="dca9e-276">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="dca9e-276">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="dca9e-277">Укажите `IncrementAmount` в элементе `<Counter>` компонента `Index` с помощью атрибута.</span><span class="sxs-lookup"><span data-stu-id="dca9e-277">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="dca9e-278">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="dca9e-278">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="dca9e-279">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="dca9e-279">Run the app.</span></span> <span data-ttu-id="dca9e-280">Компонент `Index` имеет собственный счетчик, который увеличивается на десять каждый раз при выборе кнопки " **нажать** ".</span><span class="sxs-lookup"><span data-stu-id="dca9e-280">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="dca9e-281">Компонент `Counter` (*Counter. Razor*) в `/counter` по-своему увеличивается на единицу.</span><span class="sxs-lookup"><span data-stu-id="dca9e-281">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dca9e-282">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="dca9e-282">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="dca9e-283">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="dca9e-283">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
