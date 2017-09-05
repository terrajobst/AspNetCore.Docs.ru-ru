---
title: "Основные расширяемости шифрования"
author: rick-anderson
description: "Описание IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer и фабрику верхнего уровня."
keywords: ASP.NET Core, IAuthenticatedEncryptorDescriptorDeserializer IAuthenticatedEncryptor IAuthenticatedEncryptorDescriptor,
ms.author: riande
manager: wpickett
ms.date: 8/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: 8ee4e380b154db7f1736edc793b56258655ddd52
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="core-cryptography-extensibility"></a><span data-ttu-id="cf3d4-104">Основные расширяемости шифрования</span><span class="sxs-lookup"><span data-stu-id="cf3d4-104">Core cryptography extensibility</span></span>

<a name=data-protection-extensibility-core-crypto></a>

>[!WARNING]
> <span data-ttu-id="cf3d4-105">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name=data-protection-extensibility-core-crypto-iauthenticatedencryptor></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="cf3d4-106">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="cf3d4-106">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="cf3d4-107">**IAuthenticatedEncryptor** интерфейс является основной строительный блок подсистема шифрования.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-107">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="cf3d4-108">Обычно есть один IAuthenticatedEncryptor каждого ключа, и экземпляр IAuthenticatedEncryptor переносит все криптографические материалы ключа и алгоритма сведения, необходимые для выполнения криптографических операций.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-108">There is generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="cf3d4-109">Как и предполагает его имя, тип отвечает за предоставление прошедшего проверку подлинности служб шифрования и расшифровки.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-109">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="cf3d4-110">Он предоставляет следующие два API.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-110">It exposes the following two APIs.</span></span>

* <span data-ttu-id="cf3d4-111">Расшифровать (ArraySegment<byte> зашифрованного текста ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="cf3d4-111">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="cf3d4-112">Шифрование (ArraySegment<byte> открытого текста, ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="cf3d4-112">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="cf3d4-113">Метод Encrypt возвращает большой двоичный объект, содержащий enciphered открытого текста и тег проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-113">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="cf3d4-114">Тег аутентификации должен охватывать дополнительных данных прошедшего проверку подлинности (AAD), хотя сам AAD не обязательно восстанавливаемых из окончательного полезных данных.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-114">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="cf3d4-115">Метод расшифровки проверяет тег проверки подлинности и возвращает deciphered полезных данных.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-115">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="cf3d4-116">Все ошибки (за исключением ArgumentNullException и аналогичных) следует homogenized для CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-116">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="cf3d4-117">Сам экземпляр IAuthenticatedEncryptor не обязательно фактически содержит материал ключа.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-117">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="cf3d4-118">Например реализация может делегировать аппаратный модуль безопасности для всех операций.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-118">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name=data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory></a>
<a name=data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="cf3d4-119">Создание IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="cf3d4-119">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cf3d4-120">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cf3d4-120">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="cf3d4-121">**IAuthenticatedEncryptorFactory** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-121">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="cf3d4-122">API-интерфейса выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-122">Its API is as follows.</span></span>

* <span data-ttu-id="cf3d4-123">CreateEncryptorInstance (IKey ключ): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="cf3d4-123">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="cf3d4-124">Для любого заданного экземпляра IKey любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance должны рассматриваться как эквивалент, как в приведенном ниже примере кода.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-124">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cf3d4-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cf3d4-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="cf3d4-126">**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-126">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="cf3d4-127">API-интерфейса выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-127">Its API is as follows.</span></span>

* <span data-ttu-id="cf3d4-128">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="cf3d4-128">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="cf3d4-129">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="cf3d4-129">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="cf3d4-130">Как IAuthenticatedEncryptor экземпляр IAuthenticatedEncryptorDescriptor предполагается программы-оболочки для одного определенного ключа.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-130">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="cf3d4-131">Это означает, что для любого заданного экземпляра IAuthenticatedEncryptorDescriptor любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance следует учитывать эквивалент, как в приведенном ниже примере кода.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-131">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name=data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="cf3d4-132">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x только)</span><span class="sxs-lookup"><span data-stu-id="cf3d4-132">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cf3d4-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cf3d4-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="cf3d4-134">**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как экспортировать сама себя XML.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-134">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="cf3d4-135">API-интерфейса выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-135">Its API is as follows.</span></span>

* <span data-ttu-id="cf3d4-136">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="cf3d4-136">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cf3d4-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cf3d4-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="cf3d4-138">XML-сериализация</span><span class="sxs-lookup"><span data-stu-id="cf3d4-138">XML Serialization</span></span>

<span data-ttu-id="cf3d4-139">Основное различие между IAuthenticatedEncryptor и IAuthenticatedEncryptorDescriptor является то, что дескриптор знает, как создать шифратор и передайте в него допустимые аргументы.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-139">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="cf3d4-140">Рассмотрите возможность IAuthenticatedEncryptor, реализация которого зависит от SymmetricAlgorithm и KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-140">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="cf3d4-141">Задание шифратор — использовать эти типы, но он не обязательно знать эти типы происхождения, поэтому его нельзя записывать действительно правильную описание как воссоздать сам при повторном запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-141">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="cf3d4-142">Дескриптор действует как более высокого уровня на основе этого.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-142">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="cf3d4-143">Так как дескриптор знает, как создать экземпляр шифратор (например, он знает, как создать необходимые алгоритмы), он может сериализовать эти знания в формате XML, чтобы экземпляр шифратора можно воссоздать после сброса приложения.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-143">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name=data-protection-extensibility-core-crypto-exporttoxml></a>

<span data-ttu-id="cf3d4-144">Дескриптор быть сериализованы посредством его ExportToXml подпрограммы.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-144">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="cf3d4-145">Эта процедура возвращает XmlSerializedDescriptorInfo, который содержит два свойства: это представление XElement дескриптора и тип, представляющий [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) которого может быть используется, чтобы восстановить этот дескриптор получает соответствующий XElement.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-145">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="cf3d4-146">Сериализованный дескриптора может содержать конфиденциальные сведения, например материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-146">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="cf3d4-147">Система защиты данных имеет встроенную поддержку для шифрования данных, прежде чем он сохранен в хранилище.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-147">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="cf3d4-148">Для использования этой возможности, дескриптор следует пометить элемент, который содержит конфиденциальную информацию с имени атрибута «requiresEncryption» (xmlns «http://schemas.asp.net/2015/03/dataProtection»), значение «true».</span><span class="sxs-lookup"><span data-stu-id="cf3d4-148">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="cf3d4-149">Нет вспомогательного API для установки этого атрибута.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-149">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="cf3d4-150">Вызов метода расширения, XElement.MarkAsRequiresEncryption(), расположенный в пространстве имен Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-150">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="cf3d4-151">Также возможны случаи, где сериализованный дескриптора не содержит конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-151">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="cf3d4-152">Снова рассмотрим случай криптографический ключ, хранящийся в HSM.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-152">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="cf3d4-153">Дескриптор не может записать материал ключа при сериализации сам, так как аппаратный модуль безопасности не будут предоставлены материалы в текстовом виде.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-153">The descriptor cannot write out the key material when serializing itself since the HSM will not expose the material in plaintext form.</span></span> <span data-ttu-id="cf3d4-154">Вместо этого дескриптора может запишите ключ в оболочку версию ключа (если аппаратный модуль безопасности позволяет выполнять экспорт таким образом) или аппаратный модуль безопасности собственный уникальный идентификатор для ключа.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-154">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name=data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="cf3d4-155">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="cf3d4-155">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="cf3d4-156">**IAuthenticatedEncryptorDescriptorDeserializer** интерфейс представляет тип, который знает, как выполнить десериализацию IAuthenticatedEncryptorDescriptor экземпляра из элемента XElement.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-156">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="cf3d4-157">Он предоставляет один метод:</span><span class="sxs-lookup"><span data-stu-id="cf3d4-157">It exposes a single method:</span></span>

* <span data-ttu-id="cf3d4-158">ImportFromXml (элемент XElement): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="cf3d4-158">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="cf3d4-159">Метод ImportFromXml принимает XElement, которое было возвращено [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) и создает эквивалентный исходного IAuthenticatedEncryptorDescriptor.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-159">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="cf3d4-160">Типы, реализующие IAuthenticatedEncryptorDescriptorDeserializer должны иметь одно из следующих двух открытые конструкторы:</span><span class="sxs-lookup"><span data-stu-id="cf3d4-160">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="cf3d4-161">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="cf3d4-161">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="cf3d4-162">.ctor()</span><span class="sxs-lookup"><span data-stu-id="cf3d4-162">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="cf3d4-163">IServiceProvider, переданный в конструктор может иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-163">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="cf3d4-164">Фабрика верхнего уровня</span><span class="sxs-lookup"><span data-stu-id="cf3d4-164">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cf3d4-165">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cf3d4-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="cf3d4-166">**AlgorithmConfiguration** класс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-166">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="cf3d4-167">Он предоставляет единый интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-167">It exposes a single API.</span></span>

* <span data-ttu-id="cf3d4-168">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="cf3d4-168">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="cf3d4-169">Считаете AlgorithmConfiguration фабрику верхнего уровня.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-169">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="cf3d4-170">Конфигурация служит в качестве шаблона.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-170">The configuration serves as a template.</span></span> <span data-ttu-id="cf3d4-171">Заключает в оболочку алгоритмической сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но она еще не связана с указанным ключом.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-171">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="cf3d4-172">При вызове CreateNewDescriptor, исключительно для этого вызова метода создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor которого переносится этот материал ключа и алгоритма сведения, необходимые для использования материала.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-172">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="cf3d4-173">Материал ключа может создания в программном обеспечении (и в памяти), может создать и находятся в аппаратный модуль безопасности и т. д.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-173">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="cf3d4-174">Важно точка находится любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-174">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="cf3d4-175">Тип AlgorithmConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического смену ключей](../implementation/key-management.md#data-protection-implementation-key-management).</span><span class="sxs-lookup"><span data-stu-id="cf3d4-175">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#data-protection-implementation-key-management).</span></span> <span data-ttu-id="cf3d4-176">Чтобы изменить реализацию для всех будущих ключей, установите свойство AuthenticatedEncryptorConfiguration в KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-176">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cf3d4-177">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cf3d4-177">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="cf3d4-178">**IAuthenticatedEncryptorConfiguration** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-178">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="cf3d4-179">Он предоставляет единый интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-179">It exposes a single API.</span></span>

* <span data-ttu-id="cf3d4-180">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="cf3d4-180">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="cf3d4-181">Считаете IAuthenticatedEncryptorConfiguration фабрику верхнего уровня.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-181">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="cf3d4-182">Конфигурация служит в качестве шаблона.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-182">The configuration serves as a template.</span></span> <span data-ttu-id="cf3d4-183">Заключает в оболочку алгоритмической сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но она еще не связана с указанным ключом.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-183">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="cf3d4-184">При вызове CreateNewDescriptor, исключительно для этого вызова метода создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor которого переносится этот материал ключа и алгоритма сведения, необходимые для использования материала.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-184">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="cf3d4-185">Материал ключа может создания в программном обеспечении (и в памяти), может создать и находятся в аппаратный модуль безопасности и т. д.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-185">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="cf3d4-186">Важно точка находится любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-186">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="cf3d4-187">Тип IAuthenticatedEncryptorConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического смену ключей](../implementation/key-management.md#data-protection-implementation-key-management).</span><span class="sxs-lookup"><span data-stu-id="cf3d4-187">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#data-protection-implementation-key-management).</span></span> <span data-ttu-id="cf3d4-188">Чтобы изменить реализацию для всех будущих ключей, зарегистрируйте одноэлементный IAuthenticatedEncryptorConfiguration в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="cf3d4-188">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
