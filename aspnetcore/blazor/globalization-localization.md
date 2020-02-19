---
title: Глобализация и локализация Blazor ASP.NET Core
author: guardrex
description: Узнайте, как сделать компоненты Razor доступными для пользователей с несколькими языками и региональными параметрами.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/globalization-localization
ms.openlocfilehash: aba62fa7b6285c8ba884652694f1ea3e3a66ed18
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453190"
---
# <a name="aspnet-core-opno-locblazor-globalization-and-localization"></a><span data-ttu-id="3a24c-103">Глобализация и локализация Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a24c-103">ASP.NET Core Blazor globalization and localization</span></span>

<span data-ttu-id="3a24c-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="3a24c-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="3a24c-105">Компоненты Razor можно сделать доступными для пользователей в различных культурах и языках.</span><span class="sxs-lookup"><span data-stu-id="3a24c-105">Razor components can be made accessible to users in multiple cultures and languages.</span></span> <span data-ttu-id="3a24c-106">Доступны следующие сценарии глобализации и локализации .NET:</span><span class="sxs-lookup"><span data-stu-id="3a24c-106">The following .NET globalization and localization scenarios are available:</span></span>

* <span data-ttu-id="3a24c-107">. Система ресурсов NET</span><span class="sxs-lookup"><span data-stu-id="3a24c-107">.NET's resources system</span></span>
* <span data-ttu-id="3a24c-108">Форматирование чисел и дат, зависящих от языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="3a24c-108">Culture-specific number and date formatting</span></span>

<span data-ttu-id="3a24c-109">В настоящее время поддерживается ограниченный набор сценариев локализации ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3a24c-109">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="3a24c-110">`IStringLocalizer<>` *поддерживается* в Blazor приложениях.</span><span class="sxs-lookup"><span data-stu-id="3a24c-110">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="3a24c-111">Локализация `IHtmlLocalizer<>`, `IViewLocalizer<>`и аннотаций данных ASP.NET Core сценариев MVC и **не поддерживается** в Blazor приложениях.</span><span class="sxs-lookup"><span data-stu-id="3a24c-111">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="3a24c-112">Дополнительные сведения см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="3a24c-112">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="globalization"></a><span data-ttu-id="3a24c-113">Глобализация</span><span class="sxs-lookup"><span data-stu-id="3a24c-113">Globalization</span></span>

<span data-ttu-id="3a24c-114">Функция `@bind` Blazorвыполняет форматирование и синтаксический анализ значений, отображаемых на основе текущего языка и региональных параметров пользователя.</span><span class="sxs-lookup"><span data-stu-id="3a24c-114">Blazor's `@bind` functionality performs formats and parses values for display based on the user's current culture.</span></span>

<span data-ttu-id="3a24c-115">Доступ к текущему языку и региональным параметрам можно получить из свойства <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="3a24c-115">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="3a24c-116">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) используется для следующих типов полей (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="3a24c-116">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="3a24c-117">Предыдущие типы полей:</span><span class="sxs-lookup"><span data-stu-id="3a24c-117">The preceding field types:</span></span>

* <span data-ttu-id="3a24c-118">Отображаются с использованием соответствующих правил форматирования на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="3a24c-118">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="3a24c-119">Не может содержать текст в свободной форме.</span><span class="sxs-lookup"><span data-stu-id="3a24c-119">Can't contain free-form text.</span></span>
* <span data-ttu-id="3a24c-120">Предоставление характеристик взаимодействия с пользователем в зависимости от реализации браузера.</span><span class="sxs-lookup"><span data-stu-id="3a24c-120">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="3a24c-121">Следующие типы полей имеют особые требования к форматированию, которые в настоящее время не поддерживаются Blazor, так как они не поддерживаются всеми основными браузерами.</span><span class="sxs-lookup"><span data-stu-id="3a24c-121">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="3a24c-122">`@bind` поддерживает параметр `@bind:culture`, чтобы предоставить <xref:System.Globalization.CultureInfo?displayProperty=fullName> для синтаксического анализа и форматирования значения.</span><span class="sxs-lookup"><span data-stu-id="3a24c-122">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="3a24c-123">Указание языка и региональных параметров не рекомендуется при использовании типов полей `date` и `number`.</span><span class="sxs-lookup"><span data-stu-id="3a24c-123">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="3a24c-124">`date` и `number` имеют встроенную поддержку Blazor, которая предоставляет требуемый язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="3a24c-124">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

## <a name="localization"></a><span data-ttu-id="3a24c-125">Локализация</span><span class="sxs-lookup"><span data-stu-id="3a24c-125">Localization</span></span>

Blazor<span data-ttu-id="3a24c-126"> серверные приложения локализованы по [промежуточного слоя локализации](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="3a24c-126"> Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="3a24c-127">По промежуточного слоя выбирает соответствующие языки и региональные параметры для пользователей, запрашивающих ресурсы из приложения.</span><span class="sxs-lookup"><span data-stu-id="3a24c-127">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="3a24c-128">Язык и региональные параметры можно задать с помощью одного из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="3a24c-128">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="3a24c-129">Файлы "cookie"</span><span class="sxs-lookup"><span data-stu-id="3a24c-129">Cookies</span></span>](#cookies)
* [<span data-ttu-id="3a24c-130">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="3a24c-130">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="3a24c-131">Дополнительные сведения и примеры см. на сайте <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="3a24c-131">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="3a24c-132">Настройка компоновщика для интернационализации (Blazorная сборка)</span><span class="sxs-lookup"><span data-stu-id="3a24c-132">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="3a24c-133">По умолчанию конфигурация компоновщика Blazor для приложений Blazor WebAssembly исключает сведения об интернационализации, кроме явно запрошенных языковых стандартов.</span><span class="sxs-lookup"><span data-stu-id="3a24c-133">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="3a24c-134">Дополнительные сведения и рекомендации по управлению поведением компоновщика см. в разделе <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="3a24c-134">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="3a24c-135">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="3a24c-135">Cookies</span></span>

<span data-ttu-id="3a24c-136">Файл cookie культуры локализации может сохранять язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="3a24c-136">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="3a24c-137">Файл cookie создается методом `OnGet` страницы узла приложения (*pages/Host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="3a24c-137">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="3a24c-138">По промежуточного слоя локализации считывает файл cookie при последующих запросах, чтобы задать язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="3a24c-138">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="3a24c-139">Использование файла cookie гарантирует, что соединение WebSocket сможет правильно распространить культуру.</span><span class="sxs-lookup"><span data-stu-id="3a24c-139">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="3a24c-140">Если схемы локализации основаны на пути URL-адреса или строке запроса, схема может не поддерживать работу с WebSockets, поэтому она не сохраняет культуру.</span><span class="sxs-lookup"><span data-stu-id="3a24c-140">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="3a24c-141">Поэтому рекомендуемым подходом является использование файла cookie языка и региональных параметров локализации.</span><span class="sxs-lookup"><span data-stu-id="3a24c-141">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="3a24c-142">Любой метод можно использовать для назначения языка и региональных параметров, если язык и региональные параметры сохраняются в файле cookie локализации.</span><span class="sxs-lookup"><span data-stu-id="3a24c-142">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="3a24c-143">Если у приложения уже есть установленная схема локализации для ASP.NET Core на стороне сервера, продолжайте использовать существующую инфраструктуру локализации приложения и задавайте файл cookie культуры локализации в схеме приложения.</span><span class="sxs-lookup"><span data-stu-id="3a24c-143">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="3a24c-144">В следующем примере показано, как задать текущий язык и региональные параметры в файле cookie, который может быть прочитан по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="3a24c-144">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="3a24c-145">Создайте файл *pages/Host. cshtml. CS* со следующим содержимым в приложении Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="3a24c-145">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="3a24c-146">Локализация обрабатывается приложением в следующей последовательности событий:</span><span class="sxs-lookup"><span data-stu-id="3a24c-146">Localization is handled by the app in the following sequence of events:</span></span>

1. <span data-ttu-id="3a24c-147">Браузер отправляет в приложение исходный HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="3a24c-147">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="3a24c-148">Язык и региональные параметры назначаются по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="3a24c-148">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="3a24c-149">Метод `OnGet` в *_Host. cshtml. CS* сохраняет язык и региональные параметры в файле cookie как часть ответа.</span><span class="sxs-lookup"><span data-stu-id="3a24c-149">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="3a24c-150">Браузер открывает соединение WebSocket для создания интерактивного сеанса Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="3a24c-150">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="3a24c-151">По промежуточного слоя для локализации считывает файл cookie и назначает язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="3a24c-151">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="3a24c-152">Сеанс Blazor Server начинается с правильного языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="3a24c-152">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="3a24c-153">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="3a24c-153">Provide UI to choose the culture</span></span>

<span data-ttu-id="3a24c-154">Для предоставления пользовательского интерфейса, позволяющего пользователю выбирать язык и региональные параметры, рекомендуется *подход на основе перенаправления* .</span><span class="sxs-lookup"><span data-stu-id="3a24c-154">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="3a24c-155">Процесс аналогичен тому, что происходит в веб-приложении, когда пользователь пытается получить доступ к защищенному ресурсу,&mdash;пользователь перенаправляется на страницу входа, а затем перенаправляется обратно к исходному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="3a24c-155">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="3a24c-156">Приложение сохраняет выбранный язык и региональные параметры пользователя с помощью перенаправления к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="3a24c-156">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="3a24c-157">Контроллер устанавливает выбранный язык и региональные параметры пользователя в файл cookie и перенаправляет пользователя обратно к исходному коду URI.</span><span class="sxs-lookup"><span data-stu-id="3a24c-157">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="3a24c-158">Установите конечную точку HTTP на сервере, чтобы задать выбранный язык и региональные параметры пользователя в файле cookie, и выполните перенаправление обратно в исходный URI:</span><span class="sxs-lookup"><span data-stu-id="3a24c-158">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="3a24c-159">Для предотвращения атак с открытым перенаправлением используйте результат действия `LocalRedirect`.</span><span class="sxs-lookup"><span data-stu-id="3a24c-159">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="3a24c-160">Дополнительные сведения см. в разделе <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="3a24c-160">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="3a24c-161">В следующем компоненте показан пример выполнения начального перенаправления, когда пользователь выбирает язык и региональные параметры:</span><span class="sxs-lookup"><span data-stu-id="3a24c-161">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="3a24c-162">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3a24c-162">Additional resources</span></span>

* <xref:fundamentals/localization>
