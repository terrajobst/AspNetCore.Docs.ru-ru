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
# <a name="configuring-data-protection"></a>Настройка защиты данных

<a name=data-protection-configuring></a>

При инициализации системы защиты данных применяет некоторые [параметры по умолчанию](default-settings.md#data-protection-default-settings) в зависимости от рабочей среды. Эти параметры обычно хорошо подходят для приложений, работающих на одном компьютере. Существуют случаи, когда разработчик может потребоваться изменить эти (возможно потому, что их приложения распределены между несколькими компьютерами или соответствия требованиям), и для этих сценариев система защиты данных предлагает широкие возможности API конфигурации.

<a name=data-protection-configuration-callback></a>

Отсутствует метод расширения AddDataProtection, возвращающий IDataProtectionBuilder который в свою очередь предоставляет методы расширения, что можно соединить в цепочку вместе для настройки различных защиты данных параметров. Например для хранения ключей в UNC-ресурс вместо % LOCALAPPDATA % (по умолчанию), настройте систему следующим образом:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> При изменении расположения сохраняемость ключа система будет шифровать больше не будет автоматически ключи хранятся так как он не знает ли DPAPI — это механизм шифрования соответствующие.

<a name=configuring-x509-certificate></a>

Можно настроить систему для защиты ключей неактивные вызовом ProtectKeysWith\* интерфейсы API настройки. Рассмотрим следующий пример, который хранит ключи в общем ресурсе UNC и шифрует эти ключи хранятся с конкретным сертификатом X.509.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

В разделе [шифрования ключа при хранении](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) Дополнительные примеры и сведения о механизме встроенного ключа шифрования.

Для настройки системы для использования времени жизни по умолчанию 14 дней вместо 90 дней, рассмотрим следующий пример:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

По умолчанию система защиты данных изолирует приложений друг от друга, даже если они совместно используют тот же физический репозиторий ключа. Это предотвращает приложений понимание друг друга защищенных полезных данных. Для совместного использования защищенных полезных данных между двумя приложениями, настройки системы, передача в одно и то же имя приложения для обоих приложений, как в следующем примере:

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

Наконец возможно, сценарий, где требуется приложение для автоматического развертывания ключей, как приближающихся истечение срока действия. Примером этого может быть в связи первичного и вторичного, где только основное приложение отвечает за вопросы управления ключами, а все приложения получателя просто имеют только для чтения представление кольца ключ приложения. Можно настроить получателей приложений необходимо рассматривать ключей только для чтения, настройки системы, как показано ниже:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name=data-protection-configuration-per-app-isolation></a>

## <a name="per-application-isolation"></a>Изоляция на уровне приложения

Когда система защиты данных предоставляется ASP.NET Core для узла, он будет автоматически изолировать приложения друг от друга, даже если эти приложения запущены под одной учетной записи рабочего процесса и используете же использований материала. Это аналогично немного модификатора IsolateApps из System.Web <machineKey> элемента.

Работает механизм изоляции, учитывая каждое приложение на локальном компьютере как уникальный клиента, таким образом IDataProtector, корнем для любого конкретного приложения, автоматически включает идентификатор приложения как дискриминатора. Уникальный идентификатор приложения поступают из одной из двух частей.

1. Если приложение размещается в службах IIS, уникальный идентификатор является путь конфигурации приложения. Если приложение развертывается в среде фермы, это значение должно быть стабильной при условии, что IIS средах аналогичным образом настроены на всех компьютерах в ферме.

2. Если приложение не размещается в службах IIS, уникальный идентификатор является физический путь приложения.

Уникальный идентификатор предназначен выдержать сбрасывает - как отдельное приложение и самой машины.

Этот механизм изоляции предполагается, что приложения не вредоносных. Вредоносное приложение всегда может повлиять на другие приложения, выполняемых в рамках одной учетной записи рабочего процесса. В общей среде размещения, где приложения являются взаимно без доверия поставщик услуг размещения необходимо предпринять действия для обеспечения ОС уровня изоляции между приложениями, включая Отделение приложений основного ключа репозиториев.

Если система защиты данных, не предоставляются ASP.NET Core для узла (например, если разработчик создает его себе через конкретный тип DataProtectionProvider) изоляции приложений отключена по умолчанию и все приложения резервного путем ввода материалы могут совместно использовать полезных данных, при условии, что они предоставляют соответствующие целей. Для обеспечения изоляции приложений в этой среде, вызовите метод SetApplicationName в объекте конфигурации см. в разделе [образец кода](#data-protection-code-sample-application-name) выше.

<a name=data-protection-changing-algorithms></a>

## <a name="changing-algorithms"></a>Изменение алгоритмов

Стек защиты данных позволяет изменять алгоритм по умолчанию, используемые вновь создаваемых ключей. Самый простой способ сделать это является вызов UseCryptographicAlgorithms из конфигурации обратного вызова, как в примере ниже.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

По умолчанию EncryptionAlgorithm и ValidationAlgorithm — AES-256-CBC и HMACSHA256, соответственно. Политика по умолчанию можно задать системным администратором через [расширенные политики компьютера](machine-wide-policy.md), но явный вызов UseCryptographicAlgorithms будет переопределить политику по умолчанию.

Вызов UseCryptographicAlgorithms позволяет разработчику указать нужный алгоритм (из стандартного списка встроенных), и разработчику не нужно беспокоиться о реализации этого алгоритма. Например, в сценарии выше система защиты данных будет пытаться использовать реализацию CNG алгоритма AES при использовании ОС Windows, в противном случае она вернется к управляемого класса System.Security.Cryptography.Aes.

Разработчик может указать реализацию, вручную при необходимости через вызов UseCustomCryptographicAlgorithms, как показано в ниже примерах.

>[!TIP]
> Изменение алгоритмов не влияет на существующие ключи в ключ обмена. Он влияет только на вновь созданных ключей.

<a name=data-protection-changing-algorithms-custom-managed></a>

### <a name="specifying-custom-managed-algorithms"></a>Указание пользовательских управляемых алгоритмов

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы указать пользовательские управляемые алгоритмы, создайте экземпляр ManagedAuthenticatedEncryptorConfiguration, указывающий типы реализации.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Чтобы указать пользовательские управляемые алгоритмы, создайте экземпляр ManagedAuthenticatedEncryptionSettings, указывающий типы реализации.

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

Обычно \*тип свойства должен указывать на конкретный, допускающих создание экземпляров (через открытый конструктор без параметров) реализации SymmetricAlgorithm и KeyedHashAlgorithm, хотя специальные варианты системы некоторые значения, например typeof(Aes) для удобства .

> [!NOTE]
> SymmetricAlgorithm должен иметь длину ключа 128 бит ≥ и размер блока ≥ 64-разрядная, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7. KeyedHashAlgorithm должен иметь размер хэш-кода > = 128 бит, и он должен поддерживать ключи длина равна длине дайджест хэш-алгоритм. KeyedHashAlgorithm не является обязательным для HMAC.

<a name=data-protection-changing-algorithms-cng></a>

### <a name="specifying-custom-windows-cng-algorithms"></a>Указание пользовательские алгоритмы Windows CNG

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы указать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC + проверки HMAC, создайте экземпляр CngCbcAuthenticatedEncryptorConfiguration алгоритмической сведениями.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Чтобы указать пользовательский алгоритм Windows CNG с помощью шифрования в режиме CBC + проверки HMAC, создайте экземпляр CngCbcAuthenticatedEncryptionSettings алгоритмической сведениями.

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
> Алгоритм шифрования симметричных блока должен иметь длину ключа 128 бит ≥ и размер блока ≥ 64-разрядная, и он должен поддерживать режим CBC шифрования с заполнением PKCS #7. Хэш-алгоритм должен иметь размер хэш-кода > = 128 бит и должны поддерживать открываемого с флагом BCRYPT_ALG_HANDLE_HMAC_FLAG. \*Поставщика свойства можно задать значение null, чтобы использовать поставщика по умолчанию для указанного алгоритма. В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Чтобы указать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) + проверки, создайте экземпляр CngGcmAuthenticatedEncryptorConfiguration алгоритмической сведениями.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Чтобы указать пользовательский алгоритм Windows CNG с помощью шифрования режим Galois (счетчиков) + проверки, создайте экземпляр CngGcmAuthenticatedEncryptionSettings алгоритмической сведениями.

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
> Алгоритм шифрования симметричных блока должен иметь длину ключа 128 бит ≥ и размер блока в точности 128 бит, и он должен поддерживать шифрование GCM. Свойство EncryptionAlgorithmProvider может быть присвоено значение null для использования поставщика по умолчанию для указанного алгоритма. В разделе [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Дополнительные сведения см.

### <a name="specifying-other-custom-algorithms"></a>Указание другие пользовательские алгоритмы

На то, что не представлен как полноценные API, система защиты данных является расширяемой, чтобы обеспечить возможность указания практически любой тип алгоритма. Например можно сохранить все ключи, содержащиеся в аппаратный модуль безопасности и предоставить собственную реализацию основных подпрограммы шифрования и расшифровки. В разделе IAuthenticatedEncryptorConfiguration в разделе Основные криптографии расширяемости для получения дополнительной информации.

### <a name="see-also"></a>См. также

* [Зависимые сценарии не DI](non-di-scenarios.md)
* [Расширенные политики компьютера](machine-wide-policy.md)
