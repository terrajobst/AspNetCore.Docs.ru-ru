---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: "Добавление динамического контента в кэшированные страницы (C#) | Документы Microsoft"
author: microsoft
description: "Дополнительные сведения о смешивать динамических и кэшированное содержимое на одной странице. Подстановки после кэширования позволяет отображать динамическое содержимое, например o объявления баннер..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: bee7e17ee16d75419c215558b1deb7d6f0d79448
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="d6444-104">Добавление динамического контента в кэшированные страницы (C#)</span><span class="sxs-lookup"><span data-stu-id="d6444-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="d6444-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d6444-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d6444-106">Дополнительные сведения о смешивать динамических и кэшированное содержимое на одной странице.</span><span class="sxs-lookup"><span data-stu-id="d6444-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="d6444-107">Подстановки после кэширования позволяет отображать динамическое содержимое, например рекламных объявлений или новости, страницы, которая есть выходные данные в кэше.</span><span class="sxs-lookup"><span data-stu-id="d6444-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="d6444-108">Используя преимущества кэширования вывода, могут существенно повысить производительность приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d6444-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="d6444-109">Вместо повторного создания страницы каждую страницу при запросе страницы можно создать раза и кэшируются в памяти для нескольких пользователей.</span><span class="sxs-lookup"><span data-stu-id="d6444-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="d6444-110">Но есть проблема.</span><span class="sxs-lookup"><span data-stu-id="d6444-110">But there is a problem.</span></span> <span data-ttu-id="d6444-111">Что делать, если необходимо отобразить динамическое содержимое на странице?</span><span class="sxs-lookup"><span data-stu-id="d6444-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="d6444-112">Например предположим, требуется объявление баннер отобразить на странице.</span><span class="sxs-lookup"><span data-stu-id="d6444-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="d6444-113">Вы не хотите баннерной должен быть кэширован, чтобы каждый пользователь видит точно такой же объявление.</span><span class="sxs-lookup"><span data-stu-id="d6444-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="d6444-114">Денежные средства бы сделать в этом случае!</span><span class="sxs-lookup"><span data-stu-id="d6444-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="d6444-115">К счастью есть простое решение.</span><span class="sxs-lookup"><span data-stu-id="d6444-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="d6444-116">Можно воспользоваться преимуществами функции платформы ASP.NET, который называется *подстановка после кэширования*.</span><span class="sxs-lookup"><span data-stu-id="d6444-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="d6444-117">Подстановки после кэширования позволяет заменить динамическое содержимое на странице, кэшированные в памяти.</span><span class="sxs-lookup"><span data-stu-id="d6444-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="d6444-118">Как правило при выводе страницы кэша с помощью атрибута [OutputCache], страница кэшируется на сервере и клиенте (веб-браузер).</span><span class="sxs-lookup"><span data-stu-id="d6444-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="d6444-119">При использовании подстановки после кэширования страница кэшируется только на сервере.</span><span class="sxs-lookup"><span data-stu-id="d6444-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="d6444-120">С помощью подстановки после кэширования</span><span class="sxs-lookup"><span data-stu-id="d6444-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="d6444-121">С помощью подстановки после кэширования в два этапа.</span><span class="sxs-lookup"><span data-stu-id="d6444-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="d6444-122">Во-первых необходимо определить метод, который возвращает строку, представляющую динамическое содержимое, которое будет отображаться в кэшированной страницы.</span><span class="sxs-lookup"><span data-stu-id="d6444-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="d6444-123">Далее можно вызвать метод HttpResponse.WriteSubstitution() для вставки динамическое содержимое на странице.</span><span class="sxs-lookup"><span data-stu-id="d6444-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="d6444-124">Пусть, например, требуется случайным образом отобразить элементы различных новостей кэшированных страниц.</span><span class="sxs-lookup"><span data-stu-id="d6444-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="d6444-125">Класс в список 1 предоставляет один метод с именем RenderNews(), которая случайным образом возвращает один элемент новостей из списка из трех элементов новостей.</span><span class="sxs-lookup"><span data-stu-id="d6444-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="d6444-126">**Перечисление Models\News.cs. 1.**</span><span class="sxs-lookup"><span data-stu-id="d6444-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="d6444-127">Чтобы воспользоваться преимуществами подстановки после кэширования, следует вызвать метод HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="d6444-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="d6444-128">Метод WriteSubstitution() устанавливает код с динамическим содержимым, замените кэшированных страниц.</span><span class="sxs-lookup"><span data-stu-id="d6444-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="d6444-129">Метод WriteSubstitution() используется для отображения случайных новостей в представлении в списке 2.</span><span class="sxs-lookup"><span data-stu-id="d6444-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="d6444-130">**Перечисление Views\Home\Index.aspx. 2.**</span><span class="sxs-lookup"><span data-stu-id="d6444-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="d6444-131">Метод RenderNews передается методу WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="d6444-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="d6444-132">Обратите внимание, что не вызывается метод RenderNews (отсутствуют круглые скобки).</span><span class="sxs-lookup"><span data-stu-id="d6444-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="d6444-133">Вместо этого WriteSubstitution() передается ссылка на метод.</span><span class="sxs-lookup"><span data-stu-id="d6444-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="d6444-134">Представление Index кэшируется.</span><span class="sxs-lookup"><span data-stu-id="d6444-134">The Index view is cached.</span></span> <span data-ttu-id="d6444-135">Возвращает представление контроллера в списке 3.</span><span class="sxs-lookup"><span data-stu-id="d6444-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="d6444-136">Обратите внимание, что действие Index() помечено атрибутом [OutputCache], который вызывает представление Index на кэширование в течение 60 секунд.</span><span class="sxs-lookup"><span data-stu-id="d6444-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="d6444-137">**Перечисление Controllers\HomeController.cs. 3.**</span><span class="sxs-lookup"><span data-stu-id="d6444-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="d6444-138">Несмотря на то, что кэшируются представление Index различных случайных новости отображаются при запросе страницы индекса.</span><span class="sxs-lookup"><span data-stu-id="d6444-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="d6444-139">При запросе страницы индекса, время, отображаемое на странице не изменяет 60 секунд (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="d6444-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="d6444-140">Тот факт, что не изменяет время подтверждает, что страница кэшируется.</span><span class="sxs-lookup"><span data-stu-id="d6444-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="d6444-141">Однако содержимое введенному изменение метода — случайных новости — WriteSubstitution() с каждым запросом.</span><span class="sxs-lookup"><span data-stu-id="d6444-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="d6444-142">**Рис. 1 – Добавление динамического новости в кэшированной страницы**</span><span class="sxs-lookup"><span data-stu-id="d6444-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="d6444-144">С помощью подстановки после кэширования в вспомогательные методы</span><span class="sxs-lookup"><span data-stu-id="d6444-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="d6444-145">Для инкапсуляции вызов метода WriteSubstitution() внутри пользовательских вспомогательных метода является более простой способ воспользоваться преимуществами подстановки после кэширования.</span><span class="sxs-lookup"><span data-stu-id="d6444-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="d6444-146">Вспомогательный метод со списком 4 показана этот подход.</span><span class="sxs-lookup"><span data-stu-id="d6444-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="d6444-147">**Листинг 4 – AdHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="d6444-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="d6444-148">Листинг 4 содержит статический класс, имеющий два метода: RenderBanner() и RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="d6444-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="d6444-149">Метод RenderBanner() представляет фактический вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="d6444-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="d6444-150">Этот метод расширяет стандартный класс ASP.NET MVC HtmlHelper, чтобы Html.RenderBanner() можно вызвать в представлении так же, как вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="d6444-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="d6444-151">Метод RenderBanner() вызывает метод HttpResponse.WriteSubstitution() передачу метод RenderBannerInternal() метод WriteSubsitution().</span><span class="sxs-lookup"><span data-stu-id="d6444-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="d6444-152">Метод RenderBannerInternal() является закрытым.</span><span class="sxs-lookup"><span data-stu-id="d6444-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="d6444-153">Этот метод не будет предоставляться как вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="d6444-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="d6444-154">Метод RenderBannerInternal() случайным образом возвращает одно изображение баннера объявления из списка три объявления изображения баннера.</span><span class="sxs-lookup"><span data-stu-id="d6444-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="d6444-155">Измененный представление Index в листинге 5 показано, как можно использовать вспомогательный метод RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="d6444-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="d6444-156">Обратите внимание, что дополнительный &lt;% @ импорта %&gt; директива включается в верхней части представления, чтобы импортировать пространство имен MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="d6444-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="d6444-157">Если не импортировать это пространство имен, метод RenderBanner() не будет отображаться как метод для свойств Html.</span><span class="sxs-lookup"><span data-stu-id="d6444-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="d6444-158">**Листинг 5 – Views\Home\Index.aspx (с помощью метода RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="d6444-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="d6444-159">При запросе страницы к просмотру в представлении в листинге 5 разных баннерной отображается с каждым запросом (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="d6444-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="d6444-160">Страница кэшируется, но баннерной встраивается динамически с помощью вспомогательного метода RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="d6444-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="d6444-161">**Рис. 2 – отображение случайных баннерной представление Index**</span><span class="sxs-lookup"><span data-stu-id="d6444-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="d6444-163">Сводка</span><span class="sxs-lookup"><span data-stu-id="d6444-163">Summary</span></span>

<span data-ttu-id="d6444-164">Этот учебник описано, как динамически обновить содержимое в кэшированных страниц.</span><span class="sxs-lookup"><span data-stu-id="d6444-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="d6444-165">Вы узнали, как использовать метод HttpResponse.WriteSubstitution(), чтобы включить динамическое содержимое, вставляемый в кэшированных страниц.</span><span class="sxs-lookup"><span data-stu-id="d6444-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="d6444-166">Вы также узнали, как инкапсулировать вызов метода WriteSubstitution() вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="d6444-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="d6444-167">Преимущества кэширования по возможности — он может оказывать значительное влияние на производительность веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="d6444-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="d6444-168">Как описано в этом учебнике, можно воспользоваться преимуществами кэширования даже в том случае, если необходимо отобразить динамическое содержимое на страницах.</span><span class="sxs-lookup"><span data-stu-id="d6444-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

>[!div class="step-by-step"]
<span data-ttu-id="d6444-169">[Назад](improving-performance-with-output-caching-cs.md)
[Вперед](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d6444-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
