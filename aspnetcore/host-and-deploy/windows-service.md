---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 544eefa87898e82ec2bf8f9f61ce4e26dd554bb7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068340"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Размещение ASP.NET Core в службе Windows

Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)

Приложение ASP.NET Core можно разместить в Windows в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) без использования IIS. При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

* [PowerShell 6.2 или более поздней версии.](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> Для версий ОС Windows, предшествующих Windows 10 с обновлением за октябрь 2018 г. (версия 1809, сборка 10.0.17763), необходимо импортировать модули [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) и [WindowsCompatibility](https://github.com/PowerShell/WindowsCompatibility), чтобы включить поддержку командлета [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser), используемого в разделе [Создание учетной записи пользователя](#create-a-user-account).
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a>Тип развертывания

Вы можете создать зависящее от платформы или автономное развертывание службы Windows. Дополнительные сведения и рекомендации по сценариям развертывания см. в статье [Развертывание приложений .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment"></a>развертывание, зависящее от платформы;

Зависящее от платформы развертывание (FDD) требует наличия в целевой системе общей для всей системы версии .NET Core. При использовании сценария с FDD с приложением ASP.NET Core (службой Windows) с помощью пакета SDK создается исполняемый файл (*\*.exe*), который называется *исполняемым файлом, зависящим от платформы*.

### <a name="self-contained-deployment"></a>автономное развертывание;

Для автономного развертывания (SCD) общие компоненты в целевой системе не требуются. Среда выполнения и зависимости приложения развертываются с приложением в системе для размещения.

## <a name="convert-a-project-into-a-windows-service"></a>Преобразование проекта в службу Windows

Внесите следующие изменения в существующий проект ASP.NET Core, чтобы запустить приложение в качестве службы:

### <a name="project-file-updates"></a>Обновления файла проекта

Измените файл проекта с учетом выбранного [типа развертывания](#deployment-type).

#### <a name="framework-dependent-deployment-fdd"></a>Зависящее от платформы развертывание

Добавьте [идентификатор среды выполнения](/dotnet/core/rid-catalog) Windows в `<PropertyGroup>`, где содержится требуемая версия платформы. В следующем примере для RID задано значение `win7-x64`. Добавьте свойство `<SelfContained>` со значением `false`. Эти свойства дают пакету SDK инструкцию создать исполняемый файл (*EXE*) для Windows.

Файл *web.config*, который обычно создается при публикации приложения ASP.NET Core, не требуется для приложения служб Windows. Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.

::: moniker range=">= aspnetcore-2.2"

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

Добавьте свойство `<UseAppHost>` со значением `true`. Это свойство предоставляет службу с путем активации (исполняемый файл, *EXE*) для FDD.

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

#### <a name="self-contained-deployment-scd"></a>Автономное развертывание

Подтвердите наличие [идентификатора среды выполнения](/dotnet/core/rid-catalog) Windows или добавьте его в `<PropertyGroup>`, где содержится требуемая версия платформы. Отмените создание файла *web.config*, добавив свойство `<IsTransformWebConfigDisabled>` со значением `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.

* Укажите список идентификаторов RID, разделив их точкой с запятой.
* Укажите имя свойства `<RuntimeIdentifiers>` (множественное число).

  Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).

Добавьте ссылку на пакет для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

Чтобы включить ведение журнала событий Windows, добавьте ссылку на пакет для [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Дополнительные сведения см. в разделе [Обработка событий запуска и остановки](#handle-starting-and-stopping-events).

### <a name="programmain-updates"></a>Обновления Program.Main

Внесите следующие изменения в `Program.Main`:

* Для тестирования и отладки при работе вне службы добавьте код, чтобы определить, как выполняется приложение: в качестве службы или консольного приложения. Проверьте, присоединен ли отладчик или присутствует ли аргумент командной строки `--console`.

  Если одно из условий имеет значение true (приложение выполняется не в качестве службы), вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> на веб-узле.

  Если условия имеют значение false (приложение выполняется в качестве службы), сделайте следующее:

  * Вызовите <xref:System.IO.Directory.SetCurrentDirectory*> и используйте путь к расположению для публикации приложения. Не вызывайте <xref:System.IO.Directory.GetCurrentDirectory*> для получения пути, так как при вызове <xref:System.IO.Directory.GetCurrentDirectory*> приложение службы Windows возвращает папку *C:\\WINDOWS\\system32*. Дополнительные сведения см. в разделе [Текущий каталог и корневой каталог содержимого](#current-directory-and-content-root).
  * Вызовите <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, чтобы запустить приложение в качестве службы.

  Так как для [поставщика конфигурации командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) требуется пара имя-значение для аргументов командной строки, параметр `--console` удаляется из аргументов, прежде чем <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> получит его.

* Для записи данных в журнал событий Windows добавьте поставщик EventLog в <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Задайте уровень ведения журнала с помощью ключа `Logging:LogLevel:Default` в файле *appsettings.Production.json*. Для демонстрации и тестирования в файле параметров примера приложения для рабочей среды мы укажем такой уровень ведения журнала: `Information`. В рабочей среде обычно присваивается значение `Error`. Для получения дополнительной информации см. <xref:fundamentals/logging/index#windows-eventlog-provider>.

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a>Публикация приложения

Опубликуйте приложение с помощью команды [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), [профиля публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) или Visual Studio Code. Если вы используете Visual Studio, выберите **FolderProfile** и настройте **целевое расположение**, прежде чем нажимать кнопку **Опубликовать**.

Чтобы опубликовать пример приложения с помощью интерфейса командной строки (CLI), выполните в командной строке Windows команду [dotnet publish](/dotnet/core/tools/dotnet-publish) в папке проекта с конфигурацией выпуска, передаваемой параметру [-c|--configuration](/dotnet/core/tools/dotnet-publish#options). Чтобы опубликовать этот пример в папке за пределами приложения, задайте параметр [-o|--output](/dotnet/core/tools/dotnet-publish#options) и укажите путь.

### <a name="publish-a-framework-dependent-deployment-fdd"></a>Публикация зависящего от платформы развертывания

В следующем примере приложение публикуется в папке *c:\\svc*.

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a>Публикация автономного развертывания

Идентификатор RID следует указать в свойстве `<RuntimeIdenfifier>` (или `<RuntimeIdentifiers>`) в файле проекта. Укажите среду выполнения в параметре [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) команды `dotnet publish`.

В следующем примере приложение публикуется для среды выполнения `win7-x64` в папке *c:\\svc*.

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a>Создание учетной записи пользователя

Создайте для службы учетную запись пользователя с помощью командлета [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) из административной командной оболочки PowerShell 6.

```powershell
New-LocalUser -Name {NAME}
```

Укажите [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) при появлении соответствующего запроса.

Для примера приложения создайте учетную запись пользователя с именем `ServiceUser`.

```powershell
New-LocalUser -Name ServiceUser
```

Параметр срока действия учетной записи <xref:System.DateTime> можно задать с помощью параметра `-AccountExpires`, передаваемого в командлет [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser).

Дополнительные сведения см. в статьях [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) и [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей служб).

Альтернативный подход к управлению пользователями при работе с Active Directory заключается в применении управляемых учетных записей служб. Дополнительные сведения см. в [обзоре групповых управляемых учетных записей службы](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).

## <a name="set-permission-log-on-as-a-service"></a>Установка разрешений: Вход в систему в качестве службы

Предоставьте доступ на запись, чтение и выполнение к папке приложения с помощью команды [icacls](/windows-server/administration/windows-commands/icacls) из административной командной оболочки PowerShell 6.

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* `{PATH}` — путь к папке приложения.
* `{USER ACCOUNT}` — учетная запись пользователя (SID).
* `(OI)` — флаг наследования объекта, который распространяет разрешения на вложенные файлы.
* `(CI)` — флаг наследования контейнера, который распространяет разрешения на вложенные папки.
* `{PERMISSION FLAGS}` — устанавливает права доступа к приложению.
  * Запись (`W`)
  * Чтение (`R`)
  * Выполнение (`X`)
  * Полное (`F`)
  * Изменение (`M`)
* `/t` — применяется рекурсивно к существующим вложенным папкам и файлам.

Для примера приложения, опубликованного в папке *c:\\svc*, и учетной записи `ServiceUser` с разрешениями на запись, чтение и выполнение выполните следующую команду в административной командной оболочке PowerShell 6:

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

Дополнительные сведения см. в статье об [icacls](/windows-server/administration/windows-commands/icacls).

## <a name="create-the-service"></a>Создание службы

Используйте скрипт PowerShell [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts), чтобы зарегистрировать службу. Из административной командной оболочки PowerShell 6 выполните скрипт с помощью такой команды:

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

В примере ниже для примера приложения указано следующее:

* Служба называется **MyService**.
* Опубликованная служба размещается в папке *c:\\svc*. Исполняемый файл приложения с именем *SampleApp.exe*.
* Служба работает под учетной записью `ServiceUser`. В следующем примере команды `Desktop-PC` является именем локального компьютера. Замените `Desktop-PC` именем компьютера или доменом своей системы.

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a>Управление службой

### <a name="start-the-service"></a>Запуск службы

Запустите службу с помощью команды PowerShell 6 `Start-Service -Name {NAME}`.

Чтобы запустить пример службы приложения, используйте следующую команду:

```powershell
Start-Service -Name MyService
```

Команде потребуется несколько секунд, чтобы запустить службу.

### <a name="determine-the-service-status"></a>Определение состояния службы

Чтобы проверить состояние службы, используйте команду PowerShell 6 `Get-Service -Name {NAME}`. Состояние отображается одним из следующих значений:

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

Чтобы проверить состояние примера службы приложения, используйте следующую команду:

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a>Обзор службы веб-приложений

Если служба находится в состоянии `RUNNING` и является веб-приложением, найдите приложение по его пути (по умолчанию `http://localhost:5000`, который перенаправляет на `https://localhost:5001` при использовании [ПО промежуточного слоя перенаправления на HTTPS](xref:security/enforcing-ssl)).

Чтобы получить пример службы приложений, найдите приложение по адресу `http://localhost:5000`.

### <a name="stop-the-service"></a>Остановите службу

Остановите службу с помощью команды PowerShell 6 `Stop-Service -Name {NAME}`.

Чтобы остановить пример службы приложения, используйте следующую команду:

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a>Удаление службы

После небольшой задержки для остановки службы удалите службу с помощью команды Powershell 6 `Remove-Service -Name {NAME}`.

Проверьте состояние примера службы приложений:

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a>Обработка событий запуска и остановки

Чтобы правильно обрабатывать события <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> и <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, внесите дополнительные изменения:

1. Создайте класс, производный от <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService>, с методами `OnStarting`, `OnStarted` и `OnStopping`:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Создайте метод расширения для <xref:Microsoft.AspNetCore.Hosting.IWebHost>, который передает `CustomWebHostService` в <xref:System.ServiceProcess.ServiceBase.Run*>:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. В `Program.Main` вызовите метод расширения `RunAsCustomService` вместо <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:

   ```csharp
   host.RunAsCustomService();
   ```

   Чтобы узнать расположение <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> в `Program.Main`, см. пример кода, приведенный в разделе [Преобразование проекта в службу Windows](#convert-a-project-into-a-windows-service).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Сценарии использования прокси-сервера и подсистемы балансировки нагрузки

Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка. Для получения дополнительной информации см. <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Настройка HTTPS

Чтобы настроить службу с защищенной конечной точкой, сделайте следующее:

1. Создайте сертификат X.509 для системы размещения с помощью механизмов получения и развертывания сертификата вашей платформы.

1. Укажите [конфигурацию конечной точки HTTPS для сервера Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration), чтобы использовать сертификат.

Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.

## <a name="current-directory-and-content-root"></a>Текущий каталог и корневой каталог содержимого

Для службы Windows <xref:System.IO.Directory.GetCurrentDirectory*> возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*. Папка *system32* не подходит для хранения файлов службы (например, файлов параметров). Используйте один из следующих методов для сохранения ресурсов и файлов параметров службы и доступа к ним.

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Указание папки приложения в качестве пути корневого каталога содержимого

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> — это тот же путь, предоставленный аргументом `binPath` при создании службы. Чтобы не вызывать метод `GetCurrentDirectory` для создания путей к файлам параметров, вызовите <xref:System.IO.Directory.SetCurrentDirectory*> с указанным путем к корневому каталогу содержимого приложения.

В `Program.Main` определите путь к папке с исполняемым файлом службы и используйте этот путь, чтобы создать корневой каталог содержимого приложения.

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a>Хранение файлов службы в подходящем месте на диске

Укажите абсолютный путь к папке, содержащей файлы, с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> при использовании <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
