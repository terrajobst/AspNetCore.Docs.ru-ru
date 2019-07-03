---
title: Создание клиента и сервера gRPC .NET Core в ASP.NET Core
author: juntaoluo
description: В этом учебнике объясняется, как создать службу и клиента gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 6aef56ecd61ad71e166c03c12b28b25b931cdd88
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152923"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="a963b-104">Учебник. Создание клиента и сервера gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a963b-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="a963b-105">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="a963b-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="a963b-106">В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core и сервер gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a963b-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="a963b-107">В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="a963b-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a963b-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a963b-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="a963b-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a963b-110">Создание сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="a963b-111">Создание клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="a963b-112">Тестирование службы клиента gRPC с помощью службы Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="a963b-113">Создание службы gRPC</span><span class="sxs-lookup"><span data-stu-id="a963b-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a963b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a963b-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a963b-115">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="a963b-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a963b-116">В диалоговом окне **Создать проект** выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a963b-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a963b-117">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="a963b-117">Select **Next**</span></span>
* <span data-ttu-id="a963b-118">Присвойте проекту имя **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="a963b-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="a963b-119">Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.</span><span class="sxs-lookup"><span data-stu-id="a963b-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="a963b-120">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a963b-120">Select **Create**</span></span>
* <span data-ttu-id="a963b-121">в диалоговом окне **Создание веб-приложения ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="a963b-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="a963b-122">В раскрывающемся меню выберите **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="a963b-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="a963b-123">Выберите шаблон **gRPC Service**.</span><span class="sxs-lookup"><span data-stu-id="a963b-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="a963b-124">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a963b-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a963b-125">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a963b-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a963b-126">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a963b-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a963b-127">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="a963b-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a963b-128">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a963b-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="a963b-129">С помощью команды `dotnet new` в папке *GrpcGreeter* создается служба gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="a963b-130">С помощью команды `code` папка *GrpcGreeter* открывается в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a963b-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="a963b-131">Появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'GrpcGreeter'. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="a963b-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="a963b-132">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="a963b-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a963b-133">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a963b-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a963b-134">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a963b-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="a963b-135">Эти команды позволяют создать службу gRPC с помощью [.NET Core CLI](/dotnet/core/tools/dotnet).</span><span class="sxs-lookup"><span data-stu-id="a963b-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="a963b-136">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="a963b-136">Open the project</span></span>

<span data-ttu-id="a963b-137">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="a963b-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="a963b-138">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="a963b-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a963b-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a963b-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a963b-140">Нажмите `Ctrl+F5`, чтобы запустить службу gRPC без отладчика.</span><span class="sxs-lookup"><span data-stu-id="a963b-140">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="a963b-141">Служба запустится в командной строке Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a963b-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a963b-142">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a963b-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a963b-143">Выполните команду `dotnet run` в командной строке, чтобы запустить проект gRPC Greeter с именем *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="a963b-143">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="a963b-144">В журналах отобразится служба, которая ожидает передачи данных через `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="a963b-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="a963b-145">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="a963b-145">Examine the project files</span></span>

<span data-ttu-id="a963b-146">Файлы проекта *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="a963b-146">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="a963b-147">*greet.proto*: Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="a963b-148">Дополнительные сведения см. в разделе [Введение в gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="a963b-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="a963b-149">Папка *Services* содержит реализацию службы `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="a963b-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="a963b-150">*appSettings.json*: содержит данные конфигурации, такие как протокол, используемый в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a963b-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="a963b-151">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a963b-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="a963b-152">*Program.cs*: содержит точку входа для службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="a963b-153">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="a963b-153">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="a963b-154">*Startup.cs*: содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="a963b-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="a963b-155">Дополнительные сведения: [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a963b-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="a963b-156">Создание клиента gRPC в консольном приложении .NET</span><span class="sxs-lookup"><span data-stu-id="a963b-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a963b-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a963b-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a963b-158">Откройте еще один экземпляр Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a963b-158">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="a963b-159">Выберите **Файл** > **Создать** > **Проект** в меню.</span><span class="sxs-lookup"><span data-stu-id="a963b-159">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="a963b-160">В диалоговом окне **Создать проект** выберите **Консольное приложение (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="a963b-160">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="a963b-161">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="a963b-161">Select **Next**</span></span>
* <span data-ttu-id="a963b-162">В текстовом поле **Имя** введите "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="a963b-162">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="a963b-163">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a963b-163">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a963b-164">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a963b-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a963b-165">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a963b-165">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a963b-166">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="a963b-166">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a963b-167">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a963b-167">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a963b-168">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a963b-168">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a963b-169">Следуйте инструкциям в [этой статье](/dotnet/core/tutorials/using-on-mac-vs-full-solution), чтобы создать консольное приложение с именем *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="a963b-169">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="a963b-170">Добавление необходимых пакетов</span><span class="sxs-lookup"><span data-stu-id="a963b-170">Add required packages</span></span>

<span data-ttu-id="a963b-171">Для клиентского проекта gRPC требуются следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="a963b-171">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="a963b-172">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), который содержит клиент .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a963b-172">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="a963b-173">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;</span><span class="sxs-lookup"><span data-stu-id="a963b-173">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="a963b-174">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="a963b-174">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="a963b-175">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="a963b-175">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a963b-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a963b-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a963b-177">Установка пакетов с помощью консоли диспетчера пакетов (PMC) или управления пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="a963b-177">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="a963b-178">Установка пакетов с помощью консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="a963b-178">PMC option to install packages</span></span>

* <span data-ttu-id="a963b-179">В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="a963b-179">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="a963b-180">В **консоли диспетчера пакетов** перейдите в каталог, в котором содержится файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a963b-180">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="a963b-181">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a963b-181">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="a963b-182">Установка пакетов с помощью раздела управления пакетами NuGet</span><span class="sxs-lookup"><span data-stu-id="a963b-182">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="a963b-183">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="a963b-183">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="a963b-184">Выберите вкладку **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="a963b-184">Select the **Browse** tab.</span></span>
* <span data-ttu-id="a963b-185">В поле поиска введите **Grpc.Core**.</span><span class="sxs-lookup"><span data-stu-id="a963b-185">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="a963b-186">Выберите пакет **Grpc.Core** на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="a963b-186">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="a963b-187">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a963b-187">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a963b-188">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a963b-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a963b-189">Во **встроенном терминале** выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a963b-189">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a963b-190">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a963b-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a963b-191">Щелкните правой кнопкой мыши папку **Пакеты** на **панели решения** > **Добавление пакетов**</span><span class="sxs-lookup"><span data-stu-id="a963b-191">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="a963b-192">В поле поиска введите **Grpc.Net.Client**.</span><span class="sxs-lookup"><span data-stu-id="a963b-192">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="a963b-193">В области результатов выберите пакет **Grpc.Net.Client** и нажмите кнопку **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="a963b-193">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="a963b-194">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a963b-194">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="a963b-195">Добавление greet.proto</span><span class="sxs-lookup"><span data-stu-id="a963b-195">Add greet.proto</span></span>

* <span data-ttu-id="a963b-196">Создайте папку **Protos** в клиентском проекте gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-196">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="a963b-197">Скопируйте файл **Protos\greet.proto** из службы Greeter gRPC в проект клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-197">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="a963b-198">Измените файл проекта *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="a963b-198">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a963b-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a963b-199">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="a963b-200">Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.</span><span class="sxs-lookup"><span data-stu-id="a963b-200">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a963b-201">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a963b-201">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="a963b-202">Выберите файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a963b-202">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a963b-203">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a963b-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="a963b-204">Щелкните проект правой кнопкой мыши и выберите **Сервис > Изменить файл**.</span><span class="sxs-lookup"><span data-stu-id="a963b-204">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="a963b-205">Добавьте группу элементов с элементом `<Protobuf>`, ссылающимся на файл **greet.proto**:</span><span class="sxs-lookup"><span data-stu-id="a963b-205">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="a963b-206">Создание клиента Greeter</span><span class="sxs-lookup"><span data-stu-id="a963b-206">Create the Greeter client</span></span>

<span data-ttu-id="a963b-207">Скомпилируйте проект, чтобы создать типы в пространстве имен `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="a963b-207">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="a963b-208">Типы `GrpcGreeter` создаются автоматически в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="a963b-208">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="a963b-209">Добавьте в файл *Program.cs* клиента gRPC следующий код:</span><span class="sxs-lookup"><span data-stu-id="a963b-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="a963b-210">файл *Program.cs* содержит точку входа и логику для клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="a963b-211">Клиент Greeter создается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a963b-211">The Greeter client is created by:</span></span>

* <span data-ttu-id="a963b-212">Создание экземпляра `HttpClient` со сведениями для создания подключения к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="a963b-212">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="a963b-213">Использование `HttpClient` для создания клиента Greeter:</span><span class="sxs-lookup"><span data-stu-id="a963b-213">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-9)]

<span data-ttu-id="a963b-214">Клиент Greeter вызывает асинхронный метод `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="a963b-214">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="a963b-215">Отображается результат вызова `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="a963b-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=10-12)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="a963b-216">Тестирование клиента gRPC с помощью службы Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="a963b-216">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a963b-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a963b-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a963b-218">В службе Greeter нажмите `Ctrl+F5` для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="a963b-218">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="a963b-219">В проекте `GrpcGreeterClient` нажмите `Ctrl+F5` для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="a963b-219">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a963b-220">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a963b-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a963b-221">Запустите службу Greeter.</span><span class="sxs-lookup"><span data-stu-id="a963b-221">Start the Greeter service.</span></span>
* <span data-ttu-id="a963b-222">Запустите клиент.</span><span class="sxs-lookup"><span data-stu-id="a963b-222">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="a963b-223">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="a963b-223">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="a963b-224">Служба отправляет сообщение "Hello GreeterClient" в качестве ответа.</span><span class="sxs-lookup"><span data-stu-id="a963b-224">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="a963b-225">Ответ "Hello GreeterClient" отображается в командной строке:</span><span class="sxs-lookup"><span data-stu-id="a963b-225">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="a963b-226">Служба gRPC записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="a963b-226">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="a963b-227">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a963b-227">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
