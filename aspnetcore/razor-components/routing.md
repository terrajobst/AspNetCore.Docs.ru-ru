---
title: Razor компонентов маршрутизации
author: guardrex
description: Узнайте, как для перенаправления запросов в приложениях, а также сведения о компоненте NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/routing
ms.openlocfilehash: ef82fa7e0d571979a43fd8ce712bf4f22c06f9a7
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515677"
---
# <a name="razor-components-routing"></a><span data-ttu-id="b9bd9-103">Razor компонентов маршрутизации</span><span class="sxs-lookup"><span data-stu-id="b9bd9-103">Razor Components routing</span></span>

<span data-ttu-id="b9bd9-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="b9bd9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b9bd9-105">Узнайте, как для перенаправления запросов в приложениях, а также сведения о компоненте NavLink.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="b9bd9-106">Интеграция маршрутизации ASP.NET Core конечной точки</span><span class="sxs-lookup"><span data-stu-id="b9bd9-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="b9bd9-107">Razor компоненты интегрированы в [маршрутизация ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="b9bd9-107">Razor Components are integrated into [ASP.NET Core routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="b9bd9-108">Приложения ASP.NET Core настроен на прием входящих подключений для интерактивных компонентов Razor с `MapComponentHub<TComponent>` в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-108">An ASP.NET Core app is configured to accept incoming connections for interactive Razor Components with `MapComponentHub<TComponent>` in `Startup.Configure`.</span></span> `MapComponentHub` <span data-ttu-id="b9bd9-109">Указывает, что корневой компонент `App` должны быть помещены в элемент DOM, соответствующий селекторе `app`:</span><span class="sxs-lookup"><span data-stu-id="b9bd9-109">specifies that the root component `App` should be rendered within a DOM element matching the selector `app`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a><span data-ttu-id="b9bd9-110">Шаблоны маршрутов</span><span class="sxs-lookup"><span data-stu-id="b9bd9-110">Route templates</span></span>

<span data-ttu-id="b9bd9-111">`<Router>` Компонент включает маршрутизацию, а шаблон маршрута предоставляется для каждого доступного компонента.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-111">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="b9bd9-112">`<Router>` Компонент появится в *Components/App.razor* файла:</span><span class="sxs-lookup"><span data-stu-id="b9bd9-112">The `<Router>` component appears in the *Components/App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="b9bd9-113">Когда *.razor* или *.cshtml* файл с `@page` компилируется директивы, созданном классе ему присваивается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> указания шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-113">When a *.razor* or *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="b9bd9-114">Во время выполнения, маршрутизатор ищет классы компонентов с `RouteAttribute` и отображает, какой компонент имеет шаблон маршрута, соответствующий запрошенного URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="b9bd9-115">Несколько шаблонов маршрута могут применяться к компоненту.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="b9bd9-116">Следующий компонент отвечает на запросы для `/BlazorRoute` и `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="b9bd9-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` <span data-ttu-id="b9bd9-117">поддерживает настройку резервной компонентом для подготовки к просмотру, если запрошенный маршрут не разрешается.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-117">supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="b9bd9-118">Включить этот сценарий согласиться, задав `FallbackComponent` параметр к типу класса резервный компонента.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-118">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="b9bd9-119">В следующем примере задается компонента, определенного в *Pages/MyFallbackRazorComponent.cshtml* как резервный компонент для `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="b9bd9-119">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="b9bd9-120">Чтобы правильно создать маршруты, приложение должно включать `<base>` тег в его *wwwroot/index.html* файл с основной путь приложения, указанного в `href` атрибут (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="b9bd9-120">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="b9bd9-121">Дополнительные сведения см. в разделе <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-121">For more information, see <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="b9bd9-122">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="b9bd9-122">Route parameters</span></span>

<span data-ttu-id="b9bd9-123">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонент с тем же именем (без учета регистра):</span><span class="sxs-lookup"><span data-stu-id="b9bd9-123">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="b9bd9-124">Необязательные параметры не поддерживаются, так что два `@page` директивы применяются в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="b9bd9-125">Первый позволяет навигации к компоненту без параметров.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="b9bd9-126">Второй `@page` принимает директивы `{text}` параметра маршрута и присваивает это значение `Text` свойства.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="b9bd9-127">Ограничения маршрута</span><span class="sxs-lookup"><span data-stu-id="b9bd9-127">Route constraints</span></span>

<span data-ttu-id="b9bd9-128">Ограничения маршрута обеспечивает тип соответствия сегмента маршрута в компонент.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="b9bd9-129">В следующем примере маршрут к компоненту пользователей соответствует только если:</span><span class="sxs-lookup"><span data-stu-id="b9bd9-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="b9bd9-130">`Id` Сегмента маршрута находится на URL-АДРЕСЕ запроса.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="b9bd9-131">`Id` Сегмента должно быть целым числом (`int`).</span><span class="sxs-lookup"><span data-stu-id="b9bd9-131">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

<span data-ttu-id="b9bd9-132">Ограничения маршрута, показано в следующей таблице, доступны.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-132">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="b9bd9-133">Ограничения маршрута, соответствующие инвариантного языка и региональных параметров см. в разделе Предупреждение приведенной ниже таблице, Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="b9bd9-134">Ограничение</span><span class="sxs-lookup"><span data-stu-id="b9bd9-134">Constraint</span></span> | <span data-ttu-id="b9bd9-135">Пример</span><span class="sxs-lookup"><span data-stu-id="b9bd9-135">Example</span></span>           | <span data-ttu-id="b9bd9-136">Примеры совпадений</span><span class="sxs-lookup"><span data-stu-id="b9bd9-136">Example Matches</span></span>                                                                  | <span data-ttu-id="b9bd9-137">Инвариант</span><span class="sxs-lookup"><span data-stu-id="b9bd9-137">Invariant</span></span><br><span data-ttu-id="b9bd9-138">язык и региональные параметры</span><span class="sxs-lookup"><span data-stu-id="b9bd9-138">culture</span></span><br><span data-ttu-id="b9bd9-139">соответствие</span><span class="sxs-lookup"><span data-stu-id="b9bd9-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`<span data-ttu-id="b9bd9-140">,</span><span class="sxs-lookup"><span data-stu-id="b9bd9-140">,</span></span> `FALSE`                                                                  | <span data-ttu-id="b9bd9-141">Нет</span><span class="sxs-lookup"><span data-stu-id="b9bd9-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`<span data-ttu-id="b9bd9-142">,</span><span class="sxs-lookup"><span data-stu-id="b9bd9-142">,</span></span> `2016-12-31 7:32pm`                                                | <span data-ttu-id="b9bd9-143">Да</span><span class="sxs-lookup"><span data-stu-id="b9bd9-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | `49.99`<span data-ttu-id="b9bd9-144">,</span><span class="sxs-lookup"><span data-stu-id="b9bd9-144">,</span></span> `-1,000.01`                                                             | <span data-ttu-id="b9bd9-145">Да</span><span class="sxs-lookup"><span data-stu-id="b9bd9-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | `1.234`<span data-ttu-id="b9bd9-146">,</span><span class="sxs-lookup"><span data-stu-id="b9bd9-146">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="b9bd9-147">Да</span><span class="sxs-lookup"><span data-stu-id="b9bd9-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | `1.234`<span data-ttu-id="b9bd9-148">,</span><span class="sxs-lookup"><span data-stu-id="b9bd9-148">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="b9bd9-149">Да</span><span class="sxs-lookup"><span data-stu-id="b9bd9-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`<span data-ttu-id="b9bd9-150">,</span><span class="sxs-lookup"><span data-stu-id="b9bd9-150">,</span></span> `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | <span data-ttu-id="b9bd9-151">Нет</span><span class="sxs-lookup"><span data-stu-id="b9bd9-151">No</span></span>                               |
| `int`      | `{id:int}`        | `123456789`<span data-ttu-id="b9bd9-152">,</span><span class="sxs-lookup"><span data-stu-id="b9bd9-152">,</span></span> `-123456789`                                                        | <span data-ttu-id="b9bd9-153">Да</span><span class="sxs-lookup"><span data-stu-id="b9bd9-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | `123456789`<span data-ttu-id="b9bd9-154">,</span><span class="sxs-lookup"><span data-stu-id="b9bd9-154">,</span></span> `-123456789`                                                        | <span data-ttu-id="b9bd9-155">Да</span><span class="sxs-lookup"><span data-stu-id="b9bd9-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="b9bd9-156">Ограничения маршрута, которые проверяют URL-адрес и могут быть преобразованы в тип CLR (например, `int` или `DateTime`), всегда используют инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="b9bd9-157">Эти ограничения предполагают, что URL-адрес является нелокализуемым.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="b9bd9-158">Компонент NavLink</span><span class="sxs-lookup"><span data-stu-id="b9bd9-158">NavLink component</span></span>

<span data-ttu-id="b9bd9-159">Использовать компонент NavLink вместо HTML `<a>` элементы во время создания ссылок навигации.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-159">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="b9bd9-160">Компонент NavLink ведет себя как `<a>` элемента, за исключением того, она переключает `active` класс CSS в зависимости от его `href` соответствует текущий URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-160">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="b9bd9-161">`active` Класс помогает пользователю понять, какая страница является страницей active среди отображаемых ссылок навигации.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="b9bd9-162">Создает следующий компонент NavMenu [Bootstrap](https://getbootstrap.com/docs/) панель навигации, в котором показано, как использовать компоненты NavLink:</span><span class="sxs-lookup"><span data-stu-id="b9bd9-162">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="b9bd9-163">Существует два `NavLinkMatch` параметры:</span><span class="sxs-lookup"><span data-stu-id="b9bd9-163">There are two `NavLinkMatch` options:</span></span>

* `NavLinkMatch.All` <span data-ttu-id="b9bd9-164">&ndash; Указывает, что NavLink должен быть активным, если он соответствует весь текущий URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-164">&ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* `NavLinkMatch.Prefix` <span data-ttu-id="b9bd9-165">&ndash; Указывает, что NavLink должен быть активным, если он соответствует любой префикс, текущий URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-165">&ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="b9bd9-166">В приведенном выше примере Home NavLink (`href=""`) соответствует всем URL-адресам и всегда получает `active` класс CSS.</span><span class="sxs-lookup"><span data-stu-id="b9bd9-166">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="b9bd9-167">Получает только второй NavLink `active` класса, когда пользователь посещает Blazor маршрута компонента (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="b9bd9-167">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
