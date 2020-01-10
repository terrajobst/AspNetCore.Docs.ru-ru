---
title: Ссылка на браузер в ASP.NET Core
author: ncarandini
description: Объясняется, как связь с браузером является компонентом Visual Studio, который связывает среду разработки с одним или несколькими веб-браузерами.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/09/2020
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: 19cc3c2ed91bd9e05df3c036123c78ecbf81fcc0
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828274"
---
# <a name="browser-link-in-aspnet-core"></a>Ссылка на браузер в ASP.NET Core

[Николò карандини](https://github.com/ncarandini), [Mike Уоссон](https://github.com/MikeWasson)и [Tom Dykstra)](https://github.com/tdykstra)

Ссылка на браузер является компонентом Visual Studio. Он создает коммуникационный канал между средой разработки и одним или несколькими веб-браузерами. Можно использовать ссылку на браузер для обновления веб-приложения одновременно в нескольких браузерах, что полезно для тестирования в разных браузерах.

## <a name="browser-link-setup"></a>Настройка связи с браузером

::: moniker range=">= aspnetcore-3.0"

Добавьте в проект пакет [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) . Для ASP.NET Core проектов Razor Pages или MVC также включите компиляцию файлов Razor ( *. cshtml*) во время выполнения, как описано в разделе <xref:mvc/views/view-compilation>. Синтаксис Razor изменения применяются только при включенной компиляции среды выполнения.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

При преобразовании проекта ASP.NET Core 2,0 в ASP.NET Core 2,1 и переходе к [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app)установите пакет [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) для функции связи с браузером. Шаблоны проектов ASP.NET Core 2,1 по умолчанию используют `Microsoft.AspNetCore.App` метапакет.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Шаблоны проектов **веб-приложений**ASP.NET Core 2,0, **Empty**и **Web API** используют объект [Microsoft. AspNetCore. ALL метапакет](xref:fundamentals/metapackage), содержащий ссылку на пакет для [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Поэтому использование `Microsoft.AspNetCore.All` метапакет не требует дальнейших действий, чтобы сделать связь с браузером доступной для использования.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Шаблон проекта **веб-приложения** ASP.NET Core 1. x содержит ссылку на пакет для пакета [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) . Для других типов проектов требуется добавить ссылку на пакет `Microsoft.VisualStudio.Web.BrowserLink`.

::: moniker-end

### <a name="configuration"></a>Конфигурация

Вызовите `Startup.Configure` в методе `UseBrowserLink`:

```csharp
app.UseBrowserLink();
```

Вызов `UseBrowserLink` обычно размещается в блоке `if`, который разрешает только связь с браузером в среде разработки. Например:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Для получения дополнительной информации см. <xref:fundamentals/environments>.

## <a name="how-to-use-browser-link"></a>Как использовать связь с браузером

При открытии проекта ASP.NET Core в Visual Studio отображается элемент управления панели инструментов ссылки на браузер рядом с элементом управления панели инструментов **отладки Target** :

![Раскрывающееся меню связи с браузером](using-browserlink/_static/browserLink-dropdown-menu.png)

С помощью элемента управления панели инструментов связи в браузере можно:

* Одновременное обновление веб-приложения в нескольких браузерах.
* Откройте **панель мониторинга связи с браузером**.
* Включение или отключение **связи с браузером**. Примечание. ссылка на браузер отключена по умолчанию в Visual Studio.
* Включение или отключение [автоматической синхронизации CSS](#enable-or-disable-css-auto-sync).

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Обновление веб-приложения в нескольких браузерах одновременно

Чтобы выбрать один веб-браузер для запуска при запуске проекта, используйте раскрывающееся меню в элементе управления на панели инструментов **отладки** .

![Раскрывающееся меню F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Чтобы открыть сразу несколько браузеров, нажмите кнопку **Обзор с помощью...** в одном раскрывающемся списке. Удерживая нажатой клавишу <kbd>CTRL</kbd> , выберите нужные браузеры и нажмите кнопку **Обзор**:

![Одновременное открытие нескольких браузеров](using-browserlink/_static/open-many-browsers-at-once.png)

На следующем снимке экрана показана Visual Studio с открытым представлением индекса и двумя открытыми браузерами:

![Пример синхронизации с двумя браузерами](using-browserlink/_static/sync-with-two-browsers-example.png)

Наведите указатель мыши на элемент управления ToolBar Link (ссылка на браузер), чтобы просмотреть браузеры, подключенные к проекту:

![Подсказка при наведении](using-browserlink/_static/hoover-tip.png)

Измените представление индекса, и все подключенные браузеры будут обновлены при нажатии кнопки Обновить связь с браузером:

![браузеры — синхронизация с изменениями](using-browserlink/_static/browsers-sync-to-changes.png)

Связь с браузером также работает с браузерами, запускаемыми извне Visual Studio, и переходить по URL-адресу приложения.

### <a name="the-browser-link-dashboard"></a>Панель мониторинга связи с браузером

Откройте окно **панели мониторинга связи с браузером** в раскрывающемся меню ссылки браузера, чтобы управлять подключением с помощью открытых браузеров:

![Open-бровсерслинк-Dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

Если ни один браузер не подключен, можно запустить сеанс без отладки, выбрав ссылку **Просмотреть в браузере** :

![browserlink-Dashboard-нет-подключения](using-browserlink/_static/browserlink-dashboard-no-connections.png)

В противном случае отображаются подключенные браузеры с указанием пути к странице, отображаемой в каждом браузере:

![browserlink-Dashboard-два подключения](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Можно также щелкнуть имя отдельного браузера, чтобы обновить только этот браузер.

### <a name="enable-or-disable-browser-link"></a>Включение или отключение связи с браузером

При повторном включении связи с браузером после ее отключения необходимо обновить браузеры, чтобы повторно подключить их.

### <a name="enable-or-disable-css-auto-sync"></a>Включение или отключение автоматической синхронизации CSS

При включении автоматической синхронизации CSS подключенные браузеры автоматически обновляются при внесении любых изменений в файлы CSS.

## <a name="how-it-works"></a>Как это работает

Ссылка на браузер использует [SignalR](xref:signalr/introduction) для создания коммуникационного канала между Visual Studio и браузером. Если связь с браузером включена, Visual Studio выступает в качестве SignalR сервера, к которому могут подключаться несколько клиентов (браузеров). Связь с браузером также регистрирует компонент по промежуточного слоя в конвейере запросов ASP.NET Core. Этот компонент внедряет специальные ссылки `<script>` в каждый запрос страницы с сервера. Чтобы просмотреть ссылки на скрипты, выберите в браузере пункт **Просмотреть источник** и Прокрутите содержимое тега до конца `<body>`:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Исходные файлы не изменяются. Компонент по промежуточного слоя динамически вставляет ссылки на скрипты.

Так как код на стороне браузера является всего лишь JavaScript, он работает во всех браузерах, которые SignalR поддерживаются без использования подключаемого модуля браузера.
