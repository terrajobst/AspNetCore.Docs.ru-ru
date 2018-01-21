---
title: "Формат хранилища ключей"
author: tdykstra
description: "В этом документе подробно описывается реализация формата хранения ключей защиты данных ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 4b796df19349227ceabad185c539720cee7e7c83
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="key-storage-format"></a>Формат хранилища ключей

<a name="data-protection-implementation-key-storage-format"></a>

Объекты хранятся хранятся в XML-представление. Каталог по умолчанию для хранилища ключей является % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>\<Ключ > элемент

Разделы существуют как объекты верхнего уровня в хранилище ключей. По соглашению ключи имеют имя файла **XML-файла ключа-{guid}**, где {guid} — идентификатор ключа. Каждый такой файл содержит один ключ. Формат файла выглядит следующим образом.

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

\<Ключ > элемент содержит следующие атрибуты и дочерние элементы:

* Идентификатор ключа. Это значение интерпретируется как заслуживающие доверия; Имя файла — просто делом для восприятия человеком.

* Версия \<ключ > элемент, в настоящее время фиксированной на 1.

* Даты создания, активации и истечения срока действия ключа.

* Объект \<дескриптора > элемент, который содержит сведения о реализации шифрования прошедшего проверку подлинности, содержащихся в данном разделе.

В приведенном выше примере ИД ключа {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, он был создан и активирована на 19 марта 2015 г., и имеет время существования 90 дней. (Иногда Дата активации может быть немного до даты создания, как показано в примере. Это происходит из-за nit как API-интерфейсы работают и неопасна на практике.)

## <a name="the-descriptor-element"></a>\<Дескриптора > элемент

Внешний \<дескриптора > элемент содержит атрибут deserializerType, представляющее собой имя сборки типа, который реализует IAuthenticatedEncryptorDescriptorDeserializer. Этот тип является ответственным за чтение внутреннего \<дескриптора > элемент и обработки сведений, содержащихся в.

Определенный формат \<дескриптора > элемент зависит от реализации шифратор прошедшего проверку подлинности, инкапсулированный с помощью ключа и тип каждого десериализатор ожидается немного другой формат для данного. В целом, этот элемент будет содержать алгоритмической сведения (имена, типы, идентификаторов объектов, или аналогичные) и секретного ключа. В приведенном выше примере дескриптор перенос этот ключ шифрования AES-256-CBC + HMACSHA256 проверки.

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > элемент

<encryptedSecret> Элемент, содержащий зашифрованный секретного ключа могут присутствовать Если [включено шифрование секретов неактивные](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest). Атрибут decryptorType будет квалифицированное имя типа, который реализует IXmlDecryptor. Этот тип является ответственным за чтение внутреннего <encryptedKey> элемент и расшифровки выполнить восстановление исходного открытого текста.

Как и в \<дескриптора >, определенный формат <encryptedSecret> элемента зависит от механизма шифрования на rest используется. В приведенном выше примере главный ключ зашифрован с помощью Windows DPAPI согласно комментарий.

## <a name="the-revocation-element"></a>\<Отзыва > элемент

Переработаны существуют в виде объектов верхнего уровня в хранилище ключей. По правилам переработаны имеют имя файла **отзыва-{timestamp} .xml** (для всех ключей Отмена до определенной даты) или **отзыва-{guid} .xml** (для отмены определенного ключа). Каждый файл содержит один \<отзыва > элемент.

Для отзыва сертификатов отдельных ключей, содержимое файла будет как показано ниже.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

В этом случае отменяется только указанный ключ. Если идентификатор ключа «*», но при этом в качестве ниже примере отменяются все ключи, Дата создания предшествует отзыва указанной даты.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<Причина > элемент, не считываются системой. Это удобное место для хранения восприятия причина отзыва.
