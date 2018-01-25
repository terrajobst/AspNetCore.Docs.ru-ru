---
title: "Привязка вспомогательный тег | Документы Microsoft"
author: pkellner
description: "Показано, как работать с вспомогательный тег привязки"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 74609b515936ec7da8bfc133c27cb69f51311924
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="51d67-103">Вспомогательный тег привязки</span><span class="sxs-lookup"><span data-stu-id="51d67-103">Anchor Tag Helper</span></span>

<span data-ttu-id="51d67-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="51d67-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="51d67-105">Вспомогательный тег привязки улучшает привязки HTML (`<a ... ></a>`) тега путем добавления новых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="51d67-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="51d67-106">Ссылки, созданной (на `href` тега) создается с использованием новых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="51d67-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="51d67-107">Этот URL-адрес может включать необязательный протокол, такой как https.</span><span class="sxs-lookup"><span data-stu-id="51d67-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="51d67-108">Динамика контроллер ниже используется в примерах в этом документе.</span><span class="sxs-lookup"><span data-stu-id="51d67-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="51d67-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="51d67-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="51d67-110">Атрибуты вспомогательного тег привязки</span><span class="sxs-lookup"><span data-stu-id="51d67-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="51d67-111">ASP контроллер</span><span class="sxs-lookup"><span data-stu-id="51d67-111">asp-controller</span></span>

<span data-ttu-id="51d67-112">`asp-controller`позволяет связать контроллера, который будет использоваться для создания URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="51d67-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="51d67-113">Контроллеры, указанные должен существовать в текущем проекте.</span><span class="sxs-lookup"><span data-stu-id="51d67-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="51d67-114">В следующем коде перечисляются все динамики:</span><span class="sxs-lookup"><span data-stu-id="51d67-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="51d67-115">Будет создаваемой разметкой:</span><span class="sxs-lookup"><span data-stu-id="51d67-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="51d67-116">Если `asp-controller` указан и `asp-action` отсутствует, значение по умолчанию `asp-action` будет выполняться в данный момент представления метода контроллера по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="51d67-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="51d67-117">Что находится в приведенном выше примере, если `asp-action` опущены, и создается на основе этого вспомогательного объекта тег привязки *HomeController* `Index` представление (**/Home**), созданной разметки будет:</span><span class="sxs-lookup"><span data-stu-id="51d67-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="51d67-118">Действие ASP</span><span class="sxs-lookup"><span data-stu-id="51d67-118">asp-action</span></span>

<span data-ttu-id="51d67-119">`asp-action`Имя метода действия контроллера, которое будет включено в созданный `href`.</span><span class="sxs-lookup"><span data-stu-id="51d67-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="51d67-120">Например, следующий код задать созданный `href` указывают на страницу сведений динамика:</span><span class="sxs-lookup"><span data-stu-id="51d67-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="51d67-121">Будет создаваемой разметкой:</span><span class="sxs-lookup"><span data-stu-id="51d67-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="51d67-122">Если не `asp-controller` атрибут указан, будет использоваться по умолчанию контроллера, вызов представлении при выполнении текущего представления.</span><span class="sxs-lookup"><span data-stu-id="51d67-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="51d67-123">Если атрибут `asp-action` — `Index`, то никакие действия добавляется к URL-адрес, начальные значения по умолчанию `Index` вызываемого метода.</span><span class="sxs-lookup"><span data-stu-id="51d67-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="51d67-124">Действие указанного (или по умолчанию), должен существовать в контроллер, на которые ссылается `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="51d67-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="51d67-125">ASP страницы</span><span class="sxs-lookup"><span data-stu-id="51d67-125">asp-page</span></span>

<span data-ttu-id="51d67-126">Используйте `asp-page` атрибут в тег для задания его URL-адрес, чтобы он указывал на определенную страницу.</span><span class="sxs-lookup"><span data-stu-id="51d67-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="51d67-127">Создает URL-адрес вставляя перед именем страницы с косой черты «/».</span><span class="sxs-lookup"><span data-stu-id="51d67-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="51d67-128">В примере ниже URL-адрес указывает на страницу «Говорящего» в текущем каталоге.</span><span class="sxs-lookup"><span data-stu-id="51d67-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="51d67-129">`asp-page` Атрибута в приведенном выше примере подготавливает к просмотру в формате HTML в представлении, которая похожа на следующий фрагмент кода:</span><span class="sxs-lookup"><span data-stu-id="51d67-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="51d67-130">`asp-page` Атрибута является взаимоисключающим с `asp-route`, `asp-controller`, и `asp-action` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="51d67-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="51d67-131">Тем не менее `asp-page` может использоваться с `asp-route-id` для управления маршрутизацией, как показано в следующем образце кода:</span><span class="sxs-lookup"><span data-stu-id="51d67-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="51d67-132">`asp-route-id` Выводятся следующие данные:</span><span class="sxs-lookup"><span data-stu-id="51d67-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="51d67-133">Для использования `asp-page` атрибут на страницах Razor, URL-адреса должен быть относительным путем, например `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="51d67-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="51d67-134">Относительные пути в `asp-page` атрибута не доступны в представлениях MVC.</span><span class="sxs-lookup"><span data-stu-id="51d67-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="51d67-135">Вместо этого используйте синтаксис «/» для представления MVC.</span><span class="sxs-lookup"><span data-stu-id="51d67-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="51d67-136">ASP - маршрута-{value}</span><span class="sxs-lookup"><span data-stu-id="51d67-136">asp-route-{value}</span></span>

<span data-ttu-id="51d67-137">`asp-route-`является префиксом маршрута подстановочному знаку.</span><span class="sxs-lookup"><span data-stu-id="51d67-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="51d67-138">Любое значение, помещенный после дефис в конце рассматриваются как потенциальные параметра маршрута.</span><span class="sxs-lookup"><span data-stu-id="51d67-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="51d67-139">Если маршрут по умолчанию не найден, этот префикс маршрута будет добавлена к созданный href как параметр запроса и значение.</span><span class="sxs-lookup"><span data-stu-id="51d67-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="51d67-140">В противном случае он будет подставлено в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="51d67-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="51d67-141">При условии, что метод контроллера определены следующим образом:</span><span class="sxs-lookup"><span data-stu-id="51d67-141">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="51d67-142">У шаблона маршрута по умолчанию, определенные в вашей *файла Startup.cs* следующим образом:</span><span class="sxs-lookup"><span data-stu-id="51d67-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="51d67-143">**Cshtml** файл, содержащий вспомогательные тег привязки, необходимых для использования **динамика** параметр модели, переданное из контроллера в представление является следующим образом:</span><span class="sxs-lookup"><span data-stu-id="51d67-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="51d67-144">Затем созданного кода HTML будет следующим, так как **идентификатор** был найден в маршрут по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="51d67-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="51d67-145">Если префикс маршрута не входит в состав маршрутизации найден шаблон, как бывает со следующими **cshtml** файла:</span><span class="sxs-lookup"><span data-stu-id="51d67-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="51d67-146">Затем созданного кода HTML будет следующим, так как **speakerid** не найден в соответствующих маршрута:</span><span class="sxs-lookup"><span data-stu-id="51d67-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="51d67-147">Если параметр `asp-controller` или `asp-action` не указаны, то следует той же обработки по умолчанию, как в `asp-route` атрибута.</span><span class="sxs-lookup"><span data-stu-id="51d67-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="51d67-148">ASP маршрута</span><span class="sxs-lookup"><span data-stu-id="51d67-148">asp-route</span></span>

<span data-ttu-id="51d67-149">`asp-route`предоставляет способ создания URL-адрес, непосредственно именованный маршрут.</span><span class="sxs-lookup"><span data-stu-id="51d67-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="51d67-150">Использование атрибутов маршрутизации маршрут, можно назвать как показано в `SpeakerController` и используется в его `Evaluations` метод.</span><span class="sxs-lookup"><span data-stu-id="51d67-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="51d67-151">`Name = "speakerevals"`Указывает вспомогательный тег привязки для создания маршрута непосредственно к этому методу контроллера, используя URL-адрес `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="51d67-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="51d67-152">Если `asp-controller` или `asp-action` указать в дополнение к `asp-route`, созданный маршрут не может отличаться от ожидаемого.</span><span class="sxs-lookup"><span data-stu-id="51d67-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="51d67-153">`asp-route`не должны использоваться с любой из атрибутов `asp-controller` или `asp-action` во избежание конфликта маршрута.</span><span class="sxs-lookup"><span data-stu-id="51d67-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="51d67-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="51d67-154">asp-all-route-data</span></span>

<span data-ttu-id="51d67-155">`asp-all-route-data`позволяет создавать словарь пары ключ-значение, где ключ является именем параметра, а значение — значение, связанное с этим ключом.</span><span class="sxs-lookup"><span data-stu-id="51d67-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="51d67-156">Как в следующем примере показано создается словарю встроенные и данные передаются в представление razor.</span><span class="sxs-lookup"><span data-stu-id="51d67-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="51d67-157">В качестве альтернативы данных может быть передана с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="51d67-157">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="51d67-158">Приведенный выше код создает следующий URL-адрес: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="51d67-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="51d67-159">При щелчке по ссылке метод контроллера `EvaluationsCurrent` вызывается.</span><span class="sxs-lookup"><span data-stu-id="51d67-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="51d67-160">Он вызывается, поскольку этот контроллер имеет два строковых параметра, которые соответствуют создаются на основе `asp-all-route-data` словаря.</span><span class="sxs-lookup"><span data-stu-id="51d67-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="51d67-161">Если все ключи в словаре совпадают направления параметров, эти значения будут заменены на маршруте соответствующим образом и другие несовпадающие значения будет создан как параметры запроса.</span><span class="sxs-lookup"><span data-stu-id="51d67-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="51d67-162">фрагмент ASP</span><span class="sxs-lookup"><span data-stu-id="51d67-162">asp-fragment</span></span>

<span data-ttu-id="51d67-163">`asp-fragment`Определяет фрагмент URL-адреса для добавления URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="51d67-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="51d67-164">Вспомогательный тег привязки будет добавлен символ решетки (#).</span><span class="sxs-lookup"><span data-stu-id="51d67-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="51d67-165">Если вы создаете тег:</span><span class="sxs-lookup"><span data-stu-id="51d67-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="51d67-166">Созданный URL-адрес будет: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="51d67-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="51d67-167">Хэш-теги полезны при построении приложений на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="51d67-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="51d67-168">Они могут использоваться для простой пометки и поиска на языке JavaScript, например.</span><span class="sxs-lookup"><span data-stu-id="51d67-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="51d67-169">область ASP</span><span class="sxs-lookup"><span data-stu-id="51d67-169">asp-area</span></span>

<span data-ttu-id="51d67-170">`asp-area`Задает имя области, ASP.NET Core используется для задания соответствующего маршрута.</span><span class="sxs-lookup"><span data-stu-id="51d67-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="51d67-171">Ниже приведены примеры как области атрибута вызывает повторное сопоставление маршрутов.</span><span class="sxs-lookup"><span data-stu-id="51d67-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="51d67-172">Установка `asp-area` блогах префиксов каталоге `Areas/Blogs` ко всем маршрутам связанные контроллеры и представления для этот тег привязки.</span><span class="sxs-lookup"><span data-stu-id="51d67-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="51d67-173">Имя проекта</span><span class="sxs-lookup"><span data-stu-id="51d67-173">Project name</span></span>
  * <span data-ttu-id="51d67-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="51d67-174">wwwroot</span></span>
  * <span data-ttu-id="51d67-175">Области</span><span class="sxs-lookup"><span data-stu-id="51d67-175">Areas</span></span>
    * <span data-ttu-id="51d67-176">Блоги</span><span class="sxs-lookup"><span data-stu-id="51d67-176">Blogs</span></span>
      * <span data-ttu-id="51d67-177">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="51d67-177">Controllers</span></span>
        * <span data-ttu-id="51d67-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="51d67-178">HomeController.cs</span></span>
      * <span data-ttu-id="51d67-179">Представления</span><span class="sxs-lookup"><span data-stu-id="51d67-179">Views</span></span>
        * <span data-ttu-id="51d67-180">Главная страница</span><span class="sxs-lookup"><span data-stu-id="51d67-180">Home</span></span>
          * <span data-ttu-id="51d67-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="51d67-181">Index.cshtml</span></span>
          * <span data-ttu-id="51d67-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="51d67-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="51d67-183">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="51d67-183">Controllers</span></span>

<span data-ttu-id="51d67-184">Указание области тег, который является допустимым, такие как ```area="Blogs"``` при ссылке на ```AboutBlog.cshtml``` файла будет выглядеть следующим образом с помощью вспомогательного тег привязки.</span><span class="sxs-lookup"><span data-stu-id="51d67-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="51d67-185">Созданный код HTML будет содержать сегмент областей и будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="51d67-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="51d67-186">Для областей MVC, для работы в веб-приложении шаблон маршрута необходимо включить ссылку в область, если он существует.</span><span class="sxs-lookup"><span data-stu-id="51d67-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="51d67-187">Этого шаблона, который является второй параметр из `routes.MapRoute` вызова метода, будут отображаться как:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="51d67-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="51d67-188">ASP протокол</span><span class="sxs-lookup"><span data-stu-id="51d67-188">asp-protocol</span></span>

<span data-ttu-id="51d67-189">`asp-protocol` Предназначен для указания протокола (например, `https`) в URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="51d67-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="51d67-190">Пример вспомогательного тег привязки, включающий протокол будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="51d67-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="51d67-191">и создают HTML-код следующим образом:</span><span class="sxs-lookup"><span data-stu-id="51d67-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="51d67-192">Домен, в примере используется имя localhost, но использует вспомогательный тег привязки общедоступные веб-сайта, при формировании URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="51d67-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="51d67-193">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="51d67-193">Additional resources</span></span>

* [<span data-ttu-id="51d67-194">Области</span><span class="sxs-lookup"><span data-stu-id="51d67-194">Areas</span></span>](xref:mvc/controllers/areas)
