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
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Учебник. Начало работы со службой gRPC в ASP.NET Core

Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)

В этом руководстве приводятся основные сведения о создании службы gRPC с помощью ASP.NET Core.

Мы создадим службу gRPC, отправляющую ответное приветствие.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * Создание службы gRPC.
> * Запустите службу.
> * Анализ файлов проекта.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>Создание службы gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.
* Создайте новое веб-приложение ASP.NET Core.
  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/np_3_0.1.png)
* Присвойте проекту имя **GrpcGreeter**. Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.
  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/np_3_0.2.png)
* В раскрывающемся списке выберите **.NET Core** и **ASP.NET Core 3.0**. Выберите шаблон **gRPC Service**.

  Создается следующий начальный проект:

  ![обозреватель решений](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Смените каталог (`cd`) на папку, в которой будет содержаться проект.
* Выполните следующие команды:

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * С помощью команды `dotnet new` в папке *GrpcGreeter* создается служба gRPC.
  * С помощью команды `code` папка *GrpcGreeter* открывается в новом экземпляре Visual Studio Code.

  Появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'GrpcGreeter'. Добавить их?**
* Выберите ответ **Да**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

Из терминала выполните следующие команды:

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

Эти команды позволяют создать службу gRPC с помощью [.NET Core CLI](/dotnet/core/tools/dotnet).

### <a name="open-the-project"></a>Открытие проекта

В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *GrpcGreeter.sln*.

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a>Проверка службы

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Убедитесь, что проект **GrpcGreeter.Server** назначен запускаемым проектом и нажмите клавиши CTRL+F5, чтобы запустить службу gRPC без отладчика.

  Служба запустится в командной строке Visual Studio. В журналах отобразятся сведения о том, что служба ожидает передачи данных через `http://localhost:50051`.

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/server_start.png)

* После запуска службы назначьте проект **GrpcGreeter.Server** запускаемым проектом и нажмите клавиши CTRL+F5, чтобы запустить клиент без отладчика.

  Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient". Служба отправляет ответное сообщение "Hello GreeterClient", которое отображается в командной строке.

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/client.png)

  Служба записывает сведения об успешном вызове в журналы, что отображается в командной строке.

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

* Выполните команду `dotnet run` в командной строке, чтобы запустить проект сервера GrpcGreeter.Server. В журналах отобразятся сведения о том, что служба ожидает передачи данных через `http://localhost:50051`.

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

* Выполните команду `dotnet run` в командной строке, чтобы запустить проект клиента GrpcGreeter.Client.

Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient". Служба отправляет ответное сообщение "Hello GreeterClient", которое отображается в командной строке.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Служба записывает сведения об успешном вызове в журналы, что отображается в командной строке.

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

### <a name="examine-the-project-files-of-the-grpc-project"></a>Изучение файлов проекта gRPC

Файлы GrpcGreeter.Server:

* greet.proto. Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC. Для получения дополнительной информации см. <xref:grpc/index>.
* Папка *Services* содержит реализацию службы `Greeter`.
* Файл *appSettings.json* содержит данные конфигурации, такие как протокол, используемый в Kestrel. Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.
* *Program.cs*: содержит точку входа для службы gRPC. Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.
* Startup.cs

содержит код, задающий поведение приложения. Для получения дополнительной информации см. <xref:fundamentals/startup>.

Файл клиента GrpcGreeter.Client:

файл *Program.cs* содержит точку входа и логику для клиента gRPC.

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * создание службы gRPC;
> * запуск службы и клиента для тестирования службы;
> * Анализ файлов проекта.
