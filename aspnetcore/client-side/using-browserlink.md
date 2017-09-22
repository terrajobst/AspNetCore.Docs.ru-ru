---
title: "Связь с браузером в ASP.NET Core"
author: ncarandini
description: "Функцию Visual Studio, которая связывает среду разработки с одного или нескольких веб-браузеров"
keywords: "ASP.NET Core, связь с браузером, синхронизации CSS"
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 211dd5d03e6b8414e0b2ed3234d8970c92f72452
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a>Введение в связи с браузером в ASP.NET Core 

По [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), и [Tom Dykstra](https://github.com/tdykstra)

Связь с браузером — это функция в Visual Studio, который создает канал связи между средой разработки и один или несколько веб-браузеры. Можно использовать связь с браузером для обновления веб-приложения в нескольких браузерах одновременно, это полезно для тестирования в различных браузерах.

## <a name="browser-link-setup"></a>Установка связи обозревателя

ASP.NET Core **веб-приложение** шаблонов в Visual Studio 2015 и более поздних версий, имеют все необходимое для связи с браузером.

Чтобы добавить в проект, созданный с помощью ASP.NET Core связь с браузером **пустой** или **веб-API** шаблона, выполните следующие действия:

1. Добавить *Microsoft.VisualStudio.Web.BrowserLink.Loader* пакета 
2. Добавьте в код конфигурации в *файла Startup.cs* файл.

### <a name="add-the-package"></a>Добавление пакета

Поскольку компоненты Visual Studio, самым простым способом добавления пакета — для открытия **консоль диспетчера пакетов** (**представление > Другие окна > консоль диспетчера пакетов**) и выполните следующую команду:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

Кроме того, можно использовать **диспетчера пакетов NuGet**.  Щелкните правой кнопкой мыши имя проекта в **обозревателе решений**и выберите **управление пакетами NuGet**. 

![Диспетчер пакетов Open NuGet](using-browserlink/_static/open-nuget-package-manager.png)

Затем найдите и установки пакета.

![Добавление пакета с помощью диспетчера пакетов NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a>Добавьте в код конфигурации

Откройте *файла Startup.cs* файл и в `Configure` метод добавьте следующий код:

```csharp
app.UseBrowserLink();
```

Обычно этот код находится внутри `if` блока, который обеспечивает связь с браузером только в среде разработки, как показано ниже:

[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]

Дополнительные сведения см. в статье [Работа с несколькими средами](../fundamentals/environments.md).

## <a name="how-to-use-browser-link"></a>Как использовать связь с браузером

При наличии в открытом проекте ASP.NET Core, Visual Studio отображает панели инструментов обозревателя ссылку рядом с **целевой объект отладки** управления панели инструментов:

![Раскрывающееся меню обозревателя ссылки](using-browserlink/_static/browserLink-dropdown-menu.png)

На панели инструментов связь с браузером можно:

- Обновить веб-приложения в нескольких браузерах за один раз
- Откройте **мониторинга связи с браузером**
- Включить или отключить **связь с браузером**
- Включение и отключение автоматической синхронизации CSS

> [!NOTE]
> Некоторые Visual Studio подключаемые модули, прежде всего *2015 пакет расширения Web* и *2017 г. пакет расширения веб*, предоставляют расширенные функциональные возможности для связи с браузером, но некоторые дополнительные функции не работают с ASP. Проекты NET Core.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Обновить веб-приложения в нескольких браузерах за один раз

Чтобы выбрать один веб-браузер для запуска при запуске проекта, используйте раскрывающееся меню в **целевой объект отладки** управления панели инструментов:

![Раскрывающееся меню F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Чтобы одновременно открыть несколько браузеров, выберите **просмотреть с помощью... ** же раскрывающемся списке.  Удерживая нажатой клавишу CTRL при выборе в браузерах, требуется и нажмите кнопку **Обзор**:

![Одновременное открытие большинство браузеров](using-browserlink/_static/open-many-browsers-at-once.png)

Ниже приведен пример снимка экрана отображение откройте Visual Studio с представление Index и два открытые окна браузера.

![Синхронизация с пример двух браузеров](using-browserlink/_static/sync-with-two-browsers-example.png)

Наведите указатель мыши на панели инструментов связь с браузером для просмотра в браузерах, подключенных к проекту:

![Подсказки при наведении курсора мыши](using-browserlink/_static/hoover-tip.png)

Изменить представление индекса и всех подключенных браузерах обновляются при нажатии кнопки "Обновить" в связи с браузером:

![браузеры синхронизации для изменения](using-browserlink/_static/browsers-sync-to-changes.png)

Связь с браузером также работает с браузерами, запустите из вне Visual Studio и перейдите к URL-адрес приложения.

### <a name="the-browser-link-dashboard"></a>Панели мониторинга связи с браузером

В раскрывающемся меню для управления подключением с браузерами открыть связь с браузером, откройте мониторинга связи с браузером:

![Откройте browserslink панели мониторинга.](using-browserlink/_static/open-browserlink-dashboard.png)

Если браузер не подключен, можно запустить сеанс отладки не щелкнув _Просмотр в браузере_ ссылку:

![browserlink-панели мониторинга нет подключений](using-browserlink/_static/browserlink-dashboard-no-connections.png)

В противном случае подключенной браузеры отображаются со путь к странице, отображение каждого браузера:

![browserlink панель мониторинга — два подключения](using-browserlink/_static/browserlink-dashboard-two-connections.png)

При желании можно щелкнуть имя перечисленных браузера, чтобы обновить этот одного браузера.

### <a name="enable-or-disable-browser-link"></a>Включить или отключить связь с браузером

При повторном включении связь с браузером после ее отключения, необходимо обновить в браузерах для повторного подключения их.

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

Исходные файлы не изменяются. Компонент по промежуточного слоя динамически вставляет ссылки на скрипты. 

Поскольку код браузером все JavaScript, он работает во всех браузерах, поддерживаемых SignalR, не требуя подключаемый модуль обозревателя.
