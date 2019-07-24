---
title: Службы gRPC на языке C#
author: juntaoluo
description: Ознакомьтесь с основными понятиями при написании gRPC C#Services с помощью.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: 700fe9463317f9ee30dfe4ebf5201c7b9c0c5ad6
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412467"
---
# <a name="grpc-services-with-c"></a>gRPC Services с помощью C\#

В этом документе описываются понятия, необходимые для написания [gRPC](https://grpc.io/docs/guides/) приложений в C#. Обсуждаемые здесь темы применимы как к приложениям gRPC, основанным на [C-ядер](https://grpc.io/blog/grpc-stacks), так и к ASP.NET Coreм.

## <a name="proto-file"></a>файл с таким же файлом

gRPC использует подход на основе контракта к разработке API. Буферы протоколов (protobuf) по умолчанию используются в качестве языка конструирования интерфейса (IDL). Файл с *расширением.* , содержащий:

* Определение службы gRPC.
* Сообщения, передаваемые между клиентами и серверами.

Дополнительные сведения о синтаксисе protobuf-файлов см. в [официальной документации (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Например, рассмотрим файл *greet.,* используемый в [начале работы со службой gRPC](xref:tutorials/grpc/grpc-start):

* `Greeter` Определяет службу.
* `Greeter` Служба`SayHello` определяет вызов.
* `SayHello`отправляет сообщение и получает `HelloReply`сообщение: `HelloRequest`

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Добавление файла с расширением. a в приложение\# C

Файл с *расширением.* , добавленный в проект, добавляется `<Protobuf>` в группу элементов:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>C#Поддержка инструментов для файлов с.

Пакет инструментов [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) требуется для создания C# ресурсов из файлов с *...* Созданные активы (файлы):

* Создаются по мере необходимости при каждом построении проекта.
* Не добавляются в проект или не возвращаются в систему управления версиями.
* Являются артефактом сборки, содержащимся в каталоге *obj* .

Этот пакет требуется для серверных и клиентских проектов. Метапакет содержит ссылку на `Grpc.Tools`. `Grpc.AspNetCore` Серверные проекты могут `Grpc.AspNetCore` добавляться с помощью диспетчера пакетов в Visual Studio или путем `<PackageReference>` добавления в файл проекта:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

Клиентские проекты должны `Grpc.Tools` ссылаться напрямую. Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`следующим образом:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=1&range=11)]

## <a name="generated-c-assets"></a>Созданные C# активы

Пакет инструментов создает C# типы, представляющие сообщения, определенные *во включаемых* файлах.

Для ресурсов на стороне сервера создается абстрактный базовый тип службы. Базовый тип содержит определения всех вызовов gRPC, содержащихся в файле *.* Typ. Создайте конкретную реализацию службы, производную от этого базового типа, и Реализуйте логику для вызовов gRPC. `GreeterBase` `SayHello` Для примера `greet.proto`, описанного ранее, создается абстрактный тип, содержащий виртуальный метод. Конкретная реализация `GreeterService` переопределяет метод и реализует логику, обрабатывающую вызов gRPC.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

Для клиентских ресурсов создается конкретный тип клиента. Вызовы gRPC в файле с *расширением.* имя преобразуются в методы конкретного типа, которые могут быть вызваны. В примере `GreeterClient` , описанном ранее, создается конкретный тип. `greet.proto` Вызовите метод `GreeterClient.SayHello` , чтобы инициировать вызов gRPC на сервере.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=3-6&name=snippet)]

По умолчанию серверные и клиентские ресурсы формируются для каждого файла с *расширением* , который `<Protobuf>` входит в группу элементов. Чтобы гарантировать, что в серверном проекте создаются только ресурсы сервера, `GrpcServices` атрибуту присваивается `Server`значение.

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

Аналогичным образом атрибуту присваивается `Client` значение в клиентских проектах.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
