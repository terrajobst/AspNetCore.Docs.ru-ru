---
title: Ведение журнала и диагностики в ASP.NET Core SignalR
author: anurse
description: Узнайте, как для сбора диагностических данных из приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 02/27/2019
uid: signalr/diagnostics
ms.openlocfilehash: b6bd21314ed183488999bcff3553e53493537a11
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896891"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a>Ведение журнала и диагностики в ASP.NET Core SignalR

По [Andrew Stanton медсестра](https://twitter.com/anurse)

Эта статья содержит рекомендации для сбора диагностики из приложения ASP.NET Core SignalR для устранения проблем.

## <a name="server-side-logging"></a>Ведение журнала на стороне сервера

> [!WARNING]
> Журналы на стороне сервера могут содержать конфиденциальные сведения из вашего приложения. **Никогда не** публиковать необработанные журналы из рабочих приложений на открытых форумах, таких как GitHub.

Поскольку SignalR является частью ASP.NET Core, он использует ASP.NET Core, ведение журнала система. В конфигурации по умолчанию SignalR журналы очень мало сведений, но его можно настроить. См. в документации на [ведение журнала ASP.NET Core](xref:fundamentals/logging/index#configuration) Дополнительные сведения о настройке ведения журнала ASP.NET Core.

SignalR использует две категории средства ведения журнала:

* `Microsoft.AspNetCore.SignalR` — для журналов, связанных с протоколы концентратора, активация концентраторов, вызов методов и других действий, связанных с центром.
* `Microsoft.AspNetCore.Http.Connections` — для журналов, связанных с транспортов, таких как WebSockets, длинный опрос Server-Sent событий и низкоуровневой инфраструктурой SignalR.

Чтобы включить подробные журналы из SignalR, настройте оба выше префиксами `Debug` уровня в вашей `appsettings.json` файл, добавив следующие элементы во `LogLevel` пункту в `Logging`:

[!code-json[Configuring logging](diagnostics/logging-config.json?highlight=7-8)]

Это также можно настроить в коде в вашей `CreateWebHostBuilder` метод:

[!code-csharp[Configuring logging in code](diagnostics/logging-config-code.cs?highlight=5-6)]

Если вы не используете конфигурация на основе JSON, задайте следующие значения конфигурации в системе конфигурации:

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

Обратитесь к документации для вашей системы конфигурации определить способ указания значений вложенной конфигурации. Например, при использовании переменных среды два `_` символы используются вместо `:` (например: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).

Мы рекомендуем использовать `Debug` уровня при сборе более подробные диагностики для приложения. `Trace` Уровень создает очень низкого уровня диагностики и редко требуется для диагностики проблем в приложении.

## <a name="access-server-side-logs"></a>Журналы событий на стороне сервера

Способ доступа к серверных журналов зависит от среды, в которой вы работаете.

### <a name="as-a-console-app-outside-iis"></a>Консольного приложения за пределами IIS

Если у вас в консольном приложении, [ведения журнала консоли](xref:fundamentals/logging/index#console-provider) должен быть включен по умолчанию. SignalR журналы будут отображаться в консоли.

### <a name="within-iis-express-from-visual-studio"></a>В IIS Express из Visual Studio

Visual Studio отображает выходные данные журнала в **вывода** окна. Выберите **веб-сервера ASP.NET Core** раскрывающийся список параметра.

### <a name="azure-app-service"></a>Служба приложений Azure

Включите параметр «Ведение журнала приложения (файловая система)» в разделе «Журналы диагностики» на портале службы приложений Azure и настроить уровень `Verbose`. Журналы должны быть доступны из «Потоковая передача журналов» службы, а также, как и в журналы в файловой системе приложения службы. Дополнительные сведения см. в документации на [потоковая передача журналов Azure](xref:fundamentals/logging/index#azure-log-streaming).

### <a name="other-environments"></a>Другие среды

Если у вас в другой среде (Docker, Kubernetes, служба Windows, и т.д.), см. в полной документации на [ведения журнала ASP.NET Core](xref:fundamentals/logging/index) Дополнительные сведения о том, как настроить поставщики ведения журналов, подходящий для вашей среды.

## <a name="javascript-client-logging"></a>Ведение журнала клиента JavaScript

> [!WARNING]
> Журналы на стороне клиента могут содержать конфиденциальные сведения из вашего приложения. **Никогда не** публиковать необработанные журналы из рабочих приложений на открытых форумах, таких как GitHub.

Если вы используете клиент JavaScript, можно настроить параметры ведения журнала с помощью `configureLogging` метод `HubConnectionBuilder`:

[!code-javascript[Configuring logging in the JavaScript client](diagnostics/logging-config-js.js?highlight=3)]

Чтобы отключить ведение журнала полностью, укажите `signalR.LogLevel.None` в `configureLogging` метод.

Ниже приведены доступные уровни ведения журнала для клиента JavaScript. Установка уровня журнала в одно из следующих значений включающем ведение журнала на этом уровне и все вышестоящие уровни в таблице.

| Уровень | Описание |
| ----- | ----------- |
| `None` | Сообщения не регистрируются. |
| `Critical` | Сообщения, которые означают сбой всего приложения. |
| `Error` | Сообщения, которые указывают на сбой в текущей операции. |
| `Warning` | Сообщения, которые указывают на проблему, не являющиеся неустранимыми. |
| `Information` | Информационные сообщения. |
| `Debug` | Диагностические сообщения, полезны для отладки. |
| `Trace` | Очень подробные диагностические сообщения, предназначены для диагностики конкретных проблем. |

После настройки уровень детализации, будут записаны журналы консоли браузера (или стандартный вывод в приложении NodeJS).

Если вы хотите отправить журналы системы настраиваемого протоколирования, можно предоставить объект JavaScript, реализующий интерфейс `ILogger` интерфейс. Является единственным способом, который должен быть реализован `log`, который принимает уровень события и сообщение, связанное с событием. Пример:

[!code-typescript[Creating a custom logger](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a>Ведение журнала клиента .NET

> [!WARNING]
> Журналы на стороне клиента могут содержать конфиденциальные сведения из вашего приложения. **Никогда не** публиковать необработанные журналы из рабочих приложений на открытых форумах, таких как GitHub.

Чтобы получить журналы из клиента .NET, можно использовать `ConfigureLogging` метод `HubConnectionBuilder`. Это работает так же, как `ConfigureLogging` метод `WebHostBuilder` и `HostBuilder`. Можно настроить те же поставщики ведения журналов, используемых в ASP.NET Core. Тем не менее необходимо вручную установить и включить пакеты NuGet для отдельных регистраторов.

### <a name="console-logging"></a>Журнал консоли

Чтобы включить ведение журнала консоли, добавьте [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) пакета. Затем с помощью `AddConsole` метод, чтобы настроить средство ведения журнала консоли:

[!code-csharp[Configuring console logging in .NET client](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a>Ведение журнала выходных данных окна отладки

Можно также настроить журналы, чтобы перейти к **вывода** окно в Visual Studio. Установка [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) упаковки и использовать `AddDebug` метод:

[!code-csharp[Configuring debug output window logging in .NET client](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a>Другие поставщики ведения журналов

SignalR поддерживает другие поставщики ведения журналов, например Serilog, Seq, NLog или любая другая система ведения журнала, который интегрируется с `Microsoft.Extensions.Logging`. Если система ведения журнала предоставляет `ILoggerProvider`, его можно зарегистрировать с помощью `AddProvider`:

[!code-csharp[Configuring a custom logging provider in .NET client](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a>Уровень детализации для элемента управления

При входе в систему из других мест в приложении изменение уровня по умолчанию для `Debug` может быть слишком verbose. Фильтр можно использовать для настройки уровня ведения журнала для журналов SignalR. Это можно сделать в коде, во многом так же, как на сервере:

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a>Данные трассировки сети

> [!WARNING]
> Трассировка сети содержит все содержимое каждого сообщения, отправленные приложением. **Никогда не** публиковать необработанные сети трассировок из рабочих приложений на открытых форумах, таких как GitHub.

Если возникла проблема, сетевой трассировки иногда можно предоставить массу полезной информации. Это особенно полезно в том случае, если вы собираетесь сообщите о проблеме на средства отслеживания проблем.

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a>Сбор трассировки сети с помощью Fiddler (предпочтительный вариант)

Этот метод работает для всех приложений.

Fiddler является очень мощным инструментом для сбора трассировок HTTP. Установите его из [telerik.com/fiddler](https://www.telerik.com/fiddler), запустите его и затем запустите приложение и воспроизведите проблему. Fiddler — для Windows и существует бета-версии для macOS и Linux.

При подключении по протоколу HTTPS, существуют некоторые дополнительные действия, чтобы убедиться, что Fiddler может расшифровать трафик HTTPS. Дополнительные сведения см. в разделе [документации Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).

После сбора данных трассировки, вы можете экспортировать трассировки, выбрав **файл** > **Сохранить** > **все сеансы...**  в строке меню.

![Экспорт всех сеансов с Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a>Сбор трассировки сети с помощью tcpdump (macOS и Linux только)

Этот метод работает для всех приложений.

Можно собирать трассировки необработанные TCP, с помощью tcpdump, выполнив следующую команду из командной оболочки. Может потребоваться `root` или префикса командой `sudo` появление ошибки разрешений:

```console
tcpdump -i [interface] -w trace.pcap
```

Замените `[interface]` с сетевым интерфейсом, вы хотите записать на. Обычно это что-нибудь вроде `/dev/eth0` (для стандартного интерфейса Ethernet) или `/dev/lo0` (для трафика localhost). Дополнительные сведения см. в разделе `tcpdump` справочную страницу в системе узла.

## <a name="collect-a-network-trace-in-the-browser"></a>Сбор трассировки сети в браузере

Этот метод работает только для браузерных приложений.

Большинство средств разработчика браузера имеет вкладку «Network», которая позволяет записывать сетевой активности между браузером и сервером. Тем не менее эти трассировки не включать в себя WebSocket и Server-Sent сообщения о событиях. Если вы используете эти транспорты, с помощью такого средства, как Fiddler или TcpDump (как описано ниже) подход лучше.

### <a name="microsoft-edge-and-internet-explorer"></a>Microsoft Edge и Internet Explorer

(Инструкции одинаковы для Edge и Internet Explorer)

1. Нажмите клавишу F12, чтобы открыть средства разработчика
2. Щелкните вкладку "Сеть"
3. Обновите страницу, (при необходимости) и воспроизвести проблему
4. Щелкните значок "Сохранить" в панели инструментов, чтобы экспортировать трассировки в формате «HAR»:

![Сохранить значок в Microsoft Edge разработки средств вкладку "Сеть"](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a>Google Chrome

1. Нажмите клавишу F12, чтобы открыть средства разработчика
2. Щелкните вкладку "Сеть"
3. Обновите страницу, (при необходимости) и воспроизвести проблему
4. Щелкните правой кнопкой мыши список запросов и выберите «Сохранить как HAR с содержимым»:

![Параметр «Сохранить как HAR с содержимым» в Google Chrome Dev Tools вкладку "Сеть"](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

1. Нажмите клавишу F12, чтобы открыть средства разработчика
2. Щелкните вкладку "Сеть"
3. Обновите страницу, (при необходимости) и воспроизвести проблему
4. Щелкните правой кнопкой мыши список запросов и выберите команду «Сохранить все как HAR»

![Параметр «Сохранить все как HAR» в Mozilla Firefox разработки средств вкладку "Сеть"](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a>Вложения файлов диагностики на проблемы GitHub

Можно также присоединить файлы диагностики на проблемы GitHub, переименовав их, чтобы они имели `.txt` расширения и затем перетащите ее на проблему.

> [!NOTE]
> Пожалуйста не вставьте содержимое файлов журнала и трассировки сети на сайте github. Эти журналы и трассировки могут быть довольно большими, и GitHub обычно удалит их.

![Перетаскивание файлов журнала на вопроса в GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
