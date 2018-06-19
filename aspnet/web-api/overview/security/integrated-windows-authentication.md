---
uid: web-api/overview/security/integrated-windows-authentication
title: Встроенная проверка подлинности Windows | Документы Microsoft
author: MikeWasson
description: Описывает использование встроенной проверки подлинности Windows в ASP.NET Web API.
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
ms.locfileid: "26508163"
---
<a name="integrated-windows-authentication"></a>Встроенная проверка подлинности Windows
====================
по [Mike Wasson](https://github.com/MikeWasson)

Встроенная проверка подлинности Windows пользователи могут ввести свои учетные данные Windows, с помощью Kerberos или NTLM. Клиент отправляет учетные данные в заголовок авторизации. Проверка подлинности Windows наилучшим образом подходит для среды интрасети. Дополнительные сведения см. в разделе [проверки подлинности Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Преимущества | Недостатки |
| --- | --- |
| -Встроены в IIS. -Не отправляет учетные данные пользователя в запросе. — Если клиентский компьютер принадлежит домену (например, приложение интрасети), пользователю необходимо ввести учетные данные. | -Не рекомендуется для веб-приложений. -Требует поддержки Kerberos или NTLM в клиенте. -Клиент должен находиться в домене Active Directory. |

> [!NOTE]
> Если приложение размещено в Azure и локального домена Active Directory, рассмотрите возможность федерацию в локальной AD с Azure Active Directory. Таким образом, пользователи могут входить с использованием их учетных данных локальной, но будет выполняться проверка подлинности Azure AD. Дополнительные сведения см. в разделе [проверки подлинности Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Чтобы создать приложение, использующее встроенную проверку подлинности Windows, выберите шаблон «Приложение интрасети» в мастере проектов MVC 4. Этот шаблон проекта используется следующий параметр в файле Web.config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

На стороне клиента, встроенная проверка подлинности Windows работает с любого браузера, поддерживающего [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) схему проверки подлинности, которая содержит большинство основных браузеров. Для клиентских приложений .NET **HttpClient** класс поддерживает проверку подлинности Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Проверка подлинности Windows является уязвимым для атак с подделкой (CSRF) запросов между сайтами. В разделе [атак подделки межсайтовых запросов](preventing-cross-site-request-forgery-csrf-attacks.md).
