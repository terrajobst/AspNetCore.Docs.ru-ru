---
title: Области в ASP.NET Core
author: rick-anderson
description: Сведения о том, что области — это возможность ASP.NET MVC, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833531"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="d564e-103">Области в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d564e-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="d564e-104">Авторы: [Дхананджай Кумар](https://twitter.com/debug_mode) (Dhananjay Kumar) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d564e-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d564e-105">Области — это возможность ASP.NET, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений).</span><span class="sxs-lookup"><span data-stu-id="d564e-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="d564e-106">При использовании областей создается иерархия в целях маршрутизации. Для этого к `controller` и `action` или `page` страницы Razor добавляется еще один параметр маршрута, `area`.</span><span class="sxs-lookup"><span data-stu-id="d564e-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="d564e-107">Области позволяют разделять веб-приложение ASP.NET Core на более мелкие функциональные группы, каждая из которых имеет свой собственный набор Razor Pages, контроллеров, представлений и моделей.</span><span class="sxs-lookup"><span data-stu-id="d564e-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="d564e-108">По сути, область является структурой внутри приложения.</span><span class="sxs-lookup"><span data-stu-id="d564e-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="d564e-109">В веб-проекте ASP.NET Core логические компоненты, например страницы, модель, контроллер и представление, хранятся в разных папках.</span><span class="sxs-lookup"><span data-stu-id="d564e-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="d564e-110">Среда выполнения ASP.NET Core использует соглашения об именовании для создания связи между этими компонентами.</span><span class="sxs-lookup"><span data-stu-id="d564e-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="d564e-111">Крупное приложение может быть целесообразно разделить на отдельные высокоуровневые области функциональности.</span><span class="sxs-lookup"><span data-stu-id="d564e-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="d564e-112">Например, в приложении для электронной коммерции можно выделить несколько подразделений: для оформления заказов, выставления счетов или поиска.</span><span class="sxs-lookup"><span data-stu-id="d564e-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="d564e-113">Каждое из них имеет собственную область для размещения представлений, контроллеров, Razor Pages и моделей.</span><span class="sxs-lookup"><span data-stu-id="d564e-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="d564e-114">Использовать области в проекте рекомендуется в указанных ниже случаях.</span><span class="sxs-lookup"><span data-stu-id="d564e-114">Consider using Areas in an project when:</span></span>

* <span data-ttu-id="d564e-115">Приложение состоит из нескольких высокоуровневых функциональных частей, которые могут быть разделены логически.</span><span class="sxs-lookup"><span data-stu-id="d564e-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="d564e-116">Необходимо разделить приложение так, чтобы с каждой функциональной областью можно было работать отдельно.</span><span class="sxs-lookup"><span data-stu-id="d564e-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="d564e-117">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d564e-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="d564e-118">Пример загрузки содержит простое приложение для тестирования областей.</span><span class="sxs-lookup"><span data-stu-id="d564e-118">The download sample provides a basic app for testing areas.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="d564e-119">Области для контроллеров с представлениями</span><span class="sxs-lookup"><span data-stu-id="d564e-119">Areas for controllers with views</span></span>

<span data-ttu-id="d564e-120">Типичное веб-приложение ASP.NET Core, использующее области, контроллеры и представления, содержит следующие элементы.</span><span class="sxs-lookup"><span data-stu-id="d564e-120">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="d564e-121">[Структура папки области](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="d564e-121">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="d564e-122">Контроллеры с атрибутом [&lbrack;Область&rbrack;](#attribute) для привязки контроллера к области: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="d564e-122">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span></span>
* <span data-ttu-id="d564e-123">[Маршрут к области, добавленный к запуску](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="d564e-123">The [area route added to startup](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span></span>

## <a name="area-folder-structure"></a><span data-ttu-id="d564e-124">Структура папки области</span><span class="sxs-lookup"><span data-stu-id="d564e-124">Area folder structure</span></span>
<span data-ttu-id="d564e-125">Рассмотрим приложение с двумя логическими группами: *товары* и *услуги*.</span><span class="sxs-lookup"><span data-stu-id="d564e-125">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="d564e-126">При использовании областей структура папок будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="d564e-126">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="d564e-127">Имя проекта</span><span class="sxs-lookup"><span data-stu-id="d564e-127">Project name</span></span>
  * <span data-ttu-id="d564e-128">Области</span><span class="sxs-lookup"><span data-stu-id="d564e-128">Areas</span></span>
    * <span data-ttu-id="d564e-129">Продукты</span><span class="sxs-lookup"><span data-stu-id="d564e-129">Products</span></span>
      * <span data-ttu-id="d564e-130">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="d564e-130">Controllers</span></span>
        * <span data-ttu-id="d564e-131">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="d564e-131">HomeController.cs</span></span>
        * <span data-ttu-id="d564e-132">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="d564e-132">ManageController.cs</span></span>
      * <span data-ttu-id="d564e-133">Представления</span><span class="sxs-lookup"><span data-stu-id="d564e-133">Views</span></span>
        * <span data-ttu-id="d564e-134">Главная страница</span><span class="sxs-lookup"><span data-stu-id="d564e-134">Home</span></span>
          * <span data-ttu-id="d564e-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="d564e-135">Index.cshtml</span></span>
        * <span data-ttu-id="d564e-136">Управление</span><span class="sxs-lookup"><span data-stu-id="d564e-136">Manage</span></span>
          * <span data-ttu-id="d564e-137">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="d564e-137">Index.cshtml</span></span>
          * <span data-ttu-id="d564e-138">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="d564e-138">About.cshtml</span></span>
    * <span data-ttu-id="d564e-139">Службы</span><span class="sxs-lookup"><span data-stu-id="d564e-139">Services</span></span>
      * <span data-ttu-id="d564e-140">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="d564e-140">Controllers</span></span>
        * <span data-ttu-id="d564e-141">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="d564e-141">HomeController.cs</span></span>
      * <span data-ttu-id="d564e-142">Представления</span><span class="sxs-lookup"><span data-stu-id="d564e-142">Views</span></span>
        * <span data-ttu-id="d564e-143">Главная страница</span><span class="sxs-lookup"><span data-stu-id="d564e-143">Home</span></span>
          * <span data-ttu-id="d564e-144">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="d564e-144">Index.cshtml</span></span>

<span data-ttu-id="d564e-145">Хотя предыдущий макет часто используется при работе с областями, только файлы представления необходимы для использования этой структуры папок.</span><span class="sxs-lookup"><span data-stu-id="d564e-145">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="d564e-146">Обнаружение подходящего файла представления области происходит в следующем порядке.</span><span class="sxs-lookup"><span data-stu-id="d564e-146">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="d564e-147">Расположение папок не для представления, например для *контроллеров* и *моделей*, **не** рассматривается.</span><span class="sxs-lookup"><span data-stu-id="d564e-147">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="d564e-148">Например, папка *Контроллеры* и *Модели* не требуется.</span><span class="sxs-lookup"><span data-stu-id="d564e-148">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="d564e-149">В папках *Контроллеры* и *Модели* содержится код, который компилируется в DLL-файл.</span><span class="sxs-lookup"><span data-stu-id="d564e-149">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="d564e-150">Содержимое папки *Представления* не компилируется, пока не будет сделан запрос к этому представлению.</span><span class="sxs-lookup"><span data-stu-id="d564e-150">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="d564e-151">Привязка контроллера к области</span><span class="sxs-lookup"><span data-stu-id="d564e-151">Associate the controller with an Area</span></span>

<span data-ttu-id="d564e-152">Контроллеры областей назначаются с помощью атрибута [&lbrack;Область&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute):</span><span class="sxs-lookup"><span data-stu-id="d564e-152">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="d564e-153">Добавление маршрута области</span><span class="sxs-lookup"><span data-stu-id="d564e-153">Add Area route</span></span>

<span data-ttu-id="d564e-154">Маршруты области обычно используют маршрутизацию на основе соглашений, а не атрибутов.</span><span class="sxs-lookup"><span data-stu-id="d564e-154">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="d564e-155">При маршрутизации на основе соглашений учитывается порядок.</span><span class="sxs-lookup"><span data-stu-id="d564e-155">Conventional routing is order-dependent.</span></span> <span data-ttu-id="d564e-156">Как правило, маршруты с областями должны находиться раньше в таблице маршрутов, так как они более точные, чем маршруты без областей.</span><span class="sxs-lookup"><span data-stu-id="d564e-156">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="d564e-157">`{area:...}` можно использовать как токен в шаблонах маршрутов, если пространство URL-адресов одинаково во всех областях:</span><span class="sxs-lookup"><span data-stu-id="d564e-157">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="d564e-158">В приведенном выше коде `exists` применяет ограничение, связанное с тем, что маршрут должен соответствовать области.</span><span class="sxs-lookup"><span data-stu-id="d564e-158">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="d564e-159">Использование `{area:...}` — это наименее сложный механизм для добавления маршрута к областям.</span><span class="sxs-lookup"><span data-stu-id="d564e-159">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="d564e-160">В следующем коде используется <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> для создания двух именованных маршрутов областей:</span><span class="sxs-lookup"><span data-stu-id="d564e-160">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="d564e-161">При использовании `MapAreaRoute` с ASP.NET Core 2.2 см. [эту задачу GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="d564e-161">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="d564e-162">Дополнительные сведения см. в разделе [Маршрутизация области](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="d564e-162">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-areas"></a><span data-ttu-id="d564e-163">Создание ссылки с областями</span><span class="sxs-lookup"><span data-stu-id="d564e-163">Link Generation with Areas</span></span>

<span data-ttu-id="d564e-164">В следующем коде из [примера загрузки](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) показано создание ссылок с указанной областью:</span><span class="sxs-lookup"><span data-stu-id="d564e-164">The following code from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="d564e-165">Ссылки, создаваемые предыдущим кодом, действуют в любом месте приложения.</span><span class="sxs-lookup"><span data-stu-id="d564e-165">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="d564e-166">Пример загрузки включает [частичное представление](xref:mvc/views/partial), которое содержит указанные выше ссылки и те же ссылки без указания области.</span><span class="sxs-lookup"><span data-stu-id="d564e-166">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="d564e-167">Частичное представление указывается в [файле макета](), поэтому каждая страница в приложении отображает созданные ссылки.</span><span class="sxs-lookup"><span data-stu-id="d564e-167">The partial view is referenced in the [layout file](), so every page in the app displays the generated links.</span></span> <span data-ttu-id="d564e-168">Ссылки, создаваемые без указания области, допустимы только при обращении со страницы в одной области и контроллере.</span><span class="sxs-lookup"><span data-stu-id="d564e-168">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="d564e-169">Если область или контроллер не указаны, маршрутизация зависит от значений *окружения*.</span><span class="sxs-lookup"><span data-stu-id="d564e-169">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="d564e-170">Текущие значения маршрута текущего запроса считаются значениями окружения для создания ссылки.</span><span class="sxs-lookup"><span data-stu-id="d564e-170">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="d564e-171">Во многих случаях для примера приложения при использовании значений окружения создаются неверные ссылки.</span><span class="sxs-lookup"><span data-stu-id="d564e-171">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="d564e-172">Дополнительные сведения см. в разделе [Маршрутизация к действиям контроллера](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="d564e-172">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="d564e-173">Общий макет для областей с использованием файла _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="d564e-173">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="d564e-174">Чтобы совместно использовать общий макет для всего приложения, переместите *_ViewStart.cshtml* в корневую папку приложения.</span><span class="sxs-lookup"><span data-stu-id="d564e-174">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="d564e-175">Измените папку области по умолчанию, где хранятся представления</span><span class="sxs-lookup"><span data-stu-id="d564e-175">Change default area folder where views are stored</span></span>

<span data-ttu-id="d564e-176">Следующий код изменяет папку области по умолчанию с `"Areas"` на `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="d564e-176">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a><span data-ttu-id="d564e-177">Публикация областей</span><span class="sxs-lookup"><span data-stu-id="d564e-177">Publishing Areas</span></span>

<span data-ttu-id="d564e-178">Все файлы `*.cshtml` и `wwwroot/**` публикуются в каталоге выходных данных при включении элемента `<Project Sdk="Microsoft.NET.Sdk.Web">` в файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="d564e-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
