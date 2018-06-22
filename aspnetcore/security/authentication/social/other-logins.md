---
title: Краткий опрос других поставщиков проверки подлинности
author: rick-anderson
ms.author: riande
ms.date: 11/03/2016
uid: security/authentication/otherlogins
ms.openlocfilehash: 9c2ce02f4613fddbe0e767724019d80ac056bf7b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274057"
---
# <a name="short-survey-of-other-authentication-providers"></a>Краткий опрос других поставщиков проверки подлинности

<a name="security-authentication-other-logins"></a>

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), и [Valeriy Novytskyy](https://github.com/01binary)

Здесь настраиваются инструкции для некоторых распространенных OAuth поставщиков. Пакеты NuGet сторонних разработчиков, например устройств, поддерживаемых [aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) может использоваться в дополнение к поставщики проверки подлинности, реализованные командой ASP.NET Core.

* Настройка **LinkedIn** входа: [ https://www.linkedin.com/developer/apps ](https://www.linkedin.com/developer/apps). В разделе [официальный действия](https://developer.linkedin.com/docs/oauth2).

* Настройка **Instagram** входа: [ https://www.instagram.com/developer/register/ ](https://www.instagram.com/developer/register/). В разделе [официальный действия](https://www.instagram.com/developer/authentication/).

* Настройка **Reddit** входа: [ https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps ](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps). В разделе [официальный действия](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).

* Настройка **Github** входа: [ https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew ](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew). В разделе [официальный действия](https://developer.github.com/v3/oauth/).

* Настройка **Yahoo** входа: [ https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F ](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F). В разделе [официальный действия](https://developer.yahoo.com/bbauth/user.html).

* Настройка **Tumblr** входа: [ https://www.tumblr.com/oauth/apps ](https://www.tumblr.com/oauth/apps). В разделе [официальный действия](https://www.tumblr.com/docs/api/v2#auth).

* Настройка **Pinterest** входа: [ https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F ](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F). В разделе [официальный действия](https://developers.pinterest.com/docs/api/overview/?).

* Настройка **Pocket** входа: [ https://getpocket.com/developer/apps/new ](https://getpocket.com/developer/apps/new). В разделе [официальный действия](https://getpocket.com/developer/docs/authentication).

* Настройка **Flickr** входа: [ https://www.flickr.com/services/apps/create ](https://www.flickr.com/services/apps/create). В разделе [официальный действия](https://www.flickr.com/services/api/auth.oauth.html).

* Настройка **Dribble** входа: [ https://dribbble.com/signup ](https://dribbble.com/signup). В разделе [официальный действия](http://developer.dribbble.com/v1/oauth/).

* Настройка **Vimeo** входа: [ https://vimeo.com/join ](https://vimeo.com/join). В разделе [официальный действия](https://developer.vimeo.com/api/authentication).

* Настройка **SoundCloud** входа: [ https://soundcloud.com/you/apps/new ](https://soundcloud.com/you/apps/new). В разделе [официальный действия](https://developers.soundcloud.com/blog/we-love-oauth-2).

* Настройка **VK** входа: [ https://vk.com/apps?act=manage ](https://vk.com/apps?act=manage). В разделе [официальный действия](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).

## <a name="multiple-authentication-providers"></a>Несколько поставщиков проверки подлинности

[!INCLUDE[](~/includes/chain-auth-providers.md)]
