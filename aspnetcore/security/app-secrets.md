---
title: Надежное хранение секретов приложений в разработке в ASP.NET Core
author: rick-anderson
description: Узнайте, как хранить и извлекать конфиденциальную информацию в виде секретов приложения во время разработки ASP.NET Core приложения.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 0203a5737caf1af809b739d9e266a6971cd1523b
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080716"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="185e4-103">Надежное хранение секретов приложений в разработке в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="185e4-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="185e4-104">[Рик Андерсон (](https://twitter.com/RickAndMSFT), [Даниэль Roth)](https://github.com/danroth27)и [Скотт Эдди (](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="185e4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="185e4-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="185e4-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="185e4-106">В этом документе объясняются способы хранения и извлечения конфиденциальных данных во время разработки ASP.NET Core приложения.</span><span class="sxs-lookup"><span data-stu-id="185e4-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="185e4-107">Никогда не храните пароли или другие конфиденциальные данные в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="185e4-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="185e4-108">Производственные секреты не должны использоваться для разработки или тестирования.</span><span class="sxs-lookup"><span data-stu-id="185e4-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="185e4-109">Для хранения и защиты секретов Azure в ходе тестирования и непосредственной работы используйте [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="185e4-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="185e4-110">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="185e4-110">Environment variables</span></span>

<span data-ttu-id="185e4-111">Переменные среды используются, чтобы избежать хранения секретов приложения в коде или в локальных файлах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="185e4-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="185e4-112">Переменные среды переопределяют значения конфигурации для всех ранее указанных источников конфигурации.</span><span class="sxs-lookup"><span data-stu-id="185e4-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="185e4-113">Настройте чтение значений переменных среды, вызвав <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> `Startup` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="185e4-113">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="185e4-114">Рассмотрим ASP.NET Core веб-приложение, в котором включена безопасность **отдельных учетных записей пользователей** .</span><span class="sxs-lookup"><span data-stu-id="185e4-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="185e4-115">Строка подключения к базе данных по умолчанию включается в файл *appSettings. JSON* проекта с ключом `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="185e4-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="185e4-116">Строка подключения по умолчанию — для LocalDB, которая выполняется в пользовательском режиме и не требует пароля.</span><span class="sxs-lookup"><span data-stu-id="185e4-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="185e4-117">Во время развертывания `DefaultConnection` приложения значение ключа может быть переопределено значением переменной среды.</span><span class="sxs-lookup"><span data-stu-id="185e4-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="185e4-118">Переменная среды может хранить полную строку подключения с конфиденциальными учетными данными.</span><span class="sxs-lookup"><span data-stu-id="185e4-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="185e4-119">Переменные среды обычно хранятся в виде обычного незашифрованного текста.</span><span class="sxs-lookup"><span data-stu-id="185e4-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="185e4-120">Если компьютер или процесс скомпрометирован, недоверенные стороны могут получить доступ к переменным среды.</span><span class="sxs-lookup"><span data-stu-id="185e4-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="185e4-121">Могут потребоваться дополнительные меры для предотвращения раскрытия секретов пользователя.</span><span class="sxs-lookup"><span data-stu-id="185e4-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="185e4-122">Диспетчер секретов</span><span class="sxs-lookup"><span data-stu-id="185e4-122">Secret Manager</span></span>

<span data-ttu-id="185e4-123">Средство диспетчера секретов сохраняет конфиденциальные данные во время разработки проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="185e4-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="185e4-124">В этом контексте фрагмент конфиденциальных данных является секретом приложения.</span><span class="sxs-lookup"><span data-stu-id="185e4-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="185e4-125">Секреты приложения хранятся в отдельном месте из дерева проекта.</span><span class="sxs-lookup"><span data-stu-id="185e4-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="185e4-126">Секреты приложения связаны с конкретным проектом или совместно используются в нескольких проектах.</span><span class="sxs-lookup"><span data-stu-id="185e4-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="185e4-127">Секреты приложения не возвращаются в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="185e4-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="185e4-128">Диспетчер секретов не шифрует сохраненные секреты и не должен рассматриваться как доверенное хранилище.</span><span class="sxs-lookup"><span data-stu-id="185e4-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="185e4-129">Это только для целей разработки.</span><span class="sxs-lookup"><span data-stu-id="185e4-129">It's for development purposes only.</span></span> <span data-ttu-id="185e4-130">Ключи и значения хранятся в файле конфигурации JSON в каталоге профиля пользователя.</span><span class="sxs-lookup"><span data-stu-id="185e4-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="185e4-131">Как работает средство диспетчера секретов</span><span class="sxs-lookup"><span data-stu-id="185e4-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="185e4-132">Диспетчер секретов абстрагирует детали реализации, например, где и как хранятся значения.</span><span class="sxs-lookup"><span data-stu-id="185e4-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="185e4-133">Вы можете использовать это средство, не зная сведений о реализации.</span><span class="sxs-lookup"><span data-stu-id="185e4-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="185e4-134">Значения хранятся в файле конфигурации JSON в папке профиля пользователя, защищенной системой, на локальном компьютере:</span><span class="sxs-lookup"><span data-stu-id="185e4-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="185e4-135">Windows</span><span class="sxs-lookup"><span data-stu-id="185e4-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="185e4-136">Путь к файловой системе:</span><span class="sxs-lookup"><span data-stu-id="185e4-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="185e4-137">Linux и macOS</span><span class="sxs-lookup"><span data-stu-id="185e4-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="185e4-138">Путь к файловой системе:</span><span class="sxs-lookup"><span data-stu-id="185e4-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="185e4-139">В приведенных выше путях файлов замените `<user_secrets_id>` `UserSecretsId` значением, указанным в файле *CSPROJ* .</span><span class="sxs-lookup"><span data-stu-id="185e4-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="185e4-140">Не записывайте код, который зависит от расположения или формата данных, сохраненных с помощью диспетчера секретов.</span><span class="sxs-lookup"><span data-stu-id="185e4-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="185e4-141">Эти сведения о реализации могут измениться.</span><span class="sxs-lookup"><span data-stu-id="185e4-141">These implementation details may change.</span></span> <span data-ttu-id="185e4-142">Например, секретные значения не шифруются, но могут быть в будущем.</span><span class="sxs-lookup"><span data-stu-id="185e4-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="185e4-143">Установка диспетчера секретов</span><span class="sxs-lookup"><span data-stu-id="185e4-143">Install the Secret Manager tool</span></span>

<span data-ttu-id="185e4-144">Диспетчер секретов объединяется с .NET Core CLI в пакет SDK для .NET Core 2.1.300 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="185e4-144">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="185e4-145">Для пакет SDK для .NET Core версий перед 2.1.300 необходимо установить средство.</span><span class="sxs-lookup"><span data-stu-id="185e4-145">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="185e4-146">Выполните `dotnet --version` команду из командной оболочки, чтобы просмотреть номер версии установленного пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="185e4-146">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="185e4-147">Если используемое пакет SDK для .NET Core включает средство, отображается предупреждение:</span><span class="sxs-lookup"><span data-stu-id="185e4-147">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="185e4-148">Установите пакет NuGet [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="185e4-148">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="185e4-149">Например:</span><span class="sxs-lookup"><span data-stu-id="185e4-149">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="185e4-150">Выполните следующую команду в командной оболочке, чтобы проверить установку средства:</span><span class="sxs-lookup"><span data-stu-id="185e4-150">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="185e4-151">В средстве "Диспетчер секретов" отображаются примеры использования, параметры и Справка по командам:</span><span class="sxs-lookup"><span data-stu-id="185e4-151">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="185e4-152">Для запуска инструментов, определенных в `DotNetCliToolReference` элементах *. csproj* , необходимо находиться в том же каталоге, что и файл *CSPROJ* .</span><span class="sxs-lookup"><span data-stu-id="185e4-152">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="185e4-153">Включить хранилище секретов</span><span class="sxs-lookup"><span data-stu-id="185e4-153">Enable secret storage</span></span>

<span data-ttu-id="185e4-154">Диспетчер секретов работает с параметрами конфигурации конкретного проекта, хранящимися в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="185e4-154">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="185e4-155">Средство диспетчера секретов включает `init` команду в пакет SDK для .NET Core 3.0.100 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="185e4-155">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="185e4-156">Чтобы использовать секреты пользователя, выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="185e4-156">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="185e4-157">Предыдущая команда добавляет `UserSecretsId` элемент `PropertyGroup` в файл *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="185e4-157">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="185e4-158">По умолчанию внутренний текст `UserSecretsId` является идентификатором GUID.</span><span class="sxs-lookup"><span data-stu-id="185e4-158">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="185e4-159">Внутренний текст является произвольным, но уникален для проекта.</span><span class="sxs-lookup"><span data-stu-id="185e4-159">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="185e4-160">Чтобы использовать секреты пользователя, определите `UserSecretsId` элемент `PropertyGroup` в файле *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="185e4-160">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="185e4-161">Внутренний текст `UserSecretsId` является произвольным, но уникален для проекта.</span><span class="sxs-lookup"><span data-stu-id="185e4-161">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="185e4-162">Разработчики обычно создают идентификатор GUID для `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="185e4-162">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="185e4-163">В Visual Studio щелкните правой кнопкой мыши проект в обозреватель решений и выберите в контекстном меню пункт **Управление секретами пользователя** .</span><span class="sxs-lookup"><span data-stu-id="185e4-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="185e4-164">Этот жест добавляет `UserSecretsId` элемент, заполненный идентификатором GUID, в *CSPROJ* -файл.</span><span class="sxs-lookup"><span data-stu-id="185e4-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="185e4-165">Задать секрет</span><span class="sxs-lookup"><span data-stu-id="185e4-165">Set a secret</span></span>

<span data-ttu-id="185e4-166">Определите секрет приложения, состоящий из ключа и его значения.</span><span class="sxs-lookup"><span data-stu-id="185e4-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="185e4-167">Секрет связан со `UserSecretsId` значением проекта.</span><span class="sxs-lookup"><span data-stu-id="185e4-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="185e4-168">Например, выполните следующую команду из каталога, в котором существует CSPROJ файл *.*</span><span class="sxs-lookup"><span data-stu-id="185e4-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="185e4-169">В предыдущем примере двоеточие обозначает, что `Movies` является объектным литералом `ServiceApiKey` со свойством.</span><span class="sxs-lookup"><span data-stu-id="185e4-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="185e4-170">Диспетчер секретов можно использовать и в других каталогах.</span><span class="sxs-lookup"><span data-stu-id="185e4-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="185e4-171">Используйте параметр, чтобы указать путь файловой системы, в котором существует файл *. csproj.* `--project`</span><span class="sxs-lookup"><span data-stu-id="185e4-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="185e4-172">Например:</span><span class="sxs-lookup"><span data-stu-id="185e4-172">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="185e4-173">Сведение структуры JSON в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="185e4-173">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="185e4-174">Жест **управления пользовательскими секретами** в Visual Studio открывает файл *секреты. JSON* в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="185e4-174">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="185e4-175">Замените содержимое файла *секреты. JSON* хранимыми парами "ключ-значение".</span><span class="sxs-lookup"><span data-stu-id="185e4-175">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="185e4-176">Например:</span><span class="sxs-lookup"><span data-stu-id="185e4-176">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="185e4-177">Структура JSON преобразуется в плоскую структуру после `dotnet user-secrets remove` изменений `dotnet user-secrets set`с помощью или.</span><span class="sxs-lookup"><span data-stu-id="185e4-177">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="185e4-178">Например, запуск `dotnet user-secrets remove "Movies:ConnectionString"` сворачивает `Movies` объектный литерал.</span><span class="sxs-lookup"><span data-stu-id="185e4-178">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="185e4-179">Измененный файл выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="185e4-179">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="185e4-180">Задание нескольких секретов</span><span class="sxs-lookup"><span data-stu-id="185e4-180">Set multiple secrets</span></span>

<span data-ttu-id="185e4-181">Пакет секретов можно задать с помощью конвейера `set` командной строки.</span><span class="sxs-lookup"><span data-stu-id="185e4-181">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="185e4-182">В следующем примере содержимое *входного файла JSON* передается `set` команде.</span><span class="sxs-lookup"><span data-stu-id="185e4-182">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="185e4-183">Windows</span><span class="sxs-lookup"><span data-stu-id="185e4-183">Windows</span></span>](#tab/windows)

<span data-ttu-id="185e4-184">Откройте командную оболочку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="185e4-184">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="185e4-185">Linux и macOS</span><span class="sxs-lookup"><span data-stu-id="185e4-185">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="185e4-186">Откройте командную оболочку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="185e4-186">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="185e4-187">Доступ к секрету</span><span class="sxs-lookup"><span data-stu-id="185e4-187">Access a secret</span></span>

<span data-ttu-id="185e4-188">[API настройки ASP.NET Core](xref:fundamentals/configuration/index) предоставляет доступ к секретам диспетчера секретов.</span><span class="sxs-lookup"><span data-stu-id="185e4-188">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="185e4-189">Если проект предназначен для .NET Framework, установите пакет NuGet [Microsoft. Extensions. Configuration. усерсекретс](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="185e4-189">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="185e4-190">В ASP.NET Core 2,0 или более поздней версии источник конфигурации секреты пользователя автоматически добавляется в режим разработки, когда проект <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> вызывает инициализацию нового экземпляра узла с предварительно настроенными по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="185e4-190">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="185e4-191">`CreateDefaultBuilder`вызывает <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> , <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> если имеет<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>значение:</span><span class="sxs-lookup"><span data-stu-id="185e4-191">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="185e4-192">Если `CreateDefaultBuilder` метод не вызывается, добавьте источник конфигурации секреты пользователя явным образом <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> , вызвав в `Startup` конструкторе.</span><span class="sxs-lookup"><span data-stu-id="185e4-192">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="185e4-193">Вызывайте `AddUserSecrets` только при запуске приложения в среде разработки, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="185e4-193">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="185e4-194">Установите пакет NuGet [Microsoft. Extensions. Configuration. усерсекретс](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="185e4-194">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="185e4-195">Добавьте источник конфигурации секреты пользователя с помощью вызова <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> `Startup` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="185e4-195">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="185e4-196">Секреты пользователя можно получить с `Configuration` помощью API:</span><span class="sxs-lookup"><span data-stu-id="185e4-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="185e4-197">Преобразование секретов в POCO</span><span class="sxs-lookup"><span data-stu-id="185e4-197">Map secrets to a POCO</span></span>

<span data-ttu-id="185e4-198">Сопоставление целого литерала объекта с POCO (простой класс .NET со свойствами) полезен для статистической обработки связанных свойств.</span><span class="sxs-lookup"><span data-stu-id="185e4-198">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="185e4-199">Чтобы связать предыдущие секреты с POCO, используйте `Configuration` функцию [привязки графа объектов](xref:fundamentals/configuration/index#bind-to-an-object-graph) API.</span><span class="sxs-lookup"><span data-stu-id="185e4-199">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="185e4-200">Следующий код привязывается к пользовательской `MovieSettings` POCO и обращается `ServiceApiKey` к значению свойства:</span><span class="sxs-lookup"><span data-stu-id="185e4-200">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="185e4-201">Секреты `Movies:ConnectionString` `MovieSettings`и `Movies:ServiceApiKey` сопоставлены с соответствующими свойствами в:</span><span class="sxs-lookup"><span data-stu-id="185e4-201">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="185e4-202">Замена строк секретами</span><span class="sxs-lookup"><span data-stu-id="185e4-202">String replacement with secrets</span></span>

<span data-ttu-id="185e4-203">Хранение паролей в виде обычного текста является небезопасным.</span><span class="sxs-lookup"><span data-stu-id="185e4-203">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="185e4-204">Например, строка подключения к базе данных, хранящаяся в *appSettings. JSON* , может включать пароль для указанного пользователя:</span><span class="sxs-lookup"><span data-stu-id="185e4-204">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="185e4-205">Более безопасный подход заключается в хранении пароля в качестве секрета.</span><span class="sxs-lookup"><span data-stu-id="185e4-205">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="185e4-206">Например:</span><span class="sxs-lookup"><span data-stu-id="185e4-206">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="185e4-207">Удалите пару " ключ-значение"изстрокиподключениявappSettings.`Password` JSON.</span><span class="sxs-lookup"><span data-stu-id="185e4-207">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="185e4-208">Например:</span><span class="sxs-lookup"><span data-stu-id="185e4-208">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="185e4-209">Значение секрета может быть задано для <xref:System.Data.SqlClient.SqlConnectionStringBuilder> <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> свойства объекта, чтобы завершить строку подключения:</span><span class="sxs-lookup"><span data-stu-id="185e4-209">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="185e4-210">Вывод списка секретов</span><span class="sxs-lookup"><span data-stu-id="185e4-210">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="185e4-211">Выполните следующую команду из каталога, в котором существует файл *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="185e4-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="185e4-212">Появится следующий результат:</span><span class="sxs-lookup"><span data-stu-id="185e4-212">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="185e4-213">В предыдущем примере двоеточие в именах ключей обозначает иерархию объектов в *тайне. JSON*.</span><span class="sxs-lookup"><span data-stu-id="185e4-213">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="185e4-214">Удаление одного секрета</span><span class="sxs-lookup"><span data-stu-id="185e4-214">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="185e4-215">Выполните следующую команду из каталога, в котором существует файл *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="185e4-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="185e4-216">Файл *секреты. JSON* приложения был изменен для удаления пары "ключ-значение", связанной с `MoviesConnectionString` ключом:</span><span class="sxs-lookup"><span data-stu-id="185e4-216">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="185e4-217">При `dotnet user-secrets list` выполнении отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="185e4-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="185e4-218">Удалить все секреты</span><span class="sxs-lookup"><span data-stu-id="185e4-218">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="185e4-219">Выполните следующую команду из каталога, в котором существует файл *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="185e4-219">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="185e4-220">Все секреты пользователя для приложения удалены из файла *секреты. JSON* :</span><span class="sxs-lookup"><span data-stu-id="185e4-220">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="185e4-221">При `dotnet user-secrets list` выполнении отображается следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="185e4-221">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="185e4-222">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="185e4-222">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
