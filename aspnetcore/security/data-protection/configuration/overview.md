---
title: Настройка защиты данных в ASP.NET Core
author: rick-anderson
description: Узнайте, как настроить защиту данных в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: f3cac3541ffe633886f82cec8180a219272c24d6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095604"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="0e2c8-103">Настройка защиты данных в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e2c8-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="0e2c8-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="0e2c8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0e2c8-105">При инициализации системы защиты данных, оно применяется [параметры по умолчанию](xref:security/data-protection/configuration/default-settings) зависимости от рабочей среды.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="0e2c8-106">Эти параметры обычно подходят для приложений, выполняющихся на одном компьютере.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="0e2c8-107">Бывают случаи, где разработчик может потребоваться изменить параметры по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="0e2c8-108">Приложения распределены между несколькими компьютерами.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="0e2c8-109">Для обеспечения соответствия.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-109">For compliance reasons.</span></span>

<span data-ttu-id="0e2c8-110">В таких случаях системы защиты данных предлагает широкие возможности настройки API.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="0e2c8-111">Как и файлы конфигурации, набора ключей защиты данных должны быть защищены с помощью соответствующих разрешений.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="0e2c8-112">Можно выбрать для шифрования ключей при хранении, но это не предотвращает злоумышленники созданием новых ключей.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="0e2c8-113">Следовательно это повлияет на безопасность приложения.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="0e2c8-114">Место хранения, настроен с защитой данных должны иметь его доступ возможен только из самого, аналогично тому, как бы Защита файлов конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="0e2c8-115">Например если вы решили хранить на диске вашего набора ключей, используйте разрешения файловой системы.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="0e2c8-116">Убедитесь, удостоверение, под которой запущено приложение чтения, записи и доступа к этому каталогу.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="0e2c8-117">Если вы используете хранилище таблиц Azure, веб-приложения должны иметь возможность читать, записывать и создавать новые записи в хранилище таблиц и т. д.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="0e2c8-118">Метод расширения [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) возвращает [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="0e2c8-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="0e2c8-119">`IDataProtectionBuilder` Предоставляет методы расширения, что вы можете связать вместе, чтобы настроить защиту данных.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="0e2c8-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="0e2c8-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="0e2c8-121">Для хранения ключей в [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), настройки системы с [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="0e2c8-122">Задать место хранения набора ключей (например, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="0e2c8-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="0e2c8-123">Необходимо задать расположение, так как вызов `ProtectKeysWithAzureKeyVault` реализует [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) , отключает параметры защиты автоматические данные, включая место хранения набора ключей.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="0e2c8-124">Предыдущий пример использует хранилище BLOB-объектов для хранения набора ключей.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="0e2c8-125">Дополнительные сведения см. в разделе [поставщики хранилища ключей: Azure и Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="0e2c8-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="0e2c8-126">Также можно сохранить локально с помощью набора ключей [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="0e2c8-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="0e2c8-127">`keyIdentifier` Является идентификатор ключа хранилища ключей, используемый для шифрования ключа (например, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="0e2c8-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="0e2c8-128">`ProtectKeysWithAzureKeyVault` перегрузки:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="0e2c8-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) позволяет использовать [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) системы защиты данных для использования хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="0e2c8-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, строка, строка, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) позволяет использовать `ClientId` и [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) системы защиты данных для использования хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="0e2c8-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, строки, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) позволяет использовать `ClientId` и `ClientSecret` для включения система защиты данных для использования хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="0e2c8-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="0e2c8-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="0e2c8-133">Для хранения ключей UNC-ресурсе, а не в *% LOCALAPPDATA %* расположение по умолчанию, настроить систему с [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="0e2c8-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="0e2c8-134">При изменении ключа постоянном расположении, система шифрует больше не будет автоматически ключей при хранении, так как он не знает, является ли DPAPI соответствующего механизма шифрования.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="0e2c8-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="0e2c8-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="0e2c8-136">Можно настроить систему для защиты неактивных ключей можно вызвать любую из [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) интерфейсов API настройки.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="0e2c8-137">Рассмотрим пример ниже, в котором хранятся ключи на общем ресурсе UNC и шифрует эти ключи хранятся с конкретным сертификатом X.509:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="0e2c8-138">См. в разделе [ключ шифрования неактивных](xref:security/data-protection/implementation/key-encryption-at-rest) Дополнительные примеры и обсуждения на механизмы встроенного ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="0e2c8-139">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="0e2c8-139">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="0e2c8-140">Чтобы настроить систему для использования ключа времени существования 14 дней вместо значения по умолчанию 90 дней, используйте [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="0e2c8-140">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="0e2c8-141">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="0e2c8-141">SetApplicationName</span></span>

<span data-ttu-id="0e2c8-142">По умолчанию система защиты данных изолирует приложений друг от друга, даже если они совместно используют тот же репозиторий физического ключа.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-142">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="0e2c8-143">Это предотвращает основные сведения о других защищенных полезных данных приложения.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-143">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="0e2c8-144">Для совместного использования защищенных полезных данных между двумя приложениями, используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) с одинаковым значением для каждого приложения:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-144">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="0e2c8-145">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="0e2c8-145">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="0e2c8-146">Возможно, сценарий, где вы не хотите приложению автоматически менять ключи, (создание новых ключей), так как они подходят истечения срока действия.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-146">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="0e2c8-147">Примером этого может быть приложения, в связи первичного и вторичного, где только основное приложение отвечает за управление ключами задач и дополнительный приложения просто доступное только для чтения представление набора ключей.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-147">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="0e2c8-148">Вторичный приложения можно настроить необходимо рассматривать набора ключей только для чтения, настройки системы с [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="0e2c8-148">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="0e2c8-149">Изоляция на уровне приложения</span><span class="sxs-lookup"><span data-stu-id="0e2c8-149">Per-application isolation</span></span>

<span data-ttu-id="0e2c8-150">Когда система защиты данных предоставляется узлом ASP.NET Core, она автоматически изолирует приложений друг от друга, даже если эти приложения выполняются под одной учетной записи рабочего процесса и при использовании же материала главного ключа.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-150">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="0e2c8-151">Это отчасти напоминает модификатора IsolateApps из System.Web  **\<machineKey >** элемент.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-151">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="0e2c8-152">Механизм изоляции работает путем оценки каждого приложения на локальном компьютере как уникальный клиент, таким образом [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) административного доступа для любого приложения автоматически включает в себя идентификатор приложения в качестве дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-152">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="0e2c8-153">Уникальный идентификатор приложения берутся из одного из двух мест:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-153">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="0e2c8-154">Если приложение размещается в службах IIS, уникальный идентификатор — это путь конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-154">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="0e2c8-155">Если приложение развертывается в среде веб-фермы, это значение должно быть стабильная, при условии, что в средах службы IIS настраиваются сходным образом на всех компьютерах веб-фермы.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-155">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="0e2c8-156">Если приложение не размещено в службах IIS, уникальный идентификатор — это физический путь приложения.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-156">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="0e2c8-157">Уникальный идентификатор позволяет избежать простоев в случае сброса &mdash; как отдельные приложения и самой виртуальной машине.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-157">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="0e2c8-158">Этот механизм изоляции предполагается, что приложения не являются вредоносными.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-158">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="0e2c8-159">Вредоносные приложения всегда может повлиять на любое другое приложение, под одной учетной записи рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-159">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="0e2c8-160">В общей среде размещения, когда приложения являются взаимно без доверия поставщика услуг размещения принять меры для обеспечения изоляции на уровне операционной системы между приложениями, включая Отделение приложений основного ключа хранилища.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-160">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="0e2c8-161">Если система защиты данных не предоставляется для узла ASP.NET Core (например, в том случае, если необходимо создать его с помощью `DataProtectionProvider` конкретный тип) изоляция приложений отключена по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-161">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="0e2c8-162">При отключении изоляции приложений, все приложения, с тем же материала ключа, могут использовать полезные данные до тех пор, пока они предоставляют соответствующие [целей](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="0e2c8-162">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="0e2c8-163">Чтобы обеспечить изоляцию приложения в этой среде, вызовите [SetApplicationName](#setapplicationname) метод конфигурации объекта и введите уникальное имя для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-163">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="0e2c8-164">Изменение алгоритмов с UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="0e2c8-164">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="0e2c8-165">В стеке защиты данных можно изменить алгоритм по умолчанию, используемые вновь созданные ключи.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-165">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="0e2c8-166">Самый простой способ сделать это является вызов [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) из обратного вызова конфигурации:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-166">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0e2c8-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0e2c8-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0e2c8-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0e2c8-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="0e2c8-169">По умолчанию EncryptionAlgorithm AES-256-CBC, а значение по умолчанию ValidationAlgorithm — HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-169">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="0e2c8-170">Политика по умолчанию можно задать с системным администратором через [политики на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy), но явный вызов `UseCryptographicAlgorithms` переопределяет политику по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-170">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="0e2c8-171">Вызов `UseCryptographicAlgorithms` можно указать нужный алгоритм из предопределенного списка встроенных.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-171">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="0e2c8-172">Не нужно беспокоиться о реализации этого алгоритма.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-172">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="0e2c8-173">В сценарии выше система защиты данных пытается использовать реализацию CNG алгоритма AES, если под управлением Windows.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-173">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="0e2c8-174">В противном случае она возвращается к управляемой [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) класса.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-174">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="0e2c8-175">Можно вручную указать реализацию через вызов [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="0e2c8-175">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="0e2c8-176">Изменение алгоритмов не влияет на существующие ключи в связку ключей.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-176">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="0e2c8-177">Он влияет только на вновь созданные ключи.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-177">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="0e2c8-178">Указание пользовательских управляемых алгоритмов</span><span class="sxs-lookup"><span data-stu-id="0e2c8-178">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0e2c8-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0e2c8-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0e2c8-180">Чтобы указать пользовательские управляемые алгоритмы, создайте [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) экземпляр, который указывает типы реализации:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-180">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0e2c8-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0e2c8-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0e2c8-182">Чтобы указать пользовательские управляемые алгоритмы, создайте [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) экземпляр, который указывает типы реализации:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

---

<span data-ttu-id="0e2c8-183">Обычно \*тип свойства должен указывать на конкретный, допускающий создание экземпляров (через открытый конструктор без параметров) реализации [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) и [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), хотя Специальные случаи системы некоторые значения, такие как `typeof(Aes)` для удобства.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-183">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="0e2c8-184">SymmetricAlgorithm должен иметь длину ключа 128 бит ≥ и размер блока в ≥ 64 бита, и он должен поддерживать шифрование CBC режим заполнения PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-184">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="0e2c8-185">KeyedHashAlgorithm должен иметь размер хэш-кода > = 128 бит, и он должен поддерживать ключи равна длине дайджест хэш-алгоритма и длины.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-185">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="0e2c8-186">KeyedHashAlgorithm не строго обязательно должны быть HMAC.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-186">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="0e2c8-187">Указание пользовательские алгоритмы Windows CNG</span><span class="sxs-lookup"><span data-stu-id="0e2c8-187">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0e2c8-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0e2c8-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0e2c8-189">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создайте [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) экземпляр, содержащий данные алгоритма:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-189">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0e2c8-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0e2c8-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0e2c8-191">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создайте [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) экземпляр, содержащий данные алгоритма:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

---

> [!NOTE]
> <span data-ttu-id="0e2c8-192">Алгоритм симметричного блочного шифрования должен иметь длину ключа > = 128 бит, размер блока > = 64 бита, и он должен поддерживать шифрование CBC режим заполнения PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-192">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="0e2c8-193">Хэш-алгоритм, должен иметь размер хэш-кода из > 128 бит и должен поддерживать, открытого в BCRYPT\_ALG\_ОБРАБАТЫВАТЬ\_HMAC\_флаг ФЛАГ.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-193">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="0e2c8-194">\*Поставщика свойства можно задать значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-194">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="0e2c8-195">См. в разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-195">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0e2c8-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0e2c8-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0e2c8-197">Чтобы задать пользовательский алгоритм Windows CNG с помощью счетчиков Galois режим шифрования с помощью проверки, создайте [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) экземпляр, содержащий данные алгоритма:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0e2c8-198">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0e2c8-198">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0e2c8-199">Чтобы задать пользовательский алгоритм Windows CNG с помощью счетчиков Galois режим шифрования с помощью проверки, создайте [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) экземпляр, содержащий данные алгоритма:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

---

> [!NOTE]
> <span data-ttu-id="0e2c8-200">Алгоритм симметричного блочного шифрования должен иметь длину ключа > = 128 бит, размер блока в точности 128 бит, и он должен поддерживать шифрование GCM.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-200">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="0e2c8-201">Можно задать [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) свойство значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-201">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="0e2c8-202">См. в разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-202">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="0e2c8-203">Указание другие пользовательские алгоритмы</span><span class="sxs-lookup"><span data-stu-id="0e2c8-203">Specifying other custom algorithms</span></span>

<span data-ttu-id="0e2c8-204">Хотя не представлен как первоклассную API, система защиты данных достаточно расширяемым для того, чтобы обеспечить возможность указания практически любой тип алгоритма.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-204">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="0e2c8-205">Например можно сохранить все ключи, содержащиеся в модуле оборудования безопасности (HSM) и предоставить пользовательскую реализацию основные процедуры шифрования и расшифровки.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-205">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="0e2c8-206">См. в разделе [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) в [основы расширяемости шифрования](xref:security/data-protection/extensibility/core-crypto) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-206">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="0e2c8-207">Сохранение ключей при размещении в контейнере Docker</span><span class="sxs-lookup"><span data-stu-id="0e2c8-207">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="0e2c8-208">При размещении в [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) контейнера, ключи следует хранить в одном:</span><span class="sxs-lookup"><span data-stu-id="0e2c8-208">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="0e2c8-209">Папка, — это том Docker, который сохраняется вне пределов продолжительности контейнера, например общего тома или узла подключенного тома.</span><span class="sxs-lookup"><span data-stu-id="0e2c8-209">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="0e2c8-210">Внешнего поставщика, таких как [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) или [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="0e2c8-210">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="0e2c8-211">См. также</span><span class="sxs-lookup"><span data-stu-id="0e2c8-211">See also</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
