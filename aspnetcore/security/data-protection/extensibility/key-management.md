---
title: Расширяемость управления ключами в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о расширяемости защиты данных в ASP.NET Core управление ключами.
ms.author: riande
ms.date: 11/22/2017
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 965a7ed8ca2f72a66cfe093b5978a54fea5440fd
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219320"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="4833b-103">Расширяемость управления ключами в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4833b-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="4833b-104">Чтение [управление ключами](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) раздел перед прочтением данного раздела, в том случае, как он объясняет, что некоторые основные понятия за эти API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="4833b-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="4833b-105">Типы, реализующие любые из следующих интерфейсов должны быть потокобезопасными для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="4833b-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="4833b-106">Ключ</span><span class="sxs-lookup"><span data-stu-id="4833b-106">Key</span></span>

<span data-ttu-id="4833b-107">`IKey` Интерфейс — это основные представление ключа в криптосистеме.</span><span class="sxs-lookup"><span data-stu-id="4833b-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="4833b-108">Термин используется здесь в том смысле, абстрактный, не в том смысле, литерал «ключевого материала шифрования».</span><span class="sxs-lookup"><span data-stu-id="4833b-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="4833b-109">Ключ имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="4833b-109">A key has the following properties:</span></span>

* <span data-ttu-id="4833b-110">Даты активации, создания и истечения срока действия</span><span class="sxs-lookup"><span data-stu-id="4833b-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="4833b-111">Состояние отзыва</span><span class="sxs-lookup"><span data-stu-id="4833b-111">Revocation status</span></span>

* <span data-ttu-id="4833b-112">Ключевой идентификатор (GUID)</span><span class="sxs-lookup"><span data-stu-id="4833b-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4833b-113">Кроме того `IKey` предоставляет `CreateEncryptor` метод, который может использоваться для создания [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к этому ключу.</span><span class="sxs-lookup"><span data-stu-id="4833b-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4833b-114">Кроме того `IKey` предоставляет `CreateEncryptorInstance` метод, который может использоваться для создания [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к этому ключу.</span><span class="sxs-lookup"><span data-stu-id="4833b-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="4833b-115">Не существует API для извлечения необработанных криптографических из `IKey` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="4833b-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="4833b-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="4833b-116">IKeyManager</span></span>

<span data-ttu-id="4833b-117">`IKeyManager` Интерфейс представляет объект, ответственный за общие хранилища ключей, извлечения и обработки.</span><span class="sxs-lookup"><span data-stu-id="4833b-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="4833b-118">Он предоставляет три высокоуровневых операций:</span><span class="sxs-lookup"><span data-stu-id="4833b-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="4833b-119">Создайте новый ключ и сохранить их в хранилище.</span><span class="sxs-lookup"><span data-stu-id="4833b-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="4833b-120">Получите все ключи из хранилища.</span><span class="sxs-lookup"><span data-stu-id="4833b-120">Get all keys from storage.</span></span>

* <span data-ttu-id="4833b-121">Сохранять сведения отзыва в хранилище и отозвать один или несколько ключей.</span><span class="sxs-lookup"><span data-stu-id="4833b-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="4833b-122">Написание `IKeyManager` — очень сложная задача, и большинство разработчиков не следует пытаться выполнить ее.</span><span class="sxs-lookup"><span data-stu-id="4833b-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="4833b-123">Вместо этого, большинство разработчиков следует воспользоваться функциональными возможностями [XmlKeyManager](#xmlkeymanager) класса.</span><span class="sxs-lookup"><span data-stu-id="4833b-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="4833b-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="4833b-124">XmlKeyManager</span></span>

<span data-ttu-id="4833b-125">`XmlKeyManager` Измеряется в поле конкретная реализация `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="4833b-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="4833b-126">Он предоставляет несколько полезных набор возможностей, включая перенос ключа, а также шифрование неактивных ключей.</span><span class="sxs-lookup"><span data-stu-id="4833b-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="4833b-127">Ключи в этой системе представлены в виде XML-элементы (в частности, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="4833b-127">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="4833b-128">`XmlKeyManager` зависит от нескольких компонентов во время выполнении своих задач.</span><span class="sxs-lookup"><span data-stu-id="4833b-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="4833b-129">`AlgorithmConfiguration`, который определяет алгоритмы, используемые новые ключи.</span><span class="sxs-lookup"><span data-stu-id="4833b-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="4833b-130">`IXmlRepository`, какие элементы управления, где ключи сохраняются в хранилище.</span><span class="sxs-lookup"><span data-stu-id="4833b-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="4833b-131">`IXmlEncryptor` [необязательно], который позволяет шифровать неактивных ключей.</span><span class="sxs-lookup"><span data-stu-id="4833b-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="4833b-132">`IKeyEscrowSink` [необязательно], которая предоставляет службы доверительного хранения ключа.</span><span class="sxs-lookup"><span data-stu-id="4833b-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="4833b-133">`IXmlRepository`, какие элементы управления, где ключи сохраняются в хранилище.</span><span class="sxs-lookup"><span data-stu-id="4833b-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="4833b-134">`IXmlEncryptor` [необязательно], который позволяет шифровать неактивных ключей.</span><span class="sxs-lookup"><span data-stu-id="4833b-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="4833b-135">`IKeyEscrowSink` [необязательно], которая предоставляет службы доверительного хранения ключа.</span><span class="sxs-lookup"><span data-stu-id="4833b-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="4833b-136">Ниже перечислены высокоуровневые диаграммы, которые указывают, как эти компоненты при объединении в `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="4833b-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Создание ключа](key-management/_static/keycreation2.png)

<span data-ttu-id="4833b-138">*Создание ключа / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="4833b-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="4833b-139">В реализации `CreateNewKey`, `AlgorithmConfiguration` компонент используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется как XML.</span><span class="sxs-lookup"><span data-stu-id="4833b-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="4833b-140">Если присутствует приемник доверительного хранения ключа, необработанный код XML (незашифрованного) предоставляется в приемник для долговременного хранения.</span><span class="sxs-lookup"><span data-stu-id="4833b-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="4833b-141">Незашифрованные XML этого выполняется `IXmlEncryptor` (при необходимости) для создания зашифрованного XML-документа.</span><span class="sxs-lookup"><span data-stu-id="4833b-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="4833b-142">Этот зашифрованный документ сохраняется в долговременном хранилище с помощью `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="4833b-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="4833b-143">(Если не `IXmlEncryptor` — настройки незашифрованные документ сохраняется в `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="4833b-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Получение ключа](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Создание ключа](key-management/_static/keycreation1.png)

<span data-ttu-id="4833b-146">*Создание ключа / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="4833b-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="4833b-147">В реализации `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` компонент используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется как XML.</span><span class="sxs-lookup"><span data-stu-id="4833b-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="4833b-148">Если присутствует приемник доверительного хранения ключа, необработанный код XML (незашифрованного) предоставляется в приемник для долговременного хранения.</span><span class="sxs-lookup"><span data-stu-id="4833b-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="4833b-149">Незашифрованные XML этого выполняется `IXmlEncryptor` (при необходимости) для создания зашифрованного XML-документа.</span><span class="sxs-lookup"><span data-stu-id="4833b-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="4833b-150">Этот зашифрованный документ сохраняется в долговременном хранилище с помощью `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="4833b-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="4833b-151">(Если не `IXmlEncryptor` — настройки незашифрованные документ сохраняется в `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="4833b-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Получение ключа](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="4833b-153">*Получение ключа / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="4833b-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="4833b-154">В реализации `GetAllKeys`, представляющих ключи документов XML и выдачу считываются из основного `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="4833b-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="4833b-155">Если эти документы шифруются, система автоматически расшифровывает их.</span><span class="sxs-lookup"><span data-stu-id="4833b-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="4833b-156">`XmlKeyManager` создает соответствующий `IAuthenticatedEncryptorDescriptorDeserializer` экземпляры десериализовать документы обратно в `IAuthenticatedEncryptorDescriptor` экземпляров, которые затем упаковываются в отдельных `IKey` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="4833b-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="4833b-157">Эта коллекция `IKey` экземпляров возвращается вызывающему объекту.</span><span class="sxs-lookup"><span data-stu-id="4833b-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="4833b-158">Дополнительные сведения об отдельных элементов XML можно найти в [хранилища ключей форматировать документ](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="4833b-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="4833b-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="4833b-159">IXmlRepository</span></span>

<span data-ttu-id="4833b-160">`IXmlRepository` Интерфейс представляет тип, который можно XML для сохранения и извлечения XML из резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="4833b-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="4833b-161">Он предоставляет два API:</span><span class="sxs-lookup"><span data-stu-id="4833b-161">It exposes two APIs:</span></span>

* <span data-ttu-id="4833b-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="4833b-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="4833b-163">Реализации `IXmlRepository` не требуется синтаксический анализ XML, проходящие через них.</span><span class="sxs-lookup"><span data-stu-id="4833b-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="4833b-164">Они должны обрабатывать XML-документов как непрозрачный и позволить более высоких уровнях беспокоиться о создании и анализе документов.</span><span class="sxs-lookup"><span data-stu-id="4833b-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="4833b-165">Существует четыре встроенных конкретные типы, реализующие `IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="4833b-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

* [<span data-ttu-id="4833b-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="4833b-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="4833b-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="4833b-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="4833b-168">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="4833b-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="4833b-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="4833b-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

<span data-ttu-id="4833b-170">См. в разделе [документа поставщиков хранилища ключей](xref:security/data-protection/implementation/key-storage-providers) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="4833b-170">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="4833b-171">Регистрация пользовательского `IXmlRepository` подходит при использовании резервных хранилищ (например, хранилище таблиц Azure).</span><span class="sxs-lookup"><span data-stu-id="4833b-171">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="4833b-172">Чтобы изменить репозитория по умолчанию уровня приложения, зарегистрируйте пользовательский `IXmlRepository` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="4833b-172">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a><span data-ttu-id="4833b-173">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="4833b-173">IXmlEncryptor</span></span>

<span data-ttu-id="4833b-174">`IXmlEncryptor` Интерфейс представляет тип, который можно зашифровать XML-элемент открытого текста.</span><span class="sxs-lookup"><span data-stu-id="4833b-174">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="4833b-175">Он предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="4833b-175">It exposes a single API:</span></span>

* <span data-ttu-id="4833b-176">Шифрование (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="4833b-176">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="4833b-177">Если сериализованный `IAuthenticatedEncryptorDescriptor` содержит все элементы с пометкой «требует шифрования», затем `XmlKeyManager` запустит эти элементы настроенного `IXmlEncryptor`в `Encrypt` метод и он будет сохранять зашифрованные элемент, а не открытый текст элемента `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="4833b-177">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="4833b-178">Выходные данные `Encrypt` метод `EncryptedXmlInfo` объекта.</span><span class="sxs-lookup"><span data-stu-id="4833b-178">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="4833b-179">Этот объект является оболочкой, содержащий оба результирующих зашифрованные `XElement` и тип, представляющий `IXmlDecryptor` которого может использоваться для расшифровки соответствующий элемент.</span><span class="sxs-lookup"><span data-stu-id="4833b-179">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="4833b-180">Существует четыре встроенных конкретные типы, реализующие `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="4833b-180">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="4833b-181">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="4833b-181">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="4833b-182">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="4833b-182">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="4833b-183">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="4833b-183">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="4833b-184">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="4833b-184">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="4833b-185">См. в разделе [шифрование ключей при документа rest](xref:security/data-protection/implementation/key-encryption-at-rest) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="4833b-185">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="4833b-186">Чтобы изменить ключ шифрования при хранении механизма по умолчанию уровня приложения, зарегистрируйте пользовательский `IXmlEncryptor` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="4833b-186">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a><span data-ttu-id="4833b-187">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="4833b-187">IXmlDecryptor</span></span>

<span data-ttu-id="4833b-188">`IXmlDecryptor` Интерфейс представляет тип, который знает, как для расшифровки `XElement` , был зашифрованные с помощью `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="4833b-188">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="4833b-189">Он предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="4833b-189">It exposes a single API:</span></span>

* <span data-ttu-id="4833b-190">Расшифровать (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="4833b-190">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="4833b-191">`Decrypt` Метод отменяет шифрования, выполняемые `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="4833b-191">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="4833b-192">Как правило, каждый конкретный `IXmlEncryptor` реализация будет иметь соответствующий устойчивый `IXmlDecryptor` реализации.</span><span class="sxs-lookup"><span data-stu-id="4833b-192">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="4833b-193">Типы, которые реализуют интерфейс `IXmlDecryptor` должны иметь одно из следующих двух открытые конструкторы:</span><span class="sxs-lookup"><span data-stu-id="4833b-193">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="4833b-194">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="4833b-194">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="4833b-195">.ctor()</span><span class="sxs-lookup"><span data-stu-id="4833b-195">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="4833b-196">`IServiceProvider` Передается конструктору может иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="4833b-196">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="4833b-197">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="4833b-197">IKeyEscrowSink</span></span>

<span data-ttu-id="4833b-198">`IKeyEscrowSink` Интерфейс представляет тип, который можно выполнить перенос конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="4833b-198">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="4833b-199">Отозвать сериализованный дескрипторы могут содержать конфиденциальные сведения (например, криптографическим материалом), что это, что привело к появлением [IXmlEncryptor](#ixmlencryptor) введите в первую очередь.</span><span class="sxs-lookup"><span data-stu-id="4833b-199">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="4833b-200">Тем не менее иногда происходят аварии и колец ключ можно удалить или поврежден.</span><span class="sxs-lookup"><span data-stu-id="4833b-200">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="4833b-201">Интерфейс доверительного хранения обеспечивает аварийного escape-штрих, предоставляя доступ к необработанные сериализованный XML, перед его преобразованием каким-либо настроить [IXmlEncryptor](#ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="4833b-201">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="4833b-202">Данный интерфейс предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="4833b-202">The interface exposes a single API:</span></span>

* <span data-ttu-id="4833b-203">Store (идентификатор Guid ключа, элемент XElement)</span><span class="sxs-lookup"><span data-stu-id="4833b-203">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="4833b-204">О `IKeyEscrowSink` реализации для обработки указанного элемента в безопасной форме, совместимой с бизнес-политике.</span><span class="sxs-lookup"><span data-stu-id="4833b-204">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="4833b-205">Может быть одна возможная реализация для приемника доверительного хранения для шифрования элемента XML при помощи известных корпоративный сертификат X.509, где депонирован закрытый ключ сертификата; `CertificateXmlEncryptor` типа может помочь с данным.</span><span class="sxs-lookup"><span data-stu-id="4833b-205">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="4833b-206">`IKeyEscrowSink` Реализации также отвечает за сохранение указанного элемента соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="4833b-206">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="4833b-207">По умолчанию включен механизм доверительного хранения, то, что администраторы сервера могут [глобально эту настройку](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="4833b-207">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="4833b-208">Его также можно настроить программно с помощью `IDataProtectionBuilder.AddKeyEscrowSink` метод, как показано в приведенном ниже примере.</span><span class="sxs-lookup"><span data-stu-id="4833b-208">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="4833b-209">`AddKeyEscrowSink` Зеркальный перегрузки метода `IServiceCollection.AddSingleton` и `IServiceCollection.AddInstance` перегрузок, как `IKeyEscrowSink` экземпляры должны быть одноэлементных экземпляров.</span><span class="sxs-lookup"><span data-stu-id="4833b-209">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="4833b-210">При наличии нескольких `IKeyEscrowSink` зарегистрированных экземпляров, каждый из них будет вызываться во время создания ключа, поэтому ключи может быть передан в несколько механизмов, одновременно.</span><span class="sxs-lookup"><span data-stu-id="4833b-210">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="4833b-211">Не существует API на чтение материала из `IKeyEscrowSink` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="4833b-211">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="4833b-212">Это согласуется с теории проектирования механизма доверительного хранения: она предназначена предоставить доступ к доверенным центром сертификации материал ключа, и так, как приложение сам не является доверенным центром сертификации, он не должен иметь доступ к собственный депонированные материал.</span><span class="sxs-lookup"><span data-stu-id="4833b-212">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="4833b-213">В следующем примере кода показано создание и регистрация `IKeyEscrowSink` где ключи являются передан, таким образом, что их можно восстановить только члены «CONTOSODomain Admins».</span><span class="sxs-lookup"><span data-stu-id="4833b-213">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="4833b-214">Чтобы запустить этот пример, необходимо быть на присоединенных к домену Windows 8 / компьютера Windows Server 2012 и контроллер домена должен быть Windows Server 2012 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="4833b-214">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
