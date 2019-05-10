---
title: Безопасное хранение секретов приложения во время разработки в ASP.NET Core
author: rick-anderson
description: Узнайте, как сохранять и извлекать конфиденциальную информацию в виде секретов приложения во время разработки приложения ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 195901e466262020fd1217bd9dfb6162910bb861
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893221"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Безопасное хранение секретов приложения во время разработки в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Дэниэл рот](https://github.com/danroth27), и [Scott Addie](https://github.com/scottaddie)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([как скачивать](xref:index#how-to-download-a-sample))

В этом документе описываются методы для хранения и извлечения конфиденциальных данных во время разработки приложения ASP.NET Core. Никогда не храните пароли и другие конфиденциальные данные в исходном коде. Не следует использовать секреты рабочей среды для разработки или тестирования. Для хранения и защиты секретов Azure в ходе тестирования и непосредственной работы используйте [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Переменные среды

Переменные среды позволяют избежать хранение секретов приложений в коде или в локальных файлов конфигурации. Переменные среды переопределить значения конфигурации для всех источников ранее указанной конфигурации.

::: moniker range="<= aspnetcore-1.1"

Настройки чтения значений переменных среды путем вызова <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в `Startup` конструктор:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

Рассмотрим веб-приложение ASP.NET Core, в котором **учетные записи отдельных пользователей** включена безопасность. Строку подключения базы данных по умолчанию включается в проект *appsettings.json* файл с ключом `DefaultConnection`. Строка подключения по умолчанию — для LocalDB, который работает в режиме пользователя и пароля не требуется. Во время развертывания приложения `DefaultConnection` значение ключа может быть заменено значение переменной среды. Полная строка подключения с конфиденциальные учетные данные могут хранить в переменной среды.

> [!WARNING]
> Переменные среды обычно хранятся в обычный и незашифрованное текста. Если компьютер или процесс нарушена, переменные среды может осуществляться с недоверенным сторонам. Могут потребоваться дополнительные меры, чтобы предотвратить раскрытие секретных данных пользователя.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>Менеджер секретов

Средство Secret Manager сохраняет конфиденциальные данные во время разработки проекта ASP.NET Core. В этом контексте конфиденциальных данных представляет секрет приложения. Секреты приложения хранятся в отдельном расположении в дереве проекта. Секреты приложения связанные с определенным проектом или совместно используется несколькими проектами. Секреты приложения не проверяются в систему управления версиями.

> [!WARNING]
> Средство Secret Manager не шифровать хранимые секреты и не должен рассматриваться в качестве доверенного хранилища. Это только в целях разработки. Ключи и значения хранятся в файле конфигурации JSON в каталоге профиля пользователя.

## <a name="how-the-secret-manager-tool-works"></a>Как работает менеджером секретов

Средство Secret Manager абстрагирует детали реализации, например где и как значения сохраняются. Можно использовать средство, не зная эти детали реализации. Значения хранятся в файле конфигурации JSON в папке профиля пользователя, система защищена на локальном компьютере:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Путь файловой системы:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Путь файловой системы:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

В предыдущем пути к файлам, замените `<user_secrets_id>` с `UserSecretsId` значение, указанное в *.csproj* файл.

Не создавать код, зависящий от расположения или формата данных, сохраненных с помощью диспетчера секретов. Эти детали реализации могут быть изменены. Например его значения не шифруются, но может быть в будущем.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Установите средство Secret Manager

Средство Secret Manager связке с .NET Core CLI в .NET Core SDK 2.1.300 или более поздней версии. Для версий пакета SDK для .NET Core до 2.1.300 необходим установки средства.

> [!TIP]
> Запустите `dotnet --version` из командной оболочки, чтобы увидеть установленный номер версии пакета SDK для .NET Core.

Если пакет SDK для .NET Core используется включает средство, выводится предупреждение:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Установка [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) пакет NuGet в проекте ASP.NET Core. Пример:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Выполните следующую команду в командной строке для проверки установки средства:

```console
dotnet user-secrets -h
```

Средство Secret Manager отображает пример использования, параметры и команды help:

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> Должен быть в том же каталоге, что *.csproj* файл для запуска средства, предназначенные для *.csproj* файла `DotNetCliToolReference` элементов.

::: moniker-end

## <a name="enable-secret-storage"></a>Включение хранилища секретов

Средство Secret Manager работает настройки конфигурации конкретного проекта, хранящиеся в профиле пользователя.

::: moniker range=">= aspnetcore-3.0"

Включает средство Secret Manager `init` в пакет SDK для .NET Core 3.0.100 команду или более поздней версии. Чтобы использовать секреты пользователя, выполните следующую команду в каталоге проекта:

```console
dotnet user-secrets init
```

Приведенная выше команда добавляет `UserSecretsId` сервисном `PropertyGroup` из *.csproj* файл. По умолчанию текст внутри `UserSecretsId` представляет собой идентификатор GUID. Внутренний текст может быть произвольным, но является уникальным для проекта.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Чтобы использовать секреты пользователя, определить `UserSecretsId` сервисном `PropertyGroup` из *.csproj* файл. Внутренний текст `UserSecretsId` может быть произвольным, но является уникальным для проекта. Разработчики обычно создать GUID для `UserSecretsId`.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> В Visual Studio щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление секретами пользователей** в контекстном меню. Добавляет этот жест `UserSecretsId` элемент, заполненный идентификатора GUID, что в *.csproj* файл.

## <a name="set-a-secret"></a>Значение секрета

Определите секрет приложения, состоящие из ключа и его значение. Секрет связан с проектом `UserSecretsId` значение. Например, выполните следующую команду из каталога, в котором *.csproj* файл существует:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

В приведенном выше примере двоеточия обозначает, что `Movies` — это объект литерал с `ServiceApiKey` свойство.

Средство Secret Manager можно использовать из других каталогов. Используйте `--project` параметр, чтобы указать путь в файловой системе, по которому *.csproj* файл существует. Пример:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Структура JSON, сведение в Visual Studio

Visual Studio **управление секретами пользователей** жестов откроется *secrets.json* файл в текстовом редакторе. Замените содержимое файла *secrets.json* с парами "ключ значение" для сохранения. Пример:

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

Структура JSON выравнивается после внесения изменений с помощью `dotnet user-secrets remove` или `dotnet user-secrets set`. Например, на котором работают `dotnet user-secrets remove "Movies:ConnectionString"` сворачивает `Movies` литерала объекта. Измененный файл будет выглядеть примерно так:

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>Задайте несколько секретных кодов

Пакет секреты можно задать путем передачи JSON для `set` команды. В следующем примере *input.json* содержимое файла передаются по конвейеру в `set` команды.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Откройте командную оболочку и выполните следующую команду:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Откройте командную оболочку и выполните следующую команду:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Доступ к секрета

[API конфигурации ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к Secret Manager секреты.

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Если проект нацелен на .NET Framework, установить [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

В ASP.NET Core 2.0 или более поздней версии, источник конфигурации секреты пользователя автоматически добавляется в режиме разработки, пока проект не вызывает <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> для инициализации нового экземпляра узла с помощью предварительно настроенных значений по умолчанию. `CreateDefaultBuilder` вызовы <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> при <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> — <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Когда `CreateDefaultBuilder` не вызывается, добавить источник конфигурации секреты пользователя явно, вызвав <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> в `Startup` конструктор. Вызовите `AddUserSecrets` только когда приложение выполняется в среде разработки, как показано в следующем примере:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Установка [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.

Добавить источник конфигурации секреты пользователя с помощью вызова <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> в `Startup` конструктор:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Секреты пользователя можно получить через `Configuration` API:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Сопоставить секреты POCO

Сопоставление всей литерала объекта POCO (простой класс .NET со свойствами) полезен для объединения связанных свойств.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Чтобы сопоставить выше секреты POCO, используйте `Configuration` API [привязке графа объектов](xref:fundamentals/configuration/index#bind-to-an-object-graph) функции. Следующий код привязывается к пользовательской `MovieSettings` POCO и обращается к `ServiceApiKey` значение свойства:

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

`Movies:ConnectionString` И `Movies:ServiceApiKey` секреты, сопоставляются с соответствующих свойств в `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Строка замены с секретами

Хранить пароли в виде обычного текста небезопасно. Например, строку подключения базы данных хранятся в *appsettings.json* может быть указан пароль для указанного пользователя:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Более безопасный подход — в том, чтобы сохранить пароль в качестве секрета. Пример:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Удалить `Password` пару ключ значение из строки подключения в *appsettings.json*. Пример:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Можно задать значение секрета <xref:System.Data.SqlClient.SqlConnectionStringBuilder> объекта <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> свойство, чтобы завершить строку подключения:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Вывод списка секретов

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Выполните следующую команду из каталога, в котором *.csproj* файл существует:

```console
dotnet user-secrets list
```

Появится следующий результат:

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

В приведенном выше примере двоеточия в имена ключей означает иерархии объектов в пределах *secrets.json*.

## <a name="remove-a-single-secret"></a>Удалить секрет единого

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Выполните следующую команду из каталога, в котором *.csproj* файл существует:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

Приложения *secrets.json* файл был изменен, чтобы удалить пару ключ значение, связанных с `MoviesConnectionString` ключ:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Под управлением `dotnet user-secrets list` отображается следующее сообщение:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Удалить все секреты

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Выполните следующую команду из каталога, в котором *.csproj* файл существует:

```console
dotnet user-secrets clear
```

Все секреты пользователя приложения будут удалены из *secrets.json* файла:

```json
{}
```

Под управлением `dotnet user-secrets list` отображается следующее сообщение:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
