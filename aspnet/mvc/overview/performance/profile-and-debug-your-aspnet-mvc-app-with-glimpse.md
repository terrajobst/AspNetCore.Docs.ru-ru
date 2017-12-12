---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "Профилирование и отладке приложения ASP.NET MVC с помощью Glimpse | Документы Microsoft"
author: Rick-Anderson
description: "Обзор — бурно и семейству пакетов NuGet открытым исходным кодом, предоставляющий подробные производительности, отладки и диагностических сведений для ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 98b21a54ba00a8c82c3be7ba4e39d44041ed42c6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="8937f-103">Профилирование и отладке приложения ASP.NET MVC с помощью Glimpse</span><span class="sxs-lookup"><span data-stu-id="8937f-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="8937f-104">По [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8937f-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="8937f-105">Обзор — это бурно и семейству пакетов NuGet открытым исходным кодом, предоставляющий подробные производительности, отладки и диагностические сведения для приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8937f-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="8937f-106">Тривиальное для установки, простой и высокоскоростные и отображает ключевые показатели производительности в нижней части каждой страницы.</span><span class="sxs-lookup"><span data-stu-id="8937f-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="8937f-107">Он позволяет углубляться в приложения, если необходимо узнать, что происходит на сервере.</span><span class="sxs-lookup"><span data-stu-id="8937f-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="8937f-108">Обзор предоставляет намного ценную информацию, мы рекомендуем использовать ее в течение всего цикла разработки, включая Azure тестовой среды.</span><span class="sxs-lookup"><span data-stu-id="8937f-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="8937f-109">Во время [Fiddler](http://www.telerik.com/fiddler) и [средства разработки F-12](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx) предоставляют стороны клиента представления, Glimpse предоставляет подробную информацию с сервера.</span><span class="sxs-lookup"><span data-stu-id="8937f-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="8937f-110">Этот учебник основное внимание уделяется с помощью Glimpse ASP.NET MVC и EF пакеты, но доступны другие пакеты.</span><span class="sxs-lookup"><span data-stu-id="8937f-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="8937f-111">По возможности будет связывать к соответствующему [знакомы docs](http://getglimpse.com/Docs/) поддерживать все помогающий.</span><span class="sxs-lookup"><span data-stu-id="8937f-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="8937f-112">Представление является проекта с открытым кодом, слишком можно взаимодействовать с исходным кодом и документы.</span><span class="sxs-lookup"><span data-stu-id="8937f-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="8937f-113">Обзор установки</span><span class="sxs-lookup"><span data-stu-id="8937f-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="8937f-114">Включить обзор для localhost</span><span class="sxs-lookup"><span data-stu-id="8937f-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="8937f-115">Вкладка «временная шкала»</span><span class="sxs-lookup"><span data-stu-id="8937f-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="8937f-116">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="8937f-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="8937f-117">Маршруты</span><span class="sxs-lookup"><span data-stu-id="8937f-117">Routes</span></span>](#route)
- [<span data-ttu-id="8937f-118">С помощью Glimpse в Azure</span><span class="sxs-lookup"><span data-stu-id="8937f-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="8937f-119">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8937f-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="8937f-120">Обзор установки</span><span class="sxs-lookup"><span data-stu-id="8937f-120">Installing Glimpse</span></span>

<span data-ttu-id="8937f-121">Представление можно установить из консоли диспетчера пакетов NuGet или **управление пакетами NuGet** консоли.</span><span class="sxs-lookup"><span data-stu-id="8937f-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="8937f-122">Для этой демонстрации я установлю Mvc5 и EF6 пакеты:</span><span class="sxs-lookup"><span data-stu-id="8937f-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Установка из NuGet Dlg краткого описания](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="8937f-124">Поиск *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="8937f-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF из dlg Установка NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="8937f-126">Выбрав **установленных пакетов**, вы увидите краткого описания зависимые модули, установленные:</span><span class="sxs-lookup"><span data-stu-id="8937f-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Установить пакеты Glimpse из DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="8937f-128">Следующие команды устанавливают MVC5 краткого описания и EF6 модулей с помощью консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="8937f-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="8937f-129">Включить обзор для localhost</span><span class="sxs-lookup"><span data-stu-id="8937f-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="8937f-130">Перейдите к http://localhost:&lt;порт #&gt;/glimpse.axd и нажмите кнопку **Включение краткого описания** кнопки.</span><span class="sxs-lookup"><span data-stu-id="8937f-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the **Turn Glimpse On** button.</span></span>

![Страница axd краткого описания](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="8937f-132">Если отображается панель избранного, можно перетаскивать и drop краткого описания кнопок и добавлять их в виде bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="8937f-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE с boookmarklets краткого описания](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="8937f-134">Теперь можно переходить приложения и **головок копирование отображения** (HUD), показаны в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="8937f-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Страница диспетчера контактов с HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="8937f-136">[HUD краткого описания страницы](http://getglimpse.com/Docs/Heads-up-Display) подробные сведения о времени, показанном выше.</span><span class="sxs-lookup"><span data-stu-id="8937f-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="8937f-137">Отображение данных HUD ненавязчивого производительности может уведомить проблемах немедленно - до перехода цикл тестирования.</span><span class="sxs-lookup"><span data-stu-id="8937f-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="8937f-138">Щелкнув &quot;g&quot; в правом нижнем углу появится в панели «Обзор»:</span><span class="sxs-lookup"><span data-stu-id="8937f-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Обзор панели](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="8937f-140">На рисунке выше [вкладку выполнение](http://getglimpse.com/Docs/Execution-Tab) выбран, который демонстрирует сведений о времени действия и фильтров в конвейере.</span><span class="sxs-lookup"><span data-stu-id="8937f-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="8937f-141">Вы увидите my [остановить Контрольное значение фильтра таймера](http://www.nuget.org/packages/StopWatch/) запустить на этапе 6 конвейера.</span><span class="sxs-lookup"><span data-stu-id="8937f-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="8937f-142">Мой упрощенная таймера можно предоставляют полезные профиля и синхронизации данных, он промахов время, затраченное на авторизации и отображении представления.</span><span class="sxs-lookup"><span data-stu-id="8937f-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="8937f-143">Вы найдете сведения о моем таймера в [профиля и время приложения ASP.NET MVC вплоть до Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="8937f-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="8937f-144">[Вкладки](http://getglimpse.com/Docs/Tabs) страница содержит ссылки на подробные сведения на каждой вкладке.</span><span class="sxs-lookup"><span data-stu-id="8937f-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="8937f-145">Вкладка «временная шкала»</span><span class="sxs-lookup"><span data-stu-id="8937f-145">The Timeline tab</span></span>

<span data-ttu-id="8937f-146">Я изменил Tom Dykstra необработанных [EF 6/MVC 5 учебника](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) с помощью следующего кода будет сделать контроллера преподавателей:</span><span class="sxs-lookup"><span data-stu-id="8937f-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="8937f-147">Приведенный выше код позволяет передать в строке запроса (`eager`) для управления eager или явную загрузку данных.</span><span class="sxs-lookup"><span data-stu-id="8937f-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="8937f-148">На рисунке ниже, используется явная загрузка и время страницы показывает каждый регистрации, загруженных в `Index` метода действия:</span><span class="sxs-lookup"><span data-stu-id="8937f-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![явная загрузка](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="8937f-150">В следующем коде задается eager и регистрации каждой выборке после `Index` представление называется:</span><span class="sxs-lookup"><span data-stu-id="8937f-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![Указанный Eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="8937f-152">Наводите указатель мыши на сегмент времени, чтобы получить подробные сведения о времени:</span><span class="sxs-lookup"><span data-stu-id="8937f-152">You can hover over a time segment to get detailed timing information:</span></span>

![При наведении указателя мыши, чтобы просмотреть подробные сведения о времени](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="8937f-154">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="8937f-154">Model Binding</span></span>

<span data-ttu-id="8937f-155">[Модель вкладку привязки](http://getglimpse.com/Docs/Model-Binding-Tab) предоставляет достаточные сведения помогут вам понять, как привязаны к переменным формы и почему некоторые не привязаны, как и ожидалось.</span><span class="sxs-lookup"><span data-stu-id="8937f-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="8937f-156">На рисунке ниже показана **?**</span><span class="sxs-lookup"><span data-stu-id="8937f-156">The image below shows the **?**</span></span> <span data-ttu-id="8937f-157">значок, который можно щелкнуть для открытия страницы справки краткого описания для данного компонента.</span><span class="sxs-lookup"><span data-stu-id="8937f-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Привязка представление модели краткого описания](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="8937f-159">Маршруты</span><span class="sxs-lookup"><span data-stu-id="8937f-159">Routes</span></span>

 <span data-ttu-id="8937f-160">Вкладка маршруты краткого описания будет может помочь отладки и понимания маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="8937f-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="8937f-161">На приведенном ниже рисунке выбран маршрут продукции (и зеленым, соглашение краткого описания цветом).</span><span class="sxs-lookup"><span data-stu-id="8937f-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="8937f-162">![Выбранное имя продукта](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) также отображаются маркеры ограничения, областей и данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="8937f-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="8937f-163">В разделе [Glimpse маршруты](http://getglimpse.com/Docs/Routes-Tab) и [атрибут маршрутизации в ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="8937f-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="8937f-164">С помощью Glimpse в Azure</span><span class="sxs-lookup"><span data-stu-id="8937f-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="8937f-165">Обзор политики безопасности позволяет только краткого описания данных для отображения из локального узла.</span><span class="sxs-lookup"><span data-stu-id="8937f-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="8937f-166">Эта политика безопасности могут изменяться, поэтому эти данные можно просмотреть на удаленном сервере (например, веб-приложение в Azure).</span><span class="sxs-lookup"><span data-stu-id="8937f-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="8937f-167">Для тестовой среды в Azure, добавьте выделенные метки до нижней части *web.confg* файл, чтобы включить Обзор:</span><span class="sxs-lookup"><span data-stu-id="8937f-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="8937f-168">Только после этого изменения любой пользователь может видеть данные краткого описания на удаленном узле.</span><span class="sxs-lookup"><span data-stu-id="8937f-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="8937f-169">Рассмотрите возможность добавления разметки выше профиль публикации, поэтому она развернута только примененного при использовании этого профиля публикации (например, ваш Azure теста proifle.) Чтобы ограничить представление данных, мы добавим `canViewGlimpseData` роли и только пользователи с этой ролью, для просмотра краткого описания данных.</span><span class="sxs-lookup"><span data-stu-id="8937f-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="8937f-170">Удаление комментариев из *GlimpseSecurityPolicy.cs* и измените [IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) вызов из `Administrator` для `canViewGlimpseData` роли:</span><span class="sxs-lookup"><span data-stu-id="8937f-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="8937f-171">Безопасность – большого объема данных, предоставляемые краткого описания может предоставить доступ к безопасности приложения.</span><span class="sxs-lookup"><span data-stu-id="8937f-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="8937f-172">Корпорация Майкрософт не выполнил аудита безопасности компонента Glimpse для использования в приложениях производства.</span><span class="sxs-lookup"><span data-stu-id="8937f-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="8937f-173">Сведения о добавлении ролей см. в разделе Мои [развернуть веб-приложении Secure ASP.NET MVC 5 с членством, OAuth и базы данных SQL Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) учебника.</span><span class="sxs-lookup"><span data-stu-id="8937f-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="8937f-174">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8937f-174">Additional Resources</span></span>

- [<span data-ttu-id="8937f-175">Развертывание приложения безопасного ASP.NET MVC 5 с членством, OAuth и базы данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="8937f-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="8937f-176">[Знакомы конфигурации](http://getglimpse.com/Docs/Configuration) -Doc страницы о настройке вкладки, политику среды выполнения, ведение журнала и многое другое.</span><span class="sxs-lookup"><span data-stu-id="8937f-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
