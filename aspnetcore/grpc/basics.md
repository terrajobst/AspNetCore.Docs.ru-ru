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
# <a name="grpc-services-with-c"></a><span data-ttu-id="4ea16-103">gRPC Services с\#ом C</span><span class="sxs-lookup"><span data-stu-id="4ea16-103">gRPC services with C\#</span></span>

<span data-ttu-id="4ea16-104">В этом документе описываются понятия, необходимые для написания [gRPC](https://grpc.io/docs/guides/) приложений в C#.</span><span class="sxs-lookup"><span data-stu-id="4ea16-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="4ea16-105">Обсуждаемые здесь темы применимы как к приложениям gRPC, основанным на [C-ядер](https://grpc.io/blog/grpc-stacks), так и к ASP.NET Coreм.</span><span class="sxs-lookup"><span data-stu-id="4ea16-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="4ea16-106">файл с таким же файлом</span><span class="sxs-lookup"><span data-stu-id="4ea16-106">proto file</span></span>

<span data-ttu-id="4ea16-107">Для разработки API в gRPC используется подход, при котором сначала создается контракт.</span><span class="sxs-lookup"><span data-stu-id="4ea16-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="4ea16-108">Буферы протоколов (protobuf) по умолчанию используются в качестве языка конструирования интерфейса (IDL).</span><span class="sxs-lookup"><span data-stu-id="4ea16-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="4ea16-109">Файл *\*.,* содержащий:</span><span class="sxs-lookup"><span data-stu-id="4ea16-109">The *\*.proto* file contains:</span></span>

* <span data-ttu-id="4ea16-110">Определение службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="4ea16-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="4ea16-111">Сообщения, передаваемые между клиентами и серверами.</span><span class="sxs-lookup"><span data-stu-id="4ea16-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="4ea16-112">Дополнительные сведения о синтаксисе protobuf-файлов см. в [официальной документации (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="4ea16-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="4ea16-113">Например, рассмотрим файл *greet.,* используемый в [начале работы со службой gRPC](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="4ea16-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="4ea16-114">Определяет службу `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="4ea16-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="4ea16-115">`Greeter` служба определяет `SayHello` вызов.</span><span class="sxs-lookup"><span data-stu-id="4ea16-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="4ea16-116">`SayHello` отправляет `HelloRequest` сообщение и получает `HelloReply` сообщение:</span><span class="sxs-lookup"><span data-stu-id="4ea16-116">`SayHello` sends a `HelloRequest` message and receives a `HelloReply` message:</span></span>

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="4ea16-117">Добавление файла с расширением. a в приложение C\#</span><span class="sxs-lookup"><span data-stu-id="4ea16-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="4ea16-118">Файл *\*.* -файла включается в проект путем его добавления в группу элементов `<Protobuf>`.</span><span class="sxs-lookup"><span data-stu-id="4ea16-118">The *\*.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="4ea16-119">Средства C# для работы с файлами с расширением .proto</span><span class="sxs-lookup"><span data-stu-id="4ea16-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="4ea16-120">Пакет инструментов [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) необходим для создания C# ресурсов из файлов *\*..* .</span><span class="sxs-lookup"><span data-stu-id="4ea16-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *\*.proto* files.</span></span> <span data-ttu-id="4ea16-121">Созданные активы (файлы):</span><span class="sxs-lookup"><span data-stu-id="4ea16-121">The generated assets (files):</span></span>

* <span data-ttu-id="4ea16-122">Создаются по мере необходимости при каждом построении проекта.</span><span class="sxs-lookup"><span data-stu-id="4ea16-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="4ea16-123">Не добавляются в проект или не возвращаются в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="4ea16-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="4ea16-124">Являются артефактом сборки, содержащимся в каталоге *obj* .</span><span class="sxs-lookup"><span data-stu-id="4ea16-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="4ea16-125">Этот пакет требуется для серверных и клиентских проектов.</span><span class="sxs-lookup"><span data-stu-id="4ea16-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="4ea16-126">`Grpc.AspNetCore` метапакет содержит ссылку на `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="4ea16-126">The `Grpc.AspNetCore` metapackage includes a reference to `Grpc.Tools`.</span></span> <span data-ttu-id="4ea16-127">Серверные проекты могут добавлять `Grpc.AspNetCore` с помощью диспетчера пакетов в Visual Studio или путем добавления `<PackageReference>` в файл проекта:</span><span class="sxs-lookup"><span data-stu-id="4ea16-127">Server projects can add `Grpc.AspNetCore` using the Package Manager in Visual Studio or by adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

<span data-ttu-id="4ea16-128">Клиентские проекты должны непосредственно ссылаться на `Grpc.Tools` вместе с другими пакетами, необходимыми для использования клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="4ea16-128">Client projects should directly reference `Grpc.Tools` alongside the other packages required to use the gRPC client.</span></span> <span data-ttu-id="4ea16-129">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`:</span><span class="sxs-lookup"><span data-stu-id="4ea16-129">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a><span data-ttu-id="4ea16-130">Созданные C# активы</span><span class="sxs-lookup"><span data-stu-id="4ea16-130">Generated C# assets</span></span>

<span data-ttu-id="4ea16-131">Пакет инструментов создает C# типы, представляющие сообщения, определенные в включаемых файлах *\*.* Type.</span><span class="sxs-lookup"><span data-stu-id="4ea16-131">The tooling package generates the C# types representing the messages defined in the included *\*.proto* files.</span></span>

<span data-ttu-id="4ea16-132">Для ресурсов на стороне сервера создается абстрактный базовый тип службы.</span><span class="sxs-lookup"><span data-stu-id="4ea16-132">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="4ea16-133">Базовый тип содержит определения всех вызовов gRPC, содержащихся в файле *.* Typ.</span><span class="sxs-lookup"><span data-stu-id="4ea16-133">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="4ea16-134">Создайте конкретную реализацию службы, производную от этого базового типа, и Реализуйте логику для вызовов gRPC.</span><span class="sxs-lookup"><span data-stu-id="4ea16-134">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="4ea16-135">Для `greet.proto`в примере, описанном выше, создается абстрактный тип `GreeterBase`, содержащий метод виртуального `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="4ea16-135">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="4ea16-136">Конкретная реализация `GreeterService` переопределяет метод и реализует логику, обрабатывающую вызов gRPC.</span><span class="sxs-lookup"><span data-stu-id="4ea16-136">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="4ea16-137">Для клиентских ресурсов создается конкретный тип клиента.</span><span class="sxs-lookup"><span data-stu-id="4ea16-137">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="4ea16-138">Вызовы gRPC в файле с *расширением.* имя преобразуются в методы конкретного типа, которые могут быть вызваны.</span><span class="sxs-lookup"><span data-stu-id="4ea16-138">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="4ea16-139">Для `greet.proto`, описанном ранее, создается конкретный тип `GreeterClient`.</span><span class="sxs-lookup"><span data-stu-id="4ea16-139">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="4ea16-140">Вызовите `GreeterClient.SayHelloAsync`, чтобы инициировать вызов gRPC на сервере.</span><span class="sxs-lookup"><span data-stu-id="4ea16-140">Call `GreeterClient.SayHelloAsync` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="4ea16-141">По умолчанию серверные и клиентские ресурсы формируются для каждого файла *\*.,* который входит в группу `<Protobuf>` элементов.</span><span class="sxs-lookup"><span data-stu-id="4ea16-141">By default, server and client assets are generated for each *\*.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="4ea16-142">Чтобы гарантировать, что в серверном проекте создаются только ресурсы сервера, атрибуту `GrpcServices` присваивается значение `Server`.</span><span class="sxs-lookup"><span data-stu-id="4ea16-142">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

<span data-ttu-id="4ea16-143">Аналогичным образом атрибуту присваивается значение `Client` в клиентских проектах.</span><span class="sxs-lookup"><span data-stu-id="4ea16-143">Similarly, the attribute is set to `Client` in client projects.</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a><span data-ttu-id="4ea16-144">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4ea16-144">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
