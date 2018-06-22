---
title: Основные расширяемости шифрования в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer и фабрику верхнего уровня.
ms.author: riande
ms.date: 8/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: 47432cfefe0a52c9f815d717f7269ec68fdb6af3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272900"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="0756d-103">Основные расширяемости шифрования в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0756d-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="0756d-104">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="0756d-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="0756d-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="0756d-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="0756d-106">**IAuthenticatedEncryptor** интерфейс является основной строительный блок подсистема шифрования.</span><span class="sxs-lookup"><span data-stu-id="0756d-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="0756d-107">Обычно есть один IAuthenticatedEncryptor каждого ключа, и экземпляр IAuthenticatedEncryptor переносит все криптографические материалы ключа и алгоритма сведения, необходимые для выполнения криптографических операций.</span><span class="sxs-lookup"><span data-stu-id="0756d-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="0756d-108">Как и предполагает его имя, тип отвечает за предоставление прошедшего проверку подлинности служб шифрования и расшифровки.</span><span class="sxs-lookup"><span data-stu-id="0756d-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="0756d-109">Он предоставляет следующие два API.</span><span class="sxs-lookup"><span data-stu-id="0756d-109">It exposes the following two APIs.</span></span>

* <span data-ttu-id="0756d-110">Расшифровать (ArraySegment<byte> зашифрованного текста ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="0756d-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="0756d-111">Шифрование (ArraySegment<byte> открытого текста, ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="0756d-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="0756d-112">Метод Encrypt возвращает большой двоичный объект, содержащий enciphered открытого текста и тег проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0756d-112">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="0756d-113">Тег аутентификации должен охватывать дополнительных данных прошедшего проверку подлинности (AAD), хотя сам AAD не обязательно восстанавливаемых из окончательного полезных данных.</span><span class="sxs-lookup"><span data-stu-id="0756d-113">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="0756d-114">Метод расшифровки проверяет тег проверки подлинности и возвращает deciphered полезных данных.</span><span class="sxs-lookup"><span data-stu-id="0756d-114">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="0756d-115">Все ошибки (за исключением ArgumentNullException и аналогичных) следует homogenized для CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="0756d-115">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="0756d-116">Сам экземпляр IAuthenticatedEncryptor не обязательно фактически содержит материал ключа.</span><span class="sxs-lookup"><span data-stu-id="0756d-116">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="0756d-117">Например реализация может делегировать аппаратный модуль безопасности для всех операций.</span><span class="sxs-lookup"><span data-stu-id="0756d-117">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="0756d-118">Создание IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="0756d-118">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0756d-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0756d-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0756d-120">**IAuthenticatedEncryptorFactory** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра.</span><span class="sxs-lookup"><span data-stu-id="0756d-120">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="0756d-121">API-интерфейса выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="0756d-121">Its API is as follows.</span></span>

* <span data-ttu-id="0756d-122">CreateEncryptorInstance (IKey ключ): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="0756d-122">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="0756d-123">Для любого заданного экземпляра IKey любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance должны рассматриваться как эквивалент, как в приведенном ниже примере кода.</span><span class="sxs-lookup"><span data-stu-id="0756d-123">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0756d-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0756d-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0756d-125">**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра.</span><span class="sxs-lookup"><span data-stu-id="0756d-125">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="0756d-126">API-интерфейса выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="0756d-126">Its API is as follows.</span></span>

* <span data-ttu-id="0756d-127">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="0756d-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="0756d-128">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="0756d-128">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="0756d-129">Как IAuthenticatedEncryptor экземпляр IAuthenticatedEncryptorDescriptor предполагается программы-оболочки для одного определенного ключа.</span><span class="sxs-lookup"><span data-stu-id="0756d-129">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="0756d-130">Это означает, что для любого заданного экземпляра IAuthenticatedEncryptorDescriptor любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance следует учитывать эквивалент, как в приведенном ниже примере кода.</span><span class="sxs-lookup"><span data-stu-id="0756d-130">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="0756d-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x только)</span><span class="sxs-lookup"><span data-stu-id="0756d-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0756d-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0756d-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0756d-133">**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как экспортировать сама себя XML.</span><span class="sxs-lookup"><span data-stu-id="0756d-133">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="0756d-134">API-интерфейса выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="0756d-134">Its API is as follows.</span></span>

* <span data-ttu-id="0756d-135">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="0756d-135">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0756d-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0756d-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="0756d-137">XML-сериализация</span><span class="sxs-lookup"><span data-stu-id="0756d-137">XML Serialization</span></span>

<span data-ttu-id="0756d-138">Основное различие между IAuthenticatedEncryptor и IAuthenticatedEncryptorDescriptor является то, что дескриптор знает, как создать шифратор и передайте в него допустимые аргументы.</span><span class="sxs-lookup"><span data-stu-id="0756d-138">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="0756d-139">Рассмотрите возможность IAuthenticatedEncryptor, реализация которого зависит от SymmetricAlgorithm и KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="0756d-139">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="0756d-140">Задание шифратор — использовать эти типы, но он не обязательно знать эти типы происхождения, поэтому его нельзя записывать действительно правильную описание как воссоздать сам при повторном запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="0756d-140">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="0756d-141">Дескриптор действует как более высокого уровня на основе этого.</span><span class="sxs-lookup"><span data-stu-id="0756d-141">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="0756d-142">Так как дескриптор знает, как создать экземпляр шифратор (например, он знает, как создать необходимые алгоритмы), он может сериализовать эти знания в формате XML, чтобы экземпляр шифратора можно воссоздать после сброса приложения.</span><span class="sxs-lookup"><span data-stu-id="0756d-142">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="0756d-143">Дескриптор быть сериализованы посредством его ExportToXml подпрограммы.</span><span class="sxs-lookup"><span data-stu-id="0756d-143">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="0756d-144">Эта процедура возвращает XmlSerializedDescriptorInfo, который содержит два свойства: это представление XElement дескриптора и тип, представляющий [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) которого может быть используется, чтобы восстановить этот дескриптор получает соответствующий XElement.</span><span class="sxs-lookup"><span data-stu-id="0756d-144">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="0756d-145">Сериализованный дескриптора может содержать конфиденциальные сведения, например материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="0756d-145">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="0756d-146">Система защиты данных имеет встроенную поддержку для шифрования данных, прежде чем он сохранен в хранилище.</span><span class="sxs-lookup"><span data-stu-id="0756d-146">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="0756d-147">Для использования этой возможности, дескриптор необходимо пометить элемент, который содержит конфиденциальную информацию с имени атрибута «requiresEncryption» (xmlns»<http://schemas.asp.net/2015/03/dataProtection>»), значение «true».</span><span class="sxs-lookup"><span data-stu-id="0756d-147">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="0756d-148">Нет вспомогательного API для установки этого атрибута.</span><span class="sxs-lookup"><span data-stu-id="0756d-148">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="0756d-149">Вызов метода расширения, XElement.MarkAsRequiresEncryption(), расположенный в пространстве имен Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="0756d-149">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="0756d-150">Также возможны случаи, где сериализованный дескриптора не содержит конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="0756d-150">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="0756d-151">Снова рассмотрим случай криптографический ключ, хранящийся в HSM.</span><span class="sxs-lookup"><span data-stu-id="0756d-151">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="0756d-152">Дескриптор не может записать материал ключа при сериализации сам, так как аппаратный модуль безопасности не будет предоставлять материал в текстовом виде.</span><span class="sxs-lookup"><span data-stu-id="0756d-152">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="0756d-153">Вместо этого дескриптора может запишите ключ в оболочку версию ключа (если аппаратный модуль безопасности позволяет выполнять экспорт таким образом) или аппаратный модуль безопасности собственный уникальный идентификатор для ключа.</span><span class="sxs-lookup"><span data-stu-id="0756d-153">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="0756d-154">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="0756d-154">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="0756d-155">**IAuthenticatedEncryptorDescriptorDeserializer** интерфейс представляет тип, который знает, как выполнить десериализацию IAuthenticatedEncryptorDescriptor экземпляра из элемента XElement.</span><span class="sxs-lookup"><span data-stu-id="0756d-155">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="0756d-156">Он предоставляет один метод:</span><span class="sxs-lookup"><span data-stu-id="0756d-156">It exposes a single method:</span></span>

* <span data-ttu-id="0756d-157">ImportFromXml (элемент XElement): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="0756d-157">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="0756d-158">Метод ImportFromXml принимает XElement, которое было возвращено [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) и создает эквивалентный исходного IAuthenticatedEncryptorDescriptor.</span><span class="sxs-lookup"><span data-stu-id="0756d-158">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="0756d-159">Типы, реализующие IAuthenticatedEncryptorDescriptorDeserializer должны иметь одно из следующих двух открытые конструкторы:</span><span class="sxs-lookup"><span data-stu-id="0756d-159">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="0756d-160">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="0756d-160">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="0756d-161">.ctor()</span><span class="sxs-lookup"><span data-stu-id="0756d-161">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="0756d-162">IServiceProvider, переданный в конструктор может иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="0756d-162">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="0756d-163">Фабрика верхнего уровня</span><span class="sxs-lookup"><span data-stu-id="0756d-163">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0756d-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0756d-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0756d-165">**AlgorithmConfiguration** класс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров.</span><span class="sxs-lookup"><span data-stu-id="0756d-165">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="0756d-166">Он предоставляет единый интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="0756d-166">It exposes a single API.</span></span>

* <span data-ttu-id="0756d-167">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="0756d-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="0756d-168">Считаете AlgorithmConfiguration фабрику верхнего уровня.</span><span class="sxs-lookup"><span data-stu-id="0756d-168">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="0756d-169">Конфигурация служит в качестве шаблона.</span><span class="sxs-lookup"><span data-stu-id="0756d-169">The configuration serves as a template.</span></span> <span data-ttu-id="0756d-170">Заключает в оболочку алгоритмической сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но еще не связан с указанным ключом.</span><span class="sxs-lookup"><span data-stu-id="0756d-170">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="0756d-171">При вызове CreateNewDescriptor, исключительно для этого вызова метода создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor которого переносится этот материал ключа и алгоритма сведения, необходимые для использования материала.</span><span class="sxs-lookup"><span data-stu-id="0756d-171">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="0756d-172">Материал ключа может создания в программном обеспечении (и в памяти), может создать и находятся в аппаратный модуль безопасности и т. д.</span><span class="sxs-lookup"><span data-stu-id="0756d-172">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="0756d-173">Важно точка находится любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.</span><span class="sxs-lookup"><span data-stu-id="0756d-173">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="0756d-174">Тип AlgorithmConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического смену ключей](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="0756d-174">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="0756d-175">Чтобы изменить реализацию для всех будущих ключей, установите свойство AuthenticatedEncryptorConfiguration в KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="0756d-175">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0756d-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0756d-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0756d-177">**IAuthenticatedEncryptorConfiguration** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров.</span><span class="sxs-lookup"><span data-stu-id="0756d-177">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="0756d-178">Он предоставляет единый интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="0756d-178">It exposes a single API.</span></span>

* <span data-ttu-id="0756d-179">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="0756d-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="0756d-180">Считаете IAuthenticatedEncryptorConfiguration фабрику верхнего уровня.</span><span class="sxs-lookup"><span data-stu-id="0756d-180">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="0756d-181">Конфигурация служит в качестве шаблона.</span><span class="sxs-lookup"><span data-stu-id="0756d-181">The configuration serves as a template.</span></span> <span data-ttu-id="0756d-182">Заключает в оболочку алгоритмической сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но еще не связан с указанным ключом.</span><span class="sxs-lookup"><span data-stu-id="0756d-182">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="0756d-183">При вызове CreateNewDescriptor, исключительно для этого вызова метода создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor которого переносится этот материал ключа и алгоритма сведения, необходимые для использования материала.</span><span class="sxs-lookup"><span data-stu-id="0756d-183">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="0756d-184">Материал ключа может создания в программном обеспечении (и в памяти), может создать и находятся в аппаратный модуль безопасности и т. д.</span><span class="sxs-lookup"><span data-stu-id="0756d-184">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="0756d-185">Важно точка находится любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.</span><span class="sxs-lookup"><span data-stu-id="0756d-185">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="0756d-186">Тип IAuthenticatedEncryptorConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического смену ключей](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="0756d-186">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="0756d-187">Чтобы изменить реализацию для всех будущих ключей, зарегистрируйте одноэлементный IAuthenticatedEncryptorConfiguration в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="0756d-187">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
