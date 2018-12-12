---
uid: signalr/overview/security/hub-authorization
title: Проверка подлинности и авторизация для концентраторов SignalR | Документация Майкрософт
author: pfletcher
description: В этом разделе описывается, как ограничить, какие пользователи или роли можно получить доступ к методам концентратора. В этом разделе версий программного обеспечения используется продуктом Visual Studio 2013 .NET 4.5 SignalR...
ms.author: riande
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d957d31e0c5453d0d4a48958fa4b25d61407f207
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287629"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="8ae47-104">Проверка подлинности и авторизация для концентраторов SignalR</span><span class="sxs-lookup"><span data-stu-id="8ae47-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="8ae47-105">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8ae47-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="8ae47-106">В этом разделе описывается, как ограничить, какие пользователи или роли можно получить доступ к методам концентратора.</span><span class="sxs-lookup"><span data-stu-id="8ae47-106">This topic describes how to restrict which users or roles can access hub methods.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="8ae47-107">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="8ae47-107">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="8ae47-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8ae47-108">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="8ae47-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8ae47-109">.NET 4.5</span></span>
> - <span data-ttu-id="8ae47-110">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="8ae47-110">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="8ae47-111">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="8ae47-111">Previous versions of this topic</span></span>
>
> <span data-ttu-id="8ae47-112">Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="8ae47-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="8ae47-113">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="8ae47-113">Questions and comments</span></span>
>
> <span data-ttu-id="8ae47-114">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="8ae47-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8ae47-115">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="8ae47-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="8ae47-116">Обзор</span><span class="sxs-lookup"><span data-stu-id="8ae47-116">Overview</span></span>

<span data-ttu-id="8ae47-117">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="8ae47-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="8ae47-118">Авторизовать атрибут</span><span class="sxs-lookup"><span data-stu-id="8ae47-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="8ae47-119">Требовать проверку подлинности для всех концентраторов</span><span class="sxs-lookup"><span data-stu-id="8ae47-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="8ae47-120">Настраиваемой авторизации</span><span class="sxs-lookup"><span data-stu-id="8ae47-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="8ae47-121">Передать сведения о проверке подлинности для клиентов</span><span class="sxs-lookup"><span data-stu-id="8ae47-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="8ae47-122">Параметры проверки подлинности для клиентов .NET</span><span class="sxs-lookup"><span data-stu-id="8ae47-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="8ae47-123">Файл cookie с помощью форм</span><span class="sxs-lookup"><span data-stu-id="8ae47-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="8ae47-124">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="8ae47-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="8ae47-125">Заголовка соединения</span><span class="sxs-lookup"><span data-stu-id="8ae47-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="8ae47-126">Сертификат</span><span class="sxs-lookup"><span data-stu-id="8ae47-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="8ae47-127">Авторизовать атрибут</span><span class="sxs-lookup"><span data-stu-id="8ae47-127">Authorize attribute</span></span>

<span data-ttu-id="8ae47-128">SignalR обеспечивает [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) атрибут, чтобы указать, какие пользователи или роли имеют доступ к концентратору или метод.</span><span class="sxs-lookup"><span data-stu-id="8ae47-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="8ae47-129">Этот атрибут находится в `Microsoft.AspNet.SignalR` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="8ae47-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="8ae47-130">Можно применить `Authorize` к концентратору или отдельные методы в концентраторе атрибут.</span><span class="sxs-lookup"><span data-stu-id="8ae47-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="8ae47-131">При применении `Authorize` все методы в концентраторе применяется атрибут к классу концентратора, к указанной авторизации.</span><span class="sxs-lookup"><span data-stu-id="8ae47-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="8ae47-132">В этом разделе приведены примеры различных типов требований к авторизации, которые можно применить.</span><span class="sxs-lookup"><span data-stu-id="8ae47-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="8ae47-133">Без `Authorize` атрибут, подключенный клиент может получить доступ к любому открытому методу на концентраторе.</span><span class="sxs-lookup"><span data-stu-id="8ae47-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="8ae47-134">Если вы определили роль с именем «Admin» в веб-приложении, можно указать только пользователи в этой роли можно получить доступ к концентратору следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="8ae47-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="8ae47-135">Или можно указать, что концентратор содержит один метод, который доступен всем пользователям, а второй метод, который доступен только пользователям, прошедшим проверку, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="8ae47-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="8ae47-136">Следующие примеры сценариев авторизации:</span><span class="sxs-lookup"><span data-stu-id="8ae47-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="8ae47-137">`[Authorize]` — только прошедшие проверку пользователи</span><span class="sxs-lookup"><span data-stu-id="8ae47-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="8ae47-138">`[Authorize(Roles = "Admin,Manager")]` — только прошедшим проверку подлинности пользователей в указанных ролей</span><span class="sxs-lookup"><span data-stu-id="8ae47-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="8ae47-139">`[Authorize(Users = "user1,user2")]` — только прошедшим проверку подлинности пользователей с помощью указанных имен пользователя</span><span class="sxs-lookup"><span data-stu-id="8ae47-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="8ae47-140">`[Authorize(RequireOutgoing=false)]` — только прошедшие проверку подлинности пользователи могут вызывать концентратора, но и вызовы с сервера клиентам не ограничены по авторизации, например, при только определенные пользователи могут отправлять сообщения, но все остальные могут принимать сообщения.</span><span class="sxs-lookup"><span data-stu-id="8ae47-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="8ae47-141">Свойство RequireOutgoing может применяться только к весь центр, не для отдельных пользователей методов в концентраторе.</span><span class="sxs-lookup"><span data-stu-id="8ae47-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="8ae47-142">Когда RequireOutgoing не присвоено значение false, только пользователи, соответствующие требованиям к авторизации называются с сервера.</span><span class="sxs-lookup"><span data-stu-id="8ae47-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="8ae47-143">Требовать проверку подлинности для всех концентраторов</span><span class="sxs-lookup"><span data-stu-id="8ae47-143">Require authentication for all hubs</span></span>

<span data-ttu-id="8ae47-144">Можно потребовать проверку подлинности для всех концентраторам и методам концентраторов в приложении, вызвав [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) метод при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="8ae47-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="8ae47-145">Этот метод можно использовать после нескольких концентраторов и хотите применить требования проверки подлинности для всех из них.</span><span class="sxs-lookup"><span data-stu-id="8ae47-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="8ae47-146">С помощью этого метода невозможно указать требования к авторизации исходящих, пользователя или роли.</span><span class="sxs-lookup"><span data-stu-id="8ae47-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="8ae47-147">Можно только указать, что доступ к методам концентратора является аутентифицированным пользователям.</span><span class="sxs-lookup"><span data-stu-id="8ae47-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="8ae47-148">Тем не менее по-прежнему можно применить атрибут Authorize концентраторов или методы, чтобы указать дополнительные требования.</span><span class="sxs-lookup"><span data-stu-id="8ae47-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="8ae47-149">Любое требование, указываемых в атрибут добавляется к основным требованием проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="8ae47-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="8ae47-150">В следующем примере файл запуска, который ограничивает все методы концентратора пользователям, прошедшим проверку.</span><span class="sxs-lookup"><span data-stu-id="8ae47-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="8ae47-151">При вызове метода `RequireAuthentication()` вызовет метод после обработки запрос SignalR, SignalR `InvalidOperationException` исключение.</span><span class="sxs-lookup"><span data-stu-id="8ae47-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="8ae47-152">SignalR создает это исключение, так как не удается добавить модуль в конвейер концентратора после вызова конвейера.</span><span class="sxs-lookup"><span data-stu-id="8ae47-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="8ae47-153">В предыдущем примере показан вызов `RequireAuthentication` метод в `Configuration` метод, который выполняется один раз до обработки первого запроса.</span><span class="sxs-lookup"><span data-stu-id="8ae47-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="8ae47-154">Настраиваемой авторизации</span><span class="sxs-lookup"><span data-stu-id="8ae47-154">Customized authorization</span></span>

<span data-ttu-id="8ae47-155">Если вам нужно настроить, как авторизация, можно создать класс, производный от `AuthorizeAttribute` и переопределить [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) метод.</span><span class="sxs-lookup"><span data-stu-id="8ae47-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="8ae47-156">Для каждого запроса SignalR вызывает этот метод, чтобы определить, авторизован ли пользователь для выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="8ae47-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="8ae47-157">В переопределенном методе предоставляют необходимую логику для вашего сценария авторизации.</span><span class="sxs-lookup"><span data-stu-id="8ae47-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="8ae47-158">В следующем примере показано, как для принудительного применения авторизации с помощью удостоверений на основе утверждений.</span><span class="sxs-lookup"><span data-stu-id="8ae47-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="8ae47-159">Передать сведения о проверке подлинности для клиентов</span><span class="sxs-lookup"><span data-stu-id="8ae47-159">Pass authentication information to clients</span></span>

<span data-ttu-id="8ae47-160">Может потребоваться использовать сведения о проверке подлинности в код, который выполняется на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="8ae47-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="8ae47-161">Необходимые сведения, указываемые при вызове методов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="8ae47-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="8ae47-162">Например метод приложения чата можно передать как параметр имя пользователя как пользователь, отправляющий сообщение, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="8ae47-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="8ae47-163">Или можно создать объект для представления сведения для проверки подлинности и передать этот объект в качестве параметра, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="8ae47-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="8ae47-164">Никогда не следует передавать один клиентский идентификатор подключения для других клиентов, как пользователь-злоумышленник может использовать для имитации запроса от клиента.</span><span class="sxs-lookup"><span data-stu-id="8ae47-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="8ae47-165">Параметры проверки подлинности для клиентов .NET</span><span class="sxs-lookup"><span data-stu-id="8ae47-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="8ae47-166">При наличии клиент .NET, например, консольного приложения, которое взаимодействует с концентратором, ограниченную проверку подлинности пользователей, можно передать учетные данные проверки подлинности в файле cookie, заголовке соединение или сертификата.</span><span class="sxs-lookup"><span data-stu-id="8ae47-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="8ae47-167">Примеры в этом разделе показано, как использовать эти различные методы для проверки подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="8ae47-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="8ae47-168">Они не являются полнофункциональные приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="8ae47-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="8ae47-169">Дополнительные сведения о клиентах .NET с помощью SignalR см. в разделе [руководство по API концентраторов — клиент .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="8ae47-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="8ae47-170">Куки-файл</span><span class="sxs-lookup"><span data-stu-id="8ae47-170">Cookie</span></span>

<span data-ttu-id="8ae47-171">Когда клиент .NET взаимодействует с к концентратору, который использует проверку подлинности форм ASP.NET, необходимо вручную задать файл cookie проверки подлинности для соединения.</span><span class="sxs-lookup"><span data-stu-id="8ae47-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="8ae47-172">Добавьте файл cookie для `CookieContainer` свойство [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) объекта.</span><span class="sxs-lookup"><span data-stu-id="8ae47-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="8ae47-173">Следующий пример показывает консольное приложение, которое получает файл cookie проверки подлинности с веб-страниц и добавляет этот файл cookie подключения.</span><span class="sxs-lookup"><span data-stu-id="8ae47-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="8ae47-174">Консольное приложение отправляет учетные данные для <strong>www.contoso.com/RemoteLogin</strong> которого может ссылаться на пустую страницу, которая содержит следующие файлы кода.</span><span class="sxs-lookup"><span data-stu-id="8ae47-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="8ae47-175">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="8ae47-175">Windows authentication</span></span>

<span data-ttu-id="8ae47-176">При использовании проверки подлинности Windows, можно передать учетные данные текущего пользователя с помощью [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) свойство.</span><span class="sxs-lookup"><span data-stu-id="8ae47-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="8ae47-177">Присвоено значение DefaultCredentials учетные данные для подключения.</span><span class="sxs-lookup"><span data-stu-id="8ae47-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="8ae47-178">Заголовка соединения</span><span class="sxs-lookup"><span data-stu-id="8ae47-178">Connection header</span></span>

<span data-ttu-id="8ae47-179">Если приложение не использует файлы cookie, можно передать сведения о пользователе в заголовке соединение.</span><span class="sxs-lookup"><span data-stu-id="8ae47-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="8ae47-180">Например можно передать маркер в заголовке соединение.</span><span class="sxs-lookup"><span data-stu-id="8ae47-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="8ae47-181">Затем в концентраторе, то следует проверить маркер пользователя.</span><span class="sxs-lookup"><span data-stu-id="8ae47-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="8ae47-182">Сертификат</span><span class="sxs-lookup"><span data-stu-id="8ae47-182">Certificate</span></span>

<span data-ttu-id="8ae47-183">Вы можете передать сертификат клиента для проверки пользователя.</span><span class="sxs-lookup"><span data-stu-id="8ae47-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="8ae47-184">Добавьте сертификат при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="8ae47-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="8ae47-185">Следующий пример показывает только то, как добавить сертификат клиента для подключения; он не отображается полное консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="8ae47-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="8ae47-186">Она использует [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) класс, который предоставляет несколько способов создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="8ae47-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
