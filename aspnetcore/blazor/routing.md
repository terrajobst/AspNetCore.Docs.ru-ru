---
title: Маршрутизация Blazor ASP.NET Core
author: guardrex
description: Узнайте, как маршрутизировать запросы в приложениях и о компоненте Навлинк.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/routing
ms.openlocfilehash: 32459f9f42220b01ce04e6444a9bb4a9592ee2da
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928286"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core маршрутизация Блазор

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Узнайте, как маршрутизировать запросы и как использовать компонент `NavLink` для создания навигационных ссылок в приложениях Блазор.

## <a name="aspnet-core-endpoint-routing-integration"></a>Интеграция маршрутизации конечных точек ASP.NET Core

Сервер блазор интегрирован в [ASP.NET Coreную маршрутизацию конечных точек](xref:fundamentals/routing). ASP.NET Core приложение настроено для приема входящих подключений для интерактивных компонентов с `MapBlazorHub` в `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

Наиболее типичная конфигурация — маршрутизация всех запросов на страницу Razor, которая выступает в качестве узла для серверной части серверного приложения Блазор. По соглашению страница *узла* обычно называется *_Host. cshtml*. Маршрут, указанный в файле узла, называется *резервным маршрутом* , так как он работает с низким приоритетом в соответствии с маршрутизацией. Резервный маршрут рассматривается, если другие маршруты не совпадают. Это позволяет приложению использовать другие контроллеры и страницы, не мешая работе с серверным приложением Блазор.

## <a name="route-templates"></a>Шаблоны маршрутов

Компонент `Router` позволяет выполнять маршрутизацию для каждого компонента с указанным маршрутом. `Router` компонент отображается в файле *app. Razor* :

```razor
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

При компиляции файла *. Razor* с директивой `@page` созданный класс предоставляется <xref:Microsoft.AspNetCore.Components.RouteAttribute> указания шаблона маршрута.

В среде выполнения `RouteView` компонент:

* Получает `RouteData` от `Router` вместе с любыми необходимыми параметрами.
* Визуализирует указанный компонент с его макетом (или необязательным макетом по умолчанию), используя указанные параметры.

При необходимости можно указать `DefaultLayout` параметр с классом макета, который будет использоваться для компонентов, которые не задают макет. Шаблоны Блазор по умолчанию определяют компонент `MainLayout`. *Маинлайаут. Razor* находится в *общей* папке проекта шаблона. Дополнительные сведения о макетах см. в разделе <xref:blazor/layouts>.

К компоненту можно применить несколько шаблонов маршрутов. Следующий компонент отвечает на запросы `/BlazorRoute` и `/DifferentBlazorRoute`:

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

> [!IMPORTANT]
> Для правильного разрешения URL-адресов приложение должно включать тег `<base>` в файл *wwwroot/index.HTML* (блазор) или *pages/_Host. cshtml* (блазор Server) с базовым путем к приложению, указанным в атрибуте `href` (`<base href="/">`). Для получения дополнительной информации см. <xref:host-and-deploy/blazor/index#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Указать пользовательское содержимое, когда содержимое не найдено

Компонент `Router` позволяет приложению указать пользовательское содержимое, если содержимое для запрошенного маршрута не найдено.

В файле *app. Razor* задайте пользовательское содержимое в параметре шаблона `NotFound` компонента `Router`:

```razor
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

Содержимое тегов `<NotFound>` может включать произвольные элементы, например другие интерактивные компоненты. Сведения о применении макета по умолчанию к `NotFound` содержимому см. в разделе <xref:blazor/layouts>.

## <a name="route-to-components-from-multiple-assemblies"></a>Маршрутизация к компонентам из нескольких сборок

Используйте параметр `AdditionalAssemblies`, чтобы указать дополнительные сборки для компонента `Router`, которые следует учитывать при поиске маршрутизируемых компонентов. Указанные сборки рассматриваются в дополнение к сборке, указанной `AppAssembly`. В следующем примере `Component1` представляет собой маршрутизируемый компонент, определенный в упоминаемой библиотеке классов. В следующем примере `AdditionalAssemblies` приводится поддержка маршрутизации для `Component1`.

```razor
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a>Параметры маршрута

Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента с тем же именем (без учета регистра):

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

Необязательные параметры не поддерживаются. В предыдущем примере применяются две директивы `@page`. Первый позволяет переходить к компоненту без параметра. Вторая директива `@page` принимает параметр `{text}` Route и присваивает значение свойству `Text`.

## <a name="route-constraints"></a>Ограничения маршрута

Ограничение маршрута применяет сопоставление типов в сегменте маршрута к компоненту.

В следующем примере маршрут к компоненту `Users` соответствует только в следующих случаях:

* В URL-адресе запроса имеется сегмент маршрута `Id`.
* Сегмент `Id` является целым числом (`int`).

[!code-razor[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Ограничения маршрутов, приведенные в следующей таблице, доступны. Сведения об ограничениях маршрута, соответствующих инвариантному языку и региональным параметрам, см. в предупреждении ниже таблицы для получения дополнительных сведений.

| Ограничение | Пример           | Примеры совпадений                                                                  | Инвариант<br>культура<br>сопоставление |
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

В серверных приложениях Блазор маршрут по умолчанию в *_Host. cshtml* — `/` (`@page "/"`). URL-адрес запроса, содержащий точку (`.`), не соответствует маршруту по умолчанию, так как URL-адрес является запросом файла. Приложение Блазор возвращает *404-не найденный* ответ для статического файла, который не существует. Чтобы использовать маршруты, содержащие точку, настройте *_Host. cshtml* со следующим шаблоном маршрута:

```cshtml
@page "/{**path}"
```

Шаблон `"/{**path}"` включает:

* Двойное звездочка *-все* синтаксис (`**`) для захвата пути в нескольких папках без кодирования косой черты (`/`).
* `path` имя параметра маршрута.

> [!NOTE]
> Синтаксис параметра *Catch-All* (`*`/`**`) **не** поддерживается в компонентах Razor ( *. Razor*).

Для получения дополнительной информации см. <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Компонент Навлинк

Используйте `NavLink` компонент вместо HTML-элементов Hyperlink (`<a>`) при создании навигационных ссылок. `NavLink` компонент ведет себя как элемент `<a>`, за исключением того, что он переключает `active` класс CSS в зависимости от того, соответствует ли его `href` текущему URL-адресу. Класс `active` помогает пользователю понять, какая страница является активной страницей между отображаемыми ссылками навигации.

Следующий `NavMenu` компонент создает панель навигации [начальной загрузки](https://getbootstrap.com/docs/) , которая демонстрирует использование `NavLink` компонентов.

[!code-razor[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Существует два `NavLinkMatch`ых параметра, которые можно назначить атрибуту `Match` элемента `<NavLink>`:

* `NavLinkMatch.All`, &ndash; `NavLink` активна, если он соответствует всему текущему URL-адресу.
* `NavLinkMatch.Prefix` (*по умолчанию*) &ndash; `NavLink` активна, если он соответствует любому префиксу текущего URL-адреса.

В предыдущем примере `href=""` Home `NavLink` соответствует домашнему URL-адресу и получает только `active` класс CSS в URL-адресе базового пути приложения по умолчанию (например, `https://localhost:5001/`). Второй `NavLink` получает класс `active`, когда пользователь посещает любой URL-адрес с префиксом `MyComponent` (например, `https://localhost:5001/MyComponent` и `https://localhost:5001/MyComponent/AnotherSegment`).

Дополнительные `NavLink` атрибуты компонента передаются в отображаемый тег привязки. В следующем примере компонент `NavLink` включает атрибут `target`:

```razor
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Отображается следующая разметка HTML:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>URI и вспомогательные функции состояния навигации

Используйте `Microsoft.AspNetCore.Components.NavigationManager` для работы с URI и навигацией C# в коде. `NavigationManager` предоставляет события и методы, приведенные в следующей таблице.

| Член | Описание |
| ------ | ----------- |
| `Uri` | Возвращает текущий абсолютный URI. |
| `BaseUri` | Получает базовый URI (с завершающей косой чертой), который можно добавить в начало относительных путей URI для получения абсолютного URI. Как правило, `BaseUri` соответствует атрибуту `href` элемента `<base>` документа в *wwwroot/index.HTML* (Blazor сборки) или *pages/_Host. cshtml* (Blazor Server). |
| `NavigateTo` | Переходит по указанному универсальному коду ресурса (URI). Если `forceLoad` `true`:<ul><li>Маршрутизация на стороне клиента обходится.</li><li>Браузер вынужден загрузить новую страницу с сервера, независимо от того, обрабатывается ли универсальный код ресурса клиентским маршрутизатором.</li></ul> |
| `LocationChanged` | Событие, возникающее при изменении расположения навигации. |
| `ToAbsoluteUri` | Преобразует относительный URI в абсолютный URI. |
| `ToBaseRelativePath` | Если указан базовый URI (например, URI, ранее возвращенный `GetBaseUri`), преобразует абсолютный URI в URI относительно базового префикса URI. |

Следующий компонент переходит к компоненту `Counter` приложения при выборе этой кнопки:

```razor
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
