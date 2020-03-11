---
title: Надежное хранение секретов приложений в разработке в ASP.NET Core
author: rick-anderson
description: Узнайте, как хранить и извлекать конфиденциальную информацию в виде секретов приложения во время разработки ASP.NET Core приложения.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: security/app-secrets
ms.openlocfilehash: c3f165164f3c95e8c0aab773f3731429ae224bd9
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654694"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Надежное хранение секретов приложений в разработке в ASP.NET Core

[Рик Андерсон (](https://twitter.com/RickAndMSFT), [Даниэль Roth)](https://github.com/danroth27)и [Скотт Эдди (](https://github.com/scottaddie)

[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([как скачивать](xref:index#how-to-download-a-sample))

В этом документе объясняются способы хранения и извлечения конфиденциальных данных во время разработки ASP.NET Core приложения на компьютере разработки. Никогда не храните пароли или другие конфиденциальные данные в исходном коде. Производственные секреты не должны использоваться для разработки или тестирования. Секреты не должны развертываться вместе с приложением. Вместо этого секреты должны быть доступны в рабочей среде с помощью контролируемых средств, таких как переменные среды, Azure Key Vault и т. д. Вы можете хранить и защищать тестовые и рабочие секреты Azure с помощью [поставщика конфигурации Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Переменные среды

Переменные среды используются, чтобы избежать хранения секретов приложения в коде или в локальных файлах конфигурации. Переменные среды переопределяют значения конфигурации для всех ранее указанных источников конфигурации.

::: moniker range="<= aspnetcore-1.1"

Настройте чтение значений переменных среды, вызвав <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> в конструкторе `Startup`:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

Рассмотрим ASP.NET Core веб-приложение, в котором включена безопасность **отдельных учетных записей пользователей** . Строка подключения к базе данных по умолчанию включается в файл *appSettings. JSON* проекта с ключевым `DefaultConnection`. Строка подключения по умолчанию — для LocalDB, которая выполняется в пользовательском режиме и не требует пароля. Во время развертывания приложения значение ключа `DefaultConnection` может быть переопределено значением переменной среды. Переменная среды может хранить полную строку подключения с конфиденциальными учетными данными.

> [!WARNING]
> Переменные среды обычно хранятся в виде обычного незашифрованного текста. Если компьютер или процесс скомпрометирован, недоверенные стороны могут получить доступ к переменным среды. Могут потребоваться дополнительные меры для предотвращения раскрытия секретов пользователя.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>Диспетчер секретов

Средство диспетчера секретов сохраняет конфиденциальные данные во время разработки проекта ASP.NET Core. В этом контексте фрагмент конфиденциальных данных является секретом приложения. Секреты приложения хранятся в отдельном месте из дерева проекта. Секреты приложения связаны с конкретным проектом или совместно используются в нескольких проектах. Секреты приложения не возвращаются в систему управления версиями.

> [!WARNING]
> Диспетчер секретов не шифрует сохраненные секреты и не должен рассматриваться как доверенное хранилище. Это только для целей разработки. Ключи и значения хранятся в файле конфигурации JSON в каталоге профиля пользователя.

## <a name="how-the-secret-manager-tool-works"></a>Как работает средство диспетчера секретов

Диспетчер секретов абстрагирует детали реализации, например, где и как хранятся значения. Вы можете использовать это средство, не зная сведений о реализации. Значения хранятся в файле конфигурации JSON в папке профиля пользователя, защищенной системой, на локальном компьютере:

# <a name="windows"></a>[Windows](#tab/windows)

Путь к файловой системе:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macos"></a>[Linux и macOS](#tab/linux+macos)

Путь к файловой системе:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

В приведенных выше путях файлов замените `<user_secrets_id>` значением `UserSecretsId`, указанным в файле *CSPROJ* .

Не записывайте код, который зависит от расположения или формата данных, сохраненных с помощью диспетчера секретов. Эти сведения о реализации могут измениться. Например, секретные значения не шифруются, но могут быть в будущем.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Установка диспетчера секретов

Диспетчер секретов объединяется с .NET Core CLI в пакет SDK для .NET Core 2.1.300 или более поздней версии. Для пакет SDK для .NET Core версий перед 2.1.300 необходимо установить средство.

> [!TIP]
> Запустите `dotnet --version` из командной оболочки, чтобы просмотреть номер версии установленного пакет SDK для .NET Core.

Если используемое пакет SDK для .NET Core включает средство, отображается предупреждение:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Установите пакет NuGet [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) в проекте ASP.NET Core. Пример:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Выполните следующую команду в командной оболочке, чтобы проверить установку средства:

```dotnetcli
dotnet user-secrets -h
```

В средстве "Диспетчер секретов" отображаются примеры использования, параметры и Справка по командам:

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
> Для запуска средств, определенных в элементах `DotNetCliToolReference` файла *. csproj* , необходимо находиться в том же каталоге, что и *CSPROJ* .

::: moniker-end

## <a name="enable-secret-storage"></a>Включить хранилище секретов

Диспетчер секретов работает с параметрами конфигурации конкретного проекта, хранящимися в профиле пользователя.

::: moniker range=">= aspnetcore-3.0"

Диспетчер секретов включает команду `init` в пакет SDK для .NET Core 3.0.100 или более поздней версии. Чтобы использовать секреты пользователя, выполните следующую команду в каталоге проекта:

```dotnetcli
dotnet user-secrets init
```

Предыдущая команда добавляет элемент `UserSecretsId` в `PropertyGroup` *CSPROJ* -файла. По умолчанию внутренний текст `UserSecretsId` является идентификатором GUID. Внутренний текст является произвольным, но уникален для проекта.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Чтобы использовать секреты пользователя, определите элемент `UserSecretsId` в `PropertyGroup` *CSPROJ* -файле. Внутренний текст `UserSecretsId` является произвольным, но уникален для проекта. Разработчики обычно создают идентификатор GUID для `UserSecretsId`.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> В Visual Studio щелкните правой кнопкой мыши проект в обозреватель решений и выберите в контекстном меню пункт **Управление секретами пользователя** . Этот жест добавляет элемент `UserSecretsId`, заполненный идентификатором GUID, в *CSPROJ* -файл.

## <a name="set-a-secret"></a>Задать секрет

Определите секрет приложения, состоящий из ключа и его значения. Секрет связан со значением `UserSecretsId` проекта. Например, выполните следующую команду из каталога, в котором существует CSPROJ файл *.*

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

В предыдущем примере двоеточие означает, что `Movies` является объектным литералом со свойством `ServiceApiKey`.

Диспетчер секретов можно использовать и в других каталогах. Используйте параметр `--project`, чтобы указать путь к файловой системе, в которой существует файл *. csproj* . Пример:

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Сведение структуры JSON в Visual Studio

Жест **управления пользовательскими секретами** в Visual Studio открывает файл *секреты. JSON* в текстовом редакторе. Замените содержимое файла *секреты. JSON* хранимыми парами "ключ-значение". Пример:

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

Структура JSON преобразуется в плоскую структуру после изменений с помощью `dotnet user-secrets remove` или `dotnet user-secrets set`. Например, выполнение `dotnet user-secrets remove "Movies:ConnectionString"` сворачивает `Movies` литерал объекта. Измененный файл выглядит следующим образом:

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>Задание нескольких секретов

Пакет секретов можно задать с помощью конвейера JSON в команду `set`. В следующем примере содержимое *входного файла JSON* передается команде `set`.

# <a name="windows"></a>[Windows](#tab/windows)

Откройте командную оболочку и выполните следующую команду:

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macos"></a>[Linux и macOS](#tab/linux+macos)

Откройте командную оболочку и выполните следующую команду:

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Доступ к секрету

[API настройки ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к секретам диспетчера секретов.

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Если проект предназначен для .NET Framework, установите пакет NuGet [Microsoft. Extensions. Configuration. усерсекретс](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

В ASP.NET Core 2,0 или более поздней версии источник конфигурации секреты пользователя автоматически добавляется в режим разработки, когда проект вызывает <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> для инициализации нового экземпляра узла с предварительно настроенными настройками по умолчанию. `CreateDefaultBuilder` вызывает <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>, когда <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>:

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Если `CreateDefaultBuilder` не вызывается, добавьте источник конфигурации секреты пользователя явным образом, вызвав <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> в конструкторе `Startup`. Вызывайте `AddUserSecrets` только при запуске приложения в среде разработки, как показано в следующем примере:

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Установите пакет NuGet [Microsoft. Extensions. Configuration. усерсекретс](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .

Добавьте источник конфигурации секреты пользователя с вызовом <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> в конструкторе `Startup`:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Секреты пользователя можно получить с помощью `Configuration` API:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Преобразование секретов в POCO

Сопоставление целого литерала объекта с POCO (простой класс .NET со свойствами) полезен для статистической обработки связанных свойств.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Чтобы связать предыдущие секреты с POCO, используйте функцию [привязки графа объектов](xref:fundamentals/configuration/index#bind-to-an-object-graph) `Configuration` API. Следующий код привязывается к пользовательскому `MovieSettings` POCO и обращается к значению свойства `ServiceApiKey`:

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

`Movies:ConnectionString` и `Movies:ServiceApiKey` секреты сопоставлены с соответствующими свойствами в `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Замена строк секретами

Хранение паролей в виде обычного текста является небезопасным. Например, строка подключения к базе данных, хранящаяся в *appSettings. JSON* , может включать пароль для указанного пользователя:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Более безопасный подход заключается в хранении пароля в качестве секрета. Пример:

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

Удалите `Password` пары "ключ-значение" из строки подключения в *appSettings. JSON*. Пример:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Значение секрета можно задать для свойства <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> объекта <xref:System.Data.SqlClient.SqlConnectionStringBuilder>, чтобы завершить строку подключения:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Вывод списка секретов

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Выполните следующую команду из каталога, в котором существует файл *. csproj* :

```dotnetcli
dotnet user-secrets list
```

Появится следующий результат:

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

В предыдущем примере двоеточие в именах ключей обозначает иерархию объектов в *тайне. JSON*.

## <a name="remove-a-single-secret"></a>Удаление одного секрета

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Выполните следующую команду из каталога, в котором существует файл *. csproj* :

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

Файл *секреты. JSON* приложения был изменен для удаления пары "ключ-значение", связанной с `MoviesConnectionString` ключом:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

При выполнении `dotnet user-secrets list` отображается следующее сообщение:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Удалить все секреты

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Выполните следующую команду из каталога, в котором существует файл *. csproj* :

```dotnetcli
dotnet user-secrets clear
```

Все секреты пользователя для приложения удалены из файла *секреты. JSON* :

```json
{}
```

При выполнении `dotnet user-secrets list` отображается следующее сообщение:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* Сведения о доступе к диспетчеру секретов из IIS см. в [этой статье](https://github.com/dotnet/AspNetCore.Docs/issues/16328) .
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
