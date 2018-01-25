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
# <a name="configuring-data-protection-in-aspnet-core"></a><span data-ttu-id="b1507-103">Настройка защиты данных в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1507-103">Configuring Data Protection in ASP.NET Core</span></span>

<span data-ttu-id="b1507-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="b1507-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b1507-105">При инициализации системы защиты данных, оно применяется [параметры по умолчанию](xref:security/data-protection/configuration/default-settings) в зависимости от рабочей среды.</span><span class="sxs-lookup"><span data-stu-id="b1507-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="b1507-106">Эти параметры обычно подходят для приложений, выполняющихся на одном компьютере.</span><span class="sxs-lookup"><span data-stu-id="b1507-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="b1507-107">Существуют случаи, когда разработчик может потребоваться изменить параметры по умолчанию, возможно, потому что их приложения распределены между несколькими компьютерами или соответствия требованиям.</span><span class="sxs-lookup"><span data-stu-id="b1507-107">There are cases where a developer may want to change the default settings, perhaps because their app is spread across multiple machines or for compliance reasons.</span></span> <span data-ttu-id="b1507-108">В этих сценариях в системе защиты данных предлагает широкие возможности настройки API.</span><span class="sxs-lookup"><span data-stu-id="b1507-108">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

<span data-ttu-id="b1507-109">Является методом расширения [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) , возвращающий [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="b1507-109">There's an extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) that returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="b1507-110">`IDataProtectionBuilder`Предоставляет методы расширения, что можно соединить в цепочку вместе для настройки защиты данных параметров.</span><span class="sxs-lookup"><span data-stu-id="b1507-110">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

## <a name="persistkeystofilesystem"></a><span data-ttu-id="b1507-111">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="b1507-111">PersistKeysToFileSystem</span></span>

<span data-ttu-id="b1507-112">Для хранения ключей в общем ресурсе UNC, а не на *% LOCALAPPDATA %* расположение по умолчанию, настроить систему с [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="b1507-112">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="b1507-113">При изменении расположения сохраняемость ключа системы шифрует больше не будет автоматически ключи неактивные, так как он не знает ли DPAPI — это механизм шифрования соответствующие.</span><span class="sxs-lookup"><span data-stu-id="b1507-113">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="b1507-114">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="b1507-114">ProtectKeysWith\*</span></span>

<span data-ttu-id="b1507-115">Можно настроить систему для защиты ключей неактивные при вызовах [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) интерфейсы API настройки.</span><span class="sxs-lookup"><span data-stu-id="b1507-115">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="b1507-116">Рассмотрим следующий пример, который хранит ключи в общем ресурсе UNC и шифрует эти ключи хранятся с конкретным сертификатом X.509.</span><span class="sxs-lookup"><span data-stu-id="b1507-116">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="b1507-117">В разделе [ключ шифрования на Rest](xref:security/data-protection/implementation/key-encryption-at-rest) примеры и Дополнительные сведения о механизме встроенного ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="b1507-117">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="b1507-118">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="b1507-118">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="b1507-119">Чтобы настроить систему для использования время жизни ключа 14 дней вместо значения по умолчанию 90 дней, используйте [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="b1507-119">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="b1507-120">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="b1507-120">SetApplicationName</span></span>

<span data-ttu-id="b1507-121">По умолчанию система защиты данных изолирует приложения друг от друга, даже если они совместно используют тот же физический репозиторий ключа.</span><span class="sxs-lookup"><span data-stu-id="b1507-121">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="b1507-122">Это предотвращает понимание друг друга защищенных полезных данных приложения.</span><span class="sxs-lookup"><span data-stu-id="b1507-122">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="b1507-123">Для совместного использования защищенных полезных данных между двумя приложениями, используйте [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) с тем же значением для каждого приложения:</span><span class="sxs-lookup"><span data-stu-id="b1507-123">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="b1507-124">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="b1507-124">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="b1507-125">Возможно, сценарий, где вы не хотите приложения автоматически развернуть ключи (для создания новых ключей), как приближающихся истечение срока действия.</span><span class="sxs-lookup"><span data-stu-id="b1507-125">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="b1507-126">Примером этого может быть в первичный или вторичный отношение, где только основной приложение отвечает за управление ключами проблемы и получателей приложения просто имеют только для чтения представление кольца ключ приложения.</span><span class="sxs-lookup"><span data-stu-id="b1507-126">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="b1507-127">Вторичный приложения можно настроить необходимо рассматривать ключей только для чтения, для настройки системы с [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="b1507-127">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="b1507-128">Изоляция на уровне приложения</span><span class="sxs-lookup"><span data-stu-id="b1507-128">Per-application isolation</span></span>

<span data-ttu-id="b1507-129">Когда система защиты данных предоставляется ASP.NET Core для узла, он автоматически изолирует приложений друг от друга, даже если эти приложения запущены под одной учетной записи рабочего процесса и используете же использований материала.</span><span class="sxs-lookup"><span data-stu-id="b1507-129">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="b1507-130">Это аналогично немного модификатора IsolateApps из System.Web  **\<machineKey >** элемента.</span><span class="sxs-lookup"><span data-stu-id="b1507-130">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="b1507-131">Механизм изоляции работает, учитывая каждое приложение на локальном компьютере как уникальный клиента таким образом [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) корневому каталогу для любого приложения автоматически включает в себя идентификатор приложения в качестве дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="b1507-131">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="b1507-132">Уникальный идентификатор приложения берутся из одного из двух мест:</span><span class="sxs-lookup"><span data-stu-id="b1507-132">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="b1507-133">Если приложение размещается в службах IIS, уникальный идентификатор является путь конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="b1507-133">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="b1507-134">Если приложение развертывается в среде веб-фермы, это значение должно быть стабильной при условии, что IIS средах аналогичным образом настроены на всех компьютерах веб-фермы.</span><span class="sxs-lookup"><span data-stu-id="b1507-134">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="b1507-135">Если приложение не размещено в службах IIS, уникальный идентификатор является физический путь приложения.</span><span class="sxs-lookup"><span data-stu-id="b1507-135">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="b1507-136">Уникальный идентификатор предназначен выдержать сбрасывает &mdash; как отдельные приложения и самой машины.</span><span class="sxs-lookup"><span data-stu-id="b1507-136">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="b1507-137">Этот механизм изоляции предполагается, что приложения не вредоносных.</span><span class="sxs-lookup"><span data-stu-id="b1507-137">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="b1507-138">Вредоносные приложения всегда может повлиять на другие приложения, выполняемых в рамках одной учетной записи рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="b1507-138">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="b1507-139">В общей среде размещения, где приложения являются взаимно без доверия поставщик услуг размещения необходимо предпринять действия для обеспечения ОС уровня изоляции между приложениями, включая Отделение приложений основного ключа репозиториев.</span><span class="sxs-lookup"><span data-stu-id="b1507-139">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="b1507-140">Если в системе защиты данных не указан ASP.NET Core для узла (например, в том случае, если его через экземпляр `DataProtectionProvider` конкретный тип) изоляция приложений отключена по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b1507-140">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="b1507-141">При отключении изоляции приложений, поддерживаемый же материала все приложения могут совместно использовать полезных данных при условии, что они предоставляют соответствующие [целей](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="b1507-141">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="b1507-142">Чтобы обеспечить изоляцию приложений в этой среде, вызовите [SetApplicationName](#setapplicationname) метод конфигурации объекта и введите уникальное имя для каждого приложения.</span><span class="sxs-lookup"><span data-stu-id="b1507-142">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="b1507-143">Изменение алгоритмов с UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="b1507-143">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="b1507-144">Стек защиты данных можно изменить алгоритм по умолчанию, используемые вновь создаваемых ключей.</span><span class="sxs-lookup"><span data-stu-id="b1507-144">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="b1507-145">Самый простой способ сделать это является вызов [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) из конфигурации обратного вызова:</span><span class="sxs-lookup"><span data-stu-id="b1507-145">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1507-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1507-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1507-147">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1507-147">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="b1507-148">Значение по умолчанию EncryptionAlgorithm AES-256-CBC, который по умолчанию ValidationAlgorithm HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="b1507-148">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="b1507-149">Политика по умолчанию можно задать системным администратором через [политики на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy), но явный вызов `UseCryptographicAlgorithms` переопределяет политику по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b1507-149">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="b1507-150">Вызов `UseCryptographicAlgorithms` можно указать требуемый алгоритм из списка предопределенных встроенные.</span><span class="sxs-lookup"><span data-stu-id="b1507-150">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="b1507-151">Не нужно беспокоиться о реализации этого алгоритма.</span><span class="sxs-lookup"><span data-stu-id="b1507-151">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="b1507-152">В приведенном выше сценарии система защиты данных предпринимает попытку использовать реализацию CNG алгоритма AES, при использовании ОС Windows.</span><span class="sxs-lookup"><span data-stu-id="b1507-152">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="b1507-153">В противном случае оно переключится на управляемый [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) класса.</span><span class="sxs-lookup"><span data-stu-id="b1507-153">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="b1507-154">Можно вручную указать реализацию через вызов [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="b1507-154">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="b1507-155">Изменение алгоритмов не влияет на существующие ключи в ключ обмена.</span><span class="sxs-lookup"><span data-stu-id="b1507-155">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="b1507-156">Он влияет только на вновь созданных ключей.</span><span class="sxs-lookup"><span data-stu-id="b1507-156">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="b1507-157">Указание пользовательских управляемых алгоритмов</span><span class="sxs-lookup"><span data-stu-id="b1507-157">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1507-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1507-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b1507-159">Чтобы задать пользовательские управляемые алгоритмы, создать [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) экземпляра, который указывает типы реализации:</span><span class="sxs-lookup"><span data-stu-id="b1507-159">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1507-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1507-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b1507-161">Чтобы задать пользовательские управляемые алгоритмы, создать [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) экземпляра, который указывает типы реализации:</span><span class="sxs-lookup"><span data-stu-id="b1507-161">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="b1507-162">Обычно \*тип свойства должен указывать на конкретный, допускающих создание экземпляров (через открытый конструктор без параметров) реализации [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) и [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), хотя Специальные случаи системы некоторые значения, например `typeof(Aes)` для удобства.</span><span class="sxs-lookup"><span data-stu-id="b1507-162">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="b1507-163">SymmetricAlgorithm должен иметь длину ключа 128 бит ≥ и размер блока ≥ 64-разрядная, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="b1507-163">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b1507-164">KeyedHashAlgorithm должен иметь размер хэш-кода > = 128 бит, и он должен поддерживать ключи длина равна длине дайджест хэш-алгоритм.</span><span class="sxs-lookup"><span data-stu-id="b1507-164">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="b1507-165">KeyedHashAlgorithm не является обязательным для HMAC.</span><span class="sxs-lookup"><span data-stu-id="b1507-165">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="b1507-166">Указание пользовательские алгоритмы Windows CNG</span><span class="sxs-lookup"><span data-stu-id="b1507-166">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1507-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1507-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b1507-168">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создать [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) , содержащий сведения, алгоритма:</span><span class="sxs-lookup"><span data-stu-id="b1507-168">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1507-169">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1507-169">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b1507-170">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC с проверкой HMAC, создать [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) , содержащий сведения, алгоритма:</span><span class="sxs-lookup"><span data-stu-id="b1507-170">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="b1507-171">Алгоритм шифрования симметричных блока должен иметь длину ключа > = 128 бит, размер блока > = 64 бита, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="b1507-171">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b1507-172">Хэш-алгоритм должен иметь размер хэш-кода > = 128 бит и должны поддерживать, открытого в BCRYPT\_ALG\_ОБРАБОТКИ\_HMAC\_флаг ФЛАГ.</span><span class="sxs-lookup"><span data-stu-id="b1507-172">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="b1507-173">\*Поставщика свойства можно задать значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="b1507-173">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b1507-174">В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="b1507-174">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1507-175">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1507-175">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b1507-176">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) с проверкой, создать [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) , содержащий сведения, алгоритма:</span><span class="sxs-lookup"><span data-stu-id="b1507-176">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1507-177">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1507-177">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b1507-178">Чтобы задать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) с проверкой, создать [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) , содержащий сведения, алгоритма:</span><span class="sxs-lookup"><span data-stu-id="b1507-178">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="b1507-179">Алгоритм шифрования симметричных блока должен иметь длину ключа > = 128 бит, размер блока в точности 128 бит, и он должен поддерживать шифрование GCM.</span><span class="sxs-lookup"><span data-stu-id="b1507-179">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="b1507-180">Можно задать [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) свойство NULL, чтобы использовать поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="b1507-180">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b1507-181">В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="b1507-181">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="b1507-182">Указание другие пользовательские алгоритмы</span><span class="sxs-lookup"><span data-stu-id="b1507-182">Specifying other custom algorithms</span></span>

<span data-ttu-id="b1507-183">Хотя не представлен как полноценные API, защиты данных являются расширяемыми, позволяющий указывать практически любой тип алгоритма.</span><span class="sxs-lookup"><span data-stu-id="b1507-183">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="b1507-184">Например можно сохранить все ключи, содержащиеся в модуле оборудования безопасности (HSM) и предоставить собственную реализацию основных подпрограммы шифрования и расшифровки.</span><span class="sxs-lookup"><span data-stu-id="b1507-184">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="b1507-185">В разделе [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) в [основы расширяемости шифрования](xref:security/data-protection/extensibility/core-crypto) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="b1507-185">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="b1507-186">Сохранение ключей при размещении в контейнер Docker</span><span class="sxs-lookup"><span data-stu-id="b1507-186">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="b1507-187">При размещении в [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) контейнера ключей следует хранить в либо:</span><span class="sxs-lookup"><span data-stu-id="b1507-187">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="b1507-188">Папки, тома Docker сохраняется вне пределов продолжительности контейнера, например общего тома или узла подключенного тома.</span><span class="sxs-lookup"><span data-stu-id="b1507-188">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="b1507-189">Внешних поставщиков, такие как [хранилище ключей Azure](https://azure.microsoft.com/services/key-vault/) или [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="b1507-189">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="b1507-190">См. также</span><span class="sxs-lookup"><span data-stu-id="b1507-190">See also</span></span>

* [<span data-ttu-id="b1507-191">Сценарии, не поддерживающие DI</span><span class="sxs-lookup"><span data-stu-id="b1507-191">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="b1507-192">Политика на уровне компьютера</span><span class="sxs-lookup"><span data-stu-id="b1507-192">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
