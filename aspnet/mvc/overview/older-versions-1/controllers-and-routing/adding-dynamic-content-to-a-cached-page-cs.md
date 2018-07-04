---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Добавление динамического содержимого на кэшированные страницы (C#) | Документация Майкрософт
author: microsoft
description: Узнайте, как сочетать динамическое и кэшированное содержимое, на этой же странице. Подстановка пост-кэша позволяет отображать динамическое содержимое, например o объявления заголовка...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 08f680e8d057f47a3f2802b1136edfb00634637d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364149"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="bfab7-104">Добавление динамического содержимого на кэшированные страницы (C#)</span><span class="sxs-lookup"><span data-stu-id="bfab7-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="bfab7-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bfab7-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="bfab7-106">Узнайте, как сочетать динамическое и кэшированное содержимое, на этой же странице.</span><span class="sxs-lookup"><span data-stu-id="bfab7-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="bfab7-107">Подстановка пост-кэша предоставляет возможность отображения динамического содержимого, например рекламных объявлений или новости, страницы, которая были выведены кэшируются.</span><span class="sxs-lookup"><span data-stu-id="bfab7-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="bfab7-108">Используя преимущества кэширования вывода, можно значительно повысить производительность приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bfab7-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="bfab7-109">Вместо создания заново страницу каждый раз, когда потребуются страницы, страницы могут возникать один раз и кэшируются в памяти для нескольких пользователей.</span><span class="sxs-lookup"><span data-stu-id="bfab7-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="bfab7-110">Но есть проблема.</span><span class="sxs-lookup"><span data-stu-id="bfab7-110">But there is a problem.</span></span> <span data-ttu-id="bfab7-111">Что делать, если требуется, чтобы отобразить динамическое содержимое на странице?</span><span class="sxs-lookup"><span data-stu-id="bfab7-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="bfab7-112">Например представьте, что вы хотите отобразить объявления баннер на странице.</span><span class="sxs-lookup"><span data-stu-id="bfab7-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="bfab7-113">Вы не хотите баннеров для кэширования, чтобы каждый пользователь видит те же объявление.</span><span class="sxs-lookup"><span data-stu-id="bfab7-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="bfab7-114">Вы бы прибыль таким образом!</span><span class="sxs-lookup"><span data-stu-id="bfab7-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="bfab7-115">К счастью есть простой способ.</span><span class="sxs-lookup"><span data-stu-id="bfab7-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="bfab7-116">Вы можете воспользоваться функцией платформы ASP.NET, который вызывается *подстановка после кэширования*.</span><span class="sxs-lookup"><span data-stu-id="bfab7-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="bfab7-117">Подстановка пост-кэша позволяет заменить динамического содержимого на странице, которая кэширована в памяти.</span><span class="sxs-lookup"><span data-stu-id="bfab7-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="bfab7-118">Как правило при выводе страницы кэша с помощью атрибута [OutputCache], страница кэшируется на сервере и клиент (браузер).</span><span class="sxs-lookup"><span data-stu-id="bfab7-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="bfab7-119">При использовании Подстановка пост-кэша, страница кэшируется только на сервере.</span><span class="sxs-lookup"><span data-stu-id="bfab7-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="bfab7-120">С помощью Подстановка пост-кэша</span><span class="sxs-lookup"><span data-stu-id="bfab7-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="bfab7-121">С помощью Подстановка пост-кэша в два этапа.</span><span class="sxs-lookup"><span data-stu-id="bfab7-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="bfab7-122">Во-первых необходимо определить метод, который возвращает строку, представляющую динамическое содержимое, которое будет отображаться в кэшированные страницы.</span><span class="sxs-lookup"><span data-stu-id="bfab7-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="bfab7-123">Затем вызывается метод HttpResponse.WriteSubstitution() для вставки динамического содержимого на страницы.</span><span class="sxs-lookup"><span data-stu-id="bfab7-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="bfab7-124">Например, представьте себе, необходимость в случайном порядке показать различные новости в кэшированные страницы.</span><span class="sxs-lookup"><span data-stu-id="bfab7-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="bfab7-125">Класс в листинге 1 предоставляет единственный метод, с именем RenderNews(), который случайным образом возвращает один элемент новостей из списка из трех элементов новостей.</span><span class="sxs-lookup"><span data-stu-id="bfab7-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="bfab7-126">**В листинге 1 — Models\News.cs**</span><span class="sxs-lookup"><span data-stu-id="bfab7-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="bfab7-127">Чтобы воспользоваться преимуществами Подстановка пост-кэша, можно вызвать метод HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="bfab7-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="bfab7-128">Метод WriteSubstitution() устанавливает код для замены области кэшированных страниц с динамическим содержимым.</span><span class="sxs-lookup"><span data-stu-id="bfab7-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="bfab7-129">Метод WriteSubstitution() используется для отображения случайных новостей в представлении в листинге 2.</span><span class="sxs-lookup"><span data-stu-id="bfab7-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="bfab7-130">**В листинге 2 — Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="bfab7-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="bfab7-131">Метод RenderNews передается методу WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="bfab7-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="bfab7-132">Обратите внимание на то, что метод RenderNews не вызывается (используется без скобок).</span><span class="sxs-lookup"><span data-stu-id="bfab7-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="bfab7-133">Вместо этого WriteSubstitution() передается ссылка на метод.</span><span class="sxs-lookup"><span data-stu-id="bfab7-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="bfab7-134">Представление Index кэшируется.</span><span class="sxs-lookup"><span data-stu-id="bfab7-134">The Index view is cached.</span></span> <span data-ttu-id="bfab7-135">Представление возвращает контроллер в листинге 3.</span><span class="sxs-lookup"><span data-stu-id="bfab7-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="bfab7-136">Обратите внимание на то, что действие Index() дополнен атрибутом [OutputCache], приводит к смещению представления индекса должен быть помещен в кэш 60 секунд.</span><span class="sxs-lookup"><span data-stu-id="bfab7-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="bfab7-137">**Листинг 3 – Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="bfab7-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="bfab7-138">Несмотря на то, что кэшируется в представление Index, различные Новостные случайные элементы отображаются в том случае, при запросе страницы индекса.</span><span class="sxs-lookup"><span data-stu-id="bfab7-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="bfab7-139">При запросе страницы Index, время, отображаемое на странице остается неизменным на 60 секунд (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="bfab7-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="bfab7-140">Тот факт, что время не приводит к изменению доказывает, что страница кэшируется.</span><span class="sxs-lookup"><span data-stu-id="bfab7-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="bfab7-141">Тем не менее, содержимое введенному изменении метода — случайное новости – WriteSubstitution() с каждым запросом.</span><span class="sxs-lookup"><span data-stu-id="bfab7-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="bfab7-142">**Рис. 1 – Включение динамического новости в кэшированные страницы**</span><span class="sxs-lookup"><span data-stu-id="bfab7-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="bfab7-144">С помощью Подстановка пост-кэша в вспомогательные методы</span><span class="sxs-lookup"><span data-stu-id="bfab7-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="bfab7-145">Для инкапсуляции вызов метода WriteSubstitution() пользовательского вспомогательного метода является более простой способ воспользоваться преимуществами Подстановка пост-кэша.</span><span class="sxs-lookup"><span data-stu-id="bfab7-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="bfab7-146">Этот подход проиллюстрирован с помощью вспомогательного метода в листинге 4.</span><span class="sxs-lookup"><span data-stu-id="bfab7-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="bfab7-147">**Листинг 4 — AdHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="bfab7-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="bfab7-148">Листинг 4 содержит статический класс, который предоставляет два метода: RenderBanner() и RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="bfab7-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="bfab7-149">Метод RenderBanner() представляет фактический вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="bfab7-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="bfab7-150">Этот метод расширяет стандартного класса ASP.NET MVC HtmlHelper, что Html.RenderBanner() можно вызвать в представлении так же, как и любой другой вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="bfab7-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="bfab7-151">Метод RenderBanner() вызывает метод HttpResponse.WriteSubstitution(), передавая ему RenderBannerInternal() WriteSubsitution() метода.</span><span class="sxs-lookup"><span data-stu-id="bfab7-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="bfab7-152">Метод RenderBannerInternal() — закрытый метод.</span><span class="sxs-lookup"><span data-stu-id="bfab7-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="bfab7-153">Этот метод не будет предоставляться как вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="bfab7-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="bfab7-154">Метод RenderBannerInternal() случайным образом возвращает одно изображение баннера объявления из списка три изображения баннера объявления.</span><span class="sxs-lookup"><span data-stu-id="bfab7-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="bfab7-155">Измененный представление Index в листинге 5 иллюстрирует, как можно использовать вспомогательный метод RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="bfab7-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="bfab7-156">Обратите внимание, что дополнительная &lt;% @ % импорта&gt; директива включается в верхней части представления, чтобы импортировать пространство имен MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="bfab7-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="bfab7-157">Если вы не импортируйте это пространство имен, метод RenderBanner() не будет отображаться как метод на свойстве Html.</span><span class="sxs-lookup"><span data-stu-id="bfab7-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="bfab7-158">**В листинге 5 – Views\Home\Index.aspx (с помощью метода RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="bfab7-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="bfab7-159">При запросе страницы к просмотру в представлении в листинге 5 разных баннеров отображается с каждым запросом (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="bfab7-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="bfab7-160">Страница кэшируется, однако, вспомогательный метод RenderBanner() баннеров внедряется динамически.</span><span class="sxs-lookup"><span data-stu-id="bfab7-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="bfab7-161">**Рис. 2 – индекс представления, отображающее случайных баннеров**</span><span class="sxs-lookup"><span data-stu-id="bfab7-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="bfab7-163">Сводка</span><span class="sxs-lookup"><span data-stu-id="bfab7-163">Summary</span></span>

<span data-ttu-id="bfab7-164">Этом руководстве описано, как можно динамически обновлять содержимое в кэшированные страницы.</span><span class="sxs-lookup"><span data-stu-id="bfab7-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="bfab7-165">Вы узнали, как использовать метод HttpResponse.WriteSubstitution() для включения динамического содержимого, которые следует вставить в кэшированные страницы.</span><span class="sxs-lookup"><span data-stu-id="bfab7-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="bfab7-166">Вы также узнали, как инкапсулировать вызов метода WriteSubstitution() вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="bfab7-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="bfab7-167">Преимущества кэширования по возможности — он может иметь значительное влияние на производительность веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="bfab7-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="bfab7-168">Как описано в этом руководстве, можно воспользоваться преимуществами кэширования даже в том случае, когда требуется для отображения динамического содержимого на страницы.</span><span class="sxs-lookup"><span data-stu-id="bfab7-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

> [!div class="step-by-step"]
> <span data-ttu-id="bfab7-169">[Назад](improving-performance-with-output-caching-cs.md)
> [Вперед](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="bfab7-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
