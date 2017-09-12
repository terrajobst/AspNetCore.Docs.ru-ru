---
title: "Расширенные политики компьютера"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 7ada940acfbb7fb0887fd7c0cd722bf62f211248
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="machine-wide-policy"></a><span data-ttu-id="d77f2-103">Расширенные политики компьютера</span><span class="sxs-lookup"><span data-stu-id="d77f2-103">Machine Wide Policy</span></span>

<a name=data-protection-configuration-machinewidepolicy></a>

<span data-ttu-id="d77f2-104">Если под управлением Windows, система защиты данных имеет ограниченную поддержку для настройки политики уровня компьютера по умолчанию для всех приложений, которые занимают защиты данных.</span><span class="sxs-lookup"><span data-stu-id="d77f2-104">When running on Windows, the data protection system has limited support for setting default machine-wide policy for all applications which consume data protection.</span></span> <span data-ttu-id="d77f2-105">Основная идея заключается в администратор может потребоваться изменить некоторые по умолчанию (например, время существования использоваться или ключа алгоритмы) без необходимости вручную обновить все приложения на компьютере.</span><span class="sxs-lookup"><span data-stu-id="d77f2-105">The general idea is that an administrator might wish to change some default setting (such as algorithms used or key lifetime) without needing to manually update every application on the machine.</span></span>

>[!WARNING]
> <span data-ttu-id="d77f2-106">Системный администратор может установить политику по умолчанию, но они не может применять.</span><span class="sxs-lookup"><span data-stu-id="d77f2-106">The system administrator can set default policy, but they cannot enforce it.</span></span> <span data-ttu-id="d77f2-107">Разработчик приложения всегда можно переопределить с помощью одного из своих собственных Выбор любое значение.</span><span class="sxs-lookup"><span data-stu-id="d77f2-107">The application developer can always override any value with one of their own choosing.</span></span> <span data-ttu-id="d77f2-108">Политика по умолчанию влияет только на приложения, в которых разработчик не указано явное значение для некоторых из них.</span><span class="sxs-lookup"><span data-stu-id="d77f2-108">The default policy only affects applications where the developer has not specified an explicit value for some particular setting.</span></span>

## <a name="setting-default-policy"></a><span data-ttu-id="d77f2-109">Задание политики по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d77f2-109">Setting default policy</span></span>

<span data-ttu-id="d77f2-110">Чтобы настроить политику по умолчанию, администратор может задать известных значений в системном реестре в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="d77f2-110">To set default policy, an administrator can set known values in the system registry under the following key.</span></span>

<span data-ttu-id="d77f2-111">Раздел реестра:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span><span class="sxs-lookup"><span data-stu-id="d77f2-111">Reg key: `HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span></span>

<span data-ttu-id="d77f2-112">Если вы работаете в 64-разрядной операционной системе и влияют на поведение 32-разрядных приложений, не забудьте также настроить эквивалент Wow6432Node указанный выше раздел.</span><span class="sxs-lookup"><span data-stu-id="d77f2-112">If you're on a 64-bit operating system and want to affect the behavior of 32-bit applications, remember also to configure the Wow6432Node equivalent of the above key.</span></span>

<span data-ttu-id="d77f2-113">Поддерживаемыми значениями являются:</span><span class="sxs-lookup"><span data-stu-id="d77f2-113">The supported values are:</span></span>

* <span data-ttu-id="d77f2-114">EncryptionType [строка] - Указывает, какие алгоритмы, которые следует использовать для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="d77f2-114">EncryptionType [string] - specifies which algorithms should be used for data protection.</span></span> <span data-ttu-id="d77f2-115">Это значение должно быть «CNG CBC», «CNG GCM» или «Managed» и описывается более подробно [ниже](#data-protection-encryption-types).</span><span class="sxs-lookup"><span data-stu-id="d77f2-115">This value must be "CNG-CBC", "CNG-GCM", or "Managed" and is described in more detail [below](#data-protection-encryption-types).</span></span>

* <span data-ttu-id="d77f2-116">DefaultKeyLifetime [DWORD] - Задает время существования для вновь созданных ключей.</span><span class="sxs-lookup"><span data-stu-id="d77f2-116">DefaultKeyLifetime [DWORD] - specifies the lifetime for newly-generated keys.</span></span> <span data-ttu-id="d77f2-117">Это значение указывается в днях и должен быть ≥ 7.</span><span class="sxs-lookup"><span data-stu-id="d77f2-117">This value is specified in days and must be ≥ 7.</span></span>

* <span data-ttu-id="d77f2-118">[Строка] - KeyEscrowSinks Указывает типы, которые будут использоваться для переноса ключей.</span><span class="sxs-lookup"><span data-stu-id="d77f2-118">KeyEscrowSinks [string] - specifies the types which will be used for key escrow.</span></span> <span data-ttu-id="d77f2-119">Это значение является списком разделенных точкой с запятой приемники перенос ключа, где каждый элемент в списке — полное имя типа, который реализует IKeyEscrowSink сборки.</span><span class="sxs-lookup"><span data-stu-id="d77f2-119">This value is a semicolon-delimited list of key escrow sinks, where each element in the list is the assembly qualified name of a type which implements IKeyEscrowSink.</span></span>

<a name=data-protection-encryption-types></a>

### <a name="encryption-types"></a><span data-ttu-id="d77f2-120">Типы шифрования</span><span class="sxs-lookup"><span data-stu-id="d77f2-120">Encryption types</span></span>

<span data-ttu-id="d77f2-121">Если EncryptionType «CNG CBC», система будет настроена для использования симметричный блочный шифр режим CBC по конфиденциальности и HMAC подлинность с помощью служб, предоставляемых Windows CNG (см. [указание пользовательские алгоритмы Windows CNG](overview.md#data-protection-changing-algorithms-cng)Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="d77f2-121">If EncryptionType is "CNG-CBC", the system will be configured to use a CBC-mode symmetric block cipher for confidentiality and HMAC for authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="d77f2-122">Поддерживаются следующие дополнительные значения, каждый из которых соответствует свойству в типе CngCbcAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="d77f2-122">The following additional values are supported, each of which corresponds to a property on the CngCbcAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="d77f2-123">EncryptionAlgorithm [строка] - Имя блока симметричный алгоритм шифрования, поддерживаемая CNG.</span><span class="sxs-lookup"><span data-stu-id="d77f2-123">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="d77f2-124">Этот алгоритм будет открыт в режиме CBC.</span><span class="sxs-lookup"><span data-stu-id="d77f2-124">This algorithm will be opened in CBC mode.</span></span>

* <span data-ttu-id="d77f2-125">EncryptionAlgorithmProvider [строка] - Имя реализации поставщика CNG, что может привести к EncryptionAlgorithm алгоритм.</span><span class="sxs-lookup"><span data-stu-id="d77f2-125">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="d77f2-126">EncryptionAlgorithmKeySize [DWORD] - длина (в битах) ключа для получения производных для алгоритм шифрования симметричных блока.</span><span class="sxs-lookup"><span data-stu-id="d77f2-126">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="d77f2-127">HashAlgorithm [строка] - Имя хэш-алгоритма, поддерживаемая CNG.</span><span class="sxs-lookup"><span data-stu-id="d77f2-127">HashAlgorithm [string] - the name of a hash algorithm understood by CNG.</span></span> <span data-ttu-id="d77f2-128">Этот алгоритм будет открыт в режиме HMAC.</span><span class="sxs-lookup"><span data-stu-id="d77f2-128">This algorithm will be opened in HMAC mode.</span></span>

* <span data-ttu-id="d77f2-129">HashAlgorithmProvider [строка] - Имя реализации поставщика CNG, что может привести к HashAlgorithm алгоритм.</span><span class="sxs-lookup"><span data-stu-id="d77f2-129">HashAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm HashAlgorithm.</span></span>

<span data-ttu-id="d77f2-130">Если EncryptionType «CNG GCM», система будет настроена для использования режима Galois/счетчика симметричный блочный шифр по конфиденциальности и подлинность с служб, предоставляемых Windows CNG (см. [указание пользовательские алгоритмы Windows CNG](overview.md#data-protection-changing-algorithms-cng)Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="d77f2-130">If EncryptionType is "CNG-GCM", the system will be configured to use a Galois/Counter Mode symmetric block cipher for confidentiality and authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="d77f2-131">Поддерживаются следующие дополнительные значения, каждый из которых соответствует свойству в типе CngGcmAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="d77f2-131">The following additional values are supported, each of which corresponds to a property on the CngGcmAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="d77f2-132">EncryptionAlgorithm [строка] - Имя блока симметричный алгоритм шифрования, поддерживаемая CNG.</span><span class="sxs-lookup"><span data-stu-id="d77f2-132">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="d77f2-133">Этот алгоритм будет открыт в режиме Galois (счетчиков).</span><span class="sxs-lookup"><span data-stu-id="d77f2-133">This algorithm will be opened in Galois/Counter Mode.</span></span>

* <span data-ttu-id="d77f2-134">EncryptionAlgorithmProvider [строка] - Имя реализации поставщика CNG, что может привести к EncryptionAlgorithm алгоритм.</span><span class="sxs-lookup"><span data-stu-id="d77f2-134">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="d77f2-135">EncryptionAlgorithmKeySize [DWORD] - длина (в битах) ключа для получения производных для алгоритм шифрования симметричных блока.</span><span class="sxs-lookup"><span data-stu-id="d77f2-135">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

<span data-ttu-id="d77f2-136">Если EncryptionType «управляемый», система будет настроена для использования управляемых SymmetricAlgorithm по конфиденциальности и KeyedHashAlgorithm подлинность (см. [указание пользовательский управляемый алгоритмы](overview.md#data-protection-changing-algorithms-custom-managed) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="d77f2-136">If EncryptionType is "Managed", the system will be configured to use a managed SymmetricAlgorithm for confidentiality and KeyedHashAlgorithm for authenticity (see [Specifying custom managed algorithms](overview.md#data-protection-changing-algorithms-custom-managed) for more details).</span></span> <span data-ttu-id="d77f2-137">Поддерживаются следующие дополнительные значения, каждый из которых соответствует свойству в типе ManagedAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="d77f2-137">The following additional values are supported, each of which corresponds to a property on the ManagedAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="d77f2-138">EncryptionAlgorithmType [строка] - квалифицированное имя типа, который реализует SymmetricAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="d77f2-138">EncryptionAlgorithmType [string] - the assembly-qualified name of a type which implements SymmetricAlgorithm.</span></span>

* <span data-ttu-id="d77f2-139">EncryptionAlgorithmKeySize [DWORD] - длина (в битах) ключа для формирования алгоритм симметричного шифрования.</span><span class="sxs-lookup"><span data-stu-id="d77f2-139">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric encryption algorithm.</span></span>

* <span data-ttu-id="d77f2-140">ValidationAlgorithmType [строка] - квалифицированное имя типа, который реализует KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="d77f2-140">ValidationAlgorithmType [string] - the assembly-qualified name of a type which implements KeyedHashAlgorithm.</span></span>

<span data-ttu-id="d77f2-141">Если EncryptionType имеет любое другое значение (отличные от null / пусто), система защиты данных возникает исключение при запуске.</span><span class="sxs-lookup"><span data-stu-id="d77f2-141">If EncryptionType has any other value (other than null / empty), the data protection system will throw an exception at startup.</span></span>

>[!WARNING]
> <span data-ttu-id="d77f2-142">При настройке параметр политики по умолчанию, который включает в себя имена типов (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), типы должны быть доступны для приложения.</span><span class="sxs-lookup"><span data-stu-id="d77f2-142">When configuring a default policy setting that involves type names (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), the types must be available to the application.</span></span> <span data-ttu-id="d77f2-143">На практике это означает, что для приложений, выполняемых в среде CLR рабочего стола, сборкам, которые содержат эти типы должны быть правами PartialTrust.</span><span class="sxs-lookup"><span data-stu-id="d77f2-143">In practice, this means that for applications running on Desktop CLR, the assemblies which contain these types should be GACed.</span></span> <span data-ttu-id="d77f2-144">Для приложений ASP.NET Core на [.NET Core](https://www.microsoft.com/net/core), следует устанавливать пакеты, которые содержат эти типы.</span><span class="sxs-lookup"><span data-stu-id="d77f2-144">For ASP.NET Core applications running on [.NET Core](https://www.microsoft.com/net/core), the packages which contain these types should be installed.</span></span>
