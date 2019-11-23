---
title: Службы gRPC на языке C#
author: juntaoluo
description: Ознакомьтесь с основными понятиями при написании gRPC C#Services с помощью.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: 8d99d036fd4b00fc4568e67ea5225dc006dea4b1
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925185"
---
# <a name="grpc-services-with-c"></a>gRPC Services с\#ом C

В этом документе описываются понятия, необходимые для написания [gRPC](https://grpc.io/docs/guides/) приложений в C#. Обсуждаемые здесь темы применимы как к приложениям gRPC, основанным на [C-ядер](https://grpc.io/blog/grpc-stacks), так и к ASP.NET Coreм.

## <a name="proto-file"></a>файл с таким же файлом

Для разработки API в gRPC используется подход, при котором сначала создается контракт. Буферы протоколов (protobuf) по умолчанию используются в качестве языка конструирования интерфейса (IDL). Файл *\*.,* содержащий:

* Определение службы gRPC.
* Сообщения, передаваемые между клиентами и серверами.

Дополнительные сведения о синтаксисе protobuf-файлов см. в [официальной документации (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Например, рассмотрим файл *greet.,* используемый в [начале работы со службой gRPC](xref:tutorials/grpc/grpc-start):

* Определяет службу `Greeter`.
* `Greeter` служба определяет `SayHello` вызов.
* `SayHello` отправляет `HelloRequest` сообщение и получает `HelloReply` сообщение:

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Добавление файла с расширением. a в приложение C\#

Файл *\*.* -файла включается в проект путем его добавления в группу элементов `<Protobuf>`.

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>Средства C# для работы с файлами с расширением .proto

Пакет инструментов [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) необходим для создания C# ресурсов из файлов *\*..* . Созданные активы (файлы):

* Создаются по мере необходимости при каждом построении проекта.
* Не добавляются в проект или не возвращаются в систему управления версиями.
* Являются артефактом сборки, содержащимся в каталоге *obj* .

Этот пакет требуется для серверных и клиентских проектов. `Grpc.AspNetCore` метапакет содержит ссылку на `Grpc.Tools`. Серверные проекты могут добавлять `Grpc.AspNetCore` с помощью диспетчера пакетов в Visual Studio или путем добавления `<PackageReference>` в файл проекта:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

Клиентские проекты должны непосредственно ссылаться на `Grpc.Tools` вместе с другими пакетами, необходимыми для использования клиента gRPC. Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a>Созданные C# активы

Пакет инструментов создает C# типы, представляющие сообщения, определенные в включаемых файлах *\*.* Type.

Для ресурсов на стороне сервера создается абстрактный базовый тип службы. Базовый тип содержит определения всех вызовов gRPC, содержащихся в файле *.* Typ. Создайте конкретную реализацию службы, производную от этого базового типа, и Реализуйте логику для вызовов gRPC. Для `greet.proto`в примере, описанном выше, создается абстрактный тип `GreeterBase`, содержащий метод виртуального `SayHello`. Конкретная реализация `GreeterService` переопределяет метод и реализует логику, обрабатывающую вызов gRPC.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

Для клиентских ресурсов создается конкретный тип клиента. Вызовы gRPC в файле с *расширением.* имя преобразуются в методы конкретного типа, которые могут быть вызваны. Для `greet.proto`, описанном ранее, создается конкретный тип `GreeterClient`. Вызовите `GreeterClient.SayHelloAsync`, чтобы инициировать вызов gRPC на сервере.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

По умолчанию серверные и клиентские ресурсы формируются для каждого файла *\*.,* который входит в группу `<Protobuf>` элементов. Чтобы гарантировать, что в серверном проекте создаются только ресурсы сервера, атрибуту `GrpcServices` присваивается значение `Server`.

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

Аналогичным образом атрибуту присваивается значение `Client` в клиентских проектах.

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
