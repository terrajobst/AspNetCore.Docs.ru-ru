---
title: "Вспомогательная функция тега привязки"
author: pkellner
description: "Обнаруживайте атрибуты вспомогательной функции тега привязки ASP.NET Core и роль, которую играет каждый атрибут в расширении поведения тега привязки HTML."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: f3b704174c3287edda12725b7973a2464e485bac
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="15446-103">Вспомогательная функция тега привязки</span><span class="sxs-lookup"><span data-stu-id="15446-103">Anchor Tag Helper</span></span>

<span data-ttu-id="15446-104">Авторы: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) и [Скотт Эдди](https://github.com/scottaddie) (Scott Addie).</span><span class="sxs-lookup"><span data-stu-id="15446-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="15446-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="15446-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="15446-106">[Вспомогательная функция тега привязки](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) повышает эффективность стандартного тега привязки HTML (`<a ... ></a>`) путем добавления новых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="15446-106">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="15446-107">Как правило, все имена атрибутов начинаются с `asp-`.</span><span class="sxs-lookup"><span data-stu-id="15446-107">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="15446-108">Отображаемое значение атрибута `href` элемента привязки определяется значениями атрибутов `asp-`.</span><span class="sxs-lookup"><span data-stu-id="15446-108">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="15446-109">В примерах в этом документе используется *SpeakerController*</span><span class="sxs-lookup"><span data-stu-id="15446-109">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="15446-110">Ниже перечислены атрибуты `asp-`.</span><span class="sxs-lookup"><span data-stu-id="15446-110">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="15446-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="15446-111">asp-controller</span></span>

<span data-ttu-id="15446-112">Атрибут [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) назначает контроллер, используемый для создания URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="15446-112">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="15446-113">Следующий элемент перечисляет всех говорящих:</span><span class="sxs-lookup"><span data-stu-id="15446-113">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="15446-114">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-114">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="15446-115">Если атрибут `asp-controller` указан, а атрибут `asp-action` — нет, действием контроллера, связанным с выполняющимся представлением, будет заданное по умолчанию значение `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="15446-115">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="15446-116">Если `asp-action` исключается из предыдущего исправления, а в представлении *Index* (*/Home*) *HomeController* используется вспомогательная функция тега привязки, создается следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-116">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="15446-117">asp-action</span><span class="sxs-lookup"><span data-stu-id="15446-117">asp-action</span></span>

<span data-ttu-id="15446-118">Значение атрибута [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) представляет имя действия контроллера, включенное в созданный атрибут `href`.</span><span class="sxs-lookup"><span data-stu-id="15446-118">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="15446-119">Следующий элемент задает созданное значение атрибута `href` на странице динамика оценок:</span><span class="sxs-lookup"><span data-stu-id="15446-119">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="15446-120">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-120">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="15446-121">Если атрибут `asp-controller` не указан, будет использоваться контроллер по умолчанию, вызывающий представление при выполнении текущего представления.</span><span class="sxs-lookup"><span data-stu-id="15446-121">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="15446-122">Если атрибут `asp-action` имеет значение `Index`, действия не добавляются к URL-адресу и вызывается метод `Index` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="15446-122">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="15446-123">В контроллере, на который есть ссылка в `asp-controller`, должно существовать указанное действие (или заданное по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="15446-123">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="15446-124">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="15446-124">asp-route-{value}</span></span>

<span data-ttu-id="15446-125">Атрибут [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) включает подстановочный префикс маршрута.</span><span class="sxs-lookup"><span data-stu-id="15446-125">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="15446-126">Любое значение, подставляемое вместо `{value}`, рассматривается как потенциальный параметр маршрута.</span><span class="sxs-lookup"><span data-stu-id="15446-126">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="15446-127">Если маршрут по умолчанию не найден, этот префикс маршрута будет добавлен к созданному атрибуту `href` в виде параметра запроса и значения.</span><span class="sxs-lookup"><span data-stu-id="15446-127">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="15446-128">В противном случае он будет заменен в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="15446-128">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="15446-129">Рассмотрим следующее действие контроллера:</span><span class="sxs-lookup"><span data-stu-id="15446-129">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="15446-130">С шаблоном маршрута по умолчанию, определенным в *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="15446-130">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="15446-131">Представление MVC использует модель, предоставляемую действием:</span><span class="sxs-lookup"><span data-stu-id="15446-131">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="15446-132">Заполнитель `{id?}` маршрута по умолчанию соответствует требуемому.</span><span class="sxs-lookup"><span data-stu-id="15446-132">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="15446-133">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-133">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="15446-134">Предположим, префикс маршрута не входит в состав соответствующего шаблона маршрутизации, как в случае со следующим представлением MVC:</span><span class="sxs-lookup"><span data-stu-id="15446-134">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="15446-135">Следующий код HTML будет создан, так как элемент `speakerid` не найден в соответствующем маршруте:</span><span class="sxs-lookup"><span data-stu-id="15446-135">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="15446-136">Если атрибут `asp-controller` или `asp-action` не указан, применяется та же обработка по умолчанию, что и в атрибуте `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="15446-136">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="15446-137">asp-route</span><span class="sxs-lookup"><span data-stu-id="15446-137">asp-route</span></span>

<span data-ttu-id="15446-138">Атрибут [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) используется для связывания URL-адреса непосредственно с именованным маршрутом.</span><span class="sxs-lookup"><span data-stu-id="15446-138">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="15446-139">С помощью [атрибутов маршрутизации](xref:mvc/controllers/routing#attribute-routing) маршруту можно присвоить имя, как показано в классе `SpeakerController` и используется в его действии `Evaluations`:</span><span class="sxs-lookup"><span data-stu-id="15446-139">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="15446-140">В следующем элементе атрибут `asp-route` ссылается на именованный маршрут:</span><span class="sxs-lookup"><span data-stu-id="15446-140">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="15446-141">Вспомогательная функция тега привязки создает маршрут непосредственно к этому методу контроллера, используя */Speaker/Evaluations* URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="15446-141">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="15446-142">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-142">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="15446-143">Если наряду с атрибутом `asp-route` указаны атрибут `asp-controller` или `asp-action`, созданный маршрут может отличаться от ожидаемого.</span><span class="sxs-lookup"><span data-stu-id="15446-143">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="15446-144">Во избежание конфликта маршрута `asp-route` нельзя использовать вместе с атрибутами `asp-controller` или `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="15446-144">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="15446-145">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="15446-145">asp-all-route-data</span></span>

<span data-ttu-id="15446-146">Атрибут [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) поддерживает создание словаря пар "ключ-значение".</span><span class="sxs-lookup"><span data-stu-id="15446-146">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="15446-147">Ключ является именем параметра, а значение — значением параметра.</span><span class="sxs-lookup"><span data-stu-id="15446-147">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="15446-148">В следующем примере словарь инициализируется и передается в представление Razor.</span><span class="sxs-lookup"><span data-stu-id="15446-148">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="15446-149">Кроме того, данные могут быть переданы с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="15446-149">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="15446-150">Предыдущий код вызывает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-150">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="15446-151">Словарь `asp-all-route-data` предназначен для создания строки запроса, соответствующей требованиям перегруженного действия `Evaluations`:</span><span class="sxs-lookup"><span data-stu-id="15446-151">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="15446-152">Если ключи в словаре совпадают с параметрами маршрута, эти значения будут соответствующим образом заменены в маршруте.</span><span class="sxs-lookup"><span data-stu-id="15446-152">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="15446-153">В качестве параметров запроса будут созданы другие несовпадающие значения.</span><span class="sxs-lookup"><span data-stu-id="15446-153">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="15446-154">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="15446-154">asp-fragment</span></span>

<span data-ttu-id="15446-155">Атрибут [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) определяет фрагмент URL-адреса, добавляемый к URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="15446-155">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="15446-156">Вспомогательная функция тега привязки добавляет символ решетки (#).</span><span class="sxs-lookup"><span data-stu-id="15446-156">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="15446-157">Рассмотрим следующий элемент:</span><span class="sxs-lookup"><span data-stu-id="15446-157">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="15446-158">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-158">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="15446-159">Хэштеги полезны при создании приложений на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="15446-159">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="15446-160">Например, их можно использовать для простоты пометки и поиска на языке JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15446-160">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="15446-161">asp-area</span><span class="sxs-lookup"><span data-stu-id="15446-161">asp-area</span></span>

<span data-ttu-id="15446-162">Атрибут [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) указывает имя области, используемое платформой для определения соответствующего маршрута.</span><span class="sxs-lookup"><span data-stu-id="15446-162">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="15446-163">Ниже приведен пример того, как атрибут области приводит к повторному сопоставлению маршрутов.</span><span class="sxs-lookup"><span data-stu-id="15446-163">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="15446-164">При установке для атрибута `asp-area` значения Blogs перед маршрутами связанных контроллеров и представлений для этого тега привязки добавляется каталог *Areas/Blogs*.</span><span class="sxs-lookup"><span data-stu-id="15446-164">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="15446-165">**<Имя проекта\>**</span><span class="sxs-lookup"><span data-stu-id="15446-165">**<Project name\>**</span></span>
  * <span data-ttu-id="15446-166">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="15446-166">**wwwroot**</span></span>
  * <span data-ttu-id="15446-167">**Области**</span><span class="sxs-lookup"><span data-stu-id="15446-167">**Areas**</span></span>
    * <span data-ttu-id="15446-168">**Блоги**</span><span class="sxs-lookup"><span data-stu-id="15446-168">**Blogs**</span></span>
      * <span data-ttu-id="15446-169">**Контроллеры**</span><span class="sxs-lookup"><span data-stu-id="15446-169">**Controllers**</span></span>
        * <span data-ttu-id="15446-170">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="15446-170">*HomeController.cs*</span></span>
      * <span data-ttu-id="15446-171">**Представления**</span><span class="sxs-lookup"><span data-stu-id="15446-171">**Views**</span></span>
        * <span data-ttu-id="15446-172">**Корневая папка**</span><span class="sxs-lookup"><span data-stu-id="15446-172">**Home**</span></span>
          * <span data-ttu-id="15446-173">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="15446-173">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="15446-174">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="15446-174">*Index.cshtml*</span></span>
        * <span data-ttu-id="15446-175">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="15446-175">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="15446-176">**Контроллеры**</span><span class="sxs-lookup"><span data-stu-id="15446-176">**Controllers**</span></span>

<span data-ttu-id="15446-177">С учетом предыдущей иерархии каталогов элемент для ссылки на файл *AboutBlog.cshtml* будет таким:</span><span class="sxs-lookup"><span data-stu-id="15446-177">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="15446-178">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-178">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="15446-179">Чтобы в приложении MVC использовались области, в шаблон маршрута необходимо включить ссылку на область, если она существует.</span><span class="sxs-lookup"><span data-stu-id="15446-179">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="15446-180">Этот шаблон представлен вторым параметром вызова метода `routes.MapRoute` в *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="15446-180">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]</span></span>

## <a name="asp-protocol"></a><span data-ttu-id="15446-181">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="15446-181">asp-protocol</span></span>

<span data-ttu-id="15446-182">Атрибут [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) предназначен для указания протокола (например, `https`) в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="15446-182">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="15446-183">Пример:</span><span class="sxs-lookup"><span data-stu-id="15446-183">For example:</span></span>

<span data-ttu-id="15446-184">[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]</span><span class="sxs-lookup"><span data-stu-id="15446-184">[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]</span></span>

<span data-ttu-id="15446-185">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="15446-186">Именем узла в примере является localhost, но при создании URL-адреса вспомогательная функция тега привязки использует общедоступный домен веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="15446-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="15446-187">asp-host</span><span class="sxs-lookup"><span data-stu-id="15446-187">asp-host</span></span>

<span data-ttu-id="15446-188">Атрибут [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) предназначен для определения имени узла в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="15446-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="15446-189">Пример:</span><span class="sxs-lookup"><span data-stu-id="15446-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="15446-190">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="15446-191">asp-page</span><span class="sxs-lookup"><span data-stu-id="15446-191">asp-page</span></span>

<span data-ttu-id="15446-192">Атрибут [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) используется со страницами Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="15446-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="15446-193">Используйте его для определения значения атрибута `href` тега привязки для определенной страницы.</span><span class="sxs-lookup"><span data-stu-id="15446-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="15446-194">Чтобы создать URL-адрес, перед именем страницы следует ввести символ косой черты (/).</span><span class="sxs-lookup"><span data-stu-id="15446-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="15446-195">Следующий пример указывает на страницу Razor Pages участника:</span><span class="sxs-lookup"><span data-stu-id="15446-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="15446-196">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="15446-197">Атрибут `asp-page` является взаимоисключающим с атрибутами `asp-route`, `asp-controller` и `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="15446-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="15446-198">Тем не менее `asp-page` можно использовать с `asp-route-{value}` для управления маршрутизацией, как показано в следующем элементе:</span><span class="sxs-lookup"><span data-stu-id="15446-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="15446-199">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="15446-200">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="15446-200">asp-page-handler</span></span>

<span data-ttu-id="15446-201">Атрибут [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) используется со страницами Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="15446-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="15446-202">Он предназначен для связывания с обработчиками определенных страниц.</span><span class="sxs-lookup"><span data-stu-id="15446-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="15446-203">Рассмотрим следующий обработчик страниц:</span><span class="sxs-lookup"><span data-stu-id="15446-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="15446-204">Связанный элемент модели страницы ссылается на обработчик страниц `OnGetProfile`.</span><span class="sxs-lookup"><span data-stu-id="15446-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="15446-205">Обратите внимание, что префикс `On<Verb>` имени метода обработчика страниц опущен в значении атрибута `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="15446-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="15446-206">В случае с асинхронным методом суффикс `Async` был бы также опущен.</span><span class="sxs-lookup"><span data-stu-id="15446-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="15446-207">Созданный HTML:</span><span class="sxs-lookup"><span data-stu-id="15446-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="15446-208">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="15446-208">Additional resources</span></span>

* [<span data-ttu-id="15446-209">Области</span><span class="sxs-lookup"><span data-stu-id="15446-209">Areas</span></span>](xref:mvc/controllers/areas)
* [<span data-ttu-id="15446-210">Общие сведения о Razor Pages</span><span class="sxs-lookup"><span data-stu-id="15446-210">Intro to Razor Pages</span></span>](xref:mvc/razor-pages/index)
