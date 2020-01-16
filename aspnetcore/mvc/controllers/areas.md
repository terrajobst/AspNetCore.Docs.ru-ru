---
title: Области в ASP.NET Core
author: rick-anderson
description: Сведения о том, что области — это возможность ASP.NET MVC, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений).
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 1066f4ce104e507abe63302fd3523a3a7a8dfde9
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828247"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="13a95-103">Области в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13a95-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="13a95-104">Авторы: [Дхананджай Кумар](https://twitter.com/debug_mode) (Dhananjay Kumar) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="13a95-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="13a95-105">Области — это возможность ASP.NET, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений).</span><span class="sxs-lookup"><span data-stu-id="13a95-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="13a95-106">При использовании областей создается иерархия в целях маршрутизации. Для этого к `controller` и `action` или `page` страницы Razor добавляется еще один параметр маршрута, `area`.</span><span class="sxs-lookup"><span data-stu-id="13a95-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="13a95-107">Области позволяют разделять веб-приложение ASP.NET Core на более мелкие функциональные группы, каждая из которых имеет свой собственный набор Razor Pages, контроллеров, представлений и моделей.</span><span class="sxs-lookup"><span data-stu-id="13a95-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="13a95-108">По сути, область является структурой внутри приложения.</span><span class="sxs-lookup"><span data-stu-id="13a95-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="13a95-109">В веб-проекте ASP.NET Core логические компоненты, например страницы, модель, контроллер и представление, хранятся в разных папках.</span><span class="sxs-lookup"><span data-stu-id="13a95-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="13a95-110">Среда выполнения ASP.NET Core использует соглашения об именовании для создания связи между этими компонентами.</span><span class="sxs-lookup"><span data-stu-id="13a95-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="13a95-111">Крупное приложение может быть целесообразно разделить на отдельные высокоуровневые области функциональности.</span><span class="sxs-lookup"><span data-stu-id="13a95-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="13a95-112">Например, в приложении для электронной коммерции можно выделить несколько подразделений: для оформления заказов, выставления счетов или поиска.</span><span class="sxs-lookup"><span data-stu-id="13a95-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="13a95-113">Каждое из них имеет собственную область для размещения представлений, контроллеров, Razor Pages и моделей.</span><span class="sxs-lookup"><span data-stu-id="13a95-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="13a95-114">Использовать области в проекте рекомендуется в таких случаях:</span><span class="sxs-lookup"><span data-stu-id="13a95-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="13a95-115">Приложение состоит из нескольких высокоуровневых функциональных частей, которые могут быть разделены логически.</span><span class="sxs-lookup"><span data-stu-id="13a95-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="13a95-116">Необходимо разделить приложение так, чтобы с каждой функциональной областью можно было работать отдельно.</span><span class="sxs-lookup"><span data-stu-id="13a95-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="13a95-117">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="13a95-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="13a95-118">Пример загрузки содержит простое приложение для тестирования областей.</span><span class="sxs-lookup"><span data-stu-id="13a95-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="13a95-119">Если вы используете Razor Pages, см. раздел [Области c Razor Pages](#areas-with-razor-pages) далее в этом документе.</span><span class="sxs-lookup"><span data-stu-id="13a95-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="13a95-120">Области для контроллеров с представлениями</span><span class="sxs-lookup"><span data-stu-id="13a95-120">Areas for controllers with views</span></span>

<span data-ttu-id="13a95-121">Типичное веб-приложение ASP.NET Core, использующее области, контроллеры и представления, содержит следующие элементы.</span><span class="sxs-lookup"><span data-stu-id="13a95-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="13a95-122">[Структура папки области](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="13a95-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="13a95-123">Контроллеры с атрибутом [`[Area]`](#attribute) для привязки контроллера к области:</span><span class="sxs-lookup"><span data-stu-id="13a95-123">Controllers with the [`[Area]`](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="13a95-124">[Маршрут к области, добавленный к запуску](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="13a95-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="13a95-125">Структура папки области</span><span class="sxs-lookup"><span data-stu-id="13a95-125">Area folder structure</span></span>

<span data-ttu-id="13a95-126">Рассмотрим приложение с двумя логическими группами: *товары* и *услуги*.</span><span class="sxs-lookup"><span data-stu-id="13a95-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="13a95-127">При использовании областей структура папок будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="13a95-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="13a95-128">Имя проекта</span><span class="sxs-lookup"><span data-stu-id="13a95-128">Project name</span></span>
  * <span data-ttu-id="13a95-129">Области</span><span class="sxs-lookup"><span data-stu-id="13a95-129">Areas</span></span>
    * <span data-ttu-id="13a95-130">Продукты</span><span class="sxs-lookup"><span data-stu-id="13a95-130">Products</span></span>
      * <span data-ttu-id="13a95-131">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="13a95-131">Controllers</span></span>
        * <span data-ttu-id="13a95-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="13a95-132">HomeController.cs</span></span>
        * <span data-ttu-id="13a95-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="13a95-133">ManageController.cs</span></span>
      * <span data-ttu-id="13a95-134">Представления</span><span class="sxs-lookup"><span data-stu-id="13a95-134">Views</span></span>
        * <span data-ttu-id="13a95-135">Главная страница</span><span class="sxs-lookup"><span data-stu-id="13a95-135">Home</span></span>
          * <span data-ttu-id="13a95-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="13a95-136">Index.cshtml</span></span>
        * <span data-ttu-id="13a95-137">Управление</span><span class="sxs-lookup"><span data-stu-id="13a95-137">Manage</span></span>
          * <span data-ttu-id="13a95-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="13a95-138">Index.cshtml</span></span>
          * <span data-ttu-id="13a95-139">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="13a95-139">About.cshtml</span></span>
    * <span data-ttu-id="13a95-140">Службы</span><span class="sxs-lookup"><span data-stu-id="13a95-140">Services</span></span>
      * <span data-ttu-id="13a95-141">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="13a95-141">Controllers</span></span>
        * <span data-ttu-id="13a95-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="13a95-142">HomeController.cs</span></span>
      * <span data-ttu-id="13a95-143">Представления</span><span class="sxs-lookup"><span data-stu-id="13a95-143">Views</span></span>
        * <span data-ttu-id="13a95-144">Главная страница</span><span class="sxs-lookup"><span data-stu-id="13a95-144">Home</span></span>
          * <span data-ttu-id="13a95-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="13a95-145">Index.cshtml</span></span>

<span data-ttu-id="13a95-146">Хотя предыдущий макет часто используется при работе с областями, только файлы представления необходимы для использования этой структуры папок.</span><span class="sxs-lookup"><span data-stu-id="13a95-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="13a95-147">Обнаружение подходящего файла представления области происходит в следующем порядке.</span><span class="sxs-lookup"><span data-stu-id="13a95-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
```

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="13a95-148">Привязка контроллера к области</span><span class="sxs-lookup"><span data-stu-id="13a95-148">Associate the controller with an Area</span></span>

<span data-ttu-id="13a95-149">Контроллеры областей назначаются с помощью атрибута [&lbrack;Область&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute):</span><span class="sxs-lookup"><span data-stu-id="13a95-149">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="13a95-150">Добавление маршрута области</span><span class="sxs-lookup"><span data-stu-id="13a95-150">Add Area route</span></span>

<span data-ttu-id="13a95-151">Маршруты области обычно используют маршрутизацию на основе соглашений, а не атрибутов.</span><span class="sxs-lookup"><span data-stu-id="13a95-151">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="13a95-152">При маршрутизации на основе соглашений учитывается порядок.</span><span class="sxs-lookup"><span data-stu-id="13a95-152">Conventional routing is order-dependent.</span></span> <span data-ttu-id="13a95-153">Как правило, маршруты с областями должны находиться раньше в таблице маршрутов, так как они более точные, чем маршруты без областей.</span><span class="sxs-lookup"><span data-stu-id="13a95-153">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="13a95-154">`{area:...}` можно использовать как токен в шаблонах маршрутов, если пространство URL-адресов одинаково во всех областях:</span><span class="sxs-lookup"><span data-stu-id="13a95-154">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="13a95-155">В приведенном выше коде `exists` применяет ограничение, связанное с тем, что маршрут должен соответствовать области.</span><span class="sxs-lookup"><span data-stu-id="13a95-155">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="13a95-156">Использование `{area:...}` — это наименее сложный механизм для добавления маршрута к областям.</span><span class="sxs-lookup"><span data-stu-id="13a95-156">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="13a95-157">В следующем коде используется <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> для создания двух именованных маршрутов областей:</span><span class="sxs-lookup"><span data-stu-id="13a95-157">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="13a95-158">При использовании `MapAreaRoute` с ASP.NET Core 2.2 см. [эту задачу GitHub](https://github.com/dotnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="13a95-158">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="13a95-159">Дополнительные сведения см. в разделе [Маршрутизация области](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="13a95-159">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="13a95-160">Создание ссылки с областями MVC</span><span class="sxs-lookup"><span data-stu-id="13a95-160">Link generation with MVC areas</span></span>

<span data-ttu-id="13a95-161">В следующем коде из [примера загрузки](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) показано создание ссылок с указанной областью:</span><span class="sxs-lookup"><span data-stu-id="13a95-161">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="13a95-162">Ссылки, создаваемые предыдущим кодом, действуют в любом месте приложения.</span><span class="sxs-lookup"><span data-stu-id="13a95-162">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="13a95-163">Пример загрузки включает [частичное представление](xref:mvc/views/partial), которое содержит указанные выше ссылки и те же ссылки без указания области.</span><span class="sxs-lookup"><span data-stu-id="13a95-163">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="13a95-164">Частичное представление указывается в [файле макета](xref:mvc/views/layout), поэтому каждая страница в приложении отображает созданные ссылки.</span><span class="sxs-lookup"><span data-stu-id="13a95-164">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="13a95-165">Ссылки, создаваемые без указания области, допустимы только при обращении со страницы в одной области и контроллере.</span><span class="sxs-lookup"><span data-stu-id="13a95-165">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="13a95-166">Если область или контроллер не указаны, маршрутизация зависит от значений *окружения*.</span><span class="sxs-lookup"><span data-stu-id="13a95-166">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="13a95-167">Текущие значения маршрута текущего запроса считаются значениями окружения для создания ссылки.</span><span class="sxs-lookup"><span data-stu-id="13a95-167">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="13a95-168">Во многих случаях для примера приложения при использовании значений окружения создаются неверные ссылки.</span><span class="sxs-lookup"><span data-stu-id="13a95-168">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="13a95-169">Дополнительные сведения см. в разделе [Маршрутизация к действиям контроллера](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="13a95-169">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="13a95-170">Общий макет для областей с использованием файла _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="13a95-170">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="13a95-171">Чтобы совместно использовать общий макет для всего приложения, переместите *_ViewStart.cshtml* в корневую папку приложения.</span><span class="sxs-lookup"><span data-stu-id="13a95-171">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="13a95-172">_ViewImports.cshtml</span><span class="sxs-lookup"><span data-stu-id="13a95-172">_ViewImports.cshtml</span></span>

<span data-ttu-id="13a95-173">В стандартном расположении файл */Views/_ViewImports.cshtml* не применяется к областям.</span><span class="sxs-lookup"><span data-stu-id="13a95-173">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="13a95-174">Чтобы использовать общие объекты [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using` или `@inject` в области, необходимо, чтобы соответствующий файл *_ViewImports.cshtml*[применялся к представлениям области](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="13a95-174">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="13a95-175">Чтобы поведение во всех представлениях было одинаковым, переместите файл */Views/_ViewImports.cshtml* в корневую папку приложения.</span><span class="sxs-lookup"><span data-stu-id="13a95-175">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="13a95-176">Измените папку области по умолчанию, где хранятся представления</span><span class="sxs-lookup"><span data-stu-id="13a95-176">Change default area folder where views are stored</span></span>

<span data-ttu-id="13a95-177">Следующий код изменяет папку области по умолчанию с `"Areas"` на `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="13a95-177">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="13a95-178">Области c Razor Pages</span><span class="sxs-lookup"><span data-stu-id="13a95-178">Areas with Razor Pages</span></span>

<span data-ttu-id="13a95-179">Для создания областей с Razor Pages необходима папка *Areas/<area name>/Pages* в корневом каталоге приложения.</span><span class="sxs-lookup"><span data-stu-id="13a95-179">Areas with Razor Pages require an *Areas/<area name>/Pages* folder in the root of the app.</span></span> <span data-ttu-id="13a95-180">В [этом примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) используется следующая структура папок:</span><span class="sxs-lookup"><span data-stu-id="13a95-180">The following folder structure is used with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="13a95-181">Имя проекта</span><span class="sxs-lookup"><span data-stu-id="13a95-181">Project name</span></span>
  * <span data-ttu-id="13a95-182">Области</span><span class="sxs-lookup"><span data-stu-id="13a95-182">Areas</span></span>
    * <span data-ttu-id="13a95-183">Продукты</span><span class="sxs-lookup"><span data-stu-id="13a95-183">Products</span></span>
      * <span data-ttu-id="13a95-184">Pages</span><span class="sxs-lookup"><span data-stu-id="13a95-184">Pages</span></span>
        * <span data-ttu-id="13a95-185">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="13a95-185">_ViewImports</span></span>
        * <span data-ttu-id="13a95-186">Общие сведения о</span><span class="sxs-lookup"><span data-stu-id="13a95-186">About</span></span>
        * <span data-ttu-id="13a95-187">Индекс</span><span class="sxs-lookup"><span data-stu-id="13a95-187">Index</span></span>
    * <span data-ttu-id="13a95-188">Службы</span><span class="sxs-lookup"><span data-stu-id="13a95-188">Services</span></span>
      * <span data-ttu-id="13a95-189">Pages</span><span class="sxs-lookup"><span data-stu-id="13a95-189">Pages</span></span>
        * <span data-ttu-id="13a95-190">Управление</span><span class="sxs-lookup"><span data-stu-id="13a95-190">Manage</span></span>
          * <span data-ttu-id="13a95-191">Общие сведения о</span><span class="sxs-lookup"><span data-stu-id="13a95-191">About</span></span>
          * <span data-ttu-id="13a95-192">Индекс</span><span class="sxs-lookup"><span data-stu-id="13a95-192">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="13a95-193">Создание ссылки с Razor Pages и областями</span><span class="sxs-lookup"><span data-stu-id="13a95-193">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="13a95-194">В следующем коде из [этого примера](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) показано создание ссылок с указанной областью (например, `asp-area="Products"`).</span><span class="sxs-lookup"><span data-stu-id="13a95-194">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="13a95-195">Ссылки, создаваемые предыдущим кодом, действуют в любом месте приложения.</span><span class="sxs-lookup"><span data-stu-id="13a95-195">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="13a95-196">Пример загрузки включает [частичное представление](xref:mvc/views/partial), которое содержит указанные выше ссылки и те же ссылки без указания области.</span><span class="sxs-lookup"><span data-stu-id="13a95-196">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="13a95-197">Частичное представление указывается в [файле макета](xref:mvc/views/layout), поэтому каждая страница в приложении отображает созданные ссылки.</span><span class="sxs-lookup"><span data-stu-id="13a95-197">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="13a95-198">Ссылки, создаваемые без указания области, допустимы только при обращении со страницы в одной области.</span><span class="sxs-lookup"><span data-stu-id="13a95-198">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="13a95-199">Если область не указана, маршрутизация зависит от значений *окружения*.</span><span class="sxs-lookup"><span data-stu-id="13a95-199">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="13a95-200">Текущие значения маршрута текущего запроса считаются значениями окружения для создания ссылки.</span><span class="sxs-lookup"><span data-stu-id="13a95-200">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="13a95-201">Во многих случаях для примера приложения при использовании значений окружения создаются неверные ссылки.</span><span class="sxs-lookup"><span data-stu-id="13a95-201">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="13a95-202">Для примера рассмотрим ссылки, созданные с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="13a95-202">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="13a95-203">В приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="13a95-203">For the preceding code:</span></span>

* <span data-ttu-id="13a95-204">Ссылка, созданная с помощью `<a asp-page="/Manage/About">` будет правильной, только если последний запрос был к странице в области `Services`.</span><span class="sxs-lookup"><span data-stu-id="13a95-204">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="13a95-205">Например `/Services/Manage/`, `/Services/Manage/Index` или `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="13a95-205">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="13a95-206">Ссылка, созданная с помощью `<a asp-page="/About">` будет правильной, только если последний запрос был к странице в области `/Home`.</span><span class="sxs-lookup"><span data-stu-id="13a95-206">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="13a95-207">Это код из [этого примера](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="13a95-207">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="13a95-208">Импорт пространства имен и вспомогательных функций тегов с помощью файла _ViewImports</span><span class="sxs-lookup"><span data-stu-id="13a95-208">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="13a95-209">Файл *_ViewImports.cshtml* файл можно добавить к каждой области папки *Pages* для импорта пространства имен и вспомогательных функций тегов в каждую папку страницы Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="13a95-209">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="13a95-210">Рассмотрите область *служб* в примере кода, которая не содержит файл *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="13a95-210">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="13a95-211">В следующем примере показана разметка страницы с кодом Razor */Services/Manage/About*.</span><span class="sxs-lookup"><span data-stu-id="13a95-211">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="13a95-212">В этой разметке:</span><span class="sxs-lookup"><span data-stu-id="13a95-212">In the preceding markup:</span></span>

* <span data-ttu-id="13a95-213">Для указания модели (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`) необходимо использовать полное доменное имя.</span><span class="sxs-lookup"><span data-stu-id="13a95-213">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="13a95-214">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) включены с помощью `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="13a95-214">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="13a95-215">В примере загрузка область продуктов содержит следующий файл *_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="13a95-215">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="13a95-216">В следующем примере показана разметка страницы с кодом Razor */Products/About*:</span><span class="sxs-lookup"><span data-stu-id="13a95-216">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="13a95-217">В предыдущем файле пространство имен и директива `@addTagHelper` импортируются в файл с помощью файла *Areas/Products/Pages/_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="13a95-217">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="13a95-218">Дополнительные сведения см. в разделах [Управление областью действия вспомогательной функции тега](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) и [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="13a95-218">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="13a95-219">Общий макет для областей Razor Pages</span><span class="sxs-lookup"><span data-stu-id="13a95-219">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="13a95-220">Чтобы совместно использовать общий макет для всего приложения, переместите *_ViewStart.cshtml* в корневую папку приложения.</span><span class="sxs-lookup"><span data-stu-id="13a95-220">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="13a95-221">Публикация областей</span><span class="sxs-lookup"><span data-stu-id="13a95-221">Publishing Areas</span></span>

<span data-ttu-id="13a95-222">Все файлы CSHTML и файлы в каталоге *wwwroot* публикуются в выходных данных, если в файл CSPROJ включен `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span><span class="sxs-lookup"><span data-stu-id="13a95-222">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
