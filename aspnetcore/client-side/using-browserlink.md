---
title: Связь с браузером в ASP.NET Core
author: ncarandini
description: Объясняет, как связь с браузером является функцией Visual Studio, которая связывает среду разработки с одного или нескольких веб-браузеров.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: a75a896dd7ebc488e3e9344ec705c24201924375
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="browser-link-in-aspnet-core"></a>Связь с браузером в ASP.NET Core

По [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), и [Tom Dykstra](https://github.com/tdykstra)

Связь с браузером — это функция в Visual Studio, который создает канал связи между средой разработки и один или несколько веб-браузеры. Можно использовать связь с браузером для обновления веб-приложения в нескольких браузерах одновременно, это полезно для тестирования в различных браузерах.

## <a name="browser-link-setup"></a>Установка связи обозревателя

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x **веб-приложение**, **пустой**, и **веб-API** шаблона проекты, использующие [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) мета пакет, который содержит ссылку на пакет для [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Таким образом, использование `Microsoft.AspNetCore.All` метапакет требует никаких дополнительных действий, чтобы сделать доступными для использования связь с браузером.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.x **веб-приложение** шаблон проекта содержит ссылку на пакет для [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) пакета. **Пустой** или **веб-API** шаблона проектов необходимо добавить ссылку на пакет `Microsoft.VisualStudio.Web.BrowserLink`.

Так как это функция Visual Studio, самым простым способом для добавления пакета **пустой** или **веб-API** шаблона проекта является открытие **консоль диспетчера пакетов** (**Представление** > **другие окна** > **консоль диспетчера пакетов**) и выполните следующую команду:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Кроме того, можно использовать **диспетчера пакетов NuGet**. Щелкните правой кнопкой мыши имя проекта в **обозревателе решений** и выберите **управление пакетами NuGet**:

![Диспетчер пакетов Open NuGet](using-browserlink/_static/open-nuget-package-manager.png)

Найти и установить пакет:

![Добавление пакета с помощью диспетчера пакетов NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a>Конфигурация

В `Configure` метод *файла Startup.cs* файла:

```csharp
app.UseBrowserLink();
```

Обычно код находится внутри `if` блок, позволяющий только связь с браузером в среде разработки, как показано ниже:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Дополнительные сведения см. в разделе [работать с несколькими средами](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Как использовать связь с браузером

При наличии в открытом проекте ASP.NET Core, Visual Studio отображает панели инструментов обозревателя ссылку рядом с **целевой объект отладки** управления панели инструментов:

![Раскрывающееся меню обозревателя ссылки](using-browserlink/_static/browserLink-dropdown-menu.png)

На панели инструментов связь с браузером можно:

* Обновите веб-приложения в нескольких браузерах за один раз.
* Откройте **мониторинга связи с браузером**.
* Включить или отключить **связь с браузером**. Примечание: По умолчанию в Visual Studio 2017 г. (15,3) отключена связь с браузером.
* Включить или отключить [Автосинхронизации CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Некоторые Visual Studio подключаемые модули, прежде всего *2015 пакет расширения Web* и *2017 г. пакет расширения веб*, предоставляют расширенные функциональные возможности для связи с браузером, но некоторые дополнительные функции не работают с ASP. Проекты NET Core.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Обновить веб-приложения в нескольких браузерах за один раз

Чтобы выбрать один веб-браузер для запуска при запуске проекта, используйте раскрывающееся меню в **целевой объект отладки** управления панели инструментов:

![Раскрывающееся меню F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Чтобы одновременно открыть несколько браузеров, выберите **просмотреть с помощью...**  же раскрывающемся списке. Удерживая нажатой клавишу CTRL при выборе в браузерах, требуется и нажмите кнопку **Обзор**:

![Одновременное открытие большинство браузеров](using-browserlink/_static/open-many-browsers-at-once.png)

Ниже приведен на снимке экрана показан откройте Visual Studio с помощью индекса представления и два открытые окна браузера.

![Синхронизация с пример двух браузеров](using-browserlink/_static/sync-with-two-browsers-example.png)

Наведите указатель мыши на панели инструментов связь с браузером для просмотра в браузерах, подключенных к проекту:

![Подсказки при наведении курсора мыши](using-browserlink/_static/hoover-tip.png)

Изменить представление индекса и всех подключенных браузерах обновляются при нажатии кнопки "Обновить" в связи с браузером:

![браузеры синхронизации для изменения](using-browserlink/_static/browsers-sync-to-changes.png)

Связь с браузером также работает с браузерами, запустите из вне Visual Studio и перейдите к URL-адрес приложения.

### <a name="the-browser-link-dashboard"></a>Панели мониторинга связи с браузером

В раскрывающемся меню для управления подключением с браузерами открыть связь с браузером, откройте мониторинга связи с браузером:

![Откройте browserslink панели мониторинга.](using-browserlink/_static/open-browserlink-dashboard.png)

Если браузер не подключен, можно запустить сеанс режима отладки, выбрав *Просмотр в браузере* ссылку:

![browserlink-панели мониторинга нет подключений](using-browserlink/_static/browserlink-dashboard-no-connections.png)

В противном случае — путь к странице, показывающая всеми браузерами, показаны подключенных браузеры:

![browserlink панель мониторинга — два подключения](using-browserlink/_static/browserlink-dashboard-two-connections.png)

При желании можно щелкнуть имя перечисленных браузера, чтобы обновить этот одного браузера.

### <a name="enable-or-disable-browser-link"></a>Включить или отключить связь с браузером

При повторном включении связь с браузером после ее отключения, браузеры для повторного подключения, их необходимо обновить.

### <a name="enable-or-disable-css-auto-sync"></a>Включение и отключение автоматической синхронизации CSS

При включенной автоматической синхронизации CSS подключенных браузеров будут автоматически обновляться при внесении изменений в CSS-файлах.

## <a name="how-does-it-work"></a>Как это работает?

Связь с браузером SignalR использует для создания канала связи между Visual Studio и браузером. Если включена связь с браузером, Visual Studio действует как сервер SignalR, подключенные к нескольким клиентам (браузерах). Связь с браузером также регистрирует компонент по промежуточного слоя в конвейере ASP.NET. Этот компонент вводит специальные `<script>` references в каждом запросе страницы с сервера. При выборе можно просмотреть ссылки на скрипты **Просмотр исходного кода** в браузере и прокрутки к концу `<body>` разметки содержимого:

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Исходные файлы не изменены. Компонент по промежуточного слоя динамически вставляет ссылки на скрипты. 

Поскольку код браузером все JavaScript, он работает во всех браузерах, поддерживаемых SignalR без необходимости подключаемый модуль обозревателя.
