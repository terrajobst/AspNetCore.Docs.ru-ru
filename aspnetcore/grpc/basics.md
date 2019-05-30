---
title: Службы gRPC на языке C#
author: juntaoluo
description: Изучите основные принципы, при создании служб gRPC с C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 5a88bd0e9f789058b3606691c5ebd9a74325ac9b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376346"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="ea5ac-103">gRPC служб с помощью C\#</span><span class="sxs-lookup"><span data-stu-id="ea5ac-103">gRPC services with C\#</span></span>

<span data-ttu-id="ea5ac-104">В этом документе описываются основные понятия, необходимые для написания [gRPC](https://grpc.io/docs/guides/) приложений в C#.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="ea5ac-105">Темам, рассмотренным здесь, относятся к [C-core](https://grpc.io/blog/grpc-stacks)-приложений на основе ASP.NET Core и на основе gRPC.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="ea5ac-106">Имя файла</span><span class="sxs-lookup"><span data-stu-id="ea5ac-106">proto file</span></span>

<span data-ttu-id="ea5ac-107">gRPC использует первого контракта подход к разработке API.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="ea5ac-108">Буферы протокола (protobuf) используется как язык разработки интерфейса (IDL) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="ea5ac-109">*.Proto* файл содержит:</span><span class="sxs-lookup"><span data-stu-id="ea5ac-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="ea5ac-110">Определение службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="ea5ac-111">Сообщения, отправленные между клиентами и серверами.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="ea5ac-112">Дополнительные сведения о синтаксисе файлов protobuf, см. в разделе [официальной документации (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="ea5ac-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="ea5ac-113">Например, рассмотрим *greet.proto* файл, используемый в [приступить к работе со службой gRPC](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="ea5ac-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="ea5ac-114">Определяет `Greeter` службы.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="ea5ac-115">`Greeter` Служба определяет `SayHello` вызова.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="ea5ac-116">`SayHello` отправляет `HelloRequest` сообщений и получает `HelloResponse` сообщение:</span><span class="sxs-lookup"><span data-stu-id="ea5ac-116">`SayHello` sends a `HelloRequest` message and receives a `HelloResponse` message:</span></span>

[!code-proto[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="ea5ac-117">Добавьте файл .proto C\# приложения</span><span class="sxs-lookup"><span data-stu-id="ea5ac-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="ea5ac-118">*.Proto* файл включен в проекте путем добавления его в `<Protobuf>` группы элементов:</span><span class="sxs-lookup"><span data-stu-id="ea5ac-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="ea5ac-119">C#Поддержка средств для .proto файлов</span><span class="sxs-lookup"><span data-stu-id="ea5ac-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="ea5ac-120">Пакет средств [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) необходимых для формирования C# ресурсы из *.proto* файлов.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="ea5ac-121">Созданных ресурсов (файлов):</span><span class="sxs-lookup"><span data-stu-id="ea5ac-121">The generated assets (files):</span></span>

* <span data-ttu-id="ea5ac-122">Создаются по мере необходимости каждый раз, когда проект будет собран.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="ea5ac-123">Не добавлен в проект или в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="ea5ac-124">Являются артефакт сборки, содержащиеся в *obj* каталога.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="ea5ac-125">Этот пакет необходим для клиентских и серверных проектов.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="ea5ac-126">`Grpc.Tools` можно добавить с помощью диспетчера пакетов в Visual Studio или добавление `<PackageReference>` в файл проекта:</span><span class="sxs-lookup"><span data-stu-id="ea5ac-126">`Grpc.Tools` can be added by using the Package Manager in Visual Studio or adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=17)]

<span data-ttu-id="ea5ac-127">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-127">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

## <a name="generated-c-assets"></a><span data-ttu-id="ea5ac-128">Созданный C# активы</span><span class="sxs-lookup"><span data-stu-id="ea5ac-128">Generated C# assets</span></span>

<span data-ttu-id="ea5ac-129">Создает пакет средств C# типы, представляющие сообщения, определенные в включенные *.proto* файлов.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-129">The tooling package generates the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="ea5ac-130">Для ресурсов на сервере создается абстрактный базовый тип службы.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-130">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="ea5ac-131">Базовый тип содержит определения всех вызовов gRPC содержащихся в *.proto* файла.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-131">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="ea5ac-132">Создайте реализацию конкретная служба, которая наследуется от этого базового типа и реализует логику для вызовов gRPC.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-132">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="ea5ac-133">Для `greet.proto`, пример было сказано ранее, абстрактный `GreeterBase` тип, содержащий виртуальный `SayHello` метод создается.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-133">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="ea5ac-134">Конкретная реализация `GreeterService` переопределяет метод и реализует логику обработки вызова gRPC.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-134">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="ea5ac-135">Для ресурсов на стороне клиента создается тип конкретного клиента.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-135">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="ea5ac-136">Вызывает gRPC *.proto* файл преобразуются в методы в конкретный тип, который можно использовать.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-136">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="ea5ac-137">Для `greet.proto`, пример было сказано ранее, устойчивый `GreeterClient` создан тип.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-137">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="ea5ac-138">Вызовите `GreeterClient.SayHello` инициировать gRPC вызов на сервер.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-138">Call `GreeterClient.SayHello` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

<span data-ttu-id="ea5ac-139">По умолчанию, серверных и клиентских средств будут сгенерированы для каждой *.proto* файл, включенный в `<Protobuf>` группу элементов.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-139">By default, server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="ea5ac-140">Чтобы обеспечить только серверные ресурсы создаются в проекте сервера `GrpcServices` атрибут имеет значение `Server`.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-140">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

<span data-ttu-id="ea5ac-141">Аналогично, атрибут задается `Client` в клиентские проекты.</span><span class="sxs-lookup"><span data-stu-id="ea5ac-141">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea5ac-142">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ea5ac-142">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
