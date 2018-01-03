---
title: "Привязка вспомогательный тег | Документы Microsoft"
author: pkellner
description: "Показано, как работать с вспомогательный тег привязки"
keywords: "ASP.NET Core, вспомогательная функция тегов"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: b2bdf8b2b297a66b08445d99afbc5f43d2e37ef6
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="ecbb3-104">Вспомогательный тег привязки</span><span class="sxs-lookup"><span data-stu-id="ecbb3-104">Anchor Tag Helper</span></span>

<span data-ttu-id="ecbb3-105">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="ecbb3-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="ecbb3-106">Вспомогательный тег привязки улучшает привязки HTML (`<a ... ></a>`) тега путем добавления новых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="ecbb3-107">Ссылки, созданной (на `href` тега) создается с использованием новых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="ecbb3-108">Этот URL-адрес может включать необязательный протокол, такой как https.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="ecbb3-109">Динамика контроллер ниже используется в примерах в этом документе.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-109">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="ecbb3-110">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="ecbb3-110">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="ecbb3-111">Атрибуты вспомогательного тег привязки</span><span class="sxs-lookup"><span data-stu-id="ecbb3-111">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="ecbb3-112">ASP контроллер</span><span class="sxs-lookup"><span data-stu-id="ecbb3-112">asp-controller</span></span>

<span data-ttu-id="ecbb3-113">`asp-controller`позволяет связать контроллера, который будет использоваться для создания URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="ecbb3-114">Контроллеры, указанные должен существовать в текущем проекте.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="ecbb3-115">В следующем коде перечисляются все динамики:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="ecbb3-116">Будет создаваемой разметкой:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="ecbb3-117">Если `asp-controller` указан и `asp-action` не значение по умолчанию `asp-action` будет выполняться в данный момент представления метода контроллера по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="ecbb3-118">Что находится в приведенном выше примере, если `asp-action` опущены, и создается на основе этого вспомогательного объекта тег привязки *HomeController* `Index` представление (**/Home**), созданной разметки будет:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="ecbb3-119">Действие ASP</span><span class="sxs-lookup"><span data-stu-id="ecbb3-119">asp-action</span></span>

<span data-ttu-id="ecbb3-120">`asp-action`Имя метода действия контроллера, которое будет включено в созданный `href`.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="ecbb3-121">Например, следующий код задать созданный `href` указывают на страницу сведений динамика:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="ecbb3-122">Будет создаваемой разметкой:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="ecbb3-123">Если не `asp-controller` атрибут указан, будет использоваться по умолчанию контроллера, вызов представлении при выполнении текущего представления.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="ecbb3-124">Если атрибут `asp-action` — `Index`, то никакие действия добавляется к URL-адрес, начальные значения по умолчанию `Index` вызываемого метода.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="ecbb3-125">Действие указанного (или по умолчанию), должен существовать в контроллер, на которые ссылается `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="ecbb3-126">ASP страницы</span><span class="sxs-lookup"><span data-stu-id="ecbb3-126">asp-page</span></span>

<span data-ttu-id="ecbb3-127">Используйте `asp-page` атрибут в тег для задания его URL-адрес, чтобы он указывал на определенную страницу.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-127">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="ecbb3-128">Создает URL-адрес вставляя перед именем страницы с косой черты «/».</span><span class="sxs-lookup"><span data-stu-id="ecbb3-128">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="ecbb3-129">В примере ниже URL-адрес указывает на страницу «Говорящего» в текущем каталоге.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-129">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="ecbb3-130">`asp-page` Атрибута в приведенном выше примере подготавливает к просмотру в формате HTML в представлении, которая похожа на следующий фрагмент кода:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-130">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
``

The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes. However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="ecbb3-131">`asp-route-id` Выводятся следующие данные:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-131">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="ecbb3-132">Для использования `asp-page` атрибут на страницах Razor, URL-адреса должен быть относительным путем, например `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-132">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="ecbb3-133">Относительные пути в `asp-page` атрибута не доступны в представлениях MVC.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-133">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="ecbb3-134">Вместо этого используйте синтаксис «/» для представления MVC.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-134">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="ecbb3-135">ASP - маршрута-{value}</span><span class="sxs-lookup"><span data-stu-id="ecbb3-135">asp-route-{value}</span></span>

<span data-ttu-id="ecbb3-136">`asp-route-`является префиксом маршрута подстановочному знаку.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-136">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="ecbb3-137">Любое значение, помещенный после дефис в конце рассматриваются как потенциальные параметра маршрута.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-137">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="ecbb3-138">Если маршрут по умолчанию не найден, этот префикс маршрута будет добавлена к созданный href как параметр запроса и значение.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-138">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="ecbb3-139">В противном случае он будет подставлено в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-139">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="ecbb3-140">При условии, что метод контроллера определены следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-140">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="ecbb3-141">У шаблона маршрута по умолчанию, определенные в вашей *файла Startup.cs* следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-141">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="ecbb3-142">**Cshtml** файл, содержащий вспомогательные тег привязки, необходимых для использования **динамика** параметр модели, переданное из контроллера в представление является следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-142">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="ecbb3-143">Затем созданного кода HTML будет следующим, так как **идентификатор** был найден в маршрут по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-143">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="ecbb3-144">Если префикс маршрута не является частью маршрутизации найден шаблон, как бывает со следующими **cshtml** файла:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-144">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="ecbb3-145">Затем созданного кода HTML будет следующим, так как **speakerid** не найден в соответствующих маршрута:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-145">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="ecbb3-146">Если параметр `asp-controller` или `asp-action` не указаны, то следует той же обработки по умолчанию, как в `asp-route` атрибута.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-146">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="ecbb3-147">ASP маршрута</span><span class="sxs-lookup"><span data-stu-id="ecbb3-147">asp-route</span></span>

<span data-ttu-id="ecbb3-148">`asp-route`предоставляет способ создания URL-адрес, непосредственно именованный маршрут.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-148">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="ecbb3-149">Использование атрибутов маршрутизации маршрут, можно назвать как показано в `SpeakerController` и используется в его `Evaluations` метод.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-149">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="ecbb3-150">`Name = "speakerevals"`Указывает вспомогательный тег привязки для создания маршрута непосредственно к этому методу контроллера, используя URL-адрес `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-150">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="ecbb3-151">Если `asp-controller` или `asp-action` указать в дополнение к `asp-route`, созданный маршрут не может отличаться от ожидаемого.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-151">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="ecbb3-152">`asp-route`не следует использовать с любой из атрибутов `asp-controller` или `asp-action` во избежание конфликта маршрута.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-152">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="ecbb3-153">ASP-all маршрутизации данных</span><span class="sxs-lookup"><span data-stu-id="ecbb3-153">asp-all-route-data</span></span>

<span data-ttu-id="ecbb3-154">`asp-all-route-data`позволяет создавать словарь пары ключ-значение, где ключ является именем параметра, а значение — значение, связанное с этим ключом.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-154">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="ecbb3-155">Как в следующем примере показано создается словарю встроенные и данные передаются в представление razor.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-155">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="ecbb3-156">В качестве альтернативы данных может быть передана с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-156">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="ecbb3-157">Приведенный выше код создает следующий URL-адрес: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="ecbb3-157">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="ecbb3-158">При щелчке по ссылке метод контроллера `EvaluationsCurrent` вызывается.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-158">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="ecbb3-159">Он вызывается, поскольку этот контроллер имеет два строковых параметра, которые соответствуют создаются на основе `asp-all-route-data` словаря.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-159">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="ecbb3-160">Если все ключи в словаре совпадают направления параметров, эти значения будут заменены на маршруте соответствующим образом и другие несовпадающие значения будет создан как параметры запроса.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-160">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="ecbb3-161">фрагмент ASP</span><span class="sxs-lookup"><span data-stu-id="ecbb3-161">asp-fragment</span></span>

<span data-ttu-id="ecbb3-162">`asp-fragment`Определяет фрагмент URL-адреса для добавления URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-162">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="ecbb3-163">Вспомогательный тег привязки будет добавлен символ решетки (#).</span><span class="sxs-lookup"><span data-stu-id="ecbb3-163">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="ecbb3-164">Если вы создаете тег:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-164">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="ecbb3-165">Созданный URL-адрес будет: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="ecbb3-165">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="ecbb3-166">Хэш-теги полезны при построении приложений на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-166">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="ecbb3-167">Они могут использоваться для простой пометки и поиска на языке JavaScript, например.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-167">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="ecbb3-168">область ASP</span><span class="sxs-lookup"><span data-stu-id="ecbb3-168">asp-area</span></span>

<span data-ttu-id="ecbb3-169">`asp-area`Задает имя области, ASP.NET Core используется для задания соответствующего маршрута.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-169">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="ecbb3-170">Ниже приведены примеры как области атрибута вызывает повторное сопоставление маршрутов.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-170">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="ecbb3-171">Установка `asp-area` блогах префиксов каталоге `Areas/Blogs` ко всем маршрутам связанные контроллеры и представления для этот тег привязки.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-171">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="ecbb3-172">Имя проекта</span><span class="sxs-lookup"><span data-stu-id="ecbb3-172">Project name</span></span>
  * <span data-ttu-id="ecbb3-173">wwwroot</span><span class="sxs-lookup"><span data-stu-id="ecbb3-173">wwwroot</span></span>
  * <span data-ttu-id="ecbb3-174">Области</span><span class="sxs-lookup"><span data-stu-id="ecbb3-174">Areas</span></span>
    * <span data-ttu-id="ecbb3-175">Блоги</span><span class="sxs-lookup"><span data-stu-id="ecbb3-175">Blogs</span></span>
      * <span data-ttu-id="ecbb3-176">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="ecbb3-176">Controllers</span></span>
        * <span data-ttu-id="ecbb3-177">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="ecbb3-177">HomeController.cs</span></span>
      * <span data-ttu-id="ecbb3-178">Представления</span><span class="sxs-lookup"><span data-stu-id="ecbb3-178">Views</span></span>
        * <span data-ttu-id="ecbb3-179">Главная страница</span><span class="sxs-lookup"><span data-stu-id="ecbb3-179">Home</span></span>
          * <span data-ttu-id="ecbb3-180">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="ecbb3-180">Index.cshtml</span></span>
          * <span data-ttu-id="ecbb3-181">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="ecbb3-181">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="ecbb3-182">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="ecbb3-182">Controllers</span></span>

<span data-ttu-id="ecbb3-183">Указание области тег, который является допустимым, такие как ```area="Blogs"``` при ссылке на ```AboutBlog.cshtml``` файла будет выглядеть следующим образом с помощью вспомогательного тег привязки.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-183">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="ecbb3-184">Созданный код HTML будет содержать сегмент областей и будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-184">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="ecbb3-185">Для областей MVC, для работы в веб-приложении шаблон маршрута необходимо включить ссылку в область, если он существует.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-185">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="ecbb3-186">Этого шаблона, который является второй параметр из `routes.MapRoute` вызова метода, будут отображаться как:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="ecbb3-186">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="ecbb3-187">ASP протокол</span><span class="sxs-lookup"><span data-stu-id="ecbb3-187">asp-protocol</span></span>

<span data-ttu-id="ecbb3-188">`asp-protocol` Предназначен для указания протокола (например, `https`) в URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-188">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="ecbb3-189">Пример вспомогательного тег привязки, включающий протокол будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-189">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="ecbb3-190">и создают HTML-код следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ecbb3-190">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="ecbb3-191">Домен, в примере используется имя localhost, но использует вспомогательный тег привязки общедоступные веб-сайта, при формировании URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="ecbb3-191">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ecbb3-192">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ecbb3-192">Additional resources</span></span>

* [<span data-ttu-id="ecbb3-193">Области</span><span class="sxs-lookup"><span data-stu-id="ecbb3-193">Areas</span></span>](xref:mvc/controllers/areas)
