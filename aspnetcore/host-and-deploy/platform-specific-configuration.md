---
title: "Добавление компонентов приложения, с помощью конфигурации конкретной платформы в ASP.NET Core"
author: guardrex
description: "Узнайте, как добавить компоненты к приложению ASP.NET Core из внешней сборки, используя реализацию IHostingStartup."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/platform-specific-configuration
ms.openlocfilehash: 2663cd1e05be9e8695966df959082e6e574d0b4a
ms.sourcegitcommit: 809ee4baf8bf7b4cae9e366ecae29de1037d2bbb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2018
---
# <a name="add-app-features-using-a-platform-specific-configuration-in-aspnet-core"></a>Добавление компонентов приложения, с помощью конфигурации конкретной платформы в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) реализация позволяет добавлять компоненты при запуске приложения из внешней сборки вне приложения `Startup` класса. Например, можно использовать библиотеку внешние инструменты `IHostingStartup` реализации для предоставления дополнительной настройки поставщиков или служб для приложения. `IHostingStartup` *— доступен в ASP.NET Core 2.0 и более поздних.*

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Обнаружение сборок, загруженных размещения запуска

Для получения размещение загрузки сборки, загруженные приложений или библиотек, включите ведение журнала и проверяйте журналы приложений. Регистрируются ошибки, возникающие при загрузке сборки. Загрузки сборок, загруженных размещения регистрируются на уровне отладки и регистрируются все ошибки.

Образец приложения чтения [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) в `string` массива и отображает результат в страницу индекса приложения:

[!code-csharp[Main](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Отключить загрузку автоматическое размещение загрузки сборок

Чтобы отключить автоматическое загрузку загрузки сборок, на котором размещается двумя способами.

* Задать [избежать запуска размещения](xref:fundamentals/hosting#prevent-hosting-startup) размещения параметра конфигурации.
* Задать `ASPNETCORE_preventHostingStartup` переменной среды.

Если параметр узла или переменная среды имеет значение `true` или `1`, размещения не автоматической загрузки сборки при запуске. Если заданы оба параметра узла управляет поведением.

Отключение размещения сборок при запуске, с помощью узла параметр или переменную среды глобально отключает и может отключить некоторые функции приложения. В настоящее время невозможно выборочно отключить размещения стартовой сборки, не библиотеке есть свой собственный вариант конфигурации добавлены в библиотеке. Следующем выпуске будет предлагать возможность выборочно отключить размещения сборок при запуске (в разделе [GitHub выдачи aspnet или размещения #1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup-features"></a>Реализации функций IHostingStartup

### <a name="create-the-assembly"></a>Создать сборку

`IHostingStartup` Функция развертывается как сборки с консольного приложения без точки входа. Ссылки на сборку [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) пакета:

[!code-xml[Main](platform-specific-configuration/snapshot_sample/StartupFeature.csproj)]

Объект [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) атрибут идентифицирует класс как реализация `IHostingStartup` для загрузки и выполнения при построении [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). В следующем примере пространство имен является `StartupFeature`, а класс является `StartupFeatureHostingStartup`:

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet1)]

Класс реализует `IHostingStartup`. Класс [Настройка](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) использует метод [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) для добавления в приложение функций:

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

При построении `IHostingStartup` зависимости файла проекта (*\*. deps.json*) задает `runtime` расположение сборки для *bin* папки:

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

Показана только часть файла. Имя сборки, в примере является `StartupFeature`.

### <a name="update-the-dependencies-file"></a>Обновить файл зависимости

Среда выполнения расположение указывается в  *\*. deps.json* файл. Активный компонент `runtime` элемент должен указывать расположение сборки среды выполнения компонента. Префикс `runtime` расположение с `lib/netcoreapp2.0/`:

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

В примере приложения, изменения  *\*. deps.json* файла выполняется путем [PowerShell](/powershell/scripting/powershell-scripting) сценария. Сценарий PowerShell автоматически запускается целевой объект сборки в файле проекта.

### <a name="feature-activation"></a>Активация компонента

**Поместите файл сборки**

`IHostingStartup` Реализация файла сборки должно быть *bin*-развернут в приложении или в [хранилище времени выполнения](/dotnet/core/deploying/runtime-store):

Для использования на пользователя поместите сборку в профиль пользователя в хранилище среды выполнения:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Для использования глобального поместите сборку в хранилище установки .NET Core времени выполнения:

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

При развертывании сборки в хранилище среды выполнения, также могут быть развернуты файл символов, но не требуется для работы функции.

**Поместите файл зависимости**

Реализация  *\*. deps.json* файл должен находиться в доступном расположении.

Для использования на пользователя, поместите файл в `additonalDeps` папку профиля пользователя `.dotnet` параметры: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Для использования глобального, поместите файл в `additonalDeps` папку установки .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Обратите внимание на версию `2.0.0`, отражающее версию общей среды выполнения использует целевое приложение. Общего времени выполнения отображается в  *\*. runtimeconfig.json* файл. В примере приложения общего времени выполнения задается в *HostingStartupSample.runtimeconfig.json* файла.

**Настройка переменных среды**

Задайте следующие переменные среды в контексте приложения, которое использует компонент.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Только размещения запуска сборки проверяются на наличие `HostingStartupAttribute`. Имя сборки реализации предоставляется в этой переменной среды. Пример приложения это значение устанавливается равным `StartupDiagnostics`.

Значение можно также задать с помощью [размещения сборок запуска](xref:fundamentals/hosting#hosting-startup-assemblies) размещения параметра конфигурации.

DOTNET\_ДОПОЛНИТЕЛЬНЫЕ\_DEPS

Расположение реализации  *\*. deps.json* файл.

Если файл помещается в профиле пользователя *.dotnet* папки для использования на пользователя:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Если файл помещается в установку .NET Core для глобального использования, укажите полный путь к файлу:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

Пример приложения устанавливает это значение.

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Примеры для установки переменных среды для различных операционных систем см. в разделе [работа с несколькими средами](xref:fundamentals/environments).

## <a name="sample-app"></a>Пример приложения

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) использует `IHostingStartup` для создания средства диагностики. Добавляет два middlewares приложения во время запуска, которые предоставляют сведения диагностики:

* Зарегистрированные службы
* Адрес: схему, узел, базовый путь, путь, строки запроса
* Подключение: удаленный IP, удаленный порт, локальный IP-адрес, локальный порт, сертификат клиента
* Заголовки запроса
* Переменные среды

Запуск образца:

1. Запуск диагностики проект использует [PowerShell](/powershell/scripting/powershell-scripting) для изменения его *StartupDiagnostics.deps.json* файла. PowerShell устанавливается по умолчанию в операционной системе Windows, начиная с Windows 7 с пакетом обновления 1 и Windows Server 2008 R2 SP1. Для получения PowerShell на других платформах, обратитесь к [Установка Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Выполните построение проекта запуска диагностики. Целевой объект сборки в файле проекта:
   * Перемещает сборки и файлы среды выполнения хранилище профиля пользователя символов.
   * Запускает сценарий PowerShell, чтобы изменить *StartupDiagnostics.deps.json* файла.
   * Перемещает *StartupDiagnostics.deps.json* файла профиля пользователя `additionalDeps` папки.
3. Задание переменных среды:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Запуск образца приложения.
5. Запрос `/services` служб регистрации конечной точки для просмотра приложения. Запрос `/diag` конечной точки, чтобы просмотреть диагностические данные.
