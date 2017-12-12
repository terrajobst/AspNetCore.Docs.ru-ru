---
uid: signalr/overview/older-versions/hub-authorization
title: "Проверка подлинности и авторизация для концентраторов SignalR (SignalR 1.x) | Документы Microsoft"
author: pfletcher
description: "В этом разделе описывается ограничьте какие пользователи или роли доступ к методам концентраторов."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: e52e39bf9c66419e18bf78036138d1f15376f2be
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="f6b09-103">Проверка подлинности и авторизация для концентраторов SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="f6b09-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="f6b09-104">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f6b09-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f6b09-105">В этом разделе описывается ограничьте какие пользователи или роли доступ к методам концентраторов.</span><span class="sxs-lookup"><span data-stu-id="f6b09-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="f6b09-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="f6b09-106">Overview</span></span>

<span data-ttu-id="f6b09-107">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="f6b09-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f6b09-108">Авторизовать атрибута</span><span class="sxs-lookup"><span data-stu-id="f6b09-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="f6b09-109">Требовать проверку подлинности для всех концентраторов</span><span class="sxs-lookup"><span data-stu-id="f6b09-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="f6b09-110">Настраиваемые авторизации</span><span class="sxs-lookup"><span data-stu-id="f6b09-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="f6b09-111">Передать сведения о проверке подлинности для клиентов</span><span class="sxs-lookup"><span data-stu-id="f6b09-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="f6b09-112">Параметры проверки подлинности для клиентов .NET</span><span class="sxs-lookup"><span data-stu-id="f6b09-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="f6b09-113">Файл cookie с помощью форм</span><span class="sxs-lookup"><span data-stu-id="f6b09-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="f6b09-114">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="f6b09-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="f6b09-115">Заголовка соединения</span><span class="sxs-lookup"><span data-stu-id="f6b09-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="f6b09-116">Сертификат</span><span class="sxs-lookup"><span data-stu-id="f6b09-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="f6b09-117">Авторизовать атрибута</span><span class="sxs-lookup"><span data-stu-id="f6b09-117">Authorize attribute</span></span>

<span data-ttu-id="f6b09-118">SignalR обеспечивает [авторизовать](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) атрибут, чтобы указать, какие пользователи или роли имеют доступ к концентратору или метод.</span><span class="sxs-lookup"><span data-stu-id="f6b09-118">SignalR provides the [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="f6b09-119">Этот атрибут находится в `Microsoft.AspNet.SignalR` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="f6b09-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="f6b09-120">Можно применить `Authorize` атрибут концентратор или отдельные методы в концентраторе.</span><span class="sxs-lookup"><span data-stu-id="f6b09-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="f6b09-121">При применении `Authorize` все методы в концентраторе применяется атрибут к классу hub требование проверки.</span><span class="sxs-lookup"><span data-stu-id="f6b09-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="f6b09-122">Ниже приведены требования к проверке подлинности, которые можно применить различные типы.</span><span class="sxs-lookup"><span data-stu-id="f6b09-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="f6b09-123">Без `Authorize` атрибута, все открытые методы концентратора доступны клиенту, который подключен к концентратору.</span><span class="sxs-lookup"><span data-stu-id="f6b09-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="f6b09-124">Если вы определили роль с именем «Admin» в веб-приложении, можно указать только пользователям в этой роли можно получить доступ к концентратору следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="f6b09-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="f6b09-125">Также можно указать, что концентратор содержит один метод, который доступен всем пользователям, а второй метод, который доступен только пользователям, прошедшим проверку, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f6b09-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="f6b09-126">Следующие примеры сценариев разных авторизации:</span><span class="sxs-lookup"><span data-stu-id="f6b09-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="f6b09-127">`[Authorize]`— только прошедшие проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="f6b09-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="f6b09-128">`[Authorize(Roles = "Admin,Manager")]`— только прошедшие проверку подлинности пользователей в указанные роли</span><span class="sxs-lookup"><span data-stu-id="f6b09-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="f6b09-129">`[Authorize(Users = "user1,user2")]`— только прошедшие проверку подлинности пользователи, имеющие указанные имена пользователей</span><span class="sxs-lookup"><span data-stu-id="f6b09-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="f6b09-130">`[Authorize(RequireOutgoing=false)]`— только прошедшие проверку подлинности пользователи могут вызывать концентратора, но вызовы с сервера обратно клиентам не ограничены по авторизации, например, когда только некоторые пользователи могут отправлять сообщения, но все остальные может получать сообщения.</span><span class="sxs-lookup"><span data-stu-id="f6b09-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="f6b09-131">Свойство RequireOutgoing свойства может применяться только для всего hub, а не для отдельных пользователей методов в центре.</span><span class="sxs-lookup"><span data-stu-id="f6b09-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="f6b09-132">Когда свойство RequireOutgoing не задано значение false, только пользователи, которые соответствуют требованиям к авторизации называются с сервера.</span><span class="sxs-lookup"><span data-stu-id="f6b09-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="f6b09-133">Требовать проверку подлинности для всех концентраторов</span><span class="sxs-lookup"><span data-stu-id="f6b09-133">Require authentication for all hubs</span></span>

<span data-ttu-id="f6b09-134">Можно потребовать проверку подлинности для всех концентраторам и методам концентраторов в приложении, вызвав [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) метод при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="f6b09-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="f6b09-135">Этот метод можно использовать после нескольких концентраторов и принудительно требование проверки подлинности для всех из них.</span><span class="sxs-lookup"><span data-stu-id="f6b09-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="f6b09-136">С помощью этого метода нельзя указать роль, пользователя или исходящих авторизации.</span><span class="sxs-lookup"><span data-stu-id="f6b09-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="f6b09-137">Можно только указать, что доступ к методам концентратор является только пользователи, прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="f6b09-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="f6b09-138">Тем не менее можно применять атрибут авторизовать концентраторы или методам, чтобы указать дополнительные требования.</span><span class="sxs-lookup"><span data-stu-id="f6b09-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="f6b09-139">Любое требование, указываемые в атрибутах применяется в дополнение к основным требованием проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f6b09-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="f6b09-140">Пример файла Global.asax, ограничивающее все методы концентратора пользователям, прошедшим проверку.</span><span class="sxs-lookup"><span data-stu-id="f6b09-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="f6b09-141">При вызове метода `RequireAuthentication()` вызовет метод после обработки запрос SignalR, SignalR `InvalidOperationException` исключение.</span><span class="sxs-lookup"><span data-stu-id="f6b09-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="f6b09-142">Это исключение возникает, так как не удается добавить модуль HubPipeline после конвейера была вызвана.</span><span class="sxs-lookup"><span data-stu-id="f6b09-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="f6b09-143">В предыдущем примере показан вызов `RequireAuthentication` метод в `Application_Start` метод, который выполняется один раз перед обработкой первого запроса.</span><span class="sxs-lookup"><span data-stu-id="f6b09-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="f6b09-144">Настраиваемые авторизации</span><span class="sxs-lookup"><span data-stu-id="f6b09-144">Customized authorization</span></span>

<span data-ttu-id="f6b09-145">Если нужно настроить как авторизация, можно создать класс, производный от `AuthorizeAttribute` и Переопределите [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) метод.</span><span class="sxs-lookup"><span data-stu-id="f6b09-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="f6b09-146">Этот метод вызывается для каждого запроса определить, имеет ли пользователь разрешение для выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="f6b09-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="f6b09-147">В переопределенном методе предоставляют необходимую логику для вашего сценария авторизации.</span><span class="sxs-lookup"><span data-stu-id="f6b09-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="f6b09-148">Приведенный ниже показано, как осуществить авторизации с помощью удостоверений на основе утверждений.</span><span class="sxs-lookup"><span data-stu-id="f6b09-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="f6b09-149">Передать сведения о проверке подлинности для клиентов</span><span class="sxs-lookup"><span data-stu-id="f6b09-149">Pass authentication information to clients</span></span>

<span data-ttu-id="f6b09-150">Необходимо использовать сведения о проверке подлинности в коде, который выполняется на клиенте.</span><span class="sxs-lookup"><span data-stu-id="f6b09-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="f6b09-151">При вызове методов на стороне клиента, передайте необходимые сведения.</span><span class="sxs-lookup"><span data-stu-id="f6b09-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="f6b09-152">Например метод приложения разговора может передать как параметр имя пользователя как пользователь, отправляющий сообщение, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f6b09-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="f6b09-153">Или можно создать объект для представления сведения для проверки подлинности и передать этот объект в качестве параметра, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f6b09-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="f6b09-154">Никогда не следует передавать идентификатор соединения клиента один для других клиентов, т.к. пользователь-злоумышленник может использовать для имитации запроса от клиента.</span><span class="sxs-lookup"><span data-stu-id="f6b09-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="f6b09-155">Параметры проверки подлинности для клиентов .NET</span><span class="sxs-lookup"><span data-stu-id="f6b09-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="f6b09-156">При наличии клиент .NET, таких как консольное приложение, который взаимодействует с к концентратору, который будет ограничена для пользователей с проверкой подлинности, можно передать учетные данные проверки подлинности в файле cookie, заголовка соединения или сертификата.</span><span class="sxs-lookup"><span data-stu-id="f6b09-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="f6b09-157">В примерах этого раздела показано, как эти различные методы для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="f6b09-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="f6b09-158">Они не являются полностью функциональной приложений SignalR.</span><span class="sxs-lookup"><span data-stu-id="f6b09-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="f6b09-159">Дополнительные сведения о клиентах .NET с помощью SignalR см. в разделе [Справочник по API концентраторов - клиент .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="f6b09-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="f6b09-160">Куки-файл</span><span class="sxs-lookup"><span data-stu-id="f6b09-160">Cookie</span></span>

<span data-ttu-id="f6b09-161">Когда клиент .NET взаимодействует с к концентратору, который использует проверку подлинности форм ASP.NET, необходимо вручную задать файл cookie проверки подлинности соединения.</span><span class="sxs-lookup"><span data-stu-id="f6b09-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="f6b09-162">Добавьте файл cookie, `CookieContainer` свойство [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) объекта.</span><span class="sxs-lookup"><span data-stu-id="f6b09-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="f6b09-163">В следующем примере показано консольное приложение, которое извлекает файл cookie проверки подлинности с веб-страницы и добавляет соединения этому файлу cookie.</span><span class="sxs-lookup"><span data-stu-id="f6b09-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="f6b09-164">URL-адрес `https://www.contoso.com/RemoteLogin` в примере указывает на веб-страницу, необходимо создать.</span><span class="sxs-lookup"><span data-stu-id="f6b09-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="f6b09-165">Страница будет получить отправленные пользователем имя и пароль и попробуйте выполнить вход с учетными данными пользователя.</span><span class="sxs-lookup"><span data-stu-id="f6b09-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="f6b09-166">Консольное приложение отправляет учетные данные, который может ссылаться на пустую страницу, содержащую следующий файл кода www.contoso.com/RemoteLogin.</span><span class="sxs-lookup"><span data-stu-id="f6b09-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="f6b09-167">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="f6b09-167">Windows authentication</span></span>

<span data-ttu-id="f6b09-168">При использовании проверки подлинности Windows, можно передать учетные данные текущего пользователя с помощью [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) свойство.</span><span class="sxs-lookup"><span data-stu-id="f6b09-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="f6b09-169">Задать учетные данные для подключения к значению DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="f6b09-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="f6b09-170">Заголовка соединения</span><span class="sxs-lookup"><span data-stu-id="f6b09-170">Connection header</span></span>

<span data-ttu-id="f6b09-171">Если приложение не использует файлы cookie, можно передать сведения о пользователе заголовка соединения.</span><span class="sxs-lookup"><span data-stu-id="f6b09-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="f6b09-172">Например можно передать токен в заголовке соединения.</span><span class="sxs-lookup"><span data-stu-id="f6b09-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="f6b09-173">Затем в концентраторе, нужно проверить токен пользователя.</span><span class="sxs-lookup"><span data-stu-id="f6b09-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="f6b09-174">Сертификат</span><span class="sxs-lookup"><span data-stu-id="f6b09-174">Certificate</span></span>

<span data-ttu-id="f6b09-175">Можно передать сертификат клиента, чтобы убедиться, что пользователь.</span><span class="sxs-lookup"><span data-stu-id="f6b09-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="f6b09-176">Добавить сертификат при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="f6b09-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="f6b09-177">Следующий пример демонстрирует только добавление сертификат клиента для подключения; он не отображается полное консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="f6b09-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="f6b09-178">Она использует [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) класс, который предоставляет несколько способов создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="f6b09-178">It uses the [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
