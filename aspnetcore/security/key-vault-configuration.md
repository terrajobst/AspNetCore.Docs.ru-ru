---
title: Поставщик конфигурации хранилища ключей Azure в ASP.NET Core
author: guardrex
description: Узнайте, как использовать поставщик конфигурации Azure Key Vault для настройки приложения с помощью пары "имя значение", загружен во время выполнения.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 78c63cf135ca92f0b5f6c6828b2ae34a44a7b36c
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621017"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="216d7-103">Поставщик конфигурации хранилища ключей Azure в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="216d7-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="216d7-104">По [Люк Лэтем](https://github.com/guardrex) и [Andrew Stanton медсестра](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="216d7-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="216d7-105">В этом документе объясняется, как использовать [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) поставщик конфигурации для загрузки значений конфигурации из секреты Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="216d7-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="216d7-106">Azure Key Vault — это облачная служба, который помогает защита криптографических ключей и секретов, используемых приложений и служб.</span><span class="sxs-lookup"><span data-stu-id="216d7-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="216d7-107">Распространенные сценарии использования хранилища ключей Azure с приложениями ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="216d7-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="216d7-108">Управление доступом к конфиденциальные данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="216d7-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="216d7-109">Соответствующие требования FIPS 140-2 уровня 2 проверить аппаратных модулей безопасности (HSM), при сохранении данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="216d7-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="216d7-110">Этот сценарий доступен для приложений, предназначенных для ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="216d7-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="216d7-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="216d7-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="216d7-112">Пакеты</span><span class="sxs-lookup"><span data-stu-id="216d7-112">Packages</span></span>

<span data-ttu-id="216d7-113">Чтобы использовать поставщик конфигурации Azure Key Vault, добавьте ссылки на пакет [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) пакета.</span><span class="sxs-lookup"><span data-stu-id="216d7-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="216d7-114">Чтобы внедрить [управляемые удостоверения для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview) сценарий, добавьте ссылку на пакет [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) пакета.</span><span class="sxs-lookup"><span data-stu-id="216d7-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="216d7-115">На момент написания статьи, последний стабильный выпуск `Microsoft.Azure.Services.AppAuthentication`, версии `1.0.3`, обеспечивает поддержку для [назначенный системой управляемые удостоверения](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="216d7-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="216d7-116">Поддержка *назначаемого пользователем управляемого удостоверения* доступен в `1.2.0-preview2` пакета.</span><span class="sxs-lookup"><span data-stu-id="216d7-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="216d7-117">В этом разделе демонстрируется использование удостоверения, управляемые системой, и предоставленный пример приложения использует версию `1.0.3` из `Microsoft.Azure.Services.AppAuthentication` пакета.</span><span class="sxs-lookup"><span data-stu-id="216d7-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="216d7-118">Пример приложения</span><span class="sxs-lookup"><span data-stu-id="216d7-118">Sample app</span></span>

<span data-ttu-id="216d7-119">Пример приложения выполняется в одном из двух режимов определяется `#define` инструкция в верхней части *Program.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="216d7-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="216d7-120">`Certificate` &ndash; Демонстрирует использование идентификатора клиента хранилища ключей Azure и X.509 сертификата доступа секреты, хранящиеся в хранилище ключей Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="216d7-121">Эта версия образца может выполняться из любого места, развернутых в службе приложений Azure или любом узле, может обслуживать приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="216d7-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="216d7-122">`Managed` &ndash; Демонстрирует использование [управляемые удостоверения для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview) подлинность приложения в хранилище ключей Azure с аутентификацией Azure AD без учетных данных, хранящихся в коде или конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="216d7-123">При использовании управляемых удостоверений для проверки подлинности, идентификатор приложения Azure AD и пароль (секрет клиента) не являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="216d7-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="216d7-124">`Managed` Версия образца должны быть развернуты в Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="216d7-125">Следуйте указаниям в [использование управляемого удостоверения для ресурсов Azure](#use-managed-identities-for-azure-resources) раздел.</span><span class="sxs-lookup"><span data-stu-id="216d7-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="216d7-126">Дополнительные сведения о том, как настроить пример приложения с помощью директивы препроцессора (`#define`), см. в разделе <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="216d7-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="216d7-127">Хранилища секретов в среде разработки</span><span class="sxs-lookup"><span data-stu-id="216d7-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="216d7-128">Задайте секретами локально с помощью [средство Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="216d7-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="216d7-129">При запуске примера приложения на локальном компьютере в среде разработки, секретные данные загружаются из локального хранилища менеджер секретов.</span><span class="sxs-lookup"><span data-stu-id="216d7-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="216d7-130">Средство Secret Manager требует `<UserSecretsId>` свойство в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="216d7-131">Задайте значение свойства (`{GUID}`) для любого уникального идентификатора GUID:</span><span class="sxs-lookup"><span data-stu-id="216d7-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="216d7-132">Секретные данные создаются в виде пар "имя значение".</span><span class="sxs-lookup"><span data-stu-id="216d7-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="216d7-133">Использование иерархических значений (разделы конфигурации) `:` (двоеточие) как разделитель [конфигурации ASP.NET Core](xref:fundamentals/configuration/index) имен разделов.</span><span class="sxs-lookup"><span data-stu-id="216d7-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="216d7-134">Менеджер секретов используется в командной оболочке, открыт корневой каталог содержимого проекта, где `{SECRET NAME}` — это имя и `{SECRET VALUE}` значение:</span><span class="sxs-lookup"><span data-stu-id="216d7-134">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="216d7-135">Выполните следующие команды в командной строке из содержимого корневого каталога проекта для задания секреты для примера приложения:</span><span class="sxs-lookup"><span data-stu-id="216d7-135">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="216d7-136">Когда эти секреты хранятся в хранилище ключей Azure в [хранилища секретов в рабочей среде с хранилищем ключей Azure](#secret-storage-in-the-production-environment-with-azure-key-vault) разделе `_dev` суффикс изменяется в `_prod`.</span><span class="sxs-lookup"><span data-stu-id="216d7-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="216d7-137">Суффикс предоставляет визуальную подсказку, в выходных данных приложения, указывающий источник значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="216d7-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="216d7-138">Хранилища секретов в рабочей среде с хранилищем ключей Azure</span><span class="sxs-lookup"><span data-stu-id="216d7-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="216d7-139">Инструкции, предоставленные [краткое руководство: Задание и получение секрета из хранилища ключей Azure с помощью Azure CLI](/azure/key-vault/quick-create-cli) разделе кратко перечислены ниже для создания хранилища ключей Azure и хранение секретов, используемых в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="216d7-140">См. в разделе для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="216d7-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="216d7-141">Откройте Azure Cloud shell, с помощью любого из следующих методов в [портала Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="216d7-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="216d7-142">Выберите **попробовать** в правом верхнем углу блока кода.</span><span class="sxs-lookup"><span data-stu-id="216d7-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="216d7-143">Используйте строку поиска «Интерфейс командной строки Azure» в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="216d7-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="216d7-144">Откройте Cloud Shell в браузере с **запуска Cloud Shell** кнопки.</span><span class="sxs-lookup"><span data-stu-id="216d7-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="216d7-145">Выберите **Cloud Shell** кнопку меню в правом верхнем углу портала Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="216d7-146">Дополнительные сведения см. в разделе [интерфейса командной строки Azure (CLI)](/cli/azure/) и [Обзор Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="216d7-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="216d7-147">Если вы еще не прошли аутентификацию, вход с помощью `az login` команды.</span><span class="sxs-lookup"><span data-stu-id="216d7-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="216d7-148">Создайте группу ресурсов, выполнив следующую команду, где `{RESOURCE GROUP NAME}` — имя группы ресурсов для новой группы ресурсов и `{LOCATION}` — регион Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="216d7-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="216d7-149">Создание хранилища ключей в группе ресурсов, выполнив следующую команду, где `{KEY VAULT NAME}` — это имя для нового хранилища ключей и `{LOCATION}` — регион Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="216d7-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="216d7-150">Создание секретов в хранилище ключей в виде пар "имя значение".</span><span class="sxs-lookup"><span data-stu-id="216d7-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="216d7-151">Azure имена секретов Key Vault ограничены буквы, цифры и дефисы.</span><span class="sxs-lookup"><span data-stu-id="216d7-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="216d7-152">Использование иерархических значений (разделы конфигурации) `--` (двух дефисов) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="216d7-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="216d7-153">Имена, которые обычно используются для разделения разделе из подраздела в [конфигурации ASP.NET Core](xref:fundamentals/configuration/index), не разрешены в именах секрета хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="216d7-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="216d7-154">Таким образом двух дефисов используются и поменять местами двоеточие, при загрузке секреты в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="216d7-155">Следующие секреты предназначены для использования с примером приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="216d7-156">Значения включают `_prod` суффикс, чтобы отличать их от `_dev` суффикс значения, загруженных в среде разработки с секретами пользователей.</span><span class="sxs-lookup"><span data-stu-id="216d7-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="216d7-157">Замените `{KEY VAULT NAME}` с именем хранилища ключей, созданный на предыдущем шаге:</span><span class="sxs-lookup"><span data-stu-id="216d7-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="216d7-158">Используйте идентификатор приложения и X.509 сертификат для приложений без размещения Azure</span><span class="sxs-lookup"><span data-stu-id="216d7-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="216d7-159">Настройка Azure AD, Azure Key Vault и приложение для использования идентификатор приложения Azure Active Directory и X.509 сертификата для проверки подлинности в хранилище ключей **когда приложение размещается за пределами Azure**.</span><span class="sxs-lookup"><span data-stu-id="216d7-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="216d7-160">Дополнительные сведения см. в разделе [о ключи, секреты и сертификаты](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="216d7-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="216d7-161">Несмотря на то, что с помощью идентификатора приложения и X.509 сертификата поддерживается для приложений, размещенных в Azure, мы рекомендуем использовать [управляемые удостоверения для ресурсов Azure](#use-managed-identities-for-azure-resources) при размещении приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="216d7-162">Управляемые удостоверения не требуют хранения сертификата в приложении или в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="216d7-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="216d7-163">Пример приложения использует идентификатор приложения и сертификат X.509 при `#define` инструкция в верхней части *Program.cs* для файла `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="216d7-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="216d7-164">Создайте архив PKCS #12 (*.pfx*) сертификата.</span><span class="sxs-lookup"><span data-stu-id="216d7-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="216d7-165">Существуют следующие параметры для создания сертификатов [MakeCert на Windows](/windows/desktop/seccrypto/makecert) и [OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="216d7-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="216d7-166">Установите сертификат в хранилище личных сертификатов текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="216d7-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="216d7-167">Пометить ключ как экспортируемый является необязательным.</span><span class="sxs-lookup"><span data-stu-id="216d7-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="216d7-168">Обратите внимание, отпечаток сертификата, который используется далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="216d7-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="216d7-169">Экспорт в архив PKCS #12 (*.pfx*) сертификат как сертификат в кодировке DER (*.cer*).</span><span class="sxs-lookup"><span data-stu-id="216d7-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="216d7-170">Регистрация приложения в Azure AD (**регистрация приложений**).</span><span class="sxs-lookup"><span data-stu-id="216d7-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="216d7-171">Отправить сертификат в кодировке DER (*.cer*) в Azure AD:</span><span class="sxs-lookup"><span data-stu-id="216d7-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="216d7-172">Выберите приложение в Azure AD.</span><span class="sxs-lookup"><span data-stu-id="216d7-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="216d7-173">Перейдите к **сертификаты и секреты**.</span><span class="sxs-lookup"><span data-stu-id="216d7-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="216d7-174">Выберите **отправка сертификата** для отправки сертификата, который содержит открытый ключ.</span><span class="sxs-lookup"><span data-stu-id="216d7-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="216d7-175">Объект *.cer*, *PEM-файл*, или *.crt* сертификат допустим.</span><span class="sxs-lookup"><span data-stu-id="216d7-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="216d7-176">Store имя хранилища ключей, идентификатор приложения и отпечаток сертификата в приложении *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="216d7-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="216d7-177">Перейдите к **хранилища ключей** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="216d7-178">Выберите хранилище ключей, созданный в [хранилища секретов в рабочей среде с хранилищем ключей Azure](#secret-storage-in-the-production-environment-with-azure-key-vault) раздел.</span><span class="sxs-lookup"><span data-stu-id="216d7-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="216d7-179">Выберите **политики доступа**.</span><span class="sxs-lookup"><span data-stu-id="216d7-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="216d7-180">Выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="216d7-180">Select **Add new**.</span></span>
1. <span data-ttu-id="216d7-181">Выберите **Выбор субъекта** и выберите зарегистрированного приложения по имени.</span><span class="sxs-lookup"><span data-stu-id="216d7-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="216d7-182">Выберите **выберите** кнопки.</span><span class="sxs-lookup"><span data-stu-id="216d7-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="216d7-183">Откройте **разрешения секретов** и укажите приложение с **получить** и **списка** разрешения.</span><span class="sxs-lookup"><span data-stu-id="216d7-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="216d7-184">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="216d7-184">Select **OK**.</span></span>
1. <span data-ttu-id="216d7-185">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="216d7-185">Select **Save**.</span></span>
1. <span data-ttu-id="216d7-186">Разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="216d7-186">Deploy the app.</span></span>

<span data-ttu-id="216d7-187">`Certificate` Пример приложения получает свои значения конфигурации из `IConfigurationRoot` с тем же именем, что имя секрета:</span><span class="sxs-lookup"><span data-stu-id="216d7-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="216d7-188">Не иерархическими значениями: Значение для `SecretName` получается с помощью `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="216d7-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="216d7-189">Иерархические значения (разделов): Используйте `:` нотации (двоеточие) или `GetSection` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="216d7-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="216d7-190">Чтобы получить значение конфигурации, следует используйте один из этих подходов:</span><span class="sxs-lookup"><span data-stu-id="216d7-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="216d7-191">Сертификат X.509 является управляемым Операционной системой.</span><span class="sxs-lookup"><span data-stu-id="216d7-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="216d7-192">Приложение вызывает `AddAzureKeyVault` с помощью значений, предоставленных *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="216d7-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="216d7-193">Примеры значений:</span><span class="sxs-lookup"><span data-stu-id="216d7-193">Example values:</span></span>

* <span data-ttu-id="216d7-194">Имя хранилища ключей: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="216d7-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="216d7-195">Идентификатор приложения: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="216d7-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="216d7-196">Отпечаток сертификата: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="216d7-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="216d7-197">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="216d7-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="216d7-198">При запуске приложения, веб-страница показывает загруженные значения секрета.</span><span class="sxs-lookup"><span data-stu-id="216d7-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="216d7-199">В среде разработки, загрузка значения секретов с помощью `_dev` суффикс.</span><span class="sxs-lookup"><span data-stu-id="216d7-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="216d7-200">В рабочей среде, значения загружать данные с помощью `_prod` суффикс.</span><span class="sxs-lookup"><span data-stu-id="216d7-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="216d7-201">Использование управляемых удостоверений для ресурсов Azure</span><span class="sxs-lookup"><span data-stu-id="216d7-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="216d7-202">**Приложение, развернутое в Azure** можно воспользоваться преимуществами [управляемые удостоверения для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview), который позволяет приложению проверку подлинности с помощью Azure Key Vault с использованием проверки подлинности Azure AD без учетных данных (идентификатор приложения и Секрет Password/Client) хранящихся в приложении.</span><span class="sxs-lookup"><span data-stu-id="216d7-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="216d7-203">Пример приложения использует управляемые удостоверения для ресурсов Azure при `#define` инструкция в верхней части *Program.cs* для файла `Managed`.</span><span class="sxs-lookup"><span data-stu-id="216d7-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="216d7-204">Ввести имя хранилища в приложении *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="216d7-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="216d7-205">Пример приложения не требуется идентификатор приложения и пароль (секрет клиента), если значение `Managed` версии, поэтому вы можете игнорировать эти записи конфигурации.</span><span class="sxs-lookup"><span data-stu-id="216d7-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="216d7-206">Приложение развертывается в Azure и Azure проверяет подлинность приложения для доступа к хранилищу ключей Azure, используя только имя хранилища хранятся в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="216d7-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="216d7-207">Развертывание примера приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="216d7-208">Приложения в службе приложений Azure автоматически зарегистрированы в Azure AD, при создании службы.</span><span class="sxs-lookup"><span data-stu-id="216d7-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="216d7-209">Получите идентификатор объекта из развертывания для использования в следующей команде.</span><span class="sxs-lookup"><span data-stu-id="216d7-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="216d7-210">Идентификатор объекта отображается на портале Azure на **удостоверений** панель службы приложений.</span><span class="sxs-lookup"><span data-stu-id="216d7-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="216d7-211">С помощью интерфейса командной строки Azure и идентификатор объекта приложения, укажите приложение с `list` и `get` разрешения на доступ к хранилищу ключей:</span><span class="sxs-lookup"><span data-stu-id="216d7-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="216d7-212">**Перезапустите приложение** с помощью Azure CLI, PowerShell или портала Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="216d7-213">Пример приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-213">The sample app:</span></span>

* <span data-ttu-id="216d7-214">Создает экземпляр класса `AzureServiceTokenProvider` класс без строки подключения.</span><span class="sxs-lookup"><span data-stu-id="216d7-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="216d7-215">Если не указано строку подключения, поставщик пытается получить маркер доступа с помощью управляемых удостоверений для ресурсов Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="216d7-216">Новый `KeyVaultClient` создается с `AzureServiceTokenProvider` маркера экземпляр обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="216d7-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="216d7-217">`KeyVaultClient` С реализацией по умолчанию используется экземпляр `IKeyVaultSecretManager` , загружает все значения секретов и заменяет двойной тире (`--`) с запятой (`:`) в имена ключей.</span><span class="sxs-lookup"><span data-stu-id="216d7-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="216d7-218">При запуске приложения, веб-страница показывает загруженные значения секрета.</span><span class="sxs-lookup"><span data-stu-id="216d7-218">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="216d7-219">В среде разработки, имеют значения секретов `_dev` суффикс, так как они предоставляются с секретами пользователей.</span><span class="sxs-lookup"><span data-stu-id="216d7-219">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="216d7-220">В рабочей среде, значения загружать данные с помощью `_prod` суффикс, так как они предоставляются в хранилищем ключей Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-220">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="216d7-221">Если вы получили `Access denied` ошибку, убедитесь, что приложение будет зарегистрировано в Azure AD и получают доступ к хранилищу ключей.</span><span class="sxs-lookup"><span data-stu-id="216d7-221">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="216d7-222">Убедитесь, что после перезагрузки службы в Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-222">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="216d7-223">Используйте префикс имени ключа</span><span class="sxs-lookup"><span data-stu-id="216d7-223">Use a key name prefix</span></span>

<span data-ttu-id="216d7-224">`AddAzureKeyVault` предоставляет перегрузку, которая принимает реализацию `IKeyVaultSecretManager`, который позволяет управлять ключевых секреты хранилища преобразуются в ключи конфигурации.</span><span class="sxs-lookup"><span data-stu-id="216d7-224">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="216d7-225">Например можно реализовать интерфейс для загрузки значения секретов, исходя из значения префикса, указанные вами при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-225">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="216d7-226">Это позволяет, например, загружать секреты, в зависимости от версии приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-226">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="216d7-227">Для размещения секреты для нескольких приложений в том же хранилище ключей или для размещения среды секреты не используйте префиксы секретных кодов хранилища ключей (например, *разработки* и *рабочей* секретов) в одну хранилище.</span><span class="sxs-lookup"><span data-stu-id="216d7-227">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="216d7-228">Мы рекомендуем различных приложений и сред разработки, эксплуатации использовать отдельные хранилища ключей для изоляции среды приложений для обеспечения максимальной безопасности.</span><span class="sxs-lookup"><span data-stu-id="216d7-228">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="216d7-229">В следующем примере устанавливается секрет ключа хранилища (и используя средство Secret Manager для среды разработки) для `5000-AppSecret` (периоды не допускаются в именах секрета хранилища ключей).</span><span class="sxs-lookup"><span data-stu-id="216d7-229">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="216d7-230">Этот секрет представляет секрет приложения для версии 5.0.0.0 приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-230">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="216d7-231">Для другой версии приложения, 5.1.0.0, секрет добавляется к ключу хранилища (и используя средство Secret Manager) для `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="216d7-231">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="216d7-232">Каждая версия приложения загружает его с версиями секретное значение в его конфигурацию в качестве `AppSecret`, удаление версии при загрузке секрет.</span><span class="sxs-lookup"><span data-stu-id="216d7-232">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="216d7-233">`AddAzureKeyVault` вызывается с пользовательским `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="216d7-233">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="216d7-234">`IKeyVaultSecretManager` Реализации реагирует на префиксы версии секретов, чтобы загрузить правильный секрет в конфигурации:</span><span class="sxs-lookup"><span data-stu-id="216d7-234">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="216d7-235">`Load` Метод вызывается с помощью алгоритма поставщик, выполняющий итерацию секреты хранилища, чтобы найти те, которые имеют префикс версии.</span><span class="sxs-lookup"><span data-stu-id="216d7-235">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="216d7-236">Найденный префикс версии с `Load`, алгоритм использует `GetKey` метод, чтобы вернуть имя конфигурации имя секрета.</span><span class="sxs-lookup"><span data-stu-id="216d7-236">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="216d7-237">Она отсекает версии префикс из имени секрета и возвращает остальные имя секрета для загрузки в конфигурацию приложения пары "имя значение".</span><span class="sxs-lookup"><span data-stu-id="216d7-237">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="216d7-238">Если при реализации этого подхода:</span><span class="sxs-lookup"><span data-stu-id="216d7-238">When this approach is implemented:</span></span>

1. <span data-ttu-id="216d7-239">Версия приложения, указанные в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-239">The app's version specified in the app's project file.</span></span> <span data-ttu-id="216d7-240">В следующем примере версия приложения имеет значение `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="216d7-240">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="216d7-241">Убедитесь, что `<UserSecretsId>` свойство присутствует в файле проекта приложения, где `{GUID}` представляет собой идентификатор GUID, предоставленный пользователем:</span><span class="sxs-lookup"><span data-stu-id="216d7-241">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="216d7-242">Сохранить следующие секреты локально с помощью [средство Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="216d7-242">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="216d7-243">Секретные данные сохраняются в хранилище ключей Azure с помощью следующих команд Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="216d7-243">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="216d7-244">При запуске приложения, загружаются секретных кодов хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="216d7-244">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="216d7-245">Строка секрета для `5000-AppSecret` сопоставляется версия приложения, указанные в файле проекта приложения (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="216d7-245">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="216d7-246">Версии, `5000` (с dash), удаляются из имени ключа.</span><span class="sxs-lookup"><span data-stu-id="216d7-246">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="216d7-247">В приложении, чтение конфигурации с ключом `AppSecret` загружает значение секрета.</span><span class="sxs-lookup"><span data-stu-id="216d7-247">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="216d7-248">Если версия приложения изменяется в файле проекта для `5.1.0.0` и снова запустите приложение, секрета возвращаемое значение `5.1.0.0_secret_value_dev` в среде разработки и `5.1.0.0_secret_value_prod` в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="216d7-248">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="216d7-249">Можно также предоставить собственную `KeyVaultClient` реализацию `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="216d7-249">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="216d7-250">Пользовательский клиент позволяет совместно использовать один экземпляр клиента приложения.</span><span class="sxs-lookup"><span data-stu-id="216d7-250">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="216d7-251">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="216d7-251">Bind an array to a class</span></span>

<span data-ttu-id="216d7-252">Поставщик способный считывать значения конфигурации в массив для привязки к массиву POCO.</span><span class="sxs-lookup"><span data-stu-id="216d7-252">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="216d7-253">При чтении из источника конфигурации, который позволяет ключи содержат двоеточия (`:`) разделители, числового ключа сегмента используется для различения ключи, составляющие массив (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="216d7-253">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="216d7-254">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="216d7-254">`:{n}:`).</span></span> <span data-ttu-id="216d7-255">Дополнительные сведения см. в разделе [конфигурации: Привязка к классу массив](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="216d7-255">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="216d7-256">Azure Key Vault ключи нельзя использовать двоеточие в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="216d7-256">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="216d7-257">Подход, описанный в этом разделе используются двойные тире (`--`) как разделитель для значений иерархической (разделов).</span><span class="sxs-lookup"><span data-stu-id="216d7-257">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="216d7-258">Массив ключи хранятся в хранилище ключей Azure с double штрихов и числовых ключей сегментов (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="216d7-258">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="216d7-259">Рассмотрим следующую [Serilog](https://serilog.net/) конфигурация поставщика, предоставляемые JSON-файл журнала.</span><span class="sxs-lookup"><span data-stu-id="216d7-259">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="216d7-260">Существуют два объектных литералов, определенные в `WriteTo` массива, который отражает две Serilog *приемников*, описывающих назначения для выходных данных ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="216d7-260">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="216d7-261">Конфигурации, представленной выше JSON-файл хранится в хранилище ключей Azure с помощью двойной штрих (`--`) нотации и числовые сегменты:</span><span class="sxs-lookup"><span data-stu-id="216d7-261">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="216d7-262">Ключ</span><span class="sxs-lookup"><span data-stu-id="216d7-262">Key</span></span> | <span data-ttu-id="216d7-263">Значение</span><span class="sxs-lookup"><span data-stu-id="216d7-263">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="216d7-264">Перезагрузить секретов</span><span class="sxs-lookup"><span data-stu-id="216d7-264">Reload secrets</span></span>

<span data-ttu-id="216d7-265">Секреты, кэшируются до `IConfigurationRoot.Reload()` вызывается.</span><span class="sxs-lookup"><span data-stu-id="216d7-265">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="216d7-266">Истек, отключена, и обновленные секреты в хранилище ключей не учитываются в приложении до `Reload` выполняется.</span><span class="sxs-lookup"><span data-stu-id="216d7-266">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="216d7-267">Отключенные и истекшим сроком действия секретов</span><span class="sxs-lookup"><span data-stu-id="216d7-267">Disabled and expired secrets</span></span>

<span data-ttu-id="216d7-268">Отключенные и просроченные секреты throw `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="216d7-268">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="216d7-269">Чтобы избежать возникновения в приложении, замените приложение или изменить секрет просроченного отключено.</span><span class="sxs-lookup"><span data-stu-id="216d7-269">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="216d7-270">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="216d7-270">Troubleshoot</span></span>

<span data-ttu-id="216d7-271">Если приложение не удается загрузить конфигурацию с помощью поставщика, сообщение об ошибке записывается [инфраструктуры ведения журнала ASP.NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="216d7-271">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="216d7-272">Следующие условия не позволит конфигурации загрузки:</span><span class="sxs-lookup"><span data-stu-id="216d7-272">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="216d7-273">Приложение или сертификат неправильно настроено в Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="216d7-273">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="216d7-274">Хранилище ключей не существует в хранилище ключей Azure.</span><span class="sxs-lookup"><span data-stu-id="216d7-274">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="216d7-275">Приложение не имеет разрешения на доступ к хранилищу ключей.</span><span class="sxs-lookup"><span data-stu-id="216d7-275">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="216d7-276">Политика доступа не включает `Get` и `List` разрешения.</span><span class="sxs-lookup"><span data-stu-id="216d7-276">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="216d7-277">В хранилище ключей данные конфигурации (пара «имя значение») ошибочно названы, отсутствуют, отключена или истек срок действия.</span><span class="sxs-lookup"><span data-stu-id="216d7-277">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="216d7-278">Приложение имеет имя неправильный хранилища ключей (`KeyVaultName`), идентификатор приложения Azure AD (`AzureADApplicationId`), или отпечаток сертификата Azure AD (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="216d7-278">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="216d7-279">В приложении для значения, которое вы пытаетесь загрузить неправильный ключ конфигурации (имя).</span><span class="sxs-lookup"><span data-stu-id="216d7-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="216d7-280">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="216d7-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="216d7-281">Microsoft Azure: Хранилище ключей</span><span class="sxs-lookup"><span data-stu-id="216d7-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="216d7-282">Microsoft Azure: Документация по хранилищу ключей</span><span class="sxs-lookup"><span data-stu-id="216d7-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="216d7-283">Использование защищенных аппаратным модулем безопасности и их передача ключей для хранилища ключей Azure</span><span class="sxs-lookup"><span data-stu-id="216d7-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="216d7-284">Класс KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="216d7-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="216d7-285">Краткое руководство. Задание и получение секрета из хранилища ключей Azure с помощью веб-приложения .NET</span><span class="sxs-lookup"><span data-stu-id="216d7-285">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="216d7-286">Учебник. Как использовать Azure Key Vault с помощью Windows виртуальной машине Azure в .NET</span><span class="sxs-lookup"><span data-stu-id="216d7-286">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
