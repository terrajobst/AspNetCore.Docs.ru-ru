---
title: Создание клиента и сервера gRPC .NET Core в ASP.NET Core
author: juntaoluo
description: В этом учебнике объясняется, как создать службу и клиента gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 3e90e3b17186757fe157fb6641888786bb7a0df2
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412520"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="71d15-104">Учебник. Создание клиента и сервера gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71d15-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="71d15-105">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="71d15-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="71d15-106">В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core и сервер gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="71d15-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="71d15-107">В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="71d15-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="71d15-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="71d15-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="71d15-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="71d15-110">Создание сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="71d15-111">Создание клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="71d15-112">Тестирование службы клиента gRPC с помощью службы Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71d15-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="71d15-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71d15-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71d15-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71d15-115">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="71d15-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71d15-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="71d15-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="71d15-117">Создание службы gRPC</span><span class="sxs-lookup"><span data-stu-id="71d15-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71d15-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71d15-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71d15-119">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="71d15-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="71d15-120">В диалоговом окне **Создать проект** выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="71d15-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="71d15-121">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="71d15-121">Select **Next**</span></span>
* <span data-ttu-id="71d15-122">Присвойте проекту имя **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="71d15-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="71d15-123">Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.</span><span class="sxs-lookup"><span data-stu-id="71d15-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="71d15-124">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="71d15-124">Select **Create**</span></span>
* <span data-ttu-id="71d15-125">в диалоговом окне **Создание веб-приложения ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="71d15-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="71d15-126">В раскрывающемся меню выберите **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="71d15-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="71d15-127">Выберите шаблон **gRPC Service**.</span><span class="sxs-lookup"><span data-stu-id="71d15-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="71d15-128">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="71d15-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71d15-129">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="71d15-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="71d15-130">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="71d15-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="71d15-131">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="71d15-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="71d15-132">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="71d15-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="71d15-133">С помощью команды `dotnet new` в папке *GrpcGreeter* создается служба gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="71d15-134">С помощью команды `code` папка *GrpcGreeter* открывается в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="71d15-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="71d15-135">Появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'GrpcGreeter'. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="71d15-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="71d15-136">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="71d15-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71d15-137">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="71d15-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71d15-138">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="71d15-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="71d15-139">Эти команды позволяют создать службу gRPC с помощью [.NET Core CLI](/dotnet/core/tools/dotnet).</span><span class="sxs-lookup"><span data-stu-id="71d15-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="71d15-140">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="71d15-140">Open the project</span></span>

<span data-ttu-id="71d15-141">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="71d15-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="71d15-142">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="71d15-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71d15-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71d15-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71d15-144">Нажмите `Ctrl+F5`, чтобы запустить службу gRPC без отладчика.</span><span class="sxs-lookup"><span data-stu-id="71d15-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="71d15-145">Служба запустится в командной строке Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71d15-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="71d15-146">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="71d15-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="71d15-147">Выполните команду `dotnet run` в командной строке, чтобы запустить проект gRPC Greeter с именем *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="71d15-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="71d15-148">В журналах отобразится служба, которая ожидает передачи данных через `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="71d15-148">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="71d15-149">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="71d15-149">Examine the project files</span></span>

<span data-ttu-id="71d15-150">Файлы проекта *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="71d15-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="71d15-151">*greet.proto*: Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="71d15-152">Дополнительные сведения см. в разделе [Введение в gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="71d15-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="71d15-153">Папка *Services* содержит реализацию службы `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="71d15-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="71d15-154">*appSettings.json*: содержит данные конфигурации, такие как протокол, используемый в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="71d15-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="71d15-155">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="71d15-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="71d15-156">*Program.cs*: содержит точку входа для службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="71d15-157">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="71d15-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="71d15-158">*Startup.cs*: содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="71d15-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="71d15-159">Дополнительные сведения: [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="71d15-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="71d15-160">Создание клиента gRPC в консольном приложении .NET</span><span class="sxs-lookup"><span data-stu-id="71d15-160">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71d15-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71d15-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71d15-162">Откройте еще один экземпляр Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71d15-162">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="71d15-163">Выберите **Файл** > **Создать** > **Проект** в меню.</span><span class="sxs-lookup"><span data-stu-id="71d15-163">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="71d15-164">В диалоговом окне **Создать проект** выберите **Консольное приложение (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="71d15-164">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="71d15-165">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="71d15-165">Select **Next**</span></span>
* <span data-ttu-id="71d15-166">В текстовом поле **Имя** введите "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="71d15-166">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="71d15-167">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="71d15-167">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71d15-168">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="71d15-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="71d15-169">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="71d15-169">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="71d15-170">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="71d15-170">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="71d15-171">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="71d15-171">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71d15-172">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="71d15-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71d15-173">Следуйте инструкциям в [этой статье](/dotnet/core/tutorials/using-on-mac-vs-full-solution), чтобы создать консольное приложение с именем *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="71d15-173">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="71d15-174">Добавление необходимых пакетов</span><span class="sxs-lookup"><span data-stu-id="71d15-174">Add required packages</span></span>

<span data-ttu-id="71d15-175">Для клиентского проекта gRPC требуются следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="71d15-175">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="71d15-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), который содержит клиент .NET Core.</span><span class="sxs-lookup"><span data-stu-id="71d15-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="71d15-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;</span><span class="sxs-lookup"><span data-stu-id="71d15-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="71d15-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="71d15-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="71d15-179">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="71d15-179">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71d15-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71d15-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="71d15-181">Установка пакетов с помощью консоли диспетчера пакетов (PMC) или управления пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="71d15-181">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="71d15-182">Установка пакетов с помощью консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="71d15-182">PMC option to install packages</span></span>

* <span data-ttu-id="71d15-183">В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="71d15-183">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="71d15-184">В **консоли диспетчера пакетов** перейдите в каталог, в котором содержится файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="71d15-184">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="71d15-185">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="71d15-185">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="71d15-186">Установка пакетов с помощью раздела управления пакетами NuGet</span><span class="sxs-lookup"><span data-stu-id="71d15-186">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="71d15-187">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="71d15-187">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="71d15-188">Выберите вкладку **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="71d15-188">Select the **Browse** tab.</span></span>
* <span data-ttu-id="71d15-189">В поле поиска введите **Grpc.Core**.</span><span class="sxs-lookup"><span data-stu-id="71d15-189">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="71d15-190">Выберите пакет **Grpc.Core** на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="71d15-190">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="71d15-191">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="71d15-191">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71d15-192">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="71d15-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="71d15-193">Во **встроенном терминале** выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="71d15-193">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71d15-194">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="71d15-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="71d15-195">Щелкните правой кнопкой мыши папку **Пакеты** на **панели решения** > **Добавление пакетов**</span><span class="sxs-lookup"><span data-stu-id="71d15-195">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="71d15-196">В поле поиска введите **Grpc.Net.Client**.</span><span class="sxs-lookup"><span data-stu-id="71d15-196">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="71d15-197">В области результатов выберите пакет **Grpc.Net.Client** и нажмите кнопку **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="71d15-197">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="71d15-198">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="71d15-198">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="71d15-199">Добавление greet.proto</span><span class="sxs-lookup"><span data-stu-id="71d15-199">Add greet.proto</span></span>

* <span data-ttu-id="71d15-200">Создайте папку **Protos** в клиентском проекте gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-200">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="71d15-201">Скопируйте файл **Protos\greet.proto** из службы Greeter gRPC в проект клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-201">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="71d15-202">Измените файл проекта *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="71d15-202">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71d15-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71d15-203">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="71d15-204">Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.</span><span class="sxs-lookup"><span data-stu-id="71d15-204">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71d15-205">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="71d15-205">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="71d15-206">Выберите файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="71d15-206">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71d15-207">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="71d15-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="71d15-208">Щелкните проект правой кнопкой мыши и выберите **Сервис > Изменить файл**.</span><span class="sxs-lookup"><span data-stu-id="71d15-208">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="71d15-209">Добавьте группу элементов с элементом `<Protobuf>`, ссылающимся на файл **greet.proto**:</span><span class="sxs-lookup"><span data-stu-id="71d15-209">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="71d15-210">Создание клиента Greeter</span><span class="sxs-lookup"><span data-stu-id="71d15-210">Create the Greeter client</span></span>

<span data-ttu-id="71d15-211">Скомпилируйте проект, чтобы создать типы в пространстве имен `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="71d15-211">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="71d15-212">Типы `GrpcGreeter` создаются автоматически в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="71d15-212">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="71d15-213">Добавьте в файл *Program.cs* клиента gRPC следующий код:</span><span class="sxs-lookup"><span data-stu-id="71d15-213">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="71d15-214">файл *Program.cs* содержит точку входа и логику для клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-214">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="71d15-215">Клиент Greeter создается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="71d15-215">The Greeter client is created by:</span></span>

* <span data-ttu-id="71d15-216">Создание экземпляра `HttpClient` со сведениями для создания подключения к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="71d15-216">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="71d15-217">Использование `HttpClient` для создания клиента Greeter:</span><span class="sxs-lookup"><span data-stu-id="71d15-217">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="71d15-218">Клиент Greeter вызывает асинхронный метод `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="71d15-218">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="71d15-219">Отображается результат вызова `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="71d15-219">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="71d15-220">Тестирование клиента gRPC с помощью службы Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="71d15-220">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71d15-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71d15-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71d15-222">В службе Greeter нажмите `Ctrl+F5` для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="71d15-222">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="71d15-223">В проекте `GrpcGreeterClient` нажмите `Ctrl+F5` для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="71d15-223">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="71d15-224">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="71d15-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="71d15-225">Запустите службу Greeter.</span><span class="sxs-lookup"><span data-stu-id="71d15-225">Start the Greeter service.</span></span>
* <span data-ttu-id="71d15-226">Запустите клиент.</span><span class="sxs-lookup"><span data-stu-id="71d15-226">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="71d15-227">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="71d15-227">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="71d15-228">Служба отправляет сообщение "Hello GreeterClient" в качестве ответа.</span><span class="sxs-lookup"><span data-stu-id="71d15-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="71d15-229">Ответ "Hello GreeterClient" отображается в командной строке:</span><span class="sxs-lookup"><span data-stu-id="71d15-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="71d15-230">Служба gRPC записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="71d15-230">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="71d15-231">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="71d15-231">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
