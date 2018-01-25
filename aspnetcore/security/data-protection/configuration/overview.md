---
title: "Настройка защиты данных в ASP.NET Core"
author: rick-anderson
description: "Дополнительные сведения о настройке защиты данных в ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 07/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: b560f360b4962f2d50e0fdc9a99ab389eb113f43
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="configuring-data-protection-in-aspnet-core"></a>Настройка защиты данных в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

При инициализации системы защиты данных, оно применяется [параметры по умолчанию](xref:security/data-protection/configuration/default-settings) в зависимости от рабочей среды. Эти параметры обычно подходят для приложений, выполняющихся на одном компьютере. Существуют случаи, когда разработчик может потребоваться изменить параметры по умолчанию, возможно, потому что их приложения распределены между несколькими компьютерами или соответствия требованиям. В этих сценариях в системе защиты данных предлагает широкие возможности настройки API.

Является методом расширения [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) , возвращающий [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder`Предоставляет методы расширения, что можно соединить в цепочку вместе для настройки защиты данных параметров.

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Для хранения ключей в общем ресурсе UNC, а не на *% LOCALAPPDATA %* расположение по умолчанию, настроить систему с [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> При изменении расположения сохраняемость ключа системы шифрует больше не будет автоматически ключи неактивные, так как он не знает ли DPAPI — это механизм шифрования соответствующие.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

Можно настроить систему для защиты ключей неактивные при вызовах [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) интерфейсы API настройки. Рассмотрим следующий пример, который хранит ключи в общем ресурсе UNC и шифрует эти ключи хранятся с конкретным сертификатом X.509.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

В разделе [ключ шифрования на Rest](xref:security/data-protection/implementation/key-encryption-at-rest) примеры и Дополнительные сведения о механизме встроенного ключа шифрования.

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Чтобы настроить систему для использования время жизни ключа 14 дней вместо значения по умолчанию 90 дней, используйте [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

По умолчанию система защиты данных изолирует приложения друг от друга, даже если они совместно используют тот же физический репозиторий ключа. Это предотвращает понимание друг друга защищенных полезных данных приложения. Для совместного использования защищенных полезных данных между двумя приложениями, используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) с тем же значением для каждого приложения:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Возможно, сценарий, где вы не хотите приложения автоматически развернуть ключи (для создания новых ключей), как приближающихся истечение срока действия. Примером этого может быть в первичный или вторичный отношение, где только основной приложение отвечает за управление ключами проблемы и получателей приложения просто имеют только для чтения представление кольца ключ приложения. Вторичный приложения можно настроить необходимо рассматривать ключей только для чтения, для настройки системы с [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Изоляция на уровне приложения

Когда система защиты данных предоставляется ASP.NET Core для узла, он автоматически изолирует приложений друг от друга, даже если эти приложения запущены под одной учетной записи рабочего процесса и используете же использований материала. Это аналогично немного модификатора IsolateApps из System.Web  **\<machineKey >** элемента.

Механизм изоляции работает, учитывая каждое приложение на локальном компьютере как уникальный клиента таким образом [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) корневому каталогу для любого приложения автоматически включает в себя идентификатор приложения в качестве дискриминатора. Уникальный идентификатор приложения берутся из одного из двух мест:

1. Если приложение размещается в службах IIS, уникальный идентификатор является путь конфигурации приложения. Если приложение развертывается в среде веб-фермы, это значение должно быть стабильной при условии, что IIS средах аналогичным образом настроены на всех компьютерах веб-фермы.

2. Если приложение не размещено в службах IIS, уникальный идентификатор является физический путь приложения.

Уникальный идентификатор предназначен выдержать сбрасывает &mdash; как отдельные приложения и самой машины.

Этот механизм изоляции предполагается, что приложения не вредоносных. Вредоносные приложения всегда может повлиять на другие приложения, выполняемых в рамках одной учетной записи рабочего процесса. В общей среде размещения, где приложения являются взаимно без доверия поставщик услуг размещения необходимо предпринять действия для обеспечения ОС уровня изоляции между приложениями, включая Отделение приложений основного ключа репозиториев.

Если в системе защиты данных не указан ASP.NET Core для узла (например, в том случае, если его через экземпляр `DataProtectionProvider` конкретный тип) изоляция приложений отключена по умолчанию. При отключении изоляции приложений, поддерживаемый же материала все приложения могут совместно использовать полезных данных при условии, что они предоставляют соответствующие [целей](xref:security/data-protection/consumer-apis/purpose-strings). Чтобы обеспечить изоляцию приложений в этой среде, вызовите [SetApplicationName](#setapplicationname) метод конфигурации объекта и введите уникальное имя для каждого приложения.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Изменение алгоритмов с UseCryptographicAlgorithms

Стек защиты данных можно изменить алгоритм по умолчанию, используемые вновь создаваемых ключей. Самый простой способ сделать это является вызов [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) из конфигурации обратного вызова:

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

Значение по умолчанию EncryptionAlgorithm AES-256-CBC, который по умолчанию ValidationAlgorithm HMACSHA256. Политика по умолчанию можно задать системным администратором через [политики на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy), но явный вызов `UseCryptographicAlgorithms` переопределяет политику по умолчанию.

Вызов `UseCryptographicAlgorithms` можно указать требуемый алгоритм из списка предопределенных встроенные. Не нужно беспокоиться о реализации этого алгоритма. В приведенном выше сценарии система защиты данных предпринимает попытку использовать реализацию CNG алгоритма AES, при использовании ОС Windows. В противном случае оно переключится на управляемый [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) класса.

Можно вручную указать реализацию через вызов [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Изменение алгоритмов не влияет на существующие ключи в ключ обмена. Он влияет только на вновь созданных ключей.

### <a name="specifying-custom-managed-algorithms"></a>Указание пользовательских управляемых алгоритмов

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы задать пользовательские управляемые алгоритмы, создать [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) экземпляра, который указывает типы реализации:

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

Чтобы задать пользовательские управляемые алгоритмы, создать [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) экземпляра, который указывает типы реализации:

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

Обычно \*тип свойства должен указывать на конкретный, допускающих создание экземпляров (через открытый конструктор без параметров) реализации [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) и [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), хотя Специальные случаи системы некоторые значения, например `typeof(Aes)` для удобства.

> [!NOTE]
> SymmetricAlgorithm должен иметь длину ключа 128 бит ≥ и размер блока ≥ 64-разрядная, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7. KeyedHashAlgorithm должен иметь размер хэш-кода > = 128 бит, и он должен поддерживать ключи длина равна длине дайджест хэш-алгоритм. KeyedHashAlgorithm не является обязательным для HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>Указание пользовательские алгоритмы Windows CNG

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создать [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) , содержащий сведения, алгоритма:

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

Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создать [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) , содержащий сведения, алгоритма:

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
> Алгоритм шифрования симметричных блока должен иметь длину ключа > = 128 бит, размер блока > = 64 бита, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7. Хэш-алгоритм должен иметь размер хэш-кода > = 128 бит и должны поддерживать, открытого в BCRYPT\_ALG\_ОБРАБОТКИ\_HMAC\_флаг ФЛАГ. \*Поставщика свойства можно задать значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма. В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) с проверкой, создать [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) , содержащий сведения, алгоритма:

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

Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) с проверкой, создать [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) , содержащий сведения, алгоритма:

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
> Алгоритм шифрования симметричных блока должен иметь длину ключа > = 128 бит, размер блока в точности 128 бит, и он должен поддерживать шифрование GCM. Можно задать [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) свойство NULL, чтобы использовать поставщика по умолчанию для указанного алгоритма. В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.

### <a name="specifying-other-custom-algorithms"></a>Указание другие пользовательские алгоритмы

Хотя не представлен как полноценные API, защиты данных являются расширяемыми, позволяющий указывать практически любой тип алгоритма. Например можно сохранить все ключи, содержащиеся в модуле оборудования безопасности (HSM) и предоставить собственную реализацию основных подпрограммы шифрования и расшифровки. В разделе [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) в [основы расширяемости шифрования](xref:security/data-protection/extensibility/core-crypto) для получения дополнительной информации.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Сохранение ключей при размещении в контейнер Docker

При размещении в [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) контейнера ключей следует хранить в либо:

* Папки, тома Docker сохраняется вне пределов продолжительности контейнера, например общего тома или узла подключенного тома.
* Внешних поставщиков, такие как [хранилище ключей Azure](https://azure.microsoft.com/services/key-vault/) или [Redis](https://redis.io/).

## <a name="see-also"></a>См. также

* [Сценарии, не поддерживающие DI](xref:security/data-protection/configuration/non-di-scenarios)
* [Политика на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy)
