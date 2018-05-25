---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Вспомогательный модуль с веб-страниц ASP.NET Twitter | Документы Microsoft
author: tfitzmac
description: Этого раздела и приложения показано, как добавить в проект WebMatrix 3 вспомогательный модуль Twitter. Он содержит код вспомогательный модуль Twitter и показан вызов вспомогательного...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/25/2018
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="ba883-104">Вспомогательное приложение Twitter с помощью веб-страниц ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ba883-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="ba883-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ba883-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ba883-106">Этого раздела и приложения показано, как добавить в проект WebMatrix 3 вспомогательный модуль Twitter.</span><span class="sxs-lookup"><span data-stu-id="ba883-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="ba883-107">Он содержит код вспомогательный модуль Twitter и показано, как вызвать вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="ba883-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="ba883-108">Этот код для файла Twitter.cshtml был разработан **Tian панорамирование** корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="ba883-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ba883-109">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="ba883-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ba883-110">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="ba883-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="ba883-111">Этот учебник также работает с веб-страницы ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="ba883-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="ba883-112">Вступление</span><span class="sxs-lookup"><span data-stu-id="ba883-112">Introduction</span></span>

<span data-ttu-id="ba883-113">В этом разделе показано, как добавить вспомогательный модуль Twitter в приложение и использовать синтаксис Razor для вызова вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="ba883-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="ba883-114">Вспомогательный модуль Twitter упрощает для включения кнопки Twitter и мини-приложения в приложении.</span><span class="sxs-lookup"><span data-stu-id="ba883-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="ba883-115">Для использования в Twitter мини-приложении, например шкалы пользователя или результатах поиска для хэштегом, необходимо сначала создать [мини-приложение в Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="ba883-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="ba883-116">После создания мини-приложения, вы получите идентификатор мини-приложения. При вызове вспомогательные методы, которые показывают мини-приложения можно передать в качестве параметра этот идентификатор мини-приложения.</span><span class="sxs-lookup"><span data-stu-id="ba883-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="ba883-117">В этом разделе была написана для версии 1.1 Twitter API.</span><span class="sxs-lookup"><span data-stu-id="ba883-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="ba883-118">Непосредственное добавление Twitter вспомогательного кода в проект, можно обновить вспомогательного кода при изменении Twitter API.</span><span class="sxs-lookup"><span data-stu-id="ba883-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="ba883-119">Сведения об установке WebMatrix см. в разделе [введение в ASP.NET Web Pages 2 — начало работы](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="ba883-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="ba883-120">Добавьте в проект вспомогательный модуль Twitter</span><span class="sxs-lookup"><span data-stu-id="ba883-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="ba883-121">Чтобы добавить вспомогательный модуль Twitter, сначала добавьте папку с именем **приложения\_код** в проект.</span><span class="sxs-lookup"><span data-stu-id="ba883-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="ba883-122">Создайте файл с именем **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="ba883-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Папки App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="ba883-124">Замените код по умолчанию в Twitter.cshtml следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="ba883-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="ba883-125">Вызов методов Twitter из веб-страниц</span><span class="sxs-lookup"><span data-stu-id="ba883-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="ba883-126">Приведенный ниже показано, как использовать методы вспомогательный модуль Twitter со страницы в проекте.</span><span class="sxs-lookup"><span data-stu-id="ba883-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="ba883-127">В проекте необходимо заменить значения параметров со значениями, которые относятся к вашим потребностям.</span><span class="sxs-lookup"><span data-stu-id="ba883-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="ba883-128">Идентификаторы предоставленный мини-приложения можно использовать для изучения того, как методы работают, но требуется создавать собственные мини-приложения для проекта.</span><span class="sxs-lookup"><span data-stu-id="ba883-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="ba883-129">Не все параметры, приведенные ниже, являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="ba883-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="ba883-130">Необязательные параметры используются для настройки отображения кнопки и мини-приложения.</span><span class="sxs-lookup"><span data-stu-id="ba883-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="ba883-131">Например кнопка выполните требуется только имя пользователя, выполните, но в примере показано входят: номер последователи и как указать размер кнопки и языка.</span><span class="sxs-lookup"><span data-stu-id="ba883-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="ba883-132">Просмотреть результаты</span><span class="sxs-lookup"><span data-stu-id="ba883-132">See the results</span></span>

<span data-ttu-id="ba883-133">Этот код создает следующие кнопки и мини-приложения.</span><span class="sxs-lookup"><span data-stu-id="ba883-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="ba883-134">Эти кнопки и мини-приложений могут полностью функциональные, не снимки экрана.</span><span class="sxs-lookup"><span data-stu-id="ba883-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="ba883-135">Кнопка выполните отображается на испанском языке, так как имеет значение параметра language **es**.</span><span class="sxs-lookup"><span data-stu-id="ba883-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="ba883-136">Выполните кнопки</span><span class="sxs-lookup"><span data-stu-id="ba883-136">Follow Button</span></span>

[<span data-ttu-id="ba883-137">Выполните @aspnet)</span><span class="sxs-lookup"><span data-stu-id="ba883-137">Follow @aspnet)</span></span>](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="tweet-button"></a><span data-ttu-id="ba883-138">Кнопка твит</span><span class="sxs-lookup"><span data-stu-id="ba883-138">Tweet Button</span></span>

[<span data-ttu-id="ba883-139">Твиты</span><span class="sxs-lookup"><span data-stu-id="ba883-139">Tweet</span></span>](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="user-timeline-profile"></a><span data-ttu-id="ba883-140">Временная шкала пользователя (профиль)</span><span class="sxs-lookup"><span data-stu-id="ba883-140">User Timeline (Profile)</span></span>

[<span data-ttu-id="ba883-141">Твиты @aspnet</span><span class="sxs-lookup"><span data-stu-id="ba883-141">Tweets by @aspnet</span></span>](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="favorites"></a><span data-ttu-id="ba883-142">Избранное</span><span class="sxs-lookup"><span data-stu-id="ba883-142">Favorites</span></span>

[<span data-ttu-id="ba883-143">Избранного Твиты по @Microsoft</span><span class="sxs-lookup"><span data-stu-id="ba883-143">Favorite Tweets by @Microsoft</span></span>](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="list"></a><span data-ttu-id="ba883-144">Список</span><span class="sxs-lookup"><span data-stu-id="ba883-144">List</span></span>

[<span data-ttu-id="ba883-145">Твиты из @Microsoft/MS \_потребителя\_делений</span><span class="sxs-lookup"><span data-stu-id="ba883-145">Tweets from @Microsoft/MS\_Consumer\_Bands</span></span>](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="search"></a><span data-ttu-id="ba883-146">Поиск</span><span class="sxs-lookup"><span data-stu-id="ba883-146">Search</span></span>

[<span data-ttu-id="ba883-147">Твитов о &quot;#asp.net&quot;</span><span class="sxs-lookup"><span data-stu-id="ba883-147">Tweets about &quot;#asp.net&quot;</span></span>](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>
