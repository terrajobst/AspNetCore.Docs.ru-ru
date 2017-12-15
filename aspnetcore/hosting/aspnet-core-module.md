---
title: "Справочник по конфигурации ASP.NET модуль Core"
author: guardrex
description: "Сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core."
keywords: "ASP.NET Core ancm, модуль core, iis, stdout ведения журнала, переменная среды, env var, subapplication, subapp, appoffline, app_offline, 502, схемы"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 5de0c8f7-50ce-4e2c-b3d4-a1bd9fdfcff5
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/aspnet-core-module
ms.openlocfilehash: f0759f16ada531774a3945f67495e5f634e6154e
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2017
---
# <a name="aspnet-core-module-configuration-reference"></a>Справочник по конфигурации ASP.NET модуль Core

По [Latham Люк](https://github.com/guardrex), [Рик Андерсон](https://twitter.com/RickAndMSFT), и [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Этот документ содержит сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core. Введение модуль ASP.NET Core и инструкции по установке см. в разделе [Общие сведения о модуле ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-via-webconfig"></a>Конфигурации через web.config

Модуль Core ASP.NET настраивается через сайт или приложение *web.config* файла и имеет свой собственный `aspNetCore` раздела конфигурации в пределах `system.webServer`. Ниже приведен пример *web.config* файла, который `Microsoft.NET.Sdk.Web` SDK будет предоставлять при публикации проекта [развертывания зависит от framework](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) с заполнителями для `processPath` и `arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

*Web.config* ниже приведен пример для [автономное развертывание](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) для [службе приложений Azure](https://azure.microsoft.com/services/app-service/). Дополнительные сведения см. в разделе [публикация в службах IIS](xref:publishing/iis). В разделе [конфигурации дочерних приложений](xref:publishing/iis#configuration-of-sub-applications) для важное примечание, относящиеся к конфигурации *web.config* файлы в дочерних приложений.

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>Атрибуты элемента aspNetCore

| Атрибут | Описание |
| --- | --- |
| processPath | <p>Обязательный строковый атрибут.</p><p>Путь к исполняемому файлу, который запустит процесс, прослушивающий для HTTP-запросов. Поддерживаются относительные пути. Если путь начинается с ".", путь считается относительно корневого каталога сайта.</p><p>Значение по умолчанию отсутствует.</p> |
| аргументы | <p>Необязательный строковый атрибут.</p><p>Аргументы для исполняемого файла, указанного в **processPath**.</p><p>Значение по умолчанию - пустая строка.</p> |
| startupTimeLimit | <p>Необязательный целочисленный атрибут.</p><p>Длительность в секундах, модуль будет ожидать исполняемому файлу для запуска процесса, прослушивающего порт. При превышении этого ограничения модуль будет завершить процесс. Модуль будет пытаться снова запустить процесс при получении нового запроса и попытаться перезапустить процесс на последующие входящие запросы, если не удается запустить приложение продолжит **rapidFailsPerMinute** номер раз в последовательного последней минуты.</p><p>Значение по умолчанию — 120.</p> |
| shutdownTimeLimit | <p>Необязательный целочисленный атрибут.</p><p>Длительность в секундах, для которых модуль будет ожидать исполняемый файл для корректного завершения работы при *app_offline.htm* обнаружен файл.</p><p>Значение по умолчанию — 10.</p> |
| rapidFailsPerMinute | <p>Необязательный целочисленный атрибут.</p><p>Указывает число раз в процесс **processPath** может завершиться сбоем в минуту. Если этот предел превышен, модуль останавливает запуск процесса для оставшейся части минуты.</p><p>Значение по умолчанию — 10.</p> |
| requestTimeout | <p>Timespan необязательный атрибут.</p><p>Указывает продолжительность, для которого модуль ASP.NET Core будет ожидать ответа от процесса, прослушивающего порт % ASPNETCORE_PORT %.</p><p>Значение по умолчанию - 00:02:00.</p><p>`requestTimeout` Должно быть указано в целых минутах, в противном случае по умолчанию на 2 минуты.</p> |
| stdoutLogEnabled | <p>Дополнительный логический атрибут.</p><p>Если значение равно true, **stdout** и **stderr** для процесса, указанного в **processPath** будут перенаправляться в файл, указанный в **stdoutLogFile**.</p><p>Значением по умолчанию является false.</p> |
| stdoutLogFile | <p>Необязательный строковый атрибут.</p><p>Указывает относительный или абсолютный путь, для которого **stdout** и **stderr** из процесса, указанного в **processPath** будут регистрироваться. Относительные пути задаются относительно корневого каталога сайта. Любой путь, начинающийся с "." будет относительно корневого каталога сайта, а также все пути будет рассматриваться как абсолютные пути. Все папки, путь должен существовать для модуля, который нужно создать файл журнала. Идентификатор процесса, отметка времени (*yyyyMdhms*) и расширение файла (*.log*) с подчеркивания разделители добавляются к последнему сегмент **stdoutLogFile** предоставлен.</p><p>Значение по умолчанию — `aspnetcore-stdout`.</p> |
| forwardWindowsAuthToken | "истина" или "ложь".</p><p>Значение true, если маркер будут пересылаться дочернего процесса, прослушивающего порт % ASPNETCORE_PORT % как заголовок «MS-ASPNETCORE-WINAUTHTOKEN» каждого запроса. Он отвечает этот процесс для вызова CloseHandle по этому токену каждого запроса.</p><p>Значение по умолчанию — true.</p> |
| disableStartUpErrorPage | "истина" или "ложь".</p><p>Значение true, если **502.5 - сбой процесса** странице будет подавляться и кодовую страницу 502 состояния на ваш *web.config* будет иметь приоритет.</p><p>Значением по умолчанию является false.</p> |

### <a name="setting-environment-variables"></a>Задание переменных среды

Модуль Core ASP.NET позволяет вам указать переменные среды для процесса, указанного в `processPath` атрибут, указав их в один или несколько `environmentVariable` дочерних элементов `environmentVariables` элемент коллекции в списке `aspNetCore` элемента. Переменные среды, заданные в этом разделе имеют приоритет над системных переменных среды для процесса.

Следующий пример устанавливает две переменные среды. `ASPNETCORE_ENVIRONMENT`Настраивает среду приложения для `Development`. Разработчик может временно задать это значение *web.config* файл, чтобы принудительно реализовать [страницы разработчик исключений](xref:fundamentals/error-handling) для загрузки при отладке приложения исключения. `CONFIG_DIR`Примером может служить переменной пользовательской среды, где разработчик написал код, который будет считывать значения при запуске, чтобы сформировать путь для загрузки файла конфигурации приложения.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

## <a name="appofflinehtm"></a>App_offline.htm

Если поместить файл с именем *app_offline.htm* в корневой каталог веб-приложения, будет пытаться корректного завершения работы приложения и остановить обработку входящих запросов модуль ASP.NET Core. Если приложение по-прежнему выполняется `shutdownTimeLimit` число секунд, модуль ASP.NET Core будет завершать выполняющийся процесс.

Хотя *app_offline.htm* файл присутствует, модуль ASP.NET Core реагирует на запросы, отправляет содержимое *app_offline.htm* файла. Один раз *app_offline.htm* файл удаляется, следующий запрос загружает приложение, которое затем отвечает на запросы.

## <a name="start-up-error-page"></a>При запуске страницы ошибок

Если происходит сбой запуска фоновой обработки или начинается процесс серверной части, но не удается прослушать настроенный порт модуль ASP.NET Core, появится страница кода состояния HTTP 502.5. Отмените запуск этой страницы и вернуться к кодовой странице состояние IIS 502 по умолчанию, используйте `disableStartUpErrorPage` атрибута. Дополнительные сведения о настройке пользовательские сообщения об ошибках см. в разделе [ошибки HTTP `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).

![502 состояния страницы](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Создание журнала и перенаправления

Модуль Core ASP.NET перенаправляет `stdout` и `stderr` журналы на диск, если задать `stdoutLogEnabled` и `stdoutLogFile` атрибуты `aspNetCore` элемента. Все папки в `stdoutLogFile` путь должен существовать в порядке для модуля, который нужно создать файл журнала. Отметки времени и расширением файла будет автоматически добавляться при создании файла журнала. Журналы не поворачиваются, пока не произойдет перезапуск или перезапуска процесса. Он отвечает поставщика услуг размещения, чтобы ограничить дисковое пространство, которое использовать журналы. С помощью `stdout` журнала рекомендуется только для устранения неполадок при запуске приложения, а не для целей Общие параметры ведения журнала.

Имя файла журнала составляется путем добавления идентификатор процесса (PID), отметка времени (*yyyyMdhms*) и расширение файла (*.log*) для последнего сегмента `stdoutLogFile` пути (обычно *stdout* ) с разделителями, символами подчеркивания. Например если `stdoutLogFile` путь заканчивается *stdout*, журнал приложений с PID 10652 создан на 8/10/2017 г. в 12:05:02 имеет имя файла *stdout_10652_20178101252.log*.

Ниже приведен пример `aspNetCore` элемент, который настраивает `stdout` ведения журнала. `stdoutLogFile` Пути, показано в примере подходит для службы приложений Azure. Локальный путь или путь к сетевой папке приемлемо для ведения локального журнала. Убедитесь, что удостоверение AppPool пользователя имеется разрешение на запись в указанный путь.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Модуль ASP.NET Core с IIS общей конфигурации

Модуль ASP.NET Core программа установки запускается с правами **системы** учетной записи. Так как учетная запись локальной системы не изменять разрешения для путь к общей папке, которая используется общей конфигурации IIS, установщик столкнутся отказ в доступе при попытке настроить параметры модуля в  *applicationHost.config* в общей папке.

Неподдерживаемые достаточно выполнить отключение общей конфигурации IIS, запустите программу установки, экспортировать обновленный *applicationHost.config* файл в общую папку, а также повторно включить общей конфигурации IIS.

## <a name="module-schema-and-configuration-file-locations"></a>Модуль, схемы и конфигурации расположения файлов

### <a name="module"></a>Модуль

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * % ProgramFiles (x86) %\IIS Express\aspnetcore.dll

### <a name="schema"></a>Схема

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Конфигурация

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Можно выполнить поиск *aspnetcore.dll* в *applicationHost.config* файла. Для IIS Express *applicationHost.config* файл не должен существовать по умолчанию. Файл будет создан в *{корневой каталог приложения}\.vs\config* при запуске любого проекта веб-приложения в решении Visual Studio.
