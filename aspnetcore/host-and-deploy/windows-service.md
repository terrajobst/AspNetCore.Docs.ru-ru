---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/03/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4cfca4b38543ff073bb98dc09b483d96096928ae
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692568"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Размещение ASP.NET Core в службе Windows

Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)

Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS. При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки сервера.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

* [Пакет SDK для ASP.NET Core 2.1 или более поздней версии](https://dotnet.microsoft.com/download)
* [PowerShell 6.2 или более поздней версии](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Шаблон службы рабочей роли

Шаблон службы рабочей роли ASP.NET Core может служить отправной точкой для написания длительно выполняющихся приложений служб. Чтобы использовать шаблон в качестве основы для приложения службы Windows, выполните указанные ниже действия.

1. Создайте приложение службы рабочей роли на основе шаблона .NET Core.
1. Согласно указаниям в разделе [Конфигурация приложения](#app-configuration) измените приложение службы рабочей роли так, чтобы оно могло выполняться как служба Windows.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Создайте новый проект.
1. Выберите **Новое веб-приложение ASP.NET Core**. Выберите **Далее**.
1. В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию. Выберите **Создать**.
1. В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.0**.
1. Выберите шаблон **Служба рабочей роли**. Выберите **Создать**.

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code или .NET Core CLI](#tab/visual-studio-code+netcore-cli)

Используйте шаблон службы рабочей роли (`worker`) с командой [dotnet new](/dotnet/core/tools/dotnet-new) из командной оболочки. В приведенном ниже примере создается приложение службы рабочей роли с именем `ContosoWorkerService`. Папка для приложения `ContosoWorkerService` создается автоматически при выполнении команды.

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a>Конфигурация приложения

::: moniker range=">= aspnetcore-3.0"

`IHostBuilder.UseWindowsService` в составе пакета [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) вызывается при создании узла. Если приложение выполняется как служба Windows, метод отвечает за следующие действия:

* Задает для узла время существования `WindowsServiceLifetime`.
* Задает корневой каталог содержимого.
* Включает ведение журнала событий с именем приложения в качестве имени источника по умолчанию.
  * Уровень ведения журнала можно задать с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.
  * Только администраторы могут создавать источники событий. Если источник событий создать нельзя, используя имя приложения, для источника *Приложение* регистрируется предупреждение и журналы событий отключаются.

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Приложение требует ссылки на пакеты для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) и [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Для тестирования и отладки при работе вне службы добавьте код, чтобы определить, как выполняется приложение: в качестве службы или консольного приложения. Проверьте, присоединен ли отладчик или присутствует ли параметр `--console`. Если одно из условий имеет значение true (приложение выполняется не в качестве службы), вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>. Если условия имеют значение false (приложение выполняется в качестве службы), сделайте следующее:

* Вызовите <xref:System.IO.Directory.SetCurrentDirectory*> и используйте путь к расположению для публикации приложения. Не вызывайте <xref:System.IO.Directory.GetCurrentDirectory*> для получения пути, так как при вызове <xref:System.IO.Directory.GetCurrentDirectory*> приложение службы Windows возвращает папку *C:\\WINDOWS\\system32*. Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root). Этот шаг выполняется до того, как приложение будет настроено в `CreateWebHostBuilder`.
* Вызовите <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, чтобы запустить приложение в качестве службы.

Так как [поставщик конфигурации командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) требует указания пар "имя-значение" для аргументов командной строки, параметр `--console` удаляется из аргументов, прежде чем <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> получит их.

Для записи данных в журнал событий Windows добавьте поставщик EventLog в <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Задайте уровень ведения журнала с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*.

В следующем примере `RunAsCustomService` вызывается вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> для обработки событий времени существования в приложении. Дополнительные сведения см. в разделе [Обработка событий запуска и остановки](#handle-starting-and-stopping-events).

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Тип развертывания

Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment-fdd"></a>Зависящее от платформы развертывание (FDD)

Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core. Если сценарий с FDD используется согласно инструкций в этой статье, пакет SDK создаcт исполняемый файл ( *.exe*), который называется *исполняемым файлом, зависящим от платформы*.

::: moniker range=">= aspnetcore-3.0"

Добавьте следующие элементы свойства в файл проекта:

* `<OutputType>` — тип выходных данных приложения (`Exe` для исполняемого файла).
* `<LangVersion>` — версия языка C# (`latest` или `preview`).

Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows. Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) содержит целевую версию платформы. В следующем примере для RID задано значение `win7-x64`. Свойству `<SelfContained>` задано значение `false`. Эти свойства указывают пакету SDK создать исполняемый файл ( *.exe*) для Windows и приложение, которое зависит от общей платформы .NET Core.

Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows. Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) содержит целевую версию платформы. В следующем примере для RID задано значение `win7-x64`. Свойству `<SelfContained>` задано значение `false`. Эти свойства указывают пакету SDK создать исполняемый файл ( *.exe*) для Windows и приложение, которое зависит от общей платформы .NET Core.

Свойству `<UseAppHost>` задано значение `true`. Это свойство предоставляет службу с путем активации (исполняемый файл, *EXE*) для FDD.

Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows. Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a>Автономное развертывание

Для автономного развертывания необязательно наличие общей платформы в системе размещения. Среда выполнения и зависимости приложения развертываются с приложением.

[Идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows включается в `<PropertyGroup>`, где содержится целевая платформа:

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.

* Укажите список идентификаторов RID, разделив их точкой с запятой.
* Используйте имя свойства [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (во множественном числе).

Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

Для свойства `<SelfContained>` задано значение `true`:

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a>Учетная запись пользователя службы

Создайте для службы учетную запись пользователя с помощью командлета [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) в административной оболочке PowerShell 6.

Обновление Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763) или более поздней версии:

```PowerShell
New-LocalUser -Name {NAME}
```

Версия Windows, предшествующая обновлению Windows 10 за октябрь 2018 г. (версия 1809, сборка 10.0.17763):

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

Укажите [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) при появлении соответствующего запроса.

Параметр срока действия учетной записи <xref:System.DateTime> можно задать с помощью параметра `-AccountExpires`, передаваемого в командлет [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser).

Дополнительные сведения см. в статьях [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) и [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей служб).

Альтернативный подход к управлению пользователями при работе с Active Directory заключается в применении управляемых учетных записей служб. Дополнительные сведения см. в [обзоре групповых управляемых учетных записей службы](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).

## <a name="log-on-as-a-service-rights"></a>Права на вход в качестве службы

Чтобы настроить право *Вход в качестве службы* для учетной записи пользователя службы, сделайте следующее:

1. Откройте редактор локальной политики безопасности, запустив *secpool.msc*.
1. Разверните узел **Локальные политики** и выберите **Назначение прав пользователя**.
1. Откройте политику **Вход в качестве службы**.
1. Щелкните **Добавить пользователя или группу**.
1. Укажите имя объекта (учетная запись пользователя) одним из следующих способов:
   1. Укажите учетную запись пользователя (`{DOMAIN OR COMPUTER NAME\USER}`) в поле для имени объекта и нажмите **ОК**, чтобы назначить политику пользователю.
   1. Выберите **Дополнительно**. Нажмите **Найти**. Выберите учетную запись пользователя из списка. Нажмите кнопку **ОК**. Нажмите **ОК** еще раз, чтобы назначить политику пользователю.
1. Нажмите **ОК** или **Применить**, чтобы сохранить изменения.

## <a name="create-and-manage-the-windows-service"></a>Создание службы Windows и управление ею

### <a name="create-a-service"></a>Создание службы

Зарегистрируйте службу с помощью команды PowerShell. В административной оболочке PowerShell 6 выполните следующие команды:

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* `{EXE PATH}` — путь к папке приложения на узле (например, `d:\myservice`). Не включайте исполняемый файл приложения в путь. Завершающая косая черта не требуется.
* `{DOMAIN OR COMPUTER NAME\USER}` —учетная запись пользователя службы (например, `Contoso\ServiceUser`).
* `{NAME}` — имя службы (например, `MyService`).
* `{EXE FILE PATH}` — путь к исполняемому файлу приложения (например, `d:\myservice\myservice.exe`). Включите имя исполняемого файла с расширением.
* `{DESCRIPTION}` — описание службы (например, `My sample service`).
* `{DISPLAY NAME}` — отображаемое имя службы (например, `My Service`).

### <a name="start-a-service"></a>Запуск службы

Запустите службу с помощью следующей команды PowerShell 6:

```powershell
Start-Service -Name {NAME}
```

Команде потребуется несколько секунд, чтобы запустить службу.

### <a name="determine-a-services-status"></a>Определение состояния службы

Чтобы проверить состояние службы, используйте следующую команду PowerShell 6:

```powershell
Get-Service -Name {NAME}
```

Состояние отображается одним из следующих значений:

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a>Остановка службы

Остановите службу с помощью следующей команды PowerShell 6:

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a>Удаление службы

После небольшой задержки для остановки службы удалите службу с помощью следующей команды Powershell 6:

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a>Обработка событий запуска и остановки

Чтобы обработать события <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> и <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, сделайте следующее:

1. Создайте класс, производный от <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService>, с методами `OnStarting`, `OnStarted` и `OnStopping`:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Создайте метод расширения для <xref:Microsoft.AspNetCore.Hosting.IWebHost>, который передает `CustomWebHostService` в <xref:System.ServiceProcess.ServiceBase.Run*>:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. В `Program.Main` вызовите метод расширения `RunAsCustomService` вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:

   ```csharp
   host.RunAsCustomService();
   ```

   Чтобы узнать расположение <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> в `Program.Main`, см. пример кода из раздела [Тип развертывания](#deployment-type).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Сценарии использования прокси-сервера и подсистемы балансировки нагрузки

Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка. Для получения дополнительной информации см. <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Настройка HTTPS

Чтобы настроить службу с защищенной конечной точкой, сделайте следующее:

1. Создайте сертификат X.509 для системы размещения с помощью механизмов получения и развертывания сертификата вашей платформы.

1. Укажите [конфигурацию конечной точки HTTPS для сервера Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration), чтобы использовать сертификат.

Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.

## <a name="current-directory-and-content-root"></a>Текущий каталог и корневой каталог содержимого

Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*. Папка *system32* не подходит для хранения файлов службы (например, файлов параметров). Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>Использование ContentRootPath или ContentRootFileProvider

Используйте [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) или <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> для поиска ресурсов приложения.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Указание папки приложения в качестве пути корневого каталога содержимого

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> — это тот же путь, который предоставляется аргументу `binPath` при создании службы. Чтобы не вызывать метод `GetCurrentDirectory` для создания путей к файлам параметров, вызовите <xref:System.IO.Directory.SetCurrentDirectory*> с указанным путем к корневому каталогу содержимого приложения.

В `Program.Main` определите путь к папке с исполняемым файлом службы и используйте этот путь, чтобы создать корневой каталог содержимого приложения.

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a>Хранение файлов службы в подходящем расположении на диске

Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.

## <a name="additional-resources"></a>Дополнительные ресурсы

::: moniker range=">= aspnetcore-3.0"

* [Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
