---
title: Ключевой формат хранения в ASP.NET Core
author: rick-anderson
description: Узнайте подробную информацию о формате хранения ключей ASP.NET Core Data Protection.
ms.author: riande
ms.date: 04/08/2020
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 3072c673791b589027a910b80eaba52052eb9311
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80976941"
---
# <a name="key-storage-format-in-aspnet-core"></a>Ключевой формат хранения в ASP.NET Core

<a name="data-protection-implementation-key-storage-format"></a>

Объекты хранятся в состоянии покоя в представлении XML. Каталог по умолчанию для хранилища ключей:

* Окна: «%LOCALAPPDATA%»ASP.NET-DataProtection-Keys\*
* macOS / Linux: *$HOME/.aspnet/DataProtection-Keys*

## <a name="the-key-element"></a>Ключевой \<элемент>

Ключи существуют как объекты верхнего уровня в репозитории ключа. По клавишам конвенции есть ключ файла **-'guid'.xml**, где «guid» является идентификатором ключа. Каждый такой файл содержит один ключ. Формат файла следующий.

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

Ключевой \<элемент> содержит следующие атрибуты и элементы ребенка:

* Ключевой идентификатор. Это значение рассматривается как авторитетное; имя файла просто тонкость для человеческой читаемости.

* Версия ключевого \<элемента>, в настоящее время зафиксирована на уровне 1.

* Сроки создания ключа, активации и истечения срока действия.

* Дескриптор \<> элемент, содержащий информацию о реализации аутентифицированного шифрования, содержащуюся в этом ключе.

В приведенном выше примере идентификатор ключа: 80732141-ec8f-4b80-af9c-c4d2d1ff8901, он был создан и активирован 19 марта 2015 года, и срок службы составляет 90 дней. (Иногда дата активации может быть немного до даты создания, как в этом примере. Это связано с гнидой в том, как работают AA и безвредны на практике.)

## <a name="the-descriptor-element"></a>Элемент \<дескриптора>

Внешний \<дескриптор> элемент содержит атрибут deserializerType, который является сборочно-квалифицированным названием типа, который реализует IAuthenticatedEncryptorDescriptorDeserializer. Этот тип отвечает за \<чтение внутреннего дескриптора> элемента и для разбора информации, содержащейся внутри.

Конкретный формат \<элемента дескриптора> зависит от аутентифицированной реализации шифрования, инкапсулированной ключом, и каждый тип deserializer ожидает немного иного формата для этого. Однако в целом этот элемент будет содержать алгоритмическую информацию (имена, типы, OID или аналогичные) и секретный ключевой материал. В приведенном выше примере дескриптор указывает, что этот ключ обертывает шифрование AES-256-CBC и проверку HMACSHA256.

## <a name="the-encryptedsecret-element"></a>Зашифрованный элемент Секретной \<>

** &lt;Зашифрованный секретный&gt; ** элемент, содержащий зашифрованную форму секретного ключевого материала, может присутствовать, если [включено шифрование секретов в состоянии покоя.](xref:security/data-protection/implementation/key-encryption-at-rest) Атрибут `decryptorType` — это сборочное имя типа, который реализует [IXmlDecryptor.](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor) Этот тип отвечает за чтение внутреннего ** &lt;зашифрованного&gt; ** элемента Key и расшифровку его для восстановления исходного простого текста.

Как `<descriptor>`и в остальном, конкретный формат элемента `<encryptedSecret>` зависит от используемого механизма шифрования на месте. В приведенном выше примере мастер-ключ шифруется с помощью Windows DPAPI за комментарий.

## <a name="the-revocation-element"></a>\<Отзыв> элемент

Отзывы существуют как объекты верхнего уровня в репозитории ключа. По отмене конвенции имеют **сярвый отзыв файла -timestamp.xml** (для отзыва всех ключей до определенной даты) или **отзыв-.guid.xml** (для отзыва определенного ключа). Каждый файл \<содержит один элемент отзыва>.

Для отзыва отдельных ключей содержимое файла будет ниже.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

В этом случае отменяется только указанный ключ. Однако, если ключевой идентификатор —«К», то, как в приведенном ниже примере, все ключи, дата создания которых до указанной даты отзыва, аннулируются.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

Причина, \<по которой элемент> никогда не читается системой. Это просто удобное место для хранения читаемой человеком причины отзыва.
