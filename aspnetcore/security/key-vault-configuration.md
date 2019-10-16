---
title: Поставщик конфигурации Azure Key Vault в ASP.NET Core
author: guardrex
description: Узнайте, как использовать поставщик конфигурации Azure Key Vault для настройки приложения с помощью пар "имя-значение", загружаемых во время выполнения.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: security/key-vault-configuration
ms.openlocfilehash: c8e76068dbcf2a59a15fa75a1fc5aa0032e6acc5
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334202"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="78f1e-103">Поставщик конфигурации Azure Key Vault в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78f1e-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="78f1e-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Эндрю Стантон-медперсонала](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="78f1e-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="78f1e-105">В этом документе объясняется, как использовать поставщик конфигурации [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) для загрузки значений конфигурации приложения из Azure Key Vault секреты.</span><span class="sxs-lookup"><span data-stu-id="78f1e-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="78f1e-106">Azure Key Vault — это облачная служба, которая помогает защитить криптографические ключи и секреты, используемые приложениями и службами.</span><span class="sxs-lookup"><span data-stu-id="78f1e-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="78f1e-107">Распространенные сценарии использования Azure Key Vault с приложениями ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="78f1e-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="78f1e-108">Управление доступом к конфиденциальным данным конфигурации.</span><span class="sxs-lookup"><span data-stu-id="78f1e-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="78f1e-109">Соблюдайте требования для FIPS 140-2 уровня 2 проверенных аппаратных модулей безопасности (HSM) при хранении данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="78f1e-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="78f1e-110">Этот сценарий доступен для приложений, предназначенных для ASP.NET Core 2,1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="78f1e-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="78f1e-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78f1e-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="78f1e-112">пакеты,</span><span class="sxs-lookup"><span data-stu-id="78f1e-112">Packages</span></span>

<span data-ttu-id="78f1e-113">Чтобы использовать поставщик конфигурации Azure Key Vault, добавьте ссылку на пакет в пакет [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .</span><span class="sxs-lookup"><span data-stu-id="78f1e-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="78f1e-114">Чтобы использовать [управляемые удостоверения для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview) , добавьте ссылку на пакет [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) .</span><span class="sxs-lookup"><span data-stu-id="78f1e-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="78f1e-115">На момент написания статьи последний стабильный выпуск `Microsoft.Azure.Services.AppAuthentication`, версия `1.0.3`, обеспечивает поддержку [управляемых системой удостоверений](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span><span class="sxs-lookup"><span data-stu-id="78f1e-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span></span> <span data-ttu-id="78f1e-116">Поддержка *назначенных пользователю управляемых удостоверений* доступна в пакете `1.2.0-preview2`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="78f1e-117">В этом разделе демонстрируется использование управляемых системой удостоверений, а в предоставленном примере приложения используется версия `1.0.3` пакета `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="78f1e-118">Пример приложения</span><span class="sxs-lookup"><span data-stu-id="78f1e-118">Sample app</span></span>

<span data-ttu-id="78f1e-119">Пример приложения выполняется в одном из двух режимов, определяемых инструкцией `#define` в верхней части файла *Program.CS* :</span><span class="sxs-lookup"><span data-stu-id="78f1e-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="78f1e-120">`Certificate` &ndash; демонстрирует использование идентификатора клиента Azure Key Vault и сертификата X. 509 для доступа к секретам, хранящимся в Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="78f1e-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="78f1e-121">Эту версию примера можно запустить из любого расположения, развернуть в службе приложений Azure или на любом узле, способном обслуживать ASP.NET Coreное приложение.</span><span class="sxs-lookup"><span data-stu-id="78f1e-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="78f1e-122">`Managed` &ndash; демонстрирует использование [управляемых удостоверений для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview) для проверки подлинности приложения при Azure Key Vault с проверкой подлинности Azure AD без учетных данных, хранящихся в коде или конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="78f1e-123">При использовании управляемых удостоверений для проверки подлинности не требуется идентификатор и пароль приложения Azure AD (секрет клиента).</span><span class="sxs-lookup"><span data-stu-id="78f1e-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="78f1e-124">Версия `Managed` примера должна быть развернута в Azure.</span><span class="sxs-lookup"><span data-stu-id="78f1e-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="78f1e-125">Следуйте указаниям в разделе [Использование управляемых удостоверений для ресурсов Azure](#use-managed-identities-for-azure-resources) .</span><span class="sxs-lookup"><span data-stu-id="78f1e-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="78f1e-126">Дополнительные сведения о настройке примера приложения с помощью директив препроцессора (`#define`) см. в разделе <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="78f1e-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="78f1e-127">Секретное хранилище в среде разработки</span><span class="sxs-lookup"><span data-stu-id="78f1e-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="78f1e-128">Локальное задание секретов с помощью [средства диспетчера секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="78f1e-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="78f1e-129">Когда пример приложения выполняется на локальном компьютере в среде разработки, секреты загружаются из локального хранилища диспетчера секретов.</span><span class="sxs-lookup"><span data-stu-id="78f1e-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="78f1e-130">Для средства диспетчера секретов требуется свойство `<UserSecretsId>` в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="78f1e-131">Задайте для свойства значение (`{GUID}`) любой уникальный GUID:</span><span class="sxs-lookup"><span data-stu-id="78f1e-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="78f1e-132">Секреты создаются как пары "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="78f1e-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="78f1e-133">Иерархические значения (разделы конфигурации) используют `:` (двоеточие) в качестве разделителя в именах [ASP.NET Core ключей конфигурации](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="78f1e-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="78f1e-134">Диспетчер секретов используется из командной оболочки, открытой в [корне содержимого](xref:fundamentals/index#content-root)проекта, где `{SECRET NAME}` — это имя, а `{SECRET VALUE}` — это значение:</span><span class="sxs-lookup"><span data-stu-id="78f1e-134">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="78f1e-135">Выполните следующие команды в командной оболочке из [корневого каталога содержимого](xref:fundamentals/index#content-root) проекта, чтобы задать секреты для примера приложения:</span><span class="sxs-lookup"><span data-stu-id="78f1e-135">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="78f1e-136">Если эти секреты хранятся в Azure Key Vault в [хранилище секретов в рабочей среде с Azure Key Vaultным](#secret-storage-in-the-production-environment-with-azure-key-vault) разделом, суффикс `_dev` меняется на `_prod`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="78f1e-137">Суффикс предоставляет визуальную подсказку в выходных данных приложения, указывающую источник значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="78f1e-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="78f1e-138">Секретное хранилище в рабочей среде с Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="78f1e-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="78f1e-139">Инструкции, приведенные в [кратком руководстве по установке и извлечению секретов из Azure Key Vault с помощью Azure CLI](/azure/key-vault/quick-create-cli) разделе, приведены здесь для создания Azure Key Vault и хранения секретов, используемых в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="78f1e-140">Дополнительные сведения см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="78f1e-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="78f1e-141">Откройте Azure Cloud Shell одним из следующих способов в [портал Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="78f1e-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="78f1e-142">Выберите **проверить** в правом верхнем углу блока кода.</span><span class="sxs-lookup"><span data-stu-id="78f1e-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="78f1e-143">Используйте строку поиска "Azure CLI" в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="78f1e-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="78f1e-144">Откройте Cloud Shell в браузере с помощью кнопки **запустить Cloud Shell** .</span><span class="sxs-lookup"><span data-stu-id="78f1e-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="78f1e-145">Нажмите кнопку **Cloud Shell** в меню в правом верхнем углу портал Azure.</span><span class="sxs-lookup"><span data-stu-id="78f1e-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="78f1e-146">Дополнительные сведения см. в статьях [интерфейс командной строки Azure (CLI)](/cli/azure/) и [общие сведения о Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="78f1e-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="78f1e-147">Если вы еще не прошли проверку подлинности, выполните вход с помощью команды `az login`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="78f1e-148">Создайте группу ресурсов с помощью следующей команды, где `{RESOURCE GROUP NAME}` — это имя группы ресурсов для новой группы ресурсов, а `{LOCATION}` — регион Azure (центр обработки данных):</span><span class="sxs-lookup"><span data-stu-id="78f1e-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="78f1e-149">Создайте хранилище ключей в группе ресурсов с помощью следующей команды, где `{KEY VAULT NAME}` — это имя нового хранилища ключей, а `{LOCATION}` — регион Azure (центр обработки данных):</span><span class="sxs-lookup"><span data-stu-id="78f1e-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="78f1e-150">Создайте секреты в хранилище ключей в виде пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="78f1e-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="78f1e-151">Azure Key Vault имена секретов ограничены буквенно-цифровыми символами и тире.</span><span class="sxs-lookup"><span data-stu-id="78f1e-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="78f1e-152">Иерархические значения (разделы конфигурации) используют в качестве разделителя `--` (два тире).</span><span class="sxs-lookup"><span data-stu-id="78f1e-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="78f1e-153">Двоеточия, которые обычно используются для разделения раздела из подраздела в [ASP.NET Core конфигурации](xref:fundamentals/configuration/index), не допускаются в именах секретов хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="78f1e-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="78f1e-154">Поэтому при загрузке секретов в конфигурацию приложения используются два дефиса и они переключаются на двоеточие.</span><span class="sxs-lookup"><span data-stu-id="78f1e-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="78f1e-155">Следующие секреты предназначены для использования с примером приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="78f1e-156">Значения включают суффикс `_prod`, чтобы отличить их от значений суффикса `_dev`, загруженных в среде разработки из секретов пользователя.</span><span class="sxs-lookup"><span data-stu-id="78f1e-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="78f1e-157">Замените `{KEY VAULT NAME}` именем хранилища ключей, созданным на предыдущем шаге:</span><span class="sxs-lookup"><span data-stu-id="78f1e-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="78f1e-158">Использование идентификатора приложения и сертификата X. 509 для приложений, не размещенных в Azure</span><span class="sxs-lookup"><span data-stu-id="78f1e-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="78f1e-159">Настройте Azure AD, Azure Key Vault и приложение для использования идентификатора Azure Active Directory приложения и сертификата X. 509 для проверки подлинности в хранилище ключей, **Если приложение размещено за пределами Azure**.</span><span class="sxs-lookup"><span data-stu-id="78f1e-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="78f1e-160">Дополнительные сведения см. в статье [о ключах, секретах и сертификатах](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="78f1e-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="78f1e-161">Хотя использование идентификатора приложения и сертификата X. 509 поддерживается для приложений, размещенных в Azure, мы рекомендуем использовать [управляемые удостоверения для ресурсов Azure](#use-managed-identities-for-azure-resources) при размещении приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="78f1e-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="78f1e-162">Для управляемых удостоверений не требуется хранить сертификат в приложении или в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="78f1e-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="78f1e-163">Пример приложения использует идентификатор приложения и сертификат X. 509, если инструкция `#define` в верхней части файла *Program.CS* имеет значение `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="78f1e-164">Создайте архивный сертификат PKCS # 12 (*PFX*-файл).</span><span class="sxs-lookup"><span data-stu-id="78f1e-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="78f1e-165">Для создания сертификатов можно использовать [MakeCert в Windows](/windows/desktop/seccrypto/makecert) и [OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="78f1e-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="78f1e-166">Установите сертификат в личное хранилище сертификатов текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="78f1e-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="78f1e-167">Пометка ключа как экспортируемого необязательна.</span><span class="sxs-lookup"><span data-stu-id="78f1e-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="78f1e-168">Запишите отпечаток сертификата, который будет использоваться далее в этом процессе.</span><span class="sxs-lookup"><span data-stu-id="78f1e-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="78f1e-169">Экспортируйте архивный сертификат PKCS # 12 (*PFX*) как сертификат в формате DER (*CER*).</span><span class="sxs-lookup"><span data-stu-id="78f1e-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="78f1e-170">Зарегистрируйте приложение в Azure AD (**Регистрация приложений**).</span><span class="sxs-lookup"><span data-stu-id="78f1e-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="78f1e-171">Отправьте сертификат в кодировке DER (*CER*) в Azure AD:</span><span class="sxs-lookup"><span data-stu-id="78f1e-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="78f1e-172">Выберите приложение в Azure AD.</span><span class="sxs-lookup"><span data-stu-id="78f1e-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="78f1e-173">Перейдите к разделу **сертификаты & секреты**.</span><span class="sxs-lookup"><span data-stu-id="78f1e-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="78f1e-174">Выберите **отправить сертификат** , чтобы отправить сертификат, который содержит открытый ключ.</span><span class="sxs-lookup"><span data-stu-id="78f1e-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="78f1e-175">Сертификат *. cer*, *. pem*или *. CRT* является приемлемым.</span><span class="sxs-lookup"><span data-stu-id="78f1e-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="78f1e-176">Сохраните имя хранилища ключей, идентификатор приложения и отпечаток сертификата в файле *appSettings. JSON* приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="78f1e-177">Перейдите к **разделу хранилища ключей** в портал Azure.</span><span class="sxs-lookup"><span data-stu-id="78f1e-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="78f1e-178">Выберите хранилище ключей, созданное в [хранилище секретов в рабочей среде с помощью Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) разделе.</span><span class="sxs-lookup"><span data-stu-id="78f1e-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="78f1e-179">Выберите **политики доступа**.</span><span class="sxs-lookup"><span data-stu-id="78f1e-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="78f1e-180">Выберите **Добавить новый**.</span><span class="sxs-lookup"><span data-stu-id="78f1e-180">Select **Add new**.</span></span>
1. <span data-ttu-id="78f1e-181">Щелкните **выбрать субъект** и выберите зарегистрированное приложение по имени.</span><span class="sxs-lookup"><span data-stu-id="78f1e-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="78f1e-182">Нажмите кнопку **выбрать** .</span><span class="sxs-lookup"><span data-stu-id="78f1e-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="78f1e-183">Откройте **разрешения для секрета** и предоставьте приложению разрешения **Get** и **List** .</span><span class="sxs-lookup"><span data-stu-id="78f1e-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="78f1e-184">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="78f1e-184">Select **OK**.</span></span>
1. <span data-ttu-id="78f1e-185">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="78f1e-185">Select **Save**.</span></span>
1. <span data-ttu-id="78f1e-186">Разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="78f1e-186">Deploy the app.</span></span>

<span data-ttu-id="78f1e-187">Пример приложения `Certificate` получает значения конфигурации из `IConfigurationRoot` с тем же именем, что и имя секрета:</span><span class="sxs-lookup"><span data-stu-id="78f1e-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="78f1e-188">Неиерархические значения: значение для `SecretName` получено с `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="78f1e-189">Иерархические значения (разделы): используйте нотацию `:` (двоеточие) или метод расширения `GetSection`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="78f1e-190">Используйте любой из этих подходов для получения значения конфигурации:</span><span class="sxs-lookup"><span data-stu-id="78f1e-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="78f1e-191">Сертификат X. 509 управляется ОС.</span><span class="sxs-lookup"><span data-stu-id="78f1e-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="78f1e-192">Приложение вызывает `AddAzureKeyVault` со значениями, предоставленными файлом *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="78f1e-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="78f1e-193">Примеры значений:</span><span class="sxs-lookup"><span data-stu-id="78f1e-193">Example values:</span></span>

* <span data-ttu-id="78f1e-194">Имя хранилища ключей: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="78f1e-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="78f1e-195">Идентификатор приложения: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="78f1e-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="78f1e-196">Отпечаток сертификата: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="78f1e-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="78f1e-197">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="78f1e-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="78f1e-198">При запуске приложения на веб-странице отображаются загруженные значения секрета.</span><span class="sxs-lookup"><span data-stu-id="78f1e-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="78f1e-199">В среде разработки значения секрета загружаются с суффиксом `_dev`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="78f1e-200">В рабочей среде значения загружаются с суффиксом `_prod`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="78f1e-201">Использование управляемых удостоверений для ресурсов Azure</span><span class="sxs-lookup"><span data-stu-id="78f1e-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="78f1e-202">**Приложение, развернутое в Azure** , может использовать преимущества [управляемых удостоверений для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview), что позволяет приложению проходить проверку подлинности в Azure Key Vault с помощью проверки подлинности Azure AD без учетных данных (идентификатор приложения и пароль/секрет клиента). хранится в приложении.</span><span class="sxs-lookup"><span data-stu-id="78f1e-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="78f1e-203">Пример приложения использует управляемые удостоверения для ресурсов Azure, если инструкция `#define` в верхней части файла *Program.CS* имеет значение `Managed`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="78f1e-204">Введите имя хранилища в файл *appSettings. JSON* приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="78f1e-205">Для примера приложения не требуется идентификатор приложения и пароль (секрет клиента), если задана версия `Managed`, поэтому эти записи конфигурации можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="78f1e-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="78f1e-206">Приложение развертывается в Azure, а Azure выполняет проверку подлинности приложения для доступа к Azure Key Vault только с использованием имени хранилища, хранящегося в файле *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="78f1e-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="78f1e-207">Разверните пример приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="78f1e-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="78f1e-208">Приложение, развернутое в службе приложений Azure, автоматически регистрируется в Azure AD при создании службы.</span><span class="sxs-lookup"><span data-stu-id="78f1e-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="78f1e-209">Получите идентификатор объекта из развертывания для использования в следующей команде.</span><span class="sxs-lookup"><span data-stu-id="78f1e-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="78f1e-210">Идентификатор объекта отображается в портал Azure на панели **удостоверений** службы приложений.</span><span class="sxs-lookup"><span data-stu-id="78f1e-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="78f1e-211">Используя Azure CLI и идентификатор объекта приложения, укажите приложение с разрешениями `list` и `get` для доступа к хранилищу ключей:</span><span class="sxs-lookup"><span data-stu-id="78f1e-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="78f1e-212">**Перезапустите приложение** с помощью Azure CLI, PowerShell или портал Azure.</span><span class="sxs-lookup"><span data-stu-id="78f1e-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="78f1e-213">Пример приложения:</span><span class="sxs-lookup"><span data-stu-id="78f1e-213">The sample app:</span></span>

* <span data-ttu-id="78f1e-214">Создает экземпляр класса `AzureServiceTokenProvider` без строки подключения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="78f1e-215">Если строка подключения не указана, поставщик пытается получить маркер доступа из управляемых удостоверений для ресурсов Azure.</span><span class="sxs-lookup"><span data-stu-id="78f1e-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="78f1e-216">Новый `KeyVaultClient` создается с помощью обратного вызова маркера экземпляра `AzureServiceTokenProvider`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="78f1e-217">Экземпляр `KeyVaultClient` используется с реализацией по умолчанию `IKeyVaultSecretManager`, которая загружает все значения секрета и заменяет двойные тире (`--`) двоеточием (`:`) в именах ключей.</span><span class="sxs-lookup"><span data-stu-id="78f1e-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="78f1e-218">Пример значения имени хранилища ключей: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="78f1e-218">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="78f1e-219">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="78f1e-219">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="78f1e-220">При запуске приложения на веб-странице отображаются загруженные значения секрета.</span><span class="sxs-lookup"><span data-stu-id="78f1e-220">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="78f1e-221">В среде разработки значения секрета имеют суффикс `_dev`, так как они предоставляются секретами пользователя.</span><span class="sxs-lookup"><span data-stu-id="78f1e-221">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="78f1e-222">В рабочей среде значения загружаются с суффиксом `_prod`, так как они предоставляются Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="78f1e-222">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="78f1e-223">Если вы получаете ошибку `Access denied`, убедитесь, что приложение зарегистрировано в Azure AD и предоставил доступ к хранилищу ключей.</span><span class="sxs-lookup"><span data-stu-id="78f1e-223">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="78f1e-224">Убедитесь, что вы перезапустите службу в Azure.</span><span class="sxs-lookup"><span data-stu-id="78f1e-224">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="78f1e-225">Использование префикса имени ключа</span><span class="sxs-lookup"><span data-stu-id="78f1e-225">Use a key name prefix</span></span>

<span data-ttu-id="78f1e-226">`AddAzureKeyVault` предоставляет перегрузку, которая принимает реализацию `IKeyVaultSecretManager`, что позволяет управлять преобразованием секретов хранилища ключей в конфигурационные ключи.</span><span class="sxs-lookup"><span data-stu-id="78f1e-226">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="78f1e-227">Например, можно реализовать интерфейс для загрузки секретных значений на основе значения префикса, которое вы задают при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-227">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="78f1e-228">Это позволяет, например, загружать секреты на основе версии приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-228">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="78f1e-229">Не используйте префиксы в секретах хранилища ключей, чтобы размещать секреты для нескольких приложений в одном хранилище ключей или для размещения секретов среды (например, для *разработки* и *рабочих* секретов) в одном хранилище.</span><span class="sxs-lookup"><span data-stu-id="78f1e-229">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="78f1e-230">Мы рекомендуем использовать отдельные хранилища ключей для различных приложений и сред разработки и рабочей среды, чтобы изолировать среды приложений для обеспечения наивысшего уровня безопасности.</span><span class="sxs-lookup"><span data-stu-id="78f1e-230">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="78f1e-231">В следующем примере в хранилище ключей устанавливается секрет (и используется средство диспетчера секретов для среды разработки) для `5000-AppSecret` (точки не допускаются в именах секретов хранилища ключей).</span><span class="sxs-lookup"><span data-stu-id="78f1e-231">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="78f1e-232">Этот секрет представляет секрет приложения для версии 5.0.0.0 приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-232">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="78f1e-233">Для другой версии приложения 5.1.0.0 секрет добавляется в хранилище ключей (и с помощью средства диспетчера секретов) для `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-233">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="78f1e-234">Каждая версия приложения загружает значение секрета в своей конфигурации как `AppSecret`, отключая версию при загрузке секрета.</span><span class="sxs-lookup"><span data-stu-id="78f1e-234">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="78f1e-235">`AddAzureKeyVault` вызывается с пользовательским `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="78f1e-235">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="78f1e-236">Реализация `IKeyVaultSecretManager` реагирует на префиксы версий секретов, чтобы загрузить правильный секрет в конфигурацию:</span><span class="sxs-lookup"><span data-stu-id="78f1e-236">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="78f1e-237">Метод `Load` вызывается алгоритмом поставщика, который выполняет перебор секретов хранилища для поиска тех, у которых есть префикс версии.</span><span class="sxs-lookup"><span data-stu-id="78f1e-237">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="78f1e-238">При обнаружении префикса версии с `Load` алгоритм использует метод `GetKey`, чтобы вернуть имя конфигурации для имени секрета.</span><span class="sxs-lookup"><span data-stu-id="78f1e-238">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="78f1e-239">Он отделяет префикс версии от имени секрета и возвращает оставшуюся часть имени секрета для загрузки в пары "имя-значение" конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-239">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="78f1e-240">При реализации этого подхода:</span><span class="sxs-lookup"><span data-stu-id="78f1e-240">When this approach is implemented:</span></span>

1. <span data-ttu-id="78f1e-241">Версия приложения, указанная в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-241">The app's version specified in the app's project file.</span></span> <span data-ttu-id="78f1e-242">В следующем примере для версии приложения задано значение `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="78f1e-242">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="78f1e-243">Убедитесь, что в файле проекта приложения имеется свойство `<UserSecretsId>`, где `{GUID}` является идентификатором GUID, предоставляемым пользователем:</span><span class="sxs-lookup"><span data-stu-id="78f1e-243">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="78f1e-244">Сохраните следующие секреты локально с помощью [средства диспетчера секретов](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="78f1e-244">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="78f1e-245">Секреты сохраняются в Azure Key Vault с помощью следующих Azure CLI команд:</span><span class="sxs-lookup"><span data-stu-id="78f1e-245">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="78f1e-246">При запуске приложения загружаются секреты хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="78f1e-246">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="78f1e-247">Строковый секрет для `5000-AppSecret` соответствует версии приложения, указанной в файле проекта приложения (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="78f1e-247">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="78f1e-248">Версия, `5000` (с тире), удаляется из имени ключа.</span><span class="sxs-lookup"><span data-stu-id="78f1e-248">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="78f1e-249">Во всем приложении чтение конфигурации с ключом `AppSecret` загружает секретное значение.</span><span class="sxs-lookup"><span data-stu-id="78f1e-249">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="78f1e-250">Если версия приложения изменена в файле проекта на `5.1.0.0`, а приложение запускается снова, возвращается значение секрета `5.1.0.0_secret_value_dev` в среде разработки и `5.1.0.0_secret_value_prod` в производстве.</span><span class="sxs-lookup"><span data-stu-id="78f1e-250">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="78f1e-251">Можно также предоставить собственную реализацию `KeyVaultClient` для `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-251">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="78f1e-252">Настраиваемый клиент позволяет совместно использовать один экземпляр клиента в приложении.</span><span class="sxs-lookup"><span data-stu-id="78f1e-252">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="78f1e-253">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="78f1e-253">Bind an array to a class</span></span>

<span data-ttu-id="78f1e-254">Поставщик может считывать значения конфигурации в массив для привязки к массиву POCO.</span><span class="sxs-lookup"><span data-stu-id="78f1e-254">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="78f1e-255">При чтении из источника конфигурации, в котором ключи содержат разделители с двоеточием (`:`), используется числовой сегмент, который позволяет отличать ключи, составляющие массив (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="78f1e-255">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="78f1e-256">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="78f1e-256">`:{n}:`).</span></span> <span data-ttu-id="78f1e-257">Дополнительные сведения см. в разделе [Configuration: привязка массива к классу](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="78f1e-257">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="78f1e-258">Ключи Azure Key Vault не могут использовать двоеточие в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="78f1e-258">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="78f1e-259">Описанный в этом разделе подход использует двойные тире (`--`) в качестве разделителя для иерархических значений (разделов).</span><span class="sxs-lookup"><span data-stu-id="78f1e-259">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="78f1e-260">Ключи массивов хранятся в Azure Key Vault с двойными тире и числовыми сегментами ключей (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="78f1e-260">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="78f1e-261">Изучите следующую конфигурацию поставщика ведения журнала [Serilog](https://serilog.net/) , предоставляемую файлом JSON.</span><span class="sxs-lookup"><span data-stu-id="78f1e-261">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="78f1e-262">В массиве `WriteTo` определены два объектных литерала, отражающих два *приемника*Serilog, которые описывают назначения для вывода журнала:</span><span class="sxs-lookup"><span data-stu-id="78f1e-262">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="78f1e-263">Конфигурация, показанная в предыдущем JSON-файле, хранится в Azure Key Vault с использованием нотации типа Double (`--`) и числовых сегментов.</span><span class="sxs-lookup"><span data-stu-id="78f1e-263">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="78f1e-264">Раздел</span><span class="sxs-lookup"><span data-stu-id="78f1e-264">Key</span></span> | <span data-ttu-id="78f1e-265">значения</span><span class="sxs-lookup"><span data-stu-id="78f1e-265">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="78f1e-266">Перезагрузка секретов</span><span class="sxs-lookup"><span data-stu-id="78f1e-266">Reload secrets</span></span>

<span data-ttu-id="78f1e-267">Секреты кэшируются до тех пор, пока не будет вызван `IConfigurationRoot.Reload()`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-267">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="78f1e-268">Срок действия истек, отключен и обновленные секреты в хранилище ключей не учитываются приложением, пока не будет выполнено `Reload`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-268">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="78f1e-269">Отключенные секреты и секреты с истекшим сроком действия</span><span class="sxs-lookup"><span data-stu-id="78f1e-269">Disabled and expired secrets</span></span>

<span data-ttu-id="78f1e-270">При отключенных и истекших секретах в среде выполнения создается `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-270">Disabled and expired secrets throw a `KeyVaultClientException` at runtime.</span></span> <span data-ttu-id="78f1e-271">Чтобы предотвратить создание приложения, укажите конфигурацию с помощью другого поставщика конфигурации или обновите закрытый или истекший секрет.</span><span class="sxs-lookup"><span data-stu-id="78f1e-271">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="78f1e-272">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="78f1e-272">Troubleshoot</span></span>

<span data-ttu-id="78f1e-273">Если приложению не удается загрузить конфигурацию с помощью поставщика, в [инфраструктуру ведения журнала ASP.NET Core](xref:fundamentals/logging/index)записывается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="78f1e-273">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="78f1e-274">При следующих условиях загрузка конфигурации будет запрещена:</span><span class="sxs-lookup"><span data-stu-id="78f1e-274">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="78f1e-275">Приложение или сертификат неправильно настроены в Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="78f1e-275">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="78f1e-276">Хранилище ключей не существует в Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="78f1e-276">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="78f1e-277">Приложение не имеет прав доступа к хранилищу ключей.</span><span class="sxs-lookup"><span data-stu-id="78f1e-277">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="78f1e-278">Политика доступа не включает разрешения `Get` и `List`.</span><span class="sxs-lookup"><span data-stu-id="78f1e-278">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="78f1e-279">В хранилище ключей данные конфигурации (пара "имя-значение") неправильно именованы, отсутствуют, отключены или устарели.</span><span class="sxs-lookup"><span data-stu-id="78f1e-279">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="78f1e-280">Приложение имеет неправильное имя хранилища ключей (`KeyVaultName`), идентификатор приложения Azure AD (`AzureADApplicationId`) или отпечаток сертификата Azure AD (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="78f1e-280">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="78f1e-281">Неверный ключ конфигурации (имя) в приложении для загружаемого значения.</span><span class="sxs-lookup"><span data-stu-id="78f1e-281">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="78f1e-282">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="78f1e-282">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="78f1e-283">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="78f1e-283">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="78f1e-284">Microsoft Azure: Документация по Key Vault</span><span class="sxs-lookup"><span data-stu-id="78f1e-284">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="78f1e-285">Создание и перенос ключей, защищенных АППАРАТным модулем безопасности, для Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="78f1e-285">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="78f1e-286">Класс KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="78f1e-286">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="78f1e-287">Краткое руководство. Установка и получение секрета из Azure Key Vault с помощью веб-приложения .NET</span><span class="sxs-lookup"><span data-stu-id="78f1e-287">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="78f1e-288">Руководство. Использование Azure Key Vault с виртуальной машиной Azure Windows в .NET</span><span class="sxs-lookup"><span data-stu-id="78f1e-288">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
