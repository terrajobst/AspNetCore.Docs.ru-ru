---
title: Конфигурация модели размещения ASP.NET Core Blazor
author: guardrex
description: Сведения о конфигурации модели размещения Blazor, в том числе о том, как интегрировать компоненты Razor в Razor Pages и приложения MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: bd44643877e45c5b48b0972bcc2f637fbc5d98f2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78646792"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>Конфигурация модели размещения ASP.NET Core Blazor

Автор: [Дэниэл Рот](https://github.com/danroth27) (Daniel Roth)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

В этой статье рассматривается конфигурация модели размещения.

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a>Blazor Server

### <a name="reflect-the-connection-state-in-the-ui"></a>Отражение состояния соединения в пользовательском интерфейсе

Когда клиент обнаруживает, что соединение было потеряно, пользовательский интерфейс по умолчанию отображается пользователю, в то время как клиент пытается восстановить соединение. Если происходит повторный сбой подключения, пользователю предоставляется возможность повторить попытку.

Чтобы настроить пользовательский интерфейс, определите элемент с `id` равным `components-reconnect-modal` в блоке `<body>` страницы Razor *__Host.cshtml*.

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

В следующей таблице описаны классы CSS, применяемые к элементу `components-reconnect-modal`.

| Класс CSS                       | Означает&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | Потеря соединения. Клиент пытается повторно подключиться. Отобразить модальное окно. |
| `components-reconnect-hide`     | Активное подключение к серверу восстановлено. Скрыть модальное окно. |
| `components-reconnect-failed`   | Сбой повторного подключения, возможно, из-за сбоя сети. Чтобы повторить попытку подключения, вызовите метод `window.Blazor.reconnect()`. |
| `components-reconnect-rejected` | Повторное подключение отклонено. Сервер был достигнут, но отклонил подключение, и состояние пользователя на сервере потеряно. Чтобы перезагрузить приложение, вызовите метод `location.reload()`. Это состояние соединения может появиться в следующих случаях:<ul><li>произошел сбой в цепи на стороне сервера;</li><li>клиент был отключен достаточно долго, чтобы сервер сбросил состояние пользователя. Экземпляры компонентов, с которыми взаимодействовал пользователь, удаляются;</li><li>сервер был перезагружен или был выполнен перезапуск рабочего процесса приложения.</li></ul> |

### <a name="render-mode"></a>Режим обработки

Приложения Blazor Server по умолчанию настроены на предварительную отрисовку пользовательского интерфейса на сервере, прежде чем будет установлено клиентское соединение с сервером. Это поведение настраивается на странице Razor *_Host.cshtml*.

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

Параметр `RenderMode` настраивает одно из следующих поведений компонента:

* компонент предварительно преобразуется в страницу;
* компонент отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Blazor из агента пользователя.

| `RenderMode`        | Описание |
| ------------------- | ----------- |
| `ServerPrerendered` | Преобразует компонент в статический HTML и включает метку приложения Blazor Server. При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor. |
| `Server`            | Отображает метку приложения Blazor Server. Выходные данные компонента не включаются. При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor. |
| `Static`            | Преобразует компонент в статический HTML. |

Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Отображение интерактивных компонентов с отслеживанием состояния из страниц и представлений Razor

На страницу или в представление Razor можно добавить интерактивные компоненты с отслеживанием состояния.

При отображении страницы или представления:

* компонент предварительно отображается страницей или представлением;
* исходное состояние компонента, используемое для предварительной визуализации, теряется;
* новое состояние компонента создается при установке подключения SignalR.

Следующая страница Razor визуализирует компонент `Counter`.

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Отображение неинтерактивных компонентов из страниц и представлений Razor

На следующей странице Razor компонент `Counter` статически подготавливается к просмотру с начальным значением, указанным с помощью формы.

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

Поскольку `MyComponent` отображается статически, компонент не может быть интерактивным.

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>Настройка клиента SignalR для приложений Blazor Server

Иногда необходимо настроить клиент SignalR, используемый приложениями Blazor Server. Например, может потребоваться настроить ведение журнала на клиенте SignalR для диагностики проблемы с подключением.

Чтобы настроить клиент SignalR, внесите следующие изменения в файле *Pages/_Host.cshtml*:

* добавьте атрибут `autostart="false"` в тег `<script>` для сценария `blazor.server.js`;
* вызовите `Blazor.start` и передайте объект конфигурации, указывающий построитель SignalR.

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```
