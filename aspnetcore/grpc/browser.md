---
title: Использование gRPC в приложениях на основе браузера
author: jamesnk
description: Узнайте, как настроить службы gRPC на платформе ASP.NET Core для вызова из приложений браузера с помощью gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/16/2020
uid: grpc/browser
ms.openlocfilehash: 3beeffc26ffd3c2dc85bfc22a46d97d5fd78d3d0
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78649420"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="4392e-103">Использование gRPC в приложениях на основе браузера</span><span class="sxs-lookup"><span data-stu-id="4392e-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="4392e-104">Автор: [Джеймс Ньютон-Кинг](https://twitter.com/jamesnk) (James Newton-King)</span><span class="sxs-lookup"><span data-stu-id="4392e-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4392e-105">**Поддержка gRPC-Web реализуется в .NET в экспериментальном режиме**</span><span class="sxs-lookup"><span data-stu-id="4392e-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="4392e-106">gRPC-Web для .NET — это экспериментальный проект, а не готовый продукт.</span><span class="sxs-lookup"><span data-stu-id="4392e-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="4392e-107">Наши задачи:</span><span class="sxs-lookup"><span data-stu-id="4392e-107">We want to:</span></span>
>
> * <span data-ttu-id="4392e-108">Проверить, что наш подход к реализации gRPC-Web работает.</span><span class="sxs-lookup"><span data-stu-id="4392e-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="4392e-109">Получить отзыв о том, был ли этот подход полезен разработчикам .NET в сравнении с традиционным способом настройки gRPC-Web через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="4392e-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="4392e-110">Оставьте отзыв на сайте [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet), чтобы мы понимали, что создаем полезный и эффективный продукт для разработчиков.</span><span class="sxs-lookup"><span data-stu-id="4392e-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="4392e-111">Вызвать службу HTTP/2 gRPC из приложения на основе браузера невозможно.</span><span class="sxs-lookup"><span data-stu-id="4392e-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="4392e-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) — это протокол, позволяющий приложениям JavaScript и Blazor на основе браузера вызывать службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="4392e-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="4392e-113">В этой статье описывается использование gRPC-Web в .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4392e-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="4392e-114">Настройка gRPC-Web в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4392e-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="4392e-115">Службы gRPC, размещенные в ASP.NET Core, можно настроить на поддержку gRPC-Web вместе с HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="4392e-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="4392e-116">gRPC-Web не требует вносить изменения в службы.</span><span class="sxs-lookup"><span data-stu-id="4392e-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="4392e-117">Единственного изменения потребует конфигурация запуска.</span><span class="sxs-lookup"><span data-stu-id="4392e-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="4392e-118">Чтобы включить gRPC-Web со службой gRPC ASP.NET Core, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4392e-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="4392e-119">Добавьте ссылку на пакет [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web).</span><span class="sxs-lookup"><span data-stu-id="4392e-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="4392e-120">Настройте приложение на использование gRPC-Web, добавив `AddGrpcWeb` и `UseGrpcWeb` в файл *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4392e-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="4392e-121">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="4392e-121">The preceding code:</span></span>

* <span data-ttu-id="4392e-122">Добавляет ПО промежуточного слоя gRPC-Web (`UseGrpcWeb`) после маршрутизации и перед конечными точками.</span><span class="sxs-lookup"><span data-stu-id="4392e-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="4392e-123">Указывает метод `endpoints.MapGrpcService<GreeterService>()` с поддержкой gRPC-Web с `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="4392e-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="4392e-124">Кроме того, можно настроить все службы на поддержку gRPC-Web, добавив `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` в ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="4392e-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

### <a name="grpc-web-and-cors"></a><span data-ttu-id="4392e-125">gRPC-Web и CORS</span><span class="sxs-lookup"><span data-stu-id="4392e-125">gRPC-Web and CORS</span></span>

<span data-ttu-id="4392e-126">Система безопасности браузера предотвращает запросы веб-страницы к другому домену, отличному от того, который обслуживает веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="4392e-126">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="4392e-127">Это ограничение применяется к вызовам gRPC-Web с приложениями браузера.</span><span class="sxs-lookup"><span data-stu-id="4392e-127">This restriction applies to making gRPC-Web calls with browser apps.</span></span> <span data-ttu-id="4392e-128">Например, приложение браузера, обслуживаемое `https://www.contoso.com`, блокирует вызов служб gRPC-Web, размещенных на `https://services.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="4392e-128">For example, a browser app served by `https://www.contoso.com` is blocked from calling gRPC-Web services hosted on `https://services.contoso.com`.</span></span> <span data-ttu-id="4392e-129">Можно использовать общий доступ к ресурсам независимо от источника (CORS), чтобы ослабить это ограничение.</span><span class="sxs-lookup"><span data-stu-id="4392e-129">Cross Origin Resource Sharing (CORS) can be used to relax this restriction.</span></span>

<span data-ttu-id="4392e-130">Чтобы разрешить приложению браузера выполнять вызовы gRPC-Web независимо от источника, настройте [CORS в ASP.NET Core](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="4392e-130">To allow your browser app to make cross-origin gRPC-Web calls, set up [CORS in ASP.NET Core](xref:security/cors).</span></span> <span data-ttu-id="4392e-131">Используйте встроенную поддержку CORS и предоставьте заголовки, относящиеся к gRPC, с помощью <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="4392e-131">Use the built-in CORS support, and expose gRPC-specific headers with <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span></span>

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

<span data-ttu-id="4392e-132">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="4392e-132">The preceding code:</span></span>

* <span data-ttu-id="4392e-133">Вызывает `AddCors` для добавления служб CORS и настраивает политику CORS, которая предоставляет заголовки, относящиеся к gRPC.</span><span class="sxs-lookup"><span data-stu-id="4392e-133">Calls `AddCors` to add CORS services and configures a CORS policy that exposes gRPC-specific headers.</span></span>
* <span data-ttu-id="4392e-134">Вызывает `UseCors` для добавления ПО промежуточного слоя CORS после маршрутизации и перед конечными точками.</span><span class="sxs-lookup"><span data-stu-id="4392e-134">Calls `UseCors` to add the CORS middleware after routing and before endpoints.</span></span>
* <span data-ttu-id="4392e-135">Указывает метод `endpoints.MapGrpcService<GreeterService>()` с поддержкой CORS с `RequiresCors`.</span><span class="sxs-lookup"><span data-stu-id="4392e-135">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports CORS with `RequiresCors`.</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="4392e-136">Вызов gRPC-Web из браузера</span><span class="sxs-lookup"><span data-stu-id="4392e-136">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="4392e-137">Приложения браузера могут использовать gRPC-Web для вызова служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="4392e-137">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="4392e-138">При вызове служб gRPC с помощью gRPC-Web из браузера существует ряд требований и ограничений.</span><span class="sxs-lookup"><span data-stu-id="4392e-138">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="4392e-139">Сервер должен быть настроен для поддержки gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="4392e-139">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="4392e-140">Потоковая передача клиента и вызовы двунаправленной потоковой передачи не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="4392e-140">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="4392e-141">Потоковая передача сервера поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4392e-141">Server streaming is supported.</span></span>
* <span data-ttu-id="4392e-142">Для вызова служб gRPC в другом домене требуется настроить [CORS](xref:security/cors) на сервере.</span><span class="sxs-lookup"><span data-stu-id="4392e-142">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="4392e-143">Клиент gRPC-Web JavaScript</span><span class="sxs-lookup"><span data-stu-id="4392e-143">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="4392e-144">Существует клиент gRPC-Web JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4392e-144">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="4392e-145">Инструкции по использованию gRPC-Web из JavaScript см. в статье, посвященной [написанию кода клиента JavaScript с gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="4392e-145">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="4392e-146">Настройка gRPC-Web с помощью клиента .NET gRPC</span><span class="sxs-lookup"><span data-stu-id="4392e-146">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="4392e-147">Клиент .NET gRPC можно настроить для выполнения вызовов gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="4392e-147">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="4392e-148">Это полезно для приложений [Blazor WebAssembly](xref:blazor/index#blazor-webassembly), которые размещаются в браузере и имеют те же ограничения HTTP, что и код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4392e-148">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="4392e-149">Вызов gRPC-Web с помощью клиента .NET выполняется [так же, как и HTTP/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="4392e-149">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="4392e-150">Единственным изменением является то, как создается канал.</span><span class="sxs-lookup"><span data-stu-id="4392e-150">The only modification is how the channel is created.</span></span>

<span data-ttu-id="4392e-151">Чтобы использовать gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="4392e-151">To use gRPC-Web:</span></span>

* <span data-ttu-id="4392e-152">Добавьте ссылку на пакет [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web).</span><span class="sxs-lookup"><span data-stu-id="4392e-152">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="4392e-153">Убедитесь, что ссылаетесь на пакет [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) версии 2.27.0 или более поздней.</span><span class="sxs-lookup"><span data-stu-id="4392e-153">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="4392e-154">Настройте канал на использование `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="4392e-154">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="4392e-155">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="4392e-155">The preceding code:</span></span>

* <span data-ttu-id="4392e-156">Настраивает канал для использования gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="4392e-156">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="4392e-157">Создает клиент и выполняет вызов с помощью канала.</span><span class="sxs-lookup"><span data-stu-id="4392e-157">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="4392e-158">При создании `GrpcWebHandler` имеет следующие параметры конфигурации:</span><span class="sxs-lookup"><span data-stu-id="4392e-158">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="4392e-159">**InnerHandler**: базовый <xref:System.Net.Http.HttpMessageHandler>, который выполняет HTTP-запрос gRPC, например `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="4392e-159">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="4392e-160">**Режим**. тип перечисления, указывающий, является ли HTTP-запроса gRPC `Content-Type` запросом `application/grpc-web` или `application/grpc-web-text`.</span><span class="sxs-lookup"><span data-stu-id="4392e-160">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="4392e-161">`GrpcWebMode.GrpcWeb` настраивает содержимое для отправки без кодировки.</span><span class="sxs-lookup"><span data-stu-id="4392e-161">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="4392e-162">Значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4392e-162">Default value.</span></span>
    * <span data-ttu-id="4392e-163">`GrpcWebMode.GrpcWebText` настраивает содержимое в кодировке Base64.</span><span class="sxs-lookup"><span data-stu-id="4392e-163">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="4392e-164">Требуется для вызовов потоковой передачи сервера в браузерах.</span><span class="sxs-lookup"><span data-stu-id="4392e-164">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="4392e-165">**HttpVersion**: `Version` протокола HTTP, используемая для задания [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) в базовом HTTP-запросе gRPC.</span><span class="sxs-lookup"><span data-stu-id="4392e-165">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="4392e-166">gRPC-Web не требует определенной версии и не переопределяет значение по умолчанию, если не указано иное.</span><span class="sxs-lookup"><span data-stu-id="4392e-166">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4392e-167">Созданные клиенты gRPC имеют синхронные и асинхронные методы для вызова унарных методов.</span><span class="sxs-lookup"><span data-stu-id="4392e-167">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="4392e-168">Например, `SayHello` является синхронным, а `SayHelloAsync` — асинхронным.</span><span class="sxs-lookup"><span data-stu-id="4392e-168">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="4392e-169">Вызов синхронного метода в приложении Blazor WebAssembly приведет к тому, что приложение перестанет отвечать на запросы.</span><span class="sxs-lookup"><span data-stu-id="4392e-169">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="4392e-170">В Blazor WebAssembly всегда следует использовать асинхронные методы.</span><span class="sxs-lookup"><span data-stu-id="4392e-170">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4392e-171">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4392e-171">Additional resources</span></span>

* [<span data-ttu-id="4392e-172">Проект GitHub — gRPC для веб-клиентов</span><span class="sxs-lookup"><span data-stu-id="4392e-172">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
