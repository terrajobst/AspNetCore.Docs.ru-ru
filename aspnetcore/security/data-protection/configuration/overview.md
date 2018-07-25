---
title: Настройка защиты данных в ASP.NET Core
author: rick-anderson
description: Узнайте, как настроить защиту данных в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: fd3e5dff114c81eae07186e735a15314d984dcc3
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243114"
---
# <a name="configure-aspnet-core-data-protection"></a>Настройка защиты данных в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

При инициализации системы защиты данных, оно применяется [параметры по умолчанию](xref:security/data-protection/configuration/default-settings) зависимости от рабочей среды. Эти параметры обычно подходят для приложений, выполняющихся на одном компьютере. Бывают случаи, где разработчик может потребоваться изменить параметры по умолчанию:

* Приложения распределены между несколькими компьютерами.
* Для обеспечения соответствия.

В таких случаях системы защиты данных предлагает широкие возможности настройки API.

> [!WARNING]
> Как и файлы конфигурации, набора ключей защиты данных должны быть защищены с помощью соответствующих разрешений. Можно выбрать для шифрования ключей при хранении, но это не предотвращает злоумышленники созданием новых ключей. Следовательно это повлияет на безопасность приложения. Место хранения, настроен с защитой данных должны иметь его доступ возможен только из самого, аналогично тому, как бы Защита файлов конфигурации приложения. Например если вы решили хранить на диске вашего набора ключей, используйте разрешения файловой системы. Убедитесь, удостоверение, под которой запущено приложение чтения, записи и доступа к этому каталогу. Если вы используете хранилище таблиц Azure, веб-приложения должны иметь возможность читать, записывать и создавать новые записи в хранилище таблиц и т. д.
>
> Метод расширения [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) возвращает [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder` Предоставляет методы расширения, что вы можете связать вместе, чтобы настроить защиту данных.

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

Для хранения ключей в [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), настройки системы с [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) в `Startup` класса:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Задать место хранения набора ключей (например, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)). Необходимо задать расположение, так как вызов `ProtectKeysWithAzureKeyVault` реализует [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) , отключает параметры защиты автоматические данные, включая место хранения набора ключей. Предыдущий пример использует хранилище BLOB-объектов для хранения набора ключей. Дополнительные сведения см. в разделе [поставщики хранилища ключей: Azure и Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis). Также можно сохранить локально с помощью набора ключей [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).

`keyIdentifier` Является идентификатор ключа хранилища ключей, используемый для шифрования ключа (например, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).

`ProtectKeysWithAzureKeyVault` перегрузки:

* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) позволяет использовать [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) системы защиты данных для использования хранилища ключей.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, строка, строка, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) позволяет использовать `ClientId` и [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) системы защиты данных для использования хранилища ключей.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, строки, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) позволяет использовать `ClientId` и `ClientSecret` для включения система защиты данных для использования хранилища ключей.

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Для хранения ключей UNC-ресурсе, а не в *% LOCALAPPDATA %* расположение по умолчанию, настроить систему с [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> При изменении ключа постоянном расположении, система шифрует больше не будет автоматически ключей при хранении, так как он не знает, является ли DPAPI соответствующего механизма шифрования.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

Можно настроить систему для защиты неактивных ключей можно вызвать любую из [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) интерфейсов API настройки. Рассмотрим пример ниже, в котором хранятся ключи на общем ресурсе UNC и шифрует эти ключи хранятся с конкретным сертификатом X.509:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

В ASP.NET Core 2.1 или более поздней версии, вы можете предоставить [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) для [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), такие как сертификат, загруженная из файла:

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

См. в разделе [ключ шифрования неактивных](xref:security/data-protection/implementation/key-encryption-at-rest) Дополнительные примеры и обсуждения на механизмы встроенного ключа шифрования.

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a>UnprotectKeysWithAnyCertificate

В ASP.NET Core 2.1 или более поздней версии, могут обеспечивать циркуляцию сертификатов и расшифровки ключей при хранении, используя массив [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) сертификаты с [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):

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

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Чтобы настроить систему для использования ключа времени существования 14 дней вместо значения по умолчанию 90 дней, используйте [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

По умолчанию система защиты данных изолирует приложений друг от друга, даже если они совместно используют тот же репозиторий физического ключа. Это предотвращает основные сведения о других защищенных полезных данных приложения. Для совместного использования защищенных полезных данных между двумя приложениями, используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) с одинаковым значением для каждого приложения:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Возможно, сценарий, где вы не хотите приложению автоматически менять ключи, (создание новых ключей), так как они подходят истечения срока действия. Примером этого может быть приложения, в связи первичного и вторичного, где только основное приложение отвечает за управление ключами задач и дополнительный приложения просто доступное только для чтения представление набора ключей. Вторичный приложения можно настроить необходимо рассматривать набора ключей только для чтения, настройки системы с [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Изоляция на уровне приложения

Когда система защиты данных предоставляется узлом ASP.NET Core, она автоматически изолирует приложений друг от друга, даже если эти приложения выполняются под одной учетной записи рабочего процесса и при использовании же материала главного ключа. Это отчасти напоминает модификатора IsolateApps из System.Web  **\<machineKey >** элемент.

Механизм изоляции работает путем оценки каждого приложения на локальном компьютере как уникальный клиент, таким образом [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) административного доступа для любого приложения автоматически включает в себя идентификатор приложения в качестве дискриминатора. Уникальный идентификатор приложения берутся из одного из двух мест:

1. Если приложение размещается в службах IIS, уникальный идентификатор — это путь конфигурации приложения. Если приложение развертывается в среде веб-фермы, это значение должно быть стабильная, при условии, что в средах службы IIS настраиваются сходным образом на всех компьютерах веб-фермы.

2. Если приложение не размещено в службах IIS, уникальный идентификатор — это физический путь приложения.

Уникальный идентификатор позволяет избежать простоев в случае сброса &mdash; как отдельные приложения и самой виртуальной машине.

Этот механизм изоляции предполагается, что приложения не являются вредоносными. Вредоносные приложения всегда может повлиять на любое другое приложение, под одной учетной записи рабочего процесса. В общей среде размещения, когда приложения являются взаимно без доверия поставщика услуг размещения принять меры для обеспечения изоляции на уровне операционной системы между приложениями, включая Отделение приложений основного ключа хранилища.

Если система защиты данных не предоставляется для узла ASP.NET Core (например, в том случае, если необходимо создать его с помощью `DataProtectionProvider` конкретный тип) изоляция приложений отключена по умолчанию. При отключении изоляции приложений, все приложения, с тем же материала ключа, могут использовать полезные данные до тех пор, пока они предоставляют соответствующие [целей](xref:security/data-protection/consumer-apis/purpose-strings). Чтобы обеспечить изоляцию приложения в этой среде, вызовите [SetApplicationName](#setapplicationname) метод конфигурации объекта и введите уникальное имя для каждого приложения.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Изменение алгоритмов с UseCryptographicAlgorithms

В стеке защиты данных можно изменить алгоритм по умолчанию, используемые вновь созданные ключи. Самый простой способ сделать это является вызов [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) из обратного вызова конфигурации:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

По умолчанию EncryptionAlgorithm AES-256-CBC, а значение по умолчанию ValidationAlgorithm — HMACSHA256. Политика по умолчанию можно задать с системным администратором через [политики на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy), но явный вызов `UseCryptographicAlgorithms` переопределяет политику по умолчанию.

Вызов `UseCryptographicAlgorithms` можно указать нужный алгоритм из предопределенного списка встроенных. Не нужно беспокоиться о реализации этого алгоритма. В сценарии выше система защиты данных пытается использовать реализацию CNG алгоритма AES, если под управлением Windows. В противном случае она возвращается к управляемой [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) класса.

Можно вручную указать реализацию через вызов [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Изменение алгоритмов не влияет на существующие ключи в связку ключей. Он влияет только на вновь созданные ключи.

### <a name="specifying-custom-managed-algorithms"></a>Указание пользовательских управляемых алгоритмов

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы указать пользовательские управляемые алгоритмы, создайте [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) экземпляр, который указывает типы реализации:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Чтобы указать пользовательские управляемые алгоритмы, создайте [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) экземпляр, который указывает типы реализации:

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

Обычно \*тип свойства должен указывать на конкретный, допускающий создание экземпляров (через открытый конструктор без параметров) реализации [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) и [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), хотя Специальные случаи системы некоторые значения, такие как `typeof(Aes)` для удобства.

> [!NOTE]
> SymmetricAlgorithm должен иметь длину ключа 128 бит ≥ и размер блока в ≥ 64 бита, и он должен поддерживать шифрование CBC режим заполнения PKCS #7. KeyedHashAlgorithm должен иметь размер хэш-кода > = 128 бит, и он должен поддерживать ключи равна длине дайджест хэш-алгоритма и длины. KeyedHashAlgorithm не строго обязательно должны быть HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>Указание пользовательские алгоритмы Windows CNG

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создайте [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) экземпляр, содержащий данные алгоритма:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создайте [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) экземпляр, содержащий данные алгоритма:

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
> Алгоритм симметричного блочного шифрования должен иметь длину ключа > = 128 бит, размер блока > = 64 бита, и он должен поддерживать шифрование CBC режим заполнения PKCS #7. Хэш-алгоритм, должен иметь размер хэш-кода из > 128 бит и должен поддерживать, открытого в BCRYPT\_ALG\_ОБРАБАТЫВАТЬ\_HMAC\_флаг ФЛАГ. \*Поставщика свойства можно задать значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма. См. в разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы задать пользовательский алгоритм Windows CNG с помощью счетчиков Galois режим шифрования с помощью проверки, создайте [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) экземпляр, содержащий данные алгоритма:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Чтобы задать пользовательский алгоритм Windows CNG с помощью счетчиков Galois режим шифрования с помощью проверки, создайте [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) экземпляр, содержащий данные алгоритма:

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
> Алгоритм симметричного блочного шифрования должен иметь длину ключа > = 128 бит, размер блока в точности 128 бит, и он должен поддерживать шифрование GCM. Можно задать [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) свойство значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма. См. в разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.

### <a name="specifying-other-custom-algorithms"></a>Указание другие пользовательские алгоритмы

Хотя не представлен как первоклассную API, система защиты данных достаточно расширяемым для того, чтобы обеспечить возможность указания практически любой тип алгоритма. Например можно сохранить все ключи, содержащиеся в модуле оборудования безопасности (HSM) и предоставить пользовательскую реализацию основные процедуры шифрования и расшифровки. См. в разделе [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) в [основы расширяемости шифрования](xref:security/data-protection/extensibility/core-crypto) Дополнительные сведения.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Сохранение ключей при размещении в контейнере Docker

При размещении в [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) контейнера, ключи следует хранить в одном:

* Папка, — это том Docker, который сохраняется вне пределов продолжительности контейнера, например общего тома или узла подключенного тома.
* Внешнего поставщика, таких как [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) или [Redis](https://redis.io/).

## <a name="see-also"></a>См. также

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
