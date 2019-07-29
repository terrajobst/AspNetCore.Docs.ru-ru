---
title: Проверка подлинности и авторизация в gRPC для ASP.NET Core
author: jamesnk
description: Узнайте, как использовать проверку подлинности и авторизацию в gRPC для ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 07/26/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 34f7f8a5a22159329b3d6c4524943434c460c7fb
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602425"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="9f686-103">Проверка подлинности и авторизация в gRPC для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f686-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="9f686-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="9f686-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="9f686-105">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(как скачать)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="9f686-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="9f686-106">Проверка подлинности пользователей, вызывающих службу gRPC</span><span class="sxs-lookup"><span data-stu-id="9f686-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="9f686-107">gRPC можно использовать с проверкой подлинности [ASP.NET Core](xref:security/authentication/identity) , чтобы связать пользователя с каждым вызовом.</span><span class="sxs-lookup"><span data-stu-id="9f686-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="9f686-108">Ниже приведен пример `Startup.Configure` использования проверки подлинности gRPC и ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f686-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        routes.MapGrpcService<GreeterService>();
    });
}
```

> [!NOTE]
> <span data-ttu-id="9f686-109">Порядок, в котором регистрируется ASP.NET Core по промежуточного слоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="9f686-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="9f686-110">Всегда `UseAuthentication` вызывайте `UseAuthorization` и `UseRouting` After и `UseEndpoints`before.</span><span class="sxs-lookup"><span data-stu-id="9f686-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="9f686-111">После настройки проверки подлинности доступ к пользователю можно получить в методах службы gRPC через `ServerCallContext`.</span><span class="sxs-lookup"><span data-stu-id="9f686-111">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="9f686-112">Проверка подлинности токена носителя</span><span class="sxs-lookup"><span data-stu-id="9f686-112">Bearer token authentication</span></span>

<span data-ttu-id="9f686-113">Клиент может предоставить маркер доступа для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="9f686-113">The client can provide an access token for authentication.</span></span> <span data-ttu-id="9f686-114">Сервер проверяет маркер и использует его для обнаружения пользователя.</span><span class="sxs-lookup"><span data-stu-id="9f686-114">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="9f686-115">На сервере проверка подлинности токена носителя настраивается с помощью по [промежуточного слоя JWT Bearer](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="9f686-115">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="9f686-116">В клиенте .NET gRPC маркер можно отправить с помощью вызовов в виде заголовка:</span><span class="sxs-lookup"><span data-stu-id="9f686-116">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

```csharp
public bool DoAuthenticatedCall(
    Ticketer.TicketerClient client, string token)
{
    var headers = new Metadata();
    headers.Add("Authorization", $"Bearer {token}");

    var request = new BuyTicketsRequest { Count = 1 };
    var response = await client.BuyTicketsAsync(request, headers);

    return response.Success;
}
```

### <a name="client-certificate-authentication"></a><span data-ttu-id="9f686-117">Проверка подлинности сертификата клиента</span><span class="sxs-lookup"><span data-stu-id="9f686-117">Client certificate authentication</span></span>

<span data-ttu-id="9f686-118">Клиент может также предоставить сертификат клиента для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="9f686-118">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="9f686-119">[Проверка подлинности сертификата](https://tools.ietf.org/html/rfc5246#section-7.4.4) выполняется на уровне TLS, прежде чем он когда-либо получит ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f686-119">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="9f686-120">Когда запрос переходит ASP.NET Core, [пакет проверки подлинности сертификата клиента](xref:security/authentication/certauth) позволяет разрешить сертификат в `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="9f686-120">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="9f686-121">Узел должен быть настроен для приема сертификатов клиентов.</span><span class="sxs-lookup"><span data-stu-id="9f686-121">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="9f686-122">Сведения о принятии сертификатов клиентов в Kestrel, IIS и Azure см. в статье [Настройка узла для запроса сертификатов](xref:security/authentication/certauth#configure-your-host-to-require-certificates) .</span><span class="sxs-lookup"><span data-stu-id="9f686-122">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="9f686-123">В клиенте .NET gRPC сертификат клиента добавляется `HttpClientHandler` в, который затем используется для создания клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="9f686-123">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC client
    var httpClient = new HttpClient(handler);
    httpClient.BaseAddress = new Uri(baseAddress);

    return GrpcClient.Create<Ticketer.TicketerClient>(httpClient);
}
```

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="9f686-124">Другие механизмы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9f686-124">Other authentication mechanisms</span></span>

<span data-ttu-id="9f686-125">Многие ASP.NET Core поддерживаемые механизмы проверки подлинности работают с gRPC:</span><span class="sxs-lookup"><span data-stu-id="9f686-125">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="9f686-126">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f686-126">Azure Active Directory</span></span>
* <span data-ttu-id="9f686-127">Сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="9f686-127">Client Certificate</span></span>
* <span data-ttu-id="9f686-128">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="9f686-128">IdentityServer</span></span>
* <span data-ttu-id="9f686-129">Токен JWT</span><span class="sxs-lookup"><span data-stu-id="9f686-129">JWT Token</span></span>
* <span data-ttu-id="9f686-130">OAuth 2,0</span><span class="sxs-lookup"><span data-stu-id="9f686-130">OAuth 2.0</span></span>
* <span data-ttu-id="9f686-131">OpenID Connect подключение</span><span class="sxs-lookup"><span data-stu-id="9f686-131">OpenID Connect</span></span>
* <span data-ttu-id="9f686-132">WS-Federation</span><span class="sxs-lookup"><span data-stu-id="9f686-132">WS-Federation</span></span>

<span data-ttu-id="9f686-133">Дополнительные сведения о настройке проверки подлинности на сервере см. в разделе [ASP.NET Core Authentication](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="9f686-133">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="9f686-134">Настройка клиента gRPC для использования проверки подлинности будет зависеть от используемого механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="9f686-134">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="9f686-135">В предыдущем примере маркера носителя и сертификата клиента показаны несколько способов настройки клиента gRPC для отправки метаданных проверки подлинности с помощью вызовов gRPC:</span><span class="sxs-lookup"><span data-stu-id="9f686-135">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="9f686-136">Строго типизированные клиенты gRPC `HttpClient` используют внутреннее.</span><span class="sxs-lookup"><span data-stu-id="9f686-136">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="9f686-137">Проверку подлинности можно [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)настроить для или путем добавления [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) пользовательских экземпляров `HttpClient`в.</span><span class="sxs-lookup"><span data-stu-id="9f686-137">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="9f686-138">Каждый вызов gRPC имеет необязательный `CallOptions` аргумент.</span><span class="sxs-lookup"><span data-stu-id="9f686-138">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="9f686-139">Пользовательские заголовки можно отправлять с помощью коллекции заголовков параметра.</span><span class="sxs-lookup"><span data-stu-id="9f686-139">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="9f686-140">Проверка подлинности Windows (NTLM/Kerberos/Negotiate) не может использоваться с gRPC.</span><span class="sxs-lookup"><span data-stu-id="9f686-140">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="9f686-141">для gRPC требуется HTTP/2, а HTTP/2 не поддерживает проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="9f686-141">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="9f686-142">Авторизация пользователей для доступа к службам и методам служб</span><span class="sxs-lookup"><span data-stu-id="9f686-142">Authorize users to access services and service methods</span></span>

<span data-ttu-id="9f686-143">По умолчанию все методы в службе могут вызываться пользователями, не прошедшими проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="9f686-143">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="9f686-144">Чтобы требовать проверку подлинности, примените к службе атрибут [[авторизовать]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) :</span><span class="sxs-lookup"><span data-stu-id="9f686-144">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="9f686-145">Можно использовать аргументы конструктора и свойства `[Authorize]` атрибута, чтобы ограничить доступ только тем пользователям, которые соответствуют определенным политикам [авторизации](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="9f686-145">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="9f686-146">Например, если имеется пользовательская политика `MyAuthorizationPolicy`авторизации, убедитесь, что только пользователи, соответствующие этой политике, могут получить доступ к службе, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="9f686-146">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="9f686-147">К отдельным методам службы также `[Authorize]` может применяться атрибут.</span><span class="sxs-lookup"><span data-stu-id="9f686-147">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="9f686-148">Если текущий пользователь не соответствует политикам, примененным **как** к методу, так и к классу, вызывающему объекту будет возвращена ошибка:</span><span class="sxs-lookup"><span data-stu-id="9f686-148">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
    public override Task<AvailableTicketsResponse> GetAvailableTickets(
        Empty request, ServerCallContext context)
    {
        // ... buy tickets for the current user ...
    }

    [Authorize("Administrators")]
    public override Task<BuyTicketsResponse> RefundTickets(
        BuyTicketsRequest request, ServerCallContext context)
    {
        // ... refund tickets (something only Administrators can do) ..
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="9f686-149">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9f686-149">Additional resources</span></span>

* [<span data-ttu-id="9f686-150">Проверка подлинности токена носителя в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f686-150">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="9f686-151">Настройка проверки подлинности сертификата клиента в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f686-151">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
