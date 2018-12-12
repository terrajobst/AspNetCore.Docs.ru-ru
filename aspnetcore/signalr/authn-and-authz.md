---
title: Проверка подлинности и авторизация в ASP.NET Core SignalR
author: tdykstra
description: Узнайте, как использовать проверку подлинности и авторизации в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: 6624b2b4361eb0a5bb6e45108878ee6fe5ecb443
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284435"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>Проверка подлинности и авторизация в ASP.NET Core SignalR

По [Andrew Stanton медсестра](https://twitter.com/anurse)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(способ загрузки)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>Проверка подлинности пользователей, подключающихся к концентратору SignalR

SignalR может использоваться с [проверки подлинности ASP.NET Core](xref:security/authentication/identity) чтобы связать пользователя с каждым подключением. В центре данных для проверки подлинности может осуществляться из [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) свойство. Проверка подлинности позволяет вызывать методы для всех соединений, связанный с пользователем в центре (см. в разделе [управления пользователями и группами в SignalR](xref:signalr/groups) Дополнительные сведения). Несколько соединений могут быть связаны с одним пользователем.

### <a name="cookie-authentication"></a>Файл cookie проверки подлинности

В приложении на основе веб-обозревателя файл cookie проверки подлинности позволяет свои существующие учетные данные пользователя, автоматически обмениваться с подключениями SignalR. При использовании клиентского браузера, без дополнительных настроек не требуется. Если пользователь вошел в приложение, подключении SignalR автоматически наследует этой проверки подлинности.

Файлы cookie представляют собой обозревателем позволяют отправлять маркеры доступа, но их можно отправлять клиентов без браузера. При использовании [клиента .NET](xref:signalr/dotnet-client), `Cookies` свойство можно задать в `.WithUrl` вызов, чтобы предоставить файл cookie. Тем не менее с использованием файла cookie проверки подлинности из клиента .NET требуется приложению предоставлять API для обмена данными проверки подлинности файла cookie.

### <a name="bearer-token-authentication"></a>Маркер проверки подлинности носителя

Клиент может предоставить маркер доступа, вместо того чтобы использовать файл cookie. Сервер проверяет маркер и использует его для идентификации пользователя. Эта проверка выполняется только в том случае, если соединение установлено. В течение жизненного цикла соединения сервер не повторную сверку автоматически для проверки маркера.

На сервере проверки подлинности маркера носителя настраивается с помощью [по промежуточного слоя носителя JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

В клиенте JavaScript, токен можно указать с помощью [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) параметр.

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

В клиенте .NET, отсутствует, аналогичное [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) свойство, которое может использоваться для настройки маркера:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> Функция маркера доступа, вами вызывается перед **каждые** HTTP-запроса, сделанных SignalR. Если необходимо обновлять маркер, чтобы сохранить подключение active (так как он может истечь срок действия во время соединения), выполнять из этой функции и возвращают обновленный маркер.

В стандартных веб-API в заголовке HTTP отправляются токены носителя. Тем не менее SignalR не удается задать эти заголовки в браузерах, при использовании некоторых транспортов. При использовании WebSockets и Server-Sent события, маркер передается как параметр строки запроса. Чтобы обеспечить это, на сервере, необходима дополнительная настройка:

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a>Файлы cookie и токены носителя 

Поскольку файлы cookie являются специфическими для браузеров, их отправкой из других типов клиентов повышает сложность системы, по сравнению с отправкой токены носителя. По этой причине файл cookie проверки подлинности не рекомендуется, если приложению достаточно только для проверки подлинности пользователей из клиента браузера. Токена проверки подлинности носителя — это рекомендуемый подход, при использовании клиентов, отличных от браузера клиента.

### <a name="windows-authentication"></a>Аутентификация Windows

Если [проверки подлинности Windows](xref:security/authentication/windowsauth) настраивается в приложении, SignalR можно использовать его для обеспечения безопасности концентраторы. Тем не менее чтобы отправлять сообщения отдельным пользователям, необходимо добавить пользовательский поставщик идентификатора пользователя. Это потому, что система проверки подлинности Windows не предоставляет утверждение «Идентификатор имени», SignalR использует, чтобы определить имя пользователя.

Добавьте новый класс, реализующий `IUserIdProvider` и получить одно из утверждений от пользователя для использования в качестве идентификатора. Например, чтобы использовать утверждение «Name» (это имя пользователя Windows в виде `[Domain]\[Username]`), создайте следующий класс:

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

Вместо `ClaimTypes.Name`, можно использовать любое значение от `User` (такие как идентификатор Windows SID и т.д.).

> [!NOTE]
> Значение, выбираемый должно быть уникальным среди всех пользователей в вашей системе. В противном случае сообщение, предназначенное для одного пользователя могут оказаться собираетесь другого пользователя.

Зарегистрировать этот компонент в вашей `Startup.ConfigureServices` метод **после** вызов `.AddSignalR`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

В клиенте .NET необходимо включить проверку подлинности Windows, задав [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) свойство:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

Проверка подлинности Windows поддерживается только с помощью браузера клиента, при использовании Microsoft Internet Explorer или Microsoft Edge.

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>Авторизация пользователей для доступа к концентраторам и методам концентраторов

По умолчанию все методы в концентраторе, могут вызываться пользователем, не прошедшие проверку подлинности. Чтобы требовать проверку подлинности, применить [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) атрибут к центру:

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

Можно использовать конструктор аргументов и свойств `[Authorize]` атрибута для ограничения доступа к только пользователям, которые соответствуют определенной [политик авторизации](xref:security/authorization/policies). Например, если у вас есть пользовательской политики авторизации вызывается `MyAuthorizationPolicy` убедитесь, что только пользователи, которые соответствуют этой политике можно получить доступ к концентратора, используя следующий код:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

Иной концентратор методы могут иметь `[Authorize]` также применен атрибут. Если текущий пользователь не соответствует политике, применяемый к методу, вызывающему объекту возвращается ошибка:

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Маркер проверки подлинности носителя в ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
