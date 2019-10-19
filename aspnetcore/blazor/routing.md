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
# <a name="aspnet-core-opno-locblazor-routing"></a>Маршрутизация Blazor ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Узнайте, как маршрутизировать запросы и как использовать компонент `NavLink` для создания навигационных ссылок в Blazor приложениях.

## <a name="aspnet-core-endpoint-routing-integration"></a>Интеграция маршрутизации конечных точек ASP.NET Core

Blazor сервер интегрирован в [ASP.NET Coreную маршрутизацию конечных точек](xref:fundamentals/routing). ASP.NET Core приложение настроено для приема входящих подключений для интерактивных компонентов с `MapBlazorHub` в `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

Наиболее типичная конфигурация — маршрутизация всех запросов на страницу Razor, которая выступает в качестве узла для серверной части приложения Blazor Server. По соглашению страница *узла* обычно называется *_Host. cshtml*. Маршрут, указанный в файле узла, называется *резервным маршрутом* , так как он работает с низким приоритетом в соответствии с маршрутизацией. Резервный маршрут рассматривается, если другие маршруты не совпадают. Это позволяет приложению использовать другие контроллеры и страницы, не мешая работе приложения Blazor Server.

## <a name="route-templates"></a>Шаблоны маршрутов

Компонент `Router` обеспечивает маршрутизацию для каждого компонента с указанным маршрутом. Компонент `Router` отображается в файле *app. Razor* :

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

При компиляции файла *. Razor* с директивой `@page` созданный класс предоставляет <xref:Microsoft.AspNetCore.Mvc.RouteAttribute>, задавая шаблон маршрута.

В среде выполнения компонент `RouteView`:

* Получает `RouteData` из `Router` вместе с необходимыми параметрами.
* Визуализирует указанный компонент с его макетом (или необязательным макетом по умолчанию), используя указанные параметры.

При необходимости можно указать параметр `DefaultLayout` с классом макета, который будет использоваться для компонентов, не указывающих макет. Шаблоны Blazor по умолчанию определяют компонент `MainLayout`. *Маинлайаут. Razor* находится в *общей* папке проекта шаблона. Дополнительные сведения о макетах см. в разделе <xref:blazor/layouts>.

К компоненту можно применить несколько шаблонов маршрутов. Следующий компонент отвечает на запросы для `/BlazorRoute` и `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Для правильного разрешения URL-адресов приложение должно включать тег `<base>` в файл *wwwroot/index.HTML* (Blazor _Host) или *pages/. cshtml* (Blazor Server) с базовым путем к приложению, указанным в атрибуте `href` (`<base href="/">`). Для получения дополнительной информации см. <xref:host-and-deploy/blazor/index#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Указать пользовательское содержимое, когда содержимое не найдено

Компонент `Router` позволяет приложению указать пользовательское содержимое, если содержимое для запрошенного маршрута не найдено.

В файле *app. Razor* задайте пользовательское содержимое в параметре шаблона `NotFound` компонента `Router`:

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

Содержимое тегов `<NotFound>` может включать произвольные элементы, например другие интерактивные компоненты. Сведения о применении макета по умолчанию к содержимому `NotFound` см. в разделе <xref:blazor/layouts>.

## <a name="route-to-components-from-multiple-assemblies"></a>Маршрутизация к компонентам из нескольких сборок

Используйте параметр `AdditionalAssemblies`, чтобы указать дополнительные сборки для компонента `Router`, который следует учитывать при поиске маршрутизируемых компонентов. Указанные сборки рассматриваются в дополнение к сборке, указанной `AppAssembly`. В следующем примере `Component1` — это маршрутизируемый компонент, определенный в упоминаемой библиотеке классов. Следующий пример `AdditionalAssemblies` приводит к поддержке маршрутизации для `Component1`:

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a>Параметры маршрута

Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента с тем же именем (без учета регистра):

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

Необязательные параметры не поддерживаются для Blazor приложений в ASP.NET Core 3,0. В предыдущем примере применяются две директивы `@page`. Первый позволяет переходить к компоненту без параметра. Вторая директива `@page` принимает параметр маршрута `{text}` и присваивает значение свойству `Text`.

## <a name="route-constraints"></a>Ограничения маршрута

Ограничение маршрута применяет сопоставление типов в сегменте маршрута к компоненту.

В следующем примере маршрут к компоненту `Users` соответствует только в следующих случаях:

* В URL-адресе запроса имеется сегмент маршрута `Id`.
* Сегмент `Id` является целым числом (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Ограничения маршрутов, приведенные в следующей таблице, доступны. Сведения об ограничениях маршрута, соответствующих инвариантному языку и региональным параметрам, см. в предупреждении ниже таблицы для получения дополнительных сведений.

| Ограничение | Пример           | Примеры совпадений                                                                  | Инвариант<br>язык и региональные параметры<br>соответствие |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Нет                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Да                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Да                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Да                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Да                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Нет                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Да                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Да                              |

> [!WARNING]
> Ограничения маршрута, которые проверяют URL-адрес и могут быть преобразованы в тип CLR (например, `int` или `DateTime`), всегда используют инвариантные язык и региональные параметры. Эти ограничения предполагают, что URL-адрес является нелокализуемым.

### <a name="routing-with-urls-that-contain-dots"></a>Маршрутизация с URL-адресами, содержащими точки

В Blazor серверных приложений маршрут по умолчанию в *_Host. cshtml* — `/` (`@page "/"`). URL-адрес запроса, содержащий точку (`.`), не совпадает с маршрутом по умолчанию, так как URL-адрес является запросом файла. Приложение Blazor возвращает ответ *404-не найдено* для статического файла, который не существует. Чтобы использовать маршруты, содержащие точку, настройте *_Host. cshtml* со следующим шаблоном маршрута:

```cshtml
@page "/{**path}"
```

Шаблон `"/{**path}"` включает в себя следующее:

* Двойное звездочка *-все* синтаксис (`**`) для захвата пути по нескольким папкам без кодирования косой черты (`/`).
* Имя параметра маршрута `path`.

Для получения дополнительной информации см. <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Компонент Навлинк

Используйте компонент `NavLink` вместо элементов гиперссылки HTML (`<a>`) при создании навигационных ссылок. Компонент `NavLink` ведет себя как элемент `<a>`, за исключением того, что он переключает класс CSS `active` в зависимости от того, соответствует ли его `href` текущему URL-адресу. Класс `active` помогает пользователю понять, какая страница является активной страницей между отображаемыми ссылками навигации.

Следующий компонент `NavMenu` создает панель навигации [начальной загрузки](https://getbootstrap.com/docs/) , которая демонстрирует использование компонентов `NavLink`:

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Существует два параметра `NavLinkMatch`, которые можно назначить атрибуту `Match` элемента `<NavLink>`:

* `NavLinkMatch.All` &ndash; `NavLink` активна, если он соответствует всему текущему URL-адресу.
* `NavLinkMatch.Prefix` (*по умолчанию*) &ndash; активна `NavLink`, если он соответствует любому префиксу текущего URL-адреса.

В предыдущем примере домашняя `NavLink` `href=""` соответствует домашнему URL-адресу и получает класс CSS `active` в URL-адресе базового пути приложения по умолчанию (например, `https://localhost:5001/`). Второй `NavLink` получает класс `active`, когда пользователь посещает любой URL-адрес с префиксом `MyComponent` (например, `https://localhost:5001/MyComponent` и `https://localhost:5001/MyComponent/AnotherSegment`).

Дополнительные атрибуты компонента `NavLink` передаются в отображаемый тег привязки. В следующем примере компонент `NavLink` включает атрибут `target`:

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Отображается следующая разметка HTML:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>URI и вспомогательные функции состояния навигации

Используйте `Microsoft.AspNetCore.Components.NavigationManager` для работы с URI и навигацией C# в коде. `NavigationManager` предоставляет событие и методы, приведенные в следующей таблице.

| Член | Описание |
| ------ | ----------- |
| `Uri` | Возвращает текущий абсолютный URI. |
| `BaseUri` | Получает базовый URI (с завершающей косой чертой), который можно добавить в начало относительных путей URI для получения абсолютного URI. Как правило, `BaseUri` соответствует атрибуту `href` элемента `<base>` документа в *wwwroot/index.HTML* (Blazor _Host) или *pages/. cshtml* (Blazor Server). |
| `NavigateTo` | Переходит по указанному универсальному коду ресурса (URI). Если `forceLoad` равно `true`:<ul><li>Маршрутизация на стороне клиента обходится.</li><li>Браузер вынужден загрузить новую страницу с сервера, независимо от того, обрабатывается ли универсальный код ресурса клиентским маршрутизатором.</li></ul> |
| `LocationChanged` | Событие, возникающее при изменении расположения навигации. |
| `ToAbsoluteUri` | Преобразует относительный URI в абсолютный URI. |
| `ToBaseRelativePath` | Учитывая базовый URI (например, URI, который ранее был возвращен `GetBaseUri`), преобразует абсолютный URI в URI по отношению к базовому префиксу URI. |

Следующий компонент переходит к компоненту `Counter` приложения при выборе этой кнопки:

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
