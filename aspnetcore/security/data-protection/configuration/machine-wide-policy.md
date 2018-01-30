---
title: "Политика компьютера для защиты данных поддержки в ASP.NET Core"
author: rick-anderson
description: "Дополнительные сведения о поддержке задание политики уровня компьютера по умолчанию для всех приложений, использующих защиту данных ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 53ded37e9fd5f1a2eaa37935d1c52efb1e9231ac
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Политика компьютера для защиты данных поддержки в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Если под управлением Windows, система защиты данных имеет ограниченную поддержку задание политики уровня компьютера по умолчанию для всех приложений, использующих защиту данных ASP.NET Core. Основная идея заключается в том, что администратор может потребоваться изменить параметр по умолчанию, таких как алгоритмы или время жизни ключа, без необходимости вручную обновить все приложения на компьютере.

> [!WARNING]
> Системный администратор может установить политику по умолчанию, но они не может применять. Разработчик приложения всегда можно переопределить любое значение с помощью одного из своих собственных выбор. Политика по умолчанию влияет только на приложения, где разработчик не указано явное значение для параметра.

## <a name="setting-default-policy"></a>Задание политики по умолчанию

Чтобы настроить политику по умолчанию, администратор может задать известных значений в системном реестре в следующем разделе реестра:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Если вы работаете в 64-разрядной операционной системе и влияют на поведение 32-разрядных приложений, не забудьте настроить эквивалент Wow6432Node указанный выше раздел.

Ниже приведены поддерживаемые значения.

| Значение              | Тип   | Описание: |
| ------------------ | :----: | ----------- |
| EncryptionType     | string | Указывает, какие алгоритмы, которые следует использовать для защиты данных. Значение должно быть CNG CBC, CNG GCM или управляемый код и описывается более подробно ниже. |
| DefaultKeyLifetime | DWORD  | Задает время существования для вновь созданных ключей. Значение указывается в днях и должен быть > = 7. |
| KeyEscrowSinks     | string | Указывает типы, которые используются для переноса ключей. Значение представляет собой разделенный точками с запятой список приемников перенос ключа, где каждый элемент в списке является квалифицированное имя типа, реализующего [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Типы шифрования

Если EncryptionType CNG CBC, система настроен на использование симметричный блочный шифр режим CBC по конфиденциальности и HMAC подлинность с помощью служб, предоставляемых Windows CNG (см. [указание пользовательские алгоритмы Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) для Дополнительные сведения о). Поддерживаются следующие дополнительные значения, каждое из которых соответствует свойству типа CngCbcAuthenticatedEncryptionSettings.

| Значение                       | Тип   | Описание: |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | Имя блока симметричный алгоритм шифрования, поддерживаемая CNG. Этот алгоритм открывается в режиме CBC. |
| EncryptionAlgorithmProvider | string | Имя может выдавать алгоритм EncryptionAlgorithm реализации поставщика CNG. |
| EncryptionAlgorithmKeySize  | DWORD  | Длина ключа для получения производных для алгоритм шифрования симметричных блока (в битах). |
| HashAlgorithm               | string | Имя хэш-алгоритма, поддерживаемая CNG. Этот алгоритм открывается в режиме HMAC. |
| HashAlgorithmProvider       | string | Имя реализации поставщика CNG, который может создать HashAlgorithm алгоритм. |

Если EncryptionType CNG GCM, система настроена используемый симметричный блочный шифр Galois/режим конфиденциальности и подлинность службы, предоставляемые Windows CNG (см. [указание пользовательские алгоритмы Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Дополнительные сведения). Поддерживаются следующие дополнительные значения, каждое из которых соответствует свойству типа CngGcmAuthenticatedEncryptionSettings.

| Значение                       | Тип   | Описание: |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | Имя блока симметричный алгоритм шифрования, поддерживаемая CNG. Этот алгоритм открывается в режиме Galois (счетчиков). |
| EncryptionAlgorithmProvider | string | Имя может выдавать алгоритм EncryptionAlgorithm реализации поставщика CNG. |
| EncryptionAlgorithmKeySize  | DWORD  | Длина ключа для получения производных для алгоритм шифрования симметричных блока (в битах). |

Если EncryptionType является управляемым, система настроена для использования управляемых SymmetricAlgorithm по конфиденциальности и KeyedHashAlgorithm подлинность (см. [указание пользовательский управляемый алгоритмы](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) Дополнительные сведения). Поддерживаются следующие дополнительные значения, каждое из которых соответствует свойству типа ManagedAuthenticatedEncryptionSettings.

| Значение                      | Тип   | Описание: |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | string | Имя типа, реализующего SymmetricAlgorithm сборки. |
| EncryptionAlgorithmKeySize | DWORD  | Длина (в битах) ключа для формирования алгоритм симметричного шифрования. |
| ValidationAlgorithmType    | string | Имя типа, реализующего KeyedHashAlgorithm сборки. |

Если EncryptionType имеет любое другое значение, отличное от null или пусто, в системе защиты данных возникло исключение во время запуска.

> [!WARNING]
> При настройке параметр политики по умолчанию, который включает в себя имена типов (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), типы должны быть доступны для приложения. Это означает, что приложений, работающих в среде CLR рабочего стола, сборок, содержащих эти типы должны присутствовать в глобальный кэш сборок (GAC). Для приложений ASP.NET Core, выполняющихся [.NET Core](https://www.microsoft.com/net/core), следует устанавливать пакеты, содержащие эти типы.
