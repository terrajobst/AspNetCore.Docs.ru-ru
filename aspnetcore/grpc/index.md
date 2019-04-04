---
title: Введение в gRPC на платформе ASP.NET Core
author: juntaoluo
description: Узнайте об использовании служб gRPC с сервером Kestrel и стеком ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
ms.openlocfilehash: dd1c42744bfda965df91ea1fcc0b71814317b969
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142423"
---
# <a name="introduction-to-grpc-on-aspnet-core"></a><span data-ttu-id="879b2-103">Введение в gRPC на платформе ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="879b2-103">Introduction to gRPC on ASP.NET Core</span></span>

<span data-ttu-id="879b2-104">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="879b2-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="879b2-105">[gRPC](https://grpc.io/docs/guides/) — это не зависящая от языка высокопроизводительная платформа удаленного вызова процедур (RPC).</span><span class="sxs-lookup"><span data-stu-id="879b2-105">[gRPC](https://grpc.io/docs/guides/) is a language agnostic, high-performance Remote Procedure Call (RPC) framework.</span></span> <span data-ttu-id="879b2-106">Дополнительные сведения об основах gRPC см. на [странице документации по gRPC](https://grpc.io/docs/).</span><span class="sxs-lookup"><span data-stu-id="879b2-106">For more on gRPC fundamentals, see the [gRPC documentation page](https://grpc.io/docs/).</span></span>

<span data-ttu-id="879b2-107">Ниже приведены основные преимущества gRPC.</span><span class="sxs-lookup"><span data-stu-id="879b2-107">The main benefits of gRPC are:</span></span>
* <span data-ttu-id="879b2-108">Современная высокопроизводительная упрощенная платформа RPC.</span><span class="sxs-lookup"><span data-stu-id="879b2-108">Modern high-performance lightweight RPC framework.</span></span>
* <span data-ttu-id="879b2-109">Разработка API по модели "сначала контракт" с использованием механизма Protocol Buffers по умолчанию, что позволяет выпускать не зависящие от языка реализации.</span><span class="sxs-lookup"><span data-stu-id="879b2-109">Contract-first API development, using Protocol Buffers by default, allowing for language agnostic implementations.</span></span>
* <span data-ttu-id="879b2-110">Доступные для многих языков инструменты, предназначенные для создания строго типизированных серверов и клиентов.</span><span class="sxs-lookup"><span data-stu-id="879b2-110">Tooling available for many languages to generate strongly-typed servers and clients.</span></span>
* <span data-ttu-id="879b2-111">Поддержка клиентских, серверных и двунаправленных потоковых вызовов.</span><span class="sxs-lookup"><span data-stu-id="879b2-111">Supports client, server, and bi-directional streaming calls.</span></span>
* <span data-ttu-id="879b2-112">Снижение уровня использования сети за счет двоичной сериализации Protobuf.</span><span class="sxs-lookup"><span data-stu-id="879b2-112">Reduced network usage with Protobuf binary serialization.</span></span>

<span data-ttu-id="879b2-113">Благодаря этим преимуществам gRPC идеально подходит для:</span><span class="sxs-lookup"><span data-stu-id="879b2-113">These benefits make gRPC ideal for:</span></span>
* <span data-ttu-id="879b2-114">упрощенных микрослужб, где важна эффективность;</span><span class="sxs-lookup"><span data-stu-id="879b2-114">Lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="879b2-115">многоязычных систем, где для разработки требуется несколько языков;</span><span class="sxs-lookup"><span data-stu-id="879b2-115">Polyglot systems where multiple languages are required for development.</span></span>
* <span data-ttu-id="879b2-116">работающих в режиме реального времени служб типа "точка-точка", которые должны обрабатывать запросы и ответы потоковой передачи данных.</span><span class="sxs-lookup"><span data-stu-id="879b2-116">Point-to-point real-time services that need to handle streaming requests or responses.</span></span>

<span data-ttu-id="879b2-117">Несмотря на то, что сейчас реализация C# доступна на официальной [странице gRPC](https://grpc.io/docs/quickstart/csharp.html), текущая реализация зависит от собственной библиотеки, написанной на языке C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span><span class="sxs-lookup"><span data-stu-id="879b2-117">While a C# implementation is currently available on the official [gRPC page](https://grpc.io/docs/quickstart/csharp.html), the current implementation relies on the native library written in C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span></span> <span data-ttu-id="879b2-118">В настоящее время ведется работа по выпуску новой полностью управляемой реализации на основе сервера HTTP Kestrel и стека ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="879b2-118">Work is currently in progress to provide a new implementation based on the Kestrel HTTP server and the ASP.NET Core stack that is fully managed.</span></span> <span data-ttu-id="879b2-119">Вводные сведения об использовании этой реализации для создания служб gRPC можно найти в приведенных ниже документах.</span><span class="sxs-lookup"><span data-stu-id="879b2-119">The following documents provide an introduction to building gRPC services with this new implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="879b2-120">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="879b2-120">Additional resources</span></span>

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>