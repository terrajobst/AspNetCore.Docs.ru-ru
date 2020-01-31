---
title: gRPC в приложениях браузера
author: jamesnk
description: Узнайте, как настроить gRPC Services на ASP.NET Core для вызова из приложений браузера с помощью gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/24/2020
uid: grpc/browser
ms.openlocfilehash: 6359c3b76b3cb1ba2b6d9f9a989f64cbf4c4379d
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830636"
---
# <a name="grpc-in-browser-apps"></a><span data-ttu-id="64d54-103">gRPC в приложениях браузера</span><span class="sxs-lookup"><span data-stu-id="64d54-103">gRPC in browser apps</span></span>

<span data-ttu-id="64d54-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="64d54-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64d54-105">**gRPC — веб-поддержка в .NET — экспериментальная**</span><span class="sxs-lookup"><span data-stu-id="64d54-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="64d54-106">gRPC-Web для .NET — это экспериментальный проект, а не фиксированный продукт.</span><span class="sxs-lookup"><span data-stu-id="64d54-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="64d54-107">Мы хотим:</span><span class="sxs-lookup"><span data-stu-id="64d54-107">We want to:</span></span>
>
> * <span data-ttu-id="64d54-108">Проверьте, что наш подход к реализации gRPC-Web работает.</span><span class="sxs-lookup"><span data-stu-id="64d54-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="64d54-109">Получите отзыв о том, если этот подход полезен разработчикам .NET по сравнению с традиционным способом настройки gRPC-Web через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="64d54-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="64d54-110">Оставьте отзыв на [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) , чтобы убедиться в том, что мы создаем что-то, что и для разработчиков.</span><span class="sxs-lookup"><span data-stu-id="64d54-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="64d54-111">Невозможно вызвать службу HTTP/2 gRPC из приложения на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="64d54-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="64d54-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) — это протокол, позволяющий браузеру JavaScript и приложениям блазор вызывать службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="64d54-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="64d54-113">В этой статье объясняется, как использовать gRPC-Web в .NET Core.</span><span class="sxs-lookup"><span data-stu-id="64d54-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="64d54-114">Настройка gRPC-Web в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64d54-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="64d54-115">службы gRPC, размещенные в ASP.NET Core, можно настроить для поддержки gRPC-Web вместе с HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="64d54-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="64d54-116">gRPC — веб-сайт не требует изменений в службах.</span><span class="sxs-lookup"><span data-stu-id="64d54-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="64d54-117">Единственным изменением является конфигурация запуска.</span><span class="sxs-lookup"><span data-stu-id="64d54-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="64d54-118">Чтобы включить gRPC-Web со службой ASP.NET Core gRPC, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="64d54-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="64d54-119">Добавьте ссылку на пакет [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="64d54-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="64d54-120">Настройте приложение для использования gRPC-Web, добавив `AddGrpcWeb` и `UseGrpcWeb` в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="64d54-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=3,10,14)]

<span data-ttu-id="64d54-121">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="64d54-121">The preceding code:</span></span>

* <span data-ttu-id="64d54-122">Добавляет по промежуточного слоя gRPC-Web, `UseGrpcWeb`, после маршрутизации и перед конечными точками.</span><span class="sxs-lookup"><span data-stu-id="64d54-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="64d54-123">Указывает метод `endpoints.MapGrpcService<GreeterService>()` поддерживает gRPC-Web с `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="64d54-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="64d54-124">Кроме того, можно настроить все службы для поддержки gRPC-Web, добавив `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` в ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="64d54-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=5,12,16)]

<span data-ttu-id="64d54-125">Для вызова gRPC-Web из браузера может потребоваться дополнительная настройка, например настройка ASP.NET Core для поддержки CORS.</span><span class="sxs-lookup"><span data-stu-id="64d54-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="64d54-126">Дополнительные сведения см. в разделе [Поддержка CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="64d54-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="64d54-127">Вызов gRPC-Web из браузера</span><span class="sxs-lookup"><span data-stu-id="64d54-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="64d54-128">Приложения браузера могут использовать gRPC-Web для вызова служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="64d54-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="64d54-129">При вызове gRPC Services с помощью gRPC-Web из браузера существуют некоторые требования и ограничения.</span><span class="sxs-lookup"><span data-stu-id="64d54-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="64d54-130">Сервер должен быть настроен для поддержки gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="64d54-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="64d54-131">Потоковая передача клиента и вызовы двунаправленной потоковой передачи не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="64d54-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="64d54-132">Поддерживается потоковая передача сервера.</span><span class="sxs-lookup"><span data-stu-id="64d54-132">Server streaming is supported.</span></span>
* <span data-ttu-id="64d54-133">Для вызова служб gRPC в другом домене необходимо настроить [CORS](xref:security/cors) на сервере.</span><span class="sxs-lookup"><span data-stu-id="64d54-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="64d54-134">JavaScript gRPC — веб-клиент</span><span class="sxs-lookup"><span data-stu-id="64d54-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="64d54-135">Существует клиент JavaScript gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="64d54-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="64d54-136">Инструкции по использованию gRPC-Web из JavaScript см. в статье [написание кода клиента JavaScript с помощью gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="64d54-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="64d54-137">Настройка gRPC-Web с помощью клиента .NET gRPC</span><span class="sxs-lookup"><span data-stu-id="64d54-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="64d54-138">Клиент .NET gRPC можно настроить для выполнения вызовов gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="64d54-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="64d54-139">Это полезно для Блазор приложений веб- [сборки](xref:blazor/index#blazor-webassembly) , которые размещаются в браузере и имеют те же ограничения HTTP, что и код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="64d54-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="64d54-140">Вызов gRPC-Web с клиентом .NET аналогичен [http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="64d54-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="64d54-141">Единственным изменением является то, как создается канал.</span><span class="sxs-lookup"><span data-stu-id="64d54-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="64d54-142">Чтобы использовать gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="64d54-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="64d54-143">Добавьте ссылку на пакет [GRPC .NET. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="64d54-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="64d54-144">Настройте канал для использования `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="64d54-144">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="64d54-145">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="64d54-145">The preceding code:</span></span>

* <span data-ttu-id="64d54-146">Настраивает канал для использования gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="64d54-146">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="64d54-147">Создает клиент и выполняет вызов с помощью канала.</span><span class="sxs-lookup"><span data-stu-id="64d54-147">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="64d54-148">При создании `GrpcWebHandler` имеет следующие параметры конфигурации:</span><span class="sxs-lookup"><span data-stu-id="64d54-148">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="64d54-149">**Иннерхандлер**: базовый <xref:System.Net.Http.HttpMessageHandler>, который выполняет HTTP-вызов, например `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="64d54-149">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the HTTP call, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="64d54-150">**Mode**: `GrpcWebMode` Enum.</span><span class="sxs-lookup"><span data-stu-id="64d54-150">**Mode**: `GrpcWebMode` enum.</span></span> <span data-ttu-id="64d54-151">`GrpcWebMode.GrpcWebText` настраивает содержимое в кодировке Base64, которое требуется для поддержки вызовов потоковой передачи сервера.</span><span class="sxs-lookup"><span data-stu-id="64d54-151">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded, which is required to support server streaming calls.</span></span>
* <span data-ttu-id="64d54-152">**Хттпверсион**: `Version`протокола HTTP.</span><span class="sxs-lookup"><span data-stu-id="64d54-152">**HttpVersion**: HTTP protocol `Version`.</span></span> <span data-ttu-id="64d54-153">gRPC-Web не требует определенного протокола и не укажет его при выполнении запроса, если не настроено.</span><span class="sxs-lookup"><span data-stu-id="64d54-153">gRPC-Web doesn't require a specific protocol and won't specify one when making a request unless configured.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64d54-154">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="64d54-154">Additional resources</span></span>

* [<span data-ttu-id="64d54-155">проект GitHub gRPC для веб-клиентов</span><span class="sxs-lookup"><span data-stu-id="64d54-155">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
