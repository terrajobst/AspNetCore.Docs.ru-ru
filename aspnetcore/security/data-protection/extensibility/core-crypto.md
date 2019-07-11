---
title: Расширяемость базового шифрования в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer и фабрику верхнего уровня.
ms.author: riande
ms.date: 08/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: a5f651e3313cc579b995b45905826a5bffcc241c
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814693"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="5b150-103">Расширяемость базового шифрования в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b150-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="5b150-104">Типы, реализующие любые из следующих интерфейсов должны быть потокобезопасными для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="5b150-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="5b150-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5b150-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="5b150-106">**IAuthenticatedEncryptor** интерфейс является основной строительный блок подсистема шифрования.</span><span class="sxs-lookup"><span data-stu-id="5b150-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="5b150-107">Обычно имеется один IAuthenticatedEncryptor на ключ, и экземпляр IAuthenticatedEncryptor переносит все материал криптографического ключа и алгоритма сведения, необходимые для выполнения криптографических операций.</span><span class="sxs-lookup"><span data-stu-id="5b150-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="5b150-108">Как предполагает имя, тип отвечает за предоставление прошедшим проверку подлинности служб шифрования и расшифровки.</span><span class="sxs-lookup"><span data-stu-id="5b150-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="5b150-109">Он предоставляет следующие два API.</span><span class="sxs-lookup"><span data-stu-id="5b150-109">It exposes the following two APIs.</span></span>

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

<span data-ttu-id="5b150-110">Метод Encrypt возвращает большой двоичный объект, содержащий зашифрованные открытым текстом и с тегом проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5b150-110">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="5b150-111">Тег проверки подлинности должен охватывать дополнительные данные прошедшего проверку подлинности (AAD), хотя сам AAD не требуется восстановить из окончательных полезных данных.</span><span class="sxs-lookup"><span data-stu-id="5b150-111">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="5b150-112">Метод расшифровки проверяет тег проверки подлинности и возвращает deciphered полезных данных.</span><span class="sxs-lookup"><span data-stu-id="5b150-112">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="5b150-113">Все сбои (за исключением ArgumentNullException и аналогичных) должен быть homogenized для CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="5b150-113">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="5b150-114">Сам экземпляр IAuthenticatedEncryptor фактически не должен содержать материал ключа.</span><span class="sxs-lookup"><span data-stu-id="5b150-114">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="5b150-115">Например реализация может делегировать аппаратный модуль безопасности для всех операций.</span><span class="sxs-lookup"><span data-stu-id="5b150-115">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="5b150-116">Как создать IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5b150-116">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5b150-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5b150-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5b150-118">**IAuthenticatedEncryptorFactory** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра.</span><span class="sxs-lookup"><span data-stu-id="5b150-118">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="5b150-119">Ее API выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5b150-119">Its API is as follows.</span></span>

* <span data-ttu-id="5b150-120">CreateEncryptorInstance (ключ IKey): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5b150-120">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="5b150-121">Для любого заданного экземпляра IKey любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance следует учитывать эквивалент, как в следующем примере кода.</span><span class="sxs-lookup"><span data-stu-id="5b150-121">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5b150-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5b150-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5b150-123">**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра.</span><span class="sxs-lookup"><span data-stu-id="5b150-123">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="5b150-124">Ее API выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5b150-124">Its API is as follows.</span></span>

* <span data-ttu-id="5b150-125">CreateEncryptorInstance() : IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5b150-125">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="5b150-126">ExportToXml() : XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="5b150-126">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="5b150-127">Как и IAuthenticatedEncryptor экземпляр IAuthenticatedEncryptorDescriptor считается программы-оболочки для одного конкретного ключа.</span><span class="sxs-lookup"><span data-stu-id="5b150-127">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="5b150-128">Это означает, что для любого заданного экземпляра IAuthenticatedEncryptorDescriptor любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance следует учитывать эквивалент, как в следующем примере кода.</span><span class="sxs-lookup"><span data-stu-id="5b150-128">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="5b150-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x только)</span><span class="sxs-lookup"><span data-stu-id="5b150-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5b150-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5b150-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5b150-131">**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как экспортировать сама себя в XML.</span><span class="sxs-lookup"><span data-stu-id="5b150-131">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="5b150-132">Ее API выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5b150-132">Its API is as follows.</span></span>

* <span data-ttu-id="5b150-133">ExportToXml() : XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="5b150-133">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5b150-134">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5b150-134">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="5b150-135">XML-сериализация</span><span class="sxs-lookup"><span data-stu-id="5b150-135">XML Serialization</span></span>

<span data-ttu-id="5b150-136">Основное различие между IAuthenticatedEncryptor и IAuthenticatedEncryptorDescriptor является то, что дескриптор знает, как создать шифратор и задать в нем допустимые аргументы.</span><span class="sxs-lookup"><span data-stu-id="5b150-136">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="5b150-137">Рассмотрите возможность IAuthenticatedEncryptor, реализация которой зависит от того, SymmetricAlgorithm и KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="5b150-137">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="5b150-138">Задание шифратора, – это использование этих типов, но он не обязательно знать эти типы происхождения, поэтому его нельзя записывать действительно правильное описание как воссоздать сам при повторном запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="5b150-138">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="5b150-139">Дескриптор действует как более высокий уровень поверх этого.</span><span class="sxs-lookup"><span data-stu-id="5b150-139">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="5b150-140">Так как дескриптор знает, как создать экземпляр шифратор (например, он знает, как создать необходимые алгоритмы), он может сериализовать эти знания в формате XML, таким образом, чтобы экземпляр шифратора можно воссоздать после сброса приложения.</span><span class="sxs-lookup"><span data-stu-id="5b150-140">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="5b150-141">Дескриптор может быть сериализован с помощью его ExportToXml подпрограммы.</span><span class="sxs-lookup"><span data-stu-id="5b150-141">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="5b150-142">Эта процедура возвращает XmlSerializedDescriptorInfo, который содержит два свойства: это представление XElement дескриптор и тип, представляющий [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) которого может быть используется, чтобы восстановить этот дескриптор, учитывая соответствующие XElement.</span><span class="sxs-lookup"><span data-stu-id="5b150-142">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="5b150-143">Сериализованный дескриптора могут содержать конфиденциальные сведения, такие как материал криптографического ключа.</span><span class="sxs-lookup"><span data-stu-id="5b150-143">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="5b150-144">Система защиты данных имеется встроенная поддержка для шифрования данных, прежде чем он сохранен в хранилище.</span><span class="sxs-lookup"><span data-stu-id="5b150-144">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="5b150-145">Чтобы воспользоваться преимуществами, дескриптор необходимо пометить элемент, который содержит конфиденциальные данные с именем атрибута «requiresEncryption» (xmlns "<http://schemas.asp.net/2015/03/dataProtection>«), значение «true».</span><span class="sxs-lookup"><span data-stu-id="5b150-145">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="5b150-146">Нет API модуля поддержки для установки этого атрибута.</span><span class="sxs-lookup"><span data-stu-id="5b150-146">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="5b150-147">Вызовите метод расширения XElement.MarkAsRequiresEncryption(), расположенный в пространстве имен Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="5b150-147">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="5b150-148">Также можно случаев, где дескриптор сериализованный не содержит конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="5b150-148">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="5b150-149">Давайте снова рассмотрим случай криптографический ключ, хранящийся в HSM.</span><span class="sxs-lookup"><span data-stu-id="5b150-149">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="5b150-150">Дескриптор нельзя записывать материал ключа, при сериализации самой, так как аппаратный модуль безопасности не будет предоставлять материал в виде открытого текста.</span><span class="sxs-lookup"><span data-stu-id="5b150-150">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="5b150-151">Вместо этого дескриптора может записать ключ в программе-оболочке версии ключ (если аппаратный модуль безопасности позволяет выполнять экспорт таким образом) или аппаратным модулем безопасности собственный уникальный идентификатор для ключа.</span><span class="sxs-lookup"><span data-stu-id="5b150-151">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="5b150-152">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="5b150-152">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="5b150-153">**IAuthenticatedEncryptorDescriptorDeserializer** интерфейс представляет тип, который знает, как для десериализации экземпляра IAuthenticatedEncryptorDescriptor из элемента XElement.</span><span class="sxs-lookup"><span data-stu-id="5b150-153">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="5b150-154">Она предоставляет единственный метод:</span><span class="sxs-lookup"><span data-stu-id="5b150-154">It exposes a single method:</span></span>

* <span data-ttu-id="5b150-155">ImportFromXml (элемент XElement): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="5b150-155">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="5b150-156">Метод ImportFromXml принимает XElement, который был возвращен [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) и создает эквивалент исходного IAuthenticatedEncryptorDescriptor.</span><span class="sxs-lookup"><span data-stu-id="5b150-156">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="5b150-157">Типы, реализующие IAuthenticatedEncryptorDescriptorDeserializer должны иметь одно из следующих двух открытые конструкторы:</span><span class="sxs-lookup"><span data-stu-id="5b150-157">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="5b150-158">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="5b150-158">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="5b150-159">.ctor()</span><span class="sxs-lookup"><span data-stu-id="5b150-159">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="5b150-160">IServiceProvider, переданный в конструктор может иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="5b150-160">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="5b150-161">Фабрика верхнего уровня</span><span class="sxs-lookup"><span data-stu-id="5b150-161">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5b150-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5b150-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5b150-163">**AlgorithmConfiguration** класс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров.</span><span class="sxs-lookup"><span data-stu-id="5b150-163">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="5b150-164">Он предоставляет единый интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="5b150-164">It exposes a single API.</span></span>

* <span data-ttu-id="5b150-165">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="5b150-165">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="5b150-166">Считать AlgorithmConfiguration фабрики верхнего уровня.</span><span class="sxs-lookup"><span data-stu-id="5b150-166">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="5b150-167">Конфигурация служит в качестве шаблона.</span><span class="sxs-lookup"><span data-stu-id="5b150-167">The configuration serves as a template.</span></span> <span data-ttu-id="5b150-168">Он обертывает алгоритмического сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но еще не связан с указанным ключом.</span><span class="sxs-lookup"><span data-stu-id="5b150-168">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="5b150-169">При вызове CreateNewDescriptor, исключительно для этого вызова создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor оболочку этот материал ключа и алгоритма сведения, необходимые для использования материала.</span><span class="sxs-lookup"><span data-stu-id="5b150-169">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="5b150-170">Материал ключа можно создавать в программном обеспечении (и хранятся в памяти), можно создавать и хранящиеся в аппаратный модуль безопасности и т. д.</span><span class="sxs-lookup"><span data-stu-id="5b150-170">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="5b150-171">Ключевым моментом является то, что любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.</span><span class="sxs-lookup"><span data-stu-id="5b150-171">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="5b150-172">Тип AlgorithmConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического ключа обновлением](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="5b150-172">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="5b150-173">Чтобы изменить реализацию для всех будущих ключей, задайте свойство AuthenticatedEncryptorConfiguration в KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="5b150-173">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5b150-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5b150-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5b150-175">**IAuthenticatedEncryptorConfiguration** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров.</span><span class="sxs-lookup"><span data-stu-id="5b150-175">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="5b150-176">Он предоставляет единый интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="5b150-176">It exposes a single API.</span></span>

* <span data-ttu-id="5b150-177">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="5b150-177">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="5b150-178">Считать IAuthenticatedEncryptorConfiguration фабрики верхнего уровня.</span><span class="sxs-lookup"><span data-stu-id="5b150-178">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="5b150-179">Конфигурация служит в качестве шаблона.</span><span class="sxs-lookup"><span data-stu-id="5b150-179">The configuration serves as a template.</span></span> <span data-ttu-id="5b150-180">Он обертывает алгоритмического сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но еще не связан с указанным ключом.</span><span class="sxs-lookup"><span data-stu-id="5b150-180">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="5b150-181">При вызове CreateNewDescriptor, исключительно для этого вызова создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor оболочку этот материал ключа и алгоритма сведения, необходимые для использования материала.</span><span class="sxs-lookup"><span data-stu-id="5b150-181">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="5b150-182">Материал ключа можно создавать в программном обеспечении (и хранятся в памяти), можно создавать и хранящиеся в аппаратный модуль безопасности и т. д.</span><span class="sxs-lookup"><span data-stu-id="5b150-182">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="5b150-183">Ключевым моментом является то, что любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.</span><span class="sxs-lookup"><span data-stu-id="5b150-183">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="5b150-184">Тип IAuthenticatedEncryptorConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического ключа обновлением](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="5b150-184">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="5b150-185">Чтобы изменить реализацию для всех будущих ключей, зарегистрируйте одноэлементный IAuthenticatedEncryptorConfiguration в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="5b150-185">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
