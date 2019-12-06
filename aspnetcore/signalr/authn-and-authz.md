---
title: Проверка подлинности и авторизация в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как использовать проверку подлинности и авторизацию в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- SignalR
uid: signalr/authn-and-authz
ms.openlocfilehash: 091cc9b2adc1f6a8fac79519884695d1c1725d2a
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880421"
---
# <a name="authentication-and-authorization-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="e7bae-103">Проверка подлинности и авторизация в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e7bae-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="e7bae-104">[Эндрю Стантон-медперсонала](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="e7bae-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="e7bae-105">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(Загрузка)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="e7bae-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-opno-locsignalr-hub"></a><span data-ttu-id="e7bae-106">Проверка подлинности пользователей, подключающихся к концентратору SignalR</span><span class="sxs-lookup"><span data-stu-id="e7bae-106">Authenticate users connecting to a SignalR hub</span></span>

SignalR<span data-ttu-id="e7bae-107"> можно использовать с [проверкой подлинности ASP.NET Core](xref:security/authentication/identity) , чтобы связать пользователя с каждым соединением.</span><span class="sxs-lookup"><span data-stu-id="e7bae-107"> can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="e7bae-108">В центре данные проверки подлинности можно получить из свойства [хубконнектионконтекст. User](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) .</span><span class="sxs-lookup"><span data-stu-id="e7bae-108">In a hub, authentication data can be accessed from the [HubConnectionContext.User](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="e7bae-109">Проверка подлинности позволяет концентратору вызывать методы для всех соединений, связанных с пользователем.</span><span class="sxs-lookup"><span data-stu-id="e7bae-109">Authentication allows the hub to call methods on all connections associated with a user.</span></span> <span data-ttu-id="e7bae-110">Дополнительные сведения см. [в разделе Управление пользователями и группами в SignalR](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="e7bae-110">For more information, see [Manage users and groups in SignalR](xref:signalr/groups).</span></span> <span data-ttu-id="e7bae-111">Несколько подключений могут быть связаны с одним пользователем.</span><span class="sxs-lookup"><span data-stu-id="e7bae-111">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="e7bae-112">Ниже приведен пример `Startup.Configure`, в котором используется проверка подлинности SignalR и ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e7bae-112">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="e7bae-113">Порядок, в котором регистрируется SignalR и ASP.NET Core по промежуточного слоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="e7bae-113">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="e7bae-114">Всегда вызывайте `UseAuthentication` перед `UseSignalR`, чтобы у SignalR был пользователь `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="e7bae-114">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

::: moniker-end

### <a name="cookie-authentication"></a><span data-ttu-id="e7bae-115">Проверка подлинности файлов cookie</span><span class="sxs-lookup"><span data-stu-id="e7bae-115">Cookie authentication</span></span>

<span data-ttu-id="e7bae-116">В приложении, основанном на браузере, проверка подлинности файлов cookie позволяет автоматически передавать существующие учетные данные пользователя в SignalR подключения.</span><span class="sxs-lookup"><span data-stu-id="e7bae-116">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="e7bae-117">При использовании клиента браузера дополнительная настройка не требуется.</span><span class="sxs-lookup"><span data-stu-id="e7bae-117">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="e7bae-118">Если пользователь вошел в приложение, SignalRное подключение автоматически наследует эту проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="e7bae-118">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="e7bae-119">Файлы cookie — это ориентированный на браузер способ отправки маркеров доступа, но клиенты, не являющиеся браузерами, могут их отправить.</span><span class="sxs-lookup"><span data-stu-id="e7bae-119">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="e7bae-120">При использовании [клиента .NET](xref:signalr/dotnet-client)свойство `Cookies` может быть настроено в `.WithUrl` вызове для предоставления файла cookie.</span><span class="sxs-lookup"><span data-stu-id="e7bae-120">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call to provide a cookie.</span></span> <span data-ttu-id="e7bae-121">Однако для использования проверки подлинности файлов cookie из клиента .NET приложение должно предоставить API для обмена данными проверки подлинности для файла cookie.</span><span class="sxs-lookup"><span data-stu-id="e7bae-121">However, using cookie authentication from the .NET client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="e7bae-122">Проверка подлинности токена носителя</span><span class="sxs-lookup"><span data-stu-id="e7bae-122">Bearer token authentication</span></span>

<span data-ttu-id="e7bae-123">Клиент может предоставить маркер доступа вместо использования файла cookie.</span><span class="sxs-lookup"><span data-stu-id="e7bae-123">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="e7bae-124">Сервер проверяет маркер и использует его для обнаружения пользователя.</span><span class="sxs-lookup"><span data-stu-id="e7bae-124">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="e7bae-125">Эта проверка выполняется только при установленном соединении.</span><span class="sxs-lookup"><span data-stu-id="e7bae-125">This validation is done only when the connection is established.</span></span> <span data-ttu-id="e7bae-126">Во время жизни соединения сервер не выполняет автоматическую повторную проверку, чтобы проверить отзыв маркера.</span><span class="sxs-lookup"><span data-stu-id="e7bae-126">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="e7bae-127">На сервере проверка подлинности токена носителя настраивается с помощью по [промежуточного слоя JWT Bearer](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="e7bae-127">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="e7bae-128">В клиенте JavaScript маркер можно указать с помощью параметра [акцесстокенфактори](xref:signalr/configuration#configure-bearer-authentication) .</span><span class="sxs-lookup"><span data-stu-id="e7bae-128">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=52-55)]

<span data-ttu-id="e7bae-129">В клиенте .NET существует аналогичное свойство [акцесстокенпровидер](xref:signalr/configuration#configure-bearer-authentication) , которое можно использовать для настройки маркера:</span><span class="sxs-lookup"><span data-stu-id="e7bae-129">In the .NET client, there's a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="e7bae-130">Предоставляемая функция токена доступа вызывается перед **каждым** HTTP-запросом, сделанным SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7bae-130">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="e7bae-131">Если необходимо обновить токен, чтобы подключение было активно (так как оно может истечь во время подключения), сделайте это в этой функции и возвратите обновленный маркер.</span><span class="sxs-lookup"><span data-stu-id="e7bae-131">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="e7bae-132">В стандартных веб-API токены носителя отправляются в заголовке HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7bae-132">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="e7bae-133">Однако SignalR не может задать эти заголовки в браузерах при использовании некоторых транспортов.</span><span class="sxs-lookup"><span data-stu-id="e7bae-133">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="e7bae-134">При использовании соединений WebSockets и серверных событий токен передается как параметр строки запроса.</span><span class="sxs-lookup"><span data-stu-id="e7bae-134">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="e7bae-135">Для поддержки этого на сервере требуется дополнительная настройка:</span><span class="sxs-lookup"><span data-stu-id="e7bae-135">To support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

> [!NOTE]
> <span data-ttu-id="e7bae-136">Строка запроса используется в браузерах при соединении с веб-сокетами и событиями, отправленными сервером из-за ограничений API браузера.</span><span class="sxs-lookup"><span data-stu-id="e7bae-136">The query string is used on browsers when connecting with WebSockets and Server-Sent Events due to browser API limitations.</span></span> <span data-ttu-id="e7bae-137">При использовании HTTPS значения строки запроса защищаются с помощью подключения TLS.</span><span class="sxs-lookup"><span data-stu-id="e7bae-137">When using HTTPS, query string values are secured by the TLS connection.</span></span> <span data-ttu-id="e7bae-138">Однако многие серверы заносить в журнал значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="e7bae-138">However, many servers log query string values.</span></span> <span data-ttu-id="e7bae-139">Дополнительные сведения см. [в разделе вопросы безопасности в ASP.NET Core SignalR](xref:signalr/security).</span><span class="sxs-lookup"><span data-stu-id="e7bae-139">For more information, see [Security considerations in ASP.NET Core SignalR](xref:signalr/security).</span></span> SignalR<span data-ttu-id="e7bae-140"> использует заголовки для передачи токенов в средах, которые их поддерживают (например, клиенты .NET и Java).</span><span class="sxs-lookup"><span data-stu-id="e7bae-140"> uses headers to transmit tokens in environments which support them (such as the .NET and Java clients).</span></span>

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="e7bae-141">Файлы cookie и токены носителя</span><span class="sxs-lookup"><span data-stu-id="e7bae-141">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="e7bae-142">Файлы cookie относятся только к браузерам.</span><span class="sxs-lookup"><span data-stu-id="e7bae-142">Cookies are specific to browsers.</span></span> <span data-ttu-id="e7bae-143">Их отправка из других типов клиентов повышает сложность по сравнению с отправкой токенов носителя.</span><span class="sxs-lookup"><span data-stu-id="e7bae-143">Sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="e7bae-144">Следовательно, проверка подлинности файлов cookie не рекомендуется, если приложению не требуется проверять подлинность пользователей только от клиента браузера.</span><span class="sxs-lookup"><span data-stu-id="e7bae-144">Consequently, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="e7bae-145">Проверка подлинности маркера носителя является рекомендуемым подходом при использовании клиентов, отличных от клиента браузера.</span><span class="sxs-lookup"><span data-stu-id="e7bae-145">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="e7bae-146">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="e7bae-146">Windows authentication</span></span>

<span data-ttu-id="e7bae-147">Если в приложении настроена [Проверка подлинности Windows](xref:security/authentication/windowsauth) , SignalR может использовать это удостоверение для защиты концентраторов.</span><span class="sxs-lookup"><span data-stu-id="e7bae-147">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="e7bae-148">Однако для отправки сообщений отдельным пользователям необходимо добавить настраиваемого поставщика ИДЕНТИФИКАТОРов пользователей.</span><span class="sxs-lookup"><span data-stu-id="e7bae-148">However, to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="e7bae-149">Система проверки подлинности Windows не предоставляет утверждение "идентификатор имени".</span><span class="sxs-lookup"><span data-stu-id="e7bae-149">The Windows authentication system doesn't provide the "Name Identifier" claim.</span></span> SignalR<span data-ttu-id="e7bae-150"> использует утверждение для определения имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="e7bae-150"> uses the claim to determine the user name.</span></span>

<span data-ttu-id="e7bae-151">Добавьте новый класс, реализующий `IUserIdProvider`, и получите одно из утверждений от пользователя, которое будет использоваться в качестве идентификатора.</span><span class="sxs-lookup"><span data-stu-id="e7bae-151">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="e7bae-152">Например, чтобы использовать утверждение "Name" (имя пользователя Windows в форме `[Domain]\[Username]`), создайте следующий класс:</span><span class="sxs-lookup"><span data-stu-id="e7bae-152">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="e7bae-153">Вместо `ClaimTypes.Name`можно использовать любое значение из `User` (например, идентификатор SID Windows и т. д.).</span><span class="sxs-lookup"><span data-stu-id="e7bae-153">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, and so on).</span></span>

> [!NOTE]
> <span data-ttu-id="e7bae-154">Выбранное значение должно быть уникальным для всех пользователей в системе.</span><span class="sxs-lookup"><span data-stu-id="e7bae-154">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="e7bae-155">В противном случае сообщение, предназначенное для одного пользователя, может завершить работу другого пользователя.</span><span class="sxs-lookup"><span data-stu-id="e7bae-155">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="e7bae-156">Зарегистрируйте этот компонент в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e7bae-156">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="e7bae-157">В клиенте .NET проверка подлинности Windows должна быть включена путем установки свойства [уседефаулткредентиалс](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) :</span><span class="sxs-lookup"><span data-stu-id="e7bae-157">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="e7bae-158">Проверка подлинности Windows поддерживается только клиентом браузера при использовании Microsoft Internet Explorer или Microsoft погранично.</span><span class="sxs-lookup"><span data-stu-id="e7bae-158">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="e7bae-159">Использование утверждений для настройки обработки удостоверений</span><span class="sxs-lookup"><span data-stu-id="e7bae-159">Use claims to customize identity handling</span></span>

<span data-ttu-id="e7bae-160">Приложение, выполняющее проверку подлинности пользователей, может наследовать идентификаторы пользователей SignalR от утверждений пользователей.</span><span class="sxs-lookup"><span data-stu-id="e7bae-160">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="e7bae-161">Чтобы указать, как SignalR создает идентификаторы пользователей, реализуйте `IUserIdProvider` и зарегистрируйте реализацию.</span><span class="sxs-lookup"><span data-stu-id="e7bae-161">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="e7bae-162">В примере кода показано использование утверждений для выбора адреса электронной почты пользователя в качестве идентифицирующего свойства.</span><span class="sxs-lookup"><span data-stu-id="e7bae-162">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="e7bae-163">Выбранное значение должно быть уникальным для всех пользователей в системе.</span><span class="sxs-lookup"><span data-stu-id="e7bae-163">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="e7bae-164">В противном случае сообщение, предназначенное для одного пользователя, может завершить работу другого пользователя.</span><span class="sxs-lookup"><span data-stu-id="e7bae-164">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="e7bae-165">Регистрация учетной записи добавляет утверждение с типом `ClaimsTypes.Email` в базу данных удостоверений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e7bae-165">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="e7bae-166">Зарегистрируйте этот компонент в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e7bae-166">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="e7bae-167">Авторизация пользователей для доступа к концентраторам и методам концентратора</span><span class="sxs-lookup"><span data-stu-id="e7bae-167">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="e7bae-168">По умолчанию все методы в концентраторе могут вызываться пользователем, не прошедшим проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="e7bae-168">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="e7bae-169">Чтобы требовать проверку подлинности, примените к концентратору атрибут [авторизации](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) :</span><span class="sxs-lookup"><span data-stu-id="e7bae-169">To require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="e7bae-170">Можно использовать аргументы конструктора и свойства атрибута `[Authorize]`, чтобы ограничить доступ только тем пользователям, которые соответствуют определенным [политикам авторизации](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="e7bae-170">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="e7bae-171">Например, если имеется пользовательская политика авторизации с именем `MyAuthorizationPolicy` можно убедиться, что только пользователи, соответствующие этой политике, могут получить доступ к концентратору, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="e7bae-171">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

<span data-ttu-id="e7bae-172">Отдельные методы концентратора могут также применять атрибут `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="e7bae-172">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="e7bae-173">Если текущий пользователь не соответствует политике, примененной к методу, вызывающему объекту будет возвращена ошибка.</span><span class="sxs-lookup"><span data-stu-id="e7bae-173">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a><span data-ttu-id="e7bae-174">Использование обработчиков авторизации для настройки авторизации метода концентратора</span><span class="sxs-lookup"><span data-stu-id="e7bae-174">Use authorization handlers to customize hub method authorization</span></span>

SignalR<span data-ttu-id="e7bae-175"> предоставляет обработчику авторизации пользовательский ресурс, когда методу концентратора требуется авторизация.</span><span class="sxs-lookup"><span data-stu-id="e7bae-175"> provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="e7bae-176">Ресурс является экземпляром `HubInvocationContext`.</span><span class="sxs-lookup"><span data-stu-id="e7bae-176">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="e7bae-177">`HubInvocationContext` включает `HubCallerContext`, имя вызываемого метода концентратора и аргументы для метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="e7bae-177">The `HubInvocationContext` includes the `HubCallerContext`, the name of the hub method being invoked, and the arguments to the hub method.</span></span>

<span data-ttu-id="e7bae-178">Рассмотрим пример комнаты чатов, позволяющей нескольким организациям входить в систему через Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e7bae-178">Consider the example of a chat room allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="e7bae-179">Любой пользователь, у которого есть учетная запись Майкрософт, может войти в чат, но только члены владеющей Организации должны иметь возможность запрещать пользователям или просматривать историю разговора пользователей.</span><span class="sxs-lookup"><span data-stu-id="e7bae-179">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization should be able to ban users or view users' chat histories.</span></span> <span data-ttu-id="e7bae-180">Кроме того, может потребоваться ограничить некоторые функции от определенных пользователей.</span><span class="sxs-lookup"><span data-stu-id="e7bae-180">Furthermore, we might want to restrict certain functionality from certain users.</span></span> <span data-ttu-id="e7bae-181">Использование обновленных функций в ASP.NET Core 3,0 — это вполне возможно.</span><span class="sxs-lookup"><span data-stu-id="e7bae-181">Using the updated features in ASP.NET Core 3.0, this is entirely possible.</span></span> <span data-ttu-id="e7bae-182">Обратите внимание, что `DomainRestrictedRequirement` выступает в качестве пользовательского `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="e7bae-182">Note how the `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="e7bae-183">Теперь, когда передается параметр ресурса `HubInvocationContext`, внутренняя логика может проверить контекст, в котором вызывается концентратор, и принимать решения о том, чтобы разрешить пользователю выполнять отдельные методы концентратора.</span><span class="sxs-lookup"><span data-stu-id="e7bae-183">Now that the `HubInvocationContext` resource parameter is being passed in, the internal logic can inspect the context in which the Hub is being called and make decisions on allowing the user to execute individual Hub methods.</span></span>

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

<span data-ttu-id="e7bae-184">В `Startup.ConfigureServices`добавьте новую политику, предоставив настраиваемое требование к `DomainRestrictedRequirement` в качестве параметра для создания политики `DomainRestricted`.</span><span class="sxs-lookup"><span data-stu-id="e7bae-184">In `Startup.ConfigureServices`, add the new policy, providing the custom `DomainRestrictedRequirement` requirement as a parameter to create the `DomainRestricted` policy.</span></span>

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

<span data-ttu-id="e7bae-185">В предыдущем примере класс `DomainRestrictedRequirement` является как `IAuthorizationRequirement`, так и собственным `AuthorizationHandler` для этого требования.</span><span class="sxs-lookup"><span data-stu-id="e7bae-185">In the preceding example, the `DomainRestrictedRequirement` class is both an `IAuthorizationRequirement` and its own `AuthorizationHandler` for that requirement.</span></span> <span data-ttu-id="e7bae-186">Можно разделить эти два компонента на отдельные классы для разделения проблем.</span><span class="sxs-lookup"><span data-stu-id="e7bae-186">It's acceptable to split these two components into separate classes to separate concerns.</span></span> <span data-ttu-id="e7bae-187">Преимуществом этого примера является отсутствие необходимости внедрять `AuthorizationHandler` во время запуска, так как требование и обработчик одинаковы.</span><span class="sxs-lookup"><span data-stu-id="e7bae-187">A benefit of the example's approach is there's no need to inject the `AuthorizationHandler` during startup, as the requirement and the handler are the same thing.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e7bae-188">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e7bae-188">Additional resources</span></span>

* [<span data-ttu-id="e7bae-189">Проверка подлинности токена носителя в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7bae-189">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="e7bae-190">Авторизация на основе ресурсов</span><span class="sxs-lookup"><span data-stu-id="e7bae-190">Resource-based Authorization</span></span>](xref:security/authorization/resourcebased)
