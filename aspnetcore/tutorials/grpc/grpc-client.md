---
title: Учебник. Создание клиента gRPC в .NET Core
author: juntaoluo
description: В этой серии руководств объясняется, как создать службу gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: 031afbfaf097c518a85400b0b6abbc135c1bc611
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59674162"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a>Учебник. Создание клиента gRPC в .NET Core

Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)

В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core, который может взаимодействовать со службами gRPC.

В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * Создание клиента gRPC.
> * Запуск службы для службы Greeter gRPC, созданной в предыдущем руководстве.
> * Анализ файлов проекта.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a>Создание консольного приложения .NET

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Следуйте инструкциям в [этой статье](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio), чтобы создать консольное приложение с именем *GrpcGreeterClient*.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

Следуйте инструкциям в [этой статье](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code), чтобы создать консольное приложение с именем *GrpcGreeterClient*.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

Следуйте инструкциям в [этой статье](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution), чтобы создать консольное приложение с именем *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a>Добавление необходимых пакетов

Добавьте следующие пакеты в клиентский проект gRPC:

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), который содержит API C# для клиента C-core;
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf. Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.

Пакеты можно добавить одним из описанных ниже способов.

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a>Установка пакетов с помощью консоли диспетчера пакетов

* В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.
* В **консоли диспетчера пакетов** перейдите в каталог, в котором содержится файл *GrpcGreeterClient.csproj*.
* Выполните следующую команду:

    ```powershell
    Install-Package Grpc.Core
    ```

* Повторите команду `Install-Package` для Google.Protobuf и Grpc.Tools.

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a>Установка пакетов с помощью раздела управления пакетами NuGet

* Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**
* В качестве **источника пакета** выберите "nuget.org".
* В поле поиска введите Grpc.Core.
* Выберите пакет Grpc.Core на вкладке **Обзор** и нажмите кнопку **Установить**.
* Повторите эти действия для Google.Protobuf и Grpc.Tools.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

Во **встроенном терминале** выполните следующую команду.

```console
dotnet add TodoApi.csproj package Grpc.Core
```

Повторите эти действия для Google.Protobuf и Grpc.Tools.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.
* В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".
* В поле поиска введите Grpc.Core.
* В области результатов выберите пакет Grpc.Core и нажмите кнопку **Добавить пакет**.
* Повторите эти действия для Google.Protobuf и Grpc.Tools.

---

## <a name="add-the-greetproto-file"></a>Добавление файла greet.proto

Скопируйте файл **Protos\greet.proto** из службы Greeter gRPC в проект клиента gRPC. Добавьте файл **greet.proto** в `<Protobuf>` группу элементов файла проекта GrpcGreeterClient:

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> Файл проекта GrpcGreeterClient можно открыть, щелкнув проект правой кнопкой мыши и выбрав пункт раскрывающегося меню **Изменить GrpcGreeterClient.csproj**.
>
> ![Новое веб-приложение ASP.NET Core](grpc-start/_static/edit_csproj.png)
>
> Кроме того, можно перейти в каталог GrpcGreeterClient и изменить `GrpcGreeterClient.csproj` с помощью любого текстового редактора.

Атрибут `GrpcServices="Client"` добавляется, чтобы для включенного файла protobuf создавались только ресурсы клиента C#. Выполните сборку проекта клиента, чтобы запустить формирование ресурсов клиента C#.

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a>Создание GreeterClient и запуск унарного вызова SayHello

Добавьте следующий код в метод `Main` файла `Program.cs` проекта клиента gRPC:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

Для получения доступа к обязательным типам требуются следующие директивы using:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

GreeterClient создается путем создания экземпляра `Channel`, содержащего сведения для создания подключения к службе gRPC и использования ее для создания `GreeterClient`:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

GreeterClient содержит унарный вызов `SayHello`, который может вызываться асинхронно:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

Результаты вызова `SayHello` хранятся в объекте `reply`, который затем можно вывести на экран:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

`Channel`, используемый клиентом, должен завершить работу после завершения операций для освобождения всех ресурсов:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> Вам потребуется выполнить сборку проекта, прежде чем можно будет разрешить типы в пространстве имен **Greeter**. Эти типы создаются автоматически во время сборки и недоступны перед запуском сборки.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Тестирование клиента gRPC с помощью службы Greeter gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Убедитесь, что служба Greeter gRPC, созданная в предыдущем руководстве, запущена.

* После запуска службы вернитесь к проекту **GrpcGreeterClient** и задайте его в качестве запускаемого проекта. Нажмите клавиши CTRL + F5, чтобы выполнить запуск клиента без отладчика.

  Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient". Служба отправляет ответное сообщение "Hello GreeterClient", которое отображается в командной строке.

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/client.png)

  Служба записывает сведения об успешном вызове в журналы, что отображается в командной строке.

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

* Убедитесь, что служба Greeter gRPC, созданная в предыдущем руководстве, запущена.

* Выполните команду `dotnet run` в командной строке, чтобы запустить проект клиента GrpcGreeter.Client.

Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient". Служба отправляет ответное сообщение "Hello GreeterClient", которое отображается в командной строке.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Служба записывает сведения об успешном вызове в журналы, что отображается в командной строке.

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

### <a name="examine-the-project-files-of-the-grpc-project"></a>Изучение файлов проекта gRPC

Файл GrpcGreeterClient клиента gRPC:

файл *Program.cs* содержит точку входа и логику для клиента gRPC.

## <a name="additional-resources"></a>Дополнительные ресурсы

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * Создание клиента gRPC.
> * Запуск службы для службы Greeter gRPC, созданной в предыдущем руководстве.
> * Анализ файлов проекта.

> [!div class="step-by-step"]
> [Предыдущая статья. Создание службы Greeter gRPC](xref:tutorials/grpc/grpc-start)
