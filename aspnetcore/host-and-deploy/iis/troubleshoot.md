---
title: Устранение неполадок ASP.NET Core в службах IIS
author: guardrex
description: Сведения о диагностике проблем с развертываниями приложений ASP.NET Core на платформе IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/28/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: cb42a262c89c27fa350e936184f8ddb3a02788f0
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034739"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Устранение неполадок ASP.NET Core в службах IIS

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Эта статья содержит инструкции по диагностике проблемы запуска приложения ASP.NET Core, размещенного в [службах IIS](/iis). Информация в этой статье относится к размещению в службах IIS на Windows Server и Windows Desktop.

::: moniker range=">= aspnetcore-2.2"

В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки. Рекомендации в этой статье позволяют устранять неполадки, проявляющиеся как *502.5 — ошибка процесса* или *500.30 — ошибка запуска* при локальной отладке.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

В Visual Studio проект ASP.NET Core по умолчанию использует размещение в [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) на период отладки. Рекомендации в этой статье позволяют устранять неполадки, проявляющиеся как *сбой процесса 502.5* при локальной отладке.

::: moniker-end

Дополнительные статьи по устранению неполадок:

<xref:host-and-deploy/azure-apps/troubleshoot> Служба приложений использует для размещения приложений [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) и IIS. Инструкции по использованию Службы приложений см. в посвященных ей статьях.

<xref:fundamentals/error-handling> — узнайте, как обрабатывать ошибки в приложениях ASP.NET Core при разработке в локальной системе.

[Знакомство с отладчиком Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger). В этой статье описаны возможности отладчика Visual Studio.

[Статья об отладке с помощью Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging). Узнайте о поддержке отладки, встроенной в Visual Studio Code.

## <a name="app-startup-errors"></a>Ошибки при запуске приложения

### <a name="5025-process-failure"></a>502.5 Process Failure (ошибка процесса)

Рабочий процесс завершается ошибкой. Приложение не запускается.

Модуль ASP.NET Core пытается запустить процесс dotnet серверной части, но он не запускается. Обычно причину сбоя при запуске процесса можно определить по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).

Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core. Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере. *Общая платформа* — это набор сборок (*DLLl*-файлы), которые установлены на компьютере и на которые ссылается метапакет, например `Microsoft.AspNetCore.App`. В ссылках метапакета может быть указана минимальная требуемая версия. Дополнительную информацию см. в этой публикации об [общей платформе](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

Когда ошибки в конфигурации размещения или приложения приводят к сбою рабочего процесса, возвращается страница ошибки *502.5 — ошибка процесса*.

![Окно браузера со страницей "502.5 — ошибка процесса"](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a>500.30 In-Process Startup Failure (ошибка внутрипроцессного запуска)

Рабочий процесс завершается ошибкой. Приложение не запускается.

Модуль ASP.NET Core пытается запустить среду CLR .NET Core внутри процесса, но она не запускается. Обычно причину сбоя при запуске процесса можно определить по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).

Распространенной причиной сбоя является неправильная настройка приложения с выбором отсутствующей версии общей платформы ASP.NET Core. Проверьте, какие версии общей платформы ASP.NET Core установлены на целевом компьютере.

### <a name="5000-in-process-handler-load-failure"></a>500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)

Рабочий процесс завершается ошибкой. Приложение не запускается.

Модулю ASP.NET Core не удается найти среду CLR .NET Core и внутрипроцессный обработчик запросов (*aspnetcorev2_inprocess.dll*). Проверьте следующие моменты:

* приложение предназначено для пакета NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) или [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app);
* версия общей платформы ASP.NET Core, для которой предназначено приложение, установлена на целевом компьютере.

### <a name="5000-out-of-process-handler-load-failure"></a>500.0 Out-Of-Process Handler Load Failure (ошибка загрузки внепроцессного обработчика)

Рабочий процесс завершается ошибкой. Приложение не запускается.

Модулю ASP.NET Core не удается найти внепроцессный обработчик запросов. Убедитесь, что *aspnetcorev2_outofprocess.dll* находится во вложенной папке рядом с *aspnetcorev2.dll*.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a>500.31 ANCM Failed to Find Native Dependencies (ANCM не удалось найти собственные зависимости)

Рабочий процесс завершается ошибкой. Приложение не запускается.

Модуль ASP.NET Core пытается запустить среду выполнения .NET Core внутри процесса, но она не запускается. Наиболее распространенная причина этой ошибки в том, что среда выполнения `Microsoft.NETCore.App` или `Microsoft.AspNetCore.App` не установлена. Если целевой платформой развернутого приложения является ASP.NET Core 3.0, но эта версия отсутствует на компьютере, происходит данная ошибка. Пример сообщения об ошибке:

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

В сообщении об ошибке перечислены все установленные версии .NET Core и указывается версия, которая требуется приложению. Чтобы устранить эту ошибку, выполните одно из указанных ниже действий.

* Установите соответствующую версию .NET Core на компьютере.
* Измените целевую версию .NET Core приложения на версию, имеющуюся на компьютере.
* Опубликуйте приложение как [автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd).

При запуске в среде разработки (переменная среды `ASPNETCORE_ENVIRONMENT` имеет значение `Development`) ошибка записывается в ответ HTTP. Причину сбоя при запуске процесса можно также узнать в [журнале событий приложения](#application-event-log).

### <a name="50032-ancm-failed-to-load-dll"></a>500.32 ANCM Failed to Load dll (ANCM не удалось загрузить библиотеку DLL)

Рабочий процесс завершается ошибкой. Приложение не запускается.

Наиболее распространенная причина этой ошибки в том, что приложение опубликовано для несовместимой архитектуры процессора. Если рабочий процесс выполняется как 32-разрядное приложение, но приложение было опубликовано для 64-разрядной архитектуры, происходит эта ошибка.

Чтобы устранить эту ошибку, выполните одно из указанных ниже действий.

* Повторно опубликуйте приложение для той же архитектуры процессора, для которой предназначен рабочий процесс.
* Опубликуйте приложение как [зависящее от платформы развертывание](/dotnet/core/deploying/#framework-dependent-executables-fde).

### <a name="50033-ancm-request-handler-load-failure"></a>500.33 ANCM Request Handler Load Failure (Ошибка загрузки обработчика запросов ANCM)

Рабочий процесс завершается ошибкой. Приложение не запускается.

Приложение не ссылается на платформу `Microsoft.AspNetCore.App`. В модуле ASP.NET Core могут размещаться только приложения, предназначенные для платформы `Microsoft.AspNetCore.App`.

Для устранения этой ошибки убедитесь в том, что приложение предназначено для платформы `Microsoft.AspNetCore.App`. В файле `.runtimeconfig.json` проверьте, является ли эта платформа целевой для приложения.

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a>500.34 ANCM Mixed Hosting Models Not Supported (Смешанные модели размещения ANCM не поддерживаются)

В одном рабочем процессе не могут выполняться одновременно внутрипроцессное приложение и внепроцессное приложение.

Для устранения этой ошибки запускайте приложения в отдельных пулах приложений IIS.

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a>500.35 ANCM Multiple In-Process Applications in same Process (Несколько внутрипроцессных приложений ANCM в одном процессе)

В одном рабочем процессе не могут выполняться одновременно внутрипроцессное приложение и внепроцессное приложение.

Для устранения этой ошибки запускайте приложения в отдельных пулах приложений IIS.

### <a name="50036-ancm-out-of-process-handler-load-failure"></a>500.36 ANCM Out-Of-Process Handler Load Failure (Ошибка загрузки внепроцессного обработчика ANCM)

Рядом с файлом *aspnetcorev2.dll* нет внепроцессного обработчика запросов *aspnetcorev2_outofprocess.dll*. Это свидетельствует о поврежденной установке модуля ASP.NET Core.

Для устранения этой ошибки восстановите установку [пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (для служб IIS) или Visual Studio (для IIS Express).

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a>500.37 ANCM Failed to Start Within Startup Time Limit (Модуль ANCM не запустился в течение предельного времени запуска)

Модуль ANCM не запустился в течение заданного предельного времени запуска. По умолчанию время ожидания составляет 120 секунд.

Эта ошибка может произойти, если на одном компьютере запускается много приложений. Обратите внимание на пики использования ЦП и памяти на сервере во время запуска. Возможно, потребуется регулировать процесс запуска нескольких приложений.

### <a name="50030-in-process-startup-failure"></a>500.30 In-Process Startup Failure (ошибка внутрипроцессного запуска)

Рабочий процесс завершается ошибкой. Приложение не запускается.

Модуль ASP.NET Core пытается запустить среду выполнения .NET Core внутри процесса, но она не запускается. Обычно причина сбоя при запуске процесса определяется по записям в [журнале событий приложения](#application-event-log) и [журнале вывода stdout модуля ASP.NET Core](#aspnet-core-module-stdout-log).

### <a name="5000-in-process-handler-load-failure"></a>500.0 In-Process Handler Load Failure (ошибка загрузки внутрипроцессного обработчика)

Рабочий процесс завершается ошибкой. Приложение не запускается.

При загрузке компонентов модуля ASP.NET Core произошла неизвестная ошибка. Выполните одно из указанных ниже действий.

* Обратитесь в [службу поддержки Майкрософт](https://support.microsoft.com/oas/default.aspx?prid=15832) (выберите **Средства разработчика**, а затем — **ASP.NET Core**).
* Задайте вопрос на сайте Stack Overflow.
* Сообщите о проблеме в нашем [репозитории GitHub](https://github.com/aspnet/AspNetCore).

::: moniker-end

### <a name="500-internal-server-error"></a>500 Internal Server Error (внутренняя ошибка сервера)

Приложение запускается, но ошибка не позволяет серверу выполнить запрос.

Эта ошибка возникает в коде приложения при запуске или при создании ответа. Ответ может не иметь содержимого или ответ в браузере может выглядеть как *500 — внутренняя ошибка сервера*. Как правило, в журнале событий приложения указывается, что приложение запускается в обычном режиме. Сервер действует правильно. Приложение запустилось, но не может сформировать допустимый ответ. Чтобы устранить проблему, [запустите приложение в командной строке](#run-the-app-at-a-command-prompt) на сервере или [включите журнал вывода stdout для модуля ASP.NET Core](#aspnet-core-module-stdout-log).

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>Не удалось запустить приложение (код ошибки "0x800700c1")

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

Не удалось запустить приложение, так как не удалось загрузить сборку приложения ( *.dll*).

Эта ошибка возникает, если существует несоответствие разрядности опубликованного приложения и процесса w3wp/iisexpress.

Убедитесь, что параметр 32-разрядности пула приложений указан правильно:

1. Выберите пул приложений в диспетчере служб IIS в разделе **Пулы приложений**.
1. Выберите **Дополнительные параметры** в разделе **Изменение пула приложений** панели **Действия**.
1. Установите параметр **Включить 32-разрядные приложения**:
   * При развертывании 32-разрядного (x86) приложения задайте значение `True`.
   * При развертывании 64-разрядного (x64) приложения задайте значение `False`.

### <a name="connection-reset"></a>Сброс подключения

Если ошибка возникает после отправки заголовков, сервер уже не может отправить страницу **500 — внутренняя ошибка сервера**. Это часто происходит, когда ошибка возникает во время сериализации составных объектов ответа. Этот тип ошибки отображается на стороне клиента как ошибка *Сброс подключения*. [Ведение журналов для приложений](xref:fundamentals/logging/index) может помочь устранить эти ошибки.

## <a name="default-startup-limits"></a>Ограничения по умолчанию при запуске

Параметру *startupTimeLimit* модуля ASP.NET Core установлено значение по умолчанию 120 секунд. Если оставить значение по умолчанию, то прежде чем журнал модуля зарегистрирует ошибку запуска приложения, может пройти до двух минут. Сведения о настройке модуля см. в разделе [Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Устранение неполадок при запуске приложения

### <a name="enable-the-aspnet-core-module-debug-log"></a>Включение журнала отладки модулей ASP.NET Core

Добавьте следующие параметры обработчика в файл *web.config* приложения, чтобы включить журналы отладки модулей ASP.NET Core:

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

Убедитесь, что указанный для журнала путь существует и удостоверение пула приложений имеет разрешения на запись в это расположение.

### <a name="application-event-log"></a>Журнал событий приложений

Доступ к журналу событий приложений

1. Откройте меню "Пуск", выполните поиск по фразе **Просмотр событий** и выберите приложение **Просмотр событий**.
1. В **средстве просмотра событий** откройте узел **Журналы Windows**.
1. Выберите **Приложение**, чтобы открыть журнал событий приложения.
1. Проверьте здесь наличие ошибок, связанных с проблемным приложением. Нам нужны ошибки со значениями *Модуль IIS AspNetCore* или *Модуль IIS Express AspNetCore* в столбце *Источник*.

### <a name="run-the-app-at-a-command-prompt"></a>Запуск приложения в командной строке

Многие ошибки запуска не создают полезные сведения в журнале событий приложения. Для некоторых ошибок можно найти причину, запустив приложение в командной строке на компьютере размещения.

#### <a name="framework-dependent-deployment"></a>развертывание, зависящее от платформы;

Если развертываемое приложение [зависит от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), сделайте следующее:

1. В командной строке перейдите в папку развертывания и запустите приложение, выполнив команду *dotnet.exe* для сборки приложения. В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `dotnet .\<assembly_name>.dll`.
1. Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.
1. Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных. Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию. Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.

#### <a name="self-contained-deployment"></a>автономное развертывание;

Если приложение [развертывается автономно](/dotnet/core/deploying/#self-contained-deployments-scd), сделайте следующее:

1. В командной строке перейдите в папку развертывания и запустите исполняемый файл приложения. В следующей команде укажите нужное имя сборки вместо заполнителя \<assembly_name>: `<assembly_name>.exe`.
1. Выходные данные приложения, в том числе любые ошибки, будут выведены в окно консоли.
1. Если ошибки возникают при выполнении запроса к приложению, выполните запрос к узлу и порту, где Kestrel ожидает передачи данных. Создайте запрос к `http://localhost:5000/`, используя узел и порт по умолчанию. Если приложение отвечает на запросы по адресу конечной точки Kestrel как обычно, то проблема, вероятнее, связана с конфигурацией размещения, а не с самим приложением.

### <a name="aspnet-core-module-stdout-log"></a>Журнал stdout модуля ASP.NET Core

Чтобы включить и просмотреть журналы stdout, сделайте следующее:

1. Перейдите в папку развертывания сайта на компьютере размещения.
1. Если здесь нет папки *logs*, создайте ее. Сведения о том, как настроить в MSBuild автоматическое создание папки *logs* в развертывании, см. в статье [о структуре каталогов](xref:host-and-deploy/directory-structure).
1. Измените файл *web.config*. Задайте для параметра **stdoutLogEnabled** значение `true` и измените путь **stdoutLogFile** так, чтобы он указывал на папку *logs* (например, `.\logs\stdout`). В этом пути `stdout` обозначает префикс имени для файла журнала. При создании файла журнала автоматически добавляются метка времени, идентификатор процесса и расширение файла. Если указан префикс файла `stdout`, стандартный файл журнала будет иметь примерно такое имя: *stdout_20180205184032_5412.log*.
1. Убедитесь, что удостоверение пула приложений имеет разрешения на запись в папку *logs*.
1. Сохраните обновленный файл *web.config*.
1. Сделайте запрос к приложению.
1. Перейдите в папку *logs*. Найдите и откройте последний журнал вывода stdout.
1. Проверьте, нет ли в нем ошибок.

> [!IMPORTANT]
> Завершив устранение неполадок, отключите ведение журнала stdout.

1. Измените файл *web.config*.
1. Задайте параметру **stdoutLogEnabled** значение `false`.
1. Сохраните файл.

> [!WARNING]
> Ошибка при отключении журнала stdout может привести к сбоям приложения или сервера. Ни размер файла журнала, ни количество создаваемых файлов журналов ничем не ограничены.
>
> Для обычного ведения журналов для приложения ASP.NET Core используйте библиотеку, которая ограничивает размер файла журнала и выполняет циклический сдвиг журналов. Дополнительные сведения см. в разделе [Сторонние поставщики ведения журналов](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enable-the-developer-exception-page"></a>Включение страницы исключений для разработчика

Можно добавить переменную среды `ASPNETCORE_ENVIRONMENT` [в файл web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables), чтобы запустить приложение в среде разработки. Если среда не переопределяется при запуске приложения использованием `UseEnvironment` в конструкторе узла, эта переменная среды позволяет отображать [страницу исключения для разработчика](xref:fundamentals/error-handling) при запуске приложения.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

Настройка переменной среды `ASPNETCORE_ENVIRONMENT` рекомендуется только на промежуточных и тестовых серверах, доступ к которым из Интернета закрыт. Завершив устранение неполадок, удалите эту переменную среды из файла *web.config*. Сведения о настройке переменных среды в файле *web.config* см. в статье о [дочернем элементе environmentVariables в aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Стандартные ошибки запуска

См. раздел <xref:host-and-deploy/azure-iis-errors-reference>. В этой статье рассматривается большинство распространенных ошибок, препятствующих запуску приложений.

## <a name="obtain-data-from-an-app"></a>Получение данных из приложения

Если приложение способно отвечать на запросы, получите данные о запросе, подключении и дополнительные данные из приложений с помощью встроенного терминала ПО промежуточного слоя. Дополнительные сведения и примеры с кодом см. здесь: <xref:test/troubleshoot#obtain-data-from-an-app>.

## <a name="create-a-dump"></a>Создание дампа

*Дамп* представляет собой моментальный снимок системной памяти и может помочь определить причину аварийного завершения, сбоя запуска или медленной работы приложения.

### <a name="app-crashes-or-encounters-an-exception"></a>Аварийное завершение работы приложения или исключение

Получите и проанализируйте дамп из [отчетов об ошибках Windows (WER)](/windows/desktop/wer/windows-error-reporting):

1. Создайте папку для хранения файлов аварийного дампа в `c:\dumps`. Пул приложений должен иметь доступ на запись к папке.
1. Запустите [скрипт PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):
   * Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. Запустите приложение в условиях, вызывающих аварийное завершение.
1. После аварийного завершения запустите [скрипт PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):
   * Если приложение использует [модель размещения в процессе](xref:host-and-deploy/iis/index#in-process-hosting-model), выполните скрипт для *w3wp.exe*:

     ```console
     .\DisableDumps w3wp.exe
     ```

   * Если приложение использует [модель размещения вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), выполните скрипт для *dotnet.exe*:

     ```console
     .\DisableDumps dotnet.exe
     ```

Когда приложение аварийно завершит работу и сбор дампов будет выполнен, приложение сможет завершить работу обычным образом. Скрипт PowerShell настраивает отчеты об ошибках Windows для сбора до пяти дампов для приложения.

> [!WARNING]
> Аварийные дампы могут занимать много места на диске (до нескольких гигабайтов каждый).

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>Приложение перестает отвечать на запросы, не запускается или работает в обычном режиме

Когда приложение *перестает отвечать на запросы* (но аварийное завершение не происходит), не запускается или работает в обычном режиме, см. раздел [Файлы дампа пользовательского режима: выбор лучшего инструмента](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool), чтобы выбрать подходящий инструмент для создания дампа.

### <a name="analyze-the-dump"></a>Анализ дампа

Дамп можно проанализировать несколькими способами. Дополнительные сведения см. в разделе [Анализ файла дампа пользовательского режима](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).

## <a name="remote-debugging"></a>Удаленная отладка

Изучите раздел документации по Visual Studio [об удаленной отладке ASP.NET Core на удаленном компьютере IIS в Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) предоставляет данные телеметрии из приложений, размещенных в IIS, в том числе возможности регистрации ошибок и создания отчетов. Application Insights может сообщать только об ошибках, возникающих после запуска приложения, когда становятся доступными функции ведения журнала приложения. Дополнительные сведения см. в статье [Application Insights для ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-advice"></a>Дополнительные советы

Иногда приложения-функции перестают работать сразу после обновления пакета SDK для .NET Core на компьютере разработки или обновления пакетов в самом приложении. В некоторых случаях в результате важного обновления несогласованные версии пакетов могут привести к нарушению работы приложения. Большинство этих проблем можно исправить следующим образом:

1. Удалите папки *bin* и *obj*.
1. Очистите кэши пакетов, размещенные в папках *%UserProfile%\\.nuget\\packages* и *%LocalAppData%\\Nuget\\v3-cache*.
1. Восстановите и перестройте проект.
1. Убедитесь, что предыдущее развертывание на сервере полностью удалено, прежде чем развертывать его снова.

> [!TIP]
> Очистить кэши пакета проще всего командой `dotnet nuget locals all --clear` из командной строки.
>
> Также очистку кэшей пакетов можно выполнить средством [nuget.exe](https://www.nuget.org/downloads) или командой `nuget locals all -clear`. *NuGet.exe* не входит в пакет установки операционной системы Windows для настольных компьютеров и его нужно получить отдельно на [веб-сайте NuGet](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
