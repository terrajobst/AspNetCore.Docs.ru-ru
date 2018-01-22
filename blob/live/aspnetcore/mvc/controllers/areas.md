---
title: "Области"
author: rick-anderson
description: "Показано, как работать с областями."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 666be2da6b38ffb538ae3888ea879a4104c8fd12
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="areas"></a><span data-ttu-id="fcbb0-103">Области</span><span class="sxs-lookup"><span data-stu-id="fcbb0-103">Areas</span></span>

<span data-ttu-id="fcbb0-104">По [Кумар Dhananjay](https://twitter.com/debug_mode) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fcbb0-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fcbb0-105">Области — это функция ASP.NET MVC, используемое для систематизации связанные функциональные возможности в группу в отдельном пространстве имен (для маршрутизации) и структуру папок (для представления).</span><span class="sxs-lookup"><span data-stu-id="fcbb0-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="fcbb0-106">Использование областей создает иерархию с целью маршрутизации путем добавления другого параметра маршрута, `area`в `controller` и `action`.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="fcbb0-107">Области позволяют разделить большой ASP.NET Core MVC, веб-приложения на более мелкие функциональные группы.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="fcbb0-108">Область является структурой MVC, Расположенной внутри приложения.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="fcbb0-109">В проект MVC логические компоненты, например модели, контроллера и представления хранятся в разных папках и MVC использует соглашения об именовании, чтобы создать связь между этими компонентами.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="fcbb0-110">Для большого приложения может быть выгодно для разделения приложения на отдельные области высокий уровень функциональных возможностей.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="fcbb0-111">Например, для электронной коммерции приложение с несколько подразделений, таких как извлечение, выставлении счетов и поиска и т. д. Каждый из этих частей имеют свои собственные логический компонент представления, контроллеры и модели.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="fcbb0-112">В этом сценарии можно использовать области для физически секционирования бизнес-компоненты, в том же проекте.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="fcbb0-113">Область может определяться как более мелкие функциональные единицы в проект ASP.NET Core MVC которых имеет собственный набор контроллеров, представлений и моделей.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="fcbb0-114">Рассмотрите возможность использования областей в MVC проекта при:</span><span class="sxs-lookup"><span data-stu-id="fcbb0-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="fcbb0-115">Приложение состоит из нескольких высокого уровня функциональные компоненты, которые должны быть логически разделены</span><span class="sxs-lookup"><span data-stu-id="fcbb0-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="fcbb0-116">Необходимо секционировать проект MVC, чтобы каждой функциональной области могут обрабатываться независимо друг от друга</span><span class="sxs-lookup"><span data-stu-id="fcbb0-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="fcbb0-117">Область функций:</span><span class="sxs-lookup"><span data-stu-id="fcbb0-117">Area features:</span></span>

* <span data-ttu-id="fcbb0-118">В приложении ASP.NET Core MVC может быть любое количество областей</span><span class="sxs-lookup"><span data-stu-id="fcbb0-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="fcbb0-119">Каждая область имеет свой собственный контроллеры, модели и представления</span><span class="sxs-lookup"><span data-stu-id="fcbb0-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="fcbb0-120">Позволяет организовать больших проектов MVC на несколько высокоуровневые компоненты, которые могут быть выполнены независимо друг от друга</span><span class="sxs-lookup"><span data-stu-id="fcbb0-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="fcbb0-121">Поддерживает несколько контроллеров с тем же именем — при условии, что они имеют разные *областей*</span><span class="sxs-lookup"><span data-stu-id="fcbb0-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="fcbb0-122">Давайте рассмотрим пример, чтобы продемонстрировать, как создаются и используются области.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="fcbb0-123">Предположим, что у вас есть приложение магазина, которое имеет два различных группирований контроллеры и представления: продуктов и служб.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="fcbb0-124">Типичная папка структуру, использование областей MVC выглядит ниже:</span><span class="sxs-lookup"><span data-stu-id="fcbb0-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="fcbb0-125">Имя проекта</span><span class="sxs-lookup"><span data-stu-id="fcbb0-125">Project name</span></span>

  * <span data-ttu-id="fcbb0-126">Области</span><span class="sxs-lookup"><span data-stu-id="fcbb0-126">Areas</span></span>

    * <span data-ttu-id="fcbb0-127">Продукты</span><span class="sxs-lookup"><span data-stu-id="fcbb0-127">Products</span></span>

      * <span data-ttu-id="fcbb0-128">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="fcbb0-128">Controllers</span></span>

        * <span data-ttu-id="fcbb0-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="fcbb0-129">HomeController.cs</span></span>

        * <span data-ttu-id="fcbb0-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="fcbb0-130">ManageController.cs</span></span>

      * <span data-ttu-id="fcbb0-131">Представления</span><span class="sxs-lookup"><span data-stu-id="fcbb0-131">Views</span></span>

        * <span data-ttu-id="fcbb0-132">Главная страница</span><span class="sxs-lookup"><span data-stu-id="fcbb0-132">Home</span></span>

          * <span data-ttu-id="fcbb0-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="fcbb0-133">Index.cshtml</span></span>

        * <span data-ttu-id="fcbb0-134">Управление</span><span class="sxs-lookup"><span data-stu-id="fcbb0-134">Manage</span></span>

          * <span data-ttu-id="fcbb0-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="fcbb0-135">Index.cshtml</span></span>

    * <span data-ttu-id="fcbb0-136">Службы</span><span class="sxs-lookup"><span data-stu-id="fcbb0-136">Services</span></span>

      * <span data-ttu-id="fcbb0-137">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="fcbb0-137">Controllers</span></span>

        * <span data-ttu-id="fcbb0-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="fcbb0-138">HomeController.cs</span></span>

      * <span data-ttu-id="fcbb0-139">Представления</span><span class="sxs-lookup"><span data-stu-id="fcbb0-139">Views</span></span>

        * <span data-ttu-id="fcbb0-140">Главная страница</span><span class="sxs-lookup"><span data-stu-id="fcbb0-140">Home</span></span>

          * <span data-ttu-id="fcbb0-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="fcbb0-141">Index.cshtml</span></span>

<span data-ttu-id="fcbb0-142">Когда MVC пытается визуализации представления в область, по умолчанию, он пытается найти в следующих местах:</span><span class="sxs-lookup"><span data-stu-id="fcbb0-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="fcbb0-143">Это расположение по умолчанию, которые могут быть изменены через `AreaViewLocationFormats` на `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="fcbb0-144">Например, ниже кода, вместо того, как «Области» имя папки, он был изменен на «Категории».</span><span class="sxs-lookup"><span data-stu-id="fcbb0-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="fcbb0-145">Один следует заметить, что структура *представления* папка находится только один, который считается важным здесь и содержимое остальных папок, например *контроллеров* и *моделей* does **не** имеет значения.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="fcbb0-146">Например, не обязаны иметь *контроллеров* и *моделей* папку вообще.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="fcbb0-147">Это происходит потому, что содержимое *контроллеров* и *моделей* — только код, который возвращает компилируется в DLL-библиотеку там, где и как содержимое *представления* не до запроса, представление была предпринята.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* is not until a request to that view has been made.</span></span>

<span data-ttu-id="fcbb0-148">После того как вы определили в иерархии папок, необходимо указать MVC, каждый контроллер, сопоставлена с областями.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="fcbb0-149">Это сделать с помощью оформления имени контроллера с `[Area]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="fcbb0-150">Настройка определения маршрута, который работает с только что созданный областей.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="fcbb0-151">[Маршрутизации к действиям контроллера](routing.md) статье подробно создание определений маршрутов, включая использование стандартных маршрутов и маршрутов с атрибутами.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-151">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="fcbb0-152">В этом примере мы будем использовать обычные маршрута.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="fcbb0-153">Чтобы сделать это, откройте *файла Startup.cs* файл и измените его, добавив `areaRoute` с именем ниже определения маршрута.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="fcbb0-154">Просмотр `http://<yourApp>/products`, `Index` метод действия `HomeController` в `Products` области будет вызываться.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="fcbb0-155">Компоновки</span><span class="sxs-lookup"><span data-stu-id="fcbb0-155">Link Generation</span></span>

* <span data-ttu-id="fcbb0-156">Создание ссылок из действия в пределах одной области на основе контроллера для другого действия в пределах одного контроллера.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="fcbb0-157">Предположим, что аналогично путь текущего запроса`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="fcbb0-158">Синтаксис HtmlHelper:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="fcbb0-159">Синтаксис вспомогательной функции тегов:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="fcbb0-160">Обратите внимание, что мы не должны предоставлять значения «область» и «контроллер» здесь уже доступны в контексте текущего запроса.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-160">Note that we need not supply the 'area' and 'controller' values here as they are already available in the context of the current request.</span></span> <span data-ttu-id="fcbb0-161">Эти типы значений, называются `ambient` значения.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="fcbb0-162">Создание ссылок из действия в пределах одной области на основе контроллера в другое действие на другой контроллер</span><span class="sxs-lookup"><span data-stu-id="fcbb0-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="fcbb0-163">Предположим, что аналогично путь текущего запроса`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="fcbb0-164">Синтаксис HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="fcbb0-165">Синтаксис вспомогательной функции тегов:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="fcbb0-166">Обратите внимание, что здесь используется значение окружения «области», но явно задано значение «controller» выше.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="fcbb0-167">Создание ссылок из действия в пределах одной области контроллера в другое действие на основе другой контроллер и другой области.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="fcbb0-168">Предположим, что аналогично путь текущего запроса`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="fcbb0-169">Синтаксис HtmlHelper:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="fcbb0-170">Синтаксис вспомогательной функции тегов:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="fcbb0-171">Обратите внимание, что здесь используются значения не окружения.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="fcbb0-172">Создание ссылки из действия в зависимости от контроллера в другое действие на другой контроллер и **не** в области.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="fcbb0-173">Синтаксис HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="fcbb0-174">Синтаксис вспомогательной функции тегов:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="fcbb0-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="fcbb0-175">Поскольку мы хотим создать ссылки на не области на основе действия контроллера, мы пустой значение окружения для «область» ниже.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="fcbb0-176">Публикации областей</span><span class="sxs-lookup"><span data-stu-id="fcbb0-176">Publishing Areas</span></span>

<span data-ttu-id="fcbb0-177">Все `*.cshtml` и `wwwroot/**` файлы публикуются на выходных данных, когда `<Project Sdk="Microsoft.NET.Sdk.Web">` включается в *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="fcbb0-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
