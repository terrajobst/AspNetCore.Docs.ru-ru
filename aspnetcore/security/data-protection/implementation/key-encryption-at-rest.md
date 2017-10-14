---
title: "Шифрование ключа при хранении"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f2bbbf4e-0945-43ce-be59-8bf19e448798
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 5d0eb4036a3d491336cbe9357779c150b5cbb236
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="key-encryption-at-rest"></a>Шифрование ключа при хранении

<a name="data-protection-implementation-key-encryption-at-rest"></a>

По умолчанию система защиты данных [использует эвристику](../configuration/default-settings.md#data-protection-default-settings) для определения способа шифрования материал ключа должен быть в зашифрованном виде. Разработчик можно переопределить эвристика и вручную задать как ключи должны быть в зашифрованном виде.

> [!NOTE]
> При указании явную ключа шифрования в механизм rest, система защиты данных будет отменить регистрацию механизм хранилища ключей по умолчанию, предоставленный эвристики. Вы должны [укажите механизм явного хранилища ключей](key-storage-providers.md#data-protection-implementation-key-storage-providers), в противном случае система защиты данных не смогут запуститься.

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

Система защиты данных поставляется с тремя механизмы шифрования ключа в поле.

## <a name="windows-dpapi"></a>Windows DPAPI

*Этот механизм доступен только в Windows.*

Если используется Windows DPAPI, материал ключа, будут зашифрованы через [функция CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) перед сохранением в хранилище. DPAPI — это механизм нужные данные, которые никогда не будет считываться из-за пределами текущего компьютера (хотя можно выполнить эти ключи до Active Directory; см. раздел [перемещаемых профилей и DPAPI](https://support.microsoft.com/kb/309408/#6)). Например настроить шифрование ключа статических DPAPI.

```csharp
sc.AddDataProtection()
       // only the local user account can decrypt the keys
       .ProtectKeysWithDpapi();
   ```

Если ProtectKeysWithDpapi вызывается без параметров, только текущей учетной записи Windows, может расшифровать материализованный материал ключа. При необходимости можно указать, что любой учетной записи пользователя на компьютере (не только текущей учетной записи пользователя), требуется возможность расшифровать материал ключа, как показано в следующем примере.

```csharp
sc.AddDataProtection()
       // all user accounts on the machine can decrypt the keys
       .ProtectKeysWithDpapi(protectToLocalMachine: true);
   ```

## <a name="x509-certificate"></a>Сертификат X.509

*Этот механизм не доступен на `.NET Core 1.0` или `1.1`.*

Если приложение распределены между несколькими компьютерами, может быть удобным для распределения общего сертификата X.509 на компьютерах и настройка приложений для использования этого сертификата для шифрования ключей при хранении. Пример см. ниже.

```csharp
sc.AddDataProtection()
       // searches the cert store for the cert with this thumbprint
       .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
   ```

Из-за ограничений платформы .NET Framework, поддерживаются только сертификаты с закрытыми ключами CAPI. В разделе [шифрования на основе сертификата с Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) ниже возможных способах обхода этих ограничений.

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

*Этот механизм доступен только в Windows 8 и Windows Server 2012 и более поздней версии.*

Начиная с Windows 8, операционная система поддерживает DPAPI-NG (также называемые CNG DPAPI). Корпорация Майкрософт располагает его сценария использования следующим образом.

   Облако, тем не менее, часто требуется содержимого зашифрованные на одном компьютере быть расшифрованы на другом. Таким образом начиная с Windows 8, Microsoft расширенные идея использования относительно простой интерфейс API для охвата облачных сценариев. Это новый API с названием DPAPI-NG, позволяет безопасно совместно использовать секретные данные (ключи, пароли, материал ключа) и сообщения, защищая набор субъектов, которые можно использовать для отключения их защиты на разных компьютерах после надлежащую проверку подлинности и авторизации.

   Из [о CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

Участник кодируются как дескриптор правила защиты. Рассмотрим ниже примере, который шифрует материал ключа таким образом, что только присоединенных к домену пользователя с указанным SID может расшифровать материал ключа.

```csharp
sc.AddDataProtection()
     // uses the descriptor rule "SID=S-1-5-21-..."
     .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
       flags: DpapiNGProtectionDescriptorFlags.None);
   ```

Имеется также перегрузка ProtectKeysWithDpapiNG без параметров. Это удобный метод для указания правило «SID = Мое», где мои — идентификатор безопасности текущего пользователя Windows.

```csharp
sc.AddDataProtection()
     // uses the descriptor rule "SID={current account SID}"
     .ProtectKeysWithDpapiNG();
   ```

В этом сценарии контроллер домена AD отвечает за распределение ключи шифрования, используемые в операциях DPAPI NG. Целевого пользователя будет иметь возможность расшифровки зашифрованных полезных данных с любого компьютера, присоединенных к домену (при условии, что процесс выполняется их удостоверением).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Шифрование на основе сертификата с Windows DPAPI-NG

Если вы работаете на Windows 8.1 или Windows Server 2012 R2 или более поздней версии, Windows DPAPI-NG можно использовать для выполнения шифрования на основе сертификатов, даже если приложение выполняется [.NET Core](https://www.microsoft.com/net/core). Для использования этой возможности, используйте строки дескриптора правило «сертификата = HashId:thumbprint», где отпечаток является шестнадцатеричной кодировке отпечаток SHA1 сертификата для использования. Пример см. ниже.

```csharp
sc.AddDataProtection()
       // searches the cert store for the cert with this thumbprint
       .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
           flags: DpapiNGProtectionDescriptorFlags.None);
   ```

Приложение, которое указывает на этот репозиторий должны быть запущены на Windows 8.1 или Windows Server 2012 R2 или более поздней версии, чтобы иметь возможность расшифровать этот ключ.

## <a name="custom-key-encryption"></a>Пользовательские ключа шифрования

Если в поле механизмы не подходят, разработчик может указать собственный механизм шифрования ключа, предоставляя пользовательские IXmlEncryptor.
