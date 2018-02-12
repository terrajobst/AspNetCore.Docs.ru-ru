---
title: "Вспомогательная функция тега привязки в ASP.NET Core MVC"
author: pkellner
description: "Сведения о работе со вспомогательной функцией тега привязки"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="250f6-103">Вспомогательная функция тега привязки</span><span class="sxs-lookup"><span data-stu-id="250f6-103">Anchor Tag Helper</span></span>

<span data-ttu-id="250f6-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="250f6-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="250f6-105">Вспомогательная функция тега привязки повышает эффективность тега привязки HTML (`<a ... ></a>`) путем добавления новых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="250f6-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="250f6-106">Ссылка (в теге `href`) создается с использованием новых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="250f6-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="250f6-107">Этот URL-адрес может включать в себя необязательный протокол, такой как https.</span><span class="sxs-lookup"><span data-stu-id="250f6-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="250f6-108">В примерах в этом документе используется контроллер говорящего.</span><span class="sxs-lookup"><span data-stu-id="250f6-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="250f6-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="250f6-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="250f6-110">Атрибуты вспомогательной функция тега привязки</span><span class="sxs-lookup"><span data-stu-id="250f6-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="250f6-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="250f6-111">asp-controller</span></span>

<span data-ttu-id="250f6-112">`asp-controller` используется для связи контроллера, который будет применяться для создания URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="250f6-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="250f6-113">Указанные контроллеры должны существовать в текущем проекте.</span><span class="sxs-lookup"><span data-stu-id="250f6-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="250f6-114">В следующем коде перечисляются все говорящие:</span><span class="sxs-lookup"><span data-stu-id="250f6-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="250f6-115">Созданная разметка будет иметь следующий вид:</span><span class="sxs-lookup"><span data-stu-id="250f6-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="250f6-116">Если атрибут `asp-controller` указан, а атрибут `asp-action` — нет, методом контроллера по умолчанию текущего представления будет заданный по умолчанию атрибут `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="250f6-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="250f6-117">То есть если в приведенном выше примере опускается атрибут `asp-action`, а вспомогательная функция тега привязки создается из представления *HomeController* `Index` (**/Home**), полученная разметка будет иметь следующий вид:</span><span class="sxs-lookup"><span data-stu-id="250f6-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="250f6-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="250f6-118">asp-action</span></span>

<span data-ttu-id="250f6-119">`asp-action` — это имя метода действия в контроллере, которое будет включено в созданный `href`.</span><span class="sxs-lookup"><span data-stu-id="250f6-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="250f6-120">Например, следующий код задает созданный `href` для указания на страницу сведений о говорящем:</span><span class="sxs-lookup"><span data-stu-id="250f6-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="250f6-121">Созданная разметка будет иметь следующий вид:</span><span class="sxs-lookup"><span data-stu-id="250f6-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="250f6-122">Если атрибут `asp-controller` не указан, будет использоваться контроллер по умолчанию, вызывающий представление при выполнении текущего представления.</span><span class="sxs-lookup"><span data-stu-id="250f6-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="250f6-123">Если атрибут `asp-action` имеет значение `Index`, никакое действие не добавляется к URL-адресу и вызывается метод `Index` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="250f6-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="250f6-124">В контроллере, на который есть ссылка в `asp-controller`, должно существовать указанное действие (или заданное по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="250f6-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="250f6-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="250f6-125">asp-page</span></span>

<span data-ttu-id="250f6-126">Атрибут `asp-page` в теге привязки используется для задания его URL-адреса, указывающего на определенную страницу.</span><span class="sxs-lookup"><span data-stu-id="250f6-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="250f6-127">Чтобы создать URL-адрес, перед именем страницы следует ввести символ косой черты "/".</span><span class="sxs-lookup"><span data-stu-id="250f6-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="250f6-128">В примере ниже URL-адрес указывает на страницу "Speaker" (Говорящий) в текущем каталоге.</span><span class="sxs-lookup"><span data-stu-id="250f6-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="250f6-129">Атрибут `asp-page` в приведенном выше примере отображает выходные данные в формате HTML в представлении, аналогичном следующему фрагменту кода:</span><span class="sxs-lookup"><span data-stu-id="250f6-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="250f6-130">Атрибут `asp-page` является взаимоисключающим с атрибутами `asp-route`, `asp-controller` и `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="250f6-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="250f6-131">Однако `asp-page` можно использовать с `asp-route-id` для управления маршрутизацией, как показано в следующем примере кода:</span><span class="sxs-lookup"><span data-stu-id="250f6-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="250f6-132">`asp-route-id` формирует следующие результаты:</span><span class="sxs-lookup"><span data-stu-id="250f6-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="250f6-133">Для использования атрибута `asp-page` на страницах Razor Pages URL-адрес должен быть относительным путем, например `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="250f6-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="250f6-134">Относительные пути в атрибуте `asp-page` недоступны в представлениях MVC.</span><span class="sxs-lookup"><span data-stu-id="250f6-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="250f6-135">Вместо этого в представлениях MVC следует использовать синтаксис "/".</span><span class="sxs-lookup"><span data-stu-id="250f6-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="250f6-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="250f6-136">asp-route-{value}</span></span>

<span data-ttu-id="250f6-137">`asp-route-` является префиксом маршрута.</span><span class="sxs-lookup"><span data-stu-id="250f6-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="250f6-138">Любое значение, помещенное после дефиса в конце, рассматривается как потенциальный параметр маршрута.</span><span class="sxs-lookup"><span data-stu-id="250f6-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="250f6-139">Если маршрут по умолчанию не найден, этот префикс маршрута будет добавлен к созданному href в виде параметра запроса и значения.</span><span class="sxs-lookup"><span data-stu-id="250f6-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="250f6-140">В противном случае он будет заменен в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="250f6-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="250f6-141">Предположим, что у вас есть метод контроллера, определенный следующим образом:</span><span class="sxs-lookup"><span data-stu-id="250f6-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="250f6-142">И есть шаблон маршрута по умолчанию, определенный в файле *Startup.cs* следующим образом:</span><span class="sxs-lookup"><span data-stu-id="250f6-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="250f6-143">Файл **cshtml**, содержащий вспомогательную функцию тега привязки, необходимую для использования параметра модели **speaker**, переданного из контроллера в представление, имеет следующий вид:</span><span class="sxs-lookup"><span data-stu-id="250f6-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="250f6-144">Так как в маршруте по умолчанию был найден **id**, созданный HTML будет иметь следующий вид:</span><span class="sxs-lookup"><span data-stu-id="250f6-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="250f6-145">Если префикс маршрута не входит в состав обнаруженного шаблона маршрутизации, как, например, в случае со следующим файлом **cshtml**:</span><span class="sxs-lookup"><span data-stu-id="250f6-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="250f6-146">Так как в соответствующем маршруте не был найден **speakerid**, созданный HTML будет иметь следующий вид:</span><span class="sxs-lookup"><span data-stu-id="250f6-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="250f6-147">Если атрибут `asp-controller` или `asp-action` не указан, то применяется та же обработка по умолчанию, что и в атрибуте `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="250f6-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="250f6-148">asp-route</span><span class="sxs-lookup"><span data-stu-id="250f6-148">asp-route</span></span>

<span data-ttu-id="250f6-149">`asp-route` позволяет создать URL-адрес, напрямую указывающий на именованный маршрут.</span><span class="sxs-lookup"><span data-stu-id="250f6-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="250f6-150">С помощью атрибутов маршрутизации маршруту можно присвоить имя, как показано в классе `SpeakerController` и используется в его методе `Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="250f6-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="250f6-151">`Name = "speakerevals"` указывает вспомогательной функции тега привязки на необходимость создания маршрута непосредственно к этому методу контроллера с `/Speaker/Evaluations` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="250f6-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="250f6-152">Если наряду с атрибутом `asp-route` указаны атрибут `asp-controller` или `asp-action`, созданный маршрут может отличаться от ожидаемого.</span><span class="sxs-lookup"><span data-stu-id="250f6-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="250f6-153">Во избежание конфликта маршрута атрибут `asp-route` нельзя использовать вместе с атрибутом `asp-controller` или `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="250f6-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="250f6-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="250f6-154">asp-all-route-data</span></span>

<span data-ttu-id="250f6-155">Атрибут `asp-all-route-data` позволяет создавать словарь пар "ключ-значение", где ключ является именем параметра, а значение — значением, связанным с этим ключом.</span><span class="sxs-lookup"><span data-stu-id="250f6-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="250f6-156">Как показано в следующем примере, создается встроенный словарь, и данные передаются в представление Razor.</span><span class="sxs-lookup"><span data-stu-id="250f6-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="250f6-157">Кроме того, данные могут быть переданы с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="250f6-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="250f6-158">Приведенный выше код создает следующий URL-адрес: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="250f6-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="250f6-159">При щелчке ссылки вызывается метод контроллера `EvaluationsCurrent`.</span><span class="sxs-lookup"><span data-stu-id="250f6-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="250f6-160">Это происходит потому, что у этого контроллера есть два строковых параметра, которые соответствуют содержимому, созданному из словаря `asp-all-route-data`.</span><span class="sxs-lookup"><span data-stu-id="250f6-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="250f6-161">Если ключи в словаре совпадают с параметрами маршрута, эти значения будут соответствующим образом заменены в маршруте, а в качестве параметров запроса будут созданы другие несовпадающие значения.</span><span class="sxs-lookup"><span data-stu-id="250f6-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="250f6-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="250f6-162">asp-fragment</span></span>

<span data-ttu-id="250f6-163">`asp-fragment` определяет фрагмент URL-адреса, добавляемый к URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="250f6-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="250f6-164">Вспомогательная функция тега привязки добавит символ решетки (#).</span><span class="sxs-lookup"><span data-stu-id="250f6-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="250f6-165">Если создается тег:</span><span class="sxs-lookup"><span data-stu-id="250f6-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="250f6-166">Созданный URL-адрес будет иметь следующий вид: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="250f6-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="250f6-167">Хэштеги полезны при создании приложений на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="250f6-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="250f6-168">Например, их можно использовать для простоты пометки и поиска на языке JavaScript.</span><span class="sxs-lookup"><span data-stu-id="250f6-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="250f6-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="250f6-169">asp-area</span></span>

<span data-ttu-id="250f6-170">`asp-area` указывает имя области, используемой платформой ASP.NET Core для задания соответствующего маршрута.</span><span class="sxs-lookup"><span data-stu-id="250f6-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="250f6-171">Ниже приведены примеры, демонстрирующие, как атрибут области приводит к повторному сопоставлению маршрутов.</span><span class="sxs-lookup"><span data-stu-id="250f6-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="250f6-172">При установке для атрибута `asp-area` значения Blogs перед маршрутами связанных контроллеров и представлений для этого тега привязки добавляется каталог `Areas/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="250f6-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="250f6-173">Имя проекта</span><span class="sxs-lookup"><span data-stu-id="250f6-173">Project name</span></span>
  * <span data-ttu-id="250f6-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="250f6-174">wwwroot</span></span>
  * <span data-ttu-id="250f6-175">Области</span><span class="sxs-lookup"><span data-stu-id="250f6-175">Areas</span></span>
    * <span data-ttu-id="250f6-176">Блоги</span><span class="sxs-lookup"><span data-stu-id="250f6-176">Blogs</span></span>
      * <span data-ttu-id="250f6-177">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="250f6-177">Controllers</span></span>
        * <span data-ttu-id="250f6-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="250f6-178">HomeController.cs</span></span>
      * <span data-ttu-id="250f6-179">Представления</span><span class="sxs-lookup"><span data-stu-id="250f6-179">Views</span></span>
        * <span data-ttu-id="250f6-180">Главная страница</span><span class="sxs-lookup"><span data-stu-id="250f6-180">Home</span></span>
          * <span data-ttu-id="250f6-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="250f6-181">Index.cshtml</span></span>
          * <span data-ttu-id="250f6-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="250f6-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="250f6-183">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="250f6-183">Controllers</span></span>

<span data-ttu-id="250f6-184">Указание допустимого тега области, например ```area="Blogs"```, при ссылке на файл ```AboutBlog.cshtml``` будет выглядеть следующим образом при использовании вспомогательной функции тега привязки.</span><span class="sxs-lookup"><span data-stu-id="250f6-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="250f6-185">Созданный код HTML будет содержать сегмент областей и иметь следующий вид:</span><span class="sxs-lookup"><span data-stu-id="250f6-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="250f6-186">Чтобы области MVC работали в веб-приложении, в шаблон маршрута необходимо включить ссылку на область, если она существует.</span><span class="sxs-lookup"><span data-stu-id="250f6-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="250f6-187">Этот шаблон, который является вторым параметром вызова метода `routes.MapRoute`, будет иметь следующий вид: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="250f6-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="250f6-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="250f6-188">asp-protocol</span></span>

<span data-ttu-id="250f6-189">Атрибут `asp-protocol` предназначен для указания протокола (например, `https`) в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="250f6-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="250f6-190">Пример вспомогательной функции тега привязки, содержащей протокол, будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="250f6-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="250f6-191">и создаст следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="250f6-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="250f6-192">Доменом в примере является localhost, но при формировании URL-адреса вспомогательная функция тега привязки использует общедоступный домен веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="250f6-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="250f6-193">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="250f6-193">Additional resources</span></span>

* [<span data-ttu-id="250f6-194">Области</span><span class="sxs-lookup"><span data-stu-id="250f6-194">Areas</span></span>](xref:mvc/controllers/areas)
