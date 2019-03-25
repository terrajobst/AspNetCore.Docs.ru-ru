---
title: Учебник. Начало работы с gRPC в ASP.NET Core
author: juntaoluo
description: В этой серии руководств объясняется, как создать службу gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 5f7a2f6b57804b3295b23c322dcbac553b05528b
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320060"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="68ce1-104">Учебник. Начало работы со службой gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68ce1-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="68ce1-105">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="68ce1-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="68ce1-106">В этом руководстве приводятся основные сведения о создании службы gRPC с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68ce1-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="68ce1-107">Мы создадим службу gRPC, отправляющую ответное приветствие.</span><span class="sxs-lookup"><span data-stu-id="68ce1-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="68ce1-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="68ce1-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68ce1-109">Создание службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="68ce1-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="68ce1-110">Запустите службу.</span><span class="sxs-lookup"><span data-stu-id="68ce1-110">Run the service.</span></span>
> * <span data-ttu-id="68ce1-111">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="68ce1-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="68ce1-112">Создание службы gRPC</span><span class="sxs-lookup"><span data-stu-id="68ce1-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68ce1-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68ce1-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="68ce1-114">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="68ce1-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="68ce1-115">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68ce1-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="68ce1-116">![Новое веб-приложение ASP.NET Core](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="68ce1-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="68ce1-117">Присвойте проекту имя **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="68ce1-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="68ce1-118">Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.</span><span class="sxs-lookup"><span data-stu-id="68ce1-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="68ce1-119">![Новое веб-приложение ASP.NET Core](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="68ce1-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="68ce1-120">В раскрывающемся списке выберите **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="68ce1-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="68ce1-121">Выберите шаблон **gRPC Service**.</span><span class="sxs-lookup"><span data-stu-id="68ce1-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="68ce1-122">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="68ce1-122">The following starter project is created:</span></span>

  ![обозреватель решений](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="68ce1-124">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="68ce1-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="68ce1-125">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="68ce1-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="68ce1-126">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="68ce1-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="68ce1-127">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="68ce1-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="68ce1-128">С помощью команды `dotnet new` в папке *GrpcGreeter* создается служба gRPC.</span><span class="sxs-lookup"><span data-stu-id="68ce1-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="68ce1-129">С помощью команды `code` папка *GrpcGreeter* открывается в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="68ce1-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="68ce1-130">Появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'GrpcGreeter'. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="68ce1-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="68ce1-131">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="68ce1-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="68ce1-132">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="68ce1-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="68ce1-133">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="68ce1-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="68ce1-134">Эти команды позволяют создать службу gRPC с помощью [.NET Core CLI](/dotnet/core/tools/dotnet).</span><span class="sxs-lookup"><span data-stu-id="68ce1-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="68ce1-135">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="68ce1-135">Open the project</span></span>

<span data-ttu-id="68ce1-136">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="68ce1-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a><span data-ttu-id="68ce1-137">Проверка службы</span><span class="sxs-lookup"><span data-stu-id="68ce1-137">Test the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68ce1-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68ce1-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="68ce1-139">Убедитесь, что проект **GrpcGreeter.Server** назначен запускаемым проектом и нажмите клавиши CTRL+F5, чтобы запустить службу gRPC без отладчика.</span><span class="sxs-lookup"><span data-stu-id="68ce1-139">Ensure the **GrpcGreeter.Server** is set as the Startup Project and press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="68ce1-140">Служба запустится в командной строке Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68ce1-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="68ce1-141">В журналах отобразятся сведения о том, что служба ожидает передачи данных через `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="68ce1-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/server_start.png)

* <span data-ttu-id="68ce1-143">После запуска службы назначьте проект **GrpcGreeter.Server** запускаемым проектом и нажмите клавиши CTRL+F5, чтобы запустить клиент без отладчика.</span><span class="sxs-lookup"><span data-stu-id="68ce1-143">Once the service is running, set the **GrpcGreeter.Client** is set as the Startup Project and press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="68ce1-144">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="68ce1-144">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="68ce1-145">Служба отправляет ответное сообщение "Hello GreeterClient", которое отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="68ce1-145">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/client.png)

  <span data-ttu-id="68ce1-147">Служба записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="68ce1-147">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="68ce1-149">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="68ce1-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="68ce1-150">Выполните команду `dotnet run` в командной строке, чтобы запустить проект сервера GrpcGreeter.Server.</span><span class="sxs-lookup"><span data-stu-id="68ce1-150">Run the Server project GrpcGreeter.Server from the command line using `dotnet run`.</span></span> <span data-ttu-id="68ce1-151">В журналах отобразятся сведения о том, что служба ожидает передачи данных через `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="68ce1-151">The logs show that the service started listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\example\GrpcGreeter\GrpcGreeter.Server
```

* <span data-ttu-id="68ce1-152">Выполните команду `dotnet run` в командной строке, чтобы запустить проект клиента GrpcGreeter.Client.</span><span class="sxs-lookup"><span data-stu-id="68ce1-152">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="68ce1-153">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="68ce1-153">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="68ce1-154">Служба отправляет ответное сообщение "Hello GreeterClient", которое отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="68ce1-154">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="68ce1-155">Служба записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="68ce1-155">The service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\tp\GrpcGreeter\GrpcGreeter.Server
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 107.46730000000001ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="68ce1-156">Изучение файлов проекта gRPC</span><span class="sxs-lookup"><span data-stu-id="68ce1-156">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="68ce1-157">Файлы GrpcGreeter.Server:</span><span class="sxs-lookup"><span data-stu-id="68ce1-157">GrpcGreeter.Server files:</span></span>

* <span data-ttu-id="68ce1-158">greet.proto. Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="68ce1-158">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="68ce1-159">Для получения дополнительной информации см. <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="68ce1-159">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="68ce1-160">Папка *Services* содержит реализацию службы `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="68ce1-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="68ce1-161">Файл *appSettings.json* содержит данные конфигурации, такие как протокол, используемый в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="68ce1-161">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="68ce1-162">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="68ce1-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="68ce1-163">*Program.cs*: содержит точку входа для службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="68ce1-163">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="68ce1-164">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="68ce1-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="68ce1-165">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="68ce1-165">Startup.cs</span></span>

<span data-ttu-id="68ce1-166">содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="68ce1-166">Contains code that configures app behavior.</span></span> <span data-ttu-id="68ce1-167">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="68ce1-167">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="68ce1-168">Файл клиента GrpcGreeter.Client:</span><span class="sxs-lookup"><span data-stu-id="68ce1-168">gRPC client GrpcGreeter.Client file:</span></span>

<span data-ttu-id="68ce1-169">файл *Program.cs* содержит точку входа и логику для клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="68ce1-169">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="68ce1-170">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="68ce1-170">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68ce1-171">создание службы gRPC;</span><span class="sxs-lookup"><span data-stu-id="68ce1-171">Created a gRPC service.</span></span>
> * <span data-ttu-id="68ce1-172">запуск службы и клиента для тестирования службы;</span><span class="sxs-lookup"><span data-stu-id="68ce1-172">Ran the service and a client to test the service.</span></span>
> * <span data-ttu-id="68ce1-173">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="68ce1-173">Examined the project files.</span></span>
