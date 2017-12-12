---
title: "Формат хранилища ключей"
author: tdykstra
description: "В этом документе подробно описывается реализация формата хранения ключей защиты данных ASP.NET Core."
keywords: "Хранилище ключей ASP.NET Core, защиты данных"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e8996478-f7bf-4b58-bab4-7fdb5d8556c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 4ebad05f7d55e954463ce5e277b419a7d6f773c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="key-storage-format"></a><span data-ttu-id="91177-104">Формат хранилища ключей</span><span class="sxs-lookup"><span data-stu-id="91177-104">Key Storage Format</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="91177-105">Объекты хранятся хранятся в XML-представление.</span><span class="sxs-lookup"><span data-stu-id="91177-105">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="91177-106">Каталог по умолчанию для хранилища ключей является % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="91177-106">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="91177-107">\<Ключ > элемент</span><span class="sxs-lookup"><span data-stu-id="91177-107">The \<key> element</span></span>

<span data-ttu-id="91177-108">Разделы существуют как объекты верхнего уровня в хранилище ключей.</span><span class="sxs-lookup"><span data-stu-id="91177-108">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="91177-109">По соглашению ключи имеют имя файла **XML-файла ключа-{guid}**, где {guid} — идентификатор ключа.</span><span class="sxs-lookup"><span data-stu-id="91177-109">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="91177-110">Каждый такой файл содержит один ключ.</span><span class="sxs-lookup"><span data-stu-id="91177-110">Each such file contains a single key.</span></span> <span data-ttu-id="91177-111">Формат файла выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="91177-111">The format of the file is as follows.</span></span>

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

<span data-ttu-id="91177-112">\<Ключ > элемент содержит следующие атрибуты и дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="91177-112">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="91177-113">Идентификатор ключа. Это значение интерпретируется как заслуживающие доверия; Имя файла — просто делом для восприятия человеком.</span><span class="sxs-lookup"><span data-stu-id="91177-113">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="91177-114">Версия \<ключ > элемент, в настоящее время фиксированной на 1.</span><span class="sxs-lookup"><span data-stu-id="91177-114">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="91177-115">Даты создания, активации и истечения срока действия ключа.</span><span class="sxs-lookup"><span data-stu-id="91177-115">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="91177-116">Объект \<дескриптора > элемент, который содержит сведения о реализации шифрования прошедшего проверку подлинности, содержащихся в данном разделе.</span><span class="sxs-lookup"><span data-stu-id="91177-116">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="91177-117">В приведенном выше примере ИД ключа {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, он был создан и активирована на 19 марта 2015 г., и имеет время существования 90 дней.</span><span class="sxs-lookup"><span data-stu-id="91177-117">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="91177-118">(Иногда Дата активации может быть немного до даты создания, как показано в примере.</span><span class="sxs-lookup"><span data-stu-id="91177-118">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="91177-119">Это происходит из-за nit как API-интерфейсы работают и неопасна на практике.)</span><span class="sxs-lookup"><span data-stu-id="91177-119">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="91177-120">\<Дескриптора > элемент</span><span class="sxs-lookup"><span data-stu-id="91177-120">The \<descriptor> element</span></span>

<span data-ttu-id="91177-121">Внешний \<дескриптора > элемент содержит атрибут deserializerType, представляющее собой имя сборки типа, который реализует IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="91177-121">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="91177-122">Этот тип является ответственным за чтение внутреннего \<дескриптора > элемент и обработки сведений, содержащихся в.</span><span class="sxs-lookup"><span data-stu-id="91177-122">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="91177-123">Определенный формат \<дескриптора > элемент зависит от реализации шифратор прошедшего проверку подлинности, инкапсулированный с помощью ключа и тип каждого десериализатор ожидается немного другой формат для данного.</span><span class="sxs-lookup"><span data-stu-id="91177-123">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="91177-124">В целом, этот элемент будет содержать алгоритмической сведения (имена, типы, идентификаторов объектов, или аналогичные) и секретного ключа.</span><span class="sxs-lookup"><span data-stu-id="91177-124">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="91177-125">В приведенном выше примере дескриптор перенос этот ключ шифрования AES-256-CBC + HMACSHA256 проверки.</span><span class="sxs-lookup"><span data-stu-id="91177-125">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="91177-126">\<EncryptedSecret > элемент</span><span class="sxs-lookup"><span data-stu-id="91177-126">The \<encryptedSecret> element</span></span>

<span data-ttu-id="91177-127"><encryptedSecret> Элемент, содержащий зашифрованный секретного ключа могут присутствовать Если [включено шифрование секретов неактивные](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="91177-127">An <encryptedSecret> element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span></span> <span data-ttu-id="91177-128">Атрибут decryptorType будет квалифицированное имя типа, который реализует IXmlDecryptor.</span><span class="sxs-lookup"><span data-stu-id="91177-128">The attribute decryptorType will be the assembly-qualified name of a type which implements IXmlDecryptor.</span></span> <span data-ttu-id="91177-129">Этот тип является ответственным за чтение внутреннего <encryptedKey> элемент и расшифровки выполнить восстановление исходного открытого текста.</span><span class="sxs-lookup"><span data-stu-id="91177-129">This type is responsible for reading the inner <encryptedKey> element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="91177-130">Как и в \<дескриптора >, определенный формат <encryptedSecret> элемента зависит от механизма шифрования на rest используется.</span><span class="sxs-lookup"><span data-stu-id="91177-130">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="91177-131">В приведенном выше примере главный ключ зашифрован с помощью Windows DPAPI согласно комментарий.</span><span class="sxs-lookup"><span data-stu-id="91177-131">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="91177-132">\<Отзыва > элемент</span><span class="sxs-lookup"><span data-stu-id="91177-132">The \<revocation> element</span></span>

<span data-ttu-id="91177-133">Переработаны существуют в виде объектов верхнего уровня в хранилище ключей.</span><span class="sxs-lookup"><span data-stu-id="91177-133">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="91177-134">По правилам переработаны имеют имя файла **отзыва-{timestamp} .xml** (для всех ключей Отмена до определенной даты) или **отзыва-{guid} .xml** (для отмены определенного ключа).</span><span class="sxs-lookup"><span data-stu-id="91177-134">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="91177-135">Каждый файл содержит один \<отзыва > элемент.</span><span class="sxs-lookup"><span data-stu-id="91177-135">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="91177-136">Для отзыва сертификатов отдельных ключей, содержимое файла будет как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="91177-136">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="91177-137">В этом случае отменяется только указанный ключ.</span><span class="sxs-lookup"><span data-stu-id="91177-137">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="91177-138">Если идентификатор ключа «*», но при этом в качестве ниже примере отменяются все ключи, Дата создания предшествует отзыва указанной даты.</span><span class="sxs-lookup"><span data-stu-id="91177-138">If the key id is "*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="91177-139">\<Причина > элемент, не считываются системой.</span><span class="sxs-lookup"><span data-stu-id="91177-139">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="91177-140">Это удобное место для хранения восприятия причина отзыва.</span><span class="sxs-lookup"><span data-stu-id="91177-140">It is simply a convenient place to store a human-readable reason for revocation.</span></span>
