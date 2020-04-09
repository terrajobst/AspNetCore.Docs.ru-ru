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
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="08bad-103">Ключевой формат хранения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08bad-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="08bad-104">Объекты хранятся в состоянии покоя в представлении XML.</span><span class="sxs-lookup"><span data-stu-id="08bad-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="08bad-105">Каталог по умолчанию для хранилища ключей:</span><span class="sxs-lookup"><span data-stu-id="08bad-105">The default directory for key storage is:</span></span>

* <span data-ttu-id="08bad-106">Окна: «%LOCALAPPDATA%»ASP.NET-DataProtection-Keys\*</span><span class="sxs-lookup"><span data-stu-id="08bad-106">Windows: \*%LOCALAPPDATA%\ASP.NET\DataProtection-Keys\*</span></span>
* <span data-ttu-id="08bad-107">macOS / Linux: *$HOME/.aspnet/DataProtection-Keys*</span><span class="sxs-lookup"><span data-stu-id="08bad-107">macOS / Linux: *$HOME/.aspnet/DataProtection-Keys*</span></span>

## <a name="the-key-element"></a><span data-ttu-id="08bad-108">Ключевой \<элемент></span><span class="sxs-lookup"><span data-stu-id="08bad-108">The \<key> element</span></span>

<span data-ttu-id="08bad-109">Ключи существуют как объекты верхнего уровня в репозитории ключа.</span><span class="sxs-lookup"><span data-stu-id="08bad-109">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="08bad-110">По клавишам конвенции есть ключ файла **-'guid'.xml**, где «guid» является идентификатором ключа.</span><span class="sxs-lookup"><span data-stu-id="08bad-110">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="08bad-111">Каждый такой файл содержит один ключ.</span><span class="sxs-lookup"><span data-stu-id="08bad-111">Each such file contains a single key.</span></span> <span data-ttu-id="08bad-112">Формат файла следующий.</span><span class="sxs-lookup"><span data-stu-id="08bad-112">The format of the file is as follows.</span></span>

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

<span data-ttu-id="08bad-113">Ключевой \<элемент> содержит следующие атрибуты и элементы ребенка:</span><span class="sxs-lookup"><span data-stu-id="08bad-113">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="08bad-114">Ключевой идентификатор. Это значение рассматривается как авторитетное; имя файла просто тонкость для человеческой читаемости.</span><span class="sxs-lookup"><span data-stu-id="08bad-114">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="08bad-115">Версия ключевого \<элемента>, в настоящее время зафиксирована на уровне 1.</span><span class="sxs-lookup"><span data-stu-id="08bad-115">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="08bad-116">Сроки создания ключа, активации и истечения срока действия.</span><span class="sxs-lookup"><span data-stu-id="08bad-116">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="08bad-117">Дескриптор \<> элемент, содержащий информацию о реализации аутентифицированного шифрования, содержащуюся в этом ключе.</span><span class="sxs-lookup"><span data-stu-id="08bad-117">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="08bad-118">В приведенном выше примере идентификатор ключа: 80732141-ec8f-4b80-af9c-c4d2d1ff8901, он был создан и активирован 19 марта 2015 года, и срок службы составляет 90 дней.</span><span class="sxs-lookup"><span data-stu-id="08bad-118">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="08bad-119">(Иногда дата активации может быть немного до даты создания, как в этом примере.</span><span class="sxs-lookup"><span data-stu-id="08bad-119">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="08bad-120">Это связано с гнидой в том, как работают AA и безвредны на практике.)</span><span class="sxs-lookup"><span data-stu-id="08bad-120">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="08bad-121">Элемент \<дескриптора></span><span class="sxs-lookup"><span data-stu-id="08bad-121">The \<descriptor> element</span></span>

<span data-ttu-id="08bad-122">Внешний \<дескриптор> элемент содержит атрибут deserializerType, который является сборочно-квалифицированным названием типа, который реализует IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="08bad-122">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="08bad-123">Этот тип отвечает за \<чтение внутреннего дескриптора> элемента и для разбора информации, содержащейся внутри.</span><span class="sxs-lookup"><span data-stu-id="08bad-123">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="08bad-124">Конкретный формат \<элемента дескриптора> зависит от аутентифицированной реализации шифрования, инкапсулированной ключом, и каждый тип deserializer ожидает немного иного формата для этого.</span><span class="sxs-lookup"><span data-stu-id="08bad-124">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="08bad-125">Однако в целом этот элемент будет содержать алгоритмическую информацию (имена, типы, OID или аналогичные) и секретный ключевой материал.</span><span class="sxs-lookup"><span data-stu-id="08bad-125">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="08bad-126">В приведенном выше примере дескриптор указывает, что этот ключ обертывает шифрование AES-256-CBC и проверку HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="08bad-126">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="08bad-127">Зашифрованный элемент Секретной \<></span><span class="sxs-lookup"><span data-stu-id="08bad-127">The \<encryptedSecret> element</span></span>

<span data-ttu-id="08bad-128">\*\* &lt;Зашифрованный секретный&gt; \*\* элемент, содержащий зашифрованную форму секретного ключевого материала, может присутствовать, если [включено шифрование секретов в состоянии покоя.](xref:security/data-protection/implementation/key-encryption-at-rest)</span><span class="sxs-lookup"><span data-stu-id="08bad-128">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="08bad-129">Атрибут `decryptorType` — это сборочное имя типа, который реализует [IXmlDecryptor.](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor)</span><span class="sxs-lookup"><span data-stu-id="08bad-129">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="08bad-130">Этот тип отвечает за чтение внутреннего \*\* &lt;зашифрованного&gt; \*\* элемента Key и расшифровку его для восстановления исходного простого текста.</span><span class="sxs-lookup"><span data-stu-id="08bad-130">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="08bad-131">Как `<descriptor>`и в остальном, конкретный формат элемента `<encryptedSecret>` зависит от используемого механизма шифрования на месте.</span><span class="sxs-lookup"><span data-stu-id="08bad-131">As with `<descriptor>`, the particular format of the `<encryptedSecret>` element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="08bad-132">В приведенном выше примере мастер-ключ шифруется с помощью Windows DPAPI за комментарий.</span><span class="sxs-lookup"><span data-stu-id="08bad-132">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="08bad-133">\<Отзыв> элемент</span><span class="sxs-lookup"><span data-stu-id="08bad-133">The \<revocation> element</span></span>

<span data-ttu-id="08bad-134">Отзывы существуют как объекты верхнего уровня в репозитории ключа.</span><span class="sxs-lookup"><span data-stu-id="08bad-134">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="08bad-135">По отмене конвенции имеют **сярвый отзыв файла -timestamp.xml** (для отзыва всех ключей до определенной даты) или **отзыв-.guid.xml** (для отзыва определенного ключа).</span><span class="sxs-lookup"><span data-stu-id="08bad-135">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="08bad-136">Каждый файл \<содержит один элемент отзыва>.</span><span class="sxs-lookup"><span data-stu-id="08bad-136">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="08bad-137">Для отзыва отдельных ключей содержимое файла будет ниже.</span><span class="sxs-lookup"><span data-stu-id="08bad-137">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="08bad-138">В этом случае отменяется только указанный ключ.</span><span class="sxs-lookup"><span data-stu-id="08bad-138">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="08bad-139">Однако, если ключевой идентификатор —«К», то, как в приведенном ниже примере, все ключи, дата создания которых до указанной даты отзыва, аннулируются.</span><span class="sxs-lookup"><span data-stu-id="08bad-139">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="08bad-140">Причина, \<по которой элемент> никогда не читается системой.</span><span class="sxs-lookup"><span data-stu-id="08bad-140">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="08bad-141">Это просто удобное место для хранения читаемой человеком причины отзыва.</span><span class="sxs-lookup"><span data-stu-id="08bad-141">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
