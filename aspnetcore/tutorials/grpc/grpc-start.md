---
title: Создание клиента и сервера gRPC .NET Core в ASP.NET Core
author: juntaoluo
description: В этом учебнике объясняется, как создать службу и клиента gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 8/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 9eeb71ca751005780560f0f2200edc2013541c34
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925229"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="f4854-104">Учебник. Создание клиента и сервера gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4854-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="f4854-105">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="f4854-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="f4854-106">В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core и сервер gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4854-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="f4854-107">В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="f4854-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f4854-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f4854-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="f4854-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f4854-110">Создание сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="f4854-111">Создание клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="f4854-112">Тестирование службы клиента gRPC с помощью службы Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4854-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f4854-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4854-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4854-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4854-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4854-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4854-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4854-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="f4854-117">Создание службы gRPC</span><span class="sxs-lookup"><span data-stu-id="f4854-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4854-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4854-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f4854-119">Запустите Visual Studio и щелкните **Создать проект**</span><span class="sxs-lookup"><span data-stu-id="f4854-119">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="f4854-120">или в меню **Файл** в Visual Studio выберите **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="f4854-120">Alternatively, from the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f4854-121">В диалоговом окне **Создание проекта** выберите **Служба gRPC** и нажмите кнопку **Далее**:</span><span class="sxs-lookup"><span data-stu-id="f4854-121">In the **Create a new project** dialog, select **gRPC Service** and select **Next**:</span></span>

  ![Диалоговое окно создания проекта](~/tutorials/grpc/grpc-start/static/cnp.png)

* <span data-ttu-id="f4854-123">Присвойте проекту имя **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="f4854-123">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="f4854-124">Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.</span><span class="sxs-lookup"><span data-stu-id="f4854-124">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="f4854-125">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="f4854-125">Select **Create**.</span></span>
* <span data-ttu-id="f4854-126">В диалоговом окне **Создать службу gRPC** сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="f4854-126">In the **Create a new gRPC service** dialog:</span></span>
  * <span data-ttu-id="f4854-127">Шаблон **gRPC Service** выбран.</span><span class="sxs-lookup"><span data-stu-id="f4854-127">The **gRPC Service** template is selected.</span></span>
  * <span data-ttu-id="f4854-128">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="f4854-128">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4854-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4854-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f4854-130">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f4854-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f4854-131">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="f4854-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="f4854-132">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="f4854-132">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="f4854-133">С помощью команды `dotnet new` в папке *GrpcGreeter* создается служба gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="f4854-134">С помощью команды `code` папка *GrpcGreeter* открывается в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f4854-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="f4854-135">Появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'GrpcGreeter'. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="f4854-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="f4854-136">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="f4854-136">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4854-137">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4854-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f4854-138">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="f4854-138">From a terminal, run the following commands:</span></span>

```dotnetcli
dotnet new grpc -o GrpcGreeter
cd GrpcGreeter
```

<span data-ttu-id="f4854-139">Эти команды позволяют создать службу gRPC с помощью [.NET Core CLI](/dotnet/core/tools/dotnet).</span><span class="sxs-lookup"><span data-stu-id="f4854-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="f4854-140">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="f4854-140">Open the project</span></span>

<span data-ttu-id="f4854-141">В Visual Studio выберите **Файл** > **Открыть** и щелкните файл *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="f4854-141">From Visual Studio, select **File** > **Open**, and then select the *GrpcGreeter.sln* file.</span></span>

---

### <a name="run-the-service"></a><span data-ttu-id="f4854-142">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="f4854-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4854-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4854-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f4854-144">Нажмите `Ctrl+F5`, чтобы запустить службу gRPC без отладчика.</span><span class="sxs-lookup"><span data-stu-id="f4854-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="f4854-145">Служба запустится в командной строке Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4854-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4854-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4854-146">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f4854-147">Выполните команду `dotnet run` в командной строке, чтобы запустить проект gRPC Greeter с именем *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="f4854-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4854-148">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4854-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f4854-149">Выполните команду `dotnet run` в командной строке, чтобы запустить проект gRPC Greeter с именем *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="f4854-149">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

---

<span data-ttu-id="f4854-150">В журналах отобразится служба, которая ожидает передачи данных через `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f4854-150">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="f4854-151">Шаблон gRPC настроен для использования [протокола TLS](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="f4854-151">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="f4854-152">Клиенты gRPC должны использовать протокол HTTPS для обращения к серверу.</span><span class="sxs-lookup"><span data-stu-id="f4854-152">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="f4854-153">macOS не поддерживает ASP.NET Core gRPC с TLS.</span><span class="sxs-lookup"><span data-stu-id="f4854-153">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="f4854-154">Для успешного запуска служб gRPC в macOS требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="f4854-154">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="f4854-155">Дополнительные сведения см. в статье [Не удается запустить приложение ASP.NET Core gRPC в macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="f4854-155">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="f4854-156">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="f4854-156">Examine the project files</span></span>

<span data-ttu-id="f4854-157">Файлы проекта *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="f4854-157">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="f4854-158">*greet.proto*. Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-158">*greet.proto* &ndash; The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="f4854-159">Дополнительные сведения см. в разделе [Введение в gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="f4854-159">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="f4854-160">Папка *Services* содержит реализацию службы `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="f4854-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="f4854-161">*appSettings.json* содержит данные конфигурации, включая протокол, используемый в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f4854-161">*appSettings.json* &ndash; Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="f4854-162">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="f4854-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="f4854-163">*Program.cs* содержит точку входа для службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-163">*Program.cs* &ndash; Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="f4854-164">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="f4854-164">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="f4854-165">*Startup.cs* содержит код, который определяет поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="f4854-165">*Startup.cs* &ndash; Contains code that configures app behavior.</span></span> <span data-ttu-id="f4854-166">Дополнительные сведения: [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="f4854-166">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="f4854-167">Создание клиента gRPC в консольном приложении .NET</span><span class="sxs-lookup"><span data-stu-id="f4854-167">Create the gRPC client in a .NET console app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4854-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4854-168">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f4854-169">Откройте второй экземпляр Visual Studio и щелкните **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="f4854-169">Open a second instance of Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="f4854-170">В диалоговом окне **Создание проекта** выберите **Консольное приложение (.NET Core)** и щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="f4854-170">In the **Create a new project** dialog, select **Console App (.NET Core)** and select **Next**.</span></span>
* <span data-ttu-id="f4854-171">В текстовое поле **Имя** введите **GrpcGreeterClient** и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="f4854-171">In the **Name** text box, enter **GrpcGreeterClient** and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4854-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4854-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f4854-173">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f4854-173">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f4854-174">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="f4854-174">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="f4854-175">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="f4854-175">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4854-176">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4854-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f4854-177">Чтобы создать консольное приложение с именем *GrpcGreeterClient*, см. руководство по [созданию полного решения .NET Core в macOS с помощью Visual Studio для Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution).</span><span class="sxs-lookup"><span data-stu-id="f4854-177">Follow the instructions in [Building a complete .NET Core solution on macOS using Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

---

### <a name="add-required-packages"></a><span data-ttu-id="f4854-178">Добавление необходимых пакетов</span><span class="sxs-lookup"><span data-stu-id="f4854-178">Add required packages</span></span>

<span data-ttu-id="f4854-179">Для клиентского проекта gRPC требуются следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="f4854-179">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="f4854-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), который содержит клиент .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4854-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="f4854-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;</span><span class="sxs-lookup"><span data-stu-id="f4854-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="f4854-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="f4854-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="f4854-183">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="f4854-183">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4854-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4854-184">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f4854-185">Установка пакетов с помощью консоли диспетчера пакетов (PMC) или управления пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="f4854-185">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="f4854-186">Установка пакетов с помощью консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="f4854-186">PMC option to install packages</span></span>

* <span data-ttu-id="f4854-187">В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="f4854-187">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="f4854-188">В **консоли диспетчера пакетов** выполните команду `cd GrpcGreeterClient`, чтобы перейти в папку с файлами *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f4854-188">From the **Package Manager Console** window, run `cd GrpcGreeterClient` to change directories to the folder containing the *GrpcGreeterClient.csproj* files.</span></span>
* <span data-ttu-id="f4854-189">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="f4854-189">Run the following commands:</span></span>

  ```powershell
  Install-Package Grpc.Net.Client
  Install-Package Google.Protobuf
  Install-Package Grpc.Tools
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="f4854-190">Установка пакетов с помощью раздела управления пакетами NuGet</span><span class="sxs-lookup"><span data-stu-id="f4854-190">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="f4854-191">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="f4854-191">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="f4854-192">Выберите вкладку **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="f4854-192">Select the **Browse** tab.</span></span>
* <span data-ttu-id="f4854-193">В поле поиска введите **Grpc.Net.Client**.</span><span class="sxs-lookup"><span data-stu-id="f4854-193">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="f4854-194">Выберите пакет **Grpc.Net.Client** на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="f4854-194">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="f4854-195">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="f4854-195">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4854-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4854-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f4854-197">Во **встроенном терминале** выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="f4854-197">Run the following commands from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4854-198">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4854-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f4854-199">Щелкните правой кнопкой мыши папку **Пакеты** на **панели решения** > **Добавление пакетов**</span><span class="sxs-lookup"><span data-stu-id="f4854-199">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="f4854-200">В поле поиска введите **Grpc.Net.Client**.</span><span class="sxs-lookup"><span data-stu-id="f4854-200">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="f4854-201">В области результатов выберите пакет **Grpc.Net.Client** и нажмите кнопку **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="f4854-201">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="f4854-202">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="f4854-202">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="f4854-203">Добавление greet.proto</span><span class="sxs-lookup"><span data-stu-id="f4854-203">Add greet.proto</span></span>

* <span data-ttu-id="f4854-204">Создайте папку *Protos* в клиентском проекте gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-204">Create a *Protos* folder in the gRPC client project.</span></span>
* <span data-ttu-id="f4854-205">Скопируйте файл *Protos\greet.proto* из службы Greeter gRPC в проект клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-205">Copy the *Protos\greet.proto* file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="f4854-206">Измените файл проекта *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="f4854-206">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4854-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4854-207">Visual Studio</span></span>](#tab/visual-studio)

  <span data-ttu-id="f4854-208">Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.</span><span class="sxs-lookup"><span data-stu-id="f4854-208">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4854-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4854-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

  <span data-ttu-id="f4854-210">Выберите файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f4854-210">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4854-211">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4854-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="f4854-212">Щелкните проект правой кнопкой мыши и выберите **Сервис** > **Изменить файл**.</span><span class="sxs-lookup"><span data-stu-id="f4854-212">Right-click the project and select **Tools** > **Edit File**.</span></span>

  ---

* <span data-ttu-id="f4854-213">Добавьте группу элементов с элементом `<Protobuf>`, ссылающимся на файл *greet.proto*:</span><span class="sxs-lookup"><span data-stu-id="f4854-213">Add an item group with a `<Protobuf>` element that refers to the *greet.proto* file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="f4854-214">Создание клиента Greeter</span><span class="sxs-lookup"><span data-stu-id="f4854-214">Create the Greeter client</span></span>

<span data-ttu-id="f4854-215">Скомпилируйте проект, чтобы создать типы в пространстве имен `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="f4854-215">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="f4854-216">Типы `GrpcGreeter` создаются автоматически в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="f4854-216">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="f4854-217">Добавьте в файл *Program.cs* клиента gRPC следующий код:</span><span class="sxs-lookup"><span data-stu-id="f4854-217">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="f4854-218">файл *Program.cs* содержит точку входа и логику для клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-218">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="f4854-219">Клиент Greeter создается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f4854-219">The Greeter client is created by:</span></span>

* <span data-ttu-id="f4854-220">Создание экземпляра `HttpClient` со сведениями для создания подключения к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-220">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="f4854-221">Использование `HttpClient` для создания канала gRPC и клиента Greeter:</span><span class="sxs-lookup"><span data-stu-id="f4854-221">Using the `HttpClient` to construct a gRPC channel and the Greeter client:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-5)]

<span data-ttu-id="f4854-222">Клиент Greeter вызывает асинхронный метод `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="f4854-222">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="f4854-223">Отображается результат вызова `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="f4854-223">The result of the `SayHello` call is displayed:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=6-8)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="f4854-224">Тестирование клиента gRPC с помощью службы Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="f4854-224">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4854-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4854-225">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f4854-226">В службе Greeter нажмите `Ctrl+F5` для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="f4854-226">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="f4854-227">В проекте `GrpcGreeterClient` нажмите `Ctrl+F5` для запуска клиента без отладчика.</span><span class="sxs-lookup"><span data-stu-id="f4854-227">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4854-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4854-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f4854-229">Запустите службу Greeter.</span><span class="sxs-lookup"><span data-stu-id="f4854-229">Start the Greeter service.</span></span>
* <span data-ttu-id="f4854-230">Запустите клиент.</span><span class="sxs-lookup"><span data-stu-id="f4854-230">Start the client.</span></span>


# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4854-231">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4854-231">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f4854-232">Запустите службу Greeter.</span><span class="sxs-lookup"><span data-stu-id="f4854-232">Start the Greeter service.</span></span>
* <span data-ttu-id="f4854-233">Запустите клиент.</span><span class="sxs-lookup"><span data-stu-id="f4854-233">Start the client.</span></span>

---

<span data-ttu-id="f4854-234">Клиент отправляет приветствие в службу с сообщением, в котором содержится его имя *GreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="f4854-234">The client sends a greeting to the service with a message containing its name, *GreeterClient*.</span></span> <span data-ttu-id="f4854-235">Служба отправляет сообщение "Hello GreeterClient" в качестве ответа.</span><span class="sxs-lookup"><span data-stu-id="f4854-235">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="f4854-236">Ответ "Hello GreeterClient" отображается в командной строке:</span><span class="sxs-lookup"><span data-stu-id="f4854-236">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="f4854-237">Служба gRPC записывает сведения об успешном вызове в журналы, что отображается в командной строке:</span><span class="sxs-lookup"><span data-stu-id="f4854-237">The gRPC service records the details of the successful call in the logs written to the command prompt:</span></span>

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

> [!NOTE]
> <span data-ttu-id="f4854-238">Чтобы применить код, описанный в этой статье, требуется сертификат разработки HTTPS ASP.NET Core для защиты службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="f4854-238">The code in this article requires the ASP.NET Core HTTPS development certificate to secure the gRPC service.</span></span> <span data-ttu-id="f4854-239">Если клиент возвращает сообщение `The remote certificate is invalid according to the validation procedure.`, это значит, что сертификат разработки не является доверенным.</span><span class="sxs-lookup"><span data-stu-id="f4854-239">If the client fails with the message `The remote certificate is invalid according to the validation procedure.`, the development certificate is not trusted.</span></span> <span data-ttu-id="f4854-240">См. сведения об устранении этой проблемы в руководстве по [настройке доверия к сертификату разработки HTTPS ASP.NET Core в Windows и macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="f4854-240">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

### <a name="next-steps"></a><span data-ttu-id="f4854-241">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="f4854-241">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
