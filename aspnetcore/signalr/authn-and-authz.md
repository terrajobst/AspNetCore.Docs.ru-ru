---
title: Проверка подлинности и авторизация в ASP.NET Core SignalR
author: tdykstra
description: Узнайте, как использовать проверку подлинности и авторизации в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: fa5e8d4aea6100c54fd8b98411ef877d1dd8621c
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028209"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="4f130-103">Проверка подлинности и авторизация в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4f130-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="4f130-104">По [Andrew Stanton медсестра](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="4f130-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="4f130-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(способ загрузки)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="4f130-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="4f130-106">Проверка подлинности пользователей, подключающихся к концентратору SignalR</span><span class="sxs-lookup"><span data-stu-id="4f130-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="4f130-107">SignalR может использоваться с [проверки подлинности ASP.NET Core](xref:security/authentication/index) чтобы связать пользователя с каждым подключением.</span><span class="sxs-lookup"><span data-stu-id="4f130-107">SignalR can be used with [ASP.NET Core Authentication](xref:security/authentication/index) to associate a user with each connection.</span></span> <span data-ttu-id="4f130-108">В центре данных для проверки подлинности может осуществляться из [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) свойство.</span><span class="sxs-lookup"><span data-stu-id="4f130-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="4f130-109">Проверка подлинности позволяет вызывать методы для всех соединений, связанный с пользователем в центре (см. в разделе [управления пользователями и группами в SignalR](xref:signalr/groups) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="4f130-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="4f130-110">Несколько соединений могут быть связаны с одним пользователем.</span><span class="sxs-lookup"><span data-stu-id="4f130-110">Multiple connections may be associated with a single user.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="4f130-111">Файл cookie проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="4f130-111">Cookie authentication</span></span>

<span data-ttu-id="4f130-112">В приложении на основе веб-обозревателя файл cookie проверки подлинности позволяет свои существующие учетные данные пользователя, автоматически обмениваться с подключениями SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f130-112">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="4f130-113">При использовании клиентского браузера, без дополнительных настроек не требуется.</span><span class="sxs-lookup"><span data-stu-id="4f130-113">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="4f130-114">Если пользователь вошел в приложение, подключении SignalR автоматически наследует этой проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="4f130-114">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="4f130-115">Файл cookie проверки подлинности не рекомендуется, если приложению достаточно только для проверки подлинности пользователей из клиента браузера.</span><span class="sxs-lookup"><span data-stu-id="4f130-115">Cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="4f130-116">При использовании [клиента .NET](xref:signalr/dotnet-client), `Cookies` свойство можно задать в `.WithUrl` вызов, чтобы предоставить файл cookie.</span><span class="sxs-lookup"><span data-stu-id="4f130-116">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="4f130-117">Тем не менее с использованием файла cookie проверки подлинности из клиента .NET требуется приложению предоставлять API для обмена данными проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="4f130-117">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="4f130-118">Маркер проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="4f130-118">Bearer token authentication</span></span>

<span data-ttu-id="4f130-119">Токена проверки подлинности носителя — это рекомендуемый подход, при использовании клиентов, отличных от браузера клиента.</span><span class="sxs-lookup"><span data-stu-id="4f130-119">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span> <span data-ttu-id="4f130-120">В этом случае клиент предоставляет маркер доступа, сервер проверяет и использует для идентификации пользователя.</span><span class="sxs-lookup"><span data-stu-id="4f130-120">In this approach, the client provides an access token that the server validates and uses to identify the user.</span></span> <span data-ttu-id="4f130-121">Сведения о проверки подлинности маркера носителя выходят за рамки настоящего документа.</span><span class="sxs-lookup"><span data-stu-id="4f130-121">The details of bearer token authentication are beyond the scope of this document.</span></span> <span data-ttu-id="4f130-122">На сервере проверки подлинности маркера носителя настраивается с помощью [по промежуточного слоя носителя JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="4f130-122">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="4f130-123">В клиенте JavaScript, токен можно указать с помощью [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) параметр.</span><span class="sxs-lookup"><span data-stu-id="4f130-123">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="4f130-124">В клиенте .NET, отсутствует, аналогичное [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) свойство, которое может использоваться для настройки маркера:</span><span class="sxs-lookup"><span data-stu-id="4f130-124">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="4f130-125">Функция маркера доступа, вами вызывается перед **каждые** HTTP-запроса, сделанных SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f130-125">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="4f130-126">Если необходимо обновлять маркер, чтобы сохранить подключение active (так как он может истечь срок действия во время соединения), выполнять из этой функции и возвращают обновленный маркер.</span><span class="sxs-lookup"><span data-stu-id="4f130-126">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="4f130-127">В стандартных веб-API в заголовке HTTP отправляются токены носителя.</span><span class="sxs-lookup"><span data-stu-id="4f130-127">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="4f130-128">Тем не менее SignalR не удается задать эти заголовки в браузерах, при использовании некоторых транспортов.</span><span class="sxs-lookup"><span data-stu-id="4f130-128">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="4f130-129">При использовании WebSockets и Server-Sent события, маркер передается как параметр строки запроса.</span><span class="sxs-lookup"><span data-stu-id="4f130-129">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="4f130-130">Чтобы обеспечить это, на сервере, необходима дополнительная настройка:</span><span class="sxs-lookup"><span data-stu-id="4f130-130">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="windows-authentication"></a><span data-ttu-id="4f130-131">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="4f130-131">Windows authentication</span></span>

<span data-ttu-id="4f130-132">Если [проверки подлинности Windows](xref:security/authentication/windowsauth) настраивается в приложении, SignalR можно использовать его для обеспечения безопасности концентраторы.</span><span class="sxs-lookup"><span data-stu-id="4f130-132">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="4f130-133">Тем не менее чтобы отправлять сообщения отдельным пользователям, необходимо добавить пользовательский поставщик идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="4f130-133">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="4f130-134">Это потому, что система проверки подлинности Windows не предоставляет утверждение «Идентификатор имени», SignalR использует, чтобы определить имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="4f130-134">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="4f130-135">Добавьте новый класс, реализующий `IUserIdProvider` и получить одно из утверждений от пользователя для использования в качестве идентификатора.</span><span class="sxs-lookup"><span data-stu-id="4f130-135">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="4f130-136">Например, чтобы использовать утверждение «Name» (это имя пользователя Windows в виде `[Domain]\[Username]`), создайте следующий класс:</span><span class="sxs-lookup"><span data-stu-id="4f130-136">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

<span data-ttu-id="4f130-137">Вместо `ClaimTypes.Name`, можно использовать любое значение от `User` (такие как идентификатор Windows SID и т.д.).</span><span class="sxs-lookup"><span data-stu-id="4f130-137">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="4f130-138">Значение, выбираемый должно быть уникальным среди всех пользователей в вашей системе.</span><span class="sxs-lookup"><span data-stu-id="4f130-138">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="4f130-139">В противном случае сообщение, предназначенное для одного пользователя могут оказаться собираетесь другого пользователя.</span><span class="sxs-lookup"><span data-stu-id="4f130-139">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="4f130-140">Зарегистрировать этот компонент в вашей `Startup.ConfigureServices` метод **после** вызов `.AddSignalR`</span><span class="sxs-lookup"><span data-stu-id="4f130-140">Register this component in your `Startup.ConfigureServices` method **after** the call to `.AddSignalR`</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="4f130-141">В клиенте .NET необходимо включить проверку подлинности Windows, задав [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) свойство:</span><span class="sxs-lookup"><span data-stu-id="4f130-141">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="4f130-142">Проверка подлинности Windows поддерживается только с помощью браузера клиента, при использовании Microsoft Internet Explorer или Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="4f130-142">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="4f130-143">Авторизация пользователей для доступа к концентраторам и методам концентраторов</span><span class="sxs-lookup"><span data-stu-id="4f130-143">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="4f130-144">По умолчанию все методы в концентраторе, могут вызываться пользователем, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="4f130-144">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="4f130-145">Чтобы требовать проверку подлинности, применить [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) атрибут к центру:</span><span class="sxs-lookup"><span data-stu-id="4f130-145">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="4f130-146">Можно использовать конструктор аргументов и свойств `[Authorize]` атрибута для ограничения доступа к только пользователям, которые соответствуют определенной [политик авторизации](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="4f130-146">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="4f130-147">Например, если у вас есть пользовательской политики авторизации вызывается `MyAuthorizationPolicy` убедитесь, что только пользователи, которые соответствуют этой политике можно получить доступ к концентратора, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="4f130-147">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="4f130-148">Иной концентратор методы могут иметь `[Authorize]` также применен атрибут.</span><span class="sxs-lookup"><span data-stu-id="4f130-148">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="4f130-149">Если текущий пользователь не соответствует политике, применяемый к методу, вызывающему объекту возвращается ошибка:</span><span class="sxs-lookup"><span data-stu-id="4f130-149">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub: Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```
