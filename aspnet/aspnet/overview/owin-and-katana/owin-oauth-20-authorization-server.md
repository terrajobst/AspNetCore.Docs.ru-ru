---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Сервер авторизации OAuth 2.0 OWIN | Документация Майкрософт
author: hongyes
description: Этот учебник поможет вам о том, как реализовать сервер авторизации OAuth 2.0 с помощью по промежуточного слоя OWIN OAuth. Это является расширенный учебник, только Настройка...
ms.author: aspnetcontent
ms.date: 03/20/2014
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e3b5b37b4f22f3c59d3c1f4043e9b52e46a8926b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828423"
---
<a name="owin-oauth-20-authorization-server"></a>Сервер авторизации OAuth 2.0 OWIN
====================
по [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет вам о том, как реализовать сервер авторизации OAuth 2.0 с помощью по промежуточного слоя OWIN OAuth. Это дополнительно учебник, в котором только описаны шаги, чтобы создать сервер авторизации OAuth 2.0 OWIN. Это пошаговое руководство. [Загрузить образец кода](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > Этой структуры не должен быть предназначен для может использоваться для создания безопасного рабочего приложения. Этот учебник предназначен для предоставления только контур о том, как реализовать сервер авторизации OAuth 2.0 с помощью по промежуточного слоя OWIN OAuth.
> 
> 
> ## <a name="software-versions"></a>Версии программного обеспечения
> 
> | **В этом руководстве показано** | **Также работает с** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express для Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012 с последним обновлением должна работать, но руководства не был протестирован с ним и некоторые элементы меню и диалоговые окна отличаются. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Если у вас есть вопросы, которые не имеют отношения к руководству, отправьте их в [Katana проекта на GitHub](https://github.com/aspnet/AspNetKatana/). Вопросы и комментарии о самом учебнике см. в разделе "Примечания" в нижней части страницы.


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) позволяет сторонним приложением для получения ограниченного доступа для службы HTTP. Вместо использования учетных данных владельца ресурса для доступа к защищенному ресурсу, клиент получает маркер доступа (это строка, обозначающая определенной области, время существования и другие атрибуты доступа). Маркеры доступа издает для сторонних клиентов сервера авторизации с утверждением владелец ресурса.

Этом руководстве рассматривается:

- Создание сервера авторизации для поддержки авторизации четырех типов предоставления и маркеры обновления:
    - Предоставление кода авторизации
    - Неявное разрешение
    - Пароль владельца ресурса предоставлением учетных данных
    - Предоставление учетных данных клиента
- Создание сервера ресурсов, который защищен с использованием маркера доступа.
- Создание клиентов OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Предварительные требования

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) или бесплатная версия [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), как указано в **версий программного обеспечения** в верхней части страницы.
- Знакомство с OWIN. См. в разделе [Приступая к работе с проектом Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) и [новые возможности в OWIN и Katana](index.md).
- Знакомство с [OAuth](http://tools.ietf.org/html/rfc6749) терминология, включая [ролей](http://tools.ietf.org/html/rfc6749#section-1.1), [потока протокола](http://tools.ietf.org/html/rfc6749#section-1.2), и [предоставления авторизации](http://tools.ietf.org/html/rfc6749#section-1.3). [Общие сведения об OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) хорошо представлена вступительная информация.

## <a name="create-an-authorization-server"></a>Создание сервера авторизации

В этом руководстве мы будет примерно начертить способы использования [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) и ASP.NET MVC для создания сервера авторизации. Мы надеемся вскоре предоставить для загрузки для готовый пример, этот учебник содержит каждый шаг. Во-первых, создайте пустой веб-приложение с именем *AuthorizationServer* и установите следующие пакеты:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (или упаковать любые другие учетные записи социальных сетей, например Microsoft.Owin.Security.Facebook)

Добавить [класс запуска OWIN](owin-startup-class-detection.md) в корневой папке проекта с именем *запуска*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Создание *приложения\_запустить* папки. Выберите *приложения\_запустить* папку и используйте клавиши Shift + Alt + A, чтобы добавить загруженной версии *AuthorizationServer\App\_Start\Startup.Auth.cs* файла.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Приведенный выше код обеспечивает вход в приложение и внешних файлов cookie и проверки подлинности Google, используемые сервером авторизации, сам требуется управление учетными записями.

`UseOAuthAuthorizationServer` — Это метод расширения для настройки сервера авторизации. Доступны следующие параметры программы установки.

- `AuthorizeEndpointPath`: Путь запроса, где клиентские приложения будут перенаправлять агент пользователя для получения пользователей согласие для выдачи маркера или кода. Должно начинаться с косой черты, например, "`/Authorize`«.
- `TokenEndpointPath`: Чтобы получить маркер доступа непосредственно взаимодействовать запрос путь клиентские приложения. Он должен начинаться с косой черты, например «/ Token». Если клиент выдает [клиента\_секрет](http://tools.ietf.org/html/rfc6749#appendix-A.2), но может быть передана в эту конечную точку.
- `ApplicationCanDisplayErrors`: Значение `true` Если веб-приложение хочет создать на пользовательскую страницу ошибки для ошибки проверки клиента о `/Authorize` конечной точки. Это требуется, только для случаев, когда браузер не направляется обратно в клиентское приложение, например, если `client_id` или `redirect_uri` неверны. `/Authorize` Следует ожидать конечной точки см. в разделе «oauth. Ошибка», «oauth. ErrorDescription» и «oauth. Свойства ErrorUri» добавляются в среду OWIN. 

    > [!NOTE]
    > В противном случае значение равно true, сервер авторизации возвращает страницу по умолчанию ошибка с сообщением об ошибке.
- `AllowInsecureHttp`: Значение True для разрешить запросам авторизации и маркеров поступления адресов HTTP URI и разрешите входящий трафик `redirect_uri` авторизовать параметров запросов в HTTP URI-адреса. 

    > [!WARNING]
    > Безопасность — это только в целях разработки.
- `Provider`: Объект, предоставляемый приложением для обработки событий, вызванных по промежуточного слоя сервера авторизации. Приложение может полностью реализовывать этот интерфейс, или он может создать экземпляр `OAuthAuthorizationServerProvider` и назначать делегаты, необходимые для потоков OAuth, данный сервер поддерживает.
- `AuthorizationCodeProvider`: Создает код авторизации однократного использования для возврата в клиентское приложение. Защитить приложения для сервера OAuth **необходимо** экземпляров для `AuthorizationCodeProvider` где маркер, созданный путем `OnCreate/OnCreateAsync` событие считается действительным только для одного вызова к `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Создает маркер обновления, который может использоваться для получения нового маркера доступа при необходимости. Если не указан сервер авторизации не вернет маркерам обновления из `/Token` конечной точки.

## <a name="account-management"></a>Управление учетными записями

OAuth безразлично, откуда и как вы управляете данные учетной записи пользователя. Он имеет [ASP.NET Identity](../../../identity/index.md) который отвечает за его. В этом руководстве мы упростить код учетной записи, управления и просто убедитесь, что этот пользователь может войти на использование по промежуточного слоя OWIN. Ниже приведен упрощенный пример кода для `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` используется для проверки клиента с его URL-адрес зарегистрированного перенаправления. `ValidateClientAuthentication` проверяет основные схемы заголовок и текст формы для получения учетных данных клиента.

Ниже приводится на страницу входа.

![](owin-oauth-20-authorization-server/_static/image1.png)

Просмотрите IETF OAuth 2 [Authorization Code Grant](http://tools.ietf.org/html/rfc6749#section-4.1) теперь разделе. 

**Поставщик** (в таблице ниже), [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Поставщик, который имеет тип `OAuthAuthorizationServerProvider`, который содержит все события сервера OAuth. 

| Этапы потока раздел Authorization Code Grant | Загрузка образца выполняет следующие действия с. |
| --- | --- |
|  |  |
| (A клиент запускает поток, направляя владелец ресурса агента пользователя в конечную точку авторизации. Клиент добавляет свой идентификатор клиента, запрошенную область, локальное состояние и URI перенаправления, к которому сервер авторизации будет направлять агент пользователя после доступа к предоставлен (или запрещен). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (Б), сервер авторизации выполняет проверку подлинности владельца ресурса (с помощью агента пользователя) и устанавливает ли владелец ресурса или отклоняет запрос на доступ клиента. | **&lt;Если пользователь предоставил доступ&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) при условии, что владелец ресурса предоставляет доступ, сервер авторизации перенаправляет агент пользователя обратно клиенту с помощью URI перенаправления, предоставленного ранее (в запросе или во время регистрации клиента). ... |  |
|  |  |
| (Г) клиент запрашивает маркер доступа из конечной точки маркера сервера авторизации путем включения кода авторизации, полученного на предыдущем шаге. При выполнении запроса клиент проходит проверку подлинности на сервере авторизации. Клиент включает URI перенаправления, используемый для получения кода авторизации для проверки подлинности. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Пример реализации `AuthorizationCodeProvider.CreateAsync` и `ReceiveAsync` для управления созданием и проверки кода авторизации, показано ниже.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Приведенный выше код использует параллельный словарь в памяти для сохранения билета код и удостоверений и восстановить удостоверение после получения кода. В реальном приложении он будет заменен, в постоянном хранилище данных. Конечная точка авторизации предназначен для владельца ресурса для предоставления доступа к клиенту. Как правило она должна пользовательский интерфейс, чтобы разрешить пользователю нажать кнопку и подтверждение разрешений. По промежуточного слоя OWIN OAuth позволяет коду приложения для обработки в конечную точку авторизации. В нашем примере, мы используем контроллера MVC вызывается `OAuthController` его обработать. Ниже приведен пример реализации.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize` Действие сначала проверяет, если пользователь выполнил вход на сервер авторизации. В противном случае по промежуточного слоя проверки подлинности запрашивает проверку подлинности, используя файл cookie «Приложение» вызывающего и перенаправляет на страницу входа. (См. выделенный код выше). Если пользователь выполнил вход, он будет отображать представление Authorize, как показано ниже:

![](owin-oauth-20-authorization-server/_static/image2.png)

Если **Grant** выбран переключатель, `Authorize` будет создан новый удостоверения «Bearer» и войдите с помощью его. Он активирует сервер авторизации, чтобы создать маркер носителя и отправить его обратно клиенту с полезными данными JSON. 

### <a name="implicit-grant"></a>Неявное разрешение

См. в IETF OAuth 2 [неявное предоставление](http://tools.ietf.org/html/rfc6749#section-4.2) теперь разделе.

 [Неявное предоставление](http://tools.ietf.org/html/rfc6749#section-4.2) потока показано на рисунке 4 приведена последовательность действий, и сопоставления, который OWIN OAuth следует по промежуточного слоя.  

| Этапы потока раздел неявное предоставление | Загрузка образца выполняет следующие действия с. |
| --- | --- |
|  |  |
| (A клиент запускает поток, направляя владелец ресурса агента пользователя в конечную точку авторизации. Клиент добавляет свой идентификатор клиента, запрошенную область, локальное состояние и URI перенаправления, к которому сервер авторизации будет направлять агент пользователя после доступа к предоставлен (или запрещен). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (Б), сервер авторизации выполняет проверку подлинности владельца ресурса (с помощью агента пользователя) и устанавливает ли владелец ресурса или отклоняет запрос на доступ клиента. | **&lt;Если пользователь предоставил доступ&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) при условии, что владелец ресурса предоставляет доступ, сервер авторизации перенаправляет агент пользователя обратно клиенту с помощью URI перенаправления, предоставленного ранее (в запросе или во время регистрации клиента). ... |  |
|  |  |
| (Г) клиент запрашивает маркер доступа из конечной точки маркера сервера авторизации путем включения кода авторизации, полученного на предыдущем шаге. При выполнении запроса клиент проходит проверку подлинности на сервере авторизации. Клиент включает URI перенаправления, используемый для получения кода авторизации для проверки подлинности. |  |

Так как мы уже реализован в конечную точку авторизации (`OAuthController.Authorize` действие) для предоставления кода авторизации, оно автоматически включает также неявного потока. Примечание: `Provider.ValidateClientRedirectUri` используется для проверки идентификатора клиента с его URL-адрес перенаправления, которая защищает неявный поток предоставления от маркера вредоносным клиентам, отправляющим доступ ([атаки Man-in--middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Пароль владельца ресурса предоставлением учетных данных

См. в IETF OAuth 2 [предоставление учетных данных владельца ресурса](http://tools.ietf.org/html/rfc6749#section-4.3) теперь разделе.

 [Предоставление учетных данных владельца ресурса](http://tools.ietf.org/html/rfc6749#section-4.3) приведена последовательность действий потока показано на рисунке 5, и сопоставления, который OWIN OAuth следует по промежуточного слоя.  

| Этапы потока раздел предоставление учетных данных владельца ресурса | Загрузка образца выполняет следующие действия с. |
| --- | --- |
|  |  |
| (A) владелец ресурса предоставляет клиенту имя пользователя и пароль. |  |
|  |  |
| (Б) клиент запрашивает маркер доступа из конечной точки маркера сервера авторизации путем включения учетных данных, полученных от владельца ресурса. При выполнении запроса клиент проходит проверку подлинности на сервере авторизации. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) сервер авторизации проверяет подлинность клиента и проверяет учетные данные владельца ресурса и если это допустимо, выдает маркер доступа. |  |

Ниже приведен пример реализации для `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> В приведенном выше коде объясняются в этом разделе руководства и не должны использоваться в безопасный или рабочих приложений. Он не проверяет учетные данные владельцев ресурса. Предполагается, что каждый учетных данных является допустимым и создает новое удостоверение для него. Новое удостоверение будет использоваться для создания токена доступа и токена обновления. Замените код в коде управления безопасная учетная запись.


### <a name="client-credentials-grant"></a>Предоставление учетных данных клиента

См. в IETF OAuth 2 [предоставления учетных данных клиента](http://tools.ietf.org/html/rfc6749#section-4.4) теперь разделе.

 [Предоставления учетных данных клиента](http://tools.ietf.org/html/rfc6749#section-4.4) приведена последовательность действий потока, показанный на рис. 6, и сопоставления, который OWIN OAuth следует по промежуточного слоя.  

| Этапы потока раздел предоставления учетных данных клиента | Загрузка образца выполняет следующие действия с. |
| --- | --- |
|  |  |
| (A) клиент проходит проверку подлинности на сервере авторизации и запрашивает маркер доступа из конечной точки маркера. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (Б) сервер авторизации проверяет подлинность клиента и если это допустимо, выдает маркер доступа. |  |

Ниже приведен пример реализации для `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> В приведенном выше коде объясняются в этом разделе руководства и не должны использоваться в безопасный или рабочих приложений. Замените код с вашим собственным кодом защищенный клиент управления.


### <a name="refresh-token"></a>Токен обновления

См. в IETF OAuth 2 [токен обновления](http://tools.ietf.org/html/rfc6749#section-1.5) теперь разделе.

 [Токен обновления](http://tools.ietf.org/html/rfc6749#section-1.5) потока показано на рисунке 2 приведена последовательность действий, и сопоставления, который OWIN OAuth следует по промежуточного слоя.  

| Этапы потока раздел предоставления учетных данных клиента | Загрузка образца выполняет следующие действия с. |
| --- | --- |
|  |  |
| (Ж) клиент запрашивает новый маркер доступа с проверкой подлинности на сервере авторизации и предоставляет токен обновления. Требования к проверке подлинности клиента основаны на типе клиента и для сервера политик авторизации. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (З) сервер авторизации проверяет подлинность клиента и проверяет маркер обновления и если это допустимо, выдает новый маркер доступа (и при необходимости новый маркер обновления). |  |

Ниже приведен пример реализации для `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Создать сервер ресурсов, который защищен с помощью токена доступа

Создайте проект пустой веб-приложения и установите следующие пакеты в проекте:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Создайте класс запуска и настройка проверки подлинности и веб-API. См. в разделе *AuthorizationServer\ResourceServer\Startup.cs* в состав примера.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

См. в разделе *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* в состав примера.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

См. в разделе *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* в состав примера.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` метод позволяет CORS для всех доменов.
- `UseOAuthBearerAuthentication` метод позволяет OAuth токена проверки подлинности по промежуточного слоя носителя, который будет получать и проверить токен носителя из заголовка авторизации в запросе.
- `Config.SuppressDefaultHostAuthenticaiton` по умолчанию подавляет размещаться прошедшему проверку подлинности участнику из приложения, поэтому все запросы будут анонимный после этого вызова.
- `HostAuthenticationFilter` включает проверку подлинности только для типа проверки подлинности. В этом случае это тип проверки подлинности носителя.

Чтобы продемонстрировать проверку подлинности, мы создадим ApiController для исходящих утверждений текущего пользователя.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Если сервер авторизации и сервер ресурсов не на том же компьютере, по промежуточного слоя OAuth будет использовать различные машинные ключи для шифрования и расшифровки маркера доступа носителя. Для совместного использования одного закрытого ключа между обоих проектов, мы добавим же `machinekey` в оба *web.config* файлов.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Создание клиентов OAuth 2.0

 Мы используем [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) пакет NuGet, чтобы упростить код клиента.

### <a name="authorization-code-grant-client"></a>Клиент предоставление кода авторизации

 Этот клиент — это приложение MVC. Будет запущен поток предоставления кода авторизации для получения маркера доступа из серверной части. Он состоит из одной страницы, как показано ниже:

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Authorize** кнопка будет перенаправлять браузер на сервере авторизации, чтобы уведомить владельца ресурсов для предоставления доступа для этого клиента.
- **Обновить** кнопка получит новый маркер доступа и маркер обновления, с помощью текущего обновления токена.
- **Защищенных ресурсов API-Интерфейс** кнопка будет вызывать сервер ресурсов для получения данных утверждений текущего пользователя и их отображения на странице.

Ниже приведен пример кода из `HomeController` клиента.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` требуется протокол SSL по умолчанию. Так как в примере используется протокол HTTP, необходимо добавить следующий параметр в файле конфигурации:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Безопасность — никогда не отключить SSL в рабочем приложении. Учетные данные входа теперь отправляются в незашифрованном по каналу связи. Приведенный выше код можно только для отладки в локальном образца и просмотра.


### <a name="implicit-grant-client"></a>Неявное предоставление клиента

Этот клиент использует JavaScript для:

1. Откройте новое окно и перенаправление к конечной точке авторизации сервера авторизации.
2. Получение маркера доступа из фрагментов URL-адрес при перенаправлении обратно.

Этот процесс показан на следующем рисунке:

![](owin-oauth-20-authorization-server/_static/image4.png)

Клиент должен иметь две страницы: один для домашней страницы, а другой — для обратного вызова. Ниже приведен пример кода JavaScript *Index.cshtml* файла:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Ниже приведен код в обработки обратного вызова *SignIn.cshtml* файла:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Рекомендуется переместить во внешний файл JavaScript, а не внедрить его с разметкой Razor. Для простоты примера они были объединены.


### <a name="resource-owner-password-credentials-grant-client"></a>Пароль владельца ресурса учетных данных клиента Grant

Мы демонстрирует работу консольного приложения на демонстрацию этого клиента. Ниже приведен код:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Предоставьте учетные данные. клиент

Как и предоставление учетных данных владельца ресурса, ниже приведен код приложения консоли.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
