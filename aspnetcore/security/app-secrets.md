---
title: Безопасного хранения секрета приложения при разработке в ASP.NET Core
author: rick-anderson
description: Узнайте, как для сохранения и получения конфиденциальных данных в качестве секрета приложения во время разработки приложения ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 88b4ee9a963543f8cc97cb66271628a14fe657de
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/18/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Безопасного хранения секрета приложения при разработке в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [рот Daniel](https://github.com/danroth27), и [Скотт Addie](https://github.com/scottaddie)

::: moniker range="<= aspnetcore-1.1"
[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

В этом документе описываются методы для хранения и получения конфиденциальных данных во время разработки приложения ASP.NET Core. Никогда не следует хранить пароли и другие конфиденциальные данные в исходном коде, и не следует использовать секреты производства в разработке или протестировать режим. Можно хранить и защищать Azure рабочего и тестового секреты с [поставщика хранилища ключей Azure конфигурации](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Переменные среды

Чтобы избежать хранения секретов приложения в коде или в локальных файлов конфигурации используются переменные среды. Переменные среды переопределяют значения конфигурации для всех источников ранее указанной конфигурации.

::: moniker range="<= aspnetcore-1.1"
Настройте считывания значений переменных среды, вызвав [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) в `Startup` конструктор:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Рассмотрим веб-приложение ASP.NET Core, в котором **отдельных учетных записей пользователей** включена безопасность. Строка подключения базы данных по умолчанию включен в проект *appsettings.json* файл с ключом `DefaultConnection`. Строка подключения по умолчанию — для LocalDB, которая запускается в пользовательском режиме и не требует пароля. Во время развертывания приложения `DefaultConnection` значение ключа может быть заменено значение переменной среды. Полная строка соединения с учетом учетные данные могут быть сохранены переменной среды.

> [!WARNING]
> Переменные среды обычно хранятся в обычный текстовый незашифрованным. В случае компрометации компьютера или процесс, переменные среды может осуществляться недоверенные стороны. Могут потребоваться дополнительные меры, чтобы предотвратить раскрытие секретные данные пользователей.

## <a name="secret-manager"></a>Диспетчер секрет

Секрет диспетчера сохраняет конфиденциальные данные во время разработки проекта ASP.NET Core. В этом контексте конфиденциальных данных представляет секрет приложения. Секретные данные приложения хранятся в отдельном расположении в дереве проекта. Секреты приложения связанные с конкретным проектом или совместно использоваться в нескольких проектах. Секреты приложения не возвращен в систему управления версиями.

> [!WARNING]
> Секрет диспетчера не шифрования секретов, хранимых и не должен рассматриваться как доверенного хранилища. Это только в целях разработки. Ключи и значения хранятся в файле конфигурации JSON в папке профиля пользователя.

## <a name="how-the-secret-manager-tool-works"></a>Как работает диспетчер секрет

Секрет диспетчера абстрагирует детали реализации, например, где и как значения сохраняются. Можно использовать средство, не зная эти детали реализации. Значения хранятся в [JSON](https://json.org/) файл конфигурации в папке профиля пользователя, защищенные системы на локальном компьютере:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Путь файловой системы:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Путь файловой системы:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Путь файловой системы:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

В предыдущем пути к файлам, замените `<user_secrets_id>` с `UserSecretsId` значение, указанное в *.csproj* файла.

Не создавать код, зависящий от расположения или формат данных, сохраненных с помощью средства диспетчера секрет. Эти сведения реализации могут быть изменены. Например значения секрета не зашифрованы, но может быть в будущем.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Установите средство диспетчера секрет

Секрет диспетчера входит в состав .NET Core CLI в .NET Core SDK 2.1. Для .NET Core SDK 2.0 и более ранних версиях установки средства не требуется.

Установка [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) пакета NuGet в проекте ASP.NET Core:

[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Выполните следующую команду в командной оболочке для проверки установки средства:

```console
dotnet user-secrets -h
```

Средство диспетчера секрет отображает пример использования, параметры и справки:

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
> Должен быть в том же каталоге, что *.csproj* файла для запуска средства, предназначенные для *.csproj* файла `DotNetCliToolReference` элементов.
::: moniker-end

## <a name="set-a-secret"></a>Значение секрета

Секрет диспетчера обрабатывает параметры конфигурации для конкретного проекта, хранимых в профиле пользователя. Чтобы использовать секретные данные пользователей, определить `UserSecretsId` элемент в пределах `PropertyGroup` из *.csproj* файла. Значение `UserSecretsId` может быть произвольным, но является уникальным для проекта. Разработчики обычно создать GUID для `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> В Visual Studio, щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление секретами пользователя** в контекстном меню. Добавляет этот жест `UserSecretsId` элемент, заполненный идентификатора GUID, с *.csproj* файла. Visual Studio открывает *secrets.json* файл в текстовом редакторе. Замените содержимое *secrets.json* с пары «ключ значение» для сохранения. Пример: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

Определите секрет приложения, состоящая из ключа и его значение. Секрет, связанного с данным проектом `UserSecretsId` значение. Например, выполните следующую команду из каталога, в котором *.csproj* файл существует:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

В приведенном выше примере двоеточия обозначает, `Movies` — это объект литерала с `ServiceApiKey` свойство.

Секрет диспетчера можно использовать слишком из других каталогов. Используйте `--project` параметр, чтобы указать путь в файловой системе, с которой *.csproj* файл существует. Пример:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Задать несколько секретов

Пакет секретные данные можно задать по конвейеру JSON, `set` команды. В следующем примере *input.json* содержимое файла передаются по конвейеру в `set` команды.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Откройте командную строку и выполните следующую команду:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Откройте командную строку и выполните следующую команду:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Откройте командную строку и выполните следующую команду:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Доступ к секрета

[Конфигурации API ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к Manager секрет секретные данные. Если для различных версий .NET Core 1.x или .NET Framework, установите [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.

::: moniker range="<= aspnetcore-1.1"
Добавить источник конфигурации секретные данные пользователя для `Startup` конструктор:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Секреты пользователя можно получить через `Configuration` API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Строка замены с секретными данными

Рискованно хранить пароли в виде обычного текста. Например, строка подключения базы данных хранятся в *appsettings.json* может быть указан пароль для указанного пользователя:

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

Более безопасный подход — хранить пароль в качестве секрета. Пример:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Замените пароль в *appsettings.json* заполнителем. В следующем примере `{0}` используется в качестве заполнителя форму [Строка составного формата](/dotnet/standard/base-types/composite-formatting#composite-format-string).

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

Значение секрета могут быть добавлены в заполнитель, чтобы завершить строку подключения:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
::: moniker-end

## <a name="list-the-secrets"></a>Вывод списка секретов

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Выполните следующую команду из каталога, в котором *.csproj* файл существует:

```console
dotnet user-secrets list
```

Появляется следующий результат:

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

В приведенном выше примере двоеточия в имена ключей обозначает иерархии объектов в пределах *secrets.json*.

## <a name="remove-a-single-secret"></a>Удалите один секрет

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

Все секреты пользователя для приложения были удалены из *secrets.json* файла:

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