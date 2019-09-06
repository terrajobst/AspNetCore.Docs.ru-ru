---
title: ASP.NET Core маршрутизация Блазор
author: guardrex
description: Узнайте, как маршрутизировать запросы в приложениях и о компоненте Навлинк.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: blazor/routing
ms.openlocfilehash: ae3d7ab01185dd6f2e8e0f59b78c2e693fe464b0
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310351"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="bea52-103">ASP.NET Core маршрутизация Блазор</span><span class="sxs-lookup"><span data-stu-id="bea52-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="bea52-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="bea52-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bea52-105">Узнайте, как маршрутизировать запросы и как использовать `NavLink` компонент для создания навигационных ссылок в приложениях блазор.</span><span class="sxs-lookup"><span data-stu-id="bea52-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="bea52-106">Интеграция маршрутизации конечных точек ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bea52-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="bea52-107">Серверная часть блазор интегрирована в [ASP.NET Coreную маршрутизацию конечных точек](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="bea52-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="bea52-108">Приложение ASP.NET Core настроено для приема входящих подключений для интерактивных компонентов с `MapBlazorHub` в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="bea52-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="bea52-109">Шаблоны маршрутов</span><span class="sxs-lookup"><span data-stu-id="bea52-109">Route templates</span></span>

<span data-ttu-id="bea52-110">`Router` Компонент включает маршрутизацию, и для каждого доступного компонента предоставляется шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="bea52-110">The `Router` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="bea52-111">Компонент отображается в файле *app. Razor:* `Router`</span><span class="sxs-lookup"><span data-stu-id="bea52-111">The `Router` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="bea52-112">В приложении Блазор на стороне сервера или на стороне клиента:</span><span class="sxs-lookup"><span data-stu-id="bea52-112">In a Blazor server-side or client-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

<span data-ttu-id="bea52-113">При компиляции файла *. Razor* с `@page` директивой создается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> класс, в котором указан шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="bea52-113">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="bea52-114">Во время выполнения маршрутизатор ищет классы компонентов с `RouteAttribute` помощью и отображает компонент с помощью шаблона маршрута, соответствующего запрошенному URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="bea52-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders the component with a route template that matches the requested URL.</span></span>

<span data-ttu-id="bea52-115">К компоненту можно применить несколько шаблонов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="bea52-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="bea52-116">Следующий компонент отвечает на запросы `/BlazorRoute` и: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="bea52-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="bea52-117">Для правильного разрешения URL-адресов приложение `<base>` должно включать тег в файл *wwwroot/index.HTML* (на стороне клиента блазор) или *pages/_Host. cshtml* (блазор на стороне сервера) с базовым путем к `href` приложению, указанным в атрибуте ( `<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="bea52-117">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="bea52-118">Дополнительные сведения см. в разделе <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="bea52-118">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="bea52-119">Указать пользовательское содержимое, когда содержимое не найдено</span><span class="sxs-lookup"><span data-stu-id="bea52-119">Provide custom content when content isn't found</span></span>

<span data-ttu-id="bea52-120">`Router` Компонент позволяет приложению указать пользовательское содержимое, если содержимое для запрошенного маршрута не найдено.</span><span class="sxs-lookup"><span data-stu-id="bea52-120">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="bea52-121">В файле *app. Razor* задайте пользовательское содержимое в `<NotFound>` параметре `Router` шаблона компонента:</span><span class="sxs-lookup"><span data-stu-id="bea52-121">In the *App.razor* file, set custom content in the `<NotFound>` template parameter of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

<span data-ttu-id="bea52-122">Содержимое `<NotFound>` может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="bea52-122">The content of `<NotFound>` can include arbitrary items, such as other interactive components.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="bea52-123">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="bea52-123">Route parameters</span></span>

<span data-ttu-id="bea52-124">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента с тем же именем (без учета регистра):</span><span class="sxs-lookup"><span data-stu-id="bea52-124">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="bea52-125">Необязательные параметры не поддерживаются для приложений Блазор в предварительной версии ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="bea52-125">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="bea52-126">В `@page` предыдущем примере применяются две директивы.</span><span class="sxs-lookup"><span data-stu-id="bea52-126">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="bea52-127">Первый позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="bea52-127">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="bea52-128">Вторая `@page` директива `{text}` принимает параметр Route и присваивает значение `Text` свойству.</span><span class="sxs-lookup"><span data-stu-id="bea52-128">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="bea52-129">Ограничения маршрута</span><span class="sxs-lookup"><span data-stu-id="bea52-129">Route constraints</span></span>

<span data-ttu-id="bea52-130">Ограничение маршрута применяет сопоставление типов в сегменте маршрута к компоненту.</span><span class="sxs-lookup"><span data-stu-id="bea52-130">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="bea52-131">В следующем примере маршрут к `Users` компоненту соответствует только в том случае, если:</span><span class="sxs-lookup"><span data-stu-id="bea52-131">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="bea52-132">В URL-адресе запроса имеется сегмент маршрута.`Id`</span><span class="sxs-lookup"><span data-stu-id="bea52-132">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="bea52-133">Сегмент является целым числом (`int`). `Id`</span><span class="sxs-lookup"><span data-stu-id="bea52-133">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="bea52-134">Ограничения маршрутов, приведенные в следующей таблице, доступны.</span><span class="sxs-lookup"><span data-stu-id="bea52-134">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="bea52-135">Сведения об ограничениях маршрута, соответствующих инвариантному языку и региональным параметрам, см. в предупреждении ниже таблицы для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="bea52-135">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="bea52-136">Ограничение</span><span class="sxs-lookup"><span data-stu-id="bea52-136">Constraint</span></span> | <span data-ttu-id="bea52-137">Пример</span><span class="sxs-lookup"><span data-stu-id="bea52-137">Example</span></span>           | <span data-ttu-id="bea52-138">Примеры совпадений</span><span class="sxs-lookup"><span data-stu-id="bea52-138">Example Matches</span></span>                                                                  | <span data-ttu-id="bea52-139">Инвариант</span><span class="sxs-lookup"><span data-stu-id="bea52-139">Invariant</span></span><br><span data-ttu-id="bea52-140">язык и региональные параметры</span><span class="sxs-lookup"><span data-stu-id="bea52-140">culture</span></span><br><span data-ttu-id="bea52-141">соответствие</span><span class="sxs-lookup"><span data-stu-id="bea52-141">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="bea52-142">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="bea52-142">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="bea52-143">Нет</span><span class="sxs-lookup"><span data-stu-id="bea52-143">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="bea52-144">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="bea52-144">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="bea52-145">Да</span><span class="sxs-lookup"><span data-stu-id="bea52-145">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="bea52-146">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="bea52-146">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="bea52-147">Да</span><span class="sxs-lookup"><span data-stu-id="bea52-147">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="bea52-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="bea52-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="bea52-149">Да</span><span class="sxs-lookup"><span data-stu-id="bea52-149">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="bea52-150">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="bea52-150">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="bea52-151">Да</span><span class="sxs-lookup"><span data-stu-id="bea52-151">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="bea52-152">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="bea52-152">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="bea52-153">Нет</span><span class="sxs-lookup"><span data-stu-id="bea52-153">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="bea52-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="bea52-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="bea52-155">Да</span><span class="sxs-lookup"><span data-stu-id="bea52-155">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="bea52-156">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="bea52-156">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="bea52-157">Да</span><span class="sxs-lookup"><span data-stu-id="bea52-157">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="bea52-158">Ограничения маршрута, которые проверяют URL-адрес и могут быть преобразованы в тип CLR (например, `int` или `DateTime`), всегда используют инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="bea52-158">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="bea52-159">Эти ограничения предполагают, что URL-адрес является нелокализуемым.</span><span class="sxs-lookup"><span data-stu-id="bea52-159">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="bea52-160">Маршрутизация с URL-адресами, содержащими точки</span><span class="sxs-lookup"><span data-stu-id="bea52-160">Routing with URLs that contain dots</span></span>

<span data-ttu-id="bea52-161">В Блазор приложениях на стороне сервера маршрут по умолчанию в *_Host. cshtml* имеет `/` значение`@page "/"`().</span><span class="sxs-lookup"><span data-stu-id="bea52-161">In Blazor server-side apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="bea52-162">URL-адрес запроса, содержащий точку (`.`), не совпадает с маршрутом по умолчанию, так как URL-адрес является запросом файла.</span><span class="sxs-lookup"><span data-stu-id="bea52-162">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="bea52-163">Приложение Блазор возвращает *404-не найденный* ответ для статического файла, который не существует.</span><span class="sxs-lookup"><span data-stu-id="bea52-163">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="bea52-164">Чтобы использовать маршруты, содержащие точку, настройте *_Host. cshtml* со следующим шаблоном маршрута:</span><span class="sxs-lookup"><span data-stu-id="bea52-164">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="bea52-165">`"/{**path}"` Шаблон включает:</span><span class="sxs-lookup"><span data-stu-id="bea52-165">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="bea52-166">Двойное звездочка *Catch-All* синтаксис (`**`) для записи пути по нескольким границам папок без кодирования косой черты (`/`).</span><span class="sxs-lookup"><span data-stu-id="bea52-166">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="bea52-167">Имя `path` параметра маршрута.</span><span class="sxs-lookup"><span data-stu-id="bea52-167">A `path` route parameter name.</span></span>

<span data-ttu-id="bea52-168">Дополнительные сведения см. в разделе <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="bea52-168">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="bea52-169">Компонент Навлинк</span><span class="sxs-lookup"><span data-stu-id="bea52-169">NavLink component</span></span>

<span data-ttu-id="bea52-170">Используйте компонент вместо элементов HTML-гиперссылок (`<a>`) при создании навигационных ссылок. `NavLink`</span><span class="sxs-lookup"><span data-stu-id="bea52-170">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="bea52-171">Компонент ведет себя `<a>` как элемент, `active` за исключением того, что он переключает класс CSS на основе того, `href` соответствует ли он текущему URL-адресу. `NavLink`</span><span class="sxs-lookup"><span data-stu-id="bea52-171">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="bea52-172">`active` Класс помогает пользователю понять, какая страница является активной страницей между отображаемыми ссылками навигации.</span><span class="sxs-lookup"><span data-stu-id="bea52-172">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="bea52-173">Следующий `NavMenu` компонент создает панель навигации [начальной загрузки](https://getbootstrap.com/docs/) , которая демонстрирует использование `NavLink` компонентов.</span><span class="sxs-lookup"><span data-stu-id="bea52-173">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="bea52-174">`Match` `NavLinkMatch` Атрибут`<NavLink>` элемента можно назначить двумя способами:</span><span class="sxs-lookup"><span data-stu-id="bea52-174">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="bea52-175">`NavLinkMatch.All`&ndash; Параметр`NavLink` активен, если он соответствует всему текущему URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="bea52-175">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="bea52-176">`NavLinkMatch.Prefix`(*по умолчанию*) Параметр активен, `NavLink` если он соответствует любому префиксу текущего URL-адреса. &ndash;</span><span class="sxs-lookup"><span data-stu-id="bea52-176">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="bea52-177">В предыдущем примере домашняя `NavLink` `href=""` страница совпадает с домашним `active` URL-адресом и получает класс CSS только в URL-адресе базового пути по умолчанию `https://localhost:5001/`для приложения (например,).</span><span class="sxs-lookup"><span data-stu-id="bea52-177">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="bea52-178">Второй `NavLink` получает класс, `active` когда пользователь `MyComponent` посещает любой URL-адрес с префиксом (например, `https://localhost:5001/MyComponent` и `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="bea52-178">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="bea52-179">Дополнительные `NavLink` атрибуты компонента передаются в отображаемый тег привязки.</span><span class="sxs-lookup"><span data-stu-id="bea52-179">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="bea52-180">В следующем примере `NavLink` компонент `target` включает атрибут:</span><span class="sxs-lookup"><span data-stu-id="bea52-180">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="bea52-181">Отображается следующая разметка HTML:</span><span class="sxs-lookup"><span data-stu-id="bea52-181">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="bea52-182">URI и вспомогательные функции состояния навигации</span><span class="sxs-lookup"><span data-stu-id="bea52-182">URI and navigation state helpers</span></span>

<span data-ttu-id="bea52-183">Используется `Microsoft.AspNetCore.Components.NavigationManager` для работы с URI и навигацией C# в коде.</span><span class="sxs-lookup"><span data-stu-id="bea52-183">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="bea52-184">`NavigationManager`предоставляет события и методы, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="bea52-184">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="bea52-185">Член</span><span class="sxs-lookup"><span data-stu-id="bea52-185">Member</span></span> | <span data-ttu-id="bea52-186">Описание</span><span class="sxs-lookup"><span data-stu-id="bea52-186">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="bea52-187">Возвращает текущий абсолютный URI.</span><span class="sxs-lookup"><span data-stu-id="bea52-187">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="bea52-188">Получает базовый URI (с завершающей косой чертой), который можно добавить в начало относительных путей URI для получения абсолютного URI.</span><span class="sxs-lookup"><span data-stu-id="bea52-188">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="bea52-189">Как правило `BaseUri` , соответствует `href`атрибуту элемента документа в wwwroot/index.HTML (на стороне клиента блазор) или *pages/_Host. cshtml* (блазор на стороне сервера). `<base>`</span><span class="sxs-lookup"><span data-stu-id="bea52-189">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="bea52-190">Переходит по указанному универсальному коду ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="bea52-190">Navigates to the specified URI.</span></span> <span data-ttu-id="bea52-191">Если `forceLoad` имеет `true`значение:</span><span class="sxs-lookup"><span data-stu-id="bea52-191">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="bea52-192">Маршрутизация на стороне клиента обходится.</span><span class="sxs-lookup"><span data-stu-id="bea52-192">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="bea52-193">Браузер вынужден загрузить новую страницу с сервера, независимо от того, обрабатывается ли универсальный код ресурса клиентским маршрутизатором.</span><span class="sxs-lookup"><span data-stu-id="bea52-193">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="bea52-194">Событие, возникающее при изменении расположения навигации.</span><span class="sxs-lookup"><span data-stu-id="bea52-194">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="bea52-195">Преобразует относительный URI в абсолютный URI.</span><span class="sxs-lookup"><span data-stu-id="bea52-195">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="bea52-196">Учитывая базовый URI (например, URI, ранее возвращенный `GetBaseUri`), преобразует абсолютный URI в универсальный код ресурса (URI) по отношению к базовому префиксу URI.</span><span class="sxs-lookup"><span data-stu-id="bea52-196">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="bea52-197">Следующий компонент переходит к `Counter` компоненту приложения при выборе этой кнопки:</span><span class="sxs-lookup"><span data-stu-id="bea52-197">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```

