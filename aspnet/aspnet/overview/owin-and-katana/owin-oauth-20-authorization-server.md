---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: "Сервер авторизации OAuth 2.0 OWIN | Документы Microsoft"
author: hongyes
description: "Этот учебник поможет вам о том, как реализовать с помощью OWIN OAuth по промежуточного слоя сервера авторизации OAuth 2.0. Это является расширенный учебник, только Настройка..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e5968f8d19191c3f44e9bd58f8e22a39d8d8faff
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="owin-oauth-20-authorization-server"></a>Сервер авторизации OAuth 2.0 OWIN
====================
по [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет вам о том, как реализовать с помощью OWIN OAuth по промежуточного слоя сервера авторизации OAuth 2.0. Это расширенный учебник только приводятся действия для создания сервера авторизации OAuth 2.0 OWIN. Это пошаговое руководство. [Загрузить образец кода](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > Данная структура должна не предназначен для следует использовать для создания безопасного рабочего приложения. Этот учебник предназначен для предоставления только контур о том, как реализовать с помощью OWIN OAuth по промежуточного слоя сервера авторизации OAuth 2.0.
> 
> 
> ## <a name="software-versions"></a>Версии программного обеспечения
> 
> | **В этом учебнике показано** | **Также работает с** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio Express 2013 для Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012 с последним обновлением должна работать, но учебника не были проверены с ним, и некоторые пункты выбирает в меню и диалоговые окна отличаются. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, можно разместить их на [Katana проекта на GitHub](https://github.com/aspnet/AspNetKatana/). Вопросы и комментарии, касающиеся сам учебника см. в разделе комментариев в нижней части страницы.


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) позволяет сторонним приложением для получения ограниченного доступа для службы HTTP. Вместо того чтобы использовать учетные данные владельца ресурса для доступа к защищенному ресурсу, клиент получает маркер доступа (которое представляет собой строку отмечает конкретной области, время существования и другие атрибуты доступа). Маркеры доступа предоставляются клиентам сторонних сервера авторизации с утверждением владельца ресурса.

Этот учебник рассматривается:

- Создание сервера авторизации для поддержки авторизации, четыре типы предоставления и токенов обновления:
    - Предоставления кода авторизации
    - Неявного предоставления
    - Пароль владельца ресурса предоставлением учетных данных
    - Предоставьте учетные данные клиента
- Создание ресурсов сервера, защищенного маркер доступа.
- Создание клиентов OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Предварительные требования

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) или бесплатную [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), как указано в **версий программного обеспечения** в верхней части страницы.
- Знакомство с OWIN. В разделе [Приступая к работе с проектом Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) и [новые возможности OWIN и Katana](index.md).
- Знакомство с [OAuth](http://tools.ietf.org/html/rfc6749) терминологии, включая [ролей](http://tools.ietf.org/html/rfc6749#section-1.1), [потока протокола](http://tools.ietf.org/html/rfc6749#section-1.2), и [предоставления авторизации](http://tools.ietf.org/html/rfc6749#section-1.3). [Введение OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) предоставляет представление.

## <a name="create-an-authorization-server"></a>Создание сервера авторизации

В этом учебнике мы будет примерно эскиз, как использовать [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) и ASP.NET MVC для создания сервера авторизации. Мы надеемся, что скоро предлагать обновления для полного примера этот учебник содержит каждый шаг. Сначала создайте пустой веб-приложение с именем *AuthorizationServer* и установить следующие пакеты:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (или других социальных входа пакета, например Microsoft.Owin.Security.Facebook)

Добавить [класс запуска OWIN](owin-startup-class-detection.md) в корневой папке проекта с именем *запуска*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Создание *приложения\_запустить* папки. Выберите *приложения\_запустить* папки и используйте клавиши Shift + Alt + A для добавления загруженную версию набора *AuthorizationServer\App\_Start\Startup.Auth.cs* файла.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Приведенный выше код позволяет входа приложения или внешние файлы cookie и проверки подлинности Google, используемые сервером авторизации, сам требуется управление учетными записями.

`UseOAuthAuthorizationServer` Является метод расширения для настройки сервера авторизации. Доступны следующие варианты установки.

- `AuthorizeEndpointPath`: Путь запроса, где клиентские приложения будут перенаправлять агент пользователя для получения пользователей согласие на выдачу маркера или кода. Или начинаться со косой черты, например, «`/Authorize`».
- `TokenEndpointPath`: Запрос путь клиентские приложения взаимодействуют непосредственно для получения маркера доступа. Он должен начинаться с косой черты, например «/ Token». Если клиент выдает [клиента\_секрет](http://tools.ietf.org/html/rfc6749#appendix-A.2), оно должно быть предоставлено на эту конечную точку.
- `ApplicationCanDisplayErrors`: Значение `true` Если веб-приложение хочет создать собственную страницу ошибки для ошибки проверки клиента о `/Authorize` конечной точки. Требуется только для случаев, когда браузер не направляется обратно в клиентское приложение, например, когда `client_id` или `redirect_uri` неверны. `/Authorize` Конечную точку следует ожидать «oauth. Ошибка», «oauth. ErrorDescription» и «oauth. Свойства ErrorUri» добавляются в среду OWIN. 

    > [!NOTE]
    > В противном случае значение равно true, сервер авторизации Возвращает стандартную страницу ошибок с описанием ошибки.
- `AllowInsecureHttp`: Значение True для разрешить запросам авторизации и маркеров поступления адреса HTTP URI и чтобы разрешить входящие `redirect_uri` авторизовать параметры запроса для HTTP URI-адреса. 

    > [!WARNING]
    > Безопасность — это только для целей разработки.
- `Provider`: Объект, предоставляемый приложением для обработки событий, вызванных по промежуточного слоя сервера авторизации. Приложение может полностью реализовывать этот интерфейс, или он может создать экземпляр `OAuthAuthorizationServerProvider` и назначать делегаты, необходимые для потоков OAuth, данный сервер поддерживает.
- `AuthorizationCodeProvider`: Создает код авторизации однократного использования, чтобы вернуть клиентскому приложению. Для сервера OAuth приложение безопасного **должен** экземпляров для `AuthorizationCodeProvider` где маркер, созданный путем `OnCreate/OnCreateAsync` событие считается допустимым только для одного вызова для `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Создает маркер обновления, который может использоваться для создания нового маркера доступа, если это требуется. Если не указан сервер авторизации не возвратит маркеры обновления из `/Token` конечной точки.

## <a name="account-management"></a>Управление учетными записями

OAuth безразлично, где и как управлять ими данные учетной записи пользователя. Он имеет [ASP.NET Identity](../../../identity/index.md) который отвечает за его. В этом учебнике мы позволяют упростить код учетной записи управления и убедитесь, что этот пользователь может войти в систему по промежуточного слоя OWIN куки-файл. Ниже приведен упрощенный пример кода для `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri`используется для проверки клиента с его URL-адрес зарегистрированного перенаправления. `ValidateClientAuthentication`проверяет основные схемы заголовок и текст формы для получения учетных данных клиента.

Ниже приводится на страницу входа.

![](owin-oauth-20-authorization-server/_static/image1.png)

Просмотрите IETF OAuth 2 [Authorization Code Grant](http://tools.ietf.org/html/rfc6749#section-4.1) теперь статьи. 

**Поставщик** (в таблице ниже) — [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Поставщик, который относится к типу `OAuthAuthorizationServerProvider`, который содержит все события сервера OAuth. 

| Действия из раздела Authorization Code Grant | Загрузить образец выполняет следующие действия с. |
| --- | --- |
|  |  |
| (A) клиент инициирует поток путем направления владельца ресурса агента пользователя к конечной точке авторизации. Клиент включает его идентификатор клиента, запрошенную область, локальное состояние и URI перенаправления, к которому сервер авторизации будет отправлять агенту пользователя после предоставляется (или запрещен) доступ. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (Б) сервер авторизации проверяет подлинность владельца ресурса (с помощью агента пользователя) и устанавливает ли владелец ресурсов предоставляет или запрещает доступ запроса клиента. | **&lt;Если пользователь разрешает доступ&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) при условии, что владелец ресурса предоставил доступ, сервер авторизации перенаправляет агента пользователя обратно клиенту с помощью идентификатора URI перенаправления предыдущих (в запросе или во время регистрации клиента). ... |  |
|  |  |
| (D) клиент запрашивает токен доступа из сервера авторизации маркера конечной точки, включая код авторизации, полученный на предыдущем шаге. При выполнении запроса, клиент проходит проверку подлинности на сервере авторизации. Клиент включает перенаправления, которое URI, используемый для получения кода авторизации для проверки. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Пример реализации `AuthorizationCodeProvider.CreateAsync` и `ReceiveAsync` для управления созданием и проверки кода авторизации, показано ниже.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Приведенный выше код использует параллельный словарь в памяти для сохранения билета код и удостоверений и восстановления удостоверение после получения кода. В реальном приложении он будет заменен хранилищем постоянных данных. Конечная точка авторизации предназначен для владельца ресурса для предоставления доступа клиенту. Как правило он должен пользовательский интерфейс, чтобы разрешить пользователю путем нажатия на кнопку и подтвердите прав. По промежуточного слоя OWIN OAuth позволяет коду приложения для обработки конечной точке авторизации. В нашем примере приложения мы используем контроллер MVC вызывается `OAuthController` его обработать. Ниже приведен пример реализации.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize` Действия сначала будет проверять, если пользователь выполнил вход на сервер авторизации. В противном случае промежуточного по проверки подлинности предлагает вызывающего объекта для проверки подлинности с помощью cookie «Приложение» и выполняет перенаправление на страницу входа. (См. выше выделенный код). Если пользователь выполнил вход, он будет отображать представление Authorize, как показано ниже:

![](owin-oauth-20-authorization-server/_static/image2.png)

Если **Grant** выбран переключатель, `Authorize` будет создан новый удостоверения «Bearer» и выполните вход с помощью его. Активирует сервер авторизации для создания токена носителя и отправить его клиенту с полезными данными JSON. 

### <a name="implicit-grant"></a>Неявного предоставления

Ссылаться на IETF OAuth 2 [неявного предоставления](http://tools.ietf.org/html/rfc6749#section-4.2) теперь статьи.

 [Неявного предоставления](http://tools.ietf.org/html/rfc6749#section-4.2) потока, показанный на рисунке 4 поток и сопоставление которого OWIN OAuth следует по промежуточного слоя.  

| Действия из раздела неявного предоставления | Загрузить образец выполняет следующие действия с. |
| --- | --- |
|  |  |
| (A) клиент инициирует поток путем направления владельца ресурса агента пользователя к конечной точке авторизации. Клиент включает его идентификатор клиента, запрошенную область, локальное состояние и URI перенаправления, к которому сервер авторизации будет отправлять агенту пользователя после предоставляется (или запрещен) доступ. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (Б) сервер авторизации проверяет подлинность владельца ресурса (с помощью агента пользователя) и устанавливает ли владелец ресурсов предоставляет или запрещает доступ запроса клиента. | **&lt;Если пользователь разрешает доступ&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) при условии, что владелец ресурса предоставил доступ, сервер авторизации перенаправляет агента пользователя обратно клиенту с помощью идентификатора URI перенаправления предыдущих (в запросе или во время регистрации клиента). ... |  |
|  |  |
| (D) клиент запрашивает токен доступа из сервера авторизации маркера конечной точки, включая код авторизации, полученный на предыдущем шаге. При выполнении запроса, клиент проходит проверку подлинности на сервере авторизации. Клиент включает перенаправления, которое URI, используемый для получения кода авторизации для проверки. |  |

Так как мы уже реализован в конечной точке авторизации (`OAuthController.Authorize` действия) для предоставления кода авторизации, оно автоматически включает также неявного потока. Примечание: `Provider.ValidateClientRedirectUri` используется для проверки идентификатора клиента с его URL-адрес перенаправления, который защищает неявного предоставления потока из маркера для злонамеренных клиентам, отправляющим доступ ([в третьего](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Пароль владельца ресурса предоставлением учетных данных

Ссылаться на IETF OAuth 2 [предоставления учетных данных пароля владельца ресурса](http://tools.ietf.org/html/rfc6749#section-4.3) теперь статьи.

 [Предоставления учетных данных пароля владельца ресурса](http://tools.ietf.org/html/rfc6749#section-4.3) потока показано на рисунке 5 потоке и сопоставление которого OWIN OAuth следует по промежуточного слоя.  

| Действия из раздела предоставления учетных данных пароля владельца ресурса | Загрузить образец выполняет следующие действия с. |
| --- | --- |
|  |  |
| (A) владельца ресурса предоставляет клиенту имя пользователя и пароль. |  |
|  |  |
| (Б) клиент запрашивает токен доступа из сервера авторизации маркера конечной точки, включая учетные данные, полученные от владельца ресурса. При выполнении запроса, клиент проходит проверку подлинности на сервере авторизации. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) сервер авторизации проверяет подлинность клиента и проверяет учетные данные владельца ресурса результата проверки, выдает маркер доступа. |  |

Ниже приведен пример реализации `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Приведенный выше код объясняются в этом разделе учебника и не должны использоваться в безопасных или производственного приложения. Он не проверяет учетные данные владельцев ресурса. Предполагается, что каждый учетных данных является допустимым и создает новое удостоверение для него. Нового удостоверения будет использоваться для создания токена доступа и токена обновления. Замените код коде управления защищенную учетную запись.


### <a name="client-credentials-grant"></a>Предоставьте учетные данные клиента

Ссылаться на IETF OAuth 2 [предоставления учетных данных клиента](http://tools.ietf.org/html/rfc6749#section-4.4) теперь статьи.

 [Предоставления учетных данных клиента](http://tools.ietf.org/html/rfc6749#section-4.4) потока, показанный на рисунке 6 потоке и сопоставление которого OWIN OAuth следует по промежуточного слоя.  

| Действия из раздела предоставления учетных данных клиента | Загрузить образец выполняет следующие действия с. |
| --- | --- |
|  |  |
| (A) клиент использует для проверки подлинности сервера авторизации и запрашивает токен доступа из конечной точки токена. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (Б) сервер авторизации проверяет подлинность клиента, а если допустимым, проблемы маркер доступа. |  |

Ниже приведен пример реализации `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Приведенный выше код объясняются в этом разделе учебника и не должны использоваться в безопасных или производственного приложения. Замените код коде управления безопасности клиента.


### <a name="refresh-token"></a>Токен обновления

Ссылаться на IETF OAuth 2 [токен обновления](http://tools.ietf.org/html/rfc6749#section-1.5) теперь статьи.

 [Токен обновления](http://tools.ietf.org/html/rfc6749#section-1.5) потока, показанный на рисунке 2 потока и сопоставление которого OWIN OAuth следует по промежуточного слоя.  

| Действия из раздела предоставления учетных данных клиента | Загрузить образец выполняет следующие действия с. |
| --- | --- |
|  |  |
| (G) клиент запрашивает новый маркер доступа путем проверки подлинности на сервере авторизации и токен обновления представления. Требования к проверке подлинности клиента основаны на тип клиента и сервера политики авторизации. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) сервер авторизации проверяет подлинность клиента и проверяет токен обновления и допустимым, выдает новый маркер доступа (и при необходимости новый маркер обновления). |  |

Ниже приведен пример реализации `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Создание ресурсов сервера, защищенного токена доступа

Создайте пустой веб-узел проекта приложения и установить следующие пакеты в проекте:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Создайте класс запуска и настройка проверки подлинности и веб-API. В разделе *AuthorizationServer\ResourceServer\Startup.cs* в состав примера.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

В разделе *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* в состав примера.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

В разделе *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* в состав примера.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors`метод позволяет CORS для всех доменов.
- `UseOAuthBearerAuthentication`метод, который позволяет промежуточного слоя проверки подлинности маркера OAuth носителей, который будет получать и проверить токен носителя из заголовка авторизации в запросе.
- `Config.SuppressDefaultHostAuthenticaiton`по умолчанию подавляет размещения авторизованный участник из приложения, поэтому все запросы будут анонимный после этого вызова.
- `HostAuthenticationFilter`включает проверку подлинности только для типа проверки подлинности. В этом случае он имеет тип проверки подлинности носителя.

Чтобы продемонстрировать проверку подлинности, мы создадим ApiController для вывода утверждений текущего пользователя.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Если сервер авторизации и сервер ресурсов не на том же компьютере, OAuth по промежуточного слоя будет использовать различные машинные ключи для шифрования и расшифровки маркера доступа носителя. Для совместного использования одного закрытого ключа между обоих проектов, мы добавить тот же `machinekey` в оба *web.config* файлов.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Создание клиентов OAuth 2.0

 Мы используем [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) пакет NuGet для упрощения кода клиента.

### <a name="authorization-code-grant-client"></a>Клиент предоставления кода авторизации

 Этот клиент является приложением MVC. Активирует поток предоставления кода авторизации для получения токена доступа из базы данных. Он состоит из одной страницы, как показано ниже:

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Авторизовать** кнопка будет перенаправлять браузер на сервере авторизации, чтобы уведомить владельца ресурса для предоставления доступа к этим клиентом.
- **Обновление** кнопка получит новый маркер доступа и токен обновления, с помощью текущего обновления маркера.
- **Защищенных ресурсов API-Интерфейс** кнопка будет вызывать сервер ресурсов, чтобы получать данные утверждения текущего пользователя и отображать их на странице.

Ниже приведен пример кода из `HomeController` клиента.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth`требуется протокол SSL по умолчанию. Поскольку наш Демонстрация использует HTTP, необходимо добавить следующий параметр в файле конфигурации:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Безопасность — никогда не отключить SSL в рабочем приложении. Учетные данные входа теперь отправляются в открытым текстом по каналу связи. Приведенный выше код является только для отладки локальный образца и просмотра.


### <a name="implicit-grant-client"></a>Неявное предоставление клиента

Этот клиент использует JavaScript для:

1. Откройте новое окно и перенаправления для конечной точки authorize сервера авторизации.
2. Получение маркера доступа из фрагментов URL-адрес при перенаправлении обратно.

Этот процесс показан на следующем рисунке:

![](owin-oauth-20-authorization-server/_static/image4.png)

Клиент должен иметь две страницы: один для домашней страницы, а другой — для обратного вызова. Ниже приведен пример кода JavaScript *Index.cshtml* файла:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Вот обратного вызова, обработки кода в *SignIn.cshtml* файла:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Рекомендуется переместить во внешний файл JavaScript и не внедрен с разметкой Razor. Для простоты в этом примере их объединения.


### <a name="resource-owner-password-credentials-grant-client"></a>Пароль владельца ресурса учетных данных клиента Grant

Мы использует консольное приложение для демонстрации этого клиента. Ниже приведен код:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Клиент предоставить учетные данные клиента

Как и для предоставления учетных данных пароля владельца ресурса, вот код приложения консоли.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
