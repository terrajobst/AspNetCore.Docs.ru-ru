---
title: Настройка защиты данных в ASP.NET Core
author: rick-anderson
description: Узнайте, как настроить защиту данных в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: ee43427fa1e82a365d49df50567b4ca7afb5a5d3
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897301"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="8e22e-103">Настройка защиты данных в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e22e-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="8e22e-104">При инициализации системы защиты данных, оно применяется [параметры по умолчанию](xref:security/data-protection/configuration/default-settings) зависимости от рабочей среды.</span><span class="sxs-lookup"><span data-stu-id="8e22e-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="8e22e-105">Эти параметры обычно подходят для приложений, выполняющихся на одном компьютере.</span><span class="sxs-lookup"><span data-stu-id="8e22e-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="8e22e-106">Бывают случаи, где разработчик может потребоваться изменить параметры по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="8e22e-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="8e22e-107">Приложения распределены между несколькими компьютерами.</span><span class="sxs-lookup"><span data-stu-id="8e22e-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="8e22e-108">Для обеспечения соответствия.</span><span class="sxs-lookup"><span data-stu-id="8e22e-108">For compliance reasons.</span></span>

<span data-ttu-id="8e22e-109">В таких случаях системы защиты данных предлагает широкие возможности настройки API.</span><span class="sxs-lookup"><span data-stu-id="8e22e-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="8e22e-110">Как и файлы конфигурации, набора ключей защиты данных должны быть защищены с помощью соответствующих разрешений.</span><span class="sxs-lookup"><span data-stu-id="8e22e-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="8e22e-111">Можно выбрать для шифрования ключей при хранении, но это не предотвращает злоумышленники созданием новых ключей.</span><span class="sxs-lookup"><span data-stu-id="8e22e-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="8e22e-112">Следовательно это повлияет на безопасность приложения.</span><span class="sxs-lookup"><span data-stu-id="8e22e-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="8e22e-113">Место хранения, настроен с защитой данных должны иметь его доступ возможен только из самого, аналогично тому, как бы Защита файлов конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="8e22e-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="8e22e-114">Например если вы решили хранить на диске вашего набора ключей, используйте разрешения файловой системы.</span><span class="sxs-lookup"><span data-stu-id="8e22e-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="8e22e-115">Убедитесь, удостоверение, под которой запущено приложение чтения, записи и доступа к этому каталогу.</span><span class="sxs-lookup"><span data-stu-id="8e22e-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="8e22e-116">Если вы используете хранилище таблиц Azure, веб-приложения должны иметь возможность читать, записывать и создавать новые записи в хранилище таблиц и т. д.</span><span class="sxs-lookup"><span data-stu-id="8e22e-116">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="8e22e-117">Метод расширения [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) возвращает [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="8e22e-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="8e22e-118">`IDataProtectionBuilder` Предоставляет методы расширения, что вы можете связать вместе, чтобы настроить защиту данных.</span><span class="sxs-lookup"><span data-stu-id="8e22e-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="8e22e-119">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="8e22e-119">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="8e22e-120">Для хранения ключей в [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), настройки системы с [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="8e22e-120">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="8e22e-121">Задать место хранения набора ключей (например, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="8e22e-121">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="8e22e-122">Необходимо задать расположение, так как вызов `ProtectKeysWithAzureKeyVault` реализует [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) , отключает параметры защиты автоматические данные, включая место хранения набора ключей.</span><span class="sxs-lookup"><span data-stu-id="8e22e-122">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="8e22e-123">Предыдущий пример использует хранилище BLOB-объектов для хранения набора ключей.</span><span class="sxs-lookup"><span data-stu-id="8e22e-123">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="8e22e-124">Дополнительные сведения см. в разделе [поставщики хранилища ключей: Azure и Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="8e22e-124">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="8e22e-125">Также можно сохранить локально с помощью набора ключей [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="8e22e-125">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="8e22e-126">`keyIdentifier` Является идентификатор ключа хранилища ключей, используемый для шифрования ключа.</span><span class="sxs-lookup"><span data-stu-id="8e22e-126">The `keyIdentifier` is the key vault key identifier used for key encryption.</span></span> <span data-ttu-id="8e22e-127">Например, ключ, созданный в хранилище ключей с именем `dataprotection` в `contosokeyvault` имеет идентификатор ключа `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span><span class="sxs-lookup"><span data-stu-id="8e22e-127">For example, a key created in key vault named `dataprotection` in the `contosokeyvault` has the key identifier `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span></span> <span data-ttu-id="8e22e-128">Укажите приложение с **распаковку ключа** и **Wrap Key** разрешений в хранилище ключей.</span><span class="sxs-lookup"><span data-stu-id="8e22e-128">Provide the app with **Unwrap Key** and **Wrap Key** permissions to the key vault.</span></span>

<span data-ttu-id="8e22e-129">`ProtectKeysWithAzureKeyVault` перегрузки:</span><span class="sxs-lookup"><span data-stu-id="8e22e-129">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="8e22e-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) позволяет использовать [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) системы защиты данных для использования хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="8e22e-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="8e22e-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, строка, строка, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) позволяет использовать `ClientId` и [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) системы защиты данных для использования хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="8e22e-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="8e22e-132">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, строки, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) позволяет использовать `ClientId` и `ClientSecret` для включения система защиты данных для использования хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="8e22e-132">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="8e22e-133">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="8e22e-133">PersistKeysToFileSystem</span></span>

<span data-ttu-id="8e22e-134">Для хранения ключей UNC-ресурсе, а не в *% LOCALAPPDATA %* расположение по умолчанию, настроить систему с [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="8e22e-134">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="8e22e-135">При изменении ключа постоянном расположении, система шифрует больше не будет автоматически ключей при хранении, так как он не знает, является ли DPAPI соответствующего механизма шифрования.</span><span class="sxs-lookup"><span data-stu-id="8e22e-135">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="8e22e-136">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="8e22e-136">ProtectKeysWith\*</span></span>

<span data-ttu-id="8e22e-137">Можно настроить систему для защиты неактивных ключей можно вызвать любую из [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) интерфейсов API настройки.</span><span class="sxs-lookup"><span data-stu-id="8e22e-137">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="8e22e-138">Рассмотрим пример ниже, в котором хранятся ключи на общем ресурсе UNC и шифрует эти ключи хранятся с конкретным сертификатом X.509:</span><span class="sxs-lookup"><span data-stu-id="8e22e-138">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8e22e-139">В ASP.NET Core 2.1 или более поздней версии, вы можете предоставить [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) для [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), такие как сертификат, загруженная из файла:</span><span class="sxs-lookup"><span data-stu-id="8e22e-139">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

<span data-ttu-id="8e22e-140">См. в разделе [ключ шифрования неактивных](xref:security/data-protection/implementation/key-encryption-at-rest) Дополнительные примеры и обсуждения на механизмы встроенного ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="8e22e-140">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="8e22e-141">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="8e22e-141">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="8e22e-142">В ASP.NET Core 2.1 или более поздней версии, могут обеспечивать циркуляцию сертификатов и расшифровки ключей при хранении, используя массив [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) сертификаты с [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="8e22e-142">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="8e22e-143">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="8e22e-143">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="8e22e-144">Чтобы настроить систему для использования ключа времени существования 14 дней вместо значения по умолчанию 90 дней, используйте [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="8e22e-144">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="8e22e-145">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="8e22e-145">SetApplicationName</span></span>

<span data-ttu-id="8e22e-146">По умолчанию система защиты данных изолирует приложений друг от друга в зависимости от их содержимого корневого пути, даже если они совместно используют тот же репозиторий физического ключа.</span><span class="sxs-lookup"><span data-stu-id="8e22e-146">By default, the Data Protection system isolates apps from one another based on their content root paths, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="8e22e-147">Это предотвращает основные сведения о других защищенных полезных данных приложения.</span><span class="sxs-lookup"><span data-stu-id="8e22e-147">This prevents the apps from understanding each other's protected payloads.</span></span>

<span data-ttu-id="8e22e-148">Совместное использование защищенных полезных данных между приложениями:</span><span class="sxs-lookup"><span data-stu-id="8e22e-148">To share protected payloads among apps:</span></span>

* <span data-ttu-id="8e22e-149">Настройка <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> в каждом приложении, с тем же значением.</span><span class="sxs-lookup"><span data-stu-id="8e22e-149">Configure <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in each app with the same value.</span></span>
* <span data-ttu-id="8e22e-150">Используют ту же версию API защиты данных стека между этими приложениями.</span><span class="sxs-lookup"><span data-stu-id="8e22e-150">Use the same version of the Data Protection API stack across the apps.</span></span> <span data-ttu-id="8e22e-151">Выполните **либо** из следующих в файлах проектов приложений:</span><span class="sxs-lookup"><span data-stu-id="8e22e-151">Perform **either** of the following in the apps' project files:</span></span>
  * <span data-ttu-id="8e22e-152">Ссылаться на той же версии общей платформы, с помощью [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8e22e-152">Reference the same shared framework version via the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="8e22e-153">Ссылаются на тот же [защиты данных пакета](xref:security/data-protection/introduction#package-layout) версии.</span><span class="sxs-lookup"><span data-stu-id="8e22e-153">Reference the same [Data Protection package](xref:security/data-protection/introduction#package-layout) version.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="8e22e-154">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="8e22e-154">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="8e22e-155">Возможно, сценарий, где вы не хотите приложению автоматически менять ключи, (создание новых ключей), так как они подходят истечения срока действия.</span><span class="sxs-lookup"><span data-stu-id="8e22e-155">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="8e22e-156">Примером этого может быть приложения, в связи первичного и вторичного, где только основное приложение отвечает за управление ключами задач и дополнительный приложения просто доступное только для чтения представление набора ключей.</span><span class="sxs-lookup"><span data-stu-id="8e22e-156">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="8e22e-157">Вторичный приложения можно настроить необходимо рассматривать набора ключей только для чтения, настройки системы с <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span><span class="sxs-lookup"><span data-stu-id="8e22e-157">The secondary apps can be configured to treat the key ring as read-only by configuring the system with <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="8e22e-158">Изоляция на уровне приложения</span><span class="sxs-lookup"><span data-stu-id="8e22e-158">Per-application isolation</span></span>

<span data-ttu-id="8e22e-159">Когда система защиты данных предоставляется узлом ASP.NET Core, она автоматически изолирует приложений друг от друга, даже если эти приложения выполняются под одной учетной записи рабочего процесса и при использовании же материала главного ключа.</span><span class="sxs-lookup"><span data-stu-id="8e22e-159">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="8e22e-160">Это отчасти напоминает модификатора IsolateApps из System.Web `<machineKey>` элемент.</span><span class="sxs-lookup"><span data-stu-id="8e22e-160">This is somewhat similar to the IsolateApps modifier from System.Web's `<machineKey>` element.</span></span>

<span data-ttu-id="8e22e-161">Механизм изоляции работает путем оценки каждого приложения на локальном компьютере как уникальный клиент, таким образом <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> административного доступа для любого приложения автоматически включает в себя идентификатор приложения в качестве дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="8e22e-161">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="8e22e-162">Уникальный идентификатор приложения — это физический путь приложения:</span><span class="sxs-lookup"><span data-stu-id="8e22e-162">The app's unique ID is the app's physical path:</span></span>

* <span data-ttu-id="8e22e-163">Для приложений, размещенных в [IIS](xref:fundamentals/servers/index#iis-http-server), уникальный идентификатор — это физический путь приложения IIS.</span><span class="sxs-lookup"><span data-stu-id="8e22e-163">For apps hosted in [IIS](xref:fundamentals/servers/index#iis-http-server), the unique ID is the IIS physical path of the app.</span></span> <span data-ttu-id="8e22e-164">Если приложение развертывается в среде веб-фермы, это значение стабильна, при условии, что в средах службы IIS настраиваются сходным образом на всех компьютерах веб-фермы.</span><span class="sxs-lookup"><span data-stu-id="8e22e-164">If an app is deployed in a web farm environment, this value is stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>
* <span data-ttu-id="8e22e-165">Для резидентных приложений, запущенных в [сервер Kestrel](xref:fundamentals/servers/index#kestrel), уникальный идентификатор — это физический путь к приложению на диске.</span><span class="sxs-lookup"><span data-stu-id="8e22e-165">For self-hosted apps running on the [Kestrel server](xref:fundamentals/servers/index#kestrel), the unique ID is the physical path to the app on disk.</span></span>

<span data-ttu-id="8e22e-166">Уникальный идентификатор позволяет избежать простоев в случае сброса&mdash;как отдельные приложения и самой виртуальной машине.</span><span class="sxs-lookup"><span data-stu-id="8e22e-166">The unique identifier is designed to survive resets&mdash;both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="8e22e-167">Этот механизм изоляции предполагается, что приложения не являются вредоносными.</span><span class="sxs-lookup"><span data-stu-id="8e22e-167">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="8e22e-168">Вредоносные приложения всегда может повлиять на любое другое приложение, под одной учетной записи рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="8e22e-168">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="8e22e-169">В общей среде размещения, когда приложения являются взаимно без доверия поставщика услуг размещения принять меры для обеспечения изоляции на уровне операционной системы между приложениями, включая Отделение приложений основного ключа хранилища.</span><span class="sxs-lookup"><span data-stu-id="8e22e-169">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="8e22e-170">Если система защиты данных не предоставляется для узла ASP.NET Core (например, в том случае, если необходимо создать его с помощью `DataProtectionProvider` конкретный тип) изоляция приложений отключена по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8e22e-170">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="8e22e-171">При отключении изоляции приложений, все приложения, с тем же материала ключа, могут использовать полезные данные до тех пор, пока они предоставляют соответствующие [целей](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="8e22e-171">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="8e22e-172">Чтобы обеспечить изоляцию приложения в этой среде, вызовите [SetApplicationName](#setapplicationname) метод конфигурации объекта и введите уникальное имя для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="8e22e-172">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="8e22e-173">Изменение алгоритмов с UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="8e22e-173">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="8e22e-174">В стеке защиты данных можно изменить алгоритм по умолчанию, используемые вновь созданные ключи.</span><span class="sxs-lookup"><span data-stu-id="8e22e-174">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="8e22e-175">Самый простой способ сделать это является вызов [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) из обратного вызова конфигурации:</span><span class="sxs-lookup"><span data-stu-id="8e22e-175">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

<span data-ttu-id="8e22e-176">По умолчанию EncryptionAlgorithm AES-256-CBC, а значение по умолчанию ValidationAlgorithm — HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="8e22e-176">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="8e22e-177">Политика по умолчанию можно задать с системным администратором через [политики на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy), но явный вызов `UseCryptographicAlgorithms` переопределяет политику по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8e22e-177">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="8e22e-178">Вызов `UseCryptographicAlgorithms` можно указать нужный алгоритм из предопределенного списка встроенных.</span><span class="sxs-lookup"><span data-stu-id="8e22e-178">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="8e22e-179">Не нужно беспокоиться о реализации этого алгоритма.</span><span class="sxs-lookup"><span data-stu-id="8e22e-179">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="8e22e-180">В сценарии выше система защиты данных пытается использовать реализацию CNG алгоритма AES, если под управлением Windows.</span><span class="sxs-lookup"><span data-stu-id="8e22e-180">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="8e22e-181">В противном случае она возвращается к управляемой [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) класса.</span><span class="sxs-lookup"><span data-stu-id="8e22e-181">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="8e22e-182">Можно вручную указать реализацию через вызов [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="8e22e-182">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="8e22e-183">Изменение алгоритмов не влияет на существующие ключи в связку ключей.</span><span class="sxs-lookup"><span data-stu-id="8e22e-183">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="8e22e-184">Он влияет только на вновь созданные ключи.</span><span class="sxs-lookup"><span data-stu-id="8e22e-184">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="8e22e-185">Указание пользовательских управляемых алгоритмов</span><span class="sxs-lookup"><span data-stu-id="8e22e-185">Specifying custom managed algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8e22e-186">Чтобы указать пользовательские управляемые алгоритмы, создайте [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) экземпляр, который указывает типы реализации:</span><span class="sxs-lookup"><span data-stu-id="8e22e-186">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8e22e-187">Чтобы указать пользовательские управляемые алгоритмы, создайте [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) экземпляр, который указывает типы реализации:</span><span class="sxs-lookup"><span data-stu-id="8e22e-187">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

<span data-ttu-id="8e22e-188">Обычно \*тип свойства должен указывать на конкретный, допускающий создание экземпляров (через открытый конструктор без параметров) реализации [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) и [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), хотя Специальные случаи системы некоторые значения, такие как `typeof(Aes)` для удобства.</span><span class="sxs-lookup"><span data-stu-id="8e22e-188">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="8e22e-189">SymmetricAlgorithm должен иметь длину ключа 128 бит ≥ и размер блока в ≥ 64 бита, и он должен поддерживать шифрование CBC режим заполнения PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="8e22e-189">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="8e22e-190">KeyedHashAlgorithm должен иметь размер хэш-кода > = 128 бит, и он должен поддерживать ключи равна длине дайджест хэш-алгоритма и длины.</span><span class="sxs-lookup"><span data-stu-id="8e22e-190">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="8e22e-191">KeyedHashAlgorithm не строго обязательно должны быть HMAC.</span><span class="sxs-lookup"><span data-stu-id="8e22e-191">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="8e22e-192">Указание пользовательские алгоритмы Windows CNG</span><span class="sxs-lookup"><span data-stu-id="8e22e-192">Specifying custom Windows CNG algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8e22e-193">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создайте [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) экземпляр, содержащий данные алгоритма:</span><span class="sxs-lookup"><span data-stu-id="8e22e-193">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8e22e-194">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создайте [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) экземпляр, содержащий данные алгоритма:</span><span class="sxs-lookup"><span data-stu-id="8e22e-194">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="8e22e-195">Алгоритм симметричного блочного шифрования должен иметь длину ключа > = 128 бит, размер блока > = 64 бита, и он должен поддерживать шифрование CBC режим заполнения PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="8e22e-195">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="8e22e-196">Хэш-алгоритм, должен иметь размер хэш-кода из > 128 бит и должен поддерживать, открытого в BCRYPT\_ALG\_ОБРАБАТЫВАТЬ\_HMAC\_флаг ФЛАГ.</span><span class="sxs-lookup"><span data-stu-id="8e22e-196">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="8e22e-197">\*Поставщика свойства можно задать значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="8e22e-197">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="8e22e-198">См. в разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="8e22e-198">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8e22e-199">Чтобы задать пользовательский алгоритм Windows CNG с помощью счетчиков Galois режим шифрования с помощью проверки, создайте [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) экземпляр, содержащий данные алгоритма:</span><span class="sxs-lookup"><span data-stu-id="8e22e-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8e22e-200">Чтобы задать пользовательский алгоритм Windows CNG с помощью счетчиков Galois режим шифрования с помощью проверки, создайте [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) экземпляр, содержащий данные алгоритма:</span><span class="sxs-lookup"><span data-stu-id="8e22e-200">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="8e22e-201">Алгоритм симметричного блочного шифрования должен иметь длину ключа > = 128 бит, размер блока в точности 128 бит, и он должен поддерживать шифрование GCM.</span><span class="sxs-lookup"><span data-stu-id="8e22e-201">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="8e22e-202">Можно задать [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) свойство значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="8e22e-202">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="8e22e-203">См. в разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="8e22e-203">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="8e22e-204">Указание другие пользовательские алгоритмы</span><span class="sxs-lookup"><span data-stu-id="8e22e-204">Specifying other custom algorithms</span></span>

<span data-ttu-id="8e22e-205">Хотя не представлен как первоклассную API, система защиты данных достаточно расширяемым для того, чтобы обеспечить возможность указания практически любой тип алгоритма.</span><span class="sxs-lookup"><span data-stu-id="8e22e-205">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="8e22e-206">Например можно сохранить все ключи, содержащиеся в модуле оборудования безопасности (HSM) и предоставить пользовательскую реализацию основные процедуры шифрования и расшифровки.</span><span class="sxs-lookup"><span data-stu-id="8e22e-206">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="8e22e-207">См. в разделе [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) в [основы расширяемости шифрования](xref:security/data-protection/extensibility/core-crypto) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="8e22e-207">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="8e22e-208">Сохранение ключей при размещении в контейнере Docker</span><span class="sxs-lookup"><span data-stu-id="8e22e-208">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="8e22e-209">При размещении в [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) контейнера, ключи следует хранить в одном:</span><span class="sxs-lookup"><span data-stu-id="8e22e-209">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="8e22e-210">Папка, — это том Docker, который сохраняется вне пределов продолжительности контейнера, например общего тома или узла подключенного тома.</span><span class="sxs-lookup"><span data-stu-id="8e22e-210">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="8e22e-211">Внешнего поставщика, таких как [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) или [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="8e22e-211">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e22e-212">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8e22e-212">Additional resources</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
