---
uid: web-api/overview/security/integrated-windows-authentication
title: "Встроенная проверка подлинности Windows | Документы Microsoft"
author: MikeWasson
description: "Описывает использование встроенной проверки подлинности Windows в ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="dbcb5-103">Встроенная проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="dbcb5-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="dbcb5-104">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="dbcb5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="dbcb5-105">Встроенная проверка подлинности Windows пользователи могут ввести свои учетные данные Windows, с помощью Kerberos или NTLM.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="dbcb5-106">Клиент отправляет учетные данные в заголовок авторизации.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="dbcb5-107">Проверка подлинности Windows наилучшим образом подходит для среды интрасети.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="dbcb5-108">Дополнительные сведения см. в разделе [проверки подлинности Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="dbcb5-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="dbcb5-109">Преимущества</span><span class="sxs-lookup"><span data-stu-id="dbcb5-109">Advantages</span></span> | <span data-ttu-id="dbcb5-110">Недостатки</span><span class="sxs-lookup"><span data-stu-id="dbcb5-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="dbcb5-111">-Встроены в IIS.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-111">- Built into IIS.</span></span> <span data-ttu-id="dbcb5-112">-Не отправляет учетные данные пользователя в запросе.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="dbcb5-113">— Если клиентский компьютер принадлежит домену (например, приложение интрасети), пользователю необходимо ввести учетные данные.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="dbcb5-114">-Не рекомендуется для веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="dbcb5-115">-Требует поддержки Kerberos или NTLM в клиенте.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="dbcb5-116">-Клиент должен находиться в домене Active Directory.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="dbcb5-117">Если приложение размещено в Azure и локального домена Active Directory, рассмотрите возможность федерацию в локальной AD с Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="dbcb5-118">Таким образом, пользователи могут входить с использованием их учетных данных локальной, но будет выполняться проверка подлинности Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="dbcb5-119">Дополнительные сведения см. в разделе [проверки подлинности Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="dbcb5-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="dbcb5-120">Чтобы создать приложение, использующее встроенную проверку подлинности Windows, выберите шаблон «Приложение интрасети» в мастере проектов MVC 4.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="dbcb5-121">Этот шаблон проекта используется следующий параметр в файле Web.config:</span><span class="sxs-lookup"><span data-stu-id="dbcb5-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="dbcb5-122">На стороне клиента, встроенная проверка подлинности Windows работает с любого браузера, поддерживающего [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) схему проверки подлинности, которая содержит большинство основных браузеров.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="dbcb5-123">Для клиентских приложений .NET **HttpClient** класс поддерживает проверку подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="dbcb5-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="dbcb5-124">Проверка подлинности Windows является уязвимым для атак с подделкой (CSRF) запросов между сайтами.</span><span class="sxs-lookup"><span data-stu-id="dbcb5-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="dbcb5-125">В разделе [атак подделки межсайтовых запросов](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="dbcb5-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
