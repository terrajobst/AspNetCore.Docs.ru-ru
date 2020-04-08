---
page_type: sample
description: В этом учебнике объясняется, как создать службу и клиента gRPC в ASP.NET Core.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: create-grpc-client
ms.openlocfilehash: b9feb9eed62177358fffc0d7da582f625a431e32
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78648160"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a>Создание клиента и сервера gRPC в ASP.NET Core 3.0 с помощью Visual Studio

В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core и сервер gRPC ASP.NET Core.

В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.

В этом учебнике рассмотрены следующие задачи:

* Создание сервера gRPC.
* Создание клиента gRPC.
* Тестирование службы клиента gRPC с помощью службы Greeter gRPC.

## <a name="create-a-grpc-service"></a>Создание службы gRPC

* В меню **Файл** Visual Studio откройте **Создать** > **Проект**.
* В диалоговом окне **Создать проект** выберите **Веб-приложение ASP.NET Core**.
* Выберите **Далее**
* Присвойте проекту имя **GrpcGreeter**. Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.
* Выберите **Создать**.
* в диалоговом окне **Создание веб-приложения ASP.NET Core**:
  * В раскрывающемся меню выберите **.NET Core** и **ASP.NET Core 3.0**. 
  * Выберите шаблон **gRPC Service**.
  * Выберите **Создать**.

### <a name="run-the-service"></a>Запуск службы

* Нажмите `Ctrl+F5`, чтобы запустить службу gRPC без отладчика.

  Служба запустится в командной строке Visual Studio.

В журналах отобразится служба, которая ожидает передачи данных через `https://localhost:5001`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

> [!NOTE]
> Шаблон gRPC настроен для использования [протокола TLS](https://tools.ietf.org/html/rfc5246). Клиенты gRPC должны использовать протокол HTTPS для обращения к серверу.
>
> macOS не поддерживает ASP.NET Core gRPC с TLS. Для успешного запуска служб gRPC в macOS требуется дополнительная настройка. Дополнительные сведения см. в статье [gRPC и ASP.NET Core в macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).

### <a name="examine-the-project-files"></a>Анализ файлов проекта

Файлы проекта *GrpcGreeter*:

* *greet.proto*: Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC. Дополнительные сведения см. в разделе [Введение в gRPC](xref:grpc/index).
* Папка *Services* содержит реализацию службы `Greeter`.
* *appSettings.json*: содержит данные конфигурации, такие как протокол, используемый в Kestrel. Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.
* *Program.cs*: содержит точку входа для службы gRPC. Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.
* *Startup.cs*: содержит код, задающий поведение приложения. Дополнительные сведения: [Запуск приложения](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Создание клиента gRPC в консольном приложении .NET

* Откройте еще один экземпляр Visual Studio.
* Выберите **Файл** > **Создать** > **Проект** в меню.
* В диалоговом окне **Создать проект** выберите **Консольное приложение (.NET Core)** .
* Выберите **Далее**
* В текстовом поле **Имя** введите "GrpcGreeterClient".
* Выберите **Создать**.

### <a name="add-required-packages"></a>Добавление необходимых пакетов

Для клиентского проекта gRPC требуются следующие пакеты:

* [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), который содержит клиент .NET Core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf. Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.

Установка пакетов с помощью консоли диспетчера пакетов (PMC) или управления пакетами NuGet.

#### <a name="pmc-option-to-install-packages"></a>Установка пакетов с помощью консоли диспетчера пакетов

* В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.
* В **консоли диспетчера пакетов** перейдите в каталог, в котором содержится файл *GrpcGreeterClient.csproj*.
* Выполните следующие команды:

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Установка пакетов с помощью раздела управления пакетами NuGet

* Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**
* Выберите вкладку **Обзор**.
* В поле поиска введите **Grpc.Net.Client**.
* Выберите пакет **Grpc.Net.Client** на вкладке **Обзор** и нажмите кнопку **Установить**.
* Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.

### <a name="add-greetproto"></a>Добавление greet.proto

* Создайте папку **Protos** в клиентском проекте gRPC.
* Скопируйте файл **Protos\greet.proto** из службы Greeter gRPC в проект клиента gRPC.
* Измените файл проекта *GrpcGreeterClient.csproj*:

  Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.

* Добавьте группу элементов с элементом `<Protobuf>`, ссылающимся на файл **greet.proto**:

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a>Создание клиента Greeter

Скомпилируйте проект, чтобы создать типы в пространстве имен `GrpcGreeter`. Типы `GrpcGreeter` создаются автоматически в процессе сборки.

Добавьте в файл *Program.cs* клиента gRPC следующий код:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using GrpcGreeter;
using Grpc.Net.Client;

namespace GrpcGreeterClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // The port number(5001) must match the port of the gRPC server.
            var channel = GrpcChannel.ForAddress("https://localhost:5001");
            var client = new Greeter.GreeterClient(channel);
            var reply = await client.SayHelloAsync(
                              new HelloRequest { Name = "GreeterClient" });
            Console.WriteLine("Greeting: " + reply.Message);
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

файл *Program.cs* содержит точку входа и логику для клиента gRPC.

Клиент Greeter создается следующим образом:

* Создание экземпляра `GrpcChannel` со сведениями для создания подключения к службе gRPC.
* Использование `GrpcChannel` для создания клиента Greeter.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Тестирование клиента gRPC с помощью службы Greeter gRPC

* В службе Greeter нажмите `Ctrl+F5` для запуска сервера без отладчика.
* В проекте `GrpcGreeterClient` нажмите `Ctrl+F5` для запуска клиента без отладчика.

Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient". Служба отправляет сообщение "Hello GreeterClient" в качестве ответа. Ответ "Hello GreeterClient" отображается в командной строке:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Служба gRPC записывает сведения об успешном вызове в журналы, что отображается в командной строке.

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

### <a name="docs-help--next-steps-for-grpc"></a>Справочная документация и дальнейшие инструкции для gRPC

* [Введение в gRPC на платформе ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [Службы gRPC на языке C#](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [Миграция служб gRPC из C Core на ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
