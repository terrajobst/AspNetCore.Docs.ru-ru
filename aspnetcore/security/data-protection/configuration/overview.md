---
title: Настройка защиты данных ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о настройке защиты данных в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: b2fa1921120d297f4b0dbb0294a00e0e573f4e04
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272583"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="cc903-103">Настройка защиты данных ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc903-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="cc903-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="cc903-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cc903-105">При инициализации системы защиты данных, оно применяется [параметры по умолчанию](xref:security/data-protection/configuration/default-settings) в зависимости от рабочей среды.</span><span class="sxs-lookup"><span data-stu-id="cc903-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="cc903-106">Эти параметры обычно подходят для приложений, выполняющихся на одном компьютере.</span><span class="sxs-lookup"><span data-stu-id="cc903-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="cc903-107">Существуют случаи, когда разработчик может потребоваться изменить параметры по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="cc903-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="cc903-108">Приложение распределены между несколькими компьютерами.</span><span class="sxs-lookup"><span data-stu-id="cc903-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="cc903-109">Для обеспечения соответствия.</span><span class="sxs-lookup"><span data-stu-id="cc903-109">For compliance reasons.</span></span>

<span data-ttu-id="cc903-110">В этих сценариях в системе защиты данных предлагает широкие возможности настройки API.</span><span class="sxs-lookup"><span data-stu-id="cc903-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="cc903-111">Как и файлы конфигурации, кольцо ключ защиты данных должны быть защищены с помощью соответствующих разрешений.</span><span class="sxs-lookup"><span data-stu-id="cc903-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="cc903-112">Можно выбрать для шифрования ключей при хранении, но это не помешает злоумышленники создание новых разделов.</span><span class="sxs-lookup"><span data-stu-id="cc903-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="cc903-113">Следовательно это повлияет на безопасность приложения.</span><span class="sxs-lookup"><span data-stu-id="cc903-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="cc903-114">Место хранения, настроенной с Data Protection должен иметь доступ к нему ограничением в состав самого приложения, аналогично тому, как можно будет защитить файлы конфигурации.</span><span class="sxs-lookup"><span data-stu-id="cc903-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="cc903-115">Например если вы решили хранить кольцо ваш ключ на диске, используйте разрешения файловой системы.</span><span class="sxs-lookup"><span data-stu-id="cc903-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="cc903-116">Убедитесь, идентификатор, под которой запущено приложение чтения, записи и создать доступ к этому каталогу.</span><span class="sxs-lookup"><span data-stu-id="cc903-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="cc903-117">При использовании табличного хранилища Azure, веб-приложения должен иметь возможность читать, записывать и создание новых записей в хранилище таблиц и т. д.</span><span class="sxs-lookup"><span data-stu-id="cc903-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="cc903-118">Метод расширения [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) возвращает [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="cc903-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="cc903-119">`IDataProtectionBuilder` Предоставляет методы расширения, что можно соединить в цепочку вместе для настройки защиты данных параметров.</span><span class="sxs-lookup"><span data-stu-id="cc903-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="cc903-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="cc903-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="cc903-121">Для хранения ключей в [хранилище ключей Azure](https://azure.microsoft.com/services/key-vault/), настройки системы с [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="cc903-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="cc903-122">Задайте расположение хранилища ключей (например, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="cc903-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="cc903-123">Необходимо задать расположение, так как вызов `ProtectKeysWithAzureKeyVault` реализует [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) , отключает автоматические данные параметры защиты, в том числе расположение хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="cc903-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="cc903-124">В предыдущем примере использовался хранилища больших двоичных объектов для сохранения ключей.</span><span class="sxs-lookup"><span data-stu-id="cc903-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="cc903-125">Дополнительные сведения см. в разделе [поставщики хранилища ключей: Azure и Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="cc903-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="cc903-126">Также можно сохранить ключей локально с [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="cc903-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="cc903-127">`keyIdentifier` — Идентификатор ключа хранилища ключей, используемый для ключа шифрования (например, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="cc903-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="cc903-128">`ProtectKeysWithAzureKeyVault` перегрузки:</span><span class="sxs-lookup"><span data-stu-id="cc903-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="cc903-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) позволяет использовать [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) системы защиты данных для хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="cc903-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="cc903-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, строка, строка, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) позволяет использовать `ClientId` и [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) системы защиты данных для хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="cc903-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="cc903-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, строка, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) позволяет использовать `ClientId` и `ClientSecret` для включения системы защиты данных для хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="cc903-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="cc903-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="cc903-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="cc903-133">Для хранения ключей в общем ресурсе UNC, а не на *% LOCALAPPDATA %* расположение по умолчанию, настроить систему с [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="cc903-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="cc903-134">При изменении расположения сохраняемость ключа системы шифрует больше не будет автоматически ключи неактивные, так как он не знает ли DPAPI — это механизм шифрования соответствующие.</span><span class="sxs-lookup"><span data-stu-id="cc903-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="cc903-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="cc903-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="cc903-136">Можно настроить систему для защиты ключей неактивные при вызовах [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) интерфейсы API настройки.</span><span class="sxs-lookup"><span data-stu-id="cc903-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="cc903-137">Рассмотрим следующий пример, который хранит ключи в общем ресурсе UNC и шифрует эти ключи хранятся с конкретным сертификатом X.509.</span><span class="sxs-lookup"><span data-stu-id="cc903-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="cc903-138">В разделе [ключ шифрования на Rest](xref:security/data-protection/implementation/key-encryption-at-rest) примеры и Дополнительные сведения о механизме встроенного ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="cc903-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="cc903-139">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="cc903-139">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="cc903-140">Чтобы настроить систему для использования время жизни ключа 14 дней вместо значения по умолчанию 90 дней, используйте [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="cc903-140">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="cc903-141">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="cc903-141">SetApplicationName</span></span>

<span data-ttu-id="cc903-142">По умолчанию система защиты данных изолирует приложения друг от друга, даже если они совместно используют тот же физический репозиторий ключа.</span><span class="sxs-lookup"><span data-stu-id="cc903-142">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="cc903-143">Это предотвращает понимание друг друга защищенных полезных данных приложения.</span><span class="sxs-lookup"><span data-stu-id="cc903-143">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="cc903-144">Для совместного использования защищенных полезных данных между двумя приложениями, используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) с тем же значением для каждого приложения:</span><span class="sxs-lookup"><span data-stu-id="cc903-144">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="cc903-145">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="cc903-145">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="cc903-146">Возможно, сценарий, где вы не хотите приложения автоматически развернуть ключи (для создания новых ключей), как приближающихся истечение срока действия.</span><span class="sxs-lookup"><span data-stu-id="cc903-146">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="cc903-147">Примером этого может быть в первичный или вторичный отношение, где только основной приложение отвечает за управление ключами проблемы и получателей приложения просто имеют только для чтения представление кольца ключ приложения.</span><span class="sxs-lookup"><span data-stu-id="cc903-147">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="cc903-148">Вторичный приложения можно настроить необходимо рассматривать ключей только для чтения, для настройки системы с [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="cc903-148">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="cc903-149">Изоляция на уровне приложения</span><span class="sxs-lookup"><span data-stu-id="cc903-149">Per-application isolation</span></span>

<span data-ttu-id="cc903-150">Когда система защиты данных предоставляется ASP.NET Core для узла, он автоматически изолирует приложений друг от друга, даже если эти приложения запущены под одной учетной записи рабочего процесса и используете же использований материала.</span><span class="sxs-lookup"><span data-stu-id="cc903-150">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="cc903-151">Это аналогично немного модификатора IsolateApps из System.Web  **\<machineKey >** элемента.</span><span class="sxs-lookup"><span data-stu-id="cc903-151">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="cc903-152">Механизм изоляции работает, учитывая каждое приложение на локальном компьютере как уникальный клиента таким образом [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) корневому каталогу для любого приложения автоматически включает в себя идентификатор приложения в качестве дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="cc903-152">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="cc903-153">Уникальный идентификатор приложения берутся из одного из двух мест:</span><span class="sxs-lookup"><span data-stu-id="cc903-153">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="cc903-154">Если приложение размещается в службах IIS, уникальный идентификатор является путь конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="cc903-154">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="cc903-155">Если приложение развертывается в среде веб-фермы, это значение должно быть стабильной при условии, что IIS средах аналогичным образом настроены на всех компьютерах веб-фермы.</span><span class="sxs-lookup"><span data-stu-id="cc903-155">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="cc903-156">Если приложение не размещено в службах IIS, уникальный идентификатор является физический путь приложения.</span><span class="sxs-lookup"><span data-stu-id="cc903-156">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="cc903-157">Уникальный идентификатор предназначен выдержать сбрасывает &mdash; как отдельные приложения и самой машины.</span><span class="sxs-lookup"><span data-stu-id="cc903-157">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="cc903-158">Этот механизм изоляции предполагается, что приложения не вредоносных.</span><span class="sxs-lookup"><span data-stu-id="cc903-158">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="cc903-159">Вредоносные приложения всегда может повлиять на другие приложения, выполняемых в рамках одной учетной записи рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="cc903-159">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="cc903-160">В общей среде размещения, где приложения являются взаимно без доверия поставщик услуг размещения необходимо предпринять действия для обеспечения ОС уровня изоляции между приложениями, включая Отделение приложений основного ключа репозиториев.</span><span class="sxs-lookup"><span data-stu-id="cc903-160">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="cc903-161">Если в системе защиты данных не указан ASP.NET Core для узла (например, в том случае, если его через экземпляр `DataProtectionProvider` конкретный тип) изоляция приложений отключена по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cc903-161">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="cc903-162">При отключении изоляции приложений, поддерживаемый же материала все приложения могут совместно использовать полезных данных при условии, что они предоставляют соответствующие [целей](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="cc903-162">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="cc903-163">Чтобы обеспечить изоляцию приложений в этой среде, вызовите [SetApplicationName](#setapplicationname) метод конфигурации объекта и введите уникальное имя для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="cc903-163">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="cc903-164">Изменение алгоритмов с UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="cc903-164">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="cc903-165">Стек защиты данных можно изменить алгоритм по умолчанию, используемые вновь создаваемых ключей.</span><span class="sxs-lookup"><span data-stu-id="cc903-165">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="cc903-166">Самый простой способ сделать это является вызов [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) из конфигурации обратного вызова:</span><span class="sxs-lookup"><span data-stu-id="cc903-166">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cc903-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cc903-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cc903-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cc903-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="cc903-169">Значение по умолчанию EncryptionAlgorithm AES-256-CBC, который по умолчанию ValidationAlgorithm HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="cc903-169">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="cc903-170">Политика по умолчанию можно задать системным администратором через [политики на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy), но явный вызов `UseCryptographicAlgorithms` переопределяет политику по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cc903-170">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="cc903-171">Вызов `UseCryptographicAlgorithms` можно указать требуемый алгоритм из списка предопределенных встроенные.</span><span class="sxs-lookup"><span data-stu-id="cc903-171">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="cc903-172">Не нужно беспокоиться о реализации этого алгоритма.</span><span class="sxs-lookup"><span data-stu-id="cc903-172">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="cc903-173">В приведенном выше сценарии система защиты данных предпринимает попытку использовать реализацию CNG алгоритма AES, при использовании ОС Windows.</span><span class="sxs-lookup"><span data-stu-id="cc903-173">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="cc903-174">В противном случае оно переключится на управляемый [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) класса.</span><span class="sxs-lookup"><span data-stu-id="cc903-174">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="cc903-175">Можно вручную указать реализацию через вызов [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="cc903-175">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="cc903-176">Изменение алгоритмов не влияет на существующие ключи в ключ обмена.</span><span class="sxs-lookup"><span data-stu-id="cc903-176">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="cc903-177">Он влияет только на вновь созданных ключей.</span><span class="sxs-lookup"><span data-stu-id="cc903-177">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="cc903-178">Указание пользовательских управляемых алгоритмов</span><span class="sxs-lookup"><span data-stu-id="cc903-178">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cc903-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cc903-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="cc903-180">Чтобы задать пользовательские управляемые алгоритмы, создать [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) экземпляра, который указывает типы реализации:</span><span class="sxs-lookup"><span data-stu-id="cc903-180">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cc903-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cc903-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="cc903-182">Чтобы задать пользовательские управляемые алгоритмы, создать [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) экземпляра, который указывает типы реализации:</span><span class="sxs-lookup"><span data-stu-id="cc903-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="cc903-183">Обычно \*тип свойства должен указывать на конкретный, допускающих создание экземпляров (через открытый конструктор без параметров) реализации [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) и [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), хотя Специальные случаи системы некоторые значения, например `typeof(Aes)` для удобства.</span><span class="sxs-lookup"><span data-stu-id="cc903-183">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="cc903-184">SymmetricAlgorithm должен иметь длину ключа 128 бит ≥ и размер блока ≥ 64-разрядная, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="cc903-184">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="cc903-185">KeyedHashAlgorithm должен иметь размер хэш-кода > = 128 бит, и он должен поддерживать ключи длина равна длине дайджест хэш-алгоритм.</span><span class="sxs-lookup"><span data-stu-id="cc903-185">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="cc903-186">KeyedHashAlgorithm не является обязательным для HMAC.</span><span class="sxs-lookup"><span data-stu-id="cc903-186">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="cc903-187">Указание пользовательские алгоритмы Windows CNG</span><span class="sxs-lookup"><span data-stu-id="cc903-187">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cc903-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cc903-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="cc903-189">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создать [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) , содержащий сведения, алгоритма:</span><span class="sxs-lookup"><span data-stu-id="cc903-189">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cc903-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cc903-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="cc903-191">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создать [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) , содержащий сведения, алгоритма:</span><span class="sxs-lookup"><span data-stu-id="cc903-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="cc903-192">Алгоритм шифрования симметричных блока должен иметь длину ключа > = 128 бит, размер блока > = 64 бита, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="cc903-192">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="cc903-193">Хэш-алгоритм должен иметь размер хэш-кода > = 128 бит и должны поддерживать, открытого в BCRYPT\_ALG\_ОБРАБОТКИ\_HMAC\_флаг ФЛАГ.</span><span class="sxs-lookup"><span data-stu-id="cc903-193">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="cc903-194">\*Поставщика свойства можно задать значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="cc903-194">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="cc903-195">В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="cc903-195">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cc903-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cc903-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="cc903-197">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) с проверкой, создать [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) , содержащий сведения, алгоритма:</span><span class="sxs-lookup"><span data-stu-id="cc903-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cc903-198">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cc903-198">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="cc903-199">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) с проверкой, создать [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) , содержащий сведения, алгоритма:</span><span class="sxs-lookup"><span data-stu-id="cc903-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="cc903-200">Алгоритм шифрования симметричных блока должен иметь длину ключа > = 128 бит, размер блока в точности 128 бит, и он должен поддерживать шифрование GCM.</span><span class="sxs-lookup"><span data-stu-id="cc903-200">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="cc903-201">Можно задать [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) свойство NULL, чтобы использовать поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="cc903-201">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="cc903-202">В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="cc903-202">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="cc903-203">Указание другие пользовательские алгоритмы</span><span class="sxs-lookup"><span data-stu-id="cc903-203">Specifying other custom algorithms</span></span>

<span data-ttu-id="cc903-204">Хотя не представлен как полноценные API, защиты данных являются расширяемыми, позволяющий указывать практически любой тип алгоритма.</span><span class="sxs-lookup"><span data-stu-id="cc903-204">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="cc903-205">Например можно сохранить все ключи, содержащиеся в модуле оборудования безопасности (HSM) и предоставить собственную реализацию основных подпрограммы шифрования и расшифровки.</span><span class="sxs-lookup"><span data-stu-id="cc903-205">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="cc903-206">В разделе [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) в [основы расширяемости шифрования](xref:security/data-protection/extensibility/core-crypto) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="cc903-206">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="cc903-207">Сохранение ключей при размещении в контейнер Docker</span><span class="sxs-lookup"><span data-stu-id="cc903-207">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="cc903-208">При размещении в [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) контейнера ключей следует хранить в либо:</span><span class="sxs-lookup"><span data-stu-id="cc903-208">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="cc903-209">Папки, тома Docker сохраняется вне пределов продолжительности контейнера, например общего тома или узла подключенного тома.</span><span class="sxs-lookup"><span data-stu-id="cc903-209">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="cc903-210">Внешних поставщиков, такие как [хранилище ключей Azure](https://azure.microsoft.com/services/key-vault/) или [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="cc903-210">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="cc903-211">См. также</span><span class="sxs-lookup"><span data-stu-id="cc903-211">See also</span></span>

* [<span data-ttu-id="cc903-212">Сценарии, не поддерживающие DI</span><span class="sxs-lookup"><span data-stu-id="cc903-212">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="cc903-213">Политика на уровне компьютера</span><span class="sxs-lookup"><span data-stu-id="cc903-213">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
