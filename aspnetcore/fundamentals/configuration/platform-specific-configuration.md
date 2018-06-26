---
title: Усовершенствование приложения из внешней сборки в ASP.NET Core с IHostingStartup
author: guardrex
description: Узнайте, как улучшить приложение ASP.NET Core из внешней сборки, используя реализацию IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: bd9605dd8efee2c3ba8bc82a81554cace40630be
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278087"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Усовершенствование приложения из внешней сборки в ASP.NET Core с IHostingStartup

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Реализация [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) позволяет при запуске добавлять в приложение улучшения из внешней сборки вне класса `Startup` приложения. Например, библиотека внешних средств может использовать реализацию `IHostingStartup`, чтобы предоставить приложению дополнительных поставщиков конфигурации или сервисы. `IHostingStartup` *доступно в ASP.NET Core 2.0 и более поздней версии.*

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Обнаружение загруженных начальных сборок размещения

Для обнаружения начальных сборок размещения, загруженных приложением или библиотеками, включите ведение журнала и проверяйте журналы приложений. В журнал вносятся ошибки, возникающие при загрузке сборок. Загруженные начальные сборки размещения регистрируются на уровне отладки, также регистрируются все ошибки.

Пример приложения считывает [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) в массив `string` и отображает результат на странице индексов в приложении:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Отключение автоматической загрузки начальных сборок размещения

Существует два способа отключить автоматическую загрузку начальных сборок размещения:

* Задайте параметр конфигурации узла [Запретить запуск размещения](xref:fundamentals/host/web-host#prevent-hosting-startup).
* Задайте переменную среды `ASPNETCORE_PREVENTHOSTINGSTARTUP`.

Если параметр узла или переменная среды имеют значение `true` или `1`, начальные сборки размещения не загружаются автоматически. Если заданы оба параметра, приоритет имеет параметр узла.

Параметр узла или переменная среды отключают начальные сборки размещения глобально и могут также отключить несколько характеристик приложения. В настоящее время невозможно выборочно отключить начальную сборку размещения, добавленную библиотекой, если у библиотеки нет своего параметра конфигурации. В следующем выпуске начальные сборки размещения можно будет отключать выборочно (см. [выпуск GitHub "aspnet/размещение" № 1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup"></a>Реализация IHostingStartup

### <a name="create-the-assembly"></a>Создание сборки

Усовершенствование `IHostingStartup` разворачивается как сборка на основе консольного приложения без точки входа. Сборка ссылается на пакет [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

Атрибут [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) определяет класс как реализацию `IHostingStartup` для загрузки и выполнения при построении [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). В следующем примере используется пространство имен `StartupEnhancement` и класс `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

Класс реализует `IHostingStartup`. Метод класса [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) использует [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) для добавления улучшений в приложение. `IHostingStartup.Configure` в стартовой сборке размещения вызывается средой выполнения до `Startup.Configure` в пользовательском коде, что позволяет пользовательскому коду перезаписать конфигурацию, предоставленную стартовой сборкой размещения.

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

При создании проекта `IHostingStartup` файл зависимостей (*\*. deps.json*) задает расположение `runtime` для сборки в папке *bin*:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Показана только часть файла. Имя сборки в примере — `StartupEnhancement`.

### <a name="update-the-dependencies-file"></a>Обновление файла зависимостей

Расположение среды выполнения указывается в файле *\*.deps.json*. Для активации улучшения элемент `runtime` должен указывать расположение среды выполнения сборки для улучшения. Расположение `runtime` должно иметь префикс `lib/<TARGET_FRAMEWORK_MONIKER>/`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

В примере приложения изменение файла *\*.deps.json* выполняется скриптом [PowerShell](/powershell/scripting/powershell-scripting). Скрипт PowerShell запускается автоматически целевым объектом сборки в файле проекта.

### <a name="enhancement-activation"></a>Активация улучшения

**Размещение файла сборки**

Файл сборки реализации `IHostingStartup` должен быть развернут в папке *bin* в приложении или помещен в [хранилище среды выполнения](/dotnet/core/deploying/runtime-store):

Для индивидуального использования поместите сборку в хранилище среды выполнения профиля пользователя:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Для глобального использования поместите сборку в хранилище среды выполнения установки .NET Core:

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

При развертывании сборки в хранилище среды выполнения файл символов тоже можно развернуть, но это необязательно для оптимизации работы.

**Размещение файла зависимостей**

Файл реализации *\*.deps.json* должен находиться в доступном расположении.

Для индивидуального использования поместите файл в папку `additonalDeps` в параметрах профиля пользователя `.dotnet`:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

Для глобального использования поместите файл в папку `additonalDeps` в установке .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

Версия общей платформы отражает версию общей среды выполнения, которую использует целевое приложение. Общая среда выполнения указана в файле *\*.runtimeconfig.json*. В примере приложения общая среда выполнения задается в файле *HostingStartupSample.runtimeconfig.json*.

**Настройка переменных среды**

Задайте следующие переменные среды в контексте приложения, которое использует улучшение.

ASPNETCORE_HOSTINGSTARTUPASSEMBLIES

Только начальные сборки размещения проверяются на наличие `HostingStartupAttribute`. Имя сборки реализации предоставляется в этой переменной среды. В пример приложения установлено значение `StartupDiagnostics`.

Значение также можно задать с помощью параметра конфигурации узла [Начальные сборки размещения](xref:fundamentals/host/web-host#hosting-startup-assemblies).

При наличии нескольких стартовых сборок размещения их методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) выполняются в порядке расположения сборок.

DOTNET_ADDITIONAL_DEPS

Расположение файла реализации *\*.deps.json*.

Если файл расположен в папке *.dotnet* в профиле пользователя для индивидуального использования:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Если файл расположен в установке .NET Core для глобального использования, укажите полный путь к файлу:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

В пример приложения установлено значение:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Примеры установки переменных среды для различных операционных систем см. в разделе [Использование нескольких сред](xref:fundamentals/environments).

## <a name="sample-app"></a>Пример приложения

В [примере приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([как загрузить](xref:tutorials/index#how-to-download-a-sample)) используется `IHostingStartup` для создания средства диагностики. Это средство добавляет в приложение при запуске два ПО промежуточного слоя, которые предоставляют диагностические сведения:

* Зарегистрированные службы
* Адрес: схема, узел, базовый путь, путь, строка запроса
* Подключение: удаленный IP, удаленный порт, локальный IP, локальный порт, сертификат клиента
* Заголовки запросов
* Переменные среды

Для выполнения образца:

1. В проекте диагностики запуска используется [PowerShell](/powershell/scripting/powershell-scripting) для изменения файла *StartupDiagnostics.deps.json*. PowerShell устанавливается по умолчанию в операционной системе Windows начиная с Windows 7 с пакетом обновления 1 (SP1) и Windows Server 2008 R2 с пакетом обновления 1 (SP1). Для установки PowerShell на других платформах см. раздел [Установка Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Создайте проект диагностики запуска. Целевой объект в файле проекта:
   * Перемещает сборку и файлы символов в хранилище среды выполнения в профиле пользователя.
   * Запускает скрипт PowerShell, чтобы изменить файл *StartupDiagnostics.deps.json*.
   * Перемещает файл *StartupDiagnostics.deps.json* в папку `additionalDeps` в профиле пользователя.
3. Задайте переменные среды:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Выполните пример приложения.
5. Запросите конечную точку `/services` для просмотра зарегистрированных служб приложения. Запросите конечную точку `/diag` для просмотра диагностических сведений.
