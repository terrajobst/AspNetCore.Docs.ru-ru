---
title: Учебник. Начало работы с gRPC в ASP.NET Core
author: juntaoluo
description: В этой серии руководств объясняется, как создать службу gRPC в ASP.NET Core. Узнайте, как создать проект службы gRPC, изменить файл proto и добавить дуплексный режим потоковой передачи вызовов.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672571"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Учебник. Начало работы со службой gRPC в ASP.NET Core

Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)

В этом руководстве приводятся основные сведения о создании службы gRPC с помощью ASP.NET Core.

Мы создадим службу gRPC, отправляющую ответное приветствие.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * Создание службы gRPC.
> * Запуск службы gRPC.
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

### <a name="run-the-service"></a>Запуск службы

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Нажмите CTRL + F5, чтобы запустить службу gRPC без отладчика.

  Служба запустится в командной строке Visual Studio. В журналах отобразятся сведения о том, что служба ожидает передачи данных через `http://localhost:50051`.

  ![Новое веб-приложение ASP.NET Core](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

* Выполните команду `dotnet run` в командной строке, чтобы запустить проект gRPC Greeter с именем GrpcGreeter. В журналах отобразятся сведения о том, что служба ожидает передачи данных через `http://localhost:50051`.

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
```

<!-- End of combined VS/Mac tabs -->

---

В следующем руководстве будет демонстрироваться создание клиента gRPC, который необходим для тестирования службы Greeter.

### <a name="examine-the-project-files-of-the-grpc-project"></a>Изучение файлов проекта gRPC

Файлы GrpcGreeter:

* greet.proto. Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC. Для получения дополнительной информации см. <xref:grpc/index>.
* Папка *Services* содержит реализацию службы `Greeter`.
* Файл *appSettings.json* содержит данные конфигурации, такие как протокол, используемый в Kestrel. Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.
* *Program.cs*: содержит точку входа для службы gRPC. Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.
* Startup.cs: содержит код, задающий поведение приложения. Для получения дополнительной информации см. <xref:fundamentals/startup>.

### <a name="test-the-service"></a>Проверка службы

## <a name="additional-resources"></a>Дополнительные ресурсы

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * создание службы gRPC;
> * запуск службы gRPC.
> * Анализ файлов проекта.

> [!div class="step-by-step"]
> [Далее: создание клиента gRPC в .NET Core](xref:tutorials/grpc/grpc-client)