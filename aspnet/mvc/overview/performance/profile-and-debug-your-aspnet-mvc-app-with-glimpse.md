---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Профилирование и отладка приложения ASP.NET MVC с помощью Glimpse | Документация Майкрософт
author: Rick-Anderson
description: Glimpse — бурно и семейству пакетов NuGet с открытым кодом, предоставляющий подробные сведения о производительности, отладку и диагностических сведений для ASP.NET...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: f5d174ff6823d654a24dcb2c90f10a3cbd24f1e7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829467"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="a3be2-103">Профилирование и отладка приложения ASP.NET MVC с помощью Glimpse</span><span class="sxs-lookup"><span data-stu-id="a3be2-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="a3be2-104">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a3be2-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="a3be2-105">Glimpse — бурно и семейству пакетов NuGet с открытым кодом, предоставляющий подробные сведения о производительности, отладку и диагностические сведения для приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a3be2-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="a3be2-106">Он просто установить, простой и сверхбыстрой и отображает ключевые показатели производительности в нижней части каждой страницы.</span><span class="sxs-lookup"><span data-stu-id="a3be2-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="a3be2-107">Он дает возможность углубиться в приложении, если вам нужно узнать, что происходит на сервере.</span><span class="sxs-lookup"><span data-stu-id="a3be2-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="a3be2-108">Glimpse предоставляет намного ценную информацию, мы рекомендуем использовать его на протяжении всего цикла разработки, включая Azure тестовой среды.</span><span class="sxs-lookup"><span data-stu-id="a3be2-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="a3be2-109">Хотя [Fiddler](http://www.telerik.com/fiddler) и [средства разработки F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) предоставляют на стороне клиента представления, представление предоставляет подробную информацию с сервера.</span><span class="sxs-lookup"><span data-stu-id="a3be2-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="a3be2-110">Этот учебник посвящен с помощью Glimpse ASP.NET MVC и EF пакеты, но доступны многие другие пакеты.</span><span class="sxs-lookup"><span data-stu-id="a3be2-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="a3be2-111">По возможности я будет связан соответствующий [Окиньте документация](http://getglimpse.com/Docs/) которого я целях.</span><span class="sxs-lookup"><span data-stu-id="a3be2-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="a3be2-112">Представление — это проект с открытым исходным кодом, слишком у вас есть доступ к исходному коду и документы.</span><span class="sxs-lookup"><span data-stu-id="a3be2-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="a3be2-113">Установка краткого описания</span><span class="sxs-lookup"><span data-stu-id="a3be2-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="a3be2-114">Включить краткого описания для localhost</span><span class="sxs-lookup"><span data-stu-id="a3be2-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="a3be2-115">На вкладке временной шкалы</span><span class="sxs-lookup"><span data-stu-id="a3be2-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="a3be2-116">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="a3be2-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="a3be2-117">Маршруты</span><span class="sxs-lookup"><span data-stu-id="a3be2-117">Routes</span></span>](#route)
- [<span data-ttu-id="a3be2-118">С помощью Glimpse в Azure</span><span class="sxs-lookup"><span data-stu-id="a3be2-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="a3be2-119">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a3be2-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="a3be2-120">Установка краткого описания</span><span class="sxs-lookup"><span data-stu-id="a3be2-120">Installing Glimpse</span></span>

<span data-ttu-id="a3be2-121">Представление можно установить из консоли диспетчера пакетов NuGet или **управление пакетами NuGet** консоли.</span><span class="sxs-lookup"><span data-stu-id="a3be2-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="a3be2-122">Для этой демонстрации я установлю Mvc5 и EF6 пакеты:</span><span class="sxs-lookup"><span data-stu-id="a3be2-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Установка Glimpse из NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="a3be2-124">Поиск *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="a3be2-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF из NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="a3be2-126">Выбрав **установленные пакеты**, вы увидите Glimpse зависимые модули, установленные:</span><span class="sxs-lookup"><span data-stu-id="a3be2-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Установленные пакеты Glimpse из DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="a3be2-128">Следующие команды устанавливают модули Glimpse MVC5 и EF6 из консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="a3be2-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="a3be2-129">Включить краткого описания для localhost</span><span class="sxs-lookup"><span data-stu-id="a3be2-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="a3be2-130">Перейдите к http://localhost:&lt; порт #&gt;/glimpse.axd и нажмите кнопку <strong>Включение Glimpse</strong> кнопки.</span><span class="sxs-lookup"><span data-stu-id="a3be2-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Glimpse axd страницы](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="a3be2-132">При наличии отображается панель избранного, можно перетащить и drop краткого описания кнопок и добавлять их в виде bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="a3be2-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE с помощью Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="a3be2-134">Приложения, теперь можно перейти и **головок до отображения** (HUD) отображается в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="a3be2-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Страница диспетчера контактов с HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="a3be2-136">[Glimpse HUD страницы](http://getglimpse.com/Docs/Heads-up-Display) указана информация о времени, показанный выше.</span><span class="sxs-lookup"><span data-stu-id="a3be2-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="a3be2-137">Отображение данных HUD ненавязчивого производительности может уведомить о проблемах немедленно - до перехода к цикл тестирования.</span><span class="sxs-lookup"><span data-stu-id="a3be2-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="a3be2-138">Щелкнув &quot;g&quot; в правом нижнем углу появится в панели «представление»:</span><span class="sxs-lookup"><span data-stu-id="a3be2-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Обзор панели](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="a3be2-140">На рисунке выше [вкладку выполнение](http://getglimpse.com/Docs/Execution-Tab) выбран, показывающий сведений о времени для действий и фильтры в конвейере.</span><span class="sxs-lookup"><span data-stu-id="a3be2-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="a3be2-141">Можно увидеть мой [Watch остановить фильтр таймера](http://www.nuget.org/packages/StopWatch/) запустить на этапе 6 конвейера.</span><span class="sxs-lookup"><span data-stu-id="a3be2-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="a3be2-142">Хотя моей таймера небольшого размера могут обеспечить полезные профиля и синхронизации данных, она игнорирует все время, затраченное на авторизации и отображении представления.</span><span class="sxs-lookup"><span data-stu-id="a3be2-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="a3be2-143">Сведения о моем таймера в [профиля и времени приложения ASP.NET MVC, вплоть до Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3be2-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="a3be2-144">[Вкладки](http://getglimpse.com/Docs/Tabs) странице представлены ссылки на подробные сведения на каждой вкладке.</span><span class="sxs-lookup"><span data-stu-id="a3be2-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="a3be2-145">На вкладке временной шкалы</span><span class="sxs-lookup"><span data-stu-id="a3be2-145">The Timeline tab</span></span>

<span data-ttu-id="a3be2-146">Я изменил том Дайкстра необработанных [учебник по EF MVC 5,6/](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) следующим кодом Сменить контроллер instructors:</span><span class="sxs-lookup"><span data-stu-id="a3be2-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="a3be2-147">Приведенный выше код позволяет мне передавать в строке запроса (`eager`) к элементу управления, упреждающая и явная загрузка данных.</span><span class="sxs-lookup"><span data-stu-id="a3be2-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="a3be2-148">В приведенном ниже рисунке используется явная загрузка и на странице о времени отображаются каждой регистрации, загруженных в `Index` метода действия:</span><span class="sxs-lookup"><span data-stu-id="a3be2-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![явная загрузка](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="a3be2-150">В следующем коде задается Безотложная, и каждой регистрации извлекается после `Index` представление называется:</span><span class="sxs-lookup"><span data-stu-id="a3be2-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![Eager указан](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="a3be2-152">Наводите указатель мыши на сегмент времени, чтобы получить подробные сведения о времени:</span><span class="sxs-lookup"><span data-stu-id="a3be2-152">You can hover over a time segment to get detailed timing information:</span></span>

![При наведении указателя мыши, чтобы просмотреть подробные сведения о времени](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="a3be2-154">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="a3be2-154">Model Binding</span></span>

<span data-ttu-id="a3be2-155">[Вкладке привязки модели](http://getglimpse.com/Docs/Model-Binding-Tab) предоставляет широкий набор сведений, которые помогут вам понять, как связаны переменных формы и почему некоторые не привязаны, как ожидал.</span><span class="sxs-lookup"><span data-stu-id="a3be2-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="a3be2-156">На рисунке ниже показано **?**</span><span class="sxs-lookup"><span data-stu-id="a3be2-156">The image below shows the **?**</span></span> <span data-ttu-id="a3be2-157">значок, который можно щелкнуть для открытия страницы справки краткого описания для этой функции.</span><span class="sxs-lookup"><span data-stu-id="a3be2-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![окиньте представление привязки модели](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="a3be2-159">Маршруты</span><span class="sxs-lookup"><span data-stu-id="a3be2-159">Routes</span></span>

 <span data-ttu-id="a3be2-160">На вкладке Glimpse маршруты можно поможет отладки и понимания маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="a3be2-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="a3be2-161">В приведенном ниже рисунке продукта маршрут выбирается (и показывает зеленым цветом, соглашение Glimpse).</span><span class="sxs-lookup"><span data-stu-id="a3be2-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="a3be2-162">![Имя продукта, выбранное](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) также отображаются маркеры ограничения, областей и данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="a3be2-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="a3be2-163">См. в разделе [маршруты Glimpse](http://getglimpse.com/Docs/Routes-Tab) и [маршрутизации в ASP.NET MVC 5 с помощью атрибутов](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="a3be2-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="a3be2-164">С помощью Glimpse в Azure</span><span class="sxs-lookup"><span data-stu-id="a3be2-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="a3be2-165">Политика безопасности по умолчанию представление позволяет только представление данных для отображения из локального узла.</span><span class="sxs-lookup"><span data-stu-id="a3be2-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="a3be2-166">Можно изменить эту политику безопасности, чтобы можно было просматривать эти данные на удаленном сервере (например, веб-приложения в Azure).</span><span class="sxs-lookup"><span data-stu-id="a3be2-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="a3be2-167">Для сред тестирования в Azure, добавьте знак выделенный до нижней части *web.confg* файл, чтобы включить представление:</span><span class="sxs-lookup"><span data-stu-id="a3be2-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="a3be2-168">Только после этого изменения любой пользователь может видеть представление данных на удаленном узле.</span><span class="sxs-lookup"><span data-stu-id="a3be2-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="a3be2-169">Рассмотрите возможность добавления разметки выше профиль публикации, поэтому только развертывания примененного, при использовании этого профиля публикации (например, ваш тестирования Azure proifle.) Для ограничения доступа к данным краткого описания, мы добавим `canViewGlimpseData` роли и только пользователи с этой ролью, чтобы просмотреть представление данных.</span><span class="sxs-lookup"><span data-stu-id="a3be2-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="a3be2-170">Удалить комментарии из *GlimpseSecurityPolicy.cs* файлов и изменить [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) вызов из `Administrator` для `canViewGlimpseData` роли:</span><span class="sxs-lookup"><span data-stu-id="a3be2-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="a3be2-171">Безопасность — мультимедийными данными, предоставляемые Glimpse может привести к безопасности приложения.</span><span class="sxs-lookup"><span data-stu-id="a3be2-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="a3be2-172">Майкрософт не производит аудита безопасности из краткого описания для использования в приложениях для производства.</span><span class="sxs-lookup"><span data-stu-id="a3be2-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="a3be2-173">Сведения о добавлении ролей, см. в разделе my [развертывание веб-приложения Secure ASP.NET MVC 5 с членством, OAuth и базой данных SQL в Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) руководства.</span><span class="sxs-lookup"><span data-stu-id="a3be2-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="a3be2-174">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a3be2-174">Additional Resources</span></span>

- [<span data-ttu-id="a3be2-175">Развертывание безопасного приложения ASP.NET MVC 5 с членством, OAuth и базой данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="a3be2-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="a3be2-176">[Окиньте конфигурации](http://getglimpse.com/Docs/Configuration) -Doc страницы о настройке вкладок, политику среды выполнения, ведение журнала и многое другое.</span><span class="sxs-lookup"><span data-stu-id="a3be2-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
