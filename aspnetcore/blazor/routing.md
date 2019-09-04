---
title: ASP.NET Core маршрутизация Блазор
author: guardrex
description: Узнайте, как маршрутизировать запросы в приложениях и о компоненте Навлинк.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: blazor/routing
ms.openlocfilehash: 067dad657c1e89a31fac45fdfa095cce4b10798d
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238061"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core маршрутизация Блазор

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Узнайте, как маршрутизировать запросы и как использовать `NavLink` компонент для создания навигационных ссылок в приложениях блазор.

## <a name="aspnet-core-endpoint-routing-integration"></a>Интеграция маршрутизации конечных точек ASP.NET Core

Серверная часть блазор интегрирована в [ASP.NET Coreную маршрутизацию конечных точек](xref:fundamentals/routing). Приложение ASP.NET Core настроено для приема входящих подключений для интерактивных компонентов с `MapBlazorHub` в `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a>Шаблоны маршрутов

`Router` Компонент включает маршрутизацию, и для каждого доступного компонента предоставляется шаблон маршрута. Компонент отображается в файле *app. Razor:* `Router`

В приложении Блазор на стороне сервера:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

В клиентском приложении Блазор:

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

При компиляции файла *. Razor* с `@page` директивой создается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> класс, в котором указан шаблон маршрута. Во время выполнения маршрутизатор ищет классы компонентов с `RouteAttribute` помощью и отображает компонент с помощью шаблона маршрута, соответствующего запрошенному URL-адресу.

К компоненту можно применить несколько шаблонов маршрутов. Следующий компонент отвечает на запросы `/BlazorRoute` и: `/DifferentBlazorRoute`

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Для правильной генерации маршрутов приложение `<base>` должно включать тег в файл *wwwroot/index.HTML* (на стороне клиента блазор) или *pages/_Host. cshtml* (блазор на стороне сервера) с базовым путем к `href` приложению, указанным в атрибуте ( `<base href="/">`). Дополнительные сведения см. в разделе <xref:host-and-deploy/blazor/client-side#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Указать пользовательское содержимое, когда содержимое не найдено

`Router` Компонент позволяет приложению указать пользовательское содержимое, если содержимое для запрошенного маршрута не найдено.

В файле *app. Razor* задайте пользовательское содержимое в `<NotFoundContent>` элементе `Router` компонента:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <NotFoundContent>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFoundContent>
</Router>
```

Содержимое `<NotFoundContent>` может включать произвольные элементы, например другие интерактивные компоненты.

## <a name="route-parameters"></a>Параметры маршрута

Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента с тем же именем (без учета регистра):

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

Необязательные параметры не поддерживаются для приложений Блазор в предварительной версии ASP.NET Core 3,0. В `@page` предыдущем примере применяются две директивы. Первый позволяет переходить к компоненту без параметра. Вторая `@page` директива `{text}` принимает параметр Route и присваивает значение `Text` свойству.

## <a name="route-constraints"></a>Ограничения маршрута

Ограничение маршрута применяет сопоставление типов в сегменте маршрута к компоненту.

В следующем примере маршрут к `Users` компоненту соответствует только в том случае, если:

* В URL-адресе запроса имеется сегмент маршрута.`Id`
* Сегмент является целым числом (`int`). `Id`

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

В Блазор приложениях на стороне сервера маршрут по умолчанию в *_Host. cshtml* имеет `/` значение`@page "/"`(). URL-адрес запроса, содержащий точку (`.`), не совпадает с маршрутом по умолчанию, так как URL-адрес является запросом файла. Приложение Блазор возвращает *404-не найденный* ответ для статического файла, который не существует. Чтобы использовать маршруты, содержащие точку, настройте *_Host. cshtml* со следующим шаблоном маршрута:

```cshtml
@page "/{**path}"
```

`"/{**path}"` Шаблон включает:

* Двойное звездочка *Catch-All* синтаксис (`**`) для записи пути по нескольким границам папок без кодирования косой черты (`/`).
* Имя `path` параметра маршрута.

Дополнительные сведения см. в разделе <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Компонент Навлинк

Используйте компонент вместо элементов HTML-гиперссылок (`<a>`) при создании навигационных ссылок. `NavLink` Компонент ведет себя `<a>` как элемент, `active` за исключением того, что он переключает класс CSS на основе того, `href` соответствует ли он текущему URL-адресу. `NavLink` `active` Класс помогает пользователю понять, какая страница является активной страницей между отображаемыми ссылками навигации.

Следующий `NavMenu` компонент создает панель навигации [начальной загрузки](https://getbootstrap.com/docs/) , которая демонстрирует использование `NavLink` компонентов.

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

`Match` `NavLinkMatch` Атрибут`<NavLink>` элемента можно назначить двумя способами:

* `NavLinkMatch.All`&ndash; Параметр`NavLink` активен, если он соответствует всему текущему URL-адресу.
* `NavLinkMatch.Prefix`(*по умолчанию*) Параметр активен, `NavLink` если он соответствует любому префиксу текущего URL-адреса. &ndash;

В предыдущем примере домашняя `NavLink` `href=""` страница совпадает с домашним `active` URL-адресом и получает класс CSS только в URL-адресе базового пути по умолчанию `https://localhost:5001/`для приложения (например,). Второй `NavLink` получает класс, `active` когда пользователь `MyComponent` посещает любой URL-адрес с префиксом (например, `https://localhost:5001/MyComponent` и `https://localhost:5001/MyComponent/AnotherSegment`).

Дополнительные `NavLink` атрибуты компонента передаются в отображаемый тег привязки. В следующем примере `NavLink` компонент `target` включает атрибут:

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Отображается следующая разметка HTML:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>URI и вспомогательные функции состояния навигации

Используется `Microsoft.AspNetCore.Components.IUriHelper` для работы с URI и навигацией C# в коде. `IUriHelper`предоставляет события и методы, приведенные в следующей таблице.

| Член | Описание |
| ------ | ----------- |
| `GetAbsoluteUri` | Возвращает текущий абсолютный URI. |
| `GetBaseUri` | Получает базовый URI (с завершающей косой чертой), который можно добавить в начало относительных путей URI для получения абсолютного URI. Как правило `GetBaseUri` , соответствует `href`атрибуту элемента документа в wwwroot/index.HTML (на стороне клиента блазор) или *pages/_Host. cshtml* (блазор на стороне сервера). `<base>` |
| `NavigateTo` | Переходит по указанному универсальному коду ресурса (URI). Если `forceLoad` имеет `true`значение:<ul><li>Маршрутизация на стороне клиента обходится.</li><li>Браузер вынужден загрузить новую страницу с сервера, независимо от того, обрабатывается ли универсальный код ресурса клиентским маршрутизатором.</li></ul> |
| `OnLocationChanged` | Событие, возникающее при изменении расположения навигации. |
| `ToAbsoluteUri` | Преобразует относительный URI в абсолютный URI. |
| `ToBaseRelativePath` | Учитывая базовый URI (например, URI, ранее возвращенный `GetBaseUri`), преобразует абсолютный URI в универсальный код ресурса (URI) по отношению к базовому префиксу URI. |

Следующий компонент переходит к `Counter` компоненту приложения при выборе этой кнопки:

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```

