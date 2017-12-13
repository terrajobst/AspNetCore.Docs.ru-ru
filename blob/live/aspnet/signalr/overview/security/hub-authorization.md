---
uid: signalr/overview/security/hub-authorization
title: "Проверка подлинности и авторизация для концентраторов SignalR | Документы Microsoft"
author: pfletcher
description: "В этом разделе описывается ограничьте какие пользователи или роли доступ к методам концентраторов. В этом разделе версий программного обеспечения используется Visual Studio 2013 .NET 4.5 SignalR хранить..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: f1538c933ff9e8e680d70ce1e63d24b189be47e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="91dee-104">Проверка подлинности и авторизация для концентраторов SignalR</span><span class="sxs-lookup"><span data-stu-id="91dee-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="91dee-105">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="91dee-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="91dee-106">В этом разделе описывается ограничьте какие пользователи или роли доступ к методам концентраторов.</span><span class="sxs-lookup"><span data-stu-id="91dee-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="91dee-107">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="91dee-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="91dee-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="91dee-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="91dee-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="91dee-109">.NET 4.5</span></span>
> - <span data-ttu-id="91dee-110">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="91dee-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="91dee-111">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="91dee-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="91dee-112">Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="91dee-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="91dee-113">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="91dee-113">Questions and comments</span></span>
> 
> <span data-ttu-id="91dee-114">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="91dee-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="91dee-115">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="91dee-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="91dee-116">Обзор</span><span class="sxs-lookup"><span data-stu-id="91dee-116">Overview</span></span>

<span data-ttu-id="91dee-117">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="91dee-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="91dee-118">Авторизовать атрибута</span><span class="sxs-lookup"><span data-stu-id="91dee-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="91dee-119">Требовать проверку подлинности для всех концентраторов</span><span class="sxs-lookup"><span data-stu-id="91dee-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="91dee-120">Настраиваемые авторизации</span><span class="sxs-lookup"><span data-stu-id="91dee-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="91dee-121">Передать сведения о проверке подлинности для клиентов</span><span class="sxs-lookup"><span data-stu-id="91dee-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="91dee-122">Параметры проверки подлинности для клиентов .NET</span><span class="sxs-lookup"><span data-stu-id="91dee-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="91dee-123">Файл cookie с помощью форм</span><span class="sxs-lookup"><span data-stu-id="91dee-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="91dee-124">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="91dee-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="91dee-125">Заголовка соединения</span><span class="sxs-lookup"><span data-stu-id="91dee-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="91dee-126">Сертификат</span><span class="sxs-lookup"><span data-stu-id="91dee-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="91dee-127">Авторизовать атрибута</span><span class="sxs-lookup"><span data-stu-id="91dee-127">Authorize attribute</span></span>

<span data-ttu-id="91dee-128">SignalR обеспечивает [авторизовать](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) атрибут, чтобы указать, какие пользователи или роли имеют доступ к концентратору или метод.</span><span class="sxs-lookup"><span data-stu-id="91dee-128">SignalR provides the [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="91dee-129">Этот атрибут находится в `Microsoft.AspNet.SignalR` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="91dee-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="91dee-130">Можно применить `Authorize` атрибут концентратор или отдельные методы в концентраторе.</span><span class="sxs-lookup"><span data-stu-id="91dee-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="91dee-131">При применении `Authorize` все методы в концентраторе применяется атрибут к классу hub требование проверки.</span><span class="sxs-lookup"><span data-stu-id="91dee-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="91dee-132">В этом разделе приведены примеры различных типов требования к проверке подлинности, которые можно применить.</span><span class="sxs-lookup"><span data-stu-id="91dee-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="91dee-133">Без `Authorize` атрибут привязан клиент может получить доступ к любому открытому методу на концентраторе.</span><span class="sxs-lookup"><span data-stu-id="91dee-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="91dee-134">Если вы определили роль с именем «Admin» в веб-приложении, можно указать только пользователям в этой роли можно получить доступ к концентратору следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="91dee-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="91dee-135">Также можно указать, что концентратор содержит один метод, который доступен всем пользователям, а второй метод, который доступен только пользователям, прошедшим проверку, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="91dee-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="91dee-136">Следующие примеры сценариев разных авторизации:</span><span class="sxs-lookup"><span data-stu-id="91dee-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="91dee-137">`[Authorize]`— только прошедшие проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="91dee-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="91dee-138">`[Authorize(Roles = "Admin,Manager")]`— только прошедшие проверку подлинности пользователей в указанные роли</span><span class="sxs-lookup"><span data-stu-id="91dee-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="91dee-139">`[Authorize(Users = "user1,user2")]`— только прошедшие проверку подлинности пользователи, имеющие указанные имена пользователей</span><span class="sxs-lookup"><span data-stu-id="91dee-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="91dee-140">`[Authorize(RequireOutgoing=false)]`— только прошедшие проверку подлинности пользователи могут вызывать концентратора, но вызовы с сервера обратно клиентам не ограничены по авторизации, например, когда только некоторые пользователи могут отправлять сообщения, но все остальные может получать сообщения.</span><span class="sxs-lookup"><span data-stu-id="91dee-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="91dee-141">Свойство RequireOutgoing свойства может применяться только для всего hub, а не для отдельных пользователей методов в центре.</span><span class="sxs-lookup"><span data-stu-id="91dee-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="91dee-142">Когда свойство RequireOutgoing не задано значение false, только пользователи, которые соответствуют требованиям к авторизации называются с сервера.</span><span class="sxs-lookup"><span data-stu-id="91dee-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="91dee-143">Требовать проверку подлинности для всех концентраторов</span><span class="sxs-lookup"><span data-stu-id="91dee-143">Require authentication for all hubs</span></span>

<span data-ttu-id="91dee-144">Можно потребовать проверку подлинности для всех концентраторам и методам концентраторов в приложении, вызвав [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) метод при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="91dee-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="91dee-145">Этот метод можно использовать после нескольких концентраторов и принудительно требование проверки подлинности для всех из них.</span><span class="sxs-lookup"><span data-stu-id="91dee-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="91dee-146">С помощью этого метода нельзя задать требования для ролей, пользователей или исходящих авторизации.</span><span class="sxs-lookup"><span data-stu-id="91dee-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="91dee-147">Можно только указать, что доступ к методам концентратор является только пользователи, прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="91dee-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="91dee-148">Тем не менее можно применять атрибут авторизовать концентраторы или методам, чтобы указать дополнительные требования.</span><span class="sxs-lookup"><span data-stu-id="91dee-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="91dee-149">Любое требование, указываемых в атрибут добавляется к основным требованием проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="91dee-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="91dee-150">Пример файла запуска, ограничивающее все методы концентратора пользователям, прошедшим проверку.</span><span class="sxs-lookup"><span data-stu-id="91dee-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="91dee-151">При вызове метода `RequireAuthentication()` вызовет метод после обработки запрос SignalR, SignalR `InvalidOperationException` исключение.</span><span class="sxs-lookup"><span data-stu-id="91dee-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="91dee-152">SignalR создает исключение, так как не удается добавить модуль HubPipeline после конвейера была вызвана.</span><span class="sxs-lookup"><span data-stu-id="91dee-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="91dee-153">В предыдущем примере показан вызов `RequireAuthentication` метод в `Configuration` метод, который выполняется один раз перед обработкой первого запроса.</span><span class="sxs-lookup"><span data-stu-id="91dee-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="91dee-154">Настраиваемые авторизации</span><span class="sxs-lookup"><span data-stu-id="91dee-154">Customized authorization</span></span>

<span data-ttu-id="91dee-155">Если нужно настроить как авторизация, можно создать класс, производный от `AuthorizeAttribute` и Переопределите [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) метод.</span><span class="sxs-lookup"><span data-stu-id="91dee-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="91dee-156">Для каждого запроса SignalR вызывает этот метод, чтобы определить, имеет ли пользователь разрешение для выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="91dee-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="91dee-157">В переопределенном методе предоставляют необходимую логику для вашего сценария авторизации.</span><span class="sxs-lookup"><span data-stu-id="91dee-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="91dee-158">Приведенный ниже показано, как осуществить авторизации с помощью удостоверений на основе утверждений.</span><span class="sxs-lookup"><span data-stu-id="91dee-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="91dee-159">Передать сведения о проверке подлинности для клиентов</span><span class="sxs-lookup"><span data-stu-id="91dee-159">Pass authentication information to clients</span></span>

<span data-ttu-id="91dee-160">Необходимо использовать сведения о проверке подлинности в коде, который выполняется на клиенте.</span><span class="sxs-lookup"><span data-stu-id="91dee-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="91dee-161">При вызове методов на стороне клиента, передайте необходимые сведения.</span><span class="sxs-lookup"><span data-stu-id="91dee-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="91dee-162">Например метод приложения разговора может передать как параметр имя пользователя как пользователь, отправляющий сообщение, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="91dee-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="91dee-163">Или можно создать объект для представления сведения для проверки подлинности и передать этот объект в качестве параметра, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="91dee-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="91dee-164">Никогда не следует передавать идентификатор соединения клиента один для других клиентов, т.к. пользователь-злоумышленник может использовать для имитации запроса от клиента.</span><span class="sxs-lookup"><span data-stu-id="91dee-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="91dee-165">Параметры проверки подлинности для клиентов .NET</span><span class="sxs-lookup"><span data-stu-id="91dee-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="91dee-166">При наличии клиент .NET, таких как консольное приложение, который взаимодействует с к концентратору, который будет ограничена для пользователей с проверкой подлинности, можно передать учетные данные проверки подлинности в файле cookie, заголовка соединения или сертификата.</span><span class="sxs-lookup"><span data-stu-id="91dee-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="91dee-167">В примерах этого раздела показано, как эти различные методы для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="91dee-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="91dee-168">Они не являются полностью функциональной приложений SignalR.</span><span class="sxs-lookup"><span data-stu-id="91dee-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="91dee-169">Дополнительные сведения о клиентах .NET с помощью SignalR см. в разделе [Справочник по API концентраторов - клиент .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="91dee-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="91dee-170">Куки-файл</span><span class="sxs-lookup"><span data-stu-id="91dee-170">Cookie</span></span>

<span data-ttu-id="91dee-171">Когда клиент .NET взаимодействует с к концентратору, который использует проверку подлинности форм ASP.NET, необходимо вручную задать файл cookie проверки подлинности соединения.</span><span class="sxs-lookup"><span data-stu-id="91dee-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="91dee-172">Добавьте файл cookie, `CookieContainer` свойство [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) объекта.</span><span class="sxs-lookup"><span data-stu-id="91dee-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="91dee-173">В следующем примере показано консольное приложение, которое извлекает файл cookie проверки подлинности с веб-страницы и добавляет соединения этому файлу cookie.</span><span class="sxs-lookup"><span data-stu-id="91dee-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="91dee-174">Консольное приложение отправляет учетные данные для **www.contoso.com/RemoteLogin** которого может ссылаться на пустую страницу, содержащую следующий файл кода.</span><span class="sxs-lookup"><span data-stu-id="91dee-174">The console app posts the credentials to **www.contoso.com/RemoteLogin** which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="91dee-175">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="91dee-175">Windows authentication</span></span>

<span data-ttu-id="91dee-176">При использовании проверки подлинности Windows, можно передать учетные данные текущего пользователя с помощью [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) свойство.</span><span class="sxs-lookup"><span data-stu-id="91dee-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="91dee-177">Задать учетные данные для подключения к значению DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="91dee-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="91dee-178">Заголовка соединения</span><span class="sxs-lookup"><span data-stu-id="91dee-178">Connection header</span></span>

<span data-ttu-id="91dee-179">Если приложение не использует файлы cookie, можно передать сведения о пользователе заголовка соединения.</span><span class="sxs-lookup"><span data-stu-id="91dee-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="91dee-180">Например можно передать токен в заголовке соединения.</span><span class="sxs-lookup"><span data-stu-id="91dee-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="91dee-181">Затем в концентраторе, нужно проверить токен пользователя.</span><span class="sxs-lookup"><span data-stu-id="91dee-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="91dee-182">Сертификат</span><span class="sxs-lookup"><span data-stu-id="91dee-182">Certificate</span></span>

<span data-ttu-id="91dee-183">Можно передать сертификат клиента, чтобы убедиться, что пользователь.</span><span class="sxs-lookup"><span data-stu-id="91dee-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="91dee-184">Добавить сертификат при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="91dee-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="91dee-185">Следующий пример демонстрирует только добавление сертификат клиента для подключения; он не отображается полное консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="91dee-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="91dee-186">Она использует [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) класс, который предоставляет несколько способов создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="91dee-186">It uses the [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
