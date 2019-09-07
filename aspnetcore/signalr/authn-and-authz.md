---
title: Проверка подлинности и авторизация в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как использовать проверку подлинности и авторизацию в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 07/15/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: da226f4e192be8e34a0b2cec1493a1353c995279
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746534"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="c1ac2-103">Проверка подлинности и авторизация в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c1ac2-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="c1ac2-104">[Эндрю Стантон-медперсонала](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="c1ac2-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="c1ac2-105">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(как скачать)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="c1ac2-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="c1ac2-106">Проверка подлинности пользователей, подключающихся к концентратору SignalR</span><span class="sxs-lookup"><span data-stu-id="c1ac2-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="c1ac2-107">SignalR можно использовать с [проверкой подлинности ASP.NET Core](xref:security/authentication/identity) , чтобы связать пользователя с каждым подключением.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="c1ac2-108">В центре данные проверки подлинности можно получить из [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) свойства.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="c1ac2-109">Проверка подлинности позволяет концентратору вызывать методы для всех подключений, связанных с пользователем (Дополнительные сведения см. [в разделе Управление пользователями и группами в SignalR](xref:signalr/groups) ).</span><span class="sxs-lookup"><span data-stu-id="c1ac2-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="c1ac2-110">Несколько подключений могут быть связаны с одним пользователем.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-110">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="c1ac2-111">Ниже приведен пример `Startup.Configure` использования SignalR и ASP.NET Core проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-111">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    app.UseRouting();

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chat");
        endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

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
> <span data-ttu-id="c1ac2-112">Порядок регистрации SignalR и ASP.NET Core по промежуточного слоя проверки подлинности имеет значение.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-112">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="c1ac2-113">Всегда вызывайте `UseSignalR` метод `UseAuthentication` перед тем, чтобы `HttpContext`у SignalR был пользователь.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-113">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

::: moniker-end

### <a name="cookie-authentication"></a><span data-ttu-id="c1ac2-114">Проверка подлинности файлов cookie</span><span class="sxs-lookup"><span data-stu-id="c1ac2-114">Cookie authentication</span></span>

<span data-ttu-id="c1ac2-115">В приложении на основе браузера проверка подлинности cookie позволяет существующим учетным данным пользователя автоматически передаваться на подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-115">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="c1ac2-116">При использовании клиента браузера дополнительная настройка не требуется.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-116">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="c1ac2-117">Если пользователь вошел в приложение, подключение SignalR автоматически наследует эту проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-117">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="c1ac2-118">Файлы cookie — это ориентированный на браузер способ отправки маркеров доступа, но клиенты, не являющиеся браузерами, могут их отправить.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-118">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="c1ac2-119">При использовании [клиента .NET](xref:signalr/dotnet-client) `Cookies` свойство можно настроить в `.WithUrl` вызове, чтобы предоставить файл cookie.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-119">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="c1ac2-120">Однако для использования проверки подлинности файлов cookie из клиента .NET приложение должно предоставить API для обмена данными проверки подлинности для файла cookie.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-120">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="c1ac2-121">Проверка подлинности токена носителя</span><span class="sxs-lookup"><span data-stu-id="c1ac2-121">Bearer token authentication</span></span>

<span data-ttu-id="c1ac2-122">Клиент может предоставить маркер доступа вместо использования файла cookie.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-122">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="c1ac2-123">Сервер проверяет маркер и использует его для обнаружения пользователя.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-123">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="c1ac2-124">Эта проверка выполняется только при установленном соединении.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-124">This validation is done only when the connection is established.</span></span> <span data-ttu-id="c1ac2-125">Во время жизни соединения сервер не выполняет автоматическую повторную проверку, чтобы проверить отзыв маркера.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-125">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="c1ac2-126">На сервере проверка подлинности токена носителя настраивается с помощью по [промежуточного слоя JWT Bearer](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="c1ac2-126">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="c1ac2-127">В клиенте JavaScript маркер можно указать с помощью параметра [акцесстокенфактори](xref:signalr/configuration#configure-bearer-authentication) .</span><span class="sxs-lookup"><span data-stu-id="c1ac2-127">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="c1ac2-128">В клиенте .NET существует аналогичное свойство [акцесстокенпровидер](xref:signalr/configuration#configure-bearer-authentication) , которое можно использовать для настройки маркера:</span><span class="sxs-lookup"><span data-stu-id="c1ac2-128">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="c1ac2-129">Предоставляемая функция токена доступа вызывается перед **каждым** HTTP-запросом, сделанным SignalR.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-129">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="c1ac2-130">Если необходимо обновить токен, чтобы подключение было активно (так как оно может истечь во время подключения), сделайте это в этой функции и возвратите обновленный маркер.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-130">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="c1ac2-131">В стандартных веб-API токены носителя отправляются в заголовке HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-131">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="c1ac2-132">Однако SignalR не может установить эти заголовки в браузерах при использовании некоторых транспортов.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-132">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="c1ac2-133">При использовании соединений WebSockets и серверных событий токен передается как параметр строки запроса.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-133">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="c1ac2-134">Для поддержки этого на сервере требуется дополнительная настройка:</span><span class="sxs-lookup"><span data-stu-id="c1ac2-134">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="c1ac2-135">Файлы cookie и токены носителя</span><span class="sxs-lookup"><span data-stu-id="c1ac2-135">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="c1ac2-136">Так как файлы cookie относятся только к браузерам, их отправка из других типов клиентов повышает сложность по сравнению с отправкой токенов носителя.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-136">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="c1ac2-137">По этой причине проверка подлинности файлов cookie не рекомендуется, если приложению требуется только проверка подлинности пользователей из клиента браузера.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-137">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="c1ac2-138">Проверка подлинности маркера носителя является рекомендуемым подходом при использовании клиентов, отличных от клиента браузера.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-138">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="c1ac2-139">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="c1ac2-139">Windows authentication</span></span>

<span data-ttu-id="c1ac2-140">Если в приложении настроена [Проверка подлинности Windows](xref:security/authentication/windowsauth) , SignalR может использовать это удостоверение для защиты концентраторов.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-140">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="c1ac2-141">Однако для отправки сообщений отдельным пользователям необходимо добавить настраиваемого поставщика ИДЕНТИФИКАТОРов пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-141">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="c1ac2-142">Это обусловлено тем, что система проверки подлинности Windows не предоставляет утверждение "идентификатор имени", которое SignalR использует для определения имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-142">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="c1ac2-143">Добавьте новый класс, который реализует `IUserIdProvider` и извлекает одно из утверждений пользователя для использования в качестве идентификатора.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-143">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="c1ac2-144">Например, чтобы использовать утверждение "Name" (имя пользователя Windows в форме `[Domain]\[Username]`), создайте следующий класс:</span><span class="sxs-lookup"><span data-stu-id="c1ac2-144">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="c1ac2-145">Вместо этого можно использовать любое значение `User` из (например, идентификатор SID Windows и т. д.). `ClaimTypes.Name`</span><span class="sxs-lookup"><span data-stu-id="c1ac2-145">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="c1ac2-146">Выбранное значение должно быть уникальным для всех пользователей в системе.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-146">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="c1ac2-147">В противном случае сообщение, предназначенное для одного пользователя, может завершить работу другого пользователя.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-147">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="c1ac2-148">Зарегистрируйте этот компонент в `Startup.ConfigureServices` методе.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-148">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="c1ac2-149">В клиенте .NET проверка подлинности Windows должна быть включена путем установки свойства [уседефаулткредентиалс](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) :</span><span class="sxs-lookup"><span data-stu-id="c1ac2-149">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="c1ac2-150">Проверка подлинности Windows поддерживается только клиентом браузера при использовании Microsoft Internet Explorer или Microsoft погранично.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-150">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="c1ac2-151">Использование утверждений для настройки обработки удостоверений</span><span class="sxs-lookup"><span data-stu-id="c1ac2-151">Use claims to customize identity handling</span></span>

<span data-ttu-id="c1ac2-152">Приложение, выполняющее проверку подлинности пользователей, может наследовать идентификаторы пользователей SignalR от утверждений пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-152">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="c1ac2-153">Чтобы указать, как SignalR создает идентификаторы пользователей, `IUserIdProvider` реализуйте и зарегистрируйте реализацию.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-153">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="c1ac2-154">В примере кода показано использование утверждений для выбора адреса электронной почты пользователя в качестве идентифицирующего свойства.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-154">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="c1ac2-155">Выбранное значение должно быть уникальным для всех пользователей в системе.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-155">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="c1ac2-156">В противном случае сообщение, предназначенное для одного пользователя, может завершить работу другого пользователя.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-156">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="c1ac2-157">При регистрации учетной записи в базу данных `ClaimsTypes.Email` удостоверений ASP.NET добавляется утверждение с типом.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-157">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="c1ac2-158">Зарегистрируйте этот компонент в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-158">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="c1ac2-159">Авторизация пользователей для доступа к концентраторам и методам концентратора</span><span class="sxs-lookup"><span data-stu-id="c1ac2-159">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="c1ac2-160">По умолчанию все методы в концентраторе могут вызываться пользователем, не прошедшим проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-160">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="c1ac2-161">Чтобы включить проверку подлинности, примените к концентратору атрибут [авторизации](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) :</span><span class="sxs-lookup"><span data-stu-id="c1ac2-161">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="c1ac2-162">Можно использовать аргументы конструктора и свойства `[Authorize]` атрибута, чтобы ограничить доступ только тем пользователям, которые соответствуют определенным [политикам авторизации](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="c1ac2-162">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="c1ac2-163">Например, при наличии пользовательской политики `MyAuthorizationPolicy` авторизации можно убедиться, что только пользователи, соответствующие этой политике, могут получить доступ к концентратору с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="c1ac2-163">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

<span data-ttu-id="c1ac2-164">Отдельные методы концентратора могут `[Authorize]` также применять атрибут.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-164">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="c1ac2-165">Если текущий пользователь не соответствует политике, примененной к методу, вызывающему объекту будет возвращена ошибка.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-165">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
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

::: moniker range=">= aspnetcore-3.0"

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a><span data-ttu-id="c1ac2-166">Использование обработчиков авторизации для настройки авторизации метода концентратора</span><span class="sxs-lookup"><span data-stu-id="c1ac2-166">Use authorization handlers to customize hub method authorization</span></span>

<span data-ttu-id="c1ac2-167">SignalR предоставляет обработчику авторизации пользовательский ресурс, когда методу концентратора требуется авторизация.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-167">SignalR provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="c1ac2-168">Ресурс является экземпляром `HubInvocationContext`.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-168">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="c1ac2-169">`HubInvocationContext` Включает,имявызываемогометодаконцентратораиаргументы`HubCallerContext`для метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-169">The `HubInvocationContext` includes the `HubCallerContext`, the name of the hub method being invoked, and the arguments to the hub method.</span></span>

<span data-ttu-id="c1ac2-170">Рассмотрим пример комнаты чатов, позволяющей нескольким организациям входить в систему через Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-170">Consider the example of a chat room allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="c1ac2-171">Любой пользователь, у которого есть учетная запись Майкрософт, может войти в чат, но только члены владеющей Организации должны иметь возможность запрещать пользователям или просматривать историю разговора пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-171">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization should be able to ban users or view users' chat histories.</span></span> <span data-ttu-id="c1ac2-172">Кроме того, может потребоваться ограничить некоторые функции от определенных пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-172">Furthermore, we might want to restrict certain functionality from certain users.</span></span> <span data-ttu-id="c1ac2-173">Использование обновленных функций в ASP.NET Core 3,0 — это вполне возможно.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-173">Using the updated features in ASP.NET Core 3.0, this is entirely possible.</span></span> <span data-ttu-id="c1ac2-174">Обратите внимание `DomainRestrictedRequirement` , что служит в `IAuthorizationRequirement`качестве пользовательского.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-174">Note how the `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="c1ac2-175">Теперь, `HubInvocationContext` когда передается параметр ресурса, внутренняя логика может проверить контекст, в котором вызывается концентратор, и принимать решения о том, чтобы разрешить пользователю выполнять отдельные методы концентратора.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-175">Now that the `HubInvocationContext` resource parameter is being passed in, the internal logic can inspect the context in which the Hub is being called and make decisions on allowing the user to execute individual Hub methods.</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}

public class DomainRestrictedRequirement : 
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>, 
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement, 
        HubInvocationContext resource)
    {
        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name) && 
            context.User.Identity.Name.EndsWith("@microsoft.com"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName,
        string currentUsername)
    {
        return !(currentUsername.Equals("asdf42@microsoft.com") && 
            hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase));
    }
}
```

<span data-ttu-id="c1ac2-176">В `Startup.ConfigureServices`добавьте новую политику, предоставив настраиваемое `DomainRestrictedRequirement` требование в качестве параметра для создания `DomainRestricted` политики.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-176">In `Startup.ConfigureServices`, add the new policy, providing the custom `DomainRestrictedRequirement` requirement as a parameter to create the `DomainRestricted` policy.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services
        .AddAuthorization(options =>
        {
            options.AddPolicy("DomainRestricted", policy =>
            {
                policy.Requirements.Add(new DomainRestrictedRequirement());
            });
        });
}
```

<span data-ttu-id="c1ac2-177">В предыдущем примере `DomainRestrictedRequirement` класс является `IAuthorizationRequirement` и, и его собственным `AuthorizationHandler` для этого требования.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-177">In the preceding example, the `DomainRestrictedRequirement` class is both an `IAuthorizationRequirement` and its own `AuthorizationHandler` for that requirement.</span></span> <span data-ttu-id="c1ac2-178">Можно разделить эти два компонента на отдельные классы для разделения проблем.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-178">It's acceptable to split these two components into separate classes to separate concerns.</span></span> <span data-ttu-id="c1ac2-179">Преимуществом этого примера является отсутствие необходимости внедрять `AuthorizationHandler` во время запуска, так как требование и обработчик одинаковы.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-179">A benefit of the example's approach is there's no need to inject the `AuthorizationHandler` during startup, as the requirement and the handler are the same thing.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="c1ac2-180">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c1ac2-180">Additional resources</span></span>

* [<span data-ttu-id="c1ac2-181">Проверка подлинности токена носителя в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1ac2-181">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="c1ac2-182">Авторизация на основе ресурсов</span><span class="sxs-lookup"><span data-stu-id="c1ac2-182">Resource-based Authorization</span></span>](xref:security/authorization/resourcebased)
