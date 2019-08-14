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
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a>Проверка подлинности и авторизация в gRPC для ASP.NET Core

[Джеймс Ньютона-короля](https://twitter.com/jamesnk)

[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(как скачать)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-calling-a-grpc-service"></a>Проверка подлинности пользователей, вызывающих службу gRPC

gRPC можно использовать с проверкой подлинности [ASP.NET Core](xref:security/authentication/identity) , чтобы связать пользователя с каждым вызовом.

Ниже приведен пример `Startup.Configure` использования проверки подлинности gRPC и ASP.NET Core.

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
> Порядок, в котором регистрируется ASP.NET Core по промежуточного слоя проверки подлинности. Всегда `UseAuthentication` вызывайте `UseAuthorization` и `UseRouting` After и `UseEndpoints`before.

Механизм проверки подлинности, используемый приложением во время вызова, необходимо настроить. Конфигурация проверки подлинности `Startup.ConfigureServices` добавляется в и будет отличаться в зависимости от используемого в приложении механизма проверки подлинности. Примеры защиты приложений ASP.NET Core см. в статье [примеры проверки](xref:security/authentication/samples)подлинности.

После настройки проверки подлинности доступ к пользователю можно получить в методах службы gRPC через `ServerCallContext`.

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a>Проверка подлинности токена носителя

Клиент может предоставить маркер доступа для проверки подлинности. Сервер проверяет маркер и использует его для обнаружения пользователя.

На сервере проверка подлинности токена носителя настраивается с помощью по [промежуточного слоя JWT Bearer](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

В клиенте .NET gRPC маркер можно отправить с помощью вызовов в виде заголовка:

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

### <a name="client-certificate-authentication"></a>Проверка подлинности сертификата клиента

Клиент может также предоставить сертификат клиента для проверки подлинности. [Проверка подлинности сертификата](https://tools.ietf.org/html/rfc5246#section-7.4.4) выполняется на уровне TLS, прежде чем он когда-либо получит ASP.NET Core. Когда запрос переходит ASP.NET Core, [пакет проверки подлинности сертификата клиента](xref:security/authentication/certauth) позволяет разрешить сертификат в `ClaimsPrincipal`.

> [!NOTE]
> Узел должен быть настроен для приема сертификатов клиентов. Сведения о принятии сертификатов клиентов в Kestrel, IIS и Azure см. в статье [Настройка узла для запроса сертификатов](xref:security/authentication/certauth#configure-your-host-to-require-certificates) .

В клиенте .NET gRPC сертификат клиента добавляется `HttpClientHandler` в, который затем используется для создания клиента gRPC:

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

### <a name="other-authentication-mechanisms"></a>Другие механизмы проверки подлинности

Многие ASP.NET Core поддерживаемые механизмы проверки подлинности работают с gRPC:

* Azure Active Directory
* Сертификат клиента
* IdentityServer
* Токен JWT
* OAuth 2,0
* OpenID Connect подключение
* WS-Federation

Дополнительные сведения о настройке проверки подлинности на сервере см. в разделе [ASP.NET Core Authentication](xref:security/authentication/identity).

Настройка клиента gRPC для использования проверки подлинности будет зависеть от используемого механизма проверки подлинности. В предыдущем примере маркера носителя и сертификата клиента показаны несколько способов настройки клиента gRPC для отправки метаданных проверки подлинности с помощью вызовов gRPC:

* Строго типизированные клиенты gRPC `HttpClient` используют внутреннее. Проверку подлинности можно [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)настроить для или путем добавления [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) пользовательских экземпляров `HttpClient`в.
* Каждый вызов gRPC имеет необязательный `CallOptions` аргумент. Пользовательские заголовки можно отправлять с помощью коллекции заголовков параметра.

> [!NOTE]
> Проверка подлинности Windows (NTLM/Kerberos/Negotiate) не может использоваться с gRPC. для gRPC требуется HTTP/2, а HTTP/2 не поддерживает проверку подлинности Windows.

## <a name="authorize-users-to-access-services-and-service-methods"></a>Авторизация пользователей для доступа к службам и методам служб

По умолчанию все методы в службе могут вызываться пользователями, не прошедшими проверку подлинности. Чтобы требовать проверку подлинности, примените к службе атрибут [[авторизовать]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) :

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

Можно использовать аргументы конструктора и свойства `[Authorize]` атрибута, чтобы ограничить доступ только тем пользователям, которые соответствуют определенным политикам [авторизации](xref:security/authorization/policies). Например, если имеется пользовательская политика `MyAuthorizationPolicy`авторизации, убедитесь, что только пользователи, соответствующие этой политике, могут получить доступ к службе, используя следующий код:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

К отдельным методам службы также `[Authorize]` может применяться атрибут. Если текущий пользователь не соответствует политикам, примененным **как** к методу, так и к классу, вызывающему объекту будет возвращена ошибка:

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Проверка подлинности токена носителя в ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [Настройка проверки подлинности сертификата клиента в ASP.NET Core](xref:security/authentication/certauth)
