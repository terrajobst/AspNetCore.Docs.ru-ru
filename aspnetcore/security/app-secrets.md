---
title: Безопасное хранение секретов приложения во время разработки в ASP.NET Core
author: rick-anderson
description: Узнайте, как сохранять и извлекать конфиденциальную информацию в виде секретов приложения во время разработки приложения ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/24/2018
uid: security/app-secrets
ms.openlocfilehash: 1ecd9b87b9e4aa2c511349fe407c0cea9dc9f921
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028275"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="6126e-103">Безопасное хранение секретов приложения во время разработки в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6126e-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="6126e-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Дэниэл рот](https://github.com/danroth27), и [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="6126e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="6126e-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6126e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6126e-106">В этом документе описываются методы для хранения и извлечения конфиденциальных данных во время разработки приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6126e-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="6126e-107">Никогда не храните пароли и другие конфиденциальные данные в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="6126e-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="6126e-108">Не следует использовать секреты рабочей среды для разработки или тестирования.</span><span class="sxs-lookup"><span data-stu-id="6126e-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="6126e-109">Для хранения и защиты секретов Azure в ходе тестирования и непосредственной работы используйте [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="6126e-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="6126e-110">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="6126e-110">Environment variables</span></span>

<span data-ttu-id="6126e-111">Переменные среды позволяют избежать хранение секретов приложений в коде или в локальных файлов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6126e-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="6126e-112">Переменные среды переопределить значения конфигурации для всех источников ранее указанной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6126e-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="6126e-113">Настройки чтения значений переменных среды путем вызова [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="6126e-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="6126e-114">Рассмотрим веб-приложение ASP.NET Core, в котором **учетные записи отдельных пользователей** включена безопасность.</span><span class="sxs-lookup"><span data-stu-id="6126e-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="6126e-115">Строку подключения базы данных по умолчанию включается в проект *appsettings.json* файл с ключом `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="6126e-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="6126e-116">Строка подключения по умолчанию — для LocalDB, который работает в режиме пользователя и пароля не требуется.</span><span class="sxs-lookup"><span data-stu-id="6126e-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="6126e-117">Во время развертывания приложения `DefaultConnection` значение ключа может быть заменено значение переменной среды.</span><span class="sxs-lookup"><span data-stu-id="6126e-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="6126e-118">Полная строка подключения с конфиденциальные учетные данные могут хранить в переменной среды.</span><span class="sxs-lookup"><span data-stu-id="6126e-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="6126e-119">Переменные среды обычно хранятся в обычный и незашифрованное текста.</span><span class="sxs-lookup"><span data-stu-id="6126e-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="6126e-120">Если компьютер или процесс нарушена, переменные среды может осуществляться с недоверенным сторонам.</span><span class="sxs-lookup"><span data-stu-id="6126e-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="6126e-121">Могут потребоваться дополнительные меры, чтобы предотвратить раскрытие секретных данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="6126e-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="6126e-122">Менеджер секретов</span><span class="sxs-lookup"><span data-stu-id="6126e-122">Secret Manager</span></span>

<span data-ttu-id="6126e-123">Средство Secret Manager сохраняет конфиденциальные данные во время разработки проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6126e-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="6126e-124">В этом контексте конфиденциальных данных представляет секрет приложения.</span><span class="sxs-lookup"><span data-stu-id="6126e-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="6126e-125">Секреты приложения хранятся в отдельном расположении в дереве проекта.</span><span class="sxs-lookup"><span data-stu-id="6126e-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="6126e-126">Секреты приложения связанные с определенным проектом или совместно используется несколькими проектами.</span><span class="sxs-lookup"><span data-stu-id="6126e-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="6126e-127">Секреты приложения не проверяются в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="6126e-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="6126e-128">Средство Secret Manager не шифровать хранимые секреты и не должен рассматриваться в качестве доверенного хранилища.</span><span class="sxs-lookup"><span data-stu-id="6126e-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="6126e-129">Это только в целях разработки.</span><span class="sxs-lookup"><span data-stu-id="6126e-129">It's for development purposes only.</span></span> <span data-ttu-id="6126e-130">Ключи и значения хранятся в файле конфигурации JSON в каталоге профиля пользователя.</span><span class="sxs-lookup"><span data-stu-id="6126e-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="6126e-131">Как работает менеджером секретов</span><span class="sxs-lookup"><span data-stu-id="6126e-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="6126e-132">Средство Secret Manager абстрагирует детали реализации, например где и как значения сохраняются.</span><span class="sxs-lookup"><span data-stu-id="6126e-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="6126e-133">Можно использовать средство, не зная эти детали реализации.</span><span class="sxs-lookup"><span data-stu-id="6126e-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="6126e-134">Значения хранятся в файле конфигурации JSON в папке профиля пользователя, система защищена на локальном компьютере:</span><span class="sxs-lookup"><span data-stu-id="6126e-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="6126e-135">Windows</span><span class="sxs-lookup"><span data-stu-id="6126e-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="6126e-136">Путь файловой системы:</span><span class="sxs-lookup"><span data-stu-id="6126e-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="6126e-137">macOS</span><span class="sxs-lookup"><span data-stu-id="6126e-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="6126e-138">Путь файловой системы:</span><span class="sxs-lookup"><span data-stu-id="6126e-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="6126e-139">Linux</span><span class="sxs-lookup"><span data-stu-id="6126e-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="6126e-140">Путь файловой системы:</span><span class="sxs-lookup"><span data-stu-id="6126e-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="6126e-141">В предыдущем пути к файлам, замените `<user_secrets_id>` с `UserSecretsId` значение, указанное в *.csproj* файл.</span><span class="sxs-lookup"><span data-stu-id="6126e-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="6126e-142">Не создавать код, зависящий от расположения или формата данных, сохраненных с помощью диспетчера секретов.</span><span class="sxs-lookup"><span data-stu-id="6126e-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="6126e-143">Эти детали реализации могут быть изменены.</span><span class="sxs-lookup"><span data-stu-id="6126e-143">These implementation details may change.</span></span> <span data-ttu-id="6126e-144">Например его значения не шифруются, но может быть в будущем.</span><span class="sxs-lookup"><span data-stu-id="6126e-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="6126e-145">Установите средство Secret Manager</span><span class="sxs-lookup"><span data-stu-id="6126e-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="6126e-146">Средство Secret Manager связке с .NET Core CLI в .NET Core SDK 2.1.300 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="6126e-146">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="6126e-147">Для версий пакета SDK для .NET Core до 2.1.300 необходим установки средства.</span><span class="sxs-lookup"><span data-stu-id="6126e-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="6126e-148">Запустите `dotnet --version` из командной оболочки, чтобы увидеть установленный номер версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6126e-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="6126e-149">Если пакет SDK для .NET Core используется включает средство, выводится предупреждение:</span><span class="sxs-lookup"><span data-stu-id="6126e-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="6126e-150">Установка [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) пакет NuGet в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6126e-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="6126e-151">Пример:</span><span class="sxs-lookup"><span data-stu-id="6126e-151">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="6126e-152">Выполните следующую команду в командной строке для проверки установки средства:</span><span class="sxs-lookup"><span data-stu-id="6126e-152">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="6126e-153">Средство Secret Manager отображает пример использования, параметры и команды help:</span><span class="sxs-lookup"><span data-stu-id="6126e-153">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="6126e-154">Должен быть в том же каталоге, что *.csproj* файл для запуска средства, предназначенные для *.csproj* файла `DotNetCliToolReference` элементов.</span><span class="sxs-lookup"><span data-stu-id="6126e-154">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="6126e-155">Значение секрета</span><span class="sxs-lookup"><span data-stu-id="6126e-155">Set a secret</span></span>

<span data-ttu-id="6126e-156">Средство Secret Manager работает настройки конфигурации конкретного проекта, хранящиеся в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="6126e-156">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="6126e-157">Чтобы использовать секреты пользователя, определить `UserSecretsId` сервисном `PropertyGroup` из *.csproj* файл.</span><span class="sxs-lookup"><span data-stu-id="6126e-157">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="6126e-158">Значение `UserSecretsId` может быть произвольным, но является уникальным для проекта.</span><span class="sxs-lookup"><span data-stu-id="6126e-158">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="6126e-159">Разработчики обычно создать GUID для `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="6126e-159">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="6126e-160">В Visual Studio щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление секретами пользователей** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="6126e-160">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="6126e-161">Добавляет этот жест `UserSecretsId` элемент, заполненный идентификатора GUID, что в *.csproj* файл.</span><span class="sxs-lookup"><span data-stu-id="6126e-161">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="6126e-162">Visual Studio открывает *secrets.json* файл в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="6126e-162">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="6126e-163">Замените содержимое файла *secrets.json* с парами "ключ значение" для сохранения.</span><span class="sxs-lookup"><span data-stu-id="6126e-163">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="6126e-164">Пример:</span><span class="sxs-lookup"><span data-stu-id="6126e-164">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="6126e-165">Структура JSON выравнивается после внесения изменений с помощью `dotnet user-secrets remove` или `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="6126e-165">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="6126e-166">Например, на котором работают `dotnet user-secrets remove "Movies:ConnectionString"` сворачивает `Movies` литерала объекта.</span><span class="sxs-lookup"><span data-stu-id="6126e-166">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="6126e-167">Измененный файл будет выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="6126e-167">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="6126e-168">Определите секрет приложения, состоящие из ключа и его значение.</span><span class="sxs-lookup"><span data-stu-id="6126e-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="6126e-169">Секрет связан с проектом `UserSecretsId` значение.</span><span class="sxs-lookup"><span data-stu-id="6126e-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="6126e-170">Например, выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="6126e-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="6126e-171">В приведенном выше примере двоеточия обозначает, что `Movies` — это объект литерал с `ServiceApiKey` свойство.</span><span class="sxs-lookup"><span data-stu-id="6126e-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="6126e-172">Средство Secret Manager можно использовать из других каталогов.</span><span class="sxs-lookup"><span data-stu-id="6126e-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="6126e-173">Используйте `--project` параметр, чтобы указать путь в файловой системе, по которому *.csproj* файл существует.</span><span class="sxs-lookup"><span data-stu-id="6126e-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="6126e-174">Пример:</span><span class="sxs-lookup"><span data-stu-id="6126e-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="6126e-175">Задайте несколько секретных кодов</span><span class="sxs-lookup"><span data-stu-id="6126e-175">Set multiple secrets</span></span>

<span data-ttu-id="6126e-176">Пакет секреты можно задать путем передачи JSON для `set` команды.</span><span class="sxs-lookup"><span data-stu-id="6126e-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="6126e-177">В следующем примере *input.json* содержимое файла передаются по конвейеру в `set` команды.</span><span class="sxs-lookup"><span data-stu-id="6126e-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="6126e-178">Windows</span><span class="sxs-lookup"><span data-stu-id="6126e-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="6126e-179">Откройте командную оболочку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6126e-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="6126e-180">macOS</span><span class="sxs-lookup"><span data-stu-id="6126e-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="6126e-181">Откройте командную оболочку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6126e-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="6126e-182">Linux</span><span class="sxs-lookup"><span data-stu-id="6126e-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="6126e-183">Откройте командную оболочку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6126e-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="6126e-184">Доступ к секрета</span><span class="sxs-lookup"><span data-stu-id="6126e-184">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6126e-185">[API конфигурации ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к Secret Manager секреты.</span><span class="sxs-lookup"><span data-stu-id="6126e-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="6126e-186">Если проект предназначен для платформы .NET Framework, установить [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="6126e-186">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="6126e-187">В ASP.NET Core 2.0 или более поздней версии, источник конфигурации секреты пользователя автоматически добавляется в режиме разработки, пока проект не вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) для инициализации нового экземпляра узла с помощью предварительно настроенных значений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6126e-187">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="6126e-188">`CreateDefaultBuilder` вызовы [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) при [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) — [разработки](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="6126e-188">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="6126e-189">Когда `CreateDefaultBuilder` не вызывается во время создания узла, добавьте источник конфигурации секреты пользователя с помощью вызова [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="6126e-189">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="6126e-190">[API конфигурации ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к Secret Manager секреты.</span><span class="sxs-lookup"><span data-stu-id="6126e-190">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="6126e-191">Установка [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="6126e-191">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="6126e-192">Добавить источник конфигурации секреты пользователя с помощью вызова [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="6126e-192">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="6126e-193">Секреты пользователя можно получить через `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="6126e-193">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="6126e-194">Сопоставить секреты POCO</span><span class="sxs-lookup"><span data-stu-id="6126e-194">Map secrets to a POCO</span></span>

<span data-ttu-id="6126e-195">Сопоставление всей литерала объекта POCO (простой класс .NET со свойствами) полезен для объединения связанных свойств.</span><span class="sxs-lookup"><span data-stu-id="6126e-195">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6126e-196">Чтобы сопоставить выше секреты POCO, используйте `Configuration` API [привязке графа объектов](xref:fundamentals/configuration/index#bind-to-an-object-graph) функции.</span><span class="sxs-lookup"><span data-stu-id="6126e-196">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="6126e-197">Следующий код привязывается к пользовательской `MovieSettings` POCO и обращается к `ServiceApiKey` значение свойства:</span><span class="sxs-lookup"><span data-stu-id="6126e-197">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="6126e-198">`Movies:ConnectionString` И `Movies:ServiceApiKey` секреты, сопоставляются с соответствующих свойств в `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="6126e-198">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="6126e-199">Строка замены с секретами</span><span class="sxs-lookup"><span data-stu-id="6126e-199">String replacement with secrets</span></span>

<span data-ttu-id="6126e-200">Хранить пароли в виде обычного текста небезопасно.</span><span class="sxs-lookup"><span data-stu-id="6126e-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="6126e-201">Например, строку подключения базы данных хранятся в *appsettings.json* может быть указан пароль для указанного пользователя:</span><span class="sxs-lookup"><span data-stu-id="6126e-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="6126e-202">Более безопасный подход — в том, чтобы сохранить пароль в качестве секрета.</span><span class="sxs-lookup"><span data-stu-id="6126e-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="6126e-203">Пример:</span><span class="sxs-lookup"><span data-stu-id="6126e-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="6126e-204">Удалить `Password` пару ключ значение из строки подключения в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6126e-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="6126e-205">Пример:</span><span class="sxs-lookup"><span data-stu-id="6126e-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="6126e-206">Можно задать значение секрета [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) объекта [пароль](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) свойство, чтобы завершить строку подключения:</span><span class="sxs-lookup"><span data-stu-id="6126e-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="6126e-207">Вывод списка секретов</span><span class="sxs-lookup"><span data-stu-id="6126e-207">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6126e-208">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="6126e-208">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="6126e-209">Появляется следующий результат:</span><span class="sxs-lookup"><span data-stu-id="6126e-209">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="6126e-210">В приведенном выше примере двоеточия в имена ключей означает иерархии объектов в пределах *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="6126e-210">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="6126e-211">Удалить секрет единого</span><span class="sxs-lookup"><span data-stu-id="6126e-211">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6126e-212">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="6126e-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="6126e-213">Приложения *secrets.json* файл был изменен, чтобы удалить пару ключ значение, связанных с `MoviesConnectionString` ключ:</span><span class="sxs-lookup"><span data-stu-id="6126e-213">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="6126e-214">Под управлением `dotnet user-secrets list` отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="6126e-214">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="6126e-215">Удалить все секреты</span><span class="sxs-lookup"><span data-stu-id="6126e-215">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6126e-216">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="6126e-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="6126e-217">Все секреты пользователя приложения будут удалены из *secrets.json* файла:</span><span class="sxs-lookup"><span data-stu-id="6126e-217">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="6126e-218">Под управлением `dotnet user-secrets list` отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="6126e-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="6126e-219">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6126e-219">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
