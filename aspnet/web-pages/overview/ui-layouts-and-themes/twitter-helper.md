---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Вспомогательный модуль с помощью ASP.NET Web Pages Twitter | Документация Майкрософт
author: tfitzmac
description: Этот раздел и приложения показано, как добавить вспомогательный модуль Twitter для WebMatrix 3 проекта. Он содержит код вспомогательный модуль Twitter и показан способ вызова вспомогательного приложения...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 06223826c2682ffd62d5a1717f34242f39be5eda
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837553"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="8f838-104">Вспомогательное приложение Twitter с помощью веб-страниц ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8f838-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="8f838-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8f838-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8f838-106">Этот раздел и приложения показано, как добавить вспомогательный модуль Twitter для WebMatrix 3 проекта.</span><span class="sxs-lookup"><span data-stu-id="8f838-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="8f838-107">Он содержит код вспомогательный модуль Twitter и демонстрируется вызов вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="8f838-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="8f838-108">Этот код для файла Twitter.cshtml была разработана **Tian Pan** корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="8f838-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8f838-109">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="8f838-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8f838-110">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="8f838-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="8f838-111">Этот учебник также работает с ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="8f838-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="8f838-112">Вступление</span><span class="sxs-lookup"><span data-stu-id="8f838-112">Introduction</span></span>

<span data-ttu-id="8f838-113">В этом разделе показано, как добавить вспомогательный модуль Twitter для приложения и используйте синтаксис Razor для вызова вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="8f838-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="8f838-114">Вспомогательный модуль Twitter упрощает процесс включения кнопки Twitter и мини-приложения в приложении.</span><span class="sxs-lookup"><span data-stu-id="8f838-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="8f838-115">Для использования мини-приложения Twitter, например временную шкалу пользователя или результатов поиска по хэштегу, необходимо сначала создать [мини-приложение в Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="8f838-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="8f838-116">После создания мини-приложение, вы получите идентификатор мини-приложения. При вызове вспомогательные методы, которые показывают мини-приложения можно передать в качестве параметра этот идентификатор мини-приложения.</span><span class="sxs-lookup"><span data-stu-id="8f838-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="8f838-117">Эта статья написана для Twitter API версии 1.1.</span><span class="sxs-lookup"><span data-stu-id="8f838-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="8f838-118">Путем непосредственного добавления Twitter вспомогательного кода в проект, можно обновить вспомогательный код, при изменении Twitter API.</span><span class="sxs-lookup"><span data-stu-id="8f838-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="8f838-119">Сведения об установке WebMatrix, см. в разделе [введение в ASP.NET Web Pages 2 - Приступая к работе](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="8f838-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="8f838-120">Добавьте вспомогательный модуль Twitter в проект</span><span class="sxs-lookup"><span data-stu-id="8f838-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="8f838-121">Чтобы добавить вспомогательный модуль Twitter, во-первых, добавьте папку с именем **приложения\_кода** в проект.</span><span class="sxs-lookup"><span data-stu-id="8f838-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="8f838-122">Затем создайте файл с именем **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="8f838-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Папка App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="8f838-124">Замените код по умолчанию в Twitter.cshtml следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="8f838-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="8f838-125">Вызывайте методы Twitter из веб-страниц</span><span class="sxs-lookup"><span data-stu-id="8f838-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="8f838-126">Приведенный ниже показано, как использовать методы вспомогательный модуль Twitter из страницы в проекте.</span><span class="sxs-lookup"><span data-stu-id="8f838-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="8f838-127">В проекте необходимо заменить значения параметров со значениями, которые имеют отношение к вашим потребностям.</span><span class="sxs-lookup"><span data-stu-id="8f838-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="8f838-128">Можно использовать идентификаторы предоставленный мини-приложение для просмотра, как методы работают, но вам потребуется создать собственные мини-приложения для вашего проекта.</span><span class="sxs-lookup"><span data-stu-id="8f838-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="8f838-129">Не все параметры, показанные ниже, являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="8f838-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="8f838-130">Необязательные параметры позволяют настроить способ отображения кнопки или мини-приложения.</span><span class="sxs-lookup"><span data-stu-id="8f838-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="8f838-131">Например кнопки выполните требуется только имя пользователя, выполните, но примере показано, как включить количество подписчиков, а также как указать размер кнопки и языка.</span><span class="sxs-lookup"><span data-stu-id="8f838-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="8f838-132">Просмотреть результаты</span><span class="sxs-lookup"><span data-stu-id="8f838-132">See the results</span></span>

<span data-ttu-id="8f838-133">Этот код создает следующие кнопки и мини-приложения.</span><span class="sxs-lookup"><span data-stu-id="8f838-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="8f838-134">Эти кнопки и мини-приложения могут полностью работоспособно, не снимки экрана.</span><span class="sxs-lookup"><span data-stu-id="8f838-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="8f838-135">Кнопки выполните отображается на испанском языке, поскольку было присвоено параметра language **es**.</span><span class="sxs-lookup"><span data-stu-id="8f838-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="8f838-136">Выполните кнопки</span><span class="sxs-lookup"><span data-stu-id="8f838-136">Follow Button</span></span>

<span data-ttu-id="8f838-137">[Выполните @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="8f838-137">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="8f838-138">Кнопка твита</span><span class="sxs-lookup"><span data-stu-id="8f838-138">Tweet Button</span></span>

<span data-ttu-id="8f838-139">[Твит](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="8f838-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="8f838-140">Временная шкала пользователя (профиль)</span><span class="sxs-lookup"><span data-stu-id="8f838-140">User Timeline (Profile)</span></span>

<span data-ttu-id="8f838-141">[Твиты по @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="8f838-141">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="8f838-142">Избранное</span><span class="sxs-lookup"><span data-stu-id="8f838-142">Favorites</span></span>

<span data-ttu-id="8f838-143">[Избранные Твитов @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="8f838-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="8f838-144">Список</span><span class="sxs-lookup"><span data-stu-id="8f838-144">List</span></span>

<span data-ttu-id="8f838-145">[Твиты из @Microsoft/MS \_потребителя\_делений](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="8f838-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="8f838-146">Поиск</span><span class="sxs-lookup"><span data-stu-id="8f838-146">Search</span></span>

<span data-ttu-id="8f838-147">[О &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="8f838-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
