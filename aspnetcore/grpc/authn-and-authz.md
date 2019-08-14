---
title: Проверка подлинности и авторизация в gRPC для ASP.NET Core
author: jamesnk
description: Узнайте, как использовать проверку подлинности и авторизацию в gRPC для ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/13/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 19018c4ffae1228055a4858b496f135d015625b4
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993289"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="aae2e-103">Проверка подлинности и авторизация в gRPC для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aae2e-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="aae2e-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="aae2e-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="aae2e-105">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(как скачать)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="aae2e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="aae2e-106">Проверка подлинности пользователей, вызывающих службу gRPC</span><span class="sxs-lookup"><span data-stu-id="aae2e-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="aae2e-107">gRPC можно использовать с проверкой подлинности [ASP.NET Core](xref:security/authentication/identity) , чтобы связать пользователя с каждым вызовом.</span><span class="sxs-lookup"><span data-stu-id="aae2e-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="aae2e-108">Ниже приведен пример `Startup.Configure` использования проверки подлинности gRPC и ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aae2e-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="aae2e-109">Порядок, в котором регистрируется ASP.NET Core по промежуточного слоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="aae2e-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="aae2e-110">Всегда `UseAuthentication` вызывайте `UseAuthorization` и `UseRouting` After и `UseEndpoints`before.</span><span class="sxs-lookup"><span data-stu-id="aae2e-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="aae2e-111">Механизм проверки подлинности, используемый приложением во время вызова, необходимо настроить.</span><span class="sxs-lookup"><span data-stu-id="aae2e-111">The authentication mechanism your app uses during a call needs to be configured.</span></span> <span data-ttu-id="aae2e-112">Конфигурация проверки подлинности `Startup.ConfigureServices` добавляется в и будет отличаться в зависимости от используемого в приложении механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="aae2e-112">Authentication configuration is added in `Startup.ConfigureServices` and will be different depending upon the authentication mechanism your app uses.</span></span> <span data-ttu-id="aae2e-113">Примеры защиты приложений ASP.NET Core см. в статье [примеры проверки](xref:security/authentication/samples)подлинности.</span><span class="sxs-lookup"><span data-stu-id="aae2e-113">For examples of how to secure ASP.NET Core apps, see [Authentication samples](xref:security/authentication/samples).</span></span>

<span data-ttu-id="aae2e-114">После настройки проверки подлинности доступ к пользователю можно получить в методах службы gRPC через `ServerCallContext`.</span><span class="sxs-lookup"><span data-stu-id="aae2e-114">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="aae2e-115">Проверка подлинности токена носителя</span><span class="sxs-lookup"><span data-stu-id="aae2e-115">Bearer token authentication</span></span>

<span data-ttu-id="aae2e-116">Клиент может предоставить маркер доступа для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="aae2e-116">The client can provide an access token for authentication.</span></span> <span data-ttu-id="aae2e-117">Сервер проверяет маркер и использует его для обнаружения пользователя.</span><span class="sxs-lookup"><span data-stu-id="aae2e-117">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="aae2e-118">На сервере проверка подлинности токена носителя настраивается с помощью по [промежуточного слоя JWT Bearer](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="aae2e-118">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="aae2e-119">В клиенте .NET gRPC маркер можно отправить с помощью вызовов в виде заголовка:</span><span class="sxs-lookup"><span data-stu-id="aae2e-119">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

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

### <a name="client-certificate-authentication"></a><span data-ttu-id="aae2e-120">Проверка подлинности сертификата клиента</span><span class="sxs-lookup"><span data-stu-id="aae2e-120">Client certificate authentication</span></span>

<span data-ttu-id="aae2e-121">Клиент может также предоставить сертификат клиента для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="aae2e-121">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="aae2e-122">[Проверка подлинности сертификата](https://tools.ietf.org/html/rfc5246#section-7.4.4) выполняется на уровне TLS, прежде чем он когда-либо получит ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aae2e-122">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="aae2e-123">Когда запрос переходит ASP.NET Core, [пакет проверки подлинности сертификата клиента](xref:security/authentication/certauth) позволяет разрешить сертификат в `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="aae2e-123">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="aae2e-124">Узел должен быть настроен для приема сертификатов клиентов.</span><span class="sxs-lookup"><span data-stu-id="aae2e-124">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="aae2e-125">Сведения о принятии сертификатов клиентов в Kestrel, IIS и Azure см. в статье [Настройка узла для запроса сертификатов](xref:security/authentication/certauth#configure-your-host-to-require-certificates) .</span><span class="sxs-lookup"><span data-stu-id="aae2e-125">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="aae2e-126">В клиенте .NET gRPC сертификат клиента добавляется `HttpClientHandler` в, который затем используется для создания клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="aae2e-126">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

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

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="aae2e-127">Другие механизмы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="aae2e-127">Other authentication mechanisms</span></span>

<span data-ttu-id="aae2e-128">Многие ASP.NET Core поддерживаемые механизмы проверки подлинности работают с gRPC:</span><span class="sxs-lookup"><span data-stu-id="aae2e-128">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="aae2e-129">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aae2e-129">Azure Active Directory</span></span>
* <span data-ttu-id="aae2e-130">Сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="aae2e-130">Client Certificate</span></span>
* <span data-ttu-id="aae2e-131">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="aae2e-131">IdentityServer</span></span>
* <span data-ttu-id="aae2e-132">Токен JWT</span><span class="sxs-lookup"><span data-stu-id="aae2e-132">JWT Token</span></span>
* <span data-ttu-id="aae2e-133">OAuth 2,0</span><span class="sxs-lookup"><span data-stu-id="aae2e-133">OAuth 2.0</span></span>
* <span data-ttu-id="aae2e-134">OpenID Connect подключение</span><span class="sxs-lookup"><span data-stu-id="aae2e-134">OpenID Connect</span></span>
* <span data-ttu-id="aae2e-135">WS-Federation</span><span class="sxs-lookup"><span data-stu-id="aae2e-135">WS-Federation</span></span>

<span data-ttu-id="aae2e-136">Дополнительные сведения о настройке проверки подлинности на сервере см. в разделе [ASP.NET Core Authentication](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="aae2e-136">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="aae2e-137">Настройка клиента gRPC для использования проверки подлинности будет зависеть от используемого механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="aae2e-137">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="aae2e-138">В предыдущем примере маркера носителя и сертификата клиента показаны несколько способов настройки клиента gRPC для отправки метаданных проверки подлинности с помощью вызовов gRPC:</span><span class="sxs-lookup"><span data-stu-id="aae2e-138">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="aae2e-139">Строго типизированные клиенты gRPC `HttpClient` используют внутреннее.</span><span class="sxs-lookup"><span data-stu-id="aae2e-139">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="aae2e-140">Проверку подлинности можно [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)настроить для или путем добавления [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) пользовательских экземпляров `HttpClient`в.</span><span class="sxs-lookup"><span data-stu-id="aae2e-140">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="aae2e-141">Каждый вызов gRPC имеет необязательный `CallOptions` аргумент.</span><span class="sxs-lookup"><span data-stu-id="aae2e-141">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="aae2e-142">Пользовательские заголовки можно отправлять с помощью коллекции заголовков параметра.</span><span class="sxs-lookup"><span data-stu-id="aae2e-142">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="aae2e-143">Проверка подлинности Windows (NTLM/Kerberos/Negotiate) не может использоваться с gRPC.</span><span class="sxs-lookup"><span data-stu-id="aae2e-143">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="aae2e-144">для gRPC требуется HTTP/2, а HTTP/2 не поддерживает проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="aae2e-144">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="aae2e-145">Авторизация пользователей для доступа к службам и методам служб</span><span class="sxs-lookup"><span data-stu-id="aae2e-145">Authorize users to access services and service methods</span></span>

<span data-ttu-id="aae2e-146">По умолчанию все методы в службе могут вызываться пользователями, не прошедшими проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="aae2e-146">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="aae2e-147">Чтобы требовать проверку подлинности, примените к службе атрибут [[авторизовать]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) :</span><span class="sxs-lookup"><span data-stu-id="aae2e-147">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="aae2e-148">Можно использовать аргументы конструктора и свойства `[Authorize]` атрибута, чтобы ограничить доступ только тем пользователям, которые соответствуют определенным политикам [авторизации](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="aae2e-148">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="aae2e-149">Например, если имеется пользовательская политика `MyAuthorizationPolicy`авторизации, убедитесь, что только пользователи, соответствующие этой политике, могут получить доступ к службе, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="aae2e-149">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="aae2e-150">К отдельным методам службы также `[Authorize]` может применяться атрибут.</span><span class="sxs-lookup"><span data-stu-id="aae2e-150">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="aae2e-151">Если текущий пользователь не соответствует политикам, примененным **как** к методу, так и к классу, вызывающему объекту будет возвращена ошибка:</span><span class="sxs-lookup"><span data-stu-id="aae2e-151">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="aae2e-152">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="aae2e-152">Additional resources</span></span>

* [<span data-ttu-id="aae2e-153">Проверка подлинности токена носителя в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aae2e-153">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="aae2e-154">Настройка проверки подлинности сертификата клиента в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aae2e-154">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
