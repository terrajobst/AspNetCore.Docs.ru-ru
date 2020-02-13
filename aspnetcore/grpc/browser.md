---
title: Использование gRPC в приложениях на основе браузера
author: jamesnk
description: Узнайте, как настроить gRPC Services на ASP.NET Core для вызова из приложений браузера с помощью gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/10/2020
uid: grpc/browser
ms.openlocfilehash: 333fc8c4277bbac47042d4904c276e963186914a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172276"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="be6f2-103">Использование gRPC в приложениях на основе браузера</span><span class="sxs-lookup"><span data-stu-id="be6f2-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="be6f2-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="be6f2-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be6f2-105">**gRPC — веб-поддержка в .NET — экспериментальная**</span><span class="sxs-lookup"><span data-stu-id="be6f2-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="be6f2-106">gRPC-Web для .NET — это экспериментальный проект, а не фиксированный продукт.</span><span class="sxs-lookup"><span data-stu-id="be6f2-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="be6f2-107">Мы хотим:</span><span class="sxs-lookup"><span data-stu-id="be6f2-107">We want to:</span></span>
>
> * <span data-ttu-id="be6f2-108">Проверьте, что наш подход к реализации gRPC-Web работает.</span><span class="sxs-lookup"><span data-stu-id="be6f2-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="be6f2-109">Получите отзыв о том, если этот подход полезен разработчикам .NET по сравнению с традиционным способом настройки gRPC-Web через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="be6f2-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="be6f2-110">Оставьте отзыв на [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) , чтобы убедиться в том, что мы создаем что-то, что и для разработчиков.</span><span class="sxs-lookup"><span data-stu-id="be6f2-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="be6f2-111">Невозможно вызвать службу HTTP/2 gRPC из приложения на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="be6f2-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="be6f2-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) — это протокол, позволяющий браузеру JavaScript и приложениям блазор вызывать службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="be6f2-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="be6f2-113">В этой статье объясняется, как использовать gRPC-Web в .NET Core.</span><span class="sxs-lookup"><span data-stu-id="be6f2-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="be6f2-114">Настройка gRPC-Web в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be6f2-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="be6f2-115">службы gRPC, размещенные в ASP.NET Core, можно настроить для поддержки gRPC-Web вместе с HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="be6f2-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="be6f2-116">gRPC — веб-сайт не требует изменений в службах.</span><span class="sxs-lookup"><span data-stu-id="be6f2-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="be6f2-117">Единственным изменением является конфигурация запуска.</span><span class="sxs-lookup"><span data-stu-id="be6f2-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="be6f2-118">Чтобы включить gRPC-Web со службой ASP.NET Core gRPC, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="be6f2-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="be6f2-119">Добавьте ссылку на пакет [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="be6f2-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="be6f2-120">Настройте приложение для использования gRPC-Web, добавив `AddGrpcWeb` и `UseGrpcWeb` в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="be6f2-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="be6f2-121">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="be6f2-121">The preceding code:</span></span>

* <span data-ttu-id="be6f2-122">Добавляет по промежуточного слоя gRPC-Web, `UseGrpcWeb`, после маршрутизации и перед конечными точками.</span><span class="sxs-lookup"><span data-stu-id="be6f2-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="be6f2-123">Указывает метод `endpoints.MapGrpcService<GreeterService>()` поддерживает gRPC-Web с `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="be6f2-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="be6f2-124">Кроме того, можно настроить все службы для поддержки gRPC-Web, добавив `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` в ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="be6f2-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

<span data-ttu-id="be6f2-125">Для вызова gRPC-Web из браузера может потребоваться дополнительная настройка, например настройка ASP.NET Core для поддержки CORS.</span><span class="sxs-lookup"><span data-stu-id="be6f2-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="be6f2-126">Дополнительные сведения см. в разделе [Поддержка CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="be6f2-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="be6f2-127">Вызов gRPC-Web из браузера</span><span class="sxs-lookup"><span data-stu-id="be6f2-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="be6f2-128">Приложения браузера могут использовать gRPC-Web для вызова служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="be6f2-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="be6f2-129">При вызове gRPC Services с помощью gRPC-Web из браузера существуют некоторые требования и ограничения.</span><span class="sxs-lookup"><span data-stu-id="be6f2-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="be6f2-130">Сервер должен быть настроен для поддержки gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="be6f2-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="be6f2-131">Потоковая передача клиента и вызовы двунаправленной потоковой передачи не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="be6f2-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="be6f2-132">Поддерживается потоковая передача сервера.</span><span class="sxs-lookup"><span data-stu-id="be6f2-132">Server streaming is supported.</span></span>
* <span data-ttu-id="be6f2-133">Для вызова служб gRPC в другом домене необходимо настроить [CORS](xref:security/cors) на сервере.</span><span class="sxs-lookup"><span data-stu-id="be6f2-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="be6f2-134">JavaScript gRPC — веб-клиент</span><span class="sxs-lookup"><span data-stu-id="be6f2-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="be6f2-135">Существует клиент JavaScript gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="be6f2-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="be6f2-136">Инструкции по использованию gRPC-Web из JavaScript см. в статье [написание кода клиента JavaScript с помощью gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="be6f2-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="be6f2-137">Настройка gRPC-Web с помощью клиента .NET gRPC</span><span class="sxs-lookup"><span data-stu-id="be6f2-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="be6f2-138">Клиент .NET gRPC можно настроить для выполнения вызовов gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="be6f2-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="be6f2-139">Это полезно для Блазор приложений веб- [сборки](xref:blazor/index#blazor-webassembly) , которые размещаются в браузере и имеют те же ограничения HTTP, что и код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="be6f2-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="be6f2-140">Вызов gRPC-Web с клиентом .NET аналогичен [http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="be6f2-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="be6f2-141">Единственным изменением является то, как создается канал.</span><span class="sxs-lookup"><span data-stu-id="be6f2-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="be6f2-142">Чтобы использовать gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="be6f2-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="be6f2-143">Добавьте ссылку на пакет [GRPC .NET. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="be6f2-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="be6f2-144">Убедитесь, что ссылка на [GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) Package находится в 2.27.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="be6f2-144">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="be6f2-145">Настройте канал для использования `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="be6f2-145">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="be6f2-146">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="be6f2-146">The preceding code:</span></span>

* <span data-ttu-id="be6f2-147">Настраивает канал для использования gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="be6f2-147">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="be6f2-148">Создает клиент и выполняет вызов с помощью канала.</span><span class="sxs-lookup"><span data-stu-id="be6f2-148">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="be6f2-149">При создании `GrpcWebHandler` имеет следующие параметры конфигурации:</span><span class="sxs-lookup"><span data-stu-id="be6f2-149">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="be6f2-150">**Иннерхандлер**: базовый <xref:System.Net.Http.HttpMessageHandler>, который делает HTTP-запрос gRPC, например `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="be6f2-150">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="be6f2-151">**Mode**: тип перечисления, указывающий, `Content-Type` ли запрос HTTP-запроса gRPC `application/grpc-web` или `application/grpc-web-text`.</span><span class="sxs-lookup"><span data-stu-id="be6f2-151">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="be6f2-152">`GrpcWebMode.GrpcWeb` настраивает содержимое для отправки без кодирования.</span><span class="sxs-lookup"><span data-stu-id="be6f2-152">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="be6f2-153">Значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="be6f2-153">Default value.</span></span>
    * <span data-ttu-id="be6f2-154">`GrpcWebMode.GrpcWebText` настраивает содержимое в кодировке Base64.</span><span class="sxs-lookup"><span data-stu-id="be6f2-154">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="be6f2-155">Требуется для вызовов потоковой передачи сервера в браузерах.</span><span class="sxs-lookup"><span data-stu-id="be6f2-155">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="be6f2-156">**Хттпверсион**: протокол HTTP `Version` используется для задания [HttpRequestMessage. Version](xref:System.Net.Http.HttpRequestMessage.Version) в базовом HTTP-запросе gRPC.</span><span class="sxs-lookup"><span data-stu-id="be6f2-156">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="be6f2-157">gRPC-Web не требует определенной версии и не переопределяет значение по умолчанию, если не указано иное.</span><span class="sxs-lookup"><span data-stu-id="be6f2-157">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be6f2-158">Созданные клиенты gRPC имеют синхронные и асинхронные методы для вызова унарных методов.</span><span class="sxs-lookup"><span data-stu-id="be6f2-158">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="be6f2-159">Например, `SayHello` синхронизируется и `SayHelloAsync` является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="be6f2-159">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="be6f2-160">Вызов метода Sync в приложении Блазор сборки приведет к тому, что приложение перестанет отвечать на запросы.</span><span class="sxs-lookup"><span data-stu-id="be6f2-160">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="be6f2-161">Асинхронные методы всегда должны использоваться в Блазор.</span><span class="sxs-lookup"><span data-stu-id="be6f2-161">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be6f2-162">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="be6f2-162">Additional resources</span></span>

* [<span data-ttu-id="be6f2-163">проект GitHub gRPC для веб-клиентов</span><span class="sxs-lookup"><span data-stu-id="be6f2-163">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
