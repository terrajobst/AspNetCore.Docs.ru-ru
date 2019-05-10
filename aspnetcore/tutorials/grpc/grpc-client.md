---
title: Учебник. Создание клиента gRPC в .NET Core
author: juntaoluo
description: В этой серии руководств объясняется, как создать службу gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: ec6bf5072c76de640a78b2c3f13dd1fc552b9d04
ms.sourcegitcommit: b508b115107e0f8d7f62b25cfcc8ad45e1373459
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212648"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a><span data-ttu-id="1b812-104">Учебник. Создание клиента gRPC в .NET Core</span><span class="sxs-lookup"><span data-stu-id="1b812-104">Tutorial: Create a .NET Core gRPC client</span></span>

<span data-ttu-id="1b812-105">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="1b812-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="1b812-106">В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core, который может взаимодействовать со службами gRPC.</span><span class="sxs-lookup"><span data-stu-id="1b812-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client that can communicate with gRPC services.</span></span>

<span data-ttu-id="1b812-107">В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="1b812-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

<span data-ttu-id="1b812-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="1b812-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b812-109">Создание клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="1b812-109">Create a gRPC client.</span></span>
> * <span data-ttu-id="1b812-110">Запуск службы для службы Greeter gRPC, созданной в предыдущем руководстве.</span><span class="sxs-lookup"><span data-stu-id="1b812-110">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="1b812-111">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="1b812-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a><span data-ttu-id="1b812-112">Создание консольного приложения .NET</span><span class="sxs-lookup"><span data-stu-id="1b812-112">Create a .NET console application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b812-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b812-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1b812-114">Следуйте инструкциям в [этой статье](/dotnet/core/tutorials/with-visual-studio), чтобы создать консольное приложение с именем *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="1b812-114">Follow the instructions [here](/dotnet/core/tutorials/with-visual-studio) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1b812-115">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1b812-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1b812-116">Следуйте инструкциям в [этой статье](/dotnet/core/tutorials/with-visual-studio-code), чтобы создать консольное приложение с именем *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="1b812-116">Follow the instructions [here](/dotnet/core/tutorials/with-visual-studio-code) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1b812-117">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="1b812-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1b812-118">Следуйте инструкциям в [этой статье](/dotnet/core/tutorials/using-on-mac-vs-full-solution), чтобы создать консольное приложение с именем *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="1b812-118">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a><span data-ttu-id="1b812-119">Добавление необходимых пакетов</span><span class="sxs-lookup"><span data-stu-id="1b812-119">Add required packages</span></span>

<span data-ttu-id="1b812-120">Добавьте следующие пакеты в клиентский проект gRPC:</span><span class="sxs-lookup"><span data-stu-id="1b812-120">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="1b812-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), который содержит API C# для клиента C-core;</span><span class="sxs-lookup"><span data-stu-id="1b812-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="1b812-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;</span><span class="sxs-lookup"><span data-stu-id="1b812-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="1b812-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="1b812-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="1b812-124">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="1b812-124">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="1b812-125">Пакеты можно добавить одним из описанных ниже способов.</span><span class="sxs-lookup"><span data-stu-id="1b812-125">Packages can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b812-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b812-126">Visual Studio</span></span>](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="1b812-127">Установка пакетов с помощью консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="1b812-127">PMC option to install packages</span></span>

* <span data-ttu-id="1b812-128">В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="1b812-128">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="1b812-129">В **консоли диспетчера пакетов** перейдите в каталог, в котором содержится файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="1b812-129">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="1b812-130">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="1b812-130">Run the following command:</span></span>

    ```powershell
    Install-Package Grpc.Core
    ```

* <span data-ttu-id="1b812-131">Повторите команду `Install-Package` для Google.Protobuf и Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="1b812-131">Repeat the `Install-Package` for Google.Protobuf and Grpc.Tools</span></span>

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="1b812-132">Установка пакетов с помощью раздела управления пакетами NuGet</span><span class="sxs-lookup"><span data-stu-id="1b812-132">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="1b812-133">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="1b812-133">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="1b812-134">В качестве **источника пакета** выберите "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="1b812-134">Set the **Package source** to "nuget.org"</span></span>
* <span data-ttu-id="1b812-135">В поле поиска введите Grpc.Core.</span><span class="sxs-lookup"><span data-stu-id="1b812-135">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="1b812-136">Выберите пакет Grpc.Core на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="1b812-136">Select the "Grpc.Core" package from the **Browse** tab and click **Install**</span></span>
* <span data-ttu-id="1b812-137">Повторите эти действия для Google.Protobuf и Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="1b812-137">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1b812-138">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1b812-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1b812-139">Во **встроенном терминале** выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="1b812-139">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
```

<span data-ttu-id="1b812-140">Повторите эти действия для Google.Protobuf и Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="1b812-140">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1b812-141">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="1b812-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1b812-142">Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.</span><span class="sxs-lookup"><span data-stu-id="1b812-142">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="1b812-143">В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="1b812-143">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="1b812-144">В поле поиска введите Grpc.Core.</span><span class="sxs-lookup"><span data-stu-id="1b812-144">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="1b812-145">В области результатов выберите пакет Grpc.Core и нажмите кнопку **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="1b812-145">Select the "Grpc.Core" package from the results pane and click **Add Package**</span></span>
* <span data-ttu-id="1b812-146">Повторите эти действия для Google.Protobuf и Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="1b812-146">Repeat for Google.Protobuf and Grpc.Tools</span></span>

---

## <a name="add-the-greetproto-file"></a><span data-ttu-id="1b812-147">Добавление файла greet.proto</span><span class="sxs-lookup"><span data-stu-id="1b812-147">Add the greet.proto file</span></span>

<span data-ttu-id="1b812-148">Скопируйте файл **Protos\greet.proto** из службы Greeter gRPC в проект клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="1b812-148">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span> <span data-ttu-id="1b812-149">Добавьте файл **greet.proto** в `<Protobuf>` группу элементов файла проекта GrpcGreeterClient:</span><span class="sxs-lookup"><span data-stu-id="1b812-149">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> <span data-ttu-id="1b812-150">Файл проекта GrpcGreeterClient можно открыть, щелкнув проект правой кнопкой мыши и выбрав пункт раскрывающегося меню **Изменить GrpcGreeterClient.csproj**.</span><span class="sxs-lookup"><span data-stu-id="1b812-150">You can open the project file of GrpcGreeterClient by right-clicking the project and selecting the **Edit GrpcGreeterClient.csproj** option from the dropdown menu.</span></span>
>
> ![Новое веб-приложение ASP.NET Core](grpc-start/_static/edit_csproj.png)
>
> <span data-ttu-id="1b812-152">Кроме того, можно перейти в каталог GrpcGreeterClient и изменить `GrpcGreeterClient.csproj` с помощью любого текстового редактора.</span><span class="sxs-lookup"><span data-stu-id="1b812-152">Alternatively, you can navigate to the GrpcGreeterClient directory and edit the `GrpcGreeterClient.csproj` with your favorite editor.</span></span>

<span data-ttu-id="1b812-153">Атрибут `GrpcServices="Client"` добавляется, чтобы для включенного файла protobuf создавались только ресурсы клиента C#.</span><span class="sxs-lookup"><span data-stu-id="1b812-153">The `GrpcServices="Client"` attribute is added so that only the C# client assets are generated for the included protobuf file.</span></span> <span data-ttu-id="1b812-154">Выполните сборку проекта клиента, чтобы запустить формирование ресурсов клиента C#.</span><span class="sxs-lookup"><span data-stu-id="1b812-154">Build the client project to trigger the generation of the C# client assets.</span></span>

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a><span data-ttu-id="1b812-155">Создание GreeterClient и запуск унарного вызова SayHello</span><span class="sxs-lookup"><span data-stu-id="1b812-155">Create a GreeterClient and invoke the SayHello unary call</span></span>

<span data-ttu-id="1b812-156">Добавьте следующий код в метод `Main` файла `Program.cs` проекта клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="1b812-156">Add the following code to `Main` method of the `Program.cs` file of the gRPC client project:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="1b812-157">Для получения доступа к обязательным типам требуются следующие директивы using:</span><span class="sxs-lookup"><span data-stu-id="1b812-157">To access the required types the following using statements are required:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

<span data-ttu-id="1b812-158">GreeterClient создается путем создания экземпляра `Channel`, содержащего сведения для создания подключения к службе gRPC и использования ее для создания `GreeterClient`:</span><span class="sxs-lookup"><span data-stu-id="1b812-158">The GreeterClient is created by instantiating a `Channel` containing the information for creating the connection to the gRPC service and using it to construct the `GreeterClient`:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

<span data-ttu-id="1b812-159">GreeterClient содержит унарный вызов `SayHello`, который может вызываться асинхронно:</span><span class="sxs-lookup"><span data-stu-id="1b812-159">The GreeterClient contains the unary call `SayHello` which can be invoked asynchronously:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

<span data-ttu-id="1b812-160">Результаты вызова `SayHello` хранятся в объекте `reply`, который затем можно вывести на экран:</span><span class="sxs-lookup"><span data-stu-id="1b812-160">The results of the `SayHello` call is stored in `reply` which can then be displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

<span data-ttu-id="1b812-161">`Channel`, используемый клиентом, должен завершить работу после завершения операций для освобождения всех ресурсов:</span><span class="sxs-lookup"><span data-stu-id="1b812-161">The `Channel` used by the client should be shut down when operations have finished to release all resources:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> <span data-ttu-id="1b812-162">Вам потребуется выполнить сборку проекта, прежде чем можно будет разрешить типы в пространстве имен **Greeter**.</span><span class="sxs-lookup"><span data-stu-id="1b812-162">You will need to build the project before the types in the **Greeter** namespace can be resolved.</span></span> <span data-ttu-id="1b812-163">Эти типы создаются автоматически во время сборки и недоступны перед запуском сборки.</span><span class="sxs-lookup"><span data-stu-id="1b812-163">These types are generated automatically during build and are not be available before a build is run.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="1b812-164">Тестирование клиента gRPC с помощью службы Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="1b812-164">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b812-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b812-165">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1b812-166">Убедитесь, что служба Greeter gRPC, созданная в предыдущем руководстве, запущена.</span><span class="sxs-lookup"><span data-stu-id="1b812-166">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="1b812-167">После запуска службы вернитесь к проекту **GrpcGreeterClient** и задайте его в качестве запускаемого проекта.</span><span class="sxs-lookup"><span data-stu-id="1b812-167">Once the service is running, return to the **GrpcGreeterClient** project set it as the Startup Project.</span></span> <span data-ttu-id="1b812-168">Нажмите клавиши CTRL + F5, чтобы выполнить запуск клиента без отладчика.</span><span class="sxs-lookup"><span data-stu-id="1b812-168">Press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="1b812-169">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="1b812-169">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="1b812-170">Служба отправляет ответное сообщение "Hello GreeterClient", которое отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="1b812-170">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/client.png)

  <span data-ttu-id="1b812-172">Служба записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="1b812-172">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1b812-174">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="1b812-174">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="1b812-175">Убедитесь, что служба Greeter gRPC, созданная в предыдущем руководстве, запущена.</span><span class="sxs-lookup"><span data-stu-id="1b812-175">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="1b812-176">Выполните команду `dotnet run` в командной строке, чтобы запустить проект клиента GrpcGreeter.Client.</span><span class="sxs-lookup"><span data-stu-id="1b812-176">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="1b812-177">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="1b812-177">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="1b812-178">Служба отправляет ответное сообщение "Hello GreeterClient", которое отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="1b812-178">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="1b812-179">Служба записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="1b812-179">The service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 194.5798ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="1b812-180">Изучение файлов проекта gRPC</span><span class="sxs-lookup"><span data-stu-id="1b812-180">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="1b812-181">Файл GrpcGreeterClient клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="1b812-181">gRPC client GrpcGreeterClient file:</span></span>

<span data-ttu-id="1b812-182">файл *Program.cs* содержит точку входа и логику для клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="1b812-182">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1b812-183">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1b812-183">Additional resources</span></span>

<span data-ttu-id="1b812-184">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="1b812-184">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b812-185">Создание клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="1b812-185">Create a gRPC client.</span></span>
> * <span data-ttu-id="1b812-186">Запуск службы для службы Greeter gRPC, созданной в предыдущем руководстве.</span><span class="sxs-lookup"><span data-stu-id="1b812-186">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="1b812-187">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="1b812-187">Examine the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1b812-188">Предыдущая статья. Создание службы Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="1b812-188">Previous: Create a gRPC Greeter Service</span></span>](xref:tutorials/grpc/grpc-start)
