---
title: "Настройка защиты данных"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 39fab796c24456d61a6a103c4a3f7a8722b4718c
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-data-protection"></a><span data-ttu-id="a6b96-103">Настройка защиты данных</span><span class="sxs-lookup"><span data-stu-id="a6b96-103">Configuring data protection</span></span>

<a name=data-protection-configuring></a>

<span data-ttu-id="a6b96-104">При инициализации системы защиты данных применяет некоторые [параметры по умолчанию](default-settings.md#data-protection-default-settings) в зависимости от рабочей среды.</span><span class="sxs-lookup"><span data-stu-id="a6b96-104">When the data protection system is initialized it applies some [default settings](default-settings.md#data-protection-default-settings) based on the operational environment.</span></span> <span data-ttu-id="a6b96-105">Эти параметры обычно хорошо подходят для приложений, работающих на одном компьютере.</span><span class="sxs-lookup"><span data-stu-id="a6b96-105">These settings are generally good for applications running on a single machine.</span></span> <span data-ttu-id="a6b96-106">Существуют случаи, когда разработчик может потребоваться изменить эти (возможно потому, что их приложения распределены между несколькими компьютерами или соответствия требованиям), и для этих сценариев система защиты данных предлагает широкие возможности API конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a6b96-106">There are some cases where a developer may want to change these (perhaps because their application is spread across multiple machines or for compliance reasons), and for these scenarios the data protection system offers a rich configuration API.</span></span>

<a name=data-protection-configuration-callback></a>

<span data-ttu-id="a6b96-107">Отсутствует метод расширения AddDataProtection, возвращающий IDataProtectionBuilder который в свою очередь предоставляет методы расширения, что можно соединить в цепочку вместе для настройки различных защиты данных параметров.</span><span class="sxs-lookup"><span data-stu-id="a6b96-107">There is an extension method AddDataProtection which returns an IDataProtectionBuilder which itself exposes extension methods that you can chain together to configure various data protection options.</span></span> <span data-ttu-id="a6b96-108">Например для хранения ключей в UNC-ресурс вместо % LOCALAPPDATA % (по умолчанию), настройте систему следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a6b96-108">For instance, to store keys at a UNC share instead of %LOCALAPPDATA% (the default), configure the system as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> <span data-ttu-id="a6b96-109">При изменении расположения сохраняемость ключа система будет шифровать больше не будет автоматически ключи хранятся так как он не знает ли DPAPI — это механизм шифрования соответствующие.</span><span class="sxs-lookup"><span data-stu-id="a6b96-109">If you change the key persistence location, the system will no longer automatically encrypt keys at rest since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

<a name=configuring-x509-certificate></a>

<span data-ttu-id="a6b96-110">Можно настроить систему для защиты ключей неактивные вызовом ProtectKeysWith\* интерфейсы API настройки.</span><span class="sxs-lookup"><span data-stu-id="a6b96-110">You can configure the system to protect keys at rest by calling any of the ProtectKeysWith\* configuration APIs.</span></span> <span data-ttu-id="a6b96-111">Рассмотрим следующий пример, который хранит ключи в общем ресурсе UNC и шифрует эти ключи хранятся с конкретным сертификатом X.509.</span><span class="sxs-lookup"><span data-stu-id="a6b96-111">Consider the example below, which stores keys at a UNC share and encrypts those keys at rest with a specific X.509 certificate.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="a6b96-112">В разделе [шифрования ключа при хранении](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) Дополнительные примеры и сведения о механизме встроенного ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="a6b96-112">See [key encryption at rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more examples and for discussion on the built-in key encryption mechanisms.</span></span>

<span data-ttu-id="a6b96-113">Для настройки системы для использования времени жизни по умолчанию 14 дней вместо 90 дней, рассмотрим следующий пример:</span><span class="sxs-lookup"><span data-stu-id="a6b96-113">To configure the system to use a default key lifetime of 14 days instead of 90 days, consider the following example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

<span data-ttu-id="a6b96-114">По умолчанию система защиты данных изолирует приложений друг от друга, даже если они совместно используют тот же физический репозиторий ключа.</span><span class="sxs-lookup"><span data-stu-id="a6b96-114">By default the data protection system isolates applications from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="a6b96-115">Это предотвращает приложений понимание друг друга защищенных полезных данных.</span><span class="sxs-lookup"><span data-stu-id="a6b96-115">This prevents the applications from understanding each other's protected payloads.</span></span> <span data-ttu-id="a6b96-116">Для совместного использования защищенных полезных данных между двумя приложениями, настройки системы, передача в одно и то же имя приложения для обоих приложений, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="a6b96-116">To share protected payloads between two different applications, configure the system passing in the same application name for both applications as in the below example:</span></span>

<a name=data-protection-code-sample-application-name></a>

<!-- literal_block {"ids": ["data-protection-code-sample-application-name"], "linenos": false, "names": ["data-protection-code-sample-application-name"], "xml:space": "preserve", "language": "csharp"} -->

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name=data-protection-configuring-disable-automatic-key-generation></a>

<span data-ttu-id="a6b96-117">Наконец возможно, сценарий, где требуется приложение для автоматического развертывания ключей, как приближающихся истечение срока действия.</span><span class="sxs-lookup"><span data-stu-id="a6b96-117">Finally, you may have a scenario where you do not want an application to automatically roll keys as they approach expiration.</span></span> <span data-ttu-id="a6b96-118">Примером этого может быть в связи первичного и вторичного, где только основное приложение отвечает за вопросы управления ключами, а все приложения получателя просто имеют только для чтения представление кольца ключ приложения.</span><span class="sxs-lookup"><span data-stu-id="a6b96-118">One example of this might be applications set up in a primary / secondary relationship, where only the primary application is responsible for key management concerns, and all secondary applications simply have a read-only view of the key ring.</span></span> <span data-ttu-id="a6b96-119">Можно настроить получателей приложений необходимо рассматривать ключей только для чтения, настройки системы, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="a6b96-119">The secondary applications can be configured to treat the key ring as read-only by configuring the system as below:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name=data-protection-configuration-per-app-isolation></a>

## <a name="per-application-isolation"></a><span data-ttu-id="a6b96-120">Изоляция на уровне приложения</span><span class="sxs-lookup"><span data-stu-id="a6b96-120">Per-application isolation</span></span>

<span data-ttu-id="a6b96-121">Когда система защиты данных предоставляется ASP.NET Core для узла, он будет автоматически изолировать приложения друг от друга, даже если эти приложения запущены под одной учетной записи рабочего процесса и используете же использований материала.</span><span class="sxs-lookup"><span data-stu-id="a6b96-121">When the data protection system is provided by an ASP.NET Core host, it will automatically isolate applications from one another, even if those applications are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="a6b96-122">Это аналогично немного модификатора IsolateApps из System.Web <machineKey> элемента.</span><span class="sxs-lookup"><span data-stu-id="a6b96-122">This is somewhat similar to the IsolateApps modifier from System.Web's <machineKey> element.</span></span>

<span data-ttu-id="a6b96-123">Работает механизм изоляции, учитывая каждое приложение на локальном компьютере как уникальный клиента, таким образом IDataProtector, корнем для любого конкретного приложения, автоматически включает идентификатор приложения как дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="a6b96-123">The isolation mechanism works by considering each application on the local machine as a unique tenant, thus the IDataProtector rooted for any given application automatically includes the application ID as a discriminator.</span></span> <span data-ttu-id="a6b96-124">Уникальный идентификатор приложения поступают из одной из двух частей.</span><span class="sxs-lookup"><span data-stu-id="a6b96-124">The application's unique ID comes from one of two places.</span></span>

1. <span data-ttu-id="a6b96-125">Если приложение размещается в службах IIS, уникальный идентификатор является путь конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="a6b96-125">If the application is hosted in IIS, the unique identifier is the application's configuration path.</span></span> <span data-ttu-id="a6b96-126">Если приложение развертывается в среде фермы, это значение должно быть стабильной при условии, что IIS средах аналогичным образом настроены на всех компьютерах в ферме.</span><span class="sxs-lookup"><span data-stu-id="a6b96-126">If an application is deployed in a farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the farm.</span></span>

2. <span data-ttu-id="a6b96-127">Если приложение не размещается в службах IIS, уникальный идентификатор является физический путь приложения.</span><span class="sxs-lookup"><span data-stu-id="a6b96-127">If the application is not hosted in IIS, the unique identifier is the physical path of the application.</span></span>

<span data-ttu-id="a6b96-128">Уникальный идентификатор предназначен выдержать сбрасывает - как отдельное приложение и самой машины.</span><span class="sxs-lookup"><span data-stu-id="a6b96-128">The unique identifier is designed to survive resets - both of the individual application and of the machine itself.</span></span>

<span data-ttu-id="a6b96-129">Этот механизм изоляции предполагается, что приложения не вредоносных.</span><span class="sxs-lookup"><span data-stu-id="a6b96-129">This isolation mechanism assumes that the applications are not malicious.</span></span> <span data-ttu-id="a6b96-130">Вредоносное приложение всегда может повлиять на другие приложения, выполняемых в рамках одной учетной записи рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="a6b96-130">A malicious application can always impact any other application running under the same worker process account.</span></span> <span data-ttu-id="a6b96-131">В общей среде размещения, где приложения являются взаимно без доверия поставщик услуг размещения необходимо предпринять действия для обеспечения ОС уровня изоляции между приложениями, включая Отделение приложений основного ключа репозиториев.</span><span class="sxs-lookup"><span data-stu-id="a6b96-131">In a shared hosting environment where applications are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between applications, including separating the applications' underlying key repositories.</span></span>

<span data-ttu-id="a6b96-132">Если система защиты данных, не предоставляются ASP.NET Core для узла (например, если разработчик создает его себе через конкретный тип DataProtectionProvider) изоляции приложений отключена по умолчанию и все приложения резервного путем ввода материалы могут совместно использовать полезных данных, при условии, что они предоставляют соответствующие целей.</span><span class="sxs-lookup"><span data-stu-id="a6b96-132">If the data protection system is not provided by an ASP.NET Core host (e.g., if the developer instantiates it himself via the DataProtectionProvider concrete type), application isolation is disabled by default, and all applications backed by the same keying material can share payloads as long as they provide the appropriate purposes.</span></span> <span data-ttu-id="a6b96-133">Для обеспечения изоляции приложений в этой среде, вызовите метод SetApplicationName в объекте конфигурации см. в разделе [образец кода](#data-protection-code-sample-application-name) выше.</span><span class="sxs-lookup"><span data-stu-id="a6b96-133">To provide application isolation in this environment, call the SetApplicationName method on the configuration object, see the [code sample](#data-protection-code-sample-application-name) above.</span></span>

<a name=data-protection-changing-algorithms></a>

## <a name="changing-algorithms"></a><span data-ttu-id="a6b96-134">Изменение алгоритмов</span><span class="sxs-lookup"><span data-stu-id="a6b96-134">Changing algorithms</span></span>

<span data-ttu-id="a6b96-135">Стек защиты данных позволяет изменять алгоритм по умолчанию, используемые вновь создаваемых ключей.</span><span class="sxs-lookup"><span data-stu-id="a6b96-135">The data protection stack allows changing the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="a6b96-136">Самый простой способ сделать это является вызов UseCryptographicAlgorithms из конфигурации обратного вызова, как в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="a6b96-136">The simplest way to do this is to call UseCryptographicAlgorithms from the configuration callback, as in the below example.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6b96-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6b96-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6b96-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6b96-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="a6b96-139">По умолчанию EncryptionAlgorithm и ValidationAlgorithm — AES-256-CBC и HMACSHA256, соответственно.</span><span class="sxs-lookup"><span data-stu-id="a6b96-139">The default EncryptionAlgorithm and ValidationAlgorithm are AES-256-CBC and HMACSHA256, respectively.</span></span> <span data-ttu-id="a6b96-140">Политика по умолчанию можно задать системным администратором через [расширенные политики компьютера](machine-wide-policy.md), но явный вызов UseCryptographicAlgorithms будет переопределить политику по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a6b96-140">The default policy can be set by a system administrator via [Machine Wide Policy](machine-wide-policy.md), but an explicit call to UseCryptographicAlgorithms will override the default policy.</span></span>

<span data-ttu-id="a6b96-141">Вызов UseCryptographicAlgorithms позволяет разработчику указать нужный алгоритм (из стандартного списка встроенных), и разработчику не нужно беспокоиться о реализации этого алгоритма.</span><span class="sxs-lookup"><span data-stu-id="a6b96-141">Calling UseCryptographicAlgorithms will allow the developer to specify the desired algorithm (from a predefined built-in list), and the developer does not need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="a6b96-142">Например, в сценарии выше система защиты данных будет пытаться использовать реализацию CNG алгоритма AES при использовании ОС Windows, в противном случае она вернется к управляемого класса System.Security.Cryptography.Aes.</span><span class="sxs-lookup"><span data-stu-id="a6b96-142">For instance, in the scenario above the data protection system will attempt to use the CNG implementation of AES if running on Windows, otherwise it will fall back to the managed System.Security.Cryptography.Aes class.</span></span>

<span data-ttu-id="a6b96-143">Разработчик может указать реализацию, вручную при необходимости через вызов UseCustomCryptographicAlgorithms, как показано в ниже примерах.</span><span class="sxs-lookup"><span data-stu-id="a6b96-143">The developer can manually specify an implementation if desired via a call to UseCustomCryptographicAlgorithms, as show in the below examples.</span></span>

>[!TIP]
> <span data-ttu-id="a6b96-144">Изменение алгоритмов не влияет на существующие ключи в ключ обмена.</span><span class="sxs-lookup"><span data-stu-id="a6b96-144">Changing algorithms does not affect existing keys in the key ring.</span></span> <span data-ttu-id="a6b96-145">Он влияет только на вновь созданных ключей.</span><span class="sxs-lookup"><span data-stu-id="a6b96-145">It only affects newly-generated keys.</span></span>

<a name=data-protection-changing-algorithms-custom-managed></a>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="a6b96-146">Указание пользовательских управляемых алгоритмов</span><span class="sxs-lookup"><span data-stu-id="a6b96-146">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6b96-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6b96-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a6b96-148">Чтобы указать пользовательские управляемые алгоритмы, создайте экземпляр ManagedAuthenticatedEncryptorConfiguration, указывающий типы реализации.</span><span class="sxs-lookup"><span data-stu-id="a6b96-148">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptorConfiguration instance that points to the implementation types.</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptorConfiguration()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6b96-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6b96-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a6b96-150">Чтобы указать пользовательские управляемые алгоритмы, создайте экземпляр ManagedAuthenticatedEncryptionSettings, указывающий типы реализации.</span><span class="sxs-lookup"><span data-stu-id="a6b96-150">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptionSettings instance that points to the implementation types.</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptionSettings()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="a6b96-151">Обычно \*тип свойства должен указывать на конкретный, допускающих создание экземпляров (через открытый конструктор без параметров) реализации SymmetricAlgorithm и KeyedHashAlgorithm, хотя специальные варианты системы некоторые значения, например typeof(Aes) для удобства .</span><span class="sxs-lookup"><span data-stu-id="a6b96-151">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of SymmetricAlgorithm and KeyedHashAlgorithm, though the system special-cases some values like typeof(Aes) for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="a6b96-152">SymmetricAlgorithm должен иметь длину ключа 128 бит ≥ и размер блока ≥ 64-разрядная, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="a6b96-152">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="a6b96-153">KeyedHashAlgorithm должен иметь размер хэш-кода > = 128 бит, и он должен поддерживать ключи длина равна длине дайджест хэш-алгоритм.</span><span class="sxs-lookup"><span data-stu-id="a6b96-153">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="a6b96-154">KeyedHashAlgorithm не является обязательным для HMAC.</span><span class="sxs-lookup"><span data-stu-id="a6b96-154">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

<a name=data-protection-changing-algorithms-cng></a>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="a6b96-155">Указание пользовательские алгоритмы Windows CNG</span><span class="sxs-lookup"><span data-stu-id="a6b96-155">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6b96-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6b96-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a6b96-157">Чтобы указать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC + проверки HMAC, создайте экземпляр CngCbcAuthenticatedEncryptorConfiguration алгоритмической сведениями.</span><span class="sxs-lookup"><span data-stu-id="a6b96-157">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6b96-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6b96-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a6b96-159">Чтобы указать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC + проверки HMAC, создайте экземпляр CngCbcAuthenticatedEncryptionSettings алгоритмической сведениями.</span><span class="sxs-lookup"><span data-stu-id="a6b96-159">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="a6b96-160">Алгоритм шифрования симметричных блока должен иметь длину ключа 128 бит ≥ и размер блока ≥ 64-разрядная, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="a6b96-160">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="a6b96-161">Хэш-алгоритм должен иметь размер хэш-кода > = 128 бит и должны поддерживать открываемого с флагом BCRYPT_ALG_HANDLE_HMAC_FLAG.</span><span class="sxs-lookup"><span data-stu-id="a6b96-161">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT_ALG_HANDLE_HMAC_FLAG flag.</span></span> <span data-ttu-id="a6b96-162">\*Поставщика свойства можно задать значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="a6b96-162">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="a6b96-163">В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="a6b96-163">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6b96-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6b96-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a6b96-165">Чтобы указать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) + проверки, создайте экземпляр CngGcmAuthenticatedEncryptorConfiguration алгоритмической сведениями.</span><span class="sxs-lookup"><span data-stu-id="a6b96-165">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6b96-166">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6b96-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a6b96-167">Чтобы указать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) + проверки, создайте экземпляр CngGcmAuthenticatedEncryptionSettings алгоритмической сведениями.</span><span class="sxs-lookup"><span data-stu-id="a6b96-167">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="a6b96-168">Алгоритм шифрования симметричных блока должен иметь длину ключа 128 бит ≥ и размер блока в точности 128 бит, и он должен поддерживать шифрование GCM.</span><span class="sxs-lookup"><span data-stu-id="a6b96-168">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="a6b96-169">Свойство EncryptionAlgorithmProvider может быть присвоено значение null для использования поставщика по умолчанию для указанного алгоритма.</span><span class="sxs-lookup"><span data-stu-id="a6b96-169">The EncryptionAlgorithmProvider property can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="a6b96-170">В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="a6b96-170">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="a6b96-171">Указание другие пользовательские алгоритмы</span><span class="sxs-lookup"><span data-stu-id="a6b96-171">Specifying other custom algorithms</span></span>

<span data-ttu-id="a6b96-172">На то, что не представлен как полноценные API, система защиты данных является расширяемой, чтобы обеспечить возможность указания практически любой тип алгоритма.</span><span class="sxs-lookup"><span data-stu-id="a6b96-172">Though not exposed as a first-class API, the data protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="a6b96-173">Например можно сохранить все ключи, содержащиеся в аппаратный модуль безопасности и предоставить собственную реализацию основных подпрограммы шифрования и расшифровки.</span><span class="sxs-lookup"><span data-stu-id="a6b96-173">For example, it is possible to keep all keys contained within an HSM and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="a6b96-174">В разделе IAuthenticatedEncryptorConfiguration в разделе Основные криптографии расширяемости для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="a6b96-174">See IAuthenticatedEncryptorConfiguration in the core cryptography extensibility section for more information.</span></span>

### <a name="see-also"></a><span data-ttu-id="a6b96-175">См. также</span><span class="sxs-lookup"><span data-stu-id="a6b96-175">See also</span></span>

* [<span data-ttu-id="a6b96-176">Зависимые сценарии не DI</span><span class="sxs-lookup"><span data-stu-id="a6b96-176">Non DI Aware Scenarios</span></span>](non-di-scenarios.md)
* [<span data-ttu-id="a6b96-177">Расширенные политики компьютера</span><span class="sxs-lookup"><span data-stu-id="a6b96-177">Machine Wide Policy</span></span>](machine-wide-policy.md)
