---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Добавление социальных сетей для ASP.NET Web Pages (Razor) узлов | Документы Microsoft
author: tfitzmac
description: В этой главе объясняется, как интегрировать сайт с социальными сетевых служб. В этой главе вы узнаете, как уведомить пользователей закладка веб-сайта...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="dc63a-104">Добавление социальных сетей веб-страницы ASP.NET (Razor) узлов</span><span class="sxs-lookup"><span data-stu-id="dc63a-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="dc63a-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dc63a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dc63a-106">В этой статье описывается добавление социальной сети ссылки для Facebook, Twitter, Reddit и Digg на страницы на веб-сайт ASP.NET Web Pages (Razor) и включать Twitter-каналы, игровой карты Xbox и глобально распознаваемые изображения.</span><span class="sxs-lookup"><span data-stu-id="dc63a-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="dc63a-107">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="dc63a-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="dc63a-108">Как уведомить пользователей закладка веб-узла.</span><span class="sxs-lookup"><span data-stu-id="dc63a-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="dc63a-109">Как добавить веб-канал Twitter.</span><span class="sxs-lookup"><span data-stu-id="dc63a-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="dc63a-110">Добавление Facebook **как** кнопку на страницы.</span><span class="sxs-lookup"><span data-stu-id="dc63a-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="dc63a-111">Подготовка к просмотру изображений Gravatar.com.</span><span class="sxs-lookup"><span data-stu-id="dc63a-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="dc63a-112">Как отобразить карточку игровой Xbox на вашем сайте.</span><span class="sxs-lookup"><span data-stu-id="dc63a-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="dc63a-113">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="dc63a-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="dc63a-114">Веб-страниц ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="dc63a-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="dc63a-115">Библиотека вспомогательных ASP.NET Web (пакет NuGet)</span><span class="sxs-lookup"><span data-stu-id="dc63a-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="dc63a-116">Этот учебник также работает с 3 веб-страниц ASP.NET, за исключением элементов, использующих библиотеку поддержки ASP.NET Web.</span><span class="sxs-lookup"><span data-stu-id="dc63a-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="dc63a-117">Связывание веб-сайта на сайты социальных сетей</span><span class="sxs-lookup"><span data-stu-id="dc63a-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="dc63a-118">Если нравится что-нибудь на сайте, они часто требуется использовать его совместно с друзьями.</span><span class="sxs-lookup"><span data-stu-id="dc63a-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="dc63a-119">Вы может облегчить эту задачу, отображая глифы (значки), которые пользователи могут нажимать для совместного использования на страницу Digg, Reddit, Facebook, Twitter или похожих узлов.</span><span class="sxs-lookup"><span data-stu-id="dc63a-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="dc63a-120">Чтобы отобразить эти глифы, добавьте `LinkSharecode` вспомогательный метод для страницы.</span><span class="sxs-lookup"><span data-stu-id="dc63a-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="dc63a-121">Щелкните глиф в виде отдельных пользователей, посетите страницу.</span><span class="sxs-lookup"><span data-stu-id="dc63a-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="dc63a-122">При наличии учетной записи с этот сайт социальной сети они затем учесть ссылка на страницу на этом сайте.</span><span class="sxs-lookup"><span data-stu-id="dc63a-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![Рисунок 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="dc63a-124">Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если не было добавлено его - создать страницу с именем *ListLinkShare.cshtml* и добавьте следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="dc63a-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="dc63a-125">В этом примере при `LinkShare` вспомогательный запусков, заголовок страницы передается в качестве параметра, который в свою очередь, передает заголовок страницы, чтобы сайт социальной сети.</span><span class="sxs-lookup"><span data-stu-id="dc63a-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="dc63a-126">Тем не менее можно передать во все нужные строки.</span><span class="sxs-lookup"><span data-stu-id="dc63a-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="dc63a-127">В примере также указано, какие сайты социальных сетей для включения в список.</span><span class="sxs-lookup"><span data-stu-id="dc63a-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="dc63a-128">Можно указать сайты социальных сетей, которые относятся к веб-узла.</span><span class="sxs-lookup"><span data-stu-id="dc63a-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="dc63a-129">Запустите *ListLinkShare.cshtml* страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="dc63a-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="dc63a-130">(Убедитесь, что выбран страницы **файлы** рабочей области, прежде чем запускать его.)</span><span class="sxs-lookup"><span data-stu-id="dc63a-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="dc63a-131">Щелкните глиф для одного из узлов, выполнив вход в службе.</span><span class="sxs-lookup"><span data-stu-id="dc63a-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="dc63a-132">Этой ссылке вы перейдете на страницу на сайте выбранного социальных сетей, где вы можете совместно использовать ссылку.</span><span class="sxs-lookup"><span data-stu-id="dc63a-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="dc63a-133">Например, если щелкнуть ссылку Reddit будет выполняется переход к `submit to reddit` Reddit веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="dc63a-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![Рисунок 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="dc63a-135">Добавление Twitter веб-канала.</span><span class="sxs-lookup"><span data-stu-id="dc63a-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="dc63a-136">Сведения об использовании Twitter вспомогательный класс, который совместим с текущей версией Twitter API см. в разделе [вспомогательный модуль Twitter](../ui-layouts-and-themes/twitter-helper.md).</span><span class="sxs-lookup"><span data-stu-id="dc63a-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="dc63a-137">В этом примере показано, как написать собственный вспомогательный, легко можно повторно использовать код из нескольких страниц.</span><span class="sxs-lookup"><span data-stu-id="dc63a-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="dc63a-138">Отображение Facebook &quot;как&quot; кнопка</span><span class="sxs-lookup"><span data-stu-id="dc63a-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="dc63a-139">В некоторых случаях лучшим вариантом является получение кода непосредственно из социальной сети поставщика, а не полагаться на вспомогательного класса.</span><span class="sxs-lookup"><span data-stu-id="dc63a-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="dc63a-140">Это особенно важно, если поставщик социальной сети быстрее, чем вспомогательное приложение обновляется обновляет его параметры.</span><span class="sxs-lookup"><span data-stu-id="dc63a-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="dc63a-141">Чтобы добавить компоненты Facebook (например, кнопку "Like") на сайте, можно получить фрагменты кода из [developers.facebook.com](https://developers.facebook.com/) сайта.</span><span class="sxs-lookup"><span data-stu-id="dc63a-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="dc63a-142">На сайте Facebook использование инструментов для создания фрагмента кода, который относится к веб-узла.</span><span class="sxs-lookup"><span data-stu-id="dc63a-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="dc63a-143">Следующий выделенный код является код, который был извлечен из средства как кнопка на сайте developers.facebook.com.</span><span class="sxs-lookup"><span data-stu-id="dc63a-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="dc63a-144">Необходимо указать идентификатор собственных приложений.</span><span class="sxs-lookup"><span data-stu-id="dc63a-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="dc63a-145">Подготовка к просмотру изображений глобально распознаваемые</span><span class="sxs-lookup"><span data-stu-id="dc63a-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="dc63a-146">A *глобально распознаваемые* ( &quot;глобально распознаваемым аватара&quot;) — это изображение, можно использовать на нескольких веб-сайтов в качестве аватара &#8212; то есть изображение, представляющее вы.</span><span class="sxs-lookup"><span data-stu-id="dc63a-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="dc63a-147">Например глобально распознаваемые можно определить сотрудника в записи на форуме, в блоге комментарий и так далее.</span><span class="sxs-lookup"><span data-stu-id="dc63a-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="dc63a-148">(Вы можете зарегистрировать свои собственные глобально распознаваемые на веб-сайте глобально распознаваемые в [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Если вы хотите отображать изображения, рядом с именами пользователей или адреса электронной почты на веб-сайте, можно использовать вспомогательный глобально распознаваемые.</span><span class="sxs-lookup"><span data-stu-id="dc63a-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="dc63a-149">В этом примере вы используете один глобально распознаваемые, представляющий самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="dc63a-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="dc63a-150">Еще один способ использования глобально распознаваемые — позволяют указать свой адрес глобально распознаваемые при регистрации веб-узла.</span><span class="sxs-lookup"><span data-stu-id="dc63a-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="dc63a-151">(Можно узнать, как уведомить пользователей при регистрации в [Добавление безопасности и членство для узла веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).) Затем каждый раз, когда отображаются сведения для этого пользователя, можно просто добавить глобально распознаваемые где отображение имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="dc63a-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="dc63a-152">Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если это еще не сделано.</span><span class="sxs-lookup"><span data-stu-id="dc63a-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="dc63a-153">Создать новый веб-страницу с именем *Gravatar.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dc63a-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="dc63a-154">Добавьте следующую разметку в файл:</span><span class="sxs-lookup"><span data-stu-id="dc63a-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="dc63a-155">`Gravatar.GetHtml` Метод отображает глобально распознаваемые изображение на странице.</span><span class="sxs-lookup"><span data-stu-id="dc63a-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="dc63a-156">Чтобы изменить размер изображения, может включать несколько вторым параметром.</span><span class="sxs-lookup"><span data-stu-id="dc63a-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="dc63a-157">Размер по умолчанию — 80.</span><span class="sxs-lookup"><span data-stu-id="dc63a-157">The default size is 80.</span></span> <span data-ttu-id="dc63a-158">Числа меньше 80 Создание изображения меньшего размера.</span><span class="sxs-lookup"><span data-stu-id="dc63a-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="dc63a-159">Номера больше 80 увеличить размер изображения.</span><span class="sxs-lookup"><span data-stu-id="dc63a-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="dc63a-160">В `Gravatar.GetHtml` методы, заменяют `<Your Gravatar account here>` с адресом электронной почты, который используется для вашей учетной записи глобально распознаваемые.</span><span class="sxs-lookup"><span data-stu-id="dc63a-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="dc63a-161">(При отсутствии учетной записи глобально распознаваемые можно использовать адрес электронной почты пользователя, выполняющего.)</span><span class="sxs-lookup"><span data-stu-id="dc63a-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="dc63a-162">Запустите страницу в браузере.</span><span class="sxs-lookup"><span data-stu-id="dc63a-162">Run the page in your browser.</span></span> <span data-ttu-id="dc63a-163">На странице отображаются два глобально распознаваемые изображения для указанный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="dc63a-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="dc63a-164">Второе изображение меньше, чем первый.</span><span class="sxs-lookup"><span data-stu-id="dc63a-164">The second image is smaller than the first.</span></span> 

    ![Рисунок 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="dc63a-166">Отображение игровой карты Xbox</span><span class="sxs-lookup"><span data-stu-id="dc63a-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="dc63a-167">Когда пользователи игры Xbox корпорации Майкрософт через Интернет, каждый пользователь имеет уникальный идентификатор.</span><span class="sxs-lookup"><span data-stu-id="dc63a-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="dc63a-168">Статистику для каждого исполнителя в форме игровой карты, которые показаны их репутации, игровой оценки, а также недавно четных игры.</span><span class="sxs-lookup"><span data-stu-id="dc63a-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="dc63a-169">Если вы игровой Xbox, Показать вашей игровой карты на страницах веб-узла с помощью `GamerCard` поддержки.</span><span class="sxs-lookup"><span data-stu-id="dc63a-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="dc63a-170">Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если это еще не сделано.</span><span class="sxs-lookup"><span data-stu-id="dc63a-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="dc63a-171">Создание новой страницы с именем *XboxGamer.cshtml* и добавьте следующую разметку.</span><span class="sxs-lookup"><span data-stu-id="dc63a-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="dc63a-172">Вы используете `GamerCard.GetHtml` свойство, чтобы указать псевдоним игровой карты для отображения.</span><span class="sxs-lookup"><span data-stu-id="dc63a-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="dc63a-173">Запустите страницу в браузере.</span><span class="sxs-lookup"><span data-stu-id="dc63a-173">Run the page in your browser.</span></span> <span data-ttu-id="dc63a-174">На странице отображается игровой карты Xbox, указанный вами.</span><span class="sxs-lookup"><span data-stu-id="dc63a-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![Рисунок 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
