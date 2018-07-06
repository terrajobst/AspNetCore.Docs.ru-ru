---
uid: signalr/overview/older-versions/hub-authorization
title: Проверка подлинности и авторизация для концентраторов SignalR (SignalR 1.x) | Документация Майкрософт
author: pfletcher
description: В этом разделе описывается, как ограничить, какие пользователи или роли можно получить доступ к методам концентратора.
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 1bb10a49a0d783300c145c30ad09e31f8e6055d6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808283"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="aa08e-103">Проверка подлинности и авторизация для концентраторов SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="aa08e-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="aa08e-104">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aa08e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="aa08e-105">В этом разделе описывается, как ограничить, какие пользователи или роли можно получить доступ к методам концентратора.</span><span class="sxs-lookup"><span data-stu-id="aa08e-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="aa08e-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="aa08e-106">Overview</span></span>

<span data-ttu-id="aa08e-107">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="aa08e-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="aa08e-108">Авторизовать атрибут</span><span class="sxs-lookup"><span data-stu-id="aa08e-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="aa08e-109">Требовать проверку подлинности для всех концентраторов</span><span class="sxs-lookup"><span data-stu-id="aa08e-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="aa08e-110">Настраиваемой авторизации</span><span class="sxs-lookup"><span data-stu-id="aa08e-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="aa08e-111">Передать сведения о проверке подлинности для клиентов</span><span class="sxs-lookup"><span data-stu-id="aa08e-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="aa08e-112">Параметры проверки подлинности для клиентов .NET</span><span class="sxs-lookup"><span data-stu-id="aa08e-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="aa08e-113">Файл cookie с помощью форм</span><span class="sxs-lookup"><span data-stu-id="aa08e-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="aa08e-114">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="aa08e-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="aa08e-115">Заголовка соединения</span><span class="sxs-lookup"><span data-stu-id="aa08e-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="aa08e-116">Сертификат</span><span class="sxs-lookup"><span data-stu-id="aa08e-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="aa08e-117">Авторизовать атрибут</span><span class="sxs-lookup"><span data-stu-id="aa08e-117">Authorize attribute</span></span>

<span data-ttu-id="aa08e-118">SignalR обеспечивает [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) атрибут, чтобы указать, какие пользователи или роли имеют доступ к концентратору или метод.</span><span class="sxs-lookup"><span data-stu-id="aa08e-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="aa08e-119">Этот атрибут находится в `Microsoft.AspNet.SignalR` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="aa08e-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="aa08e-120">Можно применить `Authorize` к концентратору или отдельные методы в концентраторе атрибут.</span><span class="sxs-lookup"><span data-stu-id="aa08e-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="aa08e-121">При применении `Authorize` все методы в концентраторе применяется атрибут к классу концентратора, к указанной авторизации.</span><span class="sxs-lookup"><span data-stu-id="aa08e-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="aa08e-122">Ниже приведены требования к проверке подлинности, которые можно применить различные типы.</span><span class="sxs-lookup"><span data-stu-id="aa08e-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="aa08e-123">Без `Authorize` атрибута, все открытые методы концентратора доступны для клиентов, которые подключены к центру.</span><span class="sxs-lookup"><span data-stu-id="aa08e-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="aa08e-124">Если вы определили роль с именем «Admin» в веб-приложении, можно указать только пользователи в этой роли можно получить доступ к концентратору следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="aa08e-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="aa08e-125">Или можно указать, что концентратор содержит один метод, который доступен всем пользователям, а второй метод, который доступен только пользователям, прошедшим проверку, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="aa08e-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="aa08e-126">Следующие примеры сценариев авторизации:</span><span class="sxs-lookup"><span data-stu-id="aa08e-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="aa08e-127">`[Authorize]` — только прошедшие проверку пользователи</span><span class="sxs-lookup"><span data-stu-id="aa08e-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="aa08e-128">`[Authorize(Roles = "Admin,Manager")]` — только прошедшим проверку подлинности пользователей в указанных ролей</span><span class="sxs-lookup"><span data-stu-id="aa08e-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="aa08e-129">`[Authorize(Users = "user1,user2")]` — только прошедшим проверку подлинности пользователей с помощью указанных имен пользователя</span><span class="sxs-lookup"><span data-stu-id="aa08e-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="aa08e-130">`[Authorize(RequireOutgoing=false)]` — только прошедшие проверку подлинности пользователи могут вызывать концентратора, но и вызовы с сервера клиентам не ограничены по авторизации, например, при только определенные пользователи могут отправлять сообщения, но все остальные могут принимать сообщения.</span><span class="sxs-lookup"><span data-stu-id="aa08e-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="aa08e-131">Свойство RequireOutgoing может применяться только к весь центр, не для отдельных пользователей методов в концентраторе.</span><span class="sxs-lookup"><span data-stu-id="aa08e-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="aa08e-132">Когда RequireOutgoing не присвоено значение false, только пользователи, соответствующие требованиям к авторизации называются с сервера.</span><span class="sxs-lookup"><span data-stu-id="aa08e-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="aa08e-133">Требовать проверку подлинности для всех концентраторов</span><span class="sxs-lookup"><span data-stu-id="aa08e-133">Require authentication for all hubs</span></span>

<span data-ttu-id="aa08e-134">Можно потребовать проверку подлинности для всех концентраторам и методам концентраторов в приложении, вызвав [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) метод при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="aa08e-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="aa08e-135">Этот метод можно использовать после нескольких концентраторов и хотите применить требования проверки подлинности для всех из них.</span><span class="sxs-lookup"><span data-stu-id="aa08e-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="aa08e-136">С помощью этого метода невозможно указать исходящих авторизации, пользователя или роли.</span><span class="sxs-lookup"><span data-stu-id="aa08e-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="aa08e-137">Можно только указать, что доступ к методам концентратора является аутентифицированным пользователям.</span><span class="sxs-lookup"><span data-stu-id="aa08e-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="aa08e-138">Тем не менее по-прежнему можно применить атрибут Authorize концентраторов или методы, чтобы указать дополнительные требования.</span><span class="sxs-lookup"><span data-stu-id="aa08e-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="aa08e-139">Любое требование, указываемые в атрибутах применяется в дополнение к основным требованием проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="aa08e-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="aa08e-140">В следующем примере файл Global.asax, ограничивающее все методы концентратора пользователям, прошедшим проверку.</span><span class="sxs-lookup"><span data-stu-id="aa08e-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="aa08e-141">При вызове метода `RequireAuthentication()` вызовет метод после обработки запрос SignalR, SignalR `InvalidOperationException` исключение.</span><span class="sxs-lookup"><span data-stu-id="aa08e-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="aa08e-142">Это исключение возникает, так как не удается добавить модуль в конвейер концентратора после вызова конвейера.</span><span class="sxs-lookup"><span data-stu-id="aa08e-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="aa08e-143">В предыдущем примере показан вызов `RequireAuthentication` метод в `Application_Start` метод, который выполняется один раз до обработки первого запроса.</span><span class="sxs-lookup"><span data-stu-id="aa08e-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="aa08e-144">Настраиваемой авторизации</span><span class="sxs-lookup"><span data-stu-id="aa08e-144">Customized authorization</span></span>

<span data-ttu-id="aa08e-145">Если вам нужно настроить, как авторизация, можно создать класс, производный от `AuthorizeAttribute` и переопределить [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) метод.</span><span class="sxs-lookup"><span data-stu-id="aa08e-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="aa08e-146">Этот метод вызывается для каждого запроса определить, авторизован ли пользователь для выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="aa08e-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="aa08e-147">В переопределенном методе предоставляют необходимую логику для вашего сценария авторизации.</span><span class="sxs-lookup"><span data-stu-id="aa08e-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="aa08e-148">В следующем примере показано, как для принудительного применения авторизации с помощью удостоверений на основе утверждений.</span><span class="sxs-lookup"><span data-stu-id="aa08e-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="aa08e-149">Передать сведения о проверке подлинности для клиентов</span><span class="sxs-lookup"><span data-stu-id="aa08e-149">Pass authentication information to clients</span></span>

<span data-ttu-id="aa08e-150">Может потребоваться использовать сведения о проверке подлинности в код, который выполняется на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="aa08e-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="aa08e-151">Необходимые сведения, указываемые при вызове методов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="aa08e-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="aa08e-152">Например метод приложения чата можно передать как параметр имя пользователя как пользователь, отправляющий сообщение, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="aa08e-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="aa08e-153">Или можно создать объект для представления сведения для проверки подлинности и передать этот объект в качестве параметра, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="aa08e-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="aa08e-154">Никогда не следует передавать один клиентский идентификатор подключения для других клиентов, как пользователь-злоумышленник может использовать для имитации запроса от клиента.</span><span class="sxs-lookup"><span data-stu-id="aa08e-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="aa08e-155">Параметры проверки подлинности для клиентов .NET</span><span class="sxs-lookup"><span data-stu-id="aa08e-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="aa08e-156">При наличии клиент .NET, например, консольного приложения, которое взаимодействует с концентратором, ограниченную проверку подлинности пользователей, можно передать учетные данные проверки подлинности в файле cookie, заголовке соединение или сертификата.</span><span class="sxs-lookup"><span data-stu-id="aa08e-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="aa08e-157">Примеры в этом разделе показано, как использовать эти различные методы для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="aa08e-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="aa08e-158">Они не являются полнофункциональные приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="aa08e-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="aa08e-159">Дополнительные сведения о клиентах .NET с помощью SignalR см. в разделе [руководство по API концентраторов — клиент .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="aa08e-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="aa08e-160">Куки-файл</span><span class="sxs-lookup"><span data-stu-id="aa08e-160">Cookie</span></span>

<span data-ttu-id="aa08e-161">Когда клиент .NET взаимодействует с к концентратору, который использует проверку подлинности форм ASP.NET, необходимо вручную задать файл cookie проверки подлинности для соединения.</span><span class="sxs-lookup"><span data-stu-id="aa08e-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="aa08e-162">Добавьте файл cookie для `CookieContainer` свойство [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) объекта.</span><span class="sxs-lookup"><span data-stu-id="aa08e-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="aa08e-163">Следующий пример показывает консольное приложение, которое получает файл cookie проверки подлинности с веб-страниц и добавляет этот файл cookie подключения.</span><span class="sxs-lookup"><span data-stu-id="aa08e-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="aa08e-164">URL-адрес `https://www.contoso.com/RemoteLogin` в примере указывает на веб-страницу, необходимо создать.</span><span class="sxs-lookup"><span data-stu-id="aa08e-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="aa08e-165">Страница будет извлечь имя учтенного пользователя и пароль и попытка входа пользователя с учетными данными.</span><span class="sxs-lookup"><span data-stu-id="aa08e-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="aa08e-166">Консольное приложение отправляет учетные данные для www.contoso.com/RemoteLogin который может ссылаться на пустую страницу, которая содержит следующие файлы кода.</span><span class="sxs-lookup"><span data-stu-id="aa08e-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="aa08e-167">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="aa08e-167">Windows authentication</span></span>

<span data-ttu-id="aa08e-168">При использовании проверки подлинности Windows, можно передать учетные данные текущего пользователя с помощью [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) свойство.</span><span class="sxs-lookup"><span data-stu-id="aa08e-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="aa08e-169">Присвоено значение DefaultCredentials учетные данные для подключения.</span><span class="sxs-lookup"><span data-stu-id="aa08e-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="aa08e-170">Заголовка соединения</span><span class="sxs-lookup"><span data-stu-id="aa08e-170">Connection header</span></span>

<span data-ttu-id="aa08e-171">Если приложение не использует файлы cookie, можно передать сведения о пользователе в заголовке соединение.</span><span class="sxs-lookup"><span data-stu-id="aa08e-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="aa08e-172">Например можно передать маркер в заголовке соединение.</span><span class="sxs-lookup"><span data-stu-id="aa08e-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="aa08e-173">Затем в концентраторе, то следует проверить маркер пользователя.</span><span class="sxs-lookup"><span data-stu-id="aa08e-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="aa08e-174">Сертификат</span><span class="sxs-lookup"><span data-stu-id="aa08e-174">Certificate</span></span>

<span data-ttu-id="aa08e-175">Вы можете передать сертификат клиента для проверки пользователя.</span><span class="sxs-lookup"><span data-stu-id="aa08e-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="aa08e-176">Добавьте сертификат при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="aa08e-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="aa08e-177">Следующий пример показывает только то, как добавить сертификат клиента для подключения; он не отображается полное консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="aa08e-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="aa08e-178">Она использует [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) класс, который предоставляет несколько способов создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="aa08e-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
