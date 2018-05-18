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
ms.openlocfilehash: 4db09d3d41b705597f93d05af91077f2b9236b7e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="37d64-103">Безопасного хранения секрета приложения при разработке в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37d64-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="37d64-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [рот Daniel](https://github.com/danroth27), и [Скотт Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="37d64-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="37d64-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37d64-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="37d64-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37d64-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="37d64-107">В этом документе описываются методы для хранения и получения конфиденциальных данных во время разработки приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37d64-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="37d64-108">Никогда не следует хранить пароли и другие конфиденциальные данные в исходном коде, и не следует использовать секреты производства в разработке или протестировать режим.</span><span class="sxs-lookup"><span data-stu-id="37d64-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="37d64-109">Можно хранить и защищать Azure рабочего и тестового секреты с [поставщика хранилища ключей Azure конфигурации](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="37d64-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="37d64-110">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="37d64-110">Environment variables</span></span>

<span data-ttu-id="37d64-111">Чтобы избежать хранения секретов приложения в коде или в локальных файлов конфигурации используются переменные среды.</span><span class="sxs-lookup"><span data-stu-id="37d64-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="37d64-112">Переменные среды переопределяют значения конфигурации для всех источников ранее указанной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="37d64-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="37d64-113">Настройте считывания значений переменных среды, вызвав [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="37d64-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="37d64-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="37d64-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="37d64-115">Рассмотрим веб-приложение ASP.NET Core, в котором **отдельных учетных записей пользователей** включена безопасность.</span><span class="sxs-lookup"><span data-stu-id="37d64-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="37d64-116">Строка подключения базы данных по умолчанию включен в проект *appsettings.json* файл с ключом `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="37d64-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="37d64-117">Строка подключения по умолчанию — для LocalDB, которая запускается в пользовательском режиме и не требует пароля.</span><span class="sxs-lookup"><span data-stu-id="37d64-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="37d64-118">Во время развертывания приложения `DefaultConnection` значение ключа может быть заменено значение переменной среды.</span><span class="sxs-lookup"><span data-stu-id="37d64-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="37d64-119">Полная строка соединения с учетом учетные данные могут быть сохранены переменной среды.</span><span class="sxs-lookup"><span data-stu-id="37d64-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="37d64-120">Переменные среды обычно хранятся в обычный текстовый незашифрованным.</span><span class="sxs-lookup"><span data-stu-id="37d64-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="37d64-121">В случае компрометации компьютера или процесс, переменные среды может осуществляться недоверенные стороны.</span><span class="sxs-lookup"><span data-stu-id="37d64-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="37d64-122">Могут потребоваться дополнительные меры, чтобы предотвратить раскрытие секретные данные пользователей.</span><span class="sxs-lookup"><span data-stu-id="37d64-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="37d64-123">Диспетчер секрет</span><span class="sxs-lookup"><span data-stu-id="37d64-123">Secret Manager</span></span>

<span data-ttu-id="37d64-124">Секрет диспетчера сохраняет конфиденциальные данные во время разработки проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37d64-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="37d64-125">В этом контексте конфиденциальных данных представляет секрет приложения.</span><span class="sxs-lookup"><span data-stu-id="37d64-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="37d64-126">Секретные данные приложения хранятся в отдельном расположении в дереве проекта.</span><span class="sxs-lookup"><span data-stu-id="37d64-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="37d64-127">Секреты приложения связанные с конкретным проектом или совместно использоваться в нескольких проектах.</span><span class="sxs-lookup"><span data-stu-id="37d64-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="37d64-128">Секреты приложения не возвращен в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="37d64-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="37d64-129">Секрет диспетчера не шифрования секретов, хранимых и не должен рассматриваться как доверенного хранилища.</span><span class="sxs-lookup"><span data-stu-id="37d64-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="37d64-130">Это только в целях разработки.</span><span class="sxs-lookup"><span data-stu-id="37d64-130">It's for development purposes only.</span></span> <span data-ttu-id="37d64-131">Ключи и значения хранятся в файле конфигурации JSON в папке профиля пользователя.</span><span class="sxs-lookup"><span data-stu-id="37d64-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="37d64-132">Как работает диспетчер секрет</span><span class="sxs-lookup"><span data-stu-id="37d64-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="37d64-133">Секрет диспетчера абстрагирует детали реализации, например, где и как значения сохраняются.</span><span class="sxs-lookup"><span data-stu-id="37d64-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="37d64-134">Можно использовать средство, не зная эти детали реализации.</span><span class="sxs-lookup"><span data-stu-id="37d64-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="37d64-135">Значения хранятся в [JSON](https://json.org/) файл конфигурации в папке профиля пользователя, защищенные системы на локальном компьютере:</span><span class="sxs-lookup"><span data-stu-id="37d64-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

* <span data-ttu-id="37d64-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="37d64-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span></span>
* <span data-ttu-id="37d64-137">Linux & macOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="37d64-137">Linux & macOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span></span>

<span data-ttu-id="37d64-138">В предыдущем пути к файлам, замените `<user_secrets_id>` с `UserSecretsId` значение, указанное в *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="37d64-138">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="37d64-139">Не создавать код, зависящий от расположения или формат данных, сохраненных с помощью средства диспетчера секрет.</span><span class="sxs-lookup"><span data-stu-id="37d64-139">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="37d64-140">Эти сведения реализации могут быть изменены.</span><span class="sxs-lookup"><span data-stu-id="37d64-140">These implementation details may change.</span></span> <span data-ttu-id="37d64-141">Например значения секрета не зашифрованы, но может быть в будущем.</span><span class="sxs-lookup"><span data-stu-id="37d64-141">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="37d64-142">Установите средство диспетчера секрет</span><span class="sxs-lookup"><span data-stu-id="37d64-142">Install the Secret Manager tool</span></span>

<span data-ttu-id="37d64-143">Секрет диспетчера входит в состав .NET Core CLI в .NET Core SDK 2.1.</span><span class="sxs-lookup"><span data-stu-id="37d64-143">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="37d64-144">Для .NET Core SDK 2.0 и более ранних версиях установки средства не требуется.</span><span class="sxs-lookup"><span data-stu-id="37d64-144">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="37d64-145">Установка [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) пакета NuGet в проекте ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="37d64-145">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="37d64-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="37d64-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="37d64-147">Выполните следующую команду в командной оболочке для проверки установки средства:</span><span class="sxs-lookup"><span data-stu-id="37d64-147">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="37d64-148">Средство диспетчера секрет отображает пример использования, параметры и справки:</span><span class="sxs-lookup"><span data-stu-id="37d64-148">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="37d64-149">Должен быть в том же каталоге, что *.csproj* файла для запуска средства, предназначенные для *.csproj* файла `DotNetCliToolReference` элементов.</span><span class="sxs-lookup"><span data-stu-id="37d64-149">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="37d64-150">Значение секрета</span><span class="sxs-lookup"><span data-stu-id="37d64-150">Set a secret</span></span>

<span data-ttu-id="37d64-151">Секрет диспетчера обрабатывает параметры конфигурации для конкретного проекта, хранимых в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="37d64-151">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="37d64-152">Чтобы использовать секретные данные пользователей, определить `UserSecretsId` элемент в пределах `PropertyGroup` из *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="37d64-152">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="37d64-153">Значение `UserSecretsId` может быть произвольным, но является уникальным для проекта.</span><span class="sxs-lookup"><span data-stu-id="37d64-153">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="37d64-154">Разработчики обычно создать GUID для `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="37d64-154">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="37d64-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="37d64-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="37d64-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="37d64-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="37d64-157">В Visual Studio, щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление секретами пользователя** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="37d64-157">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="37d64-158">Добавляет этот жест `UserSecretsId` элемент, заполненный идентификатора GUID, с *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="37d64-158">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="37d64-159">Visual Studio открывает *secrets.json* файл в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="37d64-159">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="37d64-160">Замените содержимое *secrets.json* с пары «ключ значение» для сохранения.</span><span class="sxs-lookup"><span data-stu-id="37d64-160">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="37d64-161">Пример: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="37d64-161">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="37d64-162">Определите секрет приложения, состоящая из ключа и его значение.</span><span class="sxs-lookup"><span data-stu-id="37d64-162">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="37d64-163">Секрет, связанного с данным проектом `UserSecretsId` значение.</span><span class="sxs-lookup"><span data-stu-id="37d64-163">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="37d64-164">Например, выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="37d64-164">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="37d64-165">В приведенном выше примере двоеточия обозначает, `Movies` — это объект литерала с `ServiceApiKey` свойство.</span><span class="sxs-lookup"><span data-stu-id="37d64-165">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="37d64-166">Секрет диспетчера можно использовать слишком из других каталогов.</span><span class="sxs-lookup"><span data-stu-id="37d64-166">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="37d64-167">Используйте `--project` параметр, чтобы указать путь в файловой системе, с которой *.csproj* файл существует.</span><span class="sxs-lookup"><span data-stu-id="37d64-167">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="37d64-168">Пример:</span><span class="sxs-lookup"><span data-stu-id="37d64-168">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="37d64-169">Задать несколько секретов</span><span class="sxs-lookup"><span data-stu-id="37d64-169">Set multiple secrets</span></span>

<span data-ttu-id="37d64-170">Пакет секретные данные можно задать по конвейеру JSON, `set` команды.</span><span class="sxs-lookup"><span data-stu-id="37d64-170">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="37d64-171">В следующем примере *input.json* содержимое файла передаются по конвейеру в `set` команды в Windows:</span><span class="sxs-lookup"><span data-stu-id="37d64-171">In the following example, the *input.json* file's contents are piped to the `set` command on Windows:</span></span>

```console
type .\input.json | dotnet user-secrets set
```

<span data-ttu-id="37d64-172">В macOS и Linux, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="37d64-172">Use the following command on macOS and Linux:</span></span>

```console
cat ./input.json | dotnet user-secrets set
```

## <a name="access-a-secret"></a><span data-ttu-id="37d64-173">Доступ к секрета</span><span class="sxs-lookup"><span data-stu-id="37d64-173">Access a secret</span></span>

<span data-ttu-id="37d64-174">[Конфигурации API ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к Manager секрет секретные данные.</span><span class="sxs-lookup"><span data-stu-id="37d64-174">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="37d64-175">Если для различных версий .NET Core 1.x или .NET Framework, установите [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="37d64-175">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="37d64-176">Добавить источник конфигурации секретные данные пользователя для `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="37d64-176">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="37d64-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="37d64-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="37d64-178">Секреты пользователя можно получить через `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="37d64-178">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="37d64-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="37d64-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="37d64-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="37d64-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="37d64-181">Строка замены с секретными данными</span><span class="sxs-lookup"><span data-stu-id="37d64-181">String replacement with secrets</span></span>

<span data-ttu-id="37d64-182">Рискованно хранить пароли в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="37d64-182">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="37d64-183">Например, строка подключения базы данных хранятся в *appsettings.json* может быть указан пароль для указанного пользователя:</span><span class="sxs-lookup"><span data-stu-id="37d64-183">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="37d64-184">Более безопасный подход — хранить пароль в качестве секрета.</span><span class="sxs-lookup"><span data-stu-id="37d64-184">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="37d64-185">Пример:</span><span class="sxs-lookup"><span data-stu-id="37d64-185">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="37d64-186">Замените пароль в *appsettings.json* заполнителем.</span><span class="sxs-lookup"><span data-stu-id="37d64-186">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="37d64-187">В следующем примере `{0}` используется в качестве заполнителя форму [Строка составного формата](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="37d64-187">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="37d64-188">Значение секрета могут быть добавлены в заполнитель, чтобы завершить строку подключения:</span><span class="sxs-lookup"><span data-stu-id="37d64-188">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="37d64-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="37d64-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="37d64-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="37d64-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="37d64-191">Вывод списка секретов</span><span class="sxs-lookup"><span data-stu-id="37d64-191">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="37d64-192">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="37d64-192">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="37d64-193">Появляется следующий результат:</span><span class="sxs-lookup"><span data-stu-id="37d64-193">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="37d64-194">В приведенном выше примере двоеточия в имена ключей обозначает иерархии объектов в пределах *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="37d64-194">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="37d64-195">Удалите один секрет</span><span class="sxs-lookup"><span data-stu-id="37d64-195">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="37d64-196">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="37d64-196">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="37d64-197">Приложения *secrets.json* файл был изменен, чтобы удалить пару ключ значение, связанных с `MoviesConnectionString` ключ:</span><span class="sxs-lookup"><span data-stu-id="37d64-197">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="37d64-198">Под управлением `dotnet user-secrets list` отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="37d64-198">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="37d64-199">Удалить все секреты</span><span class="sxs-lookup"><span data-stu-id="37d64-199">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="37d64-200">Выполните следующую команду из каталога, в котором *.csproj* файл существует:</span><span class="sxs-lookup"><span data-stu-id="37d64-200">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="37d64-201">Все секреты пользователя для приложения были удалены из *secrets.json* файла:</span><span class="sxs-lookup"><span data-stu-id="37d64-201">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="37d64-202">Под управлением `dotnet user-secrets list` отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="37d64-202">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="37d64-203">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="37d64-203">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>