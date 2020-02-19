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
# <a name="aspnet-core-opno-locblazor-globalization-and-localization"></a>Глобализация и локализация Blazor ASP.NET Core

[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)

Компоненты Razor можно сделать доступными для пользователей в различных культурах и языках. Доступны следующие сценарии глобализации и локализации .NET:

* . Система ресурсов NET
* Форматирование чисел и дат, зависящих от языка и региональных параметров

В настоящее время поддерживается ограниченный набор сценариев локализации ASP.NET Core:

* `IStringLocalizer<>` *поддерживается* в Blazor приложениях.
* Локализация `IHtmlLocalizer<>`, `IViewLocalizer<>`и аннотаций данных ASP.NET Core сценариев MVC и **не поддерживается** в Blazor приложениях.

Дополнительные сведения см. в разделе <xref:fundamentals/localization>.

## <a name="globalization"></a>Глобализация

Функция `@bind` Blazorвыполняет форматирование и синтаксический анализ значений, отображаемых на основе текущего языка и региональных параметров пользователя.

Доступ к текущему языку и региональным параметрам можно получить из свойства <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.

[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) используется для следующих типов полей (`<input type="{TYPE}" />`):

* `date`
* `number`

Предыдущие типы полей:

* Отображаются с использованием соответствующих правил форматирования на основе браузера.
* Не может содержать текст в свободной форме.
* Предоставление характеристик взаимодействия с пользователем в зависимости от реализации браузера.

Следующие типы полей имеют особые требования к форматированию, которые в настоящее время не поддерживаются Blazor, так как они не поддерживаются всеми основными браузерами.

* `datetime-local`
* `month`
* `week`

`@bind` поддерживает параметр `@bind:culture`, чтобы предоставить <xref:System.Globalization.CultureInfo?displayProperty=fullName> для синтаксического анализа и форматирования значения. Указание языка и региональных параметров не рекомендуется при использовании типов полей `date` и `number`. `date` и `number` имеют встроенную поддержку Blazor, которая предоставляет требуемый язык и региональные параметры.

## <a name="localization"></a>Локализация

Blazor серверные приложения локализованы по [промежуточного слоя локализации](xref:fundamentals/localization#localization-middleware). По промежуточного слоя выбирает соответствующие языки и региональные параметры для пользователей, запрашивающих ресурсы из приложения.

Язык и региональные параметры можно задать с помощью одного из следующих подходов:

* [Файлы "cookie"](#cookies)
* [Предоставление пользовательского интерфейса для выбора языка и региональных параметров](#provide-ui-to-choose-the-culture)

Дополнительные сведения и примеры см. на сайте <xref:fundamentals/localization>.

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a>Настройка компоновщика для интернационализации (Blazorная сборка)

По умолчанию конфигурация компоновщика Blazor для приложений Blazor WebAssembly исключает сведения об интернационализации, кроме явно запрошенных языковых стандартов. Дополнительные сведения и рекомендации по управлению поведением компоновщика см. в разделе <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.

### <a name="cookies"></a>Файлы cookie

Файл cookie культуры локализации может сохранять язык и региональные параметры пользователя. Файл cookie создается методом `OnGet` страницы узла приложения (*pages/Host. cshtml. CS*). По промежуточного слоя локализации считывает файл cookie при последующих запросах, чтобы задать язык и региональные параметры пользователя. 

Использование файла cookie гарантирует, что соединение WebSocket сможет правильно распространить культуру. Если схемы локализации основаны на пути URL-адреса или строке запроса, схема может не поддерживать работу с WebSockets, поэтому она не сохраняет культуру. Поэтому рекомендуемым подходом является использование файла cookie языка и региональных параметров локализации.

Любой метод можно использовать для назначения языка и региональных параметров, если язык и региональные параметры сохраняются в файле cookie локализации. Если у приложения уже есть установленная схема локализации для ASP.NET Core на стороне сервера, продолжайте использовать существующую инфраструктуру локализации приложения и задавайте файл cookie культуры локализации в схеме приложения.

В следующем примере показано, как задать текущий язык и региональные параметры в файле cookie, который может быть прочитан по промежуточного слоя локализации. Создайте файл *pages/Host. cshtml. CS* со следующим содержимым в приложении Blazor Server:

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

Локализация обрабатывается приложением в следующей последовательности событий:

1. Браузер отправляет в приложение исходный HTTP-запрос.
1. Язык и региональные параметры назначаются по промежуточного слоя локализации.
1. Метод `OnGet` в *_Host. cshtml. CS* сохраняет язык и региональные параметры в файле cookie как часть ответа.
1. Браузер открывает соединение WebSocket для создания интерактивного сеанса Blazor Server.
1. По промежуточного слоя для локализации считывает файл cookie и назначает язык и региональные параметры.
1. Сеанс Blazor Server начинается с правильного языка и региональных параметров.

### <a name="provide-ui-to-choose-the-culture"></a>Предоставление пользовательского интерфейса для выбора языка и региональных параметров

Для предоставления пользовательского интерфейса, позволяющего пользователю выбирать язык и региональные параметры, рекомендуется *подход на основе перенаправления* . Процесс аналогичен тому, что происходит в веб-приложении, когда пользователь пытается получить доступ к защищенному ресурсу,&mdash;пользователь перенаправляется на страницу входа, а затем перенаправляется обратно к исходному ресурсу. 

Приложение сохраняет выбранный язык и региональные параметры пользователя с помощью перенаправления к контроллеру. Контроллер устанавливает выбранный язык и региональные параметры пользователя в файл cookie и перенаправляет пользователя обратно к исходному коду URI.

Установите конечную точку HTTP на сервере, чтобы задать выбранный язык и региональные параметры пользователя в файле cookie, и выполните перенаправление обратно в исходный URI:

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
> Для предотвращения атак с открытым перенаправлением используйте результат действия `LocalRedirect`. Дополнительные сведения см. в разделе <xref:security/preventing-open-redirects>.

В следующем компоненте показан пример выполнения начального перенаправления, когда пользователь выбирает язык и региональные параметры:

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/localization>
