---
title: Службы gRPC на языке C#
author: juntaoluo
description: Изучите основные принципы, при создании служб gRPC с C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 78d744d641396c449a142375c69730333f8183cd
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223875"
---
# <a name="grpc-services-with-c"></a>gRPC служб с помощью C\#

В этом документе описываются основные понятия, необходимые для написания [gRPC](https://grpc.io/docs/guides/) приложений в C#. Темам, рассмотренным здесь, относятся к [C-core](https://grpc.io/blog/grpc-stacks)-приложений на основе ASP.NET Core и на основе gRPC.

## <a name="proto-file"></a>Имя файла

gRPC использует первого контракта подход к разработке API. Буферы протокола (protobuf) используется как язык разработки интерфейса (IDL) по умолчанию. *.Proto* файл содержит:

* Определение службы gRPC.
* Сообщения, отправленные между клиентами и серверами.

Дополнительные сведения о синтаксисе файлов protobuf, см. в разделе [официальной документации (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Например, рассмотрим *greet.proto* файл, используемый в [приступить к работе со службой gRPC](xref:tutorials/grpc/grpc-start):

* Определяет `Greeter` службы.
* `Greeter` Служба определяет `SayHello` вызова.
* `SayHello` отправляет `HelloRequest` сообщений и получает `HelloReply` сообщение:

[!code-protobuf[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Добавьте файл .proto C\# приложения

*.Proto* файл включен в проекте путем добавления его в `<Protobuf>` группы элементов:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>C#Поддержка средств для .proto файлов

Пакет средств [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) необходимых для формирования C# ресурсы из *.proto* файлов. Созданных ресурсов (файлов):

* Создаются по мере необходимости каждый раз, когда проект будет собран.
* Не добавлен в проект или в системе управления версиями.
* Являются артефакт сборки, содержащиеся в *obj* каталога.

Этот пакет необходим для клиентских и серверных проектов. `Grpc.Tools` можно добавить с помощью диспетчера пакетов в Visual Studio или добавление `<PackageReference>` в файл проекта:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=15)]

Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.

## <a name="generated-c-assets"></a>Созданный C# активы

Создает пакет средств C# типы, представляющие сообщения, определенные в включенные *.proto* файлов.

Для ресурсов на сервере создается абстрактный базовый тип службы. Базовый тип содержит определения всех вызовов gRPC содержащихся в *.proto* файла. Создайте реализацию конкретная служба, которая наследуется от этого базового типа и реализует логику для вызовов gRPC. Для `greet.proto`, пример было сказано ранее, абстрактный `GreeterBase` тип, содержащий виртуальный `SayHello` метод создается. Конкретная реализация `GreeterService` переопределяет метод и реализует логику обработки вызова gRPC.

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

Для ресурсов на стороне клиента создается тип конкретного клиента. Вызывает gRPC *.proto* файл преобразуются в методы в конкретный тип, который можно использовать. Для `greet.proto`, пример было сказано ранее, устойчивый `GreeterClient` создан тип. Вызовите `GreeterClient.SayHello` инициировать gRPC вызов на сервер.

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

По умолчанию, серверных и клиентских средств будут сгенерированы для каждой *.proto* файл, включенный в `<Protobuf>` группу элементов. Чтобы обеспечить только серверные ресурсы создаются в проекте сервера `GrpcServices` атрибут имеет значение `Server`.

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

Аналогично, атрибут задается `Client` в клиентские проекты.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
