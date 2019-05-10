---
title: Внешние поставщики проверки подлинности OAuth
author: rick-anderson
description: Обнаружение поставщиков проверки подлинности внешних OAuth, которые работают с приложениями ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/otherlogins
ms.openlocfilehash: b69c366ec1bf12ccf434991fc8a79eaf8c09da3d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897511"
---
# <a name="external-oauth-authentication-providers"></a>Внешние поставщики проверки подлинности OAuth

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Пранавом Растоги](https://github.com/rustd), и [Валерий Новицкий](https://github.com/01binary)

В следующем списке приведены распространенные внешних поставщиков проверки подлинности OAuth, которые работают с приложениями ASP.NET Core. Пакеты NuGet независимых производителей, например тех, которые обслуживается [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), можно использовать в дополнение к поставщики проверки подлинности, реализованные командой ASP.NET Core.

* [LinkedIn](https://www.linkedin.com/developer/apps) ([инструкции](https://developer.linkedin.com/docs/oauth2))

* [Instagram](https://www.instagram.com/developer/register/) ([инструкции](https://www.instagram.com/developer/authentication/))

* [Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) ([инструкции](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))

* [Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([инструкции](https://developer.github.com/v3/oauth/))

* [Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([инструкции](https://developer.yahoo.com/bbauth/user.html))

* [Tumblr](https://www.tumblr.com/oauth/apps) ([инструкции](https://www.tumblr.com/docs/api/v2#auth))

* [Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([инструкции](https://developers.pinterest.com/docs/api/overview/?))

* [Pocket](https://getpocket.com/developer/apps/new) ([инструкции](https://getpocket.com/developer/docs/authentication))

* [Flickr](https://www.flickr.com/services/apps/create) ([инструкции](https://www.flickr.com/services/api/auth.oauth.html))

* [Dribble](https://dribbble.com/signup) ([инструкции](http://developer.dribbble.com/v1/oauth/))

* [Vimeo](https://vimeo.com/join) ([инструкции](https://developer.vimeo.com/api/authentication))

* [SoundCloud](https://soundcloud.com/you/apps/new) ([инструкции](https://developers.soundcloud.com/blog/we-love-oauth-2))

* [VK](https://vk.com/apps?act=manage) ([инструкции](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))

[!INCLUDE[Multiple authentication providers](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
