---
title: "Краткий опрос других поставщиков проверки подлинности."
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 11/3/2016
ms.topic: article
ms.assetid: BC36CA84-3DE8-496E-9AA2-2F1B74AE8309
ms.prod: asp.net-core
uid: security/authentication/otherlogins
ms.openlocfilehash: 421db8c89e01cebba0c1f98cc9286288a75777f2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="short-survey-of-other-authentication-providers"></a><span data-ttu-id="f9072-102">Краткий опрос других поставщиков проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="f9072-102">Short survey of other authentication providers</span></span>

<a name=security-authentication-other-logins></a>

<span data-ttu-id="f9072-103">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), и [Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="f9072-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="f9072-104">Здесь настраиваются инструкции для некоторых распространенных OAuth поставщиков.</span><span class="sxs-lookup"><span data-stu-id="f9072-104">Here are set up instructions for some other common OAuth providers.</span></span> <span data-ttu-id="f9072-105">Пакеты NuGet сторонних разработчиков, например устройств, поддерживаемых [aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) может использоваться в дополнение к поставщики проверки подлинности, реализованные командой ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9072-105">Third-party NuGet packages such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) can be used to complement authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="f9072-106">Настройка **LinkedIn** входа: [https://developer.linkedin.com/my-apps](https://developer.linkedin.com/my-apps).</span><span class="sxs-lookup"><span data-stu-id="f9072-106">Set up **LinkedIn** sign in: [https://developer.linkedin.com/my-apps](https://developer.linkedin.com/my-apps).</span></span> <span data-ttu-id="f9072-107">В разделе [официальный действия](https://developer.linkedin.com/docs/oauth2).</span><span class="sxs-lookup"><span data-stu-id="f9072-107">See [official steps](https://developer.linkedin.com/docs/oauth2).</span></span>

* <span data-ttu-id="f9072-108">Настройка **Instagram** входа: [https://www.instagram.com/developer/clients/manage](https://www.instagram.com/developer/clients/manage).</span><span class="sxs-lookup"><span data-stu-id="f9072-108">Set up **Instagram** sign in: [https://www.instagram.com/developer/clients/manage](https://www.instagram.com/developer/clients/manage).</span></span> <span data-ttu-id="f9072-109">В разделе [официальный действия](https://www.instagram.com/developer/authentication/).</span><span class="sxs-lookup"><span data-stu-id="f9072-109">See [official steps](https://www.instagram.com/developer/authentication/).</span></span>

* <span data-ttu-id="f9072-110">Настройка **Reddit** входа: [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps).</span><span class="sxs-lookup"><span data-stu-id="f9072-110">Set up **Reddit** sign in: [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps).</span></span> <span data-ttu-id="f9072-111">В разделе [официальный действия](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).</span><span class="sxs-lookup"><span data-stu-id="f9072-111">See [official steps](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).</span></span>

* <span data-ttu-id="f9072-112">Настройка **Github** входа: [https://github.com/settings/applications/new](https://github.com/settings/applications/new).</span><span class="sxs-lookup"><span data-stu-id="f9072-112">Set up **Github** sign in: [https://github.com/settings/applications/new](https://github.com/settings/applications/new).</span></span> <span data-ttu-id="f9072-113">В разделе [официальный действия](https://developer.github.com/v3/oauth/).</span><span class="sxs-lookup"><span data-stu-id="f9072-113">See [official steps](https://developer.github.com/v3/oauth/).</span></span>

* <span data-ttu-id="f9072-114">Настройка **Yahoo** входа: [https://developer.yahoo.com/apps/create/](https://developer.yahoo.com/apps/create/).</span><span class="sxs-lookup"><span data-stu-id="f9072-114">Set up **Yahoo** sign in: [https://developer.yahoo.com/apps/create/](https://developer.yahoo.com/apps/create/).</span></span> <span data-ttu-id="f9072-115">В разделе [официальный действия](https://developer.yahoo.com/bbauth/user.html).</span><span class="sxs-lookup"><span data-stu-id="f9072-115">See [official steps](https://developer.yahoo.com/bbauth/user.html).</span></span>

* <span data-ttu-id="f9072-116">Настройка **Tumblr** входа: [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps).</span><span class="sxs-lookup"><span data-stu-id="f9072-116">Set up **Tumblr** sign in: [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps).</span></span> <span data-ttu-id="f9072-117">В разделе [официальный действия](https://www.tumblr.com/docs/en/api/v2#auth).</span><span class="sxs-lookup"><span data-stu-id="f9072-117">See [official steps](https://www.tumblr.com/docs/en/api/v2#auth).</span></span>

* <span data-ttu-id="f9072-118">Настройка **Pinterest** входа: [https://developers.pinterest.com/apps](https://developers.pinterest.com/apps).</span><span class="sxs-lookup"><span data-stu-id="f9072-118">Set up **Pinterest** sign in: [https://developers.pinterest.com/apps](https://developers.pinterest.com/apps).</span></span> <span data-ttu-id="f9072-119">В разделе [официальный действия](https://developers.pinterest.com/docs/api/overview/?).</span><span class="sxs-lookup"><span data-stu-id="f9072-119">See [official steps](https://developers.pinterest.com/docs/api/overview/?).</span></span>

* <span data-ttu-id="f9072-120">Настройка **Pocket** входа: [http://getpocket.com/developer/apps/new](http://getpocket.com/developer/apps/new).</span><span class="sxs-lookup"><span data-stu-id="f9072-120">Set up **Pocket** sign in: [http://getpocket.com/developer/apps/new](http://getpocket.com/developer/apps/new).</span></span> <span data-ttu-id="f9072-121">В разделе [официальный действия](https://getpocket.com/developer/docs/authentication).</span><span class="sxs-lookup"><span data-stu-id="f9072-121">See [official steps](https://getpocket.com/developer/docs/authentication).</span></span>

* <span data-ttu-id="f9072-122">Настройка **Flickr** входа: [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create).</span><span class="sxs-lookup"><span data-stu-id="f9072-122">Set up **Flickr** sign in: [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create).</span></span> <span data-ttu-id="f9072-123">В разделе [официальный действия](https://www.flickr.com/services/api/auth.oauth.html).</span><span class="sxs-lookup"><span data-stu-id="f9072-123">See [official steps](https://www.flickr.com/services/api/auth.oauth.html).</span></span>

* <span data-ttu-id="f9072-124">Настройка **Dribble** входа: [https://dribbble.com/signup](https://dribbble.com/signup).</span><span class="sxs-lookup"><span data-stu-id="f9072-124">Set up **Dribble** sign in: [https://dribbble.com/signup](https://dribbble.com/signup).</span></span> <span data-ttu-id="f9072-125">В разделе [официальный действия](http://developer.dribbble.com/v1/oauth/).</span><span class="sxs-lookup"><span data-stu-id="f9072-125">See [official steps](http://developer.dribbble.com/v1/oauth/).</span></span>

* <span data-ttu-id="f9072-126">Настройка **Vimeo** входа: [https://developer.vimeo.com/apps](https://developer.vimeo.com/apps).</span><span class="sxs-lookup"><span data-stu-id="f9072-126">Set up **Vimeo** sign in: [https://developer.vimeo.com/apps](https://developer.vimeo.com/apps).</span></span> <span data-ttu-id="f9072-127">В разделе [официальный действия](https://developer.vimeo.com/api/authentication).</span><span class="sxs-lookup"><span data-stu-id="f9072-127">See [official steps](https://developer.vimeo.com/api/authentication).</span></span>

* <span data-ttu-id="f9072-128">Настройка **SoundCloud** входа: [http://soundcloud.com/you/apps/new](http://soundcloud.com/you/apps/new).</span><span class="sxs-lookup"><span data-stu-id="f9072-128">Set up **SoundCloud** sign in: [http://soundcloud.com/you/apps/new](http://soundcloud.com/you/apps/new).</span></span> <span data-ttu-id="f9072-129">В разделе [официальный действия](https://developers.soundcloud.com/blog/we-love-oauth-2).</span><span class="sxs-lookup"><span data-stu-id="f9072-129">See [official steps](https://developers.soundcloud.com/blog/we-love-oauth-2).</span></span>

* <span data-ttu-id="f9072-130">Настройка **VK** входа: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage).</span><span class="sxs-lookup"><span data-stu-id="f9072-130">Set up **VK** sign in: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage).</span></span> <span data-ttu-id="f9072-131">В разделе [официальный действия](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).</span><span class="sxs-lookup"><span data-stu-id="f9072-131">See [official steps](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).</span></span>
