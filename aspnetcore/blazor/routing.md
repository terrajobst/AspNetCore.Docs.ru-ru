---
title: ASP.NET Core маршрутизация Блазор
author: guardrex
description: Узнайте, как маршрутизировать запросы в приложениях и о компоненте Навлинк.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/routing
ms.openlocfilehash: ccc8231d1925d4a55eeef618800c10652f9ae36d
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211657"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="11784-103">ASP.NET Core маршрутизация Блазор</span><span class="sxs-lookup"><span data-stu-id="11784-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="11784-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="11784-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="11784-105">Узнайте, как маршрутизировать запросы и как использовать `NavLink` компонент для создания навигационных ссылок в приложениях блазор.</span><span class="sxs-lookup"><span data-stu-id="11784-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="11784-106">Интеграция маршрутизации конечных точек ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11784-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="11784-107">Сервер блазор интегрирован в [ASP.NET Coreную маршрутизацию конечных точек](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="11784-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="11784-108">Приложение ASP.NET Core настроено для приема входящих подключений для интерактивных компонентов с `MapBlazorHub` в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="11784-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="11784-109">Наиболее типичная конфигурация — маршрутизация всех запросов на страницу Razor, которая выступает в качестве узла для серверной части серверного приложения Блазор.</span><span class="sxs-lookup"><span data-stu-id="11784-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="11784-110">По соглашению страница *узла* обычно называется *_Host. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="11784-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="11784-111">Маршрут, указанный в файле узла, называется *резервным маршрутом* , так как он работает с низким приоритетом в соответствии с маршрутизацией.</span><span class="sxs-lookup"><span data-stu-id="11784-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="11784-112">Резервный маршрут рассматривается, если другие маршруты не совпадают.</span><span class="sxs-lookup"><span data-stu-id="11784-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="11784-113">Это позволяет приложению использовать другие контроллеры и страницы, не мешая работе с серверным приложением Блазор.</span><span class="sxs-lookup"><span data-stu-id="11784-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="11784-114">Шаблоны маршрутов</span><span class="sxs-lookup"><span data-stu-id="11784-114">Route templates</span></span>

<span data-ttu-id="11784-115">`Router` Компонент обеспечивает маршрутизацию для каждого компонента с указанным маршрутом.</span><span class="sxs-lookup"><span data-stu-id="11784-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="11784-116">Компонент отображается в файле *app. Razor:* `Router`</span><span class="sxs-lookup"><span data-stu-id="11784-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="11784-117">При компиляции файла *. Razor* с `@page` директивой создается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> класс, в котором указан шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="11784-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="11784-118">Во время выполнения `RouteView` компонент:</span><span class="sxs-lookup"><span data-stu-id="11784-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="11784-119">`RouteData` Получает`Router` из вместе с любыми нужными параметрами.</span><span class="sxs-lookup"><span data-stu-id="11784-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="11784-120">Визуализирует указанный компонент с его макетом (или необязательным макетом по умолчанию), используя указанные параметры.</span><span class="sxs-lookup"><span data-stu-id="11784-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="11784-121">При необходимости можно указать `DefaultLayout` параметр с классом макета, который будет использоваться для компонентов, которые не задают макет.</span><span class="sxs-lookup"><span data-stu-id="11784-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="11784-122">Шаблоны блазор по умолчанию определяют `MainLayout` компонент.</span><span class="sxs-lookup"><span data-stu-id="11784-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="11784-123">*Маинлайаут. Razor* находится в *общей* папке проекта шаблона.</span><span class="sxs-lookup"><span data-stu-id="11784-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="11784-124">Дополнительные сведения о макетах см. <xref:blazor/layouts>в разделе.</span><span class="sxs-lookup"><span data-stu-id="11784-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="11784-125">К компоненту можно применить несколько шаблонов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="11784-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="11784-126">Следующий компонент отвечает на запросы `/BlazorRoute` и: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="11784-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="11784-127">Для правильного разрешения URL-адресов приложение `<base>` должно включать тег в файл *wwwroot/index.HTML* (блазор) или *pages/_Host. cshtml* (блазор Server) с базовым путем к `href` приложению, указанным в атрибуте (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="11784-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="11784-128">Дополнительные сведения см. в разделе <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="11784-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="11784-129">Указать пользовательское содержимое, когда содержимое не найдено</span><span class="sxs-lookup"><span data-stu-id="11784-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="11784-130">`Router` Компонент позволяет приложению указать пользовательское содержимое, если содержимое для запрошенного маршрута не найдено.</span><span class="sxs-lookup"><span data-stu-id="11784-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="11784-131">В файле *app. Razor* задайте пользовательское содержимое в `NotFound` параметре `Router` шаблона компонента:</span><span class="sxs-lookup"><span data-stu-id="11784-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="11784-132">Содержимое `<NotFound>` тегов может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="11784-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="11784-133">Сведения о применении макета по умолчанию к `NotFound` содержимому см. в разделе. <xref:blazor/layouts></span><span class="sxs-lookup"><span data-stu-id="11784-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="11784-134">Маршрутизация к компонентам из нескольких сборок</span><span class="sxs-lookup"><span data-stu-id="11784-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="11784-135">Используйте параметр `AdditionalAssemblies` , чтобы указать дополнительные сборки для компонента `Router` , которые следует учитывать при поиске маршрутизируемых компонентов.</span><span class="sxs-lookup"><span data-stu-id="11784-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="11784-136">Указанные сборки рассматриваются в дополнение к сборке, `AppAssembly`указанной в параметре.</span><span class="sxs-lookup"><span data-stu-id="11784-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="11784-137">В следующем примере — это `Component1` маршрутизируемый компонент, определенный в упоминаемой библиотеке классов.</span><span class="sxs-lookup"><span data-stu-id="11784-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="11784-138">В следующем `AdditionalAssemblies` примере приводится поддержка маршрутизации для `Component1`:</span><span class="sxs-lookup"><span data-stu-id="11784-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="11784-139">< маршрутизатор Аппассембли = "typeof (Program)". Сборка "Аддитионалассемблиес =" New [] {typeof (Component1). Сборка} >...</Router></span><span class="sxs-lookup"><span data-stu-id="11784-139"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="11784-140">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="11784-140">Route parameters</span></span>

<span data-ttu-id="11784-141">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента с тем же именем (без учета регистра):</span><span class="sxs-lookup"><span data-stu-id="11784-141">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="11784-142">Необязательные параметры не поддерживаются для приложений Блазор в предварительной версии ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="11784-142">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="11784-143">В `@page` предыдущем примере применяются две директивы.</span><span class="sxs-lookup"><span data-stu-id="11784-143">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="11784-144">Первый позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="11784-144">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="11784-145">Вторая `@page` директива `{text}` принимает параметр Route и присваивает значение `Text` свойству.</span><span class="sxs-lookup"><span data-stu-id="11784-145">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="11784-146">Ограничения маршрута</span><span class="sxs-lookup"><span data-stu-id="11784-146">Route constraints</span></span>

<span data-ttu-id="11784-147">Ограничение маршрута применяет сопоставление типов в сегменте маршрута к компоненту.</span><span class="sxs-lookup"><span data-stu-id="11784-147">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="11784-148">В следующем примере маршрут к `Users` компоненту соответствует только в том случае, если:</span><span class="sxs-lookup"><span data-stu-id="11784-148">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="11784-149">В URL-адресе запроса имеется сегмент маршрута.`Id`</span><span class="sxs-lookup"><span data-stu-id="11784-149">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="11784-150">Сегмент является целым числом (`int`). `Id`</span><span class="sxs-lookup"><span data-stu-id="11784-150">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="11784-151">Ограничения маршрутов, приведенные в следующей таблице, доступны.</span><span class="sxs-lookup"><span data-stu-id="11784-151">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="11784-152">Сведения об ограничениях маршрута, соответствующих инвариантному языку и региональным параметрам, см. в предупреждении ниже таблицы для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="11784-152">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="11784-153">Ограничение</span><span class="sxs-lookup"><span data-stu-id="11784-153">Constraint</span></span> | <span data-ttu-id="11784-154">Пример</span><span class="sxs-lookup"><span data-stu-id="11784-154">Example</span></span>           | <span data-ttu-id="11784-155">Примеры совпадений</span><span class="sxs-lookup"><span data-stu-id="11784-155">Example Matches</span></span>                                                                  | <span data-ttu-id="11784-156">Инвариант</span><span class="sxs-lookup"><span data-stu-id="11784-156">Invariant</span></span><br><span data-ttu-id="11784-157">язык и региональные параметры</span><span class="sxs-lookup"><span data-stu-id="11784-157">culture</span></span><br><span data-ttu-id="11784-158">соответствие</span><span class="sxs-lookup"><span data-stu-id="11784-158">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="11784-159">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="11784-159">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="11784-160">Нет</span><span class="sxs-lookup"><span data-stu-id="11784-160">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="11784-161">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="11784-161">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="11784-162">Да</span><span class="sxs-lookup"><span data-stu-id="11784-162">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="11784-163">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="11784-163">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="11784-164">Да</span><span class="sxs-lookup"><span data-stu-id="11784-164">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="11784-165">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="11784-165">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="11784-166">Да</span><span class="sxs-lookup"><span data-stu-id="11784-166">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="11784-167">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="11784-167">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="11784-168">Да</span><span class="sxs-lookup"><span data-stu-id="11784-168">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="11784-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="11784-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="11784-170">Нет</span><span class="sxs-lookup"><span data-stu-id="11784-170">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="11784-171">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="11784-171">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="11784-172">Да</span><span class="sxs-lookup"><span data-stu-id="11784-172">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="11784-173">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="11784-173">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="11784-174">Да</span><span class="sxs-lookup"><span data-stu-id="11784-174">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="11784-175">Ограничения маршрута, которые проверяют URL-адрес и могут быть преобразованы в тип CLR (например, `int` или `DateTime`), всегда используют инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="11784-175">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="11784-176">Эти ограничения предполагают, что URL-адрес является нелокализуемым.</span><span class="sxs-lookup"><span data-stu-id="11784-176">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="11784-177">Маршрутизация с URL-адресами, содержащими точки</span><span class="sxs-lookup"><span data-stu-id="11784-177">Routing with URLs that contain dots</span></span>

<span data-ttu-id="11784-178">В серверных приложениях Блазор маршрутом по умолчанию в *_Host. cshtml* является `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="11784-178">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="11784-179">URL-адрес запроса, содержащий точку (`.`), не совпадает с маршрутом по умолчанию, так как URL-адрес является запросом файла.</span><span class="sxs-lookup"><span data-stu-id="11784-179">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="11784-180">Приложение Блазор возвращает *404-не найденный* ответ для статического файла, который не существует.</span><span class="sxs-lookup"><span data-stu-id="11784-180">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="11784-181">Чтобы использовать маршруты, содержащие точку, настройте *_Host. cshtml* со следующим шаблоном маршрута:</span><span class="sxs-lookup"><span data-stu-id="11784-181">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="11784-182">`"/{**path}"` Шаблон включает:</span><span class="sxs-lookup"><span data-stu-id="11784-182">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="11784-183">Двойное звездочка *Catch-All* синтаксис (`**`) для записи пути по нескольким границам папок без кодирования косой черты (`/`).</span><span class="sxs-lookup"><span data-stu-id="11784-183">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="11784-184">Имя `path` параметра маршрута.</span><span class="sxs-lookup"><span data-stu-id="11784-184">A `path` route parameter name.</span></span>

<span data-ttu-id="11784-185">Дополнительные сведения см. в разделе <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="11784-185">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="11784-186">Компонент Навлинк</span><span class="sxs-lookup"><span data-stu-id="11784-186">NavLink component</span></span>

<span data-ttu-id="11784-187">Используйте компонент вместо элементов HTML-гиперссылок (`<a>`) при создании навигационных ссылок. `NavLink`</span><span class="sxs-lookup"><span data-stu-id="11784-187">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="11784-188">Компонент ведет себя `<a>` как элемент, `active` за исключением того, что он переключает класс CSS на основе того, `href` соответствует ли он текущему URL-адресу. `NavLink`</span><span class="sxs-lookup"><span data-stu-id="11784-188">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="11784-189">`active` Класс помогает пользователю понять, какая страница является активной страницей между отображаемыми ссылками навигации.</span><span class="sxs-lookup"><span data-stu-id="11784-189">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="11784-190">Следующий `NavMenu` компонент создает панель навигации [начальной загрузки](https://getbootstrap.com/docs/) , которая демонстрирует использование `NavLink` компонентов.</span><span class="sxs-lookup"><span data-stu-id="11784-190">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="11784-191">`Match` `NavLinkMatch` Атрибут`<NavLink>` элемента можно назначить двумя способами:</span><span class="sxs-lookup"><span data-stu-id="11784-191">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="11784-192">`NavLinkMatch.All`&ndash; Параметр`NavLink` активен, если он соответствует всему текущему URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="11784-192">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="11784-193">`NavLinkMatch.Prefix`(*по умолчанию*) Параметр активен, `NavLink` если он соответствует любому префиксу текущего URL-адреса. &ndash;</span><span class="sxs-lookup"><span data-stu-id="11784-193">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="11784-194">В предыдущем примере домашняя `NavLink` `href=""` страница совпадает с домашним `active` URL-адресом и получает класс CSS только в URL-адресе базового пути по умолчанию `https://localhost:5001/`для приложения (например,).</span><span class="sxs-lookup"><span data-stu-id="11784-194">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="11784-195">Второй `NavLink` получает класс, `active` когда пользователь `MyComponent` посещает любой URL-адрес с префиксом (например, `https://localhost:5001/MyComponent` и `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="11784-195">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="11784-196">Дополнительные `NavLink` атрибуты компонента передаются в отображаемый тег привязки.</span><span class="sxs-lookup"><span data-stu-id="11784-196">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="11784-197">В следующем примере `NavLink` компонент `target` включает атрибут:</span><span class="sxs-lookup"><span data-stu-id="11784-197">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="11784-198">Отображается следующая разметка HTML:</span><span class="sxs-lookup"><span data-stu-id="11784-198">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="11784-199">URI и вспомогательные функции состояния навигации</span><span class="sxs-lookup"><span data-stu-id="11784-199">URI and navigation state helpers</span></span>

<span data-ttu-id="11784-200">Используется `Microsoft.AspNetCore.Components.NavigationManager` для работы с URI и навигацией C# в коде.</span><span class="sxs-lookup"><span data-stu-id="11784-200">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="11784-201">`NavigationManager`предоставляет события и методы, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="11784-201">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="11784-202">Член</span><span class="sxs-lookup"><span data-stu-id="11784-202">Member</span></span> | <span data-ttu-id="11784-203">Описание</span><span class="sxs-lookup"><span data-stu-id="11784-203">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="11784-204">Возвращает текущий абсолютный URI.</span><span class="sxs-lookup"><span data-stu-id="11784-204">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="11784-205">Получает базовый URI (с завершающей косой чертой), который можно добавить в начало относительных путей URI для получения абсолютного URI.</span><span class="sxs-lookup"><span data-stu-id="11784-205">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="11784-206">Как правило `BaseUri` , соответствует `href`атрибуту элемента документа в wwwroot/index.HTML (блазор) или *pages/_Host. cshtml* (блазор Server). `<base>`</span><span class="sxs-lookup"><span data-stu-id="11784-206">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="11784-207">Переходит по указанному универсальному коду ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="11784-207">Navigates to the specified URI.</span></span> <span data-ttu-id="11784-208">Если `forceLoad` имеет `true`значение:</span><span class="sxs-lookup"><span data-stu-id="11784-208">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="11784-209">Маршрутизация на стороне клиента обходится.</span><span class="sxs-lookup"><span data-stu-id="11784-209">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="11784-210">Браузер вынужден загрузить новую страницу с сервера, независимо от того, обрабатывается ли универсальный код ресурса клиентским маршрутизатором.</span><span class="sxs-lookup"><span data-stu-id="11784-210">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="11784-211">Событие, возникающее при изменении расположения навигации.</span><span class="sxs-lookup"><span data-stu-id="11784-211">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="11784-212">Преобразует относительный URI в абсолютный URI.</span><span class="sxs-lookup"><span data-stu-id="11784-212">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="11784-213">Учитывая базовый URI (например, URI, ранее возвращенный `GetBaseUri`), преобразует абсолютный URI в универсальный код ресурса (URI) по отношению к базовому префиксу URI.</span><span class="sxs-lookup"><span data-stu-id="11784-213">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="11784-214">Следующий компонент переходит к `Counter` компоненту приложения при выборе этой кнопки:</span><span class="sxs-lookup"><span data-stu-id="11784-214">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
