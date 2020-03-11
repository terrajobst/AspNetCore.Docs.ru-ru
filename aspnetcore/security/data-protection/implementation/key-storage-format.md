---
title: Формат хранилища ключей в ASP.NET Core
author: rick-anderson
description: Сведения о реализации формата хранилища ключей ASP.NET Core Data Protection.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655000"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="3ac34-103">Формат хранилища ключей в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ac34-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="3ac34-104">Объекты хранятся в XML-представлении в неактивных объектах.</span><span class="sxs-lookup"><span data-stu-id="3ac34-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="3ac34-105">Каталогом по умолчанию для хранилища ключей является%Локалаппдата%\асп.нет\датапротектион-кэйс\.</span><span class="sxs-lookup"><span data-stu-id="3ac34-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="3ac34-106">Элемент > ключа \<</span><span class="sxs-lookup"><span data-stu-id="3ac34-106">The \<key> element</span></span>

<span data-ttu-id="3ac34-107">Ключи существуют как объекты верхнего уровня в репозитории ключей.</span><span class="sxs-lookup"><span data-stu-id="3ac34-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="3ac34-108">По ключам соглашения имеет **раздел filename-{GUID}. XML**, где {GUID} — это идентификатор ключа.</span><span class="sxs-lookup"><span data-stu-id="3ac34-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="3ac34-109">Каждый такой файл содержит один ключ.</span><span class="sxs-lookup"><span data-stu-id="3ac34-109">Each such file contains a single key.</span></span> <span data-ttu-id="3ac34-110">Файл имеет следующий формат.</span><span class="sxs-lookup"><span data-stu-id="3ac34-110">The format of the file is as follows.</span></span>

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

<span data-ttu-id="3ac34-111">Элемент \<key > содержит следующие атрибуты и дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="3ac34-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="3ac34-112">Идентификатор ключа. Это значение считается полномочным; имя файла — это просто ницети для удобочитаемости человека.</span><span class="sxs-lookup"><span data-stu-id="3ac34-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="3ac34-113">Версия элемента \<key >, в настоящее время фиксированная в 1.</span><span class="sxs-lookup"><span data-stu-id="3ac34-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="3ac34-114">Дата создания, активации и окончания срока действия ключа.</span><span class="sxs-lookup"><span data-stu-id="3ac34-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="3ac34-115">Элемент > дескриптора \<, который содержит сведения о реализации шифрования, прошедшего проверку подлинности, которая содержится в этом ключе.</span><span class="sxs-lookup"><span data-stu-id="3ac34-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="3ac34-116">В приведенном выше примере идентификатором ключа является {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, он был создан и активирован 19 марта 2015 и имеет время существования 90 дней.</span><span class="sxs-lookup"><span data-stu-id="3ac34-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="3ac34-117">(Иногда Дата активации может быть немного раньше даты создания, как в этом примере.</span><span class="sxs-lookup"><span data-stu-id="3ac34-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="3ac34-118">Это обусловлено nрекомендуетсяом работы API и небезопасным на практике.)</span><span class="sxs-lookup"><span data-stu-id="3ac34-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="3ac34-119">Элемент > дескриптора \<</span><span class="sxs-lookup"><span data-stu-id="3ac34-119">The \<descriptor> element</span></span>

<span data-ttu-id="3ac34-120">Элемент > дескриптора внешнего \<содержит атрибут Десериализертипе, который представляет собой имя типа с указанием сборки, которое реализует Иаусентикатеденкриптордескриптордесериализер.</span><span class="sxs-lookup"><span data-stu-id="3ac34-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="3ac34-121">Этот тип отвечает за чтение внутреннего дескриптора \<> элемента и для синтаксического анализа сведений, содержащихся в.</span><span class="sxs-lookup"><span data-stu-id="3ac34-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="3ac34-122">Конкретный формат элемента > дескриптора \<зависит от реализации, прошедшей проверку подлинности, которая инкапсулируется ключом, и каждый тип десериализатора ожидает для этого несколько других форматов.</span><span class="sxs-lookup"><span data-stu-id="3ac34-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="3ac34-123">Однако в общем случае этот элемент будет содержать алгоритмические сведения (имена, типы, OID и т. д.) и материал секретного ключа.</span><span class="sxs-lookup"><span data-stu-id="3ac34-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="3ac34-124">В приведенном выше примере дескриптор указывает, что этот ключ заключает в оболочку шифрование AES-256-CBC и проверку HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="3ac34-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="3ac34-125">Элемент \<Енкриптедсекрет ></span><span class="sxs-lookup"><span data-stu-id="3ac34-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="3ac34-126">Элемент **&lt;енкриптедсекрет&gt;** , содержащий зашифрованную форму материала секретного ключа, может присутствовать в случае [, если включено шифрование секретов](xref:security/data-protection/implementation/key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="3ac34-126">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="3ac34-127">Атрибут `decryptorType` является именем с указанием сборки типа, который реализует [иксмлдекриптор](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span><span class="sxs-lookup"><span data-stu-id="3ac34-127">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="3ac34-128">Этот тип отвечает за чтение внутреннего элемента **&lt;encryptedKey&gt;** и его расшифровку для восстановления исходного открытого текста.</span><span class="sxs-lookup"><span data-stu-id="3ac34-128">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="3ac34-129">Как и в случае с `<descriptor>`, конкретный формат элемента `<encryptedSecret>` зависит от используемого механизма шифрования неактивных данных.</span><span class="sxs-lookup"><span data-stu-id="3ac34-129">As with `<descriptor>`, the particular format of the `<encryptedSecret>` element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="3ac34-130">В приведенном выше примере главный ключ шифруется с помощью Windows DPAPI в соответствии с комментарием.</span><span class="sxs-lookup"><span data-stu-id="3ac34-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="3ac34-131">Элемент > отзыва \<</span><span class="sxs-lookup"><span data-stu-id="3ac34-131">The \<revocation> element</span></span>

<span data-ttu-id="3ac34-132">Отзыва существуют как объекты верхнего уровня в репозитории ключей.</span><span class="sxs-lookup"><span data-stu-id="3ac34-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="3ac34-133">При отзыве соглашения используется отзыв имени файла **: {timestamp}. XML** (для отмены всех ключей до определенной даты) или **отзыв: {GUID}. XML** (для отзыва определенного ключа).</span><span class="sxs-lookup"><span data-stu-id="3ac34-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="3ac34-134">Каждый файл содержит один элемент > \<отзыва.</span><span class="sxs-lookup"><span data-stu-id="3ac34-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="3ac34-135">Для отзыва отдельных ключей содержимое файла будет следующим.</span><span class="sxs-lookup"><span data-stu-id="3ac34-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="3ac34-136">В этом случае отменяется только указанный ключ.</span><span class="sxs-lookup"><span data-stu-id="3ac34-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="3ac34-137">Если идентификатор ключа — "\*", то, как показано в следующем примере, будут отозваны все ключи, Дата создания которых предшествует указанной дате отзыва.</span><span class="sxs-lookup"><span data-stu-id="3ac34-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="3ac34-138">Элемент > Причина \<не читается системой.</span><span class="sxs-lookup"><span data-stu-id="3ac34-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="3ac34-139">Это просто удобное место для того, чтобы сохранить понятную пользователю причину отзыва.</span><span class="sxs-lookup"><span data-stu-id="3ac34-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
