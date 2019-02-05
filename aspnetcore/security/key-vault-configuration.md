---
title: Поставщик конфигурации хранилища ключей Azure в ASP.NET Core
author: guardrex
description: Узнайте, как использовать поставщик конфигурации Azure Key Vault для настройки приложения с помощью пары "имя значение", загружен во время выполнения.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 8e40c8308a692731e71fb8ebebfc64e606874290
ms.sourcegitcommit: 98e9c7187772d4ddefe6d8e85d0d206749dbd2ef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2019
ms.locfileid: "55737659"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="91d75-103">Поставщик конфигурации хранилища ключей Azure в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="91d75-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="91d75-104">По [Люк Лэтем](https://github.com/guardrex) и [Andrew Stanton медсестра](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="91d75-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="91d75-105">В этом документе объясняется, как использовать [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) поставщик конфигурации для загрузки значений конфигурации из секреты Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="91d75-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="91d75-106">Azure Key Vault — это облачная служба, который помогает защита криптографических ключей и секретов, используемых приложений и служб.</span><span class="sxs-lookup"><span data-stu-id="91d75-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="91d75-107">Распространенные сценарии использования хранилища ключей Azure с приложениями ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="91d75-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="91d75-108">Управление доступом к конфиденциальные данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="91d75-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="91d75-109">Соответствующие требования FIPS 140-2 уровня 2 проверить аппаратных модулей безопасности (HSM), при сохранении данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="91d75-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="91d75-110">Этот сценарий доступен для приложений, предназначенных для ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="91d75-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="91d75-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="91d75-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="91d75-112">Пакеты</span><span class="sxs-lookup"><span data-stu-id="91d75-112">Packages</span></span>

<span data-ttu-id="91d75-113">Чтобы использовать поставщик конфигурации Azure Key Vault, добавьте ссылки на пакет [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) пакета.</span><span class="sxs-lookup"><span data-stu-id="91d75-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="91d75-114">Чтобы внедрить сценарий управляемое удостоверение службы Azure, добавьте ссылки на пакет [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) пакета.</span><span class="sxs-lookup"><span data-stu-id="91d75-114">To adopt the Azure Managed Service Identity scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="91d75-115">На момент написания статьи, последний стабильный выпуск `Microsoft.Azure.Services.AppAuthentication`, версии `1.0.3`, обеспечивает поддержку для [назначенный системой управляемые удостоверения](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="91d75-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="91d75-116">Поддержка *назначаемого пользователем управляемого удостоверения* доступен в `1.0.2-preview` пакета.</span><span class="sxs-lookup"><span data-stu-id="91d75-116">Support for *user-assigned managed identities* is available in the `1.0.2-preview` package.</span></span> <span data-ttu-id="91d75-117">В этом разделе демонстрируется использование удостоверения, управляемые системой, и предоставленный пример приложения использует версию `1.0.3` из `Microsoft.Azure.Services.AppAuthentication` пакета.</span><span class="sxs-lookup"><span data-stu-id="91d75-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="91d75-118">Пример приложения</span><span class="sxs-lookup"><span data-stu-id="91d75-118">Sample app</span></span>

<span data-ttu-id="91d75-119">Пример приложения выполняется в одном из двух режимов определяется `#define` инструкция в верхней части *Program.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="91d75-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="91d75-120">`Basic` &ndash; Демонстрирует использование идентификатор приложения Azure Key Vault и пароль (секрет клиента) для доступа к секретные данные, хранящиеся в хранилище ключей.</span><span class="sxs-lookup"><span data-stu-id="91d75-120">`Basic` &ndash; Demonstrates the use of an Azure Key Vault Application ID and Password (Client Secret) to access secrets stored in the key vault.</span></span> <span data-ttu-id="91d75-121">Развертывание `Basic` примера на любом узле, может обслуживать приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="91d75-121">Deploy the `Basic` version of the sample to any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="91d75-122">`Managed` &ndash; Демонстрирует использование Azure [управляемого удостоверения службы (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) подлинность приложения в хранилище ключей Azure с аутентификацией Azure AD без учетных данных, хранящихся в коде или конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-122">`Managed` &ndash; Demonstrates how to use Azure's [Managed Service Identity (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="91d75-123">При использовании MSI для проверки подлинности, идентификатор приложения Azure AD и пароль (секрет клиента) не являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="91d75-123">When using MSI to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="91d75-124">`Managed` Версия образца должны быть развернуты в Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-124">The `Managed` version of the sample must be deployed to Azure.</span></span>

<span data-ttu-id="91d75-125">Дополнительные сведения о том, как настроить пример приложения с помощью директивы препроцессора (`#define`), см. в разделе <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="91d75-125">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="91d75-126">Хранилища секретов в среде разработки</span><span class="sxs-lookup"><span data-stu-id="91d75-126">Secret storage in the Development environment</span></span>

<span data-ttu-id="91d75-127">Задайте секретами локально с помощью [средство Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="91d75-127">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="91d75-128">При запуске примера приложения на локальном компьютере в среде разработки, секретные данные загружаются из локального хранилища менеджер секретов.</span><span class="sxs-lookup"><span data-stu-id="91d75-128">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="91d75-129">Средство Secret Manager требует `<UserSecretsId>` свойство в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-129">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="91d75-130">Задайте значение свойства (`{GUID}`) для любого уникального идентификатора GUID:</span><span class="sxs-lookup"><span data-stu-id="91d75-130">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="91d75-131">Секретные данные создаются в виде пар "имя значение".</span><span class="sxs-lookup"><span data-stu-id="91d75-131">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="91d75-132">Использование иерархических значений (разделы конфигурации) `:` (двоеточие) как разделитель [конфигурации ASP.NET Core](xref:fundamentals/configuration/index) имен разделов.</span><span class="sxs-lookup"><span data-stu-id="91d75-132">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="91d75-133">Менеджер секретов используется в командной оболочке, открыт корневой каталог содержимого проекта, где `{SECRET NAME}` — это имя и `{SECRET VALUE}` значение:</span><span class="sxs-lookup"><span data-stu-id="91d75-133">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="91d75-134">Выполните следующие команды в командной строке из содержимого корневого каталога проекта для задания секреты для примера приложения:</span><span class="sxs-lookup"><span data-stu-id="91d75-134">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="91d75-135">Когда эти секреты хранятся в хранилище ключей Azure в [хранилища секретов в рабочей среде с хранилищем ключей Azure](#secret-storage-in-the-production-environment-with-azure-key-vault) разделе `_dev` суффикс изменяется в `_prod`.</span><span class="sxs-lookup"><span data-stu-id="91d75-135">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="91d75-136">Суффикс предоставляет визуальную подсказку, в выходных данных приложения, указывающий источник значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="91d75-136">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="91d75-137">Хранилища секретов в рабочей среде с хранилищем ключей Azure</span><span class="sxs-lookup"><span data-stu-id="91d75-137">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="91d75-138">Инструкции, предоставленные [краткое руководство: Задание и получение секрета из хранилища ключей Azure с помощью Azure CLI](/azure/key-vault/quick-create-cli) разделе кратко перечислены ниже для создания хранилища ключей Azure и хранение секретов, используемых в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-138">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="91d75-139">См. в разделе для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="91d75-139">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="91d75-140">Откройте Azure Cloud shell, с помощью любого из следующих методов в [портала Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="91d75-140">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="91d75-141">Выберите **попробовать** в правом верхнем углу блока кода.</span><span class="sxs-lookup"><span data-stu-id="91d75-141">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="91d75-142">Используйте строку поиска «Интерфейс командной строки Azure» в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="91d75-142">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="91d75-143">Откройте Cloud Shell в браузере с **запуска Cloud Shell** кнопки.</span><span class="sxs-lookup"><span data-stu-id="91d75-143">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="91d75-144">Выберите **Cloud Shell** кнопку меню в правом верхнем углу портала Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-144">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="91d75-145">Дополнительные сведения см. в разделе [интерфейса командной строки Azure (CLI)](/cli/azure/) и [Обзор Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="91d75-145">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="91d75-146">Если вы еще не прошли аутентификацию, вход с помощью `az login` команды.</span><span class="sxs-lookup"><span data-stu-id="91d75-146">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="91d75-147">Создайте группу ресурсов, выполнив следующую команду, где `{RESOURCE GROUP NAME}` — имя группы ресурсов для новой группы ресурсов и `{LOCATION}` — регион Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="91d75-147">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="91d75-148">Создание хранилища ключей в группе ресурсов, выполнив следующую команду, где `{KEY VAULT NAME}` — это имя для нового хранилища ключей и `{LOCATION}` — регион Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="91d75-148">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="91d75-149">Создание секретов в хранилище ключей в виде пар "имя значение".</span><span class="sxs-lookup"><span data-stu-id="91d75-149">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="91d75-150">Azure имена секретов Key Vault ограничены буквы, цифры и дефисы.</span><span class="sxs-lookup"><span data-stu-id="91d75-150">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="91d75-151">Использование иерархических значений (разделы конфигурации) `--` (двух дефисов) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="91d75-151">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="91d75-152">Имена, которые обычно используются для разделения разделе из подраздела в [конфигурации ASP.NET Core](xref:fundamentals/configuration/index), не разрешены в именах секрета хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="91d75-152">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="91d75-153">Таким образом двух дефисов используются и поменять местами двоеточие, при загрузке секреты в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-153">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="91d75-154">Следующие секреты предназначены для использования с примером приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-154">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="91d75-155">Значения включают `_prod` суффикс, чтобы отличать их от `_dev` суффикс значения, загруженных в среде разработки с секретами пользователей.</span><span class="sxs-lookup"><span data-stu-id="91d75-155">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="91d75-156">Замените `{KEY VAULT NAME}` с именем хранилища ключей, созданный на предыдущем шаге:</span><span class="sxs-lookup"><span data-stu-id="91d75-156">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret"></a><span data-ttu-id="91d75-157">Используйте идентификатор приложения и секрет клиента</span><span class="sxs-lookup"><span data-stu-id="91d75-157">Use Application ID and Client Secret</span></span>

<span data-ttu-id="91d75-158">Настройка Azure AD, Azure Key Vault и приложение для использования идентификатор приложения и пароль (секрет клиента) для проверки подлинности в хранилище ключей, если приложение размещается за пределами Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-158">Configure Azure AD, Azure Key Vault, and the app to use an Application ID and Password (Client Secret) to authenticate to a key vault when the app is hosted outside of Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="91d75-159">Несмотря на то, что использование идентификатор приложения и пароль (секрет клиента) поддерживается для приложений, размещенных в Azure, мы рекомендуем использовать [управляемого удостоверения службы (MSI) поставщика](#use-the-managed-service-identity-msi-provider) при размещении приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-159">Although using an Application ID and Password (Client Secret) is supported for apps hosted in Azure, we recommend using the [Managed Service Identity (MSI) Provider](#use-the-managed-service-identity-msi-provider) when hosting an app in Azure.</span></span> <span data-ttu-id="91d75-160">MSI не требует хранения учетных данных в приложение или его конфигурацию, поэтому считается более безопасный подход.</span><span class="sxs-lookup"><span data-stu-id="91d75-160">MSI doesn't require storing credentials in the app or its configuration, so it's regarded as a generally safer approach.</span></span>

<span data-ttu-id="91d75-161">Пример приложения использует идентификатор приложения и пароль (секрет клиента) при `#define` инструкция в верхней части *Program.cs* для файла `Basic`.</span><span class="sxs-lookup"><span data-stu-id="91d75-161">The sample app uses an Application ID and Password (Client Secret) when the `#define` statement at the top of the *Program.cs* file is set to `Basic`.</span></span>

1. <span data-ttu-id="91d75-162">Регистрация приложения в Azure AD и установить пароль (секрет клиента) для удостоверения приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-162">Register the app with Azure AD and establish a Password (Client Secret) for the app's identity.</span></span>
1. <span data-ttu-id="91d75-163">Store имя хранилища ключей, идентификатор приложения и пароль и секрет клиента приложения *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="91d75-163">Store the key vault name, Application ID, and Password/Client Secret in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="91d75-164">Перейдите к **хранилища ключей** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-164">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="91d75-165">Выберите хранилище ключей, созданный в [хранилища секретов в рабочей среде с хранилищем ключей Azure](#secret-storage-in-the-production-environment-with-azure-key-vault) раздел.</span><span class="sxs-lookup"><span data-stu-id="91d75-165">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="91d75-166">Выберите **политики доступа**.</span><span class="sxs-lookup"><span data-stu-id="91d75-166">Select **Access policies**.</span></span>
1. <span data-ttu-id="91d75-167">Выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="91d75-167">Select **Add new**.</span></span>
1. <span data-ttu-id="91d75-168">Выберите **Выбор субъекта** и выберите зарегистрированного приложения по имени.</span><span class="sxs-lookup"><span data-stu-id="91d75-168">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="91d75-169">Выберите **выберите** кнопки.</span><span class="sxs-lookup"><span data-stu-id="91d75-169">Select the **Select** button.</span></span>
1. <span data-ttu-id="91d75-170">Откройте **разрешения секретов** и укажите приложение с **получить** и **списка** разрешения.</span><span class="sxs-lookup"><span data-stu-id="91d75-170">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="91d75-171">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="91d75-171">Select **OK**.</span></span>
1. <span data-ttu-id="91d75-172">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="91d75-172">Select **Save**.</span></span>
1. <span data-ttu-id="91d75-173">Разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="91d75-173">Deploy the app.</span></span>

<span data-ttu-id="91d75-174">`Basic` Пример приложения получает свои значения конфигурации из `IConfigurationRoot` с тем же именем, что имя секрета:</span><span class="sxs-lookup"><span data-stu-id="91d75-174">The `Basic` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="91d75-175">Не иерархическими значениями: Значение для `SecretName` получается с помощью `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="91d75-175">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="91d75-176">Иерархические значения (разделов): Используйте `:` нотации (двоеточие) или `GetSection` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="91d75-176">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="91d75-177">Чтобы получить значение конфигурации, следует используйте один из этих подходов:</span><span class="sxs-lookup"><span data-stu-id="91d75-177">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="91d75-178">Приложение вызывает `AddAzureKeyVault` с помощью значений, предоставленных *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="91d75-178">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=11-14)]

<span data-ttu-id="91d75-179">Примеры значений:</span><span class="sxs-lookup"><span data-stu-id="91d75-179">Example values:</span></span>

* <span data-ttu-id="91d75-180">Имя хранилища ключей: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="91d75-180">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="91d75-181">Идентификатор приложения: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="91d75-181">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="91d75-182">Пароль: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="91d75-182">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="91d75-183">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="91d75-183">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="91d75-184">При запуске приложения, веб-страница показывает загруженные значения секрета.</span><span class="sxs-lookup"><span data-stu-id="91d75-184">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="91d75-185">В среде разработки, загрузка значения секретов с помощью `_dev` суффикс.</span><span class="sxs-lookup"><span data-stu-id="91d75-185">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="91d75-186">В рабочей среде, значения загружать данные с помощью `_prod` суффикс.</span><span class="sxs-lookup"><span data-stu-id="91d75-186">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-the-managed-service-identity-msi-provider"></a><span data-ttu-id="91d75-187">Использование поставщика удостоверений (MSI) управляемой службы</span><span class="sxs-lookup"><span data-stu-id="91d75-187">Use the Managed Service Identity (MSI) Provider</span></span>

<span data-ttu-id="91d75-188">Приложение, развернутое в Azure можно использовать из управляемого удостоверения службы (MSI), что позволяет приложению для проверки подлинности с хранилищем ключей Azure с использованием проверки подлинности Azure AD без учетных данных (идентификатор приложения и пароль и секрет клиента), хранящихся в приложении.</span><span class="sxs-lookup"><span data-stu-id="91d75-188">An app deployed to Azure can take advantage of Managed Service Identity (MSI), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="91d75-189">Пример приложения использует MSI при `#define` инструкция в верхней части *Program.cs* для файла `Managed`.</span><span class="sxs-lookup"><span data-stu-id="91d75-189">The sample app uses MSI when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="91d75-190">Ввести имя хранилища в приложении *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="91d75-190">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="91d75-191">Пример приложения не требуется идентификатор приложения и пароль (секрет клиента), если значение `Managed` версии, поэтому вы можете игнорировать эти записи конфигурации.</span><span class="sxs-lookup"><span data-stu-id="91d75-191">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="91d75-192">Приложение развертывается в Azure и Azure проверяет подлинность приложения для доступа к хранилищу ключей Azure, используя только имя хранилища хранятся в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="91d75-192">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="91d75-193">Развертывание примера приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-193">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="91d75-194">Приложения в службе приложений Azure автоматически зарегистрированы в Azure AD, при создании службы.</span><span class="sxs-lookup"><span data-stu-id="91d75-194">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="91d75-195">Получите идентификатор объекта из развертывания для использования в следующей команде.</span><span class="sxs-lookup"><span data-stu-id="91d75-195">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="91d75-196">Идентификатор объекта отображается на портале Azure на **удостоверений** панель службы приложений.</span><span class="sxs-lookup"><span data-stu-id="91d75-196">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="91d75-197">С помощью интерфейса командной строки Azure и идентификатор объекта приложения, укажите приложение с `list` и `get` разрешения на доступ к хранилищу ключей:</span><span class="sxs-lookup"><span data-stu-id="91d75-197">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="91d75-198">**Перезапустите приложение** с помощью Azure CLI, PowerShell или портала Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-198">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="91d75-199">Пример приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-199">The sample app:</span></span>

* <span data-ttu-id="91d75-200">Создает экземпляр класса `AzureServiceTokenProvider` класс без строки подключения.</span><span class="sxs-lookup"><span data-stu-id="91d75-200">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="91d75-201">Когда строка подключения не указан, то поставщик пытается получить маркер доступа из MSI.</span><span class="sxs-lookup"><span data-stu-id="91d75-201">When a connection string isn't provided, the provider attempts to obtain an access token from MSI.</span></span>
* <span data-ttu-id="91d75-202">Новый `KeyVaultClient` создается с `AzureServiceTokenProvider` маркера экземпляр обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="91d75-202">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="91d75-203">`KeyVaultClient` С реализацией по умолчанию используется экземпляр `IKeyVaultSecretManager` , загружает все значения секретов и заменяет двойной тире (`--`) с запятой (`:`) в имена ключей.</span><span class="sxs-lookup"><span data-stu-id="91d75-203">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="91d75-204">При запуске приложения, веб-страница показывает загруженные значения секрета.</span><span class="sxs-lookup"><span data-stu-id="91d75-204">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="91d75-205">В среде разработки, имеют значения секретов `_dev` суффикс, так как они предоставляются с секретами пользователей.</span><span class="sxs-lookup"><span data-stu-id="91d75-205">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="91d75-206">В рабочей среде, значения загружать данные с помощью `_prod` суффикс, так как они предоставляются в хранилищем ключей Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-206">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="91d75-207">Если вы получили `Access denied` ошибку, убедитесь, что приложение будет зарегистрировано в Azure AD и получают доступ к хранилищу ключей.</span><span class="sxs-lookup"><span data-stu-id="91d75-207">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="91d75-208">Убедитесь, что после перезагрузки службы в Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-208">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="91d75-209">Используйте префикс имени ключа</span><span class="sxs-lookup"><span data-stu-id="91d75-209">Use a key name prefix</span></span>

<span data-ttu-id="91d75-210">`AddAzureKeyVault` предоставляет перегрузку, которая принимает реализацию `IKeyVaultSecretManager`, который позволяет управлять ключевых секреты хранилища преобразуются в ключи конфигурации.</span><span class="sxs-lookup"><span data-stu-id="91d75-210">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="91d75-211">Например можно реализовать интерфейс для загрузки значения секретов, исходя из значения префикса, указанные вами при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-211">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="91d75-212">Это позволяет, например, загружать секреты, в зависимости от версии приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-212">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="91d75-213">Для размещения секреты для нескольких приложений в том же хранилище ключей или для размещения среды секреты не используйте префиксы секретных кодов хранилища ключей (например, *разработки* и *рабочей* секретов) в одну хранилище.</span><span class="sxs-lookup"><span data-stu-id="91d75-213">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="91d75-214">Мы рекомендуем различных приложений и сред разработки, эксплуатации использовать отдельные хранилища ключей для изоляции среды приложений для обеспечения максимальной безопасности.</span><span class="sxs-lookup"><span data-stu-id="91d75-214">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="91d75-215">В следующем примере устанавливается секрет ключа хранилища (и используя средство Secret Manager для среды разработки) для `5000-AppSecret` (периоды не допускаются в именах секрета хранилища ключей).</span><span class="sxs-lookup"><span data-stu-id="91d75-215">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="91d75-216">Этот секрет представляет секрет приложения для версии 5.0.0.0 приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-216">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="91d75-217">Для другой версии приложения, 5.1.0.0, секрет добавляется к ключу хранилища (и используя средство Secret Manager) для `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="91d75-217">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="91d75-218">Каждая версия приложения загружает его с версиями секретное значение в его конфигурацию в качестве `AppSecret`, удаление версии при загрузке секрет.</span><span class="sxs-lookup"><span data-stu-id="91d75-218">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="91d75-219">`AddAzureKeyVault` вызывается с пользовательским `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="91d75-219">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="91d75-220">Предоставленные значения для имени хранилища ключей, идентификатор приложения и пароль (секрет клиента) *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="91d75-220">The values for key vault name, Application ID, and Password (Client Secret) are provided by the *appsettings.json* file:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="91d75-221">Примеры значений:</span><span class="sxs-lookup"><span data-stu-id="91d75-221">Example values:</span></span>

* <span data-ttu-id="91d75-222">Имя хранилища ключей: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="91d75-222">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="91d75-223">Идентификатор приложения: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="91d75-223">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="91d75-224">Пароль: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="91d75-224">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="91d75-225">`IKeyVaultSecretManager` Реализации реагирует на префиксы версии секретов, чтобы загрузить правильный секрет в конфигурации:</span><span class="sxs-lookup"><span data-stu-id="91d75-225">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="91d75-226">`Load` Метод вызывается с помощью алгоритма поставщик, выполняющий итерацию секреты хранилища, чтобы найти те, которые имеют префикс версии.</span><span class="sxs-lookup"><span data-stu-id="91d75-226">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="91d75-227">Найденный префикс версии с `Load`, алгоритм использует `GetKey` метод, чтобы вернуть имя конфигурации имя секрета.</span><span class="sxs-lookup"><span data-stu-id="91d75-227">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="91d75-228">Она отсекает версии префикс из имени секрета и возвращает остальные имя секрета для загрузки в конфигурацию приложения пары "имя значение".</span><span class="sxs-lookup"><span data-stu-id="91d75-228">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="91d75-229">Если при реализации этого подхода:</span><span class="sxs-lookup"><span data-stu-id="91d75-229">When this approach is implemented:</span></span>

1. <span data-ttu-id="91d75-230">Версия приложения, указанные в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-230">The app's version specified in the app's project file.</span></span> <span data-ttu-id="91d75-231">В следующем примере версия приложения имеет значение `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="91d75-231">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="91d75-232">Убедитесь, что `<UserSecretsId>` свойство присутствует в файле проекта приложения, где `{GUID}` представляет собой идентификатор GUID, предоставленный пользователем:</span><span class="sxs-lookup"><span data-stu-id="91d75-232">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="91d75-233">Сохранить следующие секреты локально с помощью [средство Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="91d75-233">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="91d75-234">Секретные данные сохраняются в хранилище ключей Azure с помощью следующих команд Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="91d75-234">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="91d75-235">При запуске приложения, загружаются секретных кодов хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="91d75-235">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="91d75-236">Строка секрета для `5000-AppSecret` сопоставляется версия приложения, указанные в файле проекта приложения (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="91d75-236">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="91d75-237">Версии, `5000` (с dash), удаляются из имени ключа.</span><span class="sxs-lookup"><span data-stu-id="91d75-237">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="91d75-238">В приложении, чтение конфигурации с ключом `AppSecret` загружает значение секрета.</span><span class="sxs-lookup"><span data-stu-id="91d75-238">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="91d75-239">Если версия приложения изменяется в файле проекта для `5.1.0.0` и снова запустите приложение, секрета возвращаемое значение `5.1.0.0_secret_value_dev` в среде разработки и `5.1.0.0_secret_value_prod` в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="91d75-239">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="91d75-240">Можно также предоставить собственную `KeyVaultClient` реализацию `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="91d75-240">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="91d75-241">Пользовательский клиент позволяет совместно использовать один экземпляр клиента приложения.</span><span class="sxs-lookup"><span data-stu-id="91d75-241">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="authenticate-to-azure-key-vault-with-an-x509-certificate"></a><span data-ttu-id="91d75-242">Проверку подлинности в Azure Key Vault с помощью сертификата X.509</span><span class="sxs-lookup"><span data-stu-id="91d75-242">Authenticate to Azure Key Vault with an X.509 certificate</span></span>

<span data-ttu-id="91d75-243">Разработка приложения .NET Framework в среде, которая поддерживает сертификаты, можно выполнить в хранилище ключей Azure с использованием сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="91d75-243">When developing a .NET Framework app in an environment that supports certificates, you can authenticate to Azure Key Vault with an X.509 certificate.</span></span> <span data-ttu-id="91d75-244">Закрытый ключ сертификата X.509 находится под управлением операционной системы.</span><span class="sxs-lookup"><span data-stu-id="91d75-244">The X.509 certificate's private key is managed by the OS.</span></span> <span data-ttu-id="91d75-245">Дополнительные сведения см. в разделе [проверка подлинности с помощью сертификата вместо секрета клиента](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span><span class="sxs-lookup"><span data-stu-id="91d75-245">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span> <span data-ttu-id="91d75-246">Используйте `AddAzureKeyVault` перегрузку, которая принимает `X509Certificate2` (`_env` в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="91d75-246">Use the `AddAzureKeyVault` overload that accepts an `X509Certificate2` (`_env` in the following example :</span></span>

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="91d75-247">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="91d75-247">Bind an array to a class</span></span>

<span data-ttu-id="91d75-248">Поставщик способный считывать значения конфигурации в массив для привязки к массиву POCO.</span><span class="sxs-lookup"><span data-stu-id="91d75-248">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="91d75-249">При чтении из источника конфигурации, который позволяет ключи содержат двоеточия (`:`) разделители, числового ключа сегмента используется для различения ключи, составляющие массив (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="91d75-249">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="91d75-250">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="91d75-250">`:{n}:`).</span></span> <span data-ttu-id="91d75-251">Дополнительные сведения см. в разделе [конфигурации: Привязка к классу массив](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="91d75-251">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="91d75-252">Azure Key Vault ключи нельзя использовать двоеточие в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="91d75-252">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="91d75-253">Подход, описанный в этом разделе используются двойные тире (`--`) как разделитель для значений иерархической (разделов).</span><span class="sxs-lookup"><span data-stu-id="91d75-253">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="91d75-254">Массив ключи хранятся в хранилище ключей Azure с double штрихов и числовых ключей сегментов (`--0--`, `--1--`,...</span><span class="sxs-lookup"><span data-stu-id="91d75-254">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, …</span></span> <span data-ttu-id="91d75-255">`--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="91d75-255">`--{n}--`).</span></span>

<span data-ttu-id="91d75-256">Рассмотрим следующую [Serilog](https://serilog.net/) конфигурация поставщика, предоставляемые JSON-файл журнала.</span><span class="sxs-lookup"><span data-stu-id="91d75-256">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="91d75-257">Существуют два объектных литералов, определенные в `WriteTo` массива, который отражает две Serilog *приемников*, описывающих назначения для выходных данных ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="91d75-257">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

<span data-ttu-id="91d75-258">Конфигурации, представленной выше JSON-файл хранится в хранилище ключей Azure с помощью двойной штрих (`--`) нотации и числовые сегменты:</span><span class="sxs-lookup"><span data-stu-id="91d75-258">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="91d75-259">Ключ</span><span class="sxs-lookup"><span data-stu-id="91d75-259">Key</span></span> | <span data-ttu-id="91d75-260">Значение</span><span class="sxs-lookup"><span data-stu-id="91d75-260">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="91d75-261">Перезагрузить секретов</span><span class="sxs-lookup"><span data-stu-id="91d75-261">Reload secrets</span></span>

<span data-ttu-id="91d75-262">Секреты, кэшируются до `IConfigurationRoot.Reload()` вызывается.</span><span class="sxs-lookup"><span data-stu-id="91d75-262">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="91d75-263">Истек, отключена, и обновленные секреты в хранилище ключей не учитываются в приложении до `Reload` выполняется.</span><span class="sxs-lookup"><span data-stu-id="91d75-263">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="91d75-264">Отключенные и истекшим сроком действия секретов</span><span class="sxs-lookup"><span data-stu-id="91d75-264">Disabled and expired secrets</span></span>

<span data-ttu-id="91d75-265">Отключенные и просроченные секреты throw `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="91d75-265">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="91d75-266">Чтобы избежать возникновения в приложении, замените приложение или изменить секрет просроченного отключено.</span><span class="sxs-lookup"><span data-stu-id="91d75-266">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="91d75-267">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="91d75-267">Troubleshoot</span></span>

<span data-ttu-id="91d75-268">Если приложение не удается загрузить конфигурацию с помощью поставщика, сообщение об ошибке записывается [инфраструктуры ведения журнала ASP.NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="91d75-268">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="91d75-269">Следующие условия не позволит конфигурации загрузки:</span><span class="sxs-lookup"><span data-stu-id="91d75-269">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="91d75-270">Приложение не настроено правильно, в Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="91d75-270">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="91d75-271">Хранилище ключей не существует в хранилище ключей Azure.</span><span class="sxs-lookup"><span data-stu-id="91d75-271">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="91d75-272">Приложение не имеет разрешения на доступ к хранилищу ключей.</span><span class="sxs-lookup"><span data-stu-id="91d75-272">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="91d75-273">Политика доступа не включает `Get` и `List` разрешения.</span><span class="sxs-lookup"><span data-stu-id="91d75-273">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="91d75-274">В хранилище ключей данные конфигурации (пара «имя значение») ошибочно названы, отсутствуют, отключена или истек срок действия.</span><span class="sxs-lookup"><span data-stu-id="91d75-274">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="91d75-275">Приложение имеет имя неправильный хранилища ключей (`Vault`), идентификатор приложения Azure AD (`ClientId`), или ключ Azure AD (`ClientSecret`).</span><span class="sxs-lookup"><span data-stu-id="91d75-275">The app has the wrong key vault name (`Vault`), Azure AD App Id (`ClientId`), or Azure AD Key (`ClientSecret`).</span></span>
* <span data-ttu-id="91d75-276">Ключ Azure AD (`ClientSecret`) срок действия истек.</span><span class="sxs-lookup"><span data-stu-id="91d75-276">The Azure AD Key (`ClientSecret`) is expired.</span></span>
* <span data-ttu-id="91d75-277">В приложении для значения, которое вы пытаетесь загрузить неправильный ключ конфигурации (имя).</span><span class="sxs-lookup"><span data-stu-id="91d75-277">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="91d75-278">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="91d75-278">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="91d75-279">Microsoft Azure: Хранилище ключей</span><span class="sxs-lookup"><span data-stu-id="91d75-279">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="91d75-280">Microsoft Azure: Документация по хранилищу ключей</span><span class="sxs-lookup"><span data-stu-id="91d75-280">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="91d75-281">Использование защищенных аппаратным модулем безопасности и их передача ключей для хранилища ключей Azure</span><span class="sxs-lookup"><span data-stu-id="91d75-281">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="91d75-282">Класс KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="91d75-282">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
