---
title: Шифрование ключей при хранении в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о реализации защиты данных в ASP.NET Core ключа шифрования неактивных данных.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892311"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="27de7-103">Шифрование ключей при хранении в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27de7-103">Key encryption at rest in ASP.NET Core</span></span>

<span data-ttu-id="27de7-104">Система защиты данных [использует механизм обнаружения по умолчанию](xref:security/data-protection/configuration/default-settings) чтобы определить, как криптографические ключи должны быть зашифрованы при хранении.</span><span class="sxs-lookup"><span data-stu-id="27de7-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine how cryptographic keys should be encrypted at rest.</span></span> <span data-ttu-id="27de7-105">Разработчик может переопределить механизм обнаружения и вручную задать как ключи должны быть зашифрованы при хранении.</span><span class="sxs-lookup"><span data-stu-id="27de7-105">The developer can override the discovery mechanism and manually specify how keys should be encrypted at rest.</span></span>

> [!WARNING]
> <span data-ttu-id="27de7-106">Если указать явно [сохраняемости расположение ключа](xref:security/data-protection/implementation/key-storage-providers), система защиты данных, отменяет регистрацию ключа шифрования по умолчанию механизм rest.</span><span class="sxs-lookup"><span data-stu-id="27de7-106">If you specify an explicit [key persistence location](xref:security/data-protection/implementation/key-storage-providers), the data protection system deregisters the default key encryption at rest mechanism.</span></span> <span data-ttu-id="27de7-107">Следовательно ключи не шифруются в неактивном состоянии.</span><span class="sxs-lookup"><span data-stu-id="27de7-107">Consequently, keys are no longer encrypted at rest.</span></span> <span data-ttu-id="27de7-108">Мы рекомендуем вам [укажите механизм явного ключа шифрования](xref:security/data-protection/implementation/key-encryption-at-rest) для развертывания в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="27de7-108">We recommend that you [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span> <span data-ttu-id="27de7-109">Механизм шифрования при хранении параметры описаны в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="27de7-109">The encryption-at-rest mechanism options are described in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a><span data-ttu-id="27de7-110">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="27de7-110">Azure Key Vault</span></span>

<span data-ttu-id="27de7-111">Для хранения ключей в [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), настройки системы с [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="27de7-111">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="27de7-112">Дополнительные сведения см. в разделе [Настройка защиты данных в ASP.NET Core: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span><span class="sxs-lookup"><span data-stu-id="27de7-112">For more information, see [Configure ASP.NET Core Data Protection: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span></span>

::: moniker-end

## <a name="windows-dpapi"></a><span data-ttu-id="27de7-113">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="27de7-113">Windows DPAPI</span></span>

<span data-ttu-id="27de7-114">**Применяется только для развертывания Windows.**</span><span class="sxs-lookup"><span data-stu-id="27de7-114">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="27de7-115">Если используется Windows DPAPI, материал ключа шифруется с использованием [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) перед сохранением в хранилище.</span><span class="sxs-lookup"><span data-stu-id="27de7-115">When Windows DPAPI is used, key material is encrypted with [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) before being persisted to storage.</span></span> <span data-ttu-id="27de7-116">DPAPI является соответствующего механизма шифрования для данных, не было прочитано за пределами текущего компьютера (хотя можно выполнить эти ключи до Active Directory; см. в разделе [перемещаемых профилей и DPAPI](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="27de7-116">DPAPI is an appropriate encryption mechanism for data that's never read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="27de7-117">Чтобы настроить шифрование ключей при хранении DPAPI, вызовите один из [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) методы расширения:</span><span class="sxs-lookup"><span data-stu-id="27de7-117">To configure DPAPI key-at-rest encryption, call one of the [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) extension methods:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

<span data-ttu-id="27de7-118">Если `ProtectKeysWithDpapi` вызывается без параметров, только для текущей учетной записи пользователя Windows, может расшифровать сохраненного набора ключей.</span><span class="sxs-lookup"><span data-stu-id="27de7-118">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key ring.</span></span> <span data-ttu-id="27de7-119">При необходимости можно указать, что любой учетной записи пользователя на компьютере (не только текущей учетной записи пользователя) иметь возможность расшифровать набора ключей:</span><span class="sxs-lookup"><span data-stu-id="27de7-119">You can optionally specify that any user account on the machine (not just the current user account) be able to decipher the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a><span data-ttu-id="27de7-120">Сертификат X.509</span><span class="sxs-lookup"><span data-stu-id="27de7-120">X.509 certificate</span></span>

<span data-ttu-id="27de7-121">Если приложение распределены между несколькими компьютерами, то может оказаться удобнее для распределения между машинами сертификат X.509 с общим доступом и настройки размещенных приложений для использования сертификата для шифрования ключей при хранении:</span><span class="sxs-lookup"><span data-stu-id="27de7-121">If the app is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and configure the hosted apps to use the certificate for encryption of keys at rest:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

<span data-ttu-id="27de7-122">Из-за ограничений платформы .NET Framework поддерживаются только сертификаты с закрытыми ключами CAPI.</span><span class="sxs-lookup"><span data-stu-id="27de7-122">Due to .NET Framework limitations, only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="27de7-123">См. в разделе содержимое ниже для возможных решений этих ограничений.</span><span class="sxs-lookup"><span data-stu-id="27de7-123">See the content below for possible workarounds to these limitations.</span></span>

::: moniker-end

## <a name="windows-dpapi-ng"></a><span data-ttu-id="27de7-124">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="27de7-124">Windows DPAPI-NG</span></span>

<span data-ttu-id="27de7-125">**Этот механизм можно найти только в Windows 8 и Windows Server 2012 или более поздней версии.**</span><span class="sxs-lookup"><span data-stu-id="27de7-125">**This mechanism is available only on Windows 8/Windows Server 2012 or later.**</span></span>

<span data-ttu-id="27de7-126">Начиная с Windows 8, ОС Windows поддерживает DPAPI-NG (также называемый CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="27de7-126">Beginning with Windows 8, Windows OS supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="27de7-127">Дополнительные сведения см. в разделе [о DPAPI CNG](/windows/desktop/SecCNG/cng-dpapi).</span><span class="sxs-lookup"><span data-stu-id="27de7-127">For more information, see [About CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span></span>

<span data-ttu-id="27de7-128">Участник кодируется как дескриптор правила защиты.</span><span class="sxs-lookup"><span data-stu-id="27de7-128">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="27de7-129">В следующем примере, который вызывает [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), только для пользователя, присоединенных к домену, с указанным SID может расшифровать набора ключей:</span><span class="sxs-lookup"><span data-stu-id="27de7-129">In the following example that calls [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), only the domain-joined user with the specified SID can decrypt the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="27de7-130">Имеется также перегрузку без параметров метода `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="27de7-130">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="27de7-131">Используйте этот метод для указания правило «SID = {CURRENT_ACCOUNT_SID}», где *CURRENT_ACCOUNT_SID* является идентификатор безопасности учетной записи текущего пользователя Windows:</span><span class="sxs-lookup"><span data-stu-id="27de7-131">Use this convenience method to specify the rule "SID={CURRENT_ACCOUNT_SID}", where *CURRENT_ACCOUNT_SID* is the SID of the current Windows user account:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

<span data-ttu-id="27de7-132">В этом случае контроллер домена AD отвечает за распределение ключей шифрования, используемых в операциях DPAPI-NG.</span><span class="sxs-lookup"><span data-stu-id="27de7-132">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="27de7-133">Целевого пользователя могут быть расшифрованы зашифрованных полезных данных с любого компьютера, присоединенного к домену (при условии, что процесс выполняется свои удостоверения).</span><span class="sxs-lookup"><span data-stu-id="27de7-133">The target user can decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="27de7-134">Шифрование на основе сертификата с помощью Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="27de7-134">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="27de7-135">Если приложение выполняется в Windows 8.1 и Windows Server 2012 R2 или более поздней версии, можно использовать для выполнения шифрования на основе сертификата Windows DPAPI-NG.</span><span class="sxs-lookup"><span data-stu-id="27de7-135">If the app is running on Windows 8.1/Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption.</span></span> <span data-ttu-id="27de7-136">Использование строки дескриптора правило «СЕРТИФИКАТ = HashId:THUMBPRINT», где *ОТПЕЧАТОК* является шестнадцатеричной кодировке отпечаток SHA1 сертификата:</span><span class="sxs-lookup"><span data-stu-id="27de7-136">Use the rule descriptor string "CERTIFICATE=HashId:THUMBPRINT", where *THUMBPRINT* is the hex-encoded SHA1 thumbprint of the certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="27de7-137">Любое приложение, на который указывает на этот репозиторий должен работать на Windows 8.1 и Windows Server 2012 R2 или более позднюю версию для расшифровки ключа.</span><span class="sxs-lookup"><span data-stu-id="27de7-137">Any app pointed at this repository must be running on Windows 8.1/Windows Server 2012 R2 or later to decipher the keys.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="27de7-138">Пользовательский ключ шифрования</span><span class="sxs-lookup"><span data-stu-id="27de7-138">Custom key encryption</span></span>

<span data-ttu-id="27de7-139">Если механизмы в поле не подходит, разработчик может указать собственный механизм шифрования ключей, предоставляя пользовательский [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="27de7-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key encryption mechanism by providing a custom [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span></span>
