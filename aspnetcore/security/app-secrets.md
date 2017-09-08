---
title: "Безопасного хранения секрета приложения во время разработка решений в ASP.NET Core"
author: rick-anderson
description: "Показано, как безопасно хранить секреты во время разработки"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: 99a1129549d6b9802315c7e5accfa22907994a41
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="04f80-104">Безопасного хранения секрета приложения во время разработки в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04f80-104">Safe storage of app secrets during development in ASP.NET Core</span></span>

<a name=security-app-secrets></a>

<span data-ttu-id="04f80-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [рот Daniel](https://github.com/danroth27), и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="04f80-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="04f80-106">В этом документе показано, как можно использовать диспетчер секрет при разработке решений для хранить секреты вне кода.</span><span class="sxs-lookup"><span data-stu-id="04f80-106">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="04f80-107">Самое главное, никогда не следует хранить пароли и другие конфиденциальные данные в исходном коде и секретные данные в рабочей среде не следует использовать в режиме разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="04f80-107">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="04f80-108">Вместо этого можно использовать [конфигурации](../fundamentals/configuration.md) системы для считывания этих значений из переменных среды или из значения, сохраненные с помощью диспетчера секрет средства.</span><span class="sxs-lookup"><span data-stu-id="04f80-108">You can instead use the [configuration](../fundamentals/configuration.md) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="04f80-109">Секрет диспетчера предотвращает конфиденциальные данные проверяются в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="04f80-109">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="04f80-110">[Конфигурации](../fundamentals/configuration.md) системы, может прочитать секретные данные, хранимые с помощью средства диспетчера секрет, описанных в этой статье.</span><span class="sxs-lookup"><span data-stu-id="04f80-110">The [configuration](../fundamentals/configuration.md) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="04f80-111">Секрет диспетчера используется только при разработке.</span><span class="sxs-lookup"><span data-stu-id="04f80-111">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="04f80-112">Можно защитить Azure рабочего и тестового секреты с [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="04f80-112">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="04f80-113">В разделе [конфигурации поставщика хранилища ключей Azure](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="04f80-113">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="04f80-114">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="04f80-114">Environment variables</span></span>

<span data-ttu-id="04f80-115">Чтобы избежать хранения секрета приложения в коде или в локальных файлов конфигурации, хранить секреты в переменных среды.</span><span class="sxs-lookup"><span data-stu-id="04f80-115">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="04f80-116">Вы можете настроить [конфигурации](../fundamentals/configuration.md) framework для считывания значений из переменных среды, вызвав `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="04f80-116">You can setup the [configuration](../fundamentals/configuration.md) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="04f80-117">Затем можно использовать переменные среды для переопределения значения конфигурации для всех источников ранее указанной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="04f80-117">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="04f80-118">Например, при создании нового веб-приложения ASP.NET Core для каждой учетной записи, оно добавляет строку подключения по умолчанию для *appsettings.json* файл в проекте с помощью ключа `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="04f80-118">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="04f80-119">Строка подключения по умолчанию настроена на использование LocalDB, которая запускается в пользовательском режиме и не требует пароля.</span><span class="sxs-lookup"><span data-stu-id="04f80-119">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="04f80-120">При развертывании приложения на тестовом или рабочем сервере, можно переопределить `DefaultConnection` значение ключа с параметром переменной среды, содержащее строку подключения (возможно, с учетом учетные данные) для тестовой или рабочей базы данных сервер.</span><span class="sxs-lookup"><span data-stu-id="04f80-120">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="04f80-121">Переменные среды обычно хранятся в виде обычного текста и не шифруются.</span><span class="sxs-lookup"><span data-stu-id="04f80-121">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="04f80-122">В случае компрометации компьютера или процесс, переменные среды возможен ненадежных сторонами.</span><span class="sxs-lookup"><span data-stu-id="04f80-122">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="04f80-123">По-прежнему могут требоваться дополнительные меры, чтобы предотвратить раскрытие секретные данные пользователей.</span><span class="sxs-lookup"><span data-stu-id="04f80-123">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="04f80-124">Диспетчер секрет</span><span class="sxs-lookup"><span data-stu-id="04f80-124">Secret Manager</span></span>

<span data-ttu-id="04f80-125">Секрет диспетчера сохраняет конфиденциальные данные вне вашего дерева проектов для разработки.</span><span class="sxs-lookup"><span data-stu-id="04f80-125">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="04f80-126">Секрет диспетчера — это средство проекта, который может использоваться для хранения конфиденциальных данных для [.NET Core](https://microsoft.com/net/core) проекта во время разработки.</span><span class="sxs-lookup"><span data-stu-id="04f80-126">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="04f80-127">С помощью средства диспетчера секрет можно связать секрета приложения с конкретным проектом и использовать их совместно в нескольких проектах.</span><span class="sxs-lookup"><span data-stu-id="04f80-127">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="04f80-128">Секрет диспетчера хранимых секретные данные не шифруются и не будет считаться доверенного хранилища.</span><span class="sxs-lookup"><span data-stu-id="04f80-128">The Secret Manager tool does not encrypt the stored secrets and should not be treated as a trusted store.</span></span> <span data-ttu-id="04f80-129">Это только в целях разработки.</span><span class="sxs-lookup"><span data-stu-id="04f80-129">It is for development purposes only.</span></span> <span data-ttu-id="04f80-130">Ключи и значения хранятся в файле конфигурации JSON в папке профиля пользователя.</span><span class="sxs-lookup"><span data-stu-id="04f80-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

### <a name="visual-studio-2017-installing-the-secret-manager-tool"></a><span data-ttu-id="04f80-131">Visual Studio 2017 г.: Секрет диспетчера установки</span><span class="sxs-lookup"><span data-stu-id="04f80-131">Visual Studio 2017: Installing the Secret Manager tool</span></span>

<span data-ttu-id="04f80-132">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **изменить \<project_name\>.csproj** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="04f80-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="04f80-133">Добавьте в выделенной строке *.csproj* и сохранение файлов для восстановления, связанный с ним пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="04f80-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

<span data-ttu-id="04f80-134">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="04f80-134">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span></span>

<span data-ttu-id="04f80-135">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление секретами пользователя** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="04f80-135">Right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="04f80-136">Это жест добавляет новый `UserSecretsId` узла в `PropertyGroup` из *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="04f80-136">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="04f80-137">Также открывает `secrets.json` файл в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="04f80-137">It also opens a `secrets.json` file in the text editor.</span></span>

<span data-ttu-id="04f80-138">Добавьте следующий код в файл `secrets.json`:</span><span class="sxs-lookup"><span data-stu-id="04f80-138">Add the following to `secrets.json`:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-2015-installing-the-secret-manager-tool"></a><span data-ttu-id="04f80-139">Visual Studio 2015: Секрет диспетчера установки</span><span class="sxs-lookup"><span data-stu-id="04f80-139">Visual Studio 2015: Installing the Secret Manager tool</span></span>

<span data-ttu-id="04f80-140">Откройте проект `project.json` файла.</span><span class="sxs-lookup"><span data-stu-id="04f80-140">Open the project's `project.json` file.</span></span> <span data-ttu-id="04f80-141">Добавьте ссылку на `Microsoft.Extensions.SecretManager.Tools` в `tools` свойство и сохранить, чтобы восстановить связанный с ним пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="04f80-141">Add a reference to `Microsoft.Extensions.SecretManager.Tools` within the `tools` property, and save to restore the associated NuGet package:</span></span>

```json
"tools": {
    "Microsoft.Extensions.SecretManager.Tools": "1.0.0-preview2-final",
    "Microsoft.AspNetCore.Server.IISIntegration.Tools": "1.0.0-preview2-final"
},
```

<span data-ttu-id="04f80-142">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление секретами пользователя** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="04f80-142">Right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="04f80-143">Это жест добавляет новый `userSecretsId` свойства `project.json`.</span><span class="sxs-lookup"><span data-stu-id="04f80-143">This gesture adds a new `userSecretsId` property to `project.json`.</span></span> <span data-ttu-id="04f80-144">Также открывает `secrets.json` файл в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="04f80-144">It also opens a `secrets.json` file in the text editor.</span></span>

<span data-ttu-id="04f80-145">Добавьте следующий код в файл `secrets.json`:</span><span class="sxs-lookup"><span data-stu-id="04f80-145">Add the following to `secrets.json`:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-code-or-command-line-installing-the-secret-manager-tool"></a><span data-ttu-id="04f80-146">Visual Studio Code или командной строки: Установка средства диспетчера секрет</span><span class="sxs-lookup"><span data-stu-id="04f80-146">Visual Studio Code or Command Line: Installing the Secret Manager tool</span></span>

<span data-ttu-id="04f80-147">Добавить `Microsoft.Extensions.SecretManager.Tools` для *.csproj* файл и запустите `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="04f80-147">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run `dotnet restore`.</span></span>

<span data-ttu-id="04f80-148">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="04f80-148">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span></span>

<span data-ttu-id="04f80-149">Тестирование диспетчера секрет, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="04f80-149">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="04f80-150">Диспетчер секрет выводится использования, параметры и справки.</span><span class="sxs-lookup"><span data-stu-id="04f80-150">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="04f80-151">Должен быть в том же каталоге, что *.csproj* файла для запуска средства, предназначенные для *.csproj* файла `DotNetCliToolReference` узлов.</span><span class="sxs-lookup"><span data-stu-id="04f80-151">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="04f80-152">Секрет диспетчера обрабатывает параметры конфигурации для конкретного проекта, которые хранятся в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="04f80-152">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="04f80-153">Чтобы использовать секретные данные пользователей, необходимо указать проект `UserSecretsId` значение в его *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="04f80-153">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="04f80-154">Значение `UserSecretsId` может быть произвольным, но уникален в проект.</span><span class="sxs-lookup"><span data-stu-id="04f80-154">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="04f80-155">Разработчики обычно создать GUID для `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="04f80-155">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="04f80-156">Добавить `UserSecretsId` проекта в *.csproj* файла:</span><span class="sxs-lookup"><span data-stu-id="04f80-156">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

<span data-ttu-id="04f80-157">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?range=7-9&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="04f80-157">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?range=7-9&highlight=2)]</span></span>

<span data-ttu-id="04f80-158">Использование диспетчера секрет, чтобы задать секрет.</span><span class="sxs-lookup"><span data-stu-id="04f80-158">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="04f80-159">Например в окно командной строки от каталога проекта, введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="04f80-159">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="04f80-160">Запустить диспетчер секрет из других каталогов, но необходимо использовать `--project` параметр для передачи в пути к *.csproj* файла:</span><span class="sxs-lookup"><span data-stu-id="04f80-160">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="04f80-161">Также можно использовать средство диспетчера секрет для перечисления, удаления и очистите секреты приложения.</span><span class="sxs-lookup"><span data-stu-id="04f80-161">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="04f80-162">Доступ к секретной информации пользователя через конфигурацию</span><span class="sxs-lookup"><span data-stu-id="04f80-162">Accessing user secrets via configuration</span></span>

<span data-ttu-id="04f80-163">Секреты Manager секрет доступ через систему конфигурации.</span><span class="sxs-lookup"><span data-stu-id="04f80-163">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="04f80-164">Добавить `Microsoft.Extensions.Configuration.UserSecrets` упаковка и запуск `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="04f80-164">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run `dotnet restore`.</span></span>

<span data-ttu-id="04f80-165">Добавить источник конфигурации секретные данные пользователя для `Startup` метод:</span><span class="sxs-lookup"><span data-stu-id="04f80-165">Add the user secrets configuration source to the `Startup` method:</span></span>

<span data-ttu-id="04f80-166">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]</span><span class="sxs-lookup"><span data-stu-id="04f80-166">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]</span></span>

<span data-ttu-id="04f80-167">Вы можете использовать секретные данные пользователей через API-Интерфейс настройки:</span><span class="sxs-lookup"><span data-stu-id="04f80-167">You can access user secrets via the configuration API:</span></span>

<span data-ttu-id="04f80-168">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="04f80-168">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="04f80-169">Как работает диспетчер секрет</span><span class="sxs-lookup"><span data-stu-id="04f80-169">How the Secret Manager tool works</span></span>

<span data-ttu-id="04f80-170">Секрет диспетчера абстрагирует детали реализации, например, где и как значения сохраняются.</span><span class="sxs-lookup"><span data-stu-id="04f80-170">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="04f80-171">Можно использовать средство, не зная эти детали реализации.</span><span class="sxs-lookup"><span data-stu-id="04f80-171">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="04f80-172">В текущей версии значения хранятся в [JSON](http://json.org/) файл конфигурации в папке профиля пользователя:</span><span class="sxs-lookup"><span data-stu-id="04f80-172">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="04f80-173">Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="04f80-173">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="04f80-174">Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="04f80-174">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="04f80-175">MAC:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="04f80-175">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="04f80-176">Значение `userSecretsId` берется из значения, указанного в *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="04f80-176">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="04f80-177">Не следует писать код, зависящий от расположение или формат данных, сохраненных с помощью диспетчера секрет средства, как эти сведения реализации может измениться.</span><span class="sxs-lookup"><span data-stu-id="04f80-177">You should not write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="04f80-178">Например, в настоящее время являются значения секрета *не* сегодня зашифрованы, но может быть когда-нибудь.</span><span class="sxs-lookup"><span data-stu-id="04f80-178">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04f80-179">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="04f80-179">Additional Resources</span></span>

* [<span data-ttu-id="04f80-180">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="04f80-180">Configuration</span></span>](../fundamentals/configuration.md)
