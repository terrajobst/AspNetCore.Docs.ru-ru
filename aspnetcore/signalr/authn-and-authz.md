---
title: Проверка подлинности и авторизация в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как использовать проверку подлинности и авторизации в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 05/09/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: e8f9dc48be780fb91bdec6ea4d579f5e4f16197b
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516943"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="7bb77-103">Проверка подлинности и авторизация в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="7bb77-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="7bb77-104">По [Andrew Stanton медсестра](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="7bb77-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="7bb77-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(способ загрузки)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="7bb77-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="7bb77-106">Проверка подлинности пользователей, подключающихся к концентратору SignalR</span><span class="sxs-lookup"><span data-stu-id="7bb77-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="7bb77-107">SignalR может использоваться с [проверки подлинности ASP.NET Core](xref:security/authentication/identity) чтобы связать пользователя с каждым подключением.</span><span class="sxs-lookup"><span data-stu-id="7bb77-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="7bb77-108">В центре данных для проверки подлинности может осуществляться из [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) свойство.</span><span class="sxs-lookup"><span data-stu-id="7bb77-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="7bb77-109">Проверка подлинности позволяет вызывать методы для всех соединений, связанный с пользователем в центре (см. в разделе [управления пользователями и группами в SignalR](xref:signalr/groups) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="7bb77-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="7bb77-110">Несколько соединений могут быть связаны с одним пользователем.</span><span class="sxs-lookup"><span data-stu-id="7bb77-110">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="7bb77-111">Ниже приведен пример `Startup.Configure` которого использует проверку подлинности SignalR и ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7bb77-111">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();
    
    app.UseAuthentication();

    app.UseSignalR(hubs =>
    {
        hubs.MapHub<ChatHub>("/chat");
    });

    app.UseMvc(routes =>
    {
        routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

> [!NOTE]
> <span data-ttu-id="7bb77-112">Порядок, в котором вам зарегистрировать промежуточного по проверки подлинности SignalR и ASP.NET Core имеет значение.</span><span class="sxs-lookup"><span data-stu-id="7bb77-112">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="7bb77-113">Всегда вызывайте метод `UseAuthentication` перед `UseSignalR` таким образом, SignalR пользователя `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7bb77-113">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="7bb77-114">Файл cookie проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="7bb77-114">Cookie authentication</span></span>

<span data-ttu-id="7bb77-115">В приложении на основе веб-обозревателя файл cookie проверки подлинности позволяет свои существующие учетные данные пользователя, автоматически обмениваться с подключениями SignalR.</span><span class="sxs-lookup"><span data-stu-id="7bb77-115">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="7bb77-116">При использовании клиентского браузера, без дополнительных настроек не требуется.</span><span class="sxs-lookup"><span data-stu-id="7bb77-116">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="7bb77-117">Если пользователь вошел в приложение, подключении SignalR автоматически наследует этой проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="7bb77-117">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="7bb77-118">Файлы cookie представляют собой обозревателем позволяют отправлять маркеры доступа, но их можно отправлять клиентов без браузера.</span><span class="sxs-lookup"><span data-stu-id="7bb77-118">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="7bb77-119">При использовании [клиента .NET](xref:signalr/dotnet-client), `Cookies` свойство можно задать в `.WithUrl` вызов, чтобы предоставить файл cookie.</span><span class="sxs-lookup"><span data-stu-id="7bb77-119">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="7bb77-120">Тем не менее с использованием файла cookie проверки подлинности из клиента .NET требуется приложению предоставлять API для обмена данными проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="7bb77-120">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="7bb77-121">Маркер проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="7bb77-121">Bearer token authentication</span></span>

<span data-ttu-id="7bb77-122">Клиент может предоставить маркер доступа, вместо того чтобы использовать файл cookie.</span><span class="sxs-lookup"><span data-stu-id="7bb77-122">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="7bb77-123">Сервер проверяет маркер и использует его для идентификации пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb77-123">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="7bb77-124">Эта проверка выполняется только в том случае, если соединение установлено.</span><span class="sxs-lookup"><span data-stu-id="7bb77-124">This validation is done only when the connection is established.</span></span> <span data-ttu-id="7bb77-125">В течение жизненного цикла соединения сервер не повторную сверку автоматически для проверки маркера.</span><span class="sxs-lookup"><span data-stu-id="7bb77-125">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="7bb77-126">На сервере проверки подлинности маркера носителя настраивается с помощью [по промежуточного слоя носителя JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="7bb77-126">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="7bb77-127">В клиенте JavaScript, токен можно указать с помощью [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) параметр.</span><span class="sxs-lookup"><span data-stu-id="7bb77-127">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="7bb77-128">В клиенте .NET, отсутствует, аналогичное [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) свойство, которое может использоваться для настройки маркера:</span><span class="sxs-lookup"><span data-stu-id="7bb77-128">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="7bb77-129">Функция маркера доступа, вами вызывается перед **каждые** HTTP-запроса, сделанных SignalR.</span><span class="sxs-lookup"><span data-stu-id="7bb77-129">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="7bb77-130">Если необходимо обновлять маркер, чтобы сохранить подключение active (так как он может истечь срок действия во время соединения), выполнять из этой функции и возвращают обновленный маркер.</span><span class="sxs-lookup"><span data-stu-id="7bb77-130">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="7bb77-131">В стандартных веб-API в заголовке HTTP отправляются токены носителя.</span><span class="sxs-lookup"><span data-stu-id="7bb77-131">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="7bb77-132">Тем не менее SignalR не удается задать эти заголовки в браузерах, при использовании некоторых транспортов.</span><span class="sxs-lookup"><span data-stu-id="7bb77-132">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="7bb77-133">При использовании WebSockets и Server-Sent события, маркер передается как параметр строки запроса.</span><span class="sxs-lookup"><span data-stu-id="7bb77-133">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="7bb77-134">Чтобы обеспечить это, на сервере, необходима дополнительная настройка:</span><span class="sxs-lookup"><span data-stu-id="7bb77-134">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="7bb77-135">Файлы cookie и токены носителя</span><span class="sxs-lookup"><span data-stu-id="7bb77-135">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="7bb77-136">Поскольку файлы cookie являются специфическими для браузеров, их отправкой из других типов клиентов повышает сложность системы, по сравнению с отправкой токены носителя.</span><span class="sxs-lookup"><span data-stu-id="7bb77-136">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="7bb77-137">По этой причине файл cookie проверки подлинности не рекомендуется, если приложению достаточно только для проверки подлинности пользователей из клиента браузера.</span><span class="sxs-lookup"><span data-stu-id="7bb77-137">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="7bb77-138">Токена проверки подлинности носителя — это рекомендуемый подход, при использовании клиентов, отличных от браузера клиента.</span><span class="sxs-lookup"><span data-stu-id="7bb77-138">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="7bb77-139">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="7bb77-139">Windows authentication</span></span>

<span data-ttu-id="7bb77-140">Если [проверки подлинности Windows](xref:security/authentication/windowsauth) настраивается в приложении, SignalR можно использовать его для обеспечения безопасности концентраторы.</span><span class="sxs-lookup"><span data-stu-id="7bb77-140">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="7bb77-141">Тем не менее чтобы отправлять сообщения отдельным пользователям, необходимо добавить пользовательский поставщик идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb77-141">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="7bb77-142">Это потому, что система проверки подлинности Windows не предоставляет утверждение «Идентификатор имени», SignalR использует, чтобы определить имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb77-142">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="7bb77-143">Добавьте новый класс, реализующий `IUserIdProvider` и получить одно из утверждений от пользователя для использования в качестве идентификатора.</span><span class="sxs-lookup"><span data-stu-id="7bb77-143">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="7bb77-144">Например, чтобы использовать утверждение «Name» (это имя пользователя Windows в виде `[Domain]\[Username]`), создайте следующий класс:</span><span class="sxs-lookup"><span data-stu-id="7bb77-144">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="7bb77-145">Вместо `ClaimTypes.Name`, можно использовать любое значение от `User` (такие как идентификатор Windows SID и т.д.).</span><span class="sxs-lookup"><span data-stu-id="7bb77-145">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="7bb77-146">Значение, выбираемый должно быть уникальным среди всех пользователей в вашей системе.</span><span class="sxs-lookup"><span data-stu-id="7bb77-146">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="7bb77-147">В противном случае сообщение, предназначенное для одного пользователя могут оказаться собираетесь другого пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb77-147">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="7bb77-148">Зарегистрировать этот компонент в вашей `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="7bb77-148">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="7bb77-149">В клиенте .NET необходимо включить проверку подлинности Windows, задав [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) свойство:</span><span class="sxs-lookup"><span data-stu-id="7bb77-149">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="7bb77-150">Проверка подлинности Windows поддерживается только с помощью браузера клиента, при использовании Microsoft Internet Explorer или Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="7bb77-150">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="7bb77-151">Использование утверждений для настройки обработки удостоверений</span><span class="sxs-lookup"><span data-stu-id="7bb77-151">Use claims to customize identity handling</span></span>

<span data-ttu-id="7bb77-152">Приложение, которое выполняет проверку подлинности пользователей можно получить идентификаторы пользователей SignalR из утверждения пользователей.</span><span class="sxs-lookup"><span data-stu-id="7bb77-152">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="7bb77-153">Чтобы указать, каким образом SignalR создает идентификаторы пользователей, реализовать `IUserIdProvider` и зарегистрируйте реализацию.</span><span class="sxs-lookup"><span data-stu-id="7bb77-153">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="7bb77-154">В примере кода показано, как использовать утверждения для выбора адреса электронной почты пользователя в качестве идентифицирующего свойство.</span><span class="sxs-lookup"><span data-stu-id="7bb77-154">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="7bb77-155">Значение, выбираемый должно быть уникальным среди всех пользователей в вашей системе.</span><span class="sxs-lookup"><span data-stu-id="7bb77-155">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="7bb77-156">В противном случае сообщение, предназначенное для одного пользователя могут оказаться собираетесь другого пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb77-156">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="7bb77-157">Регистрация учетной записи добавляет утверждение с типом `ClaimsTypes.Email` для базы данных удостоверений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7bb77-157">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="7bb77-158">Зарегистрировать этот компонент в вашей `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7bb77-158">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="7bb77-159">Авторизация пользователей для доступа к концентраторам и методам концентраторов</span><span class="sxs-lookup"><span data-stu-id="7bb77-159">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="7bb77-160">По умолчанию все методы в концентраторе, могут вызываться пользователем, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="7bb77-160">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="7bb77-161">Чтобы требовать проверку подлинности, применить [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) атрибут к центру:</span><span class="sxs-lookup"><span data-stu-id="7bb77-161">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="7bb77-162">Можно использовать конструктор аргументов и свойств `[Authorize]` атрибута для ограничения доступа к только пользователям, которые соответствуют определенной [политик авторизации](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="7bb77-162">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="7bb77-163">Например, если у вас есть пользовательской политики авторизации вызывается `MyAuthorizationPolicy` убедитесь, что только пользователи, которые соответствуют этой политике можно получить доступ к концентратора, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="7bb77-163">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="7bb77-164">Иной концентратор методы могут иметь `[Authorize]` также применен атрибут.</span><span class="sxs-lookup"><span data-stu-id="7bb77-164">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="7bb77-165">Если текущий пользователь не соответствует политике, применяемый к методу, вызывающему объекту возвращается ошибка:</span><span class="sxs-lookup"><span data-stu-id="7bb77-165">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="7bb77-166">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7bb77-166">Additional resources</span></span>

* [<span data-ttu-id="7bb77-167">Маркер проверки подлинности носителя в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bb77-167">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
