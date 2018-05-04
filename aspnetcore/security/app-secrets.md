---
title: Безопасного хранения секрета приложения при разработке в ASP.NET Core
author: rick-anderson
description: Показано, как безопасно хранить секреты во время разработки
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 0a04f5762a35426f342b58b8b60288c66c057ae7
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="c4ea1-103">Безопасного хранения секрета приложения при разработке в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4ea1-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="c4ea1-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [рот Daniel](https://github.com/danroth27), и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c4ea1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="c4ea1-105">В этом документе показано, как можно использовать диспетчер секрет при разработке решений для хранить секреты вне кода.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-105">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="c4ea1-106">Самое главное, никогда не следует хранить пароли и другие конфиденциальные данные в исходном коде и секретные данные в рабочей среде не следует использовать в режиме разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-106">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="c4ea1-107">Вместо этого можно использовать [конфигурации](xref:fundamentals/configuration/index) системы для считывания этих значений из переменных среды или из значения, сохраненные с помощью диспетчера секрет средства.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-107">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="c4ea1-108">Секрет диспетчера предотвращает конфиденциальные данные проверяются в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-108">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="c4ea1-109">[Конфигурации](xref:fundamentals/configuration/index) системы, может прочитать секретные данные, хранимые с помощью средства диспетчера секрет, описанных в этой статье.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-109">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="c4ea1-110">Секрет диспетчера используется только при разработке.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-110">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="c4ea1-111">Можно защитить Azure рабочего и тестового секреты с [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-111">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="c4ea1-112">В разделе [конфигурации поставщика хранилища ключей Azure](xref:security/key-vault-configuration) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-112">See [Azure Key Vault configuration provider](xref:security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="c4ea1-113">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="c4ea1-113">Environment variables</span></span>

<span data-ttu-id="c4ea1-114">Чтобы избежать хранения секрета приложения в коде или в локальных файлов конфигурации, хранить секреты в переменных среды.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-114">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="c4ea1-115">Вы можете настроить [конфигурации](xref:fundamentals/configuration/index) framework для считывания значений из переменных среды, вызвав `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-115">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="c4ea1-116">Затем можно использовать переменные среды для переопределения значения конфигурации для всех источников ранее указанной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-116">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="c4ea1-117">Например, при создании нового веб-приложения ASP.NET Core для каждой учетной записи, оно добавляет строку подключения по умолчанию для *appsettings.json* файл в проекте с помощью ключа `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-117">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="c4ea1-118">Строка подключения по умолчанию настроена на использование LocalDB, которая запускается в пользовательском режиме и не требует пароля.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-118">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="c4ea1-119">При развертывании приложения на тестовом или рабочем сервере, можно переопределить `DefaultConnection` значение ключа с параметром переменной среды, содержащее строку подключения (возможно, с учетом учетные данные) для тестовой или рабочей базы данных сервер.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-119">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="c4ea1-120">Переменные среды обычно хранятся в виде обычного текста и не шифруются.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-120">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="c4ea1-121">В случае компрометации компьютера или процесс, переменные среды возможен ненадежных сторонами.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-121">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="c4ea1-122">По-прежнему могут требоваться дополнительные меры, чтобы предотвратить раскрытие секретные данные пользователей.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-122">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="c4ea1-123">Диспетчер секрет</span><span class="sxs-lookup"><span data-stu-id="c4ea1-123">Secret Manager</span></span>

<span data-ttu-id="c4ea1-124">Секрет диспетчера сохраняет конфиденциальные данные вне вашего дерева проектов для разработки.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-124">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="c4ea1-125">Секрет диспетчера — это средство проекта, который может использоваться для хранения конфиденциальных данных для проекта .NET Core во время разработки.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-125">The Secret Manager tool is a project tool that can be used to store secrets for a .NET Core project during development.</span></span> <span data-ttu-id="c4ea1-126">С помощью средства диспетчера секрет можно связать секрета приложения с конкретным проектом и использовать их совместно в нескольких проектах.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-126">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="c4ea1-127">Секрет диспетчера не шифрования секретов, хранимых и не должен рассматриваться как доверенного хранилища.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="c4ea1-128">Это только в целях разработки.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-128">It's for development purposes only.</span></span> <span data-ttu-id="c4ea1-129">Ключи и значения хранятся в файле конфигурации JSON в папке профиля пользователя.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="c4ea1-130">Установка средства диспетчера секрет</span><span class="sxs-lookup"><span data-stu-id="c4ea1-130">Installing the Secret Manager tool</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4ea1-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4ea1-131">Visual Studio</span></span>](#tab/visual-studio/)
<span data-ttu-id="c4ea1-132">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **изменить \<project_name\>.csproj** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="c4ea1-133">Добавьте в выделенной строке *.csproj* и сохранение файлов для восстановления, связанный с ним пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="c4ea1-134">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление секретами пользователя** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-134">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="c4ea1-135">Это жест добавляет новый `UserSecretsId` узла в `PropertyGroup` из *.csproj* файла, как видно из следующего примера:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-135">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="c4ea1-136">Сохранение измененного *.csproj* также файл откроется `secrets.json` файл в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-136">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="c4ea1-137">Замените содержимое `secrets.json` файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-137">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4ea1-138">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-138">Visual Studio Code</span></span>](#tab/visual-studio-code/)
<span data-ttu-id="c4ea1-139">Добавить `Microsoft.Extensions.SecretManager.Tools` для *.csproj* файл и запустите [восстановления dotnet](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="c4ea1-139">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span> <span data-ttu-id="c4ea1-140">Можно использовать те же действия для установки средства диспетчера секрета, используемого для командной строки.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-140">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="c4ea1-141">Тестирование диспетчера секрет, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-141">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="c4ea1-142">Диспетчер секрет выводится использования, параметры и справки.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-142">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="c4ea1-143">Должен быть в том же каталоге, что *.csproj* файла для запуска средства, предназначенные для *.csproj* файла `DotNetCliToolReference` узлов.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-143">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="c4ea1-144">Секрет диспетчера обрабатывает параметры конфигурации для конкретного проекта, которые хранятся в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-144">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="c4ea1-145">Чтобы использовать секретные данные пользователей, необходимо указать проект `UserSecretsId` значение в его *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-145">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="c4ea1-146">Значение `UserSecretsId` может быть произвольным, но уникален в проект.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-146">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="c4ea1-147">Разработчики обычно создать GUID для `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-147">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="c4ea1-148">Добавить `UserSecretsId` проекта в *.csproj* файла:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-148">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="c4ea1-149">Использование диспетчера секрет, чтобы задать секрет.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-149">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="c4ea1-150">Например в окно командной строки от каталога проекта, введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-150">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="c4ea1-151">Запустить диспетчер секрет из других каталогов, но необходимо использовать `--project` параметр для передачи в пути к *.csproj* файла:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-151">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="c4ea1-152">Также можно использовать средство диспетчера секрет для перечисления, удаления и очистите секреты приложения.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-152">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

* * *
## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="c4ea1-153">Доступ к секретной информации пользователя через конфигурацию</span><span class="sxs-lookup"><span data-stu-id="c4ea1-153">Accessing user secrets via configuration</span></span>

<span data-ttu-id="c4ea1-154">Секреты Manager секрет доступ через систему конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-154">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="c4ea1-155">Добавить `Microsoft.Extensions.Configuration.UserSecrets` упаковка и запуск [восстановления dotnet](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="c4ea1-155">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>

<span data-ttu-id="c4ea1-156">Добавить источник конфигурации секретные данные пользователя для `Startup` метод:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-156">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="c4ea1-157">Вы можете использовать секретные данные пользователей через API-Интерфейс настройки:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-157">You can access user secrets via the configuration API:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="c4ea1-158">Как работает диспетчер секрет</span><span class="sxs-lookup"><span data-stu-id="c4ea1-158">How the Secret Manager tool works</span></span>

<span data-ttu-id="c4ea1-159">Секрет диспетчера абстрагирует детали реализации, например, где и как значения сохраняются.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-159">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="c4ea1-160">Можно использовать средство, не зная эти детали реализации.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-160">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="c4ea1-161">В текущей версии значения хранятся в [JSON](http://json.org/) файл конфигурации в папке профиля пользователя:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-161">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="c4ea1-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="c4ea1-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="c4ea1-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="c4ea1-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="c4ea1-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="c4ea1-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="c4ea1-165">Значение `userSecretsId` берется из значения, указанного в *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-165">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="c4ea1-166">Не следует писать код, зависящий от расположение или формат данных, сохраненных с помощью диспетчера секрет средства, как эти сведения реализации может измениться.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-166">You shouldn't write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="c4ea1-167">Например, в настоящее время являются значения секрета *не* сегодня зашифрованы, но может быть когда-нибудь.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-167">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c4ea1-168">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c4ea1-168">Additional resources</span></span>

* [<span data-ttu-id="c4ea1-169">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="c4ea1-169">Configuration</span></span>](xref:fundamentals/configuration/index)
