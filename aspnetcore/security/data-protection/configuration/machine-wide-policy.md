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
ms.openlocfilehash: fde8f75422c9dd84311a65b21e1e38b47fbe0306
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="machine-wide-policy"></a>Расширенные политики компьютера

<a name="data-protection-configuration-machinewidepolicy"></a>

Если под управлением Windows, система защиты данных имеет ограниченную поддержку для настройки политики уровня компьютера по умолчанию для всех приложений, которые занимают защиты данных. Основная идея заключается в администратор может потребоваться изменить некоторые по умолчанию (например, время существования использоваться или ключа алгоритмы) без необходимости вручную обновить все приложения на компьютере.

>[!WARNING]
> Системный администратор может установить политику по умолчанию, но они не может применять. Разработчик приложения всегда можно переопределить с помощью одного из своих собственных Выбор любое значение. Политика по умолчанию влияет только на приложения, в которых разработчик не указано явное значение для некоторых из них.

## <a name="setting-default-policy"></a>Задание политики по умолчанию

Чтобы настроить политику по умолчанию, администратор может задать известных значений в системном реестре в следующем разделе.

Раздел реестра:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`

Если вы работаете в 64-разрядной операционной системе и влияют на поведение 32-разрядных приложений, не забудьте также настроить эквивалент Wow6432Node указанный выше раздел.

Поддерживаемыми значениями являются:

* EncryptionType [строка] - Указывает, какие алгоритмы, которые следует использовать для защиты данных. Это значение должно быть «CNG CBC», «CNG GCM» или «Managed» и описывается более подробно [ниже](#data-protection-encryption-types).

* DefaultKeyLifetime [DWORD] - Задает время существования для вновь созданных ключей. Это значение указывается в днях и должен быть ≥ 7.

* [Строка] - KeyEscrowSinks Указывает типы, которые будут использоваться для переноса ключей. Это значение является списком разделенных точкой с запятой приемники перенос ключа, где каждый элемент в списке — полное имя типа, который реализует IKeyEscrowSink сборки.

<a name="data-protection-encryption-types"></a>

### <a name="encryption-types"></a>Типы шифрования

Если EncryptionType «CNG CBC», система будет настроена для использования симметричный блочный шифр режим CBC по конфиденциальности и HMAC подлинность с помощью служб, предоставляемых Windows CNG (см. [указание пользовательские алгоритмы Windows CNG](overview.md#data-protection-changing-algorithms-cng)Дополнительные сведения). Поддерживаются следующие дополнительные значения, каждый из которых соответствует свойству в типе CngCbcAuthenticatedEncryptionSettings:

* EncryptionAlgorithm [строка] - Имя блока симметричный алгоритм шифрования, поддерживаемая CNG. Этот алгоритм будет открыт в режиме CBC.

* EncryptionAlgorithmProvider [строка] - Имя реализации поставщика CNG, что может привести к EncryptionAlgorithm алгоритм.

* EncryptionAlgorithmKeySize [DWORD] - длина (в битах) ключа для получения производных для алгоритм шифрования симметричных блока.

* HashAlgorithm [строка] - Имя хэш-алгоритма, поддерживаемая CNG. Этот алгоритм будет открыт в режиме HMAC.

* HashAlgorithmProvider [строка] - Имя реализации поставщика CNG, что может привести к HashAlgorithm алгоритм.

Если EncryptionType «CNG GCM», система будет настроена для использования режима Galois/счетчика симметричный блочный шифр по конфиденциальности и подлинность с служб, предоставляемых Windows CNG (см. [указание пользовательские алгоритмы Windows CNG](overview.md#data-protection-changing-algorithms-cng)Дополнительные сведения). Поддерживаются следующие дополнительные значения, каждый из которых соответствует свойству в типе CngGcmAuthenticatedEncryptionSettings:

* EncryptionAlgorithm [строка] - Имя блока симметричный алгоритм шифрования, поддерживаемая CNG. Этот алгоритм будет открыт в режиме Galois (счетчиков).

* EncryptionAlgorithmProvider [строка] - Имя реализации поставщика CNG, что может привести к EncryptionAlgorithm алгоритм.

* EncryptionAlgorithmKeySize [DWORD] - длина (в битах) ключа для получения производных для алгоритм шифрования симметричных блока.

Если EncryptionType «управляемый», система будет настроена для использования управляемых SymmetricAlgorithm по конфиденциальности и KeyedHashAlgorithm подлинность (см. [указание пользовательский управляемый алгоритмы](overview.md#data-protection-changing-algorithms-custom-managed) Дополнительные сведения). Поддерживаются следующие дополнительные значения, каждый из которых соответствует свойству в типе ManagedAuthenticatedEncryptionSettings:

* EncryptionAlgorithmType [строка] - квалифицированное имя типа, который реализует SymmetricAlgorithm.

* EncryptionAlgorithmKeySize [DWORD] - длина (в битах) ключа для формирования алгоритм симметричного шифрования.

* ValidationAlgorithmType [строка] - квалифицированное имя типа, который реализует KeyedHashAlgorithm.

Если EncryptionType имеет любое другое значение (отличные от null / пусто), система защиты данных возникает исключение при запуске.

>[!WARNING]
> При настройке параметр политики по умолчанию, который включает в себя имена типов (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), типы должны быть доступны для приложения. На практике это означает, что для приложений, выполняемых в среде CLR рабочего стола, сборкам, которые содержат эти типы должны быть правами PartialTrust. Для приложений ASP.NET Core на [.NET Core](https://www.microsoft.com/net/core), следует устанавливать пакеты, которые содержат эти типы.
