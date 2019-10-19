---
title: Маршрутизация Blazor ASP.NET Core
author: guardrex
description: Узнайте, как маршрутизировать запросы в приложениях и о компоненте Навлинк.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/routing
ms.openlocfilehash: d4b76c00f79f333884fa7e30b27eadc6e36de287
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589940"
---
# <a name="aspnet-core-opno-locblazor-routing"></a><span data-ttu-id="5b339-103">Маршрутизация Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b339-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="5b339-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="5b339-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="5b339-105">Узнайте, как маршрутизировать запросы и как использовать компонент `NavLink` для создания навигационных ссылок в Blazor приложениях.</span><span class="sxs-lookup"><span data-stu-id="5b339-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="5b339-106">Интеграция маршрутизации конечных точек ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b339-106">ASP.NET Core endpoint routing integration</span></span>

Blazor<span data-ttu-id="5b339-107"> сервер интегрирован в [ASP.NET Coreную маршрутизацию конечных точек](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="5b339-107"> Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="5b339-108">ASP.NET Core приложение настроено для приема входящих подключений для интерактивных компонентов с `MapBlazorHub` в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="5b339-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="5b339-109">Наиболее типичная конфигурация — маршрутизация всех запросов на страницу Razor, которая выступает в качестве узла для серверной части приложения Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="5b339-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="5b339-110">По соглашению страница *узла* обычно называется *_Host. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5b339-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="5b339-111">Маршрут, указанный в файле узла, называется *резервным маршрутом* , так как он работает с низким приоритетом в соответствии с маршрутизацией.</span><span class="sxs-lookup"><span data-stu-id="5b339-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="5b339-112">Резервный маршрут рассматривается, если другие маршруты не совпадают.</span><span class="sxs-lookup"><span data-stu-id="5b339-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="5b339-113">Это позволяет приложению использовать другие контроллеры и страницы, не мешая работе приложения Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="5b339-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="5b339-114">Шаблоны маршрутов</span><span class="sxs-lookup"><span data-stu-id="5b339-114">Route templates</span></span>

<span data-ttu-id="5b339-115">Компонент `Router` обеспечивает маршрутизацию для каждого компонента с указанным маршрутом.</span><span class="sxs-lookup"><span data-stu-id="5b339-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="5b339-116">Компонент `Router` отображается в файле *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="5b339-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="5b339-117">При компиляции файла *. Razor* с директивой `@page` созданный класс предоставляет <xref:Microsoft.AspNetCore.Mvc.RouteAttribute>, задавая шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="5b339-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="5b339-118">В среде выполнения компонент `RouteView`:</span><span class="sxs-lookup"><span data-stu-id="5b339-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="5b339-119">Получает `RouteData` из `Router` вместе с необходимыми параметрами.</span><span class="sxs-lookup"><span data-stu-id="5b339-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="5b339-120">Визуализирует указанный компонент с его макетом (или необязательным макетом по умолчанию), используя указанные параметры.</span><span class="sxs-lookup"><span data-stu-id="5b339-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="5b339-121">При необходимости можно указать параметр `DefaultLayout` с классом макета, который будет использоваться для компонентов, не указывающих макет.</span><span class="sxs-lookup"><span data-stu-id="5b339-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="5b339-122">Шаблоны Blazor по умолчанию определяют компонент `MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="5b339-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="5b339-123">*Маинлайаут. Razor* находится в *общей* папке проекта шаблона.</span><span class="sxs-lookup"><span data-stu-id="5b339-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="5b339-124">Дополнительные сведения о макетах см. в разделе <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="5b339-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="5b339-125">К компоненту можно применить несколько шаблонов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="5b339-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="5b339-126">Следующий компонент отвечает на запросы для `/BlazorRoute` и `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="5b339-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="5b339-127">Для правильного разрешения URL-адресов приложение должно включать тег `<base>` в файл *wwwroot/index.HTML* (Blazor _Host) или *pages/. cshtml* (Blazor Server) с базовым путем к приложению, указанным в атрибуте `href` (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="5b339-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="5b339-128">Для получения дополнительной информации см. <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="5b339-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="5b339-129">Указать пользовательское содержимое, когда содержимое не найдено</span><span class="sxs-lookup"><span data-stu-id="5b339-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="5b339-130">Компонент `Router` позволяет приложению указать пользовательское содержимое, если содержимое для запрошенного маршрута не найдено.</span><span class="sxs-lookup"><span data-stu-id="5b339-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="5b339-131">В файле *app. Razor* задайте пользовательское содержимое в параметре шаблона `NotFound` компонента `Router`:</span><span class="sxs-lookup"><span data-stu-id="5b339-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="5b339-132">Содержимое тегов `<NotFound>` может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="5b339-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="5b339-133">Сведения о применении макета по умолчанию к содержимому `NotFound` см. в разделе <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="5b339-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="5b339-134">Маршрутизация к компонентам из нескольких сборок</span><span class="sxs-lookup"><span data-stu-id="5b339-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="5b339-135">Используйте параметр `AdditionalAssemblies`, чтобы указать дополнительные сборки для компонента `Router`, который следует учитывать при поиске маршрутизируемых компонентов.</span><span class="sxs-lookup"><span data-stu-id="5b339-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="5b339-136">Указанные сборки рассматриваются в дополнение к сборке, указанной `AppAssembly`.</span><span class="sxs-lookup"><span data-stu-id="5b339-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="5b339-137">В следующем примере `Component1` — это маршрутизируемый компонент, определенный в упоминаемой библиотеке классов.</span><span class="sxs-lookup"><span data-stu-id="5b339-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="5b339-138">Следующий пример `AdditionalAssemblies` приводит к поддержке маршрутизации для `Component1`:</span><span class="sxs-lookup"><span data-stu-id="5b339-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="5b339-139">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="5b339-139">Route parameters</span></span>

<span data-ttu-id="5b339-140">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента с тем же именем (без учета регистра):</span><span class="sxs-lookup"><span data-stu-id="5b339-140">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="5b339-141">Необязательные параметры не поддерживаются для Blazor приложений в ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="5b339-141">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0.</span></span> <span data-ttu-id="5b339-142">В предыдущем примере применяются две директивы `@page`.</span><span class="sxs-lookup"><span data-stu-id="5b339-142">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="5b339-143">Первый позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="5b339-143">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="5b339-144">Вторая директива `@page` принимает параметр маршрута `{text}` и присваивает значение свойству `Text`.</span><span class="sxs-lookup"><span data-stu-id="5b339-144">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="5b339-145">Ограничения маршрута</span><span class="sxs-lookup"><span data-stu-id="5b339-145">Route constraints</span></span>

<span data-ttu-id="5b339-146">Ограничение маршрута применяет сопоставление типов в сегменте маршрута к компоненту.</span><span class="sxs-lookup"><span data-stu-id="5b339-146">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="5b339-147">В следующем примере маршрут к компоненту `Users` соответствует только в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="5b339-147">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="5b339-148">В URL-адресе запроса имеется сегмент маршрута `Id`.</span><span class="sxs-lookup"><span data-stu-id="5b339-148">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="5b339-149">Сегмент `Id` является целым числом (`int`).</span><span class="sxs-lookup"><span data-stu-id="5b339-149">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="5b339-150">Ограничения маршрутов, приведенные в следующей таблице, доступны.</span><span class="sxs-lookup"><span data-stu-id="5b339-150">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="5b339-151">Сведения об ограничениях маршрута, соответствующих инвариантному языку и региональным параметрам, см. в предупреждении ниже таблицы для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="5b339-151">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="5b339-152">Ограничение</span><span class="sxs-lookup"><span data-stu-id="5b339-152">Constraint</span></span> | <span data-ttu-id="5b339-153">Пример</span><span class="sxs-lookup"><span data-stu-id="5b339-153">Example</span></span>           | <span data-ttu-id="5b339-154">Примеры совпадений</span><span class="sxs-lookup"><span data-stu-id="5b339-154">Example Matches</span></span>                                                                  | <span data-ttu-id="5b339-155">Инвариант</span><span class="sxs-lookup"><span data-stu-id="5b339-155">Invariant</span></span><br><span data-ttu-id="5b339-156">язык и региональные параметры</span><span class="sxs-lookup"><span data-stu-id="5b339-156">culture</span></span><br><span data-ttu-id="5b339-157">соответствие</span><span class="sxs-lookup"><span data-stu-id="5b339-157">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="5b339-158">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="5b339-158">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="5b339-159">Нет</span><span class="sxs-lookup"><span data-stu-id="5b339-159">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="5b339-160">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="5b339-160">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="5b339-161">Да</span><span class="sxs-lookup"><span data-stu-id="5b339-161">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="5b339-162">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="5b339-162">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="5b339-163">Да</span><span class="sxs-lookup"><span data-stu-id="5b339-163">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="5b339-164">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="5b339-164">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="5b339-165">Да</span><span class="sxs-lookup"><span data-stu-id="5b339-165">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="5b339-166">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="5b339-166">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="5b339-167">Да</span><span class="sxs-lookup"><span data-stu-id="5b339-167">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="5b339-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="5b339-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="5b339-169">Нет</span><span class="sxs-lookup"><span data-stu-id="5b339-169">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="5b339-170">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="5b339-170">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="5b339-171">Да</span><span class="sxs-lookup"><span data-stu-id="5b339-171">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="5b339-172">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="5b339-172">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="5b339-173">Да</span><span class="sxs-lookup"><span data-stu-id="5b339-173">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="5b339-174">Ограничения маршрута, которые проверяют URL-адрес и могут быть преобразованы в тип CLR (например, `int` или `DateTime`), всегда используют инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="5b339-174">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="5b339-175">Эти ограничения предполагают, что URL-адрес является нелокализуемым.</span><span class="sxs-lookup"><span data-stu-id="5b339-175">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="5b339-176">Маршрутизация с URL-адресами, содержащими точки</span><span class="sxs-lookup"><span data-stu-id="5b339-176">Routing with URLs that contain dots</span></span>

<span data-ttu-id="5b339-177">В Blazor серверных приложений маршрут по умолчанию в *_Host. cshtml* — `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="5b339-177">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="5b339-178">URL-адрес запроса, содержащий точку (`.`), не совпадает с маршрутом по умолчанию, так как URL-адрес является запросом файла.</span><span class="sxs-lookup"><span data-stu-id="5b339-178">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="5b339-179">Приложение Blazor возвращает ответ *404-не найдено* для статического файла, который не существует.</span><span class="sxs-lookup"><span data-stu-id="5b339-179">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="5b339-180">Чтобы использовать маршруты, содержащие точку, настройте *_Host. cshtml* со следующим шаблоном маршрута:</span><span class="sxs-lookup"><span data-stu-id="5b339-180">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="5b339-181">Шаблон `"/{**path}"` включает в себя следующее:</span><span class="sxs-lookup"><span data-stu-id="5b339-181">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="5b339-182">Двойное звездочка *-все* синтаксис (`**`) для захвата пути по нескольким папкам без кодирования косой черты (`/`).</span><span class="sxs-lookup"><span data-stu-id="5b339-182">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="5b339-183">Имя параметра маршрута `path`.</span><span class="sxs-lookup"><span data-stu-id="5b339-183">A `path` route parameter name.</span></span>

<span data-ttu-id="5b339-184">Для получения дополнительной информации см. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="5b339-184">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="5b339-185">Компонент Навлинк</span><span class="sxs-lookup"><span data-stu-id="5b339-185">NavLink component</span></span>

<span data-ttu-id="5b339-186">Используйте компонент `NavLink` вместо элементов гиперссылки HTML (`<a>`) при создании навигационных ссылок.</span><span class="sxs-lookup"><span data-stu-id="5b339-186">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="5b339-187">Компонент `NavLink` ведет себя как элемент `<a>`, за исключением того, что он переключает класс CSS `active` в зависимости от того, соответствует ли его `href` текущему URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="5b339-187">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="5b339-188">Класс `active` помогает пользователю понять, какая страница является активной страницей между отображаемыми ссылками навигации.</span><span class="sxs-lookup"><span data-stu-id="5b339-188">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="5b339-189">Следующий компонент `NavMenu` создает панель навигации [начальной загрузки](https://getbootstrap.com/docs/) , которая демонстрирует использование компонентов `NavLink`:</span><span class="sxs-lookup"><span data-stu-id="5b339-189">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="5b339-190">Существует два параметра `NavLinkMatch`, которые можно назначить атрибуту `Match` элемента `<NavLink>`:</span><span class="sxs-lookup"><span data-stu-id="5b339-190">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="5b339-191">`NavLinkMatch.All` &ndash; `NavLink` активна, если он соответствует всему текущему URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="5b339-191">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="5b339-192">`NavLinkMatch.Prefix` (*по умолчанию*) &ndash; активна `NavLink`, если он соответствует любому префиксу текущего URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="5b339-192">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="5b339-193">В предыдущем примере домашняя `NavLink` `href=""` соответствует домашнему URL-адресу и получает класс CSS `active` в URL-адресе базового пути приложения по умолчанию (например, `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="5b339-193">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="5b339-194">Второй `NavLink` получает класс `active`, когда пользователь посещает любой URL-адрес с префиксом `MyComponent` (например, `https://localhost:5001/MyComponent` и `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="5b339-194">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="5b339-195">Дополнительные атрибуты компонента `NavLink` передаются в отображаемый тег привязки.</span><span class="sxs-lookup"><span data-stu-id="5b339-195">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="5b339-196">В следующем примере компонент `NavLink` включает атрибут `target`:</span><span class="sxs-lookup"><span data-stu-id="5b339-196">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="5b339-197">Отображается следующая разметка HTML:</span><span class="sxs-lookup"><span data-stu-id="5b339-197">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="5b339-198">URI и вспомогательные функции состояния навигации</span><span class="sxs-lookup"><span data-stu-id="5b339-198">URI and navigation state helpers</span></span>

<span data-ttu-id="5b339-199">Используйте `Microsoft.AspNetCore.Components.NavigationManager` для работы с URI и навигацией C# в коде.</span><span class="sxs-lookup"><span data-stu-id="5b339-199">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="5b339-200">`NavigationManager` предоставляет событие и методы, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5b339-200">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="5b339-201">Член</span><span class="sxs-lookup"><span data-stu-id="5b339-201">Member</span></span> | <span data-ttu-id="5b339-202">Описание</span><span class="sxs-lookup"><span data-stu-id="5b339-202">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="5b339-203">Возвращает текущий абсолютный URI.</span><span class="sxs-lookup"><span data-stu-id="5b339-203">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="5b339-204">Получает базовый URI (с завершающей косой чертой), который можно добавить в начало относительных путей URI для получения абсолютного URI.</span><span class="sxs-lookup"><span data-stu-id="5b339-204">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="5b339-205">Как правило, `BaseUri` соответствует атрибуту `href` элемента `<base>` документа в *wwwroot/index.HTML* (Blazor _Host) или *pages/. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="5b339-205">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="5b339-206">Переходит по указанному универсальному коду ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="5b339-206">Navigates to the specified URI.</span></span> <span data-ttu-id="5b339-207">Если `forceLoad` равно `true`:</span><span class="sxs-lookup"><span data-stu-id="5b339-207">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="5b339-208">Маршрутизация на стороне клиента обходится.</span><span class="sxs-lookup"><span data-stu-id="5b339-208">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="5b339-209">Браузер вынужден загрузить новую страницу с сервера, независимо от того, обрабатывается ли универсальный код ресурса клиентским маршрутизатором.</span><span class="sxs-lookup"><span data-stu-id="5b339-209">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="5b339-210">Событие, возникающее при изменении расположения навигации.</span><span class="sxs-lookup"><span data-stu-id="5b339-210">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="5b339-211">Преобразует относительный URI в абсолютный URI.</span><span class="sxs-lookup"><span data-stu-id="5b339-211">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="5b339-212">Учитывая базовый URI (например, URI, который ранее был возвращен `GetBaseUri`), преобразует абсолютный URI в URI по отношению к базовому префиксу URI.</span><span class="sxs-lookup"><span data-stu-id="5b339-212">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="5b339-213">Следующий компонент переходит к компоненту `Counter` приложения при выборе этой кнопки:</span><span class="sxs-lookup"><span data-stu-id="5b339-213">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
