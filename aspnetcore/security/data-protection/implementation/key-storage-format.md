---
title: Формат хранилища ключей в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о реализации формата хранилища ключей защиты данных в ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897481"
---
# <a name="key-storage-format-in-aspnet-core"></a>Формат хранилища ключей в ASP.NET Core

<a name="data-protection-implementation-key-storage-format"></a>

Объекты хранятся в местах хранения на XML-представление. Каталог по умолчанию для хранения ключей составляет % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>\<Ключ > элемент

Ключи существуют в виде объектов верхнего уровня в хранилище ключей. Ключи обычно имеют имя файла **ключа-{guid} .xml**, где {guid} — идентификатор ключа. Каждый такой файл содержит один ключ. Формат файла выглядит следующим образом.

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

* Идентификатор ключа. Это значение интерпретируется как заслуживающие доверия; Имя файла — просто проявление хорошего вкуса для восприятия человеком.

* Версия \<ключ > элемент, в настоящее время исправлено на 1.

* Даты создания, активации и истечения срока действия ключа.

* Объект \<дескриптора > элемент, который содержит сведения о реализации шифрование с проверкой подлинности, содержащихся в этот ключ.

В приведенном выше примере идентификатор ключа — {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, ее создания и активации 19 марта 2015 г., и он имеет время существования 90 дней. (Иногда дату активации может быть немного до даты создания, как в следующем примере. Это вызвано nit в том, как API-интерфейсы работают и не оказывает отрицательного воздействия на практике.)

## <a name="the-descriptor-element"></a>\<Дескриптора > элемент

Внешний \<дескриптора > элемент содержит атрибут deserializerType, которое является именем типа, который реализует IAuthenticatedEncryptorDescriptorDeserializer с указанием сборки. Этот тип отвечает за чтение внутреннего \<дескриптора > элемента и для синтаксического анализа сведений, содержащихся в.

Определенный формат \<дескриптора > элемент зависит от реализации прошедшего проверку подлинности шифратора, инкапсулированный с помощью ключа, и каждый тип десериализатор ожидает другой формат для этого. Вообще говоря, однако этот элемент будет содержать алгоритмического сведения (имена, типы, OID, или аналогичные) и секретного ключа. В приведенном выше примере дескриптор перенос этот ключ шифрования AES-256-CBC + HMACSHA256 проверки.

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > элемент

**&lt;EncryptedSecret&gt;** элемент, который содержит зашифрованный материал секретного ключа могут присутствовать Если [включено шифрование секретов при хранении](xref:security/data-protection/implementation/key-encryption-at-rest). Атрибут `decryptorType` квалифицированное имя типа, который реализует [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor). Этот тип отвечает за чтение внутреннего **&lt;encryptedKey&gt;** элемент и расшифровывает его, чтобы восстановить исходным открытым текстом.

Как и в `<descriptor>`, определенный формат `<encryptedSecret>` элемент зависит от механизма шифрования неактивных данных используется. В приведенном выше примере главный ключ зашифрован с помощью Windows DPAPI в комментарий.

## <a name="the-revocation-element"></a>\<Отзыва > элемент

Отзывы существуют в виде объектов верхнего уровня в хранилище ключей. По соглашению имя файла быть переработаны **отзыва-{timestamp} .xml** (для отзыва всех ключей до определенной даты) или **отзыва-{guid} .xml** (для отзыва определенного ключа). Каждый файл содержит один \<отзыва > элемента.

Для отзыва сертификатов отдельных ключей будет содержимое файла следующим образом.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

В этом случае аннулируются только указанный ключ. Если идентификатор ключа «*», тем не менее, как показано на приведенном ниже примере, отменяются все ключи, Дата создания — до даты указанного отзыва.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<Причина > элемента не было прочитано системой. Это просто удобное место для хранения читаемую пользователем причину отзыва.
