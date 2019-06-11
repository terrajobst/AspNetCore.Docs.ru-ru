---
title: Создание клиента и сервера gRPC .NET Core в ASP.NET Core
author: juntaoluo
description: В этом учебнике объясняется, как создать службу и клиента gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/05/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 71e3321819eb7169f0896abe3e07849f59ea6fc7
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692529"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="9c5bf-104">Учебник. Создание клиента и сервера gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c5bf-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="9c5bf-105">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="9c5bf-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="9c5bf-106">В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core и сервер gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="9c5bf-107">В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="9c5bf-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9c5bf-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="9c5bf-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9c5bf-110">Создание сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="9c5bf-111">Создание клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="9c5bf-112">Тестирование службы клиента gRPC с помощью службы Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="9c5bf-113">Создание службы gRPC</span><span class="sxs-lookup"><span data-stu-id="9c5bf-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c5bf-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c5bf-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9c5bf-115">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9c5bf-116">В диалоговом окне **Создать проект** выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9c5bf-117">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="9c5bf-117">Select **Next**</span></span>
* <span data-ttu-id="9c5bf-118">Присвойте проекту имя **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="9c5bf-119">Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="9c5bf-120">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-120">Select **Create**</span></span>
* <span data-ttu-id="9c5bf-121">в диалоговом окне **Создание веб-приложения ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="9c5bf-122">В раскрывающемся меню выберите **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="9c5bf-123">Выберите шаблон **gRPC Service**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="9c5bf-124">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c5bf-125">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9c5bf-126">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9c5bf-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="9c5bf-127">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="9c5bf-128">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="9c5bf-129">С помощью команды `dotnet new` в папке *GrpcGreeter* создается служба gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="9c5bf-130">С помощью команды `code` папка *GrpcGreeter* открывается в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="9c5bf-131">Появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'GrpcGreeter'. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="9c5bf-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="9c5bf-132">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c5bf-133">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c5bf-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9c5bf-134">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="9c5bf-135">Эти команды позволяют создать службу gRPC с помощью [.NET Core CLI](/dotnet/core/tools/dotnet).</span><span class="sxs-lookup"><span data-stu-id="9c5bf-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="9c5bf-136">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="9c5bf-136">Open the project</span></span>

<span data-ttu-id="9c5bf-137">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="9c5bf-138">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="9c5bf-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c5bf-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c5bf-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9c5bf-140">Нажмите CTRL + F5, чтобы запустить службу gRPC без отладчика.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-140">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="9c5bf-141">Служба запустится в командной строке Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9c5bf-142">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c5bf-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="9c5bf-143">Выполните команду `dotnet run` в командной строке, чтобы запустить проект gRPC Greeter с именем GrpcGreeter.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-143">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="9c5bf-144">В журналах отобразится служба, которая ожидает передачи данных через `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="9c5bf-145">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="9c5bf-145">Examine the project files</span></span>

<span data-ttu-id="9c5bf-146">Файлы GrpcGreeter:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-146">GrpcGreeter files:</span></span>

* <span data-ttu-id="9c5bf-147">*greet.proto*: Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="9c5bf-148">Дополнительные сведения см. в разделе [Введение в gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="9c5bf-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="9c5bf-149">Папка *Services* содержит реализацию службы `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="9c5bf-150">*appSettings.json*: содержит данные конфигурации, такие как протокол, используемый в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="9c5bf-151">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="9c5bf-152">*Program.cs*: содержит точку входа для службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="9c5bf-153">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-153">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="9c5bf-154">*Startup.cs*: содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="9c5bf-155">Дополнительные сведения: [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="9c5bf-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="9c5bf-156">Создание клиента gRPC в консольном приложении .NET</span><span class="sxs-lookup"><span data-stu-id="9c5bf-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c5bf-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c5bf-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9c5bf-158">Выберите **Файл** > **Создать** > **Проект** в меню.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="9c5bf-159">В диалоговом окне **Создать проект** выберите **Консольное приложение (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="9c5bf-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="9c5bf-160">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="9c5bf-160">Select **Next**</span></span>
* <span data-ttu-id="9c5bf-161">В текстовом поле **Имя** введите "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="9c5bf-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="9c5bf-162">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c5bf-163">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9c5bf-164">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9c5bf-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="9c5bf-165">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="9c5bf-166">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c5bf-167">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c5bf-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9c5bf-168">Следуйте инструкциям в [этой статье](/dotnet/core/tutorials/using-on-mac-vs-full-solution), чтобы создать консольное приложение с именем *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="9c5bf-169">Добавление необходимых пакетов</span><span class="sxs-lookup"><span data-stu-id="9c5bf-169">Add required packages</span></span>

<span data-ttu-id="9c5bf-170">Добавьте следующие пакеты в клиентский проект gRPC:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="9c5bf-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), который содержит API C# для клиента C-core;</span><span class="sxs-lookup"><span data-stu-id="9c5bf-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="9c5bf-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;</span><span class="sxs-lookup"><span data-stu-id="9c5bf-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="9c5bf-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="9c5bf-174">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c5bf-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c5bf-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9c5bf-176">Установка пакетов с помощью консоли диспетчера пакетов (PMC) или управления пакетами NuGet</span><span class="sxs-lookup"><span data-stu-id="9c5bf-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Package</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="9c5bf-177">Установка пакетов с помощью консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="9c5bf-177">PMC option to install packages</span></span>

* <span data-ttu-id="9c5bf-178">В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="9c5bf-179">В **консоли диспетчера пакетов** перейдите в каталог, в котором содержится файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="9c5bf-180">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Core
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="9c5bf-181">Установка пакетов с помощью раздела управления пакетами NuGet</span><span class="sxs-lookup"><span data-stu-id="9c5bf-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="9c5bf-182">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="9c5bf-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="9c5bf-183">Выберите вкладку **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="9c5bf-184">В поле поиска введите **Grpc.Core**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="9c5bf-185">Выберите пакет **Grpc.Core** на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="9c5bf-186">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c5bf-187">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9c5bf-188">Во **встроенном терминале** выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c5bf-189">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c5bf-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9c5bf-190">Щелкните правой кнопкой мыши папку **Пакеты** на **панели решения** > **Добавление пакетов**</span><span class="sxs-lookup"><span data-stu-id="9c5bf-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="9c5bf-191">В поле поиска введите **Grpc.Core**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-191">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="9c5bf-192">В области результатов выберите пакет **Grpc.Core** и нажмите кнопку **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="9c5bf-192">Select the **Grpc.Core** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="9c5bf-193">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="9c5bf-194">Добавление greet.proto</span><span class="sxs-lookup"><span data-stu-id="9c5bf-194">Add greet.proto</span></span>

* <span data-ttu-id="9c5bf-195">Создайте папку **Protos** в клиентском проекте gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="9c5bf-196">Скопируйте файл **Protos\greet.proto** из службы Greeter gRPC в проект клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="9c5bf-197">Измените файл проекта *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c5bf-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c5bf-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="9c5bf-199">Щелкните правой кнопкой мыши проект и выберите **Изменить GrpcGreeterClient.csproj**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c5bf-200">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="9c5bf-201">Выберите файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c5bf-202">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c5bf-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="9c5bf-203">Щелкните проект правой кнопкой мыши и выберите **Сервис > Изменить файл**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-203">Right click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="9c5bf-204">Добавьте файл **greet.proto** в `<Protobuf>` группу элементов файла проекта GrpcGreeterClient:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="9c5bf-205">Выполните сборку проекта клиента, чтобы запустить формирование ресурсов клиента C#.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greeter-client"></a><span data-ttu-id="9c5bf-206">Создание клиента Greeter</span><span class="sxs-lookup"><span data-stu-id="9c5bf-206">Create the Greeter client</span></span>

<span data-ttu-id="9c5bf-207">Соберите проект, чтобы создать типы в пространстве имен **Greeter**.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="9c5bf-208">Типы `Greeter` создаются автоматически в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="9c5bf-209">Добавьте в файл *Program.cs* клиента gRPC следующий код:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="9c5bf-210">файл *Program.cs* содержит точку входа и логику для клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="9c5bf-211">Клиент Greeter создается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-211">The Greeter client is created by:</span></span>

* <span data-ttu-id="9c5bf-212">Создание экземпляра `Channel` со сведениями для создания подключения к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-212">Instantiating a `Channel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="9c5bf-213">Использование `Channel` для создания клиента Greeter:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-213">Using the `Channel` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

<span data-ttu-id="9c5bf-214">Клиент Greeter вызывает асинхронный метод `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-214">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="9c5bf-215">Отображается результат вызова `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

<span data-ttu-id="9c5bf-216">Завершите работу `Channel`, используемого клиентом, после завершения операций для освобождения всех ресурсов.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-216">Shut down the `Channel` used by the client when operations have finished to release all resources.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="9c5bf-217">Тестирование клиента gRPC с помощью службы Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="9c5bf-217">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c5bf-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c5bf-218">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9c5bf-219">В службе Greeter нажмите клавиши CTRL+F5 для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-219">In the Greeter service, press Ctrl+F5 to start the server without the debugger.</span></span>
* <span data-ttu-id="9c5bf-220">В проекте GrpcGreeterClient нажмите клавиши CTRL+F5 для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-220">In the GrpcGreeterClient project, press Ctrl+F5 to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9c5bf-221">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c5bf-221">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="9c5bf-222">Запустите службу Greeter.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-222">Start the Greeter service.</span></span>
* <span data-ttu-id="9c5bf-223">Запустите клиент.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-223">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="9c5bf-224">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="9c5bf-224">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="9c5bf-225">Служба отправляет сообщение "Hello GreeterClient" в качестве ответа.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-225">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="9c5bf-226">Ответ "Hello GreeterClient" отображается в командной строке:</span><span class="sxs-lookup"><span data-stu-id="9c5bf-226">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="9c5bf-227">Служба gRPC записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="9c5bf-227">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="9c5bf-228">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="9c5bf-228">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
