---
title: Формат хранилища ключей в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о реализации формата хранилища ключей защиты данных в ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: bca19ad001dd20b5d02ae5470f7d928082496037
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219281"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="5efae-103">Формат хранилища ключей в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5efae-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="5efae-104">Объекты хранятся в местах хранения на XML-представление.</span><span class="sxs-lookup"><span data-stu-id="5efae-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="5efae-105">Каталог по умолчанию для хранения ключей составляет % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="5efae-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="5efae-106">\<Ключ > элемент</span><span class="sxs-lookup"><span data-stu-id="5efae-106">The \<key> element</span></span>

<span data-ttu-id="5efae-107">Ключи существуют в виде объектов верхнего уровня в хранилище ключей.</span><span class="sxs-lookup"><span data-stu-id="5efae-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="5efae-108">Ключи обычно имеют имя файла **ключа-{guid} .xml**, где {guid} — идентификатор ключа.</span><span class="sxs-lookup"><span data-stu-id="5efae-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="5efae-109">Каждый такой файл содержит один ключ.</span><span class="sxs-lookup"><span data-stu-id="5efae-109">Each such file contains a single key.</span></span> <span data-ttu-id="5efae-110">Формат файла выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5efae-110">The format of the file is as follows.</span></span>

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

<span data-ttu-id="5efae-111">\<Ключ > элемент содержит следующие атрибуты и дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="5efae-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="5efae-112">Идентификатор ключа. Это значение интерпретируется как заслуживающие доверия; Имя файла — просто проявление хорошего вкуса для восприятия человеком.</span><span class="sxs-lookup"><span data-stu-id="5efae-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="5efae-113">Версия \<ключ > элемент, в настоящее время исправлено на 1.</span><span class="sxs-lookup"><span data-stu-id="5efae-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="5efae-114">Даты создания, активации и истечения срока действия ключа.</span><span class="sxs-lookup"><span data-stu-id="5efae-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="5efae-115">Объект \<дескриптора > элемент, который содержит сведения о реализации шифрование с проверкой подлинности, содержащихся в этот ключ.</span><span class="sxs-lookup"><span data-stu-id="5efae-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="5efae-116">В приведенном выше примере идентификатор ключа — {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, ее создания и активации 19 марта 2015 г., и он имеет время существования 90 дней.</span><span class="sxs-lookup"><span data-stu-id="5efae-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="5efae-117">(Иногда дату активации может быть немного до даты создания, как в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="5efae-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="5efae-118">Это вызвано nit в том, как API-интерфейсы работают и не оказывает отрицательного воздействия на практике.)</span><span class="sxs-lookup"><span data-stu-id="5efae-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="5efae-119">\<Дескриптора > элемент</span><span class="sxs-lookup"><span data-stu-id="5efae-119">The \<descriptor> element</span></span>

<span data-ttu-id="5efae-120">Внешний \<дескриптора > элемент содержит атрибут deserializerType, которое является именем типа, который реализует IAuthenticatedEncryptorDescriptorDeserializer с указанием сборки.</span><span class="sxs-lookup"><span data-stu-id="5efae-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="5efae-121">Этот тип отвечает за чтение внутреннего \<дескриптора > элемента и для синтаксического анализа сведений, содержащихся в.</span><span class="sxs-lookup"><span data-stu-id="5efae-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="5efae-122">Определенный формат \<дескриптора > элемент зависит от реализации прошедшего проверку подлинности шифратора, инкапсулированный с помощью ключа, и каждый тип десериализатор ожидает другой формат для этого.</span><span class="sxs-lookup"><span data-stu-id="5efae-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="5efae-123">Вообще говоря, однако этот элемент будет содержать алгоритмического сведения (имена, типы, OID, или аналогичные) и секретного ключа.</span><span class="sxs-lookup"><span data-stu-id="5efae-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="5efae-124">В приведенном выше примере дескриптор перенос этот ключ шифрования AES-256-CBC + HMACSHA256 проверки.</span><span class="sxs-lookup"><span data-stu-id="5efae-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="5efae-125">\<EncryptedSecret > элемент</span><span class="sxs-lookup"><span data-stu-id="5efae-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="5efae-126">**&lt;EncryptedSecret&gt;** элемент, который содержит зашифрованный материал секретного ключа могут присутствовать Если [включено шифрование секретов при хранении](xref:security/data-protection/implementation/key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="5efae-126">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="5efae-127">Атрибут `decryptorType` квалифицированное имя типа, который реализует [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span><span class="sxs-lookup"><span data-stu-id="5efae-127">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="5efae-128">Этот тип отвечает за чтение внутреннего **&lt;encryptedKey&gt;** элемент и расшифровывает его, чтобы восстановить исходным открытым текстом.</span><span class="sxs-lookup"><span data-stu-id="5efae-128">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="5efae-129">Как и в \<дескриптора >, определенный формат <encryptedSecret> элемент зависит от механизма шифрования неактивных данных используется.</span><span class="sxs-lookup"><span data-stu-id="5efae-129">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="5efae-130">В приведенном выше примере главный ключ зашифрован с помощью Windows DPAPI в комментарий.</span><span class="sxs-lookup"><span data-stu-id="5efae-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="5efae-131">\<Отзыва > элемент</span><span class="sxs-lookup"><span data-stu-id="5efae-131">The \<revocation> element</span></span>

<span data-ttu-id="5efae-132">Отзывы существуют в виде объектов верхнего уровня в хранилище ключей.</span><span class="sxs-lookup"><span data-stu-id="5efae-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="5efae-133">По соглашению имя файла быть переработаны **отзыва-{timestamp} .xml** (для отзыва всех ключей до определенной даты) или **отзыва-{guid} .xml** (для отзыва определенного ключа).</span><span class="sxs-lookup"><span data-stu-id="5efae-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="5efae-134">Каждый файл содержит один \<отзыва > элемента.</span><span class="sxs-lookup"><span data-stu-id="5efae-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="5efae-135">Для отзыва сертификатов отдельных ключей будет содержимое файла следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5efae-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="5efae-136">В этом случае аннулируются только указанный ключ.</span><span class="sxs-lookup"><span data-stu-id="5efae-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="5efae-137">Если идентификатор ключа «\*», тем не менее, как показано на приведенном ниже примере, отменяются все ключи, Дата создания — до даты указанного отзыва.</span><span class="sxs-lookup"><span data-stu-id="5efae-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="5efae-138">\<Причина > элемента не было прочитано системой.</span><span class="sxs-lookup"><span data-stu-id="5efae-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="5efae-139">Это просто удобное место для хранения читаемую пользователем причину отзыва.</span><span class="sxs-lookup"><span data-stu-id="5efae-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
