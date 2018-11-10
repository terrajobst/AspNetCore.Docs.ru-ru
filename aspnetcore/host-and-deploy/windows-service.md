---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758197"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Размещение ASP.NET Core в службе Windows

Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)

Приложение ASP.NET Core можно разместить в Windows без использования IIS в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Преобразование проекта в службу Windows

Чтобы настроить запуск в виде службы для существующего проекта ASP.NET Core, выполните следующий минимальный набор изменений.

1. В файле проекта:

   * Подтвердите наличие [идентификатора среды выполнения](/dotnet/core/rid-catalog) Windows или добавьте его в `<PropertyGroup>`, где содержится требуемая версия платформы:

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.

      * Укажите список идентификаторов RID, разделив их точкой с запятой.
      * Укажите имя свойства `<RuntimeIdentifiers>` (множественное число).

      Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).

   * Добавьте ссылку на пакет для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Внесите следующие изменения в `Program.Main`:

   * Вызовите [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) вместо `host.Run`.

   * Вызовите [UseContentRoot](xref:fundamentals/host/web-host#content-root) и используйте путь к расположению публикации приложения вместо `Directory.GetCurrentDirectory()`.

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. Опубликуйте приложение с помощью команды [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), [профиля публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) или Visual Studio Code. Если вы используете Visual Studio, выберите **FolderProfile** и настройте **целевое расположение**, прежде чем нажимать кнопку **Опубликовать**.

   Чтобы опубликовать пример приложения через интерфейс командной строки (CLI), выполните в командной строке команду [dotnet publish](/dotnet/core/tools/dotnet-publish) в папке проекта. Идентификатор RID следует указать в свойстве `<RuntimeIdenfifier>` (или `<RuntimeIdentifiers>`) в файле проекта. В следующем примере публикуется приложение в конфигурации выпуска для среды выполнения `win7-x64` в папке, созданной в каталоге *c:\\svc*:

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. Создайте учетную запись пользователя для службы с помощью команды `net user`:

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   Для примера приложения создайте учетную запись пользователя с именем `ServiceUser` и пароль. В следующей команде замените `{PASSWORD}` на [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   Если вам нужно добавить пользователя в группу, используйте команду `net localgroup`, где `{GROUP}` — это имя группы:

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   Дополнительные сведения см. в статье [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей службы).

1. Предоставьте доступ на запись, чтение и выполнение для папки приложения с помощью команды [icacls](/windows-server/administration/windows-commands/icacls):

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * `{PATH}` &ndash; путь к папке приложения.
   * `{USER ACCOUNT}` &ndash; учетная запись пользователя (SID).
   * `(OI)` &ndash; флаг наследования объекта, который распространяет разрешения на вложенные файлы.
   * `(CI)` &ndash; флаг наследования контейнера, который распространяет разрешения на вложенные папки.
   * `{PERMISSION FLAGS}` &ndash; устанавливает разрешения для доступа к приложениям.
     * Запись (`W`)
     * Чтение (`R`)
     * Выполнение (`X`)
     * Полное (`F`)
     * Изменение (`M`)
   * `/t` &ndash; применяется рекурсивно к имеющимся вложенным папкам и файлам.

   Для примера приложения, опубликованного в папке *c:\\svc*, и учетной записи `ServiceUser` с разрешениями на запись, чтение и выполнение используйте следующую команду:

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   Дополнительные сведения см. в статье об [icacls](/windows-server/administration/windows-commands/icacls).

1. Используйте программу командной строки [sc.exe](https://technet.microsoft.com/library/bb490995), чтобы создать службу. Значение `binPath` обозначает путь к исполняемому файлу приложения, включая имя самого файла. **Требуется пробел между знаком равенства и кавычками для каждого обязательного параметра и значения.**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}` &ndash; имя, присваиваемое службе в [диспетчере служб](/windows/desktop/services/service-control-manager).
   * `{PATH}` &ndash; путь к исполняемому файлу.
   * `{DOMAIN}` (если компьютер не присоединен к домену, имя локального компьютера) и `{USER ACCOUNT}` &ndash; домен (или имя локального компьютера) и учетная запись пользователя, под которой выполняется служба. **Не** пропускайте параметр `obj`. Значение по умолчанию параметра `obj` — это [учетная запись LocalSystem](/windows/desktop/services/localsystem-account). Выполнение службы под учетной записью `LocalSystem` представляет собой серьезную угрозу безопасности. Всегда запускайте службу под учетной записью пользователя с ограниченными привилегиями на сервере.
   * `{PASSWORD}` &ndash; пароль учетной записи пользователя.

   В следующем примере:

   * Служба называется **MyService**.
   * Опубликованная служба размещается в папке *c:\\svc*. Имя исполняемого файла приложения — *AspNetCoreService.exe*. Значение `binPath` заключено в прямые кавычки (").
   * Служба работает под учетной записью `ServiceUser`. Замените `{DOMAIN}` на домен учетной записи пользователя или имя локального компьютера. Заключите значение `obj` в прямые кавычки ("). Пример: если система размещения — это локальный компьютер с именем `MairaPC`, задайте для параметра `obj` значение `"MairaPC\ServiceUser"`.
   * Замените `{PASSWORD}` на пароль учетной записи пользователя. Значение `password` заключено в прямые кавычки (").

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > Убедитесь, что между знаками равенства и значениями параметров есть пробелы.

1. Запустите службу с помощью команды `sc start {SERVICE NAME}`.

   Чтобы запустить пример службы приложения, используйте следующую команду:

   ```console
   sc start MyService
   ```

   Команде потребуется несколько секунд, чтобы запустить службу.

1. Чтобы проверить состояние службы, используйте команду `sc query {SERVICE NAME}`. Состояние отображается одним из следующих значений:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Чтобы проверить состояние примера службы приложения, используйте следующую команду:

   ```console
   sc query MyService
   ```

1. Если служба находится в состоянии `RUNNING` и является веб-приложением, найдите приложение по его пути (по умолчанию `http://localhost:5000`, который перенаправляет на `https://localhost:5001` при использовании [ПО промежуточного слоя перенаправления на HTTPS](xref:security/enforcing-ssl)).

   Чтобы получить пример службы приложений, найдите приложение по адресу `http://localhost:5000`.

1. Остановите службу с помощью команды `sc stop {SERVICE NAME}`.

   Чтобы остановить пример службы приложения, используйте следующую команду:

   ```console
   sc stop MyService
   ```

1. После небольшой задержки для остановки службы удалите службу с помощью команды `sc delete {SERVICE NAME}`.

   Проверьте состояние примера службы приложений:

   ```console
   sc query MyService
   ```

   Когда пример службы приложений находится в состоянии `STOPPED`, используйте следующую команду для удаления примера службы приложений:

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a>Запуск приложения вне службы

Тестирование и отладку проще выполнять при работе вне службы. Поэтому разработчики обычно добавляют код, который вызывает `RunAsService` только при соблюдении определенных условий. Например, приложение может выполняться как консольное приложение с аргументом командной строки `--console` или при присоединении отладчика:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Так как для конфигурации ASP.NET Core требуется пара имя-значение для аргументов командной строки, параметр `--console` удаляется до передачи аргументов в [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.

## <a name="handle-stopping-and-starting-events"></a>Обработка событий остановки и запуска

Для обработки событий [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) и [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) внесите следующие дополнительные изменения:

1. Создайте класс, производный от [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Создайте метод расширения для [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), который передает пользовательский параметр `WebHostService` в [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. В `Program.Main` измените вызов [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) на вызов нового метода расширения `RunAsCustomService`:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.

Если пользовательский код `WebHostService` обращается к службе путем внедрения зависимости (например, к средству ведения журнала), ее можно получить из свойства [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Сценарии использования прокси-сервера и подсистемы балансировки нагрузки

Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка. Дополнительные сведения см. в разделе <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Настройка HTTPS

Чтобы настроить службу с защищенной конечной точкой, сделайте следующее:

1. Создайте сертификат X.509 для системы размещения с помощью механизмов получения и развертывания сертификата вашей платформы.
1. Укажите [конфигурацию конечной точки HTTPS для сервера Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration), чтобы использовать сертификат.

Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.

## <a name="current-directory-and-content-root"></a>Текущий каталог и корневой каталог содержимого

Для службы Windows `Directory.GetCurrentDirectory()` возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*. Папка *system32* не подходит для хранения файлов службы (например, файлов параметров). Используйте один из следующих методов для сохранения и доступа к ресурсам и файлам параметров службы с помощью [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) при использовании [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):

* Используйте путь корневого каталога содержимого. `IHostingEnvironment.ContentRootPath` — это тот же путь, предоставленный аргументом `binPath` при создании службы. Вместо использования `Directory.GetCurrentDirectory()` для создания путей к файлам параметров используйте путь корневого каталога содержимого и храните файлы в корневом каталоге содержимого приложения.
* Храните файлы в подходящем месте на диске. Укажите абсолютный путь с помощью `SetBasePath` к папке, содержащей файлы.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)
* <xref:fundamentals/host/web-host>
