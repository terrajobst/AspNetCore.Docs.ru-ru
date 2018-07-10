---
title: Связь с браузером в ASP.NET Core
author: ncarandini
description: Объясняет, как связь с браузером — это компонент Visual Studio, который связывает среде разработки с помощью одного или нескольких веб-браузеров.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 5ab15c841c472e6c9d47bad70fcf5e0c6dc3010f
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894183"
---
# <a name="browser-link-in-aspnet-core"></a>Связь с браузером в ASP.NET Core

По [Nicolò Carandini](https://github.com/ncarandini), [Майк Уоссон](https://github.com/MikeWasson), и [том Дайкстра](https://github.com/tdykstra)

Связь с браузером — это функция в Visual Studio, который создает канал взаимодействия между средой разработки и один или несколько веб-браузеров. Можно использовать связь с браузером для обновления веб-приложения в нескольких браузерах одновременно, что полезно для тестирования обозреватели.

## <a name="browser-link-setup"></a>Установка связи обозревателя

::: moniker range=">= aspnetcore-2.1"

При преобразовании в проект ASP.NET Core 2.0 в ASP.NET Core 2.1 и переход к [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), установить [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) пакета для Функциональные возможности BrowserLink. Используйте шаблоны проектов ASP.NET Core 2.1 `Microsoft.AspNetCore.App` метапакет по умолчанию.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 **веб-приложение**, **пустой**, и **веб-API** проекта используйте шаблоны [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) , который содержит ссылку на пакет для [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Таким образом, использование `Microsoft.AspNetCore.All` метапакет не требуется никаких дополнительных действий, чтобы сделать доступными для использования связь с браузером.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

ASP.NET Core 1.x **веб-приложение** шаблон проекта содержит ссылку на пакет для [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) пакета. **Пустой** или **веб-API** шаблона проектов требуется добавить ссылки на пакет `Microsoft.VisualStudio.Web.BrowserLink`.

Так как эта функция Visual Studio, самым простым способом для добавления пакета **пустой** или **веб-API** проекта шаблона является открытие **консоль диспетчера пакетов** (**Представление** > **Other Windows** > **консоль диспетчера пакетов**) и выполните следующую команду:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Кроме того, можно использовать **диспетчер пакетов NuGet**. Щелкните правой кнопкой мыши имя проекта в **обозревателе решений** и выберите **управление пакетами NuGet**:

![Диспетчер пакетов NuGet, откройте](using-browserlink/_static/open-nuget-package-manager.png)

Найдите и установите пакет:

![Добавление пакета с помощью диспетчера пакетов NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>Конфигурация

В методе `Startup.Configure`:

```csharp
app.UseBrowserLink();
```

Обычно код находится внутри `if` блок, который обеспечивает только связь с браузером в среде разработки, как показано ниже:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Как использовать связь с браузером

При наличии в открытом проекте ASP.NET Core, Visual Studio отображает панели инструментов браузера ссылку рядом с полем **целевой объект отладки** элемент управления панели инструментов:

![Раскрывающееся меню браузера ссылку](using-browserlink/_static/browserLink-dropdown-menu.png)

На панели инструментов привязывание к браузеру вы можете:

* Обновите веб-приложения в нескольких браузерах за один раз.
* Откройте **мониторинга связи с браузером**.
* Включение или отключение **связь с браузером**. Примечание: Привязывание к браузеру отключен по умолчанию в Visual Studio 2017 (версии 15.3).
* Включение или отключение [Автосинхронизация CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Некоторые Visual Studio подключаемые модули, особенно *2015 пакет расширения веб* и *2017 пакет расширения Web*, предлагают расширенные функциональные возможности для связи с браузером, но некоторые дополнительные функции, не работают с ASP. Проекты .NET Core.

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Обновить веб-приложения в нескольких браузерах за один раз

Чтобы выбрать один веб-браузер для запуска при запуске проекта, используйте раскрывающееся меню в **целевой объект отладки** элемент управления панели инструментов:

![Выберите в раскрывающемся меню F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Чтобы одновременно открыть несколько браузеров, выберите **просмотр с помощью...**  же раскрывающемся списке. Удерживая нажатой клавишу CTRL, чтобы выберите браузеры, требуется и нажмите кнопку **Обзор**:

![За один раз открыть Многие браузеры](using-browserlink/_static/open-many-browsers-at-once.png)

Ниже приведен на снимке экрана показан откройте Visual Studio в представлении индекса и две открытые окна браузера.

![Синхронизация с двумя браузеры примера](using-browserlink/_static/sync-with-two-browsers-example.png)

Наведите указатель мыши на панели инструментов привязывание к браузеру для браузеров, которые подключены к проекту см. в разделе:

![Подсказки при наведении курсора мыши](using-browserlink/_static/hoover-tip.png)

Изменение представления индекса, и все подключенные браузеры обновляются при нажатии кнопки обновления привязывание к браузеру:

![браузеры синхронизации для изменения](using-browserlink/_static/browsers-sync-to-changes.png)

Связь с браузером также работает с браузерами, запустите из вне Visual Studio и перейдите к URL-адрес приложения.

### <a name="the-browser-link-dashboard"></a>Мониторинга связи с браузером

Откройте мониторинга связи с браузером в раскрывающемся меню управлять взаимодействием с открытым браузеры привязывание к браузеру:

![открыть browserslink панели мониторинга.](using-browserlink/_static/open-browserlink-dashboard.png)

Если браузер не подключен, можете начать сеанс без отладки, выбрав *просмотреть в браузере* ссылку:

![browserlink-панели мониторинга нет подключений](using-browserlink/_static/browserlink-dashboard-no-connections.png)

В противном случае подключенные браузеры, показаны на путь к странице, на которой отображаются каждым браузером.

![browserlink-панель мониторинга — двух подключений](using-browserlink/_static/browserlink-dashboard-two-connections.png)

При желании можно щелкнуть имя перечисленных браузера, чтобы обновить этот одного браузера.

### <a name="enable-or-disable-browser-link"></a>Включить или отключить связь с браузером

При повторном включении связь с браузером после ее отключения, браузеры для повторного подключения, их необходимо обновить.

### <a name="enable-or-disable-css-auto-sync"></a>Включить или отключить автоматическая синхронизация CSS

Если включена автоматическая синхронизация CSS, подключенные браузеры обновляются автоматически при любом изменении файлов CSS.

## <a name="how-it-works"></a>Принцип работы

Связь с браузером использует SignalR для создания коммуникационного канала между Visual Studio и браузером. При включении связь с браузером, Visual Studio выступает в качестве сервера SignalR, несколько клиентов (обозревателей) можно подключиться. Связь с браузером также регистрирует компонент по промежуточного слоя в конвейере запросов ASP.NET. Этот компонент вводит специальные `<script>` ссылок в каждом запросе страницы с сервера. Вы увидите ссылки на скрипты, выбрав **Просмотр исходного кода** в браузере и прокрутка в конец `<body>` отметить содержимое:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Исходные файлы не изменяются. Компонент по промежуточного слоя динамически внедряет ссылок на скрипты.

Так как код на стороне обозревателя все JavaScript, он работает во всех браузерах, поддерживаемых SignalR, не требуя подключаемый модуль обозревателя.
