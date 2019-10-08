---
title: Проверка подлинности и авторизация в gRPC для ASP.NET Core
author: jamesnk
description: Узнайте, как использовать проверку подлинности и авторизацию в gRPC для ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/13/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: e8dd384ec43a66e56891925dcaa529085fa200c7
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999858"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="3e5c5-103">Проверка подлинности и авторизация в gRPC для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3e5c5-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="3e5c5-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="3e5c5-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="3e5c5-105">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(Загрузка)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="3e5c5-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="3e5c5-106">Проверка подлинности пользователей, вызывающих службу gRPC</span><span class="sxs-lookup"><span data-stu-id="3e5c5-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="3e5c5-107">gRPC можно использовать с [проверкой подлинности ASP.NET Core](xref:security/authentication/identity) , чтобы связать пользователя с каждым вызовом.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="3e5c5-108">Ниже приведен пример `Startup.Configure`, в котором используется проверка подлинности gRPC и ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="3e5c5-109">Порядок, в котором регистрируется ASP.NET Core по промежуточного слоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="3e5c5-110">Всегда вызывайте `UseAuthentication` и `UseAuthorization` после `UseRouting` и до `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="3e5c5-111">Механизм проверки подлинности, используемый приложением во время вызова, необходимо настроить.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-111">The authentication mechanism your app uses during a call needs to be configured.</span></span> <span data-ttu-id="3e5c5-112">Конфигурация проверки подлинности добавляется в `Startup.ConfigureServices` и будет отличаться в зависимости от используемого в приложении механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-112">Authentication configuration is added in `Startup.ConfigureServices` and will be different depending upon the authentication mechanism your app uses.</span></span> <span data-ttu-id="3e5c5-113">Примеры защиты приложений ASP.NET Core см. в статье [примеры проверки подлинности](xref:security/authentication/samples).</span><span class="sxs-lookup"><span data-stu-id="3e5c5-113">For examples of how to secure ASP.NET Core apps, see [Authentication samples](xref:security/authentication/samples).</span></span>

<span data-ttu-id="3e5c5-114">После настройки проверки подлинности пользователь может получить доступ к gRPC методам службы через `ServerCallContext`.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-114">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="3e5c5-115">Проверка подлинности токена носителя</span><span class="sxs-lookup"><span data-stu-id="3e5c5-115">Bearer token authentication</span></span>

<span data-ttu-id="3e5c5-116">Клиент может предоставить маркер доступа для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-116">The client can provide an access token for authentication.</span></span> <span data-ttu-id="3e5c5-117">Сервер проверяет маркер и использует его для обнаружения пользователя.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-117">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="3e5c5-118">На сервере проверка подлинности токена носителя настраивается с помощью по [промежуточного слоя JWT Bearer](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="3e5c5-118">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="3e5c5-119">В клиенте .NET gRPC маркер можно отправить с помощью вызовов в виде заголовка:</span><span class="sxs-lookup"><span data-stu-id="3e5c5-119">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

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

<span data-ttu-id="3e5c5-120">Настройка `ChannelCredentials` в канале — альтернативный способ отправки маркера службе с вызовами gRPC.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-120">Configuring `ChannelCredentials` on a channel is an alternative way to send the token to the service with gRPC calls.</span></span> <span data-ttu-id="3e5c5-121">Учетные данные запускаются каждый раз при вызове gRPC, что позволяет избежать необходимости писать код в нескольких местах для самостоятельного передачи маркера.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-121">The credential is run each time a gRPC call is made, which avoids the need to write code in multiple places to pass the token yourself.</span></span>

<span data-ttu-id="3e5c5-122">Учетные данные в следующем примере настраивают канал для отправки маркера при каждом вызове gRPC:</span><span class="sxs-lookup"><span data-stu-id="3e5c5-122">The credential in the following example configures the channel to send the token with every gRPC call:</span></span>

```csharp
private static GrpcChannel CreateAuthenticatedChannel(string address)
{
    var credentials = CallCredentials.FromInterceptor((context, metadata) =>
    {
        if (!string.IsNullOrEmpty(_token))
        {
            metadata.Add("Authorization", $"Bearer {_token}");
        }
        return Task.CompletedTask;
    });

    // SslCredentials is used here because this channel is using TLS.
    // Channels that aren't using TLS should use ChannelCredentials.Insecure instead.
    var channel = GrpcChannel.ForAddress(address, new GrpcChannelOptions
    {
        Credentials = ChannelCredentials.Create(new SslCredentials(), credentials)
    });
    return channel;
}
```

### <a name="client-certificate-authentication"></a><span data-ttu-id="3e5c5-123">Проверка подлинности сертификата клиента</span><span class="sxs-lookup"><span data-stu-id="3e5c5-123">Client certificate authentication</span></span>

<span data-ttu-id="3e5c5-124">Клиент может также предоставить сертификат клиента для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-124">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="3e5c5-125">[Проверка подлинности сертификата](https://tools.ietf.org/html/rfc5246#section-7.4.4) выполняется на уровне TLS, прежде чем он когда-либо получит ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-125">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="3e5c5-126">Когда запрос переходит ASP.NET Core, [пакет проверки подлинности сертификата клиента](xref:security/authentication/certauth) позволяет разрешить сертификат в `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-126">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="3e5c5-127">Узел должен быть настроен для приема сертификатов клиентов.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-127">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="3e5c5-128">Сведения о принятии сертификатов клиентов в Kestrel, IIS и Azure см. в статье [Настройка узла для запроса сертификатов](xref:security/authentication/certauth#configure-your-host-to-require-certificates) .</span><span class="sxs-lookup"><span data-stu-id="3e5c5-128">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="3e5c5-129">В клиенте .NET gRPC сертификат клиента добавляется в `HttpClientHandler`, который затем используется для создания клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="3e5c5-129">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC channel
    var channel = GrpcChannel.ForAddress(baseAddress, new GrpcChannelOptions
    {
        HttpClient = new HttpClient(handler)
    });

    return new Ticketer.TicketerClient(channel);
}
```

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="3e5c5-130">Другие механизмы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="3e5c5-130">Other authentication mechanisms</span></span>

<span data-ttu-id="3e5c5-131">Многие ASP.NET Core поддерживаемые механизмы проверки подлинности работают с gRPC:</span><span class="sxs-lookup"><span data-stu-id="3e5c5-131">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="3e5c5-132">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e5c5-132">Azure Active Directory</span></span>
* <span data-ttu-id="3e5c5-133">Сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="3e5c5-133">Client Certificate</span></span>
* <span data-ttu-id="3e5c5-134">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="3e5c5-134">IdentityServer</span></span>
* <span data-ttu-id="3e5c5-135">Токен JWT</span><span class="sxs-lookup"><span data-stu-id="3e5c5-135">JWT Token</span></span>
* <span data-ttu-id="3e5c5-136">OAuth 2,0</span><span class="sxs-lookup"><span data-stu-id="3e5c5-136">OAuth 2.0</span></span>
* <span data-ttu-id="3e5c5-137">OpenID Connect подключение</span><span class="sxs-lookup"><span data-stu-id="3e5c5-137">OpenID Connect</span></span>
* <span data-ttu-id="3e5c5-138">WS-Federation</span><span class="sxs-lookup"><span data-stu-id="3e5c5-138">WS-Federation</span></span>

<span data-ttu-id="3e5c5-139">Дополнительные сведения о настройке проверки подлинности на сервере см. в разделе [ASP.NET Core Authentication](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="3e5c5-139">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="3e5c5-140">Настройка клиента gRPC для использования проверки подлинности будет зависеть от используемого механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-140">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="3e5c5-141">В предыдущем примере маркера носителя и сертификата клиента показаны несколько способов настройки клиента gRPC для отправки метаданных проверки подлинности с помощью вызовов gRPC:</span><span class="sxs-lookup"><span data-stu-id="3e5c5-141">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="3e5c5-142">Строго типизированные клиенты gRPC используют `HttpClient` внутренне.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-142">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="3e5c5-143">Проверку подлинности можно настроить для [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)или путем добавления пользовательских экземпляров [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) в `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-143">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="3e5c5-144">Каждый вызов gRPC имеет необязательный аргумент `CallOptions`.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-144">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="3e5c5-145">Пользовательские заголовки можно отправлять с помощью коллекции заголовков параметра.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-145">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="3e5c5-146">Проверка подлинности Windows (NTLM/Kerberos/Negotiate) не может использоваться с gRPC.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-146">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="3e5c5-147">для gRPC требуется HTTP/2, а HTTP/2 не поддерживает проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-147">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="3e5c5-148">Авторизация пользователей для доступа к службам и методам служб</span><span class="sxs-lookup"><span data-stu-id="3e5c5-148">Authorize users to access services and service methods</span></span>

<span data-ttu-id="3e5c5-149">По умолчанию все методы в службе могут вызываться пользователями, не прошедшими проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-149">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="3e5c5-150">Чтобы требовать проверку подлинности, примените к службе атрибут [[авторизовать]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) :</span><span class="sxs-lookup"><span data-stu-id="3e5c5-150">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="3e5c5-151">Можно использовать аргументы конструктора и свойства атрибута `[Authorize]`, чтобы ограничить доступ только пользователями, соответствующими определенным [политикам авторизации](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="3e5c5-151">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="3e5c5-152">Например, если имеется пользовательская политика авторизации с именем `MyAuthorizationPolicy`, убедитесь, что только пользователи, соответствующие этой политике, имеют доступ к службе, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="3e5c5-152">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="3e5c5-153">Для отдельных методов службы также можно применить атрибут `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="3e5c5-153">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="3e5c5-154">Если текущий пользователь не соответствует политикам, примененным **как** к методу, так и к классу, вызывающему объекту будет возвращена ошибка:</span><span class="sxs-lookup"><span data-stu-id="3e5c5-154">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="3e5c5-155">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3e5c5-155">Additional resources</span></span>

* [<span data-ttu-id="3e5c5-156">Проверка подлинности токена носителя в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3e5c5-156">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="3e5c5-157">Настройка проверки подлинности сертификата клиента в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3e5c5-157">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
