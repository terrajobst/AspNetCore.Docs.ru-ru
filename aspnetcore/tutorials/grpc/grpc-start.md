---
title: Создание клиента и сервера gRPC .NET Core в ASP.NET Core
author: juntaoluo
description: В этом учебнике объясняется, как создать службу и клиента gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 8507c3dcefeb61a4dd34a1ff967c082d287cf646
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555895"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="4f5ee-104">Учебник. Создание клиента и сервера gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f5ee-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="4f5ee-105">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="4f5ee-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="4f5ee-106">В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core и сервер gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="4f5ee-107">В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="4f5ee-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4f5ee-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="4f5ee-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4f5ee-110">Создание сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="4f5ee-111">Создание клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="4f5ee-112">Тестирование службы клиента gRPC с помощью службы Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f5ee-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4f5ee-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5ee-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5ee-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f5ee-115">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f5ee-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4f5ee-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="4f5ee-117">Создание службы gRPC</span><span class="sxs-lookup"><span data-stu-id="4f5ee-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5ee-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5ee-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f5ee-119">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4f5ee-120">В диалоговом окне **Создать проект** выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4f5ee-121">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="4f5ee-121">Select **Next**</span></span>
* <span data-ttu-id="4f5ee-122">Присвойте проекту имя **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="4f5ee-123">Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="4f5ee-124">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-124">Select **Create**</span></span>
* <span data-ttu-id="4f5ee-125">в диалоговом окне **Создание веб-приложения ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="4f5ee-126">В раскрывающемся меню выберите **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="4f5ee-127">Выберите шаблон **gRPC Service**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="4f5ee-128">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f5ee-129">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4f5ee-130">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="4f5ee-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4f5ee-131">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="4f5ee-132">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="4f5ee-133">С помощью команды `dotnet new` в папке *GrpcGreeter* создается служба gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="4f5ee-134">С помощью команды `code` папка *GrpcGreeter* открывается в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="4f5ee-135">Появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'GrpcGreeter'. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="4f5ee-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="4f5ee-136">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f5ee-137">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4f5ee-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4f5ee-138">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="4f5ee-139">Эти команды позволяют создать службу gRPC с помощью [.NET Core CLI](/dotnet/core/tools/dotnet).</span><span class="sxs-lookup"><span data-stu-id="4f5ee-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="4f5ee-140">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="4f5ee-140">Open the project</span></span>

<span data-ttu-id="4f5ee-141">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="4f5ee-142">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="4f5ee-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5ee-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5ee-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f5ee-144">Нажмите `Ctrl+F5`, чтобы запустить службу gRPC без отладчика.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="4f5ee-145">Служба запустится в командной строке Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="4f5ee-146">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4f5ee-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="4f5ee-147">Выполните команду `dotnet run` в командной строке, чтобы запустить проект gRPC Greeter с именем *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="4f5ee-148">В журналах отобразится служба, которая ожидает передачи данных через `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-148">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="4f5ee-149">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="4f5ee-149">Examine the project files</span></span>

<span data-ttu-id="4f5ee-150">Файлы проекта *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="4f5ee-151">*greet.proto*: Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="4f5ee-152">Дополнительные сведения см. в разделе [Введение в gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="4f5ee-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="4f5ee-153">Папка *Services* содержит реализацию службы `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="4f5ee-154">*appSettings.json*: содержит данные конфигурации, такие как протокол, используемый в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="4f5ee-155">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="4f5ee-156">*Program.cs*: содержит точку входа для службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="4f5ee-157">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="4f5ee-158">*Startup.cs*: содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="4f5ee-159">Дополнительные сведения: [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="4f5ee-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="4f5ee-160">Создание клиента gRPC в консольном приложении .NET</span><span class="sxs-lookup"><span data-stu-id="4f5ee-160">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5ee-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5ee-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f5ee-162">Откройте еще один экземпляр Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-162">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="4f5ee-163">Выберите **Файл** > **Создать** > **Проект** в меню.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-163">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="4f5ee-164">В диалоговом окне **Создать проект** выберите **Консольное приложение (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="4f5ee-164">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="4f5ee-165">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="4f5ee-165">Select **Next**</span></span>
* <span data-ttu-id="4f5ee-166">В текстовом поле **Имя** введите "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="4f5ee-166">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="4f5ee-167">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-167">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f5ee-168">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4f5ee-169">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="4f5ee-169">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4f5ee-170">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-170">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="4f5ee-171">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-171">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f5ee-172">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4f5ee-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4f5ee-173">Следуйте инструкциям в [этой статье](/dotnet/core/tutorials/using-on-mac-vs-full-solution), чтобы создать консольное приложение с именем *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-173">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="4f5ee-174">Добавление необходимых пакетов</span><span class="sxs-lookup"><span data-stu-id="4f5ee-174">Add required packages</span></span>

<span data-ttu-id="4f5ee-175">Для клиентского проекта gRPC требуются следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-175">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="4f5ee-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), который содержит клиент .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="4f5ee-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;</span><span class="sxs-lookup"><span data-stu-id="4f5ee-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="4f5ee-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="4f5ee-179">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-179">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5ee-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5ee-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4f5ee-181">Установка пакетов с помощью консоли диспетчера пакетов (PMC) или управления пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-181">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="4f5ee-182">Установка пакетов с помощью консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="4f5ee-182">PMC option to install packages</span></span>

* <span data-ttu-id="4f5ee-183">В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-183">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="4f5ee-184">В **консоли диспетчера пакетов** перейдите в каталог, в котором содержится файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-184">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="4f5ee-185">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-185">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="4f5ee-186">Установка пакетов с помощью раздела управления пакетами NuGet</span><span class="sxs-lookup"><span data-stu-id="4f5ee-186">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="4f5ee-187">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="4f5ee-187">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="4f5ee-188">Выберите вкладку **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-188">Select the **Browse** tab.</span></span>
* <span data-ttu-id="4f5ee-189">В поле поиска введите **Grpc.Core**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-189">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="4f5ee-190">Выберите пакет **Grpc.Core** на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-190">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="4f5ee-191">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-191">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f5ee-192">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4f5ee-193">Во **встроенном терминале** выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-193">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f5ee-194">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4f5ee-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4f5ee-195">Щелкните правой кнопкой мыши папку **Пакеты** на **панели решения** > **Добавление пакетов**</span><span class="sxs-lookup"><span data-stu-id="4f5ee-195">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="4f5ee-196">В поле поиска введите **Grpc.Net.Client**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-196">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="4f5ee-197">В области результатов выберите пакет **Grpc.Net.Client** и нажмите кнопку **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="4f5ee-197">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="4f5ee-198">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-198">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="4f5ee-199">Добавление greet.proto</span><span class="sxs-lookup"><span data-stu-id="4f5ee-199">Add greet.proto</span></span>

* <span data-ttu-id="4f5ee-200">Создайте папку **Protos** в клиентском проекте gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-200">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="4f5ee-201">Скопируйте файл **Protos\greet.proto** из службы Greeter gRPC в проект клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-201">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="4f5ee-202">Измените файл проекта *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-202">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5ee-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5ee-203">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="4f5ee-204">Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-204">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f5ee-205">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-205">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="4f5ee-206">Выберите файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-206">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f5ee-207">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4f5ee-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="4f5ee-208">Щелкните проект правой кнопкой мыши и выберите **Сервис > Изменить файл**.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-208">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="4f5ee-209">Добавьте группу элементов с элементом `<Protobuf>`, ссылающимся на файл **greet.proto**:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-209">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="4f5ee-210">Создание клиента Greeter</span><span class="sxs-lookup"><span data-stu-id="4f5ee-210">Create the Greeter client</span></span>

<span data-ttu-id="4f5ee-211">Скомпилируйте проект, чтобы создать типы в пространстве имен `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-211">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="4f5ee-212">Типы `GrpcGreeter` создаются автоматически в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-212">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="4f5ee-213">Добавьте в файл *Program.cs* клиента gRPC следующий код:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-213">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="4f5ee-214">файл *Program.cs* содержит точку входа и логику для клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-214">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="4f5ee-215">Клиент Greeter создается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-215">The Greeter client is created by:</span></span>

* <span data-ttu-id="4f5ee-216">Создание экземпляра `HttpClient` со сведениями для создания подключения к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-216">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="4f5ee-217">Использование `HttpClient` для создания клиента Greeter:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-217">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-9)]

<span data-ttu-id="4f5ee-218">Клиент Greeter вызывает асинхронный метод `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-218">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="4f5ee-219">Отображается результат вызова `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-219">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=10-12)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="4f5ee-220">Тестирование клиента gRPC с помощью службы Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="4f5ee-220">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5ee-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5ee-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f5ee-222">В службе Greeter нажмите `Ctrl+F5` для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-222">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="4f5ee-223">В проекте `GrpcGreeterClient` нажмите `Ctrl+F5` для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-223">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="4f5ee-224">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4f5ee-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="4f5ee-225">Запустите службу Greeter.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-225">Start the Greeter service.</span></span>
* <span data-ttu-id="4f5ee-226">Запустите клиент.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-226">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="4f5ee-227">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="4f5ee-227">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="4f5ee-228">Служба отправляет сообщение "Hello GreeterClient" в качестве ответа.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="4f5ee-229">Ответ "Hello GreeterClient" отображается в командной строке:</span><span class="sxs-lookup"><span data-stu-id="4f5ee-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="4f5ee-230">Служба gRPC записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="4f5ee-230">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a><span data-ttu-id="4f5ee-231">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="4f5ee-231">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
