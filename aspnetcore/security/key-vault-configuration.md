---
title: Поставщик конфигурации Azure Key Vault в ASP.NET Core
author: guardrex
description: Узнайте, как использовать поставщик конфигурации Azure Key Vault для настройки приложения с помощью пар "имя-значение", загружаемых во время выполнения.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/16/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 37ba756cc4170c145d2ab1f9f0a465057cc826c1
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358712"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="01fe9-103">Поставщик конфигурации Azure Key Vault в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01fe9-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="01fe9-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Эндрю Стантон-медперсонала](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="01fe9-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="01fe9-105">В этом документе объясняется, как использовать поставщик конфигурации [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) для загрузки значений конфигурации приложения из Azure Key Vault секреты.</span><span class="sxs-lookup"><span data-stu-id="01fe9-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="01fe9-106">Azure Key Vault — это облачная служба, которая помогает защитить криптографические ключи и секреты, используемые приложениями и службами.</span><span class="sxs-lookup"><span data-stu-id="01fe9-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="01fe9-107">Распространенные сценарии использования Azure Key Vault с приложениями ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="01fe9-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="01fe9-108">Управление доступом к конфиденциальным данным конфигурации.</span><span class="sxs-lookup"><span data-stu-id="01fe9-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="01fe9-109">Соблюдайте требования для FIPS 140-2 уровня 2 проверенных аппаратных модулей безопасности (HSM) при хранении данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="01fe9-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="01fe9-110">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="01fe9-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="01fe9-111">пакеты,</span><span class="sxs-lookup"><span data-stu-id="01fe9-111">Packages</span></span>

<span data-ttu-id="01fe9-112">Добавьте ссылку на пакет в пакет [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .</span><span class="sxs-lookup"><span data-stu-id="01fe9-112">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="01fe9-113">Пример приложения</span><span class="sxs-lookup"><span data-stu-id="01fe9-113">Sample app</span></span>

<span data-ttu-id="01fe9-114">Пример приложения выполняется в одном из двух режимов, определенных инструкцией `#define` в верхней части файла *Program.CS* :</span><span class="sxs-lookup"><span data-stu-id="01fe9-114">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="01fe9-115">`Certificate` &ndash; демонстрирует использование идентификатора клиента Azure Key Vault и сертификата X. 509 для доступа к секретам, хранящимся в Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="01fe9-115">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="01fe9-116">Эту версию примера можно запустить из любого расположения, развернуть в службе приложений Azure или на любом узле, способном обслуживать ASP.NET Coreное приложение.</span><span class="sxs-lookup"><span data-stu-id="01fe9-116">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="01fe9-117">`Managed` &ndash; демонстрируется использование [управляемых удостоверений для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview) для проверки подлинности приложения при Azure Key Vault с проверкой подлинности Azure AD без учетных данных, хранящихся в коде или конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-117">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="01fe9-118">При использовании управляемых удостоверений для проверки подлинности не требуется идентификатор и пароль приложения Azure AD (секрет клиента).</span><span class="sxs-lookup"><span data-stu-id="01fe9-118">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="01fe9-119">Версия `Managed` образца должна быть развернута в Azure.</span><span class="sxs-lookup"><span data-stu-id="01fe9-119">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="01fe9-120">Следуйте указаниям в разделе [Использование управляемых удостоверений для ресурсов Azure](#use-managed-identities-for-azure-resources) .</span><span class="sxs-lookup"><span data-stu-id="01fe9-120">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="01fe9-121">Дополнительные сведения о настройке примера приложения с помощью директив препроцессора (`#define`) см. в разделе <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="01fe9-121">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="01fe9-122">Секретное хранилище в среде разработки</span><span class="sxs-lookup"><span data-stu-id="01fe9-122">Secret storage in the Development environment</span></span>

<span data-ttu-id="01fe9-123">Локальное задание секретов с помощью [средства диспетчера секретов](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="01fe9-123">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="01fe9-124">Когда пример приложения выполняется на локальном компьютере в среде разработки, секреты загружаются из локального хранилища диспетчера секретов.</span><span class="sxs-lookup"><span data-stu-id="01fe9-124">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="01fe9-125">Для средства диспетчера секретов требуется свойство `<UserSecretsId>` в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-125">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="01fe9-126">Задайте для свойства значение (`{GUID}`) любой уникальный GUID:</span><span class="sxs-lookup"><span data-stu-id="01fe9-126">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="01fe9-127">Секреты создаются как пары "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="01fe9-127">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="01fe9-128">Иерархические значения (разделы конфигурации) используют `:` (двоеточие) в качестве разделителя в именах [ASP.NET Core ключей конфигурации](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="01fe9-128">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="01fe9-129">Диспетчер секретов используется из командной оболочки, открытой в [корне содержимого](xref:fundamentals/index#content-root)проекта, где `{SECRET NAME}` — это имя, а `{SECRET VALUE}` — значение.</span><span class="sxs-lookup"><span data-stu-id="01fe9-129">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="01fe9-130">Выполните следующие команды в командной оболочке из [корневого каталога содержимого](xref:fundamentals/index#content-root) проекта, чтобы задать секреты для примера приложения:</span><span class="sxs-lookup"><span data-stu-id="01fe9-130">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="01fe9-131">Если эти секреты хранятся в Azure Key Vault в [хранилище секретов в рабочей среде с Azure Key Vaultным](#secret-storage-in-the-production-environment-with-azure-key-vault) разделом, суффикс `_dev` меняется на `_prod`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-131">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="01fe9-132">Суффикс предоставляет визуальную подсказку в выходных данных приложения, указывающую источник значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="01fe9-132">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="01fe9-133">Секретное хранилище в рабочей среде с Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="01fe9-133">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="01fe9-134">Инструкции, приведенные в [кратком руководстве по установке и извлечению секретов из Azure Key Vault с помощью Azure CLI](/azure/key-vault/quick-create-cli) разделе, приведены здесь для создания Azure Key Vault и хранения секретов, используемых в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-134">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="01fe9-135">Дополнительные сведения см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="01fe9-135">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="01fe9-136">Откройте Azure Cloud Shell одним из следующих способов в [портал Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="01fe9-136">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="01fe9-137">Нажмите кнопку **Попробовать** в правом верхнем углу блока с кодом.</span><span class="sxs-lookup"><span data-stu-id="01fe9-137">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="01fe9-138">Используйте строку поиска "Azure CLI" в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="01fe9-138">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="01fe9-139">Откройте Cloud Shell в браузере с помощью кнопки **запустить Cloud Shell** .</span><span class="sxs-lookup"><span data-stu-id="01fe9-139">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="01fe9-140">Нажмите кнопку меню **Cloud Shell** в правом верхнем углу окна портала Azure.</span><span class="sxs-lookup"><span data-stu-id="01fe9-140">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="01fe9-141">Дополнительные сведения см. в статьях [интерфейс командной строки Azure (CLI)](/cli/azure/) и [общие сведения о Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="01fe9-141">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="01fe9-142">Если вы еще не прошли проверку подлинности, выполните вход с помощью команды `az login`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-142">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="01fe9-143">Создайте группу ресурсов с помощью следующей команды, где `{RESOURCE GROUP NAME}` — это имя группы ресурсов для новой группы ресурсов, а `{LOCATION}` — регион Azure (центр обработки данных):</span><span class="sxs-lookup"><span data-stu-id="01fe9-143">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="01fe9-144">Создайте хранилище ключей в группе ресурсов с помощью следующей команды, где `{KEY VAULT NAME}` — это имя нового хранилища ключей, а `{LOCATION}` — регион Azure (центр обработки данных):</span><span class="sxs-lookup"><span data-stu-id="01fe9-144">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="01fe9-145">Создайте секреты в хранилище ключей в виде пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="01fe9-145">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="01fe9-146">Azure Key Vault имена секретов ограничены буквенно-цифровыми символами и тире.</span><span class="sxs-lookup"><span data-stu-id="01fe9-146">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="01fe9-147">Иерархические значения (разделы конфигурации) используют `--` (два тире) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="01fe9-147">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="01fe9-148">Двоеточия, которые обычно используются для разделения раздела из подраздела в [ASP.NET Core конфигурации](xref:fundamentals/configuration/index), не допускаются в именах секретов хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="01fe9-148">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="01fe9-149">Поэтому при загрузке секретов в конфигурацию приложения используются два дефиса и они переключаются на двоеточие.</span><span class="sxs-lookup"><span data-stu-id="01fe9-149">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="01fe9-150">Следующие секреты предназначены для использования с примером приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-150">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="01fe9-151">Значения включают суффикс `_prod`, чтобы отличить их от `_dev`ных суффиксов, загруженных в среде разработки из секретов пользователя.</span><span class="sxs-lookup"><span data-stu-id="01fe9-151">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="01fe9-152">Замените `{KEY VAULT NAME}` именем хранилища ключей, созданным на предыдущем шаге:</span><span class="sxs-lookup"><span data-stu-id="01fe9-152">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="01fe9-153">Использование идентификатора приложения и сертификата X. 509 для приложений, не размещенных в Azure</span><span class="sxs-lookup"><span data-stu-id="01fe9-153">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="01fe9-154">Настройте Azure AD, Azure Key Vault и приложение для использования идентификатора Azure Active Directory приложения и сертификата X. 509 для проверки подлинности в хранилище ключей, **Если приложение размещено за пределами Azure**.</span><span class="sxs-lookup"><span data-stu-id="01fe9-154">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="01fe9-155">См. дополнительные сведения о [ключах, секретах и сертификатах](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="01fe9-155">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="01fe9-156">Хотя использование идентификатора приложения и сертификата X. 509 поддерживается для приложений, размещенных в Azure, мы рекомендуем использовать [управляемые удостоверения для ресурсов Azure](#use-managed-identities-for-azure-resources) при размещении приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="01fe9-156">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="01fe9-157">Для управляемых удостоверений не требуется хранить сертификат в приложении или в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="01fe9-157">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="01fe9-158">В примере приложения используется идентификатор приложения и сертификат X. 509, если в инструкции `#define` в верхней части файла *Program.CS* установлено значение `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-158">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="01fe9-159">Создайте архивный сертификат PKCS # 12 (*PFX*-файл).</span><span class="sxs-lookup"><span data-stu-id="01fe9-159">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="01fe9-160">Для создания сертификатов можно использовать [MakeCert в Windows](/windows/desktop/seccrypto/makecert) и [OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="01fe9-160">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="01fe9-161">Установите сертификат в личное хранилище сертификатов текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="01fe9-161">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="01fe9-162">Пометка ключа как экспортируемого необязательна.</span><span class="sxs-lookup"><span data-stu-id="01fe9-162">Marking the key as exportable is optional.</span></span> <span data-ttu-id="01fe9-163">Запишите отпечаток сертификата, который будет использоваться далее в этом процессе.</span><span class="sxs-lookup"><span data-stu-id="01fe9-163">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="01fe9-164">Экспортируйте архивный сертификат PKCS # 12 (*PFX*) как сертификат в формате DER (*CER*).</span><span class="sxs-lookup"><span data-stu-id="01fe9-164">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="01fe9-165">Зарегистрируйте приложение в Azure AD (**Регистрация приложений**).</span><span class="sxs-lookup"><span data-stu-id="01fe9-165">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="01fe9-166">Отправьте сертификат в кодировке DER (*CER*) в Azure AD:</span><span class="sxs-lookup"><span data-stu-id="01fe9-166">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="01fe9-167">Выберите приложение в Azure AD.</span><span class="sxs-lookup"><span data-stu-id="01fe9-167">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="01fe9-168">Перейдите к разделу **сертификаты & секреты**.</span><span class="sxs-lookup"><span data-stu-id="01fe9-168">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="01fe9-169">Выберите **отправить сертификат** , чтобы отправить сертификат, который содержит открытый ключ.</span><span class="sxs-lookup"><span data-stu-id="01fe9-169">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="01fe9-170">Сертификат *. cer*, *. pem*или *. CRT* является приемлемым.</span><span class="sxs-lookup"><span data-stu-id="01fe9-170">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="01fe9-171">Сохраните имя хранилища ключей, идентификатор приложения и отпечаток сертификата в файле *appSettings. JSON* приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-171">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="01fe9-172">Перейдите к **разделу хранилища ключей** в портал Azure.</span><span class="sxs-lookup"><span data-stu-id="01fe9-172">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="01fe9-173">Выберите хранилище ключей, созданное в [хранилище секретов в рабочей среде с помощью Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) разделе.</span><span class="sxs-lookup"><span data-stu-id="01fe9-173">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="01fe9-174">Выберите **Политики доступа**.</span><span class="sxs-lookup"><span data-stu-id="01fe9-174">Select **Access policies**.</span></span>
1. <span data-ttu-id="01fe9-175">Выберите **Добавить политику доступа**.</span><span class="sxs-lookup"><span data-stu-id="01fe9-175">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="01fe9-176">Откройте **разрешения для секрета** и предоставьте приложению разрешения **Get** и **List** .</span><span class="sxs-lookup"><span data-stu-id="01fe9-176">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="01fe9-177">Щелкните **выбрать субъект** и выберите зарегистрированное приложение по имени.</span><span class="sxs-lookup"><span data-stu-id="01fe9-177">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="01fe9-178">Нажмите кнопку **Выбрать**.</span><span class="sxs-lookup"><span data-stu-id="01fe9-178">Select the **Select** button.</span></span>
1. <span data-ttu-id="01fe9-179">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="01fe9-179">Select **OK**.</span></span>
1. <span data-ttu-id="01fe9-180">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="01fe9-180">Select **Save**.</span></span>
1. <span data-ttu-id="01fe9-181">Разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="01fe9-181">Deploy the app.</span></span>

<span data-ttu-id="01fe9-182">`Certificate` пример приложения получает значения конфигурации из `IConfigurationRoot` с тем же именем, что и имя секрета:</span><span class="sxs-lookup"><span data-stu-id="01fe9-182">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="01fe9-183">Неиерархические значения: значение для `SecretName` получено с `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-183">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="01fe9-184">Иерархические значения (разделы): используйте нотацию `:` (двоеточие) или метод расширения `GetSection`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-184">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="01fe9-185">Используйте любой из этих подходов для получения значения конфигурации:</span><span class="sxs-lookup"><span data-stu-id="01fe9-185">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="01fe9-186">Сертификат X. 509 управляется ОС.</span><span class="sxs-lookup"><span data-stu-id="01fe9-186">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="01fe9-187">Приложение вызывает <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> со значениями, предоставленными файлом *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="01fe9-187">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

<span data-ttu-id="01fe9-188">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="01fe9-188">Example values:</span></span>

* <span data-ttu-id="01fe9-189">Имя хранилища ключей: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="01fe9-189">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="01fe9-190">Идентификатор приложения: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="01fe9-190">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="01fe9-191">Отпечаток сертификата: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="01fe9-191">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="01fe9-192">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="01fe9-192">*appsettings.json*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

<span data-ttu-id="01fe9-193">При запуске приложения на веб-странице отображаются загруженные значения секрета.</span><span class="sxs-lookup"><span data-stu-id="01fe9-193">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="01fe9-194">В среде разработки значения секрета загружаются с суффиксом `_dev`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-194">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="01fe9-195">В рабочей среде значения загружаются с суффиксом `_prod`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-195">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="01fe9-196">Использование управляемых удостоверений для ресурсов Azure</span><span class="sxs-lookup"><span data-stu-id="01fe9-196">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="01fe9-197">**Приложение, развернутое в Azure** , может использовать преимущества [управляемых удостоверений для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview), что позволяет приложению проходить проверку подлинности в Azure Key Vault с помощью проверки подлинности Azure AD без учетных данных (идентификатор приложения и пароль/секрет клиента), хранимых в приложении.</span><span class="sxs-lookup"><span data-stu-id="01fe9-197">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="01fe9-198">Пример приложения использует управляемые удостоверения для ресурсов Azure, когда инструкция `#define` в верхней части файла *Program.CS* имеет значение `Managed`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-198">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="01fe9-199">Введите имя хранилища в файл *appSettings. JSON* приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-199">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="01fe9-200">В примере приложения не требуется идентификатор приложения и пароль (секрет клиента), если задана версия `Managed`, поэтому эти записи конфигурации можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="01fe9-200">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="01fe9-201">Приложение развертывается в Azure, а Azure выполняет проверку подлинности приложения для доступа к Azure Key Vault только с использованием имени хранилища, хранящегося в файле *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="01fe9-201">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="01fe9-202">Разверните пример приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="01fe9-202">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="01fe9-203">Приложение, развернутое в службе приложений Azure, автоматически регистрируется в Azure AD при создании службы.</span><span class="sxs-lookup"><span data-stu-id="01fe9-203">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="01fe9-204">Получите идентификатор объекта из развертывания для использования в следующей команде.</span><span class="sxs-lookup"><span data-stu-id="01fe9-204">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="01fe9-205">Идентификатор объекта отображается в портал Azure на панели **удостоверений** службы приложений.</span><span class="sxs-lookup"><span data-stu-id="01fe9-205">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="01fe9-206">Используя Azure CLI и идентификатор объекта приложения, укажите приложение с разрешениями `list` и `get` для доступа к хранилищу ключей:</span><span class="sxs-lookup"><span data-stu-id="01fe9-206">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="01fe9-207">**Перезапустите приложение** с помощью Azure CLI, PowerShell или портал Azure.</span><span class="sxs-lookup"><span data-stu-id="01fe9-207">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="01fe9-208">Пример приложения:</span><span class="sxs-lookup"><span data-stu-id="01fe9-208">The sample app:</span></span>

* <span data-ttu-id="01fe9-209">Создает экземпляр класса `AzureServiceTokenProvider` без строки подключения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-209">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="01fe9-210">Если строка подключения не указана, поставщик пытается получить маркер доступа из управляемых удостоверений для ресурсов Azure.</span><span class="sxs-lookup"><span data-stu-id="01fe9-210">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="01fe9-211">Новый <xref:Microsoft.Azure.KeyVault.KeyVaultClient> создается с помощью обратного вызова маркера экземпляра `AzureServiceTokenProvider`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-211">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="01fe9-212"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> экземпляр используется с реализацией по умолчанию <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, которая загружает все значения секрета и заменяет двойные тире (`--`) двоеточием (`:`) в именах ключей.</span><span class="sxs-lookup"><span data-stu-id="01fe9-212">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

<span data-ttu-id="01fe9-213">Пример значения имени хранилища ключей: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="01fe9-213">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="01fe9-214">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="01fe9-214">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="01fe9-215">При запуске приложения на веб-странице отображаются загруженные значения секрета.</span><span class="sxs-lookup"><span data-stu-id="01fe9-215">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="01fe9-216">В среде разработки значения секрета имеют суффикс `_dev`, так как они предоставляются секретами пользователя.</span><span class="sxs-lookup"><span data-stu-id="01fe9-216">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="01fe9-217">В рабочей среде значения загружаются с суффиксом `_prod`, так как они предоставляются Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="01fe9-217">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="01fe9-218">При возникновении ошибки `Access denied` убедитесь, что приложение зарегистрировано в Azure AD и предоставил доступ к хранилищу ключей.</span><span class="sxs-lookup"><span data-stu-id="01fe9-218">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="01fe9-219">Убедитесь, что вы перезапустите службу в Azure.</span><span class="sxs-lookup"><span data-stu-id="01fe9-219">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="01fe9-220">Сведения об использовании поставщика с управляемым удостоверением и конвейером Azure DevOps см. в статье [Создание подключения Azure Resource Manager службы к виртуальной машине с помощью управляемого удостоверения службы](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span><span class="sxs-lookup"><span data-stu-id="01fe9-220">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configuration-options"></a><span data-ttu-id="01fe9-221">Параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="01fe9-221">Configuration options</span></span>

<span data-ttu-id="01fe9-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> может принимать <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span><span class="sxs-lookup"><span data-stu-id="01fe9-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> can accept an <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span></span>

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| <span data-ttu-id="01fe9-223">Идентификаторы</span><span class="sxs-lookup"><span data-stu-id="01fe9-223">Property</span></span>         | <span data-ttu-id="01fe9-224">Описание</span><span class="sxs-lookup"><span data-stu-id="01fe9-224">Description</span></span> |
| ---------------- | ----------- |
| `Client`         | <span data-ttu-id="01fe9-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient>, используемый для получения значений.</span><span class="sxs-lookup"><span data-stu-id="01fe9-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> to use for retrieving values.</span></span> |
| `Manager`        | <span data-ttu-id="01fe9-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> экземпляр, используемый для управления загрузкой секрета.</span><span class="sxs-lookup"><span data-stu-id="01fe9-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instance used to control secret loading.</span></span> |
| `ReloadInterval` | <span data-ttu-id="01fe9-227">`Timespan` ожидания между попытками опроса хранилища ключей на предмет изменений.</span><span class="sxs-lookup"><span data-stu-id="01fe9-227">`Timespan` to wait between attempts at polling the key vault for changes.</span></span> <span data-ttu-id="01fe9-228">Значение по умолчанию — `null` (конфигурация не перегружается).</span><span class="sxs-lookup"><span data-stu-id="01fe9-228">The default value is `null` (configuration isn't reloaded).</span></span> |
| `Vault`          | <span data-ttu-id="01fe9-229">URI хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="01fe9-229">Key vault URI.</span></span> |

::: moniker-end

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="01fe9-230">Использование префикса имени ключа</span><span class="sxs-lookup"><span data-stu-id="01fe9-230">Use a key name prefix</span></span>

<span data-ttu-id="01fe9-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> предоставляет перегрузку, которая принимает реализацию <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, которая позволяет управлять преобразованием секретов ключей хранилища в конфигурационные ключи.</span><span class="sxs-lookup"><span data-stu-id="01fe9-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="01fe9-232">Например, можно реализовать интерфейс для загрузки секретных значений на основе значения префикса, которое вы задают при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-232">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="01fe9-233">Это позволяет, например, загружать секреты на основе версии приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-233">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="01fe9-234">Не используйте префиксы в секретах хранилища ключей, чтобы размещать секреты для нескольких приложений в одном хранилище ключей или для размещения секретов среды (например, для *разработки* и *рабочих* секретов) в одном хранилище.</span><span class="sxs-lookup"><span data-stu-id="01fe9-234">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="01fe9-235">Мы рекомендуем использовать отдельные хранилища ключей для различных приложений и сред разработки и рабочей среды, чтобы изолировать среды приложений для обеспечения наивысшего уровня безопасности.</span><span class="sxs-lookup"><span data-stu-id="01fe9-235">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="01fe9-236">В следующем примере в хранилище ключей устанавливается секрет (и используется средство диспетчера секретов для среды разработки) для `5000-AppSecret` (точки не допускаются в именах секретов хранилища ключей).</span><span class="sxs-lookup"><span data-stu-id="01fe9-236">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="01fe9-237">Этот секрет представляет секрет приложения для версии 5.0.0.0 приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-237">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="01fe9-238">Для другой версии приложения 5.1.0.0 секрет добавляется в хранилище ключей (и с помощью средства диспетчера секретов) для `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-238">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="01fe9-239">Каждая версия приложения загружает секретное значение в конфигурацию в виде `AppSecret`, отключая версию при загрузке секрета.</span><span class="sxs-lookup"><span data-stu-id="01fe9-239">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="01fe9-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> вызывается с настраиваемым <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span><span class="sxs-lookup"><span data-stu-id="01fe9-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="01fe9-241"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>ная реализация реагирует на префиксы версий секретов, чтобы загрузить правильный секрет в конфигурацию:</span><span class="sxs-lookup"><span data-stu-id="01fe9-241">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="01fe9-242">`Load` загружает секрет, если его имя начинается с префикса.</span><span class="sxs-lookup"><span data-stu-id="01fe9-242">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="01fe9-243">Другие секреты не загружены.</span><span class="sxs-lookup"><span data-stu-id="01fe9-243">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="01fe9-244">`GetKey`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-244">`GetKey`:</span></span>
  * <span data-ttu-id="01fe9-245">Удаляет префикс из имени секрета.</span><span class="sxs-lookup"><span data-stu-id="01fe9-245">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="01fe9-246">Заменяет два тире в любом имени на `KeyDelimiter`, который является разделителем, используемым в конфигурации (обычно это двоеточие).</span><span class="sxs-lookup"><span data-stu-id="01fe9-246">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="01fe9-247">Azure Key Vault не допускает двоеточия в именах секретов.</span><span class="sxs-lookup"><span data-stu-id="01fe9-247">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="01fe9-248">Метод `Load` вызывается алгоритмом поставщика, который выполняет перебор секретов хранилища для поиска тех, у которых есть префикс версии.</span><span class="sxs-lookup"><span data-stu-id="01fe9-248">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="01fe9-249">При обнаружении префикса версии с `Load`алгоритм использует метод `GetKey`, чтобы получить имя конфигурации для имени секрета.</span><span class="sxs-lookup"><span data-stu-id="01fe9-249">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="01fe9-250">Он отделяет префикс версии от имени секрета и возвращает оставшуюся часть имени секрета для загрузки в пары "имя-значение" конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-250">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="01fe9-251">При реализации этого подхода:</span><span class="sxs-lookup"><span data-stu-id="01fe9-251">When this approach is implemented:</span></span>

1. <span data-ttu-id="01fe9-252">Версия приложения, указанная в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-252">The app's version specified in the app's project file.</span></span> <span data-ttu-id="01fe9-253">В следующем примере для версии приложения задано значение `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="01fe9-253">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="01fe9-254">Убедитесь, что в файле проекта приложения имеется `<UserSecretsId>` свойство, где `{GUID}` является идентификатором GUID, предоставляемым пользователем:</span><span class="sxs-lookup"><span data-stu-id="01fe9-254">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="01fe9-255">Сохраните следующие секреты локально с помощью [средства диспетчера секретов](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="01fe9-255">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="01fe9-256">Секреты сохраняются в Azure Key Vault с помощью следующих Azure CLI команд:</span><span class="sxs-lookup"><span data-stu-id="01fe9-256">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="01fe9-257">При запуске приложения загружаются секреты хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="01fe9-257">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="01fe9-258">Строковый секрет для `5000-AppSecret` соответствует версии приложения, указанной в файле проекта приложения (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="01fe9-258">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="01fe9-259">Версия `5000` (с тире) удаляется из имени ключа.</span><span class="sxs-lookup"><span data-stu-id="01fe9-259">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="01fe9-260">На протяжении всего приложения чтение конфигурации с ключом `AppSecret` загружает секретное значение.</span><span class="sxs-lookup"><span data-stu-id="01fe9-260">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="01fe9-261">Если версия приложения изменена в файле проекта на `5.1.0.0` и приложение запускается снова, то возвращаемое значение секрета будет `5.1.0.0_secret_value_dev` в среде разработки и `5.1.0.0_secret_value_prod` в производстве.</span><span class="sxs-lookup"><span data-stu-id="01fe9-261">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="01fe9-262">Для <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>можно также предоставить собственную реализацию <xref:Microsoft.Azure.KeyVault.KeyVaultClient>.</span><span class="sxs-lookup"><span data-stu-id="01fe9-262">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="01fe9-263">Настраиваемый клиент позволяет совместно использовать один экземпляр клиента в приложении.</span><span class="sxs-lookup"><span data-stu-id="01fe9-263">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="01fe9-264">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="01fe9-264">Bind an array to a class</span></span>

<span data-ttu-id="01fe9-265">Поставщик может считывать значения конфигурации в массив для привязки к массиву POCO.</span><span class="sxs-lookup"><span data-stu-id="01fe9-265">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="01fe9-266">При чтении из источника конфигурации, позволяющего ключам содержать разделители с двоеточием (`:`), используется числовой сегмент ключа для различения ключей, составляющих массив (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="01fe9-266">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="01fe9-267">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="01fe9-267">`:{n}:`).</span></span> <span data-ttu-id="01fe9-268">Дополнительные сведения см. в разделе [Configuration: привязка массива к классу](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="01fe9-268">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="01fe9-269">Ключи Azure Key Vault не могут использовать двоеточие в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="01fe9-269">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="01fe9-270">Описанный в этом разделе подход использует двойные тире (`--`) в качестве разделителя иерархических значений (разделов).</span><span class="sxs-lookup"><span data-stu-id="01fe9-270">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="01fe9-271">Ключи массивов хранятся в Azure Key Vault с двойными тире и числовыми сегментами ключей (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="01fe9-271">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="01fe9-272">Изучите следующую конфигурацию поставщика ведения журнала [Serilog](https://serilog.net/) , предоставляемую файлом JSON.</span><span class="sxs-lookup"><span data-stu-id="01fe9-272">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="01fe9-273">В массиве `WriteTo` определены два объектных литерала, отражающих два *приемника*Serilog, которые описывают назначения для вывода журнала:</span><span class="sxs-lookup"><span data-stu-id="01fe9-273">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="01fe9-274">Конфигурация, показанная в предыдущем JSON-файле, хранится в Azure Key Vault с использованием нотации двойного тире (`--`) и числовых сегментов.</span><span class="sxs-lookup"><span data-stu-id="01fe9-274">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="01fe9-275">Key</span><span class="sxs-lookup"><span data-stu-id="01fe9-275">Key</span></span> | <span data-ttu-id="01fe9-276">{2&gt;Value&lt;2}</span><span class="sxs-lookup"><span data-stu-id="01fe9-276">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="01fe9-277">Перезагрузка секретов</span><span class="sxs-lookup"><span data-stu-id="01fe9-277">Reload secrets</span></span>

<span data-ttu-id="01fe9-278">Секреты кэшируются до тех пор, пока не будет вызван `IConfigurationRoot.Reload()`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-278">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="01fe9-279">Срок действия истек, отключен и обновленные секреты в хранилище ключей не учитываются приложением, пока не будет выполнено `Reload`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-279">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="01fe9-280">Отключенные секреты и секреты с истекшим сроком действия</span><span class="sxs-lookup"><span data-stu-id="01fe9-280">Disabled and expired secrets</span></span>

<span data-ttu-id="01fe9-281">Отключенные и просроченные секреты создают <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span><span class="sxs-lookup"><span data-stu-id="01fe9-281">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="01fe9-282">Чтобы предотвратить создание приложения, укажите конфигурацию с помощью другого поставщика конфигурации или обновите закрытый или истекший секрет.</span><span class="sxs-lookup"><span data-stu-id="01fe9-282">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="01fe9-283">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="01fe9-283">Troubleshoot</span></span>

<span data-ttu-id="01fe9-284">Если приложению не удается загрузить конфигурацию с помощью поставщика, в [инфраструктуру ведения журнала ASP.NET Core](xref:fundamentals/logging/index)записывается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="01fe9-284">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="01fe9-285">При следующих условиях загрузка конфигурации будет запрещена:</span><span class="sxs-lookup"><span data-stu-id="01fe9-285">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="01fe9-286">Приложение или сертификат неправильно настроены в Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="01fe9-286">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="01fe9-287">Хранилище ключей не существует в Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="01fe9-287">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="01fe9-288">Приложение не имеет прав доступа к хранилищу ключей.</span><span class="sxs-lookup"><span data-stu-id="01fe9-288">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="01fe9-289">Политика доступа не включает разрешения `Get` и `List`.</span><span class="sxs-lookup"><span data-stu-id="01fe9-289">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="01fe9-290">В хранилище ключей данные конфигурации (пара "имя-значение") неправильно именованы, отсутствуют, отключены или устарели.</span><span class="sxs-lookup"><span data-stu-id="01fe9-290">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="01fe9-291">Приложение имеет неправильное имя хранилища ключей (`KeyVaultName`), идентификатор приложения Azure AD (`AzureADApplicationId`) или отпечаток сертификата Azure AD (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="01fe9-291">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="01fe9-292">Неверный ключ конфигурации (имя) в приложении для загружаемого значения.</span><span class="sxs-lookup"><span data-stu-id="01fe9-292">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="01fe9-293">При добавлении политики доступа для приложения в хранилище ключей была создана политика, но кнопка **сохранить** не была выбрана в пользовательском интерфейсе **политик доступа** .</span><span class="sxs-lookup"><span data-stu-id="01fe9-293">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01fe9-294">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="01fe9-294">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="01fe9-295">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="01fe9-295">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="01fe9-296">Microsoft Azure: Документация по Key Vault</span><span class="sxs-lookup"><span data-stu-id="01fe9-296">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="01fe9-297">Создание и перенос ключей, защищенных АППАРАТным модулем безопасности, для Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="01fe9-297">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="01fe9-298">Класс KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="01fe9-298">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="01fe9-299">Краткое руководство. Установка и получение секрета из Azure Key Vault с помощью веб-приложения .NET</span><span class="sxs-lookup"><span data-stu-id="01fe9-299">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="01fe9-300">Руководство. Использование Azure Key Vault с виртуальной машиной Azure Windows в .NET</span><span class="sxs-lookup"><span data-stu-id="01fe9-300">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
