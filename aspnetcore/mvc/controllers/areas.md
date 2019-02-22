---
title: Области в ASP.NET Core
author: rick-anderson
description: Сведения о том, что области — это возможность ASP.NET MVC, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений).
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: 19e818fa198936ea1bee0da8039e88a3c0abbf6b
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410616"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="10e67-103">Области в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10e67-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="10e67-104">Авторы: [Дхананджай Кумар](https://twitter.com/debug_mode) (Dhananjay Kumar) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="10e67-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="10e67-105">Области — это возможность ASP.NET MVC, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений).</span><span class="sxs-lookup"><span data-stu-id="10e67-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="10e67-106">При использовании областей создается иерархия в целях маршрутизации. Для этого к `controller` и `action` добавляется еще один параметр маршрута, `area`.</span><span class="sxs-lookup"><span data-stu-id="10e67-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="10e67-107">Области позволяют разделить большое веб-приложение ASP.NET Core MVC на более мелкие функциональные группы.</span><span class="sxs-lookup"><span data-stu-id="10e67-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="10e67-108">По сути, область является структурой MVC внутри приложения.</span><span class="sxs-lookup"><span data-stu-id="10e67-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="10e67-109">В проекте MVC логические компоненты, такие как модель, контроллер и представление, находятся в разных папках, и для создания связи между этими компонентами MVC использует соглашения об именовании.</span><span class="sxs-lookup"><span data-stu-id="10e67-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="10e67-110">Крупное приложение может быть целесообразно разделить на отдельные высокоуровневые области функциональности.</span><span class="sxs-lookup"><span data-stu-id="10e67-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="10e67-111">Например, в приложении для электронной коммерции можно выделить несколько подразделений: для оформления заказов, выставления счетов, поиска и т. д. Каждое из них имеет собственные представления, контроллеры и модели логических компонентов.</span><span class="sxs-lookup"><span data-stu-id="10e67-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="10e67-112">В этом сценарии можно использовать области для физического разделения бизнес-компонентов в рамках одного проекта.</span><span class="sxs-lookup"><span data-stu-id="10e67-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="10e67-113">Область можно определить как небольшой функциональный модуль в проекте ASP.NET Core MVC с собственным набором контроллеров, представлений и моделей.</span><span class="sxs-lookup"><span data-stu-id="10e67-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="10e67-114">Использовать области в проекте MVC рекомендуется в указанных ниже случаях.</span><span class="sxs-lookup"><span data-stu-id="10e67-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="10e67-115">Приложение состоит из нескольких высокоуровневых функциональных частей, которые должны быть разделены логически.</span><span class="sxs-lookup"><span data-stu-id="10e67-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="10e67-116">Необходимо разделить проект MVC так, чтобы с каждой функциональной областью можно было работать отдельно.</span><span class="sxs-lookup"><span data-stu-id="10e67-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="10e67-117">Возможности областей:</span><span class="sxs-lookup"><span data-stu-id="10e67-117">Area features:</span></span>

* <span data-ttu-id="10e67-118">Приложение ASP.NET Core MVC может иметь любое количество областей.</span><span class="sxs-lookup"><span data-stu-id="10e67-118">An ASP.NET Core MVC app can have any number of areas.</span></span>

* <span data-ttu-id="10e67-119">Каждая область имеет собственные контроллеры, модели и представления.</span><span class="sxs-lookup"><span data-stu-id="10e67-119">Each area has its own controllers, models, and views.</span></span>

* <span data-ttu-id="10e67-120">Области позволяют разбивать большие проекты MVC на несколько высокоуровневых компонентов, с которыми можно работать по отдельности.</span><span class="sxs-lookup"><span data-stu-id="10e67-120">Areas allow you to organize large MVC projects into multiple high-level components that can be worked on independently.</span></span>

* <span data-ttu-id="10e67-121">Области поддерживают использование нескольких контроллеров с одинаковыми именами при условии, что у них разные *области*.</span><span class="sxs-lookup"><span data-stu-id="10e67-121">Areas support multiple controllers with the same name, as long as they have different *areas*.</span></span>

<span data-ttu-id="10e67-122">Давайте рассмотрим пример, чтобы наглядно увидеть, как создаются и используются области.</span><span class="sxs-lookup"><span data-stu-id="10e67-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="10e67-123">Допустим, есть приложение магазина, в котором представлены две отдельные группы контроллеров и представлений: Products (Продукты) и Services (Услуги).</span><span class="sxs-lookup"><span data-stu-id="10e67-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="10e67-124">Типичная структура папок при использовании областей MVC будет выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="10e67-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="10e67-125">Имя проекта</span><span class="sxs-lookup"><span data-stu-id="10e67-125">Project name</span></span>
  * <span data-ttu-id="10e67-126">Области</span><span class="sxs-lookup"><span data-stu-id="10e67-126">Areas</span></span>
    * <span data-ttu-id="10e67-127">Продукты</span><span class="sxs-lookup"><span data-stu-id="10e67-127">Products</span></span>
      * <span data-ttu-id="10e67-128">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="10e67-128">Controllers</span></span>
        * <span data-ttu-id="10e67-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="10e67-129">HomeController.cs</span></span>
        * <span data-ttu-id="10e67-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="10e67-130">ManageController.cs</span></span>
      * <span data-ttu-id="10e67-131">Представления</span><span class="sxs-lookup"><span data-stu-id="10e67-131">Views</span></span>
        * <span data-ttu-id="10e67-132">Главная страница</span><span class="sxs-lookup"><span data-stu-id="10e67-132">Home</span></span>
          * <span data-ttu-id="10e67-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="10e67-133">Index.cshtml</span></span>
        * <span data-ttu-id="10e67-134">Управление</span><span class="sxs-lookup"><span data-stu-id="10e67-134">Manage</span></span>
          * <span data-ttu-id="10e67-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="10e67-135">Index.cshtml</span></span>
    * <span data-ttu-id="10e67-136">Службы</span><span class="sxs-lookup"><span data-stu-id="10e67-136">Services</span></span>
      * <span data-ttu-id="10e67-137">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="10e67-137">Controllers</span></span>
        * <span data-ttu-id="10e67-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="10e67-138">HomeController.cs</span></span>
      * <span data-ttu-id="10e67-139">Представления</span><span class="sxs-lookup"><span data-stu-id="10e67-139">Views</span></span>
        * <span data-ttu-id="10e67-140">Главная страница</span><span class="sxs-lookup"><span data-stu-id="10e67-140">Home</span></span>
          * <span data-ttu-id="10e67-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="10e67-141">Index.cshtml</span></span>

<span data-ttu-id="10e67-142">Когда платформа MVC пытается преобразовать представление в области для просмотра, она по умолчанию выполняет поиск в следующих расположениях:</span><span class="sxs-lookup"><span data-stu-id="10e67-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="10e67-143">Это расположения по умолчанию, которые можно изменить с помощью свойства `AreaViewLocationFormats` объекта `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="10e67-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="10e67-144">Например, в приведенном ниже коде папка с именем Areas была переименована в Categories.</span><span class="sxs-lookup"><span data-stu-id="10e67-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="10e67-145">Следует заметить, что в данном случае важность представляет только структура папки *Views*. Содержимое остальных папок, таких как *Controllers* и *Models*, **не** имеет значения.</span><span class="sxs-lookup"><span data-stu-id="10e67-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="10e67-146">Например, папок *Controllers* и *Models* могло бы и не быть.</span><span class="sxs-lookup"><span data-stu-id="10e67-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="10e67-147">Связано это с тем, что содержимое папок *Controllers* и *Models* — это всего лишь код, который компилируется в библиотеку DLL, в то время как содержимое папки *Views* не компилируется, пока не будет выполнен запрос к представлению.</span><span class="sxs-lookup"><span data-stu-id="10e67-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="10e67-148">Определив иерархию папок, необходимо сообщить платформе MVC, что каждый контроллер связан с областью.</span><span class="sxs-lookup"><span data-stu-id="10e67-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="10e67-149">Для этого имя контроллера декорируется атрибутом `[Area]`.</span><span class="sxs-lookup"><span data-stu-id="10e67-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="10e67-150">Создайте определение маршрутов для новых областей.</span><span class="sxs-lookup"><span data-stu-id="10e67-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="10e67-151">В статье [Маршрутизация к действиям контроллера](routing.md) подробно описывается, как создаются определения маршрутов, включая различия в использовании маршрутов на основе соглашений и на основе атрибутов.</span><span class="sxs-lookup"><span data-stu-id="10e67-151">The [Route to controller actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="10e67-152">В этом примере мы используем маршрут на основе соглашения.</span><span class="sxs-lookup"><span data-stu-id="10e67-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="10e67-153">Для этого откройте файл *Startup.cs* и измените его, добавив приведенное ниже определение именованного маршрута `areaRoute`.</span><span class="sxs-lookup"><span data-stu-id="10e67-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="10e67-154">При переходе по адресу `http://<yourApp>/products` будет вызван метод действия `Index` контроллера `HomeController` в области `Products`.</span><span class="sxs-lookup"><span data-stu-id="10e67-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="10e67-155">Создание ссылок</span><span class="sxs-lookup"><span data-stu-id="10e67-155">Link Generation</span></span>

* <span data-ttu-id="10e67-156">Создание ссылок на основе действия из относящегося к области контроллера, которые указывают на другое действие в том же контроллере.</span><span class="sxs-lookup"><span data-stu-id="10e67-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="10e67-157">Допустим, путь текущего запроса имеет вид `/Products/Home/Create`.</span><span class="sxs-lookup"><span data-stu-id="10e67-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="10e67-158">Синтаксис HtmlHelper: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="10e67-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="10e67-159">Синтаксис TagHelper: `<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="10e67-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="10e67-160">Обратите внимание на то, что указывать здесь значения area и controller не нужно, так как они уже доступны в контексте текущего запроса.</span><span class="sxs-lookup"><span data-stu-id="10e67-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="10e67-161">Подобные значения называются значениями `ambient`.</span><span class="sxs-lookup"><span data-stu-id="10e67-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="10e67-162">Создание ссылок на основе действия из относящегося к области контроллера, которые указывают на другое действие в другом контроллере.</span><span class="sxs-lookup"><span data-stu-id="10e67-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="10e67-163">Допустим, путь текущего запроса имеет вид `/Products/Home/Create`.</span><span class="sxs-lookup"><span data-stu-id="10e67-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="10e67-164">Синтаксис HtmlHelper: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="10e67-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="10e67-165">Синтаксис TagHelper: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="10e67-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="10e67-166">Обратите внимание на то, что здесь используется значение окружение для area, но значение controller задано явным образом выше.</span><span class="sxs-lookup"><span data-stu-id="10e67-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="10e67-167">Создание ссылок на основе действия из относящегося к области контроллера, которые указывают на другое действие в другом контроллере и другой области.</span><span class="sxs-lookup"><span data-stu-id="10e67-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="10e67-168">Допустим, путь текущего запроса имеет вид `/Products/Home/Create`.</span><span class="sxs-lookup"><span data-stu-id="10e67-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="10e67-169">Синтаксис HtmlHelper: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="10e67-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="10e67-170">Синтаксис TagHelper: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="10e67-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="10e67-171">Обратите внимание на то, что здесь значения окружения не используются.</span><span class="sxs-lookup"><span data-stu-id="10e67-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="10e67-172">Создание ссылок на основе действия из относящегося к области контроллера, которые указывают на другое действие в другом контроллере, **не** относящемся к области.</span><span class="sxs-lookup"><span data-stu-id="10e67-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="10e67-173">Синтаксис HtmlHelper: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="10e67-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="10e67-174">Синтаксис TagHelper: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="10e67-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="10e67-175">Так как нужно создать ссылки на действие контроллера, не входящего в область, здесь значение окружения для area очищается.</span><span class="sxs-lookup"><span data-stu-id="10e67-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="10e67-176">Публикация областей</span><span class="sxs-lookup"><span data-stu-id="10e67-176">Publishing Areas</span></span>

<span data-ttu-id="10e67-177">Все файлы `*.cshtml` и `wwwroot/**` публикуются в каталоге выходных данных при включении элемента `<Project Sdk="Microsoft.NET.Sdk.Web">` в файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="10e67-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
