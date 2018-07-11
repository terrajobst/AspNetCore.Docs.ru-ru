---
title: Безопасное хранение секретов приложения во время разработки в ASP.NET Core
author: rick-anderson
description: Узнайте, как сохранять и извлекать конфиденциальную информацию в виде секретов приложения во время разработки приложения ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2018
uid: security/app-secrets
ms.openlocfilehash: d3b2de1a17012986ef8dea7aaf8636dd35d10fa1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126915"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="f55e4-103">Безопасное хранение секретов приложения во время разработки в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f55e4-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="f55e4-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Дэниэл рот](https://github.com/danroth27), и [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="f55e4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="f55e4-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f55e4-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f55e4-106">В этом документе описываются методы для хранения и извлечения конфиденциальных данных во время разработки приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f55e4-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="f55e4-107">Никогда не следует хранить пароли и другие конфиденциальные данные в исходном коде и не используйте секреты производства в разработки или тестирования режим.</span><span class="sxs-lookup"><span data-stu-id="f55e4-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="f55e4-108">Для хранения и защиты секретов Azure в ходе тестирования и непосредственной работы используйте [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="f55e4-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="f55e4-109">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="f55e4-109">Environment variables</span></span>

<span data-ttu-id="f55e4-110">Переменные среды позволяют избежать хранение секретов приложений в коде или в локальных файлов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f55e4-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="f55e4-111">Переменные среды переопределить значения конфигурации для всех источников ранее указанной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f55e4-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f55e4-112">Настройки чтения значений переменных среды путем вызова [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="f55e4-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

<span data-ttu-id="f55e4-113">Рассмотрим веб-приложение ASP.NET Core, в котором **учетные записи отдельных пользователей** включена безопасность.</span><span class="sxs-lookup"><span data-stu-id="f55e4-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="f55e4-114">Строку подключения базы данных по умолчанию включается в проект *appsettings.json* файл с ключом `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="f55e4-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="f55e4-115">Строка подключения по умолчанию — для LocalDB, который работает в режиме пользователя и пароля не требуется.</span><span class="sxs-lookup"><span data-stu-id="f55e4-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="f55e4-116">Во время развертывания приложения `DefaultConnection` значение ключа может быть заменено значение переменной среды.</span><span class="sxs-lookup"><span data-stu-id="f55e4-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="f55e4-117">Полная строка подключения с конфиденциальные учетные данные могут хранить в переменной среды.</span><span class="sxs-lookup"><span data-stu-id="f55e4-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="f55e4-118">Переменные среды обычно хранятся в обычный и незашифрованное текста.</span><span class="sxs-lookup"><span data-stu-id="f55e4-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="f55e4-119">Если компьютер или процесс нарушена, переменные среды может осуществляться с недоверенным сторонам.</span><span class="sxs-lookup"><span data-stu-id="f55e4-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="f55e4-120">Могут потребоваться дополнительные меры, чтобы предотвратить раскрытие секретных данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="f55e4-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="f55e4-121">Менеджер секретов</span><span class="sxs-lookup"><span data-stu-id="f55e4-121">Secret Manager</span></span>

<span data-ttu-id="f55e4-122">Средство Secret Manager сохраняет конфиденциальные данные во время разработки проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f55e4-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="f55e4-123">В этом контексте конфиденциальных данных представляет секрет приложения.</span><span class="sxs-lookup"><span data-stu-id="f55e4-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="f55e4-124">Секреты приложения хранятся в отдельном расположении в дереве проекта.</span><span class="sxs-lookup"><span data-stu-id="f55e4-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="f55e4-125">Секреты приложения связанные с определенным проектом или совместно используется несколькими проектами.</span><span class="sxs-lookup"><span data-stu-id="f55e4-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="f55e4-126">Секреты приложения не проверяются в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="f55e4-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="f55e4-127">Средство Secret Manager не шифровать хранимые секреты и не должен рассматриваться в качестве доверенного хранилища.</span><span class="sxs-lookup"><span data-stu-id="f55e4-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="f55e4-128">Это только в целях разработки.</span><span class="sxs-lookup"><span data-stu-id="f55e4-128">It's for development purposes only.</span></span> <span data-ttu-id="f55e4-129">Ключи и значения хранятся в файле конфигурации JSON в каталоге профиля пользователя.</span><span class="sxs-lookup"><span data-stu-id="f55e4-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="f55e4-130">Как работает менеджером секретов</span><span class="sxs-lookup"><span data-stu-id="f55e4-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="f55e4-131">Средство Secret Manager абстрагирует детали реализации, например где и как значения сохраняются.</span><span class="sxs-lookup"><span data-stu-id="f55e4-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="f55e4-132">Можно использовать средство, не зная эти детали реализации.</span><span class="sxs-lookup"><span data-stu-id="f55e4-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="f55e4-133">Значения хранятся в файле конфигурации JSON в папке профиля пользователя, система защищена на локальном компьютере:</span><span class="sxs-lookup"><span data-stu-id="f55e4-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f55e4-134">Windows</span><span class="sxs-lookup"><span data-stu-id="f55e4-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="f55e4-135">Путь файловой системы:</span><span class="sxs-lookup"><span data-stu-id="f55e4-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="f55e4-136">macOS</span><span class="sxs-lookup"><span data-stu-id="f55e4-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="f55e4-137">Путь файловой системы:</span><span class="sxs-lookup"><span data-stu-id="f55e4-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="f55e4-138">Linux</span><span class="sxs-lookup"><span data-stu-id="f55e4-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="f55e4-139">Путь файловой системы:</span><span class="sxs-lookup"><span data-stu-id="f55e4-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="f55e4-140">В предыдущем пути к файлам, замените `<user_secrets_id>` с `UserSecretsId` значение, указанное в *.csproj* файл.</span><span class="sxs-lookup"><span data-stu-id="f55e4-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="f55e4-141">Не создавать код, зависящий от расположения или формата данных, сохраненных с помощью диспетчера секретов.</span><span class="sxs-lookup"><span data-stu-id="f55e4-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="f55e4-142">Эти детали реализации могут быть изменены.</span><span class="sxs-lookup"><span data-stu-id="f55e4-142">These implementation details may change.</span></span> <span data-ttu-id="f55e4-143">Например его значения не шифруются, но может быть в будущем.</span><span class="sxs-lookup"><span data-stu-id="f55e4-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="f55e4-144">Установите средство Secret Manager</span><span class="sxs-lookup"><span data-stu-id="f55e4-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="f55e4-145">Средство Secret Manager входит в состав .NET Core CLI .NET Core в пакете SDK для 2.1.300.</span><span class="sxs-lookup"><span data-stu-id="f55e4-145">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="f55e4-146">Для версий пакета SDK для .NET Core до 2.1.300 необходим установки средства.</span><span class="sxs-lookup"><span data-stu-id="f55e4-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="f55e4-147">Запустите `dotnet --version` из командной оболочки, чтобы увидеть установленный номер версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f55e4-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="f55e4-148">Если пакет SDK для .NET Core используется включает средство, выводится предупреждение:</span><span class="sxs-lookup"><span data-stu-id="f55e4-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="f55e4-149">Установка [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) пакет NuGet в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f55e4-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="f55e4-150">Пример:</span><span class="sxs-lookup"><span data-stu-id="f55e4-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

<span data-ttu-id="f55e4-151">Выполните следующую команду в командной строке для проверки установки средства:</span><span class="sxs-lookup"><span data-stu-id="f55e4-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="f55e4-152">Средство Secret Manager отображает пример использования, параметры и команды help:</span><span class="sxs-lookup"><span data-stu-id="f55e4-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="f55e4-153">Должен быть в том же каталоге, что *.csproj* файл для запуска средства, предназначенные для *.csproj* файла `DotNetCliToolReference` элементов.</span><span class="sxs-lookup"><span data-stu-id="f55e4-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="f55e4-154">Значение секрета</span><span class="sxs-lookup"><span data-stu-id="f55e4-154">Set a secret</span></span>

<span data-ttu-id="f55e4-155">Средство Secret Manager работает настройки конфигурации конкретного проекта, хранящиеся в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="f55e4-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="f55e4-156">Чтобы использовать секреты пользователя, определить `UserSecretsId` сервисном `PropertyGroup` из *.csproj* файл.</span><span class="sxs-lookup"><span data-stu-id="f55e4-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="f55e4-157">Значение `UserSecretsId` может быть произвольным, но является уникальным для проекта.</span><span class="sxs-lookup"><span data-stu-id="f55e4-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="f55e4-158">Разработчики обычно создать GUID для `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="f55e4-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> <span data-ttu-id="f55e4-159">В Visual Studio щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление секретами пользователей** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="f55e4-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="f55e4-160">Добавляет этот жест `UserSecretsId` элемент, заполненный идентификатора GUID, что в *.csproj* файл.</span><span class="sxs-lookup"><span data-stu-id="f55e4-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="f55e4-161">Visual Studio открывает *secrets.json* файл в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="f55e4-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="f55e4-162">Замените содержимое файла *secrets.json* с парами "ключ значение" для сохранения.</span><span class="sxs-lookup"><span data-stu-id="f55e4-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="f55e4-163">Пример: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="f55e4-163">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="f55e4-164">Определите секрет приложения, состоящие из ключа и его значение.</span><span class="sxs-lookup"><span data-stu-id="f55e4-164">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="f55e4-165">Секрет связан с проектом `UserSecretsId` значение.</span><span class="sxs-lookup"><span data-stu-id="f55e4-165">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="f55e4-166">Например, выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="f55e4-166">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="f55e4-167">В приведенном выше примере двоеточия обозначает, что `Movies` — это объект литерал с `ServiceApiKey` свойство.</span><span class="sxs-lookup"><span data-stu-id="f55e4-167">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="f55e4-168">Средство Secret Manager можно использовать из других каталогов.</span><span class="sxs-lookup"><span data-stu-id="f55e4-168">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="f55e4-169">Используйте `--project` параметр, чтобы указать путь в файловой системе, по которому *.csproj* файл существует.</span><span class="sxs-lookup"><span data-stu-id="f55e4-169">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="f55e4-170">Пример:</span><span class="sxs-lookup"><span data-stu-id="f55e4-170">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="f55e4-171">Задайте несколько секретных кодов</span><span class="sxs-lookup"><span data-stu-id="f55e4-171">Set multiple secrets</span></span>

<span data-ttu-id="f55e4-172">Пакет секреты можно задать путем передачи JSON для `set` команды.</span><span class="sxs-lookup"><span data-stu-id="f55e4-172">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="f55e4-173">В следующем примере *input.json* содержимое файла передаются по конвейеру в `set` команды.</span><span class="sxs-lookup"><span data-stu-id="f55e4-173">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f55e4-174">Windows</span><span class="sxs-lookup"><span data-stu-id="f55e4-174">Windows</span></span>](#tab/windows)

<span data-ttu-id="f55e4-175">Откройте командную оболочку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f55e4-175">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="f55e4-176">macOS</span><span class="sxs-lookup"><span data-stu-id="f55e4-176">macOS</span></span>](#tab/macos)

<span data-ttu-id="f55e4-177">Откройте командную оболочку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f55e4-177">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="f55e4-178">Linux</span><span class="sxs-lookup"><span data-stu-id="f55e4-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="f55e4-179">Откройте командную оболочку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f55e4-179">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="f55e4-180">Доступ к секрета</span><span class="sxs-lookup"><span data-stu-id="f55e4-180">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f55e4-181">[API конфигурации ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к Secret Manager секреты.</span><span class="sxs-lookup"><span data-stu-id="f55e4-181">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="f55e4-182">Установка [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="f55e4-182">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="f55e4-183">Добавить источник конфигурации секреты пользователя с помощью вызова [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="f55e4-183">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f55e4-184">[API конфигурации ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к Secret Manager секреты.</span><span class="sxs-lookup"><span data-stu-id="f55e4-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="f55e4-185">Если проект предназначен для платформы .NET Framework, установить [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="f55e4-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="f55e4-186">В ASP.NET Core 2.0 или более поздней версии, источник конфигурации секреты пользователя автоматически добавляется в режиме разработки, пока проект не вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) для инициализации нового экземпляра узла с помощью предварительно настроенных значений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f55e4-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="f55e4-187">`CreateDefaultBuilder` вызовы [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) при [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) — [разработки](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="f55e4-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="f55e4-188">Когда `CreateDefaultBuilder` не вызывается во время создания узла, добавьте источник конфигурации секреты пользователя с помощью вызова [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="f55e4-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

<span data-ttu-id="f55e4-189">Секреты пользователя можно получить через `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="f55e4-189">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="f55e4-190">Строка замены с секретами</span><span class="sxs-lookup"><span data-stu-id="f55e4-190">String replacement with secrets</span></span>

<span data-ttu-id="f55e4-191">Хранить пароли в виде обычного текста небезопасно.</span><span class="sxs-lookup"><span data-stu-id="f55e4-191">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="f55e4-192">Например, строку подключения базы данных хранятся в *appsettings.json* может быть указан пароль для указанного пользователя:</span><span class="sxs-lookup"><span data-stu-id="f55e4-192">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="f55e4-193">Более безопасный подход — в том, чтобы сохранить пароль в качестве секрета.</span><span class="sxs-lookup"><span data-stu-id="f55e4-193">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="f55e4-194">Пример:</span><span class="sxs-lookup"><span data-stu-id="f55e4-194">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="f55e4-195">Удалить `Password` пару ключ значение из строки подключения в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f55e4-195">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="f55e4-196">Пример:</span><span class="sxs-lookup"><span data-stu-id="f55e4-196">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="f55e4-197">Можно задать значение секрета [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) объекта [пароль](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) свойство, чтобы завершить строку подключения:</span><span class="sxs-lookup"><span data-stu-id="f55e4-197">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="f55e4-198">Вывод списка секретов</span><span class="sxs-lookup"><span data-stu-id="f55e4-198">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f55e4-199">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="f55e4-199">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="f55e4-200">Появляется следующий результат:</span><span class="sxs-lookup"><span data-stu-id="f55e4-200">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="f55e4-201">В приведенном выше примере двоеточия в имена ключей означает иерархии объектов в пределах *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="f55e4-201">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="f55e4-202">Удалить секрет единого</span><span class="sxs-lookup"><span data-stu-id="f55e4-202">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f55e4-203">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="f55e4-203">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="f55e4-204">Приложения *secrets.json* файл был изменен, чтобы удалить пару ключ значение, связанных с `MoviesConnectionString` ключ:</span><span class="sxs-lookup"><span data-stu-id="f55e4-204">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="f55e4-205">Под управлением `dotnet user-secrets list` отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="f55e4-205">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="f55e4-206">Удалить все секреты</span><span class="sxs-lookup"><span data-stu-id="f55e4-206">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f55e4-207">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="f55e4-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="f55e4-208">Все секреты пользователя приложения будут удалены из *secrets.json* файла:</span><span class="sxs-lookup"><span data-stu-id="f55e4-208">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="f55e4-209">Под управлением `dotnet user-secrets list` отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="f55e4-209">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="f55e4-210">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f55e4-210">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
