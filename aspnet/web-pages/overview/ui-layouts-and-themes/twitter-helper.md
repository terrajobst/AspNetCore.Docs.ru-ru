---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Вспомогательный модуль с помощью ASP.NET Web Pages Twitter | Документация Майкрософт
author: Rick-Anderson
description: Этот раздел и приложения показано, как добавить вспомогательный модуль Twitter для WebMatrix 3 проекта. Он содержит код вспомогательный модуль Twitter и показан способ вызова вспомогательного приложения...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299434"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="a52f8-104">Вспомогательное приложение Twitter с помощью веб-страниц ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a52f8-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="a52f8-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a52f8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a52f8-106">Вспомогательные функции Twitter являются устаревшими.</span><span class="sxs-lookup"><span data-stu-id="a52f8-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="a52f8-107">Twitter новейшие средства взаимодействия для веб-сайтов, см. в разделе [Twitter для веб-сайтов Обзор](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="a52f8-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="a52f8-108">Этот раздел и приложения показано, как добавить вспомогательный модуль Twitter для WebMatrix 3 проекта.</span><span class="sxs-lookup"><span data-stu-id="a52f8-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="a52f8-109">Он содержит код вспомогательный модуль Twitter и демонстрируется вызов вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="a52f8-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="a52f8-110">Этот код для файла Twitter.cshtml была разработана **Tian Pan** корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="a52f8-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a52f8-111">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="a52f8-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a52f8-112">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a52f8-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a52f8-113">Этот учебник также работает с ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="a52f8-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="a52f8-114">Вступление</span><span class="sxs-lookup"><span data-stu-id="a52f8-114">Introduction</span></span>

<span data-ttu-id="a52f8-115">В этом разделе показано, как добавить вспомогательный модуль Twitter для приложения и используйте синтаксис Razor для вызова вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="a52f8-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="a52f8-116">Вспомогательный модуль Twitter упрощает процесс включения кнопки Twitter и мини-приложения в приложении.</span><span class="sxs-lookup"><span data-stu-id="a52f8-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="a52f8-117">Для использования мини-приложения Twitter, например временную шкалу пользователя или результатов поиска по хэштегу, необходимо сначала создать [мини-приложение в Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="a52f8-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="a52f8-118">После создания мини-приложение, вы получите идентификатор мини-приложения. При вызове вспомогательные методы, которые показывают мини-приложения можно передать в качестве параметра этот идентификатор мини-приложения.</span><span class="sxs-lookup"><span data-stu-id="a52f8-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="a52f8-119">Эта статья написана для Twitter API версии 1.1.</span><span class="sxs-lookup"><span data-stu-id="a52f8-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="a52f8-120">Путем непосредственного добавления Twitter вспомогательного кода в проект, можно обновить вспомогательный код, при изменении Twitter API.</span><span class="sxs-lookup"><span data-stu-id="a52f8-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="a52f8-121">Сведения об установке WebMatrix, см. в разделе [введение в ASP.NET Web Pages 2 - Приступая к работе](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="a52f8-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="a52f8-122">Добавьте вспомогательный модуль Twitter в проект</span><span class="sxs-lookup"><span data-stu-id="a52f8-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="a52f8-123">Чтобы добавить вспомогательный модуль Twitter, во-первых, добавьте папку с именем **приложения\_кода** в проект.</span><span class="sxs-lookup"><span data-stu-id="a52f8-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="a52f8-124">Затем создайте файл с именем **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="a52f8-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Папка App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="a52f8-126">Замените код по умолчанию в Twitter.cshtml следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="a52f8-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="a52f8-127">Вызывайте методы Twitter из веб-страниц</span><span class="sxs-lookup"><span data-stu-id="a52f8-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="a52f8-128">Приведенный ниже показано, как использовать методы вспомогательный модуль Twitter из страницы в проекте.</span><span class="sxs-lookup"><span data-stu-id="a52f8-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="a52f8-129">В проекте необходимо заменить значения параметров со значениями, которые имеют отношение к вашим потребностям.</span><span class="sxs-lookup"><span data-stu-id="a52f8-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="a52f8-130">Можно использовать идентификаторы предоставленный мини-приложение для просмотра, как методы работают, но вам потребуется создать собственные мини-приложения для вашего проекта.</span><span class="sxs-lookup"><span data-stu-id="a52f8-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="a52f8-131">Не все параметры, показанные ниже, являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="a52f8-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="a52f8-132">Необязательные параметры позволяют настроить способ отображения кнопки или мини-приложения.</span><span class="sxs-lookup"><span data-stu-id="a52f8-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="a52f8-133">Например кнопки выполните требуется только имя пользователя, выполните, но примере показано, как включить количество подписчиков, а также как указать размер кнопки и языка.</span><span class="sxs-lookup"><span data-stu-id="a52f8-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="a52f8-134">Просмотреть результаты</span><span class="sxs-lookup"><span data-stu-id="a52f8-134">See the results</span></span>

<span data-ttu-id="a52f8-135">Этот код создает следующие кнопки и мини-приложения.</span><span class="sxs-lookup"><span data-stu-id="a52f8-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="a52f8-136">Эти кнопки и мини-приложения могут полностью работоспособно, не снимки экрана.</span><span class="sxs-lookup"><span data-stu-id="a52f8-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="a52f8-137">Кнопки выполните отображается на испанском языке, поскольку было присвоено параметра language **es**.</span><span class="sxs-lookup"><span data-stu-id="a52f8-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="a52f8-138">Выполните кнопки</span><span class="sxs-lookup"><span data-stu-id="a52f8-138">Follow Button</span></span>

<span data-ttu-id="a52f8-139">[Выполните @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="a52f8-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="a52f8-140">Кнопка твита</span><span class="sxs-lookup"><span data-stu-id="a52f8-140">Tweet Button</span></span>

<span data-ttu-id="a52f8-141">[Твит](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="a52f8-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="a52f8-142">Временная шкала пользователя (профиль)</span><span class="sxs-lookup"><span data-stu-id="a52f8-142">User Timeline (Profile)</span></span>

<span data-ttu-id="a52f8-143">[Твиты по @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="a52f8-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="a52f8-144">Избранное</span><span class="sxs-lookup"><span data-stu-id="a52f8-144">Favorites</span></span>

<span data-ttu-id="a52f8-145">[Избранные Твитов @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="a52f8-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="a52f8-146">Список</span><span class="sxs-lookup"><span data-stu-id="a52f8-146">List</span></span>

<span data-ttu-id="a52f8-147">[Твиты из @Microsoft/MS \_потребителя\_делений](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="a52f8-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="a52f8-148">Поиск</span><span class="sxs-lookup"><span data-stu-id="a52f8-148">Search</span></span>

<span data-ttu-id="a52f8-149">[О &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="a52f8-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
