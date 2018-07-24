---
title: Шифрование ключей при хранении в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о реализации защиты данных в ASP.NET Core ключа шифрования неактивных данных.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219294"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Шифрование ключей при хранении в ASP.NET Core

Система защиты данных [использует механизм обнаружения по умолчанию](xref:security/data-protection/configuration/default-settings) чтобы определить, как криптографические ключи должны быть зашифрованы при хранении. Разработчик может переопределить механизм обнаружения и вручную задать как ключи должны быть зашифрованы при хранении.

> [!WARNING]
> Если указать явно [сохраняемости расположение ключа](xref:security/data-protection/implementation/key-storage-providers), система защиты данных, отменяет регистрацию ключа шифрования по умолчанию механизм rest. Следовательно ключи не шифруются в неактивном состоянии. Мы рекомендуем вам [укажите механизм явного ключа шифрования](xref:security/data-protection/implementation/key-encryption-at-rest) для развертывания в рабочей среде. Механизм шифрования при хранении параметры описаны в этом разделе.

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a>Хранилище ключей Azure;

Для хранения ключей в [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), настройки системы с [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) в `Startup` класса:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Дополнительные сведения см. в разделе [Настройка защиты данных в ASP.NET Core: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).

::: moniker-end

## <a name="windows-dpapi"></a>Windows DPAPI

**Применяется только для развертывания Windows.**

Если используется Windows DPAPI, материал ключа шифруется с использованием [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) перед сохранением в хранилище. DPAPI является соответствующего механизма шифрования для данных, не было прочитано за пределами текущего компьютера (хотя можно выполнить эти ключи до Active Directory; см. в разделе [перемещаемых профилей и DPAPI](https://support.microsoft.com/kb/309408/#6)). Чтобы настроить шифрование ключей при хранении DPAPI, вызовите один из [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) методы расширения:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

Если `ProtectKeysWithDpapi` вызывается без параметров, только для текущей учетной записи пользователя Windows, может расшифровать сохраненного набора ключей. При необходимости можно указать, что любой учетной записи пользователя на компьютере (не только текущей учетной записи пользователя) иметь возможность расшифровать набора ключей:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a>Сертификат X.509

Если приложение распределены между несколькими компьютерами, то может оказаться удобнее для распределения между машинами сертификат X.509 с общим доступом и настройки размещенных приложений для использования сертификата для шифрования ключей при хранении:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

Из-за ограничений платформы .NET Framework поддерживаются только сертификаты с закрытыми ключами CAPI. См. в разделе содержимое ниже для возможных решений этих ограничений.

::: moniker-end

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

**Этот механизм можно найти только в Windows 8 и Windows Server 2012 или более поздней версии.**

Начиная с Windows 8, ОС Windows поддерживает DPAPI-NG (также называемый CNG DPAPI). Дополнительные сведения см. в разделе [о DPAPI CNG](/windows/desktop/SecCNG/cng-dpapi).

Участник кодируется как дескриптор правила защиты. В следующем примере, который вызывает [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), только для пользователя, присоединенных к домену, с указанным SID может расшифровать набора ключей:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Имеется также перегрузку без параметров метода `ProtectKeysWithDpapiNG`. Используйте этот метод для указания правило «SID = {CURRENT_ACCOUNT_SID}», где *CURRENT_ACCOUNT_SID* является идентификатор безопасности учетной записи текущего пользователя Windows:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

В этом случае контроллер домена AD отвечает за распределение ключей шифрования, используемых в операциях DPAPI-NG. Целевого пользователя могут быть расшифрованы зашифрованных полезных данных с любого компьютера, присоединенного к домену (при условии, что процесс выполняется свои удостоверения).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Шифрование на основе сертификата с помощью Windows DPAPI-NG

Если приложение выполняется в Windows 8.1 и Windows Server 2012 R2 или более поздней версии, можно использовать для выполнения шифрования на основе сертификата Windows DPAPI-NG. Использование строки дескриптора правило «СЕРТИФИКАТ = HashId:THUMBPRINT», где *ОТПЕЧАТОК* является шестнадцатеричной кодировке отпечаток SHA1 сертификата:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Любое приложение, на который указывает на этот репозиторий должен работать на Windows 8.1 и Windows Server 2012 R2 или более позднюю версию для расшифровки ключа.

## <a name="custom-key-encryption"></a>Пользовательский ключ шифрования

Если механизмы в поле не подходит, разработчик может указать собственный механизм шифрования ключей, предоставляя пользовательский [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).
