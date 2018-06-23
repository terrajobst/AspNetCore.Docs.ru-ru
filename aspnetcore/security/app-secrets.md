---
title: Безопасного хранения секрета приложения при разработке в ASP.NET Core
author: rick-anderson
description: Узнайте, как для сохранения и получения конфиденциальных данных в качестве секрета приложения во время разработки приложения ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2018
uid: security/app-secrets
ms.openlocfilehash: d3b2de1a17012986ef8dea7aaf8636dd35d10fa1
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314179"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="10300-103">Безопасного хранения секрета приложения при разработке в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10300-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="10300-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [рот Daniel](https://github.com/danroth27), и [Скотт Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="10300-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="10300-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="10300-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="10300-106">В этом документе описываются методы для хранения и получения конфиденциальных данных во время разработки приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10300-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="10300-107">Никогда не следует хранить пароли и другие конфиденциальные данные в исходном коде, и не следует использовать секреты производства в разработке или протестировать режим.</span><span class="sxs-lookup"><span data-stu-id="10300-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="10300-108">Можно хранить и защищать Azure рабочего и тестового секреты с [поставщика хранилища ключей Azure конфигурации](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="10300-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="10300-109">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="10300-109">Environment variables</span></span>

<span data-ttu-id="10300-110">Чтобы избежать хранения секретов приложения в коде или в локальных файлов конфигурации используются переменные среды.</span><span class="sxs-lookup"><span data-stu-id="10300-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="10300-111">Переменные среды переопределяют значения конфигурации для всех источников ранее указанной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="10300-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="10300-112">Настройте считывания значений переменных среды, вызвав [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="10300-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="10300-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="10300-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="10300-114">Рассмотрим веб-приложение ASP.NET Core, в котором **отдельных учетных записей пользователей** включена безопасность.</span><span class="sxs-lookup"><span data-stu-id="10300-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="10300-115">Строка подключения базы данных по умолчанию включен в проект *appsettings.json* файл с ключом `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="10300-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="10300-116">Строка подключения по умолчанию — для LocalDB, которая запускается в пользовательском режиме и не требует пароля.</span><span class="sxs-lookup"><span data-stu-id="10300-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="10300-117">Во время развертывания приложения `DefaultConnection` значение ключа может быть заменено значение переменной среды.</span><span class="sxs-lookup"><span data-stu-id="10300-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="10300-118">Полная строка соединения с учетом учетные данные могут быть сохранены переменной среды.</span><span class="sxs-lookup"><span data-stu-id="10300-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="10300-119">Переменные среды обычно хранятся в обычный текстовый незашифрованным.</span><span class="sxs-lookup"><span data-stu-id="10300-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="10300-120">В случае компрометации компьютера или процесс, переменные среды может осуществляться недоверенные стороны.</span><span class="sxs-lookup"><span data-stu-id="10300-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="10300-121">Могут потребоваться дополнительные меры, чтобы предотвратить раскрытие секретные данные пользователей.</span><span class="sxs-lookup"><span data-stu-id="10300-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="10300-122">Диспетчер секрет</span><span class="sxs-lookup"><span data-stu-id="10300-122">Secret Manager</span></span>

<span data-ttu-id="10300-123">Секрет диспетчера сохраняет конфиденциальные данные во время разработки проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10300-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="10300-124">В этом контексте конфиденциальных данных представляет секрет приложения.</span><span class="sxs-lookup"><span data-stu-id="10300-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="10300-125">Секретные данные приложения хранятся в отдельном расположении в дереве проекта.</span><span class="sxs-lookup"><span data-stu-id="10300-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="10300-126">Секреты приложения связанные с конкретным проектом или совместно использоваться в нескольких проектах.</span><span class="sxs-lookup"><span data-stu-id="10300-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="10300-127">Секреты приложения не возвращен в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="10300-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="10300-128">Секрет диспетчера не шифрования секретов, хранимых и не должен рассматриваться как доверенного хранилища.</span><span class="sxs-lookup"><span data-stu-id="10300-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="10300-129">Это только в целях разработки.</span><span class="sxs-lookup"><span data-stu-id="10300-129">It's for development purposes only.</span></span> <span data-ttu-id="10300-130">Ключи и значения хранятся в файле конфигурации JSON в папке профиля пользователя.</span><span class="sxs-lookup"><span data-stu-id="10300-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="10300-131">Как работает диспетчер секрет</span><span class="sxs-lookup"><span data-stu-id="10300-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="10300-132">Секрет диспетчера абстрагирует детали реализации, например, где и как значения сохраняются.</span><span class="sxs-lookup"><span data-stu-id="10300-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="10300-133">Можно использовать средство, не зная эти детали реализации.</span><span class="sxs-lookup"><span data-stu-id="10300-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="10300-134">Значения хранятся в файле конфигурации JSON в папке профиля пользователя, защищенные системы на локальном компьютере:</span><span class="sxs-lookup"><span data-stu-id="10300-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="10300-135">Windows</span><span class="sxs-lookup"><span data-stu-id="10300-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="10300-136">Путь файловой системы:</span><span class="sxs-lookup"><span data-stu-id="10300-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="10300-137">macOS</span><span class="sxs-lookup"><span data-stu-id="10300-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="10300-138">Путь файловой системы:</span><span class="sxs-lookup"><span data-stu-id="10300-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="10300-139">Linux</span><span class="sxs-lookup"><span data-stu-id="10300-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="10300-140">Путь файловой системы:</span><span class="sxs-lookup"><span data-stu-id="10300-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="10300-141">В предыдущем пути к файлам, замените `<user_secrets_id>` с `UserSecretsId` значение, указанное в *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="10300-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="10300-142">Не создавать код, зависящий от расположения или формат данных, сохраненных с помощью средства диспетчера секрет.</span><span class="sxs-lookup"><span data-stu-id="10300-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="10300-143">Эти сведения реализации могут быть изменены.</span><span class="sxs-lookup"><span data-stu-id="10300-143">These implementation details may change.</span></span> <span data-ttu-id="10300-144">Например значения секрета не зашифрованы, но может быть в будущем.</span><span class="sxs-lookup"><span data-stu-id="10300-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="10300-145">Установите средство диспетчера секрет</span><span class="sxs-lookup"><span data-stu-id="10300-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="10300-146">Секрет диспетчера входит в состав .NET Core CLI начиная с пакета SDK для .NET Core 2.1.300.</span><span class="sxs-lookup"><span data-stu-id="10300-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="10300-147">Для версии пакета SDK .NET Core перед 2.1.300 установки средства не нужны.</span><span class="sxs-lookup"><span data-stu-id="10300-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="10300-148">Запустите `dotnet --version` из командной оболочки, чтобы увидеть установленный номер версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="10300-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="10300-149">Если используется .NET Core SDK включает средство, выводится предупреждение:</span><span class="sxs-lookup"><span data-stu-id="10300-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="10300-150">Установка [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) пакета NuGet в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10300-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="10300-151">Пример:</span><span class="sxs-lookup"><span data-stu-id="10300-151">For example:</span></span>

<span data-ttu-id="10300-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="10300-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="10300-153">Выполните следующую команду в командной оболочке для проверки установки средства:</span><span class="sxs-lookup"><span data-stu-id="10300-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="10300-154">Средство диспетчера секрет отображает пример использования, параметры и справки:</span><span class="sxs-lookup"><span data-stu-id="10300-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="10300-155">Должен быть в том же каталоге, что *.csproj* файла для запуска средства, предназначенные для *.csproj* файла `DotNetCliToolReference` элементов.</span><span class="sxs-lookup"><span data-stu-id="10300-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="10300-156">Значение секрета</span><span class="sxs-lookup"><span data-stu-id="10300-156">Set a secret</span></span>

<span data-ttu-id="10300-157">Секрет диспетчера обрабатывает параметры конфигурации для конкретного проекта, хранимых в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="10300-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="10300-158">Чтобы использовать секретные данные пользователей, определить `UserSecretsId` элемент в пределах `PropertyGroup` из *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="10300-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="10300-159">Значение `UserSecretsId` может быть произвольным, но является уникальным для проекта.</span><span class="sxs-lookup"><span data-stu-id="10300-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="10300-160">Разработчики обычно создать GUID для `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="10300-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="10300-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="10300-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="10300-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="10300-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="10300-163">В Visual Studio, щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление секретами пользователя** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="10300-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="10300-164">Добавляет этот жест `UserSecretsId` элемент, заполненный идентификатора GUID, с *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="10300-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="10300-165">Visual Studio открывает *secrets.json* файл в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="10300-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="10300-166">Замените содержимое *secrets.json* с пары «ключ значение» для сохранения.</span><span class="sxs-lookup"><span data-stu-id="10300-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="10300-167">Пример: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="10300-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="10300-168">Определите секрет приложения, состоящая из ключа и его значение.</span><span class="sxs-lookup"><span data-stu-id="10300-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="10300-169">Секрет, связанного с данным проектом `UserSecretsId` значение.</span><span class="sxs-lookup"><span data-stu-id="10300-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="10300-170">Например, выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="10300-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="10300-171">В приведенном выше примере двоеточия обозначает, `Movies` — это объект литерала с `ServiceApiKey` свойство.</span><span class="sxs-lookup"><span data-stu-id="10300-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="10300-172">Секрет диспетчера можно использовать слишком из других каталогов.</span><span class="sxs-lookup"><span data-stu-id="10300-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="10300-173">Используйте `--project` параметр, чтобы указать путь в файловой системе, с которой *.csproj* файл существует.</span><span class="sxs-lookup"><span data-stu-id="10300-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="10300-174">Пример:</span><span class="sxs-lookup"><span data-stu-id="10300-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="10300-175">Задать несколько секретов</span><span class="sxs-lookup"><span data-stu-id="10300-175">Set multiple secrets</span></span>

<span data-ttu-id="10300-176">Пакет секретные данные можно задать по конвейеру JSON, `set` команды.</span><span class="sxs-lookup"><span data-stu-id="10300-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="10300-177">В следующем примере *input.json* содержимое файла передаются по конвейеру в `set` команды.</span><span class="sxs-lookup"><span data-stu-id="10300-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="10300-178">Windows</span><span class="sxs-lookup"><span data-stu-id="10300-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="10300-179">Откройте командную строку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="10300-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="10300-180">macOS</span><span class="sxs-lookup"><span data-stu-id="10300-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="10300-181">Откройте командную строку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="10300-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="10300-182">Linux</span><span class="sxs-lookup"><span data-stu-id="10300-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="10300-183">Откройте командную строку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="10300-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="10300-184">Доступ к секрета</span><span class="sxs-lookup"><span data-stu-id="10300-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="10300-185">[Конфигурации API ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к Manager секрет секретные данные.</span><span class="sxs-lookup"><span data-stu-id="10300-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="10300-186">Установка [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="10300-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="10300-187">Добавить источник конфигурации секретные данные пользователей с помощью вызова [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="10300-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="10300-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="10300-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="10300-189">[Конфигурации API ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к Manager секрет секретные данные.</span><span class="sxs-lookup"><span data-stu-id="10300-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="10300-190">Если проект предназначен для .NET Framework, установите [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="10300-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="10300-191">Core ASP.NET 2.0 или более поздней версии, источник конфигурации секретов пользователя автоматически добавляется в режиме разработки при вызове проекта [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) для инициализации нового экземпляра узла с заранее настроенные значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="10300-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="10300-192">`CreateDefaultBuilder` вызовы [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) при [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) — [разработки](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="10300-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="10300-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="10300-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="10300-194">Когда `CreateDefaultBuilder` не вызывается во время создания узлов, добавить источник конфигурации секретные данные пользователей с помощью вызова [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="10300-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="10300-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="10300-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="10300-196">Секреты пользователя можно получить через `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="10300-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="10300-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="10300-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="10300-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="10300-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="10300-199">Строка замены с секретными данными</span><span class="sxs-lookup"><span data-stu-id="10300-199">String replacement with secrets</span></span>

<span data-ttu-id="10300-200">Хранить пароли в виде обычного текста небезопасно.</span><span class="sxs-lookup"><span data-stu-id="10300-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="10300-201">Например, строка подключения базы данных хранятся в *appsettings.json* может быть указан пароль для указанного пользователя:</span><span class="sxs-lookup"><span data-stu-id="10300-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="10300-202">Более безопасный подход — хранить пароль в качестве секрета.</span><span class="sxs-lookup"><span data-stu-id="10300-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="10300-203">Пример:</span><span class="sxs-lookup"><span data-stu-id="10300-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="10300-204">Удалить `Password` пары ключ значение из строки подключения в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="10300-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="10300-205">Пример:</span><span class="sxs-lookup"><span data-stu-id="10300-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="10300-206">Значение секрета можно установить на [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) объекта [пароль](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) свойства для выполнения строки подключения:</span><span class="sxs-lookup"><span data-stu-id="10300-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="10300-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="10300-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="10300-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span><span class="sxs-lookup"><span data-stu-id="10300-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="10300-209">Вывод списка секретов</span><span class="sxs-lookup"><span data-stu-id="10300-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="10300-210">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="10300-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="10300-211">Появляется следующий результат:</span><span class="sxs-lookup"><span data-stu-id="10300-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="10300-212">В приведенном выше примере двоеточия в имена ключей обозначает иерархии объектов в пределах *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="10300-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="10300-213">Удалите один секрет</span><span class="sxs-lookup"><span data-stu-id="10300-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="10300-214">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="10300-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="10300-215">Приложения *secrets.json* файл был изменен, чтобы удалить пару ключ значение, связанных с `MoviesConnectionString` ключ:</span><span class="sxs-lookup"><span data-stu-id="10300-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="10300-216">Под управлением `dotnet user-secrets list` отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="10300-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="10300-217">Удалить все секреты</span><span class="sxs-lookup"><span data-stu-id="10300-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="10300-218">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="10300-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="10300-219">Все секреты пользователя для приложения были удалены из *secrets.json* файла:</span><span class="sxs-lookup"><span data-stu-id="10300-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="10300-220">Под управлением `dotnet user-secrets list` отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="10300-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="10300-221">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="10300-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
