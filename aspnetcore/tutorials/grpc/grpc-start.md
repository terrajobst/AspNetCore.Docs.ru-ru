---
title: Создание клиента и сервера gRPC .NET Core в ASP.NET Core
author: juntaoluo
description: В этом учебнике объясняется, как создать службу и клиента gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 496f659bd51e2404a936bea8aad77e674e1a285d
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022496"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="185f6-104">Учебник. Создание клиента и сервера gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="185f6-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="185f6-105">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="185f6-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="185f6-106">В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core и сервер gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="185f6-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="185f6-107">В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="185f6-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="185f6-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="185f6-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="185f6-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="185f6-110">Создание сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="185f6-111">Создание клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="185f6-112">Тестирование службы клиента gRPC с помощью службы Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="185f6-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="185f6-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="185f6-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="185f6-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="185f6-115">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="185f6-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="185f6-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="185f6-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="185f6-117">Создание службы gRPC</span><span class="sxs-lookup"><span data-stu-id="185f6-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="185f6-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="185f6-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="185f6-119">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="185f6-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="185f6-120">В диалоговом окне **Создать проект** выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="185f6-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="185f6-121">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="185f6-121">Select **Next**</span></span>
* <span data-ttu-id="185f6-122">Присвойте проекту имя **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="185f6-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="185f6-123">Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.</span><span class="sxs-lookup"><span data-stu-id="185f6-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="185f6-124">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="185f6-124">Select **Create**</span></span>
* <span data-ttu-id="185f6-125">в диалоговом окне **Создание веб-приложения ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="185f6-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="185f6-126">В раскрывающемся меню выберите **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="185f6-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="185f6-127">Выберите шаблон **gRPC Service**.</span><span class="sxs-lookup"><span data-stu-id="185f6-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="185f6-128">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="185f6-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="185f6-129">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="185f6-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="185f6-130">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="185f6-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="185f6-131">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="185f6-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="185f6-132">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="185f6-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="185f6-133">С помощью команды `dotnet new` в папке *GrpcGreeter* создается служба gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="185f6-134">С помощью команды `code` папка *GrpcGreeter* открывается в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="185f6-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="185f6-135">Появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'GrpcGreeter'. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="185f6-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="185f6-136">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="185f6-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="185f6-137">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="185f6-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="185f6-138">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="185f6-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="185f6-139">Эти команды позволяют создать службу gRPC с помощью [.NET Core CLI](/dotnet/core/tools/dotnet).</span><span class="sxs-lookup"><span data-stu-id="185f6-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="185f6-140">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="185f6-140">Open the project</span></span>

<span data-ttu-id="185f6-141">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="185f6-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="185f6-142">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="185f6-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="185f6-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="185f6-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="185f6-144">Нажмите `Ctrl+F5`, чтобы запустить службу gRPC без отладчика.</span><span class="sxs-lookup"><span data-stu-id="185f6-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="185f6-145">Служба запустится в командной строке Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="185f6-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="185f6-146">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="185f6-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="185f6-147">Выполните команду `dotnet run` в командной строке, чтобы запустить проект gRPC Greeter с именем *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="185f6-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="185f6-148">В журналах отобразится служба, которая ожидает передачи данных через `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="185f6-148">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="185f6-149">Шаблон gRPC настроен для использования [протокола TLS](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="185f6-149">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="185f6-150">Клиенты gRPC должны использовать протокол HTTPS для обращения к серверу.</span><span class="sxs-lookup"><span data-stu-id="185f6-150">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="185f6-151">macOS не поддерживает ASP.NET Core gRPC с TLS.</span><span class="sxs-lookup"><span data-stu-id="185f6-151">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="185f6-152">Для успешного запуска служб gRPC в macOS требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="185f6-152">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="185f6-153">Дополнительные сведения см. в статье [Не удается запустить приложение ASP.NET Core gRPC в macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="185f6-153">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="185f6-154">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="185f6-154">Examine the project files</span></span>

<span data-ttu-id="185f6-155">Файлы проекта *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="185f6-155">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="185f6-156">*greet.proto*: Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-156">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="185f6-157">Дополнительные сведения см. в разделе [Введение в gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="185f6-157">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="185f6-158">Папка *Services* содержит реализацию службы `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="185f6-158">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="185f6-159">*appSettings.json*: содержит данные конфигурации, такие как протокол, используемый в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="185f6-159">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="185f6-160">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="185f6-160">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="185f6-161">*Program.cs*: содержит точку входа для службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-161">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="185f6-162">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="185f6-162">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="185f6-163">*Startup.cs*: содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="185f6-163">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="185f6-164">Дополнительные сведения: [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="185f6-164">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="185f6-165">Создание клиента gRPC в консольном приложении .NET</span><span class="sxs-lookup"><span data-stu-id="185f6-165">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="185f6-166">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="185f6-166">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="185f6-167">Откройте еще один экземпляр Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="185f6-167">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="185f6-168">Выберите **Файл** > **Создать** > **Проект** в меню.</span><span class="sxs-lookup"><span data-stu-id="185f6-168">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="185f6-169">В диалоговом окне **Создать проект** выберите **Консольное приложение (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="185f6-169">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="185f6-170">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="185f6-170">Select **Next**</span></span>
* <span data-ttu-id="185f6-171">В текстовом поле **Имя** введите "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="185f6-171">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="185f6-172">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="185f6-172">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="185f6-173">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="185f6-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="185f6-174">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="185f6-174">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="185f6-175">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="185f6-175">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="185f6-176">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="185f6-176">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="185f6-177">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="185f6-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="185f6-178">Следуйте инструкциям в [этой статье](/dotnet/core/tutorials/using-on-mac-vs-full-solution), чтобы создать консольное приложение с именем *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="185f6-178">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="185f6-179">Добавление необходимых пакетов</span><span class="sxs-lookup"><span data-stu-id="185f6-179">Add required packages</span></span>

<span data-ttu-id="185f6-180">Для клиентского проекта gRPC требуются следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="185f6-180">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="185f6-181">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), который содержит клиент .NET Core.</span><span class="sxs-lookup"><span data-stu-id="185f6-181">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="185f6-182">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;</span><span class="sxs-lookup"><span data-stu-id="185f6-182">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="185f6-183">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="185f6-183">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="185f6-184">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="185f6-184">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="185f6-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="185f6-185">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="185f6-186">Установка пакетов с помощью консоли диспетчера пакетов (PMC) или управления пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="185f6-186">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="185f6-187">Установка пакетов с помощью консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="185f6-187">PMC option to install packages</span></span>

* <span data-ttu-id="185f6-188">В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="185f6-188">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="185f6-189">В **консоли диспетчера пакетов** перейдите в каталог, в котором содержится файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="185f6-189">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="185f6-190">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="185f6-190">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="185f6-191">Установка пакетов с помощью раздела управления пакетами NuGet</span><span class="sxs-lookup"><span data-stu-id="185f6-191">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="185f6-192">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="185f6-192">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="185f6-193">Выберите вкладку **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="185f6-193">Select the **Browse** tab.</span></span>
* <span data-ttu-id="185f6-194">В поле поиска введите **Grpc.Net.Client**.</span><span class="sxs-lookup"><span data-stu-id="185f6-194">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="185f6-195">Выберите пакет **Grpc.Net.Client** на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="185f6-195">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="185f6-196">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="185f6-196">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="185f6-197">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="185f6-197">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="185f6-198">Во **встроенном терминале** выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="185f6-198">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="185f6-199">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="185f6-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="185f6-200">Щелкните правой кнопкой мыши папку **Пакеты** на **панели решения** > **Добавление пакетов**</span><span class="sxs-lookup"><span data-stu-id="185f6-200">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="185f6-201">В поле поиска введите **Grpc.Net.Client**.</span><span class="sxs-lookup"><span data-stu-id="185f6-201">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="185f6-202">В области результатов выберите пакет **Grpc.Net.Client** и нажмите кнопку **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="185f6-202">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="185f6-203">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="185f6-203">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="185f6-204">Добавление greet.proto</span><span class="sxs-lookup"><span data-stu-id="185f6-204">Add greet.proto</span></span>

* <span data-ttu-id="185f6-205">Создайте папку **Protos** в клиентском проекте gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-205">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="185f6-206">Скопируйте файл **Protos\greet.proto** из службы Greeter gRPC в проект клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-206">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="185f6-207">Измените файл проекта *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="185f6-207">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="185f6-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="185f6-208">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="185f6-209">Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.</span><span class="sxs-lookup"><span data-stu-id="185f6-209">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="185f6-210">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="185f6-210">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="185f6-211">Выберите файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="185f6-211">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="185f6-212">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="185f6-212">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="185f6-213">Щелкните проект правой кнопкой мыши и выберите **Сервис > Изменить файл**.</span><span class="sxs-lookup"><span data-stu-id="185f6-213">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="185f6-214">Добавьте группу элементов с элементом `<Protobuf>`, ссылающимся на файл **greet.proto**:</span><span class="sxs-lookup"><span data-stu-id="185f6-214">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="185f6-215">Создание клиента Greeter</span><span class="sxs-lookup"><span data-stu-id="185f6-215">Create the Greeter client</span></span>

<span data-ttu-id="185f6-216">Скомпилируйте проект, чтобы создать типы в пространстве имен `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="185f6-216">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="185f6-217">Типы `GrpcGreeter` создаются автоматически в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="185f6-217">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="185f6-218">Добавьте в файл *Program.cs* клиента gRPC следующий код:</span><span class="sxs-lookup"><span data-stu-id="185f6-218">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="185f6-219">файл *Program.cs* содержит точку входа и логику для клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-219">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="185f6-220">Клиент Greeter создается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="185f6-220">The Greeter client is created by:</span></span>

* <span data-ttu-id="185f6-221">Создание экземпляра `HttpClient` со сведениями для создания подключения к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="185f6-221">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="185f6-222">Использование `HttpClient` для создания клиента Greeter:</span><span class="sxs-lookup"><span data-stu-id="185f6-222">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="185f6-223">Клиент Greeter вызывает асинхронный метод `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="185f6-223">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="185f6-224">Отображается результат вызова `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="185f6-224">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="185f6-225">Тестирование клиента gRPC с помощью службы Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="185f6-225">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="185f6-226">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="185f6-226">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="185f6-227">В службе Greeter нажмите `Ctrl+F5` для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="185f6-227">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="185f6-228">В проекте `GrpcGreeterClient` нажмите `Ctrl+F5` для запуска клиента без отладчика.</span><span class="sxs-lookup"><span data-stu-id="185f6-228">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="185f6-229">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="185f6-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="185f6-230">Запустите службу Greeter.</span><span class="sxs-lookup"><span data-stu-id="185f6-230">Start the Greeter service.</span></span>
* <span data-ttu-id="185f6-231">Запустите клиент.</span><span class="sxs-lookup"><span data-stu-id="185f6-231">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="185f6-232">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="185f6-232">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="185f6-233">Служба отправляет сообщение "Hello GreeterClient" в качестве ответа.</span><span class="sxs-lookup"><span data-stu-id="185f6-233">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="185f6-234">Ответ "Hello GreeterClient" отображается в командной строке:</span><span class="sxs-lookup"><span data-stu-id="185f6-234">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="185f6-235">Служба gRPC записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="185f6-235">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a><span data-ttu-id="185f6-236">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="185f6-236">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
