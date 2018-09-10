---
title: Усовершенствование приложения из внешней сборки в ASP.NET Core с IHostingStartup
author: guardrex
description: Узнайте, как улучшить приложение ASP.NET Core из внешней сборки, используя реализацию IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 2eddfa03b28564fcca7cc098e353b05e23b7c6f6
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336306"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Усовершенствование приложения из внешней сборки в ASP.NET Core с IHostingStartup

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Реализация [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (размещение при запуске) позволяет добавлять в приложение улучшения из внешней сборки при запуске. Например, внешняя библиотека может использовать реализацию размещения при запуске, чтобы доставить дополнительные поставщики конфигурации или службы для приложения. `IHostingStartup` *доступен в ASP.NET Core 2.0 или в более поздних версиях.*

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>Атрибут HostingStartup

Атрибут [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) указывает на наличие начальной сборки размещения, которая будет активирована во время выполнения.

Входная сборка или сборка, содержащая класс `Startup`, автоматически сканируется на наличие атрибута `HostingStartup`. Список сборок, в котором будет выполняться поиск атрибута `HostingStartup`, загружается во время выполнения из конфигурации в [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey). Список сборок, которые необходимо исключить из обнаружения, загружается из [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey). Дополнительные сведения см. в разделах [Веб-узел: начальные сборки размещения](xref:fundamentals/host/web-host#hosting-startup-assemblies) и [Веб-узел: исключаемые сборки размещения при запуске](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).

В следующем примере пространство имен для начальной сборки размещения — `StartupEnhancement`. Класс, содержащий код запуска размещения, — `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Атрибут `HostingStartup` обычно находится в файле класса реализации начальной сборки размещения `IHostingStartup`.

## <a name="discover-loaded-hosting-startup-assemblies"></a>Обнаружение загруженных начальных сборок размещения

Для обнаружения загруженных сборок размещения при запуске включите ведение журнала и проверьте журналы приложения. В журнал вносятся ошибки, возникающие при загрузке сборок. Загруженные начальные сборки размещения регистрируются на уровне отладки, также регистрируются все ошибки.

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Отключение автоматической загрузки начальных сборок размещения

::: moniker range=">= aspnetcore-2.1"

Чтобы отключить автоматическую загрузку начальных сборок размещения, используйте один из следующих подходов:

* Чтобы предотвратить загрузку всех начальных сборок размещения, установите значение `true` или `1` для одного из следующих параметров:
  * Параметр конфигурации узла [Запретить размещение при запуске](xref:fundamentals/host/web-host#prevent-hosting-startup).
  * Переменная среды `ASPNETCORE_PREVENTHOSTINGSTARTUP`.
* Чтобы предотвратить загрузку конкретных сборок размещения, установите строку, содержащую разделенный точками с запятой список сборок размещения, которые необходимо исключить при запуске, в качестве значения одного из следующих параметров:
  * Параметр конфигурации узла [Исключаемые сборки размещения при запуске](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).
  * Переменная среды `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Чтобы отключить автоматическую загрузку начальных сборок размещения, установите значение `true` или `1` для одной из следующих переменных:

* Параметр конфигурации узла [Запретить размещение при запуске](xref:fundamentals/host/web-host#prevent-hosting-startup).
* Переменная среды `ASPNETCORE_PREVENTHOSTINGSTARTUP`.

::: moniker-end

Если заданы параметры конфигурации узла и переменная среды, на поведение влияют параметры узла.

При отключении сборок размещения при запуске с использованием параметра узла или переменной среды сборка отключается на глобальном уровне, и также могут быть отключены некоторые характеристики приложения.

## <a name="project"></a>Проект

Создайте размещение при запуске для любого из указанных ниже типов проектов:

* [Библиотека классов](#class-library)
* [Консольное приложение без точки входа](#console-app-without-an-entry-point)

### <a name="class-library"></a>Библиотека классов

Расширение размещения при запуске можно указать в библиотеке классов. Библиотека содержит атрибут `HostingStartup`.

[Пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) включает приложение Razor Pages *HostingStartupApp* и библиотеку классов *HostingStartupLibrary*. Библиотека классов:

* Содержит класс размещения при запуске `ServiceKeyInjection`, который реализует `IHostingStartup`. `ServiceKeyInjection` добавляет пару строк службы в конфигурацию приложения с помощью поставщика конфигурации, размещаемой в памяти ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).
* Включает атрибут `HostingStartup`, определяющий пространство имен и класс размещения при запуске.

Метод `ServiceKeyInjection` класса [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) использует [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) для добавления улучшений в приложение. `IHostingStartup.Configure` в начальной сборке размещения вызывается средой выполнения до `Startup.Configure` в пользовательском коде, что позволяет пользовательскому коду перезаписать конфигурацию, предоставленную начальной сборкой размещения.

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

Страница индекса приложения считывает и отображает значения конфигурации для двух ключей, устанавливаемых начальной сборкой размещения библиотеки класса:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[Пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) также включает проект пакета NuGet, предоставляющий отдельное размещение при запуске *HostingStartupPackage*. Характеристики пакета аналогичны характеристикам библиотеки классов, приведенным ранее. Пакет:

* Содержит класс размещения при запуске `ServiceKeyInjection`, который реализует `IHostingStartup`. `ServiceKeyInjection` добавляет пару строк службы в конфигурацию приложения.
* Включает атрибут `HostingStartup`.

*HostingStartupPackage/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

Страница индекса приложения считывает и отображает значения конфигурации для двух ключей, устанавливаемых начальной сборкой размещения пакета:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>Консольное приложение без точки входа

*Этот подход может использоваться только для приложений .NET Core, но не для приложений .NET Framework.*

Улучшение динамического размещения при запуске, для активации которого не требуется ссылка во время компиляции, может быть указано в консольном приложении без точки входа. Приложение содержит атрибут `HostingStartup`. Чтобы создать динамическое размещение при запуске, выполните следующие действия:

1. Библиотека реализации создается на основе класса, содержащего реализацию `IHostingStartup`. Библиотека реализации считается обычным пакетом.
1. Консольное приложение без точки входа ссылается на пакет библиотеки реализации. Консольное приложение используется по следующим причинам:
   * Файл зависимостей — это запускаемый ресурс приложения, поэтому библиотека не может предоставить файл зависимостей.
   * Библиотеку невозможно добавить непосредственно в [хранилище пакетов среды выполнения](/dotnet/core/deploying/runtime-store), для которого требуется запускаемый проект для общей среды выполнения.
1. Консольное приложение публикуется для получения зависимостей начальной загрузки. После публикации консольного приложения неиспользуемые зависимости исключаются из файла зависимостей.
1. Приложения и его файл зависимостей размещаются в хранилище пакетов среды выполнения. Чтобы обнаружить сборку начального размещения и ее файл зависимостей, ссылка на них осуществляется в паре переменных среды.

Консольное приложение ссылается на пакет [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

Атрибут [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) определяет класс как реализацию `IHostingStartup` для загрузки и выполнения при построении [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). В следующем примере используется пространство имен `StartupEnhancement` и класс `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Класс реализует `IHostingStartup`. Метод класса [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) использует [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) для добавления улучшений в приложение. `IHostingStartup.Configure` в начальной сборке размещения вызывается средой выполнения до `Startup.Configure` в пользовательском коде, что позволяет пользовательскому коду перезаписать конфигурацию, предоставленную начальной сборкой размещения.

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

При создании проекта `IHostingStartup` файл зависимостей (*\*. deps.json*) задает расположение `runtime` для сборки в папке *bin*:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Показана только часть файла. Имя сборки в примере — `StartupEnhancement`.

## <a name="specify-the-hosting-startup-assembly"></a>Указание начальных сборок размещения

Для библиотеки класса или размещения при запуске с помощью консольного приложения укажите имя начальной сборки размещения в переменной среды `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`. Переменная среды — это список сборок, разделенный точками с запятой.

Только начальные сборки размещения проверяются на наличие атрибута `HostingStartup`. Для примера приложения *HostingStartupApp* необходимо установить следующее значение для переменной среды, чтобы обнаружить сборки размещения при запуске, описанные ранее:

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

Начальную сборку размещения также можно задать с помощью параметра конфигурации узла [Начальные сборки размещения](xref:fundamentals/host/web-host#hosting-startup-assemblies).

При наличии нескольких стартовых сборок размещения их методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) выполняются в порядке расположения сборок.

## <a name="activation"></a>Активация

Ниже приведены варианты активации размещения при запуске:

* [Хранилище среды выполнения](#runtime-store) &ndash; для активации не требуется ссылка во время компиляции. Пример приложения помещает начальную сборку размещения и файлы зависимостей в папку *deployment*, чтобы облегчить развертывание размещения при запуске в среде с несколькими компьютерами. Папка *deployment* также включает сценарий PowerShell, который создает или изменяет переменные среды в системе развертывания, чтобы включить размещение при запуске.
* Ссылки во время компиляции, необходимые для активации
  * [Пакет NuGet](#nuget-package)
  * [Папка bin проекта](#project-bin-folder)

### <a name="runtime-store"></a>Хранилище среды выполнения

Реализация размещения при запуске помещается в [хранилище среды выполнения](/dotnet/core/deploying/runtime-store). Ссылка на сборку во время компиляции не требуется расширенному приложению.

После сборки размещения при запуске файл проекта размещения при запуске выступает в качестве файла манифеста для команды [dotnet store](/dotnet/core/tools/dotnet-store).

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

Эта команда помещает начальную сборку размещения и другие зависимости, которые не являются частью общей платформы, в хранилище среды выполнения в профиле пользователя:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

Если вы хотите разместить сборку и зависимости для глобального использования, добавьте параметр `-o|--output` в команду `dotnet store`, указав следующий путь:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

**Изменение и размещение файла зависимостей размещения при запуске**

Расположение среды выполнения указывается в файле *\*.deps.json*. Для активации улучшения элемент `runtime` должен указывать на расположение среды выполнения сборки для улучшения. Расположение `runtime` должно иметь префикс `lib/<TARGET_FRAMEWORK_MONIKER>/`:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

В коде примера (проект *StartupDiagnostics*) изменение файла *\*.deps.json* осуществляется сценарием [PowerShell](/powershell/scripting/powershell-scripting). Скрипт PowerShell запускается автоматически целевым объектом сборки в файле проекта.

Файл реализации *\*.deps.json* должен находиться в доступном расположении.

Для индивидуального использования поместите файл в папку *additionalDeps* в параметрах профиля пользователя `.dotnet`:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Для глобального использования поместите файл в папку *additionalDeps* в установке .NET Core:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Версия общей платформы отражает версию общей среды выполнения, которую использует целевое приложение. Общая среда выполнения указана в файле *\*.runtimeconfig.json*. В примере приложения (*HostingStartupApp*) общая среда выполнения задается в файле *HostingStartupApp.runtimeconfig.json*.

**Указание расположения для файла зависимостей размещения при запуске**

Расположение файла зависимостей размещения *\*.deps.json* указывается в переменной среды `DOTNET_ADDITIONAL_DEPS`.

Если файл размещен в папке *.dotnet* профиля пользователя, установите следующее значение переменной среды:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

Если файл расположен в установке .NET Core для глобального использования, укажите полный путь к файлу:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

Для примера приложения (*HostingStartupApp*) файл зависимостей (*HostingStartupApp.runtimeconfig.json*) размещается в профиле пользователя.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Установите следующее значение для переменной среды `DOTNET_ADDITIONAL_DEPS`:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Установите следующее значение для переменной среды `DOTNET_ADDITIONAL_DEPS`:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Установите следующее значение для переменной среды `DOTNET_ADDITIONAL_DEPS`:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

Примеры установки переменных среды для различных операционных систем см. в разделе [Использование нескольких сред](xref:fundamentals/environments).

**Развертывание**

Чтобы упростить развертывание размещения при запуске в среде с несколькими компьютерами, пример приложения создает в опубликованном результате папку *deployment*. Эта папка содержит:

* Начальную сборку размещения.
* Файл зависимостей размещения при запуске.
* Сценарий PowerShell, который создает или изменяет `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` и `DOTNET_ADDITIONAL_DEPS` для поддержки активации размещения при запуске. Запустите сценарий из командной строки PowerShell с правами администратора в системе развертывания.

### <a name="nuget-package"></a>Пакет NuGet

Расширение размещения при запуске можно указать в пакете NuGet. Пакет содержит атрибут `HostingStartup`. Типы размещения при запуске, предоставляемые пакетом, становятся доступными для приложения с использованием одного из следующих методов:

* Файл проекта расширенного приложения создает ссылку на пакет для размещения при запуске в файле проекта приложения (ссылка во время компиляции). Со ссылкой во время компиляции начальная сборка размещения и все ее зависимости включаются в файл зависимостей приложения (*\*.deps.json*). Этот подход применяется к пакету начальной сборки размещения, опубликованному на [nuget.org](https://www.nuget.org/).
* Файл зависимостей размещения при запуске становится доступным для расширенного приложения, как описано в разделе [Хранилище среды выполнения](#runtime-store) (без ссылок во время компиляции).

Дополнительные сведения о пакетах NuGet и хранилище среды выполнения см. в разделах:

* [Создание пакета NuGet с помощью кроссплатформенных средств](/dotnet/core/deploying/creating-nuget-packages)
* [Публикация пакетов](/nuget/create-packages/publish-a-package)
* [Хранилище пакетов среды выполнения](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Папка bin проекта

Расширение размещения при запуске может быть представлено сборкой, разворачиваемой в папке *bin* расширенного приложения. Типы размещения при запуске, предоставляемые сборкой, становятся доступными для приложения с использованием одного из следующих методов:

* Файл проекта расширенного приложения создает ссылку сборки на размещение при запуске (ссылка во время компиляции). Со ссылкой во время компиляции начальная сборка размещения и все ее зависимости включаются в файл зависимостей приложения (*\*.deps.json*). Этот подход применяется, когда в сценарии развертывания используются вызовы для перемещения скомпилированной сборки библиотеки размещения при запуске (файла DLL) в принимающий проект или в расположение, доступное для принимающего проекта, и создается ссылка на сборку размещения при запуске во время компиляции.
* Файл зависимостей размещения при запуске становится доступным для расширенного приложения, как описано в разделе [Хранилище среды выполнения](#runtime-store) (без ссылок во время компиляции).

## <a name="sample-code"></a>Пример кода

В [примере кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([инструкции по скачиванию примера](xref:tutorials/index#how-to-download-a-sample)) показаны сценарии реализации размещения при запуске:

* В каждой из двух сборок начального размещения (библиотеки классов) устанавливаются две пары "ключ-значение", хранящиеся в памяти:
  * Пакет NuGet (*HostingStartupPackage*)
  * Библиотека классов (*HostingStartupLibrary*)
* Размещения при запуске активируется из сборки среды выполнения, развертываемой из хранилища (*StartupDiagnostics*). Эта сборка добавляет два вида ПО промежуточного слоя, которые предоставляют диагностические сведения о следующих показателях:
  * Зарегистрированные службы
  * Адрес (схема, узел, базовый путь, путь, строка запроса)
  * Подключение (удаленный IP-адрес, удаленный порт, локальный IP-адрес, локальный порт, сертификат клиента)
  * Заголовки запросов
  * Переменные среды

Для выполнения образца:

**Активация из пакета NuGet**

1. Скомпилируйте пакет *HostingStartupPackage* с помощью команды [dotnet pack](/dotnet/core/tools/dotnet-pack).
1. Добавьте имя сборки пакета *HostingStartupPackage* в переменную среды `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.
1. Скомпилируйте и запустите приложение. Ссылка на пакет появится в расширенном приложении (ссылка во время компиляции). Объект `<PropertyGroup>` в файле проекта приложения определяет выходные данные проекта пакета (*../ HostingStartupPackage/bin/Debug*) в качестве источника пакета. Это позволяет приложению использовать пакет без отправки пакета на сайт [nuget.org](https://www.nuget.org/). Дополнительные сведения см. в примечаниях в файле проекта HostingStartupApp.

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. Обратите внимание, что ключевые значения конфигурации службы, отображаемой на индексной странице, соответствуют значениям, установленным с помощью метода пакета `ServiceKeyInjection.Configure`.

Если вы внесли изменения в проект *HostingStartupPackage* и перекомпилировали его, очистите локальные кэши пакета NuGet, чтобы убедиться, что *HostingStartupApp* получает обновленный пакет, а не устаревший пакет из локального кэша. Чтобы очистить локальные кэши NuGet, выполните следующую команду [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals):

```console
dotnet nuget locals all --clear
```

**Активации из библиотеки классов**

1. Скомпилируйте библиотеку классов *HostingStartupLibrary* с помощью команды [dotnet build](/dotnet/core/tools/dotnet-build).
1. Добавьте имя сборки библиотеки классов *HostingStartupLibrary* в переменную среды `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.
1. Разверните сборку библиотеки классов в папку *bin* приложения. Для этого скопируйте файл *HostingStartupLibrary.dll* из выходной папки компиляции библиотеки в папку *bin/Debug* приложения.
1. Скомпилируйте и запустите приложение. `<ItemGroup>` в файле проекта приложения ссылается на сборку библиотеки класса (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (ссылка во время компиляции). Дополнительные сведения см. в примечаниях в файле проекта HostingStartupApp.

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. Обратите внимание, что ключевые значения конфигурации службы, отображаемой на индексной странице, соответствуют значениям, установленным с помощью метода библиотеки класса `ServiceKeyInjection.Configure`.

**Активация из сборки среды выполнения, развернутой в хранилище**

1. В проекте *StartupDiagnostics* используется [PowerShell](/powershell/scripting/powershell-scripting) для изменения файла *StartupDiagnostics.deps.json*. PowerShell устанавливается по умолчанию в Windows начиная с Windows 7 с пакетом обновления 1 (SP1) и Windows Server 2008 R2 с пакетом обновления 1 (SP1). Для установки PowerShell на других платформах см. раздел [Установка Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).
1. Соберите проект *StartupDiagnostics*. После сборки проекта цель сборки в файле проекта автоматически выполняет следующие действия:
   * Запускает скрипт PowerShell, чтобы изменить файл *StartupDiagnostics.deps.json*.
   * Перемещает файл *StartupDiagnostics.deps.json* в папку *additionalDeps* в профиле пользователя.
1. Выполните команду `dotnet store` в командной строке каталога начального размещения, чтобы сохранить сборку и ее зависимости в хранилище среды выполнения профиля пользователя:

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   В Windows команда использует [идентификатор среды выполнения (RID)](/dotnet/core/rid-catalog) `win7-x64`. При выполнении размещения при запуске для другой среды выполнения укажите соответствующий RID.
1. Задайте переменные среды:
   * Добавьте имя сборки *StartupDiagnostics* в переменную среды `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.
   * В Windows установите значение `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\` для переменной среды `DOTNET_ADDITIONAL_DEPS`. В macOS и Linux, установите значение `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/` для переменной среды `DOTNET_ADDITIONAL_DEPS`, где `<USER>` — профиль пользователя, который содержит размещение при запуске.
1. Выполните пример приложения.
1. Запросите конечную точку `/services` для просмотра зарегистрированных служб приложения. Запросите конечную точку `/diag` для просмотра диагностических сведений.
