---
title: Расширяемость управления ключами в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о расширяемости защиты данных в ASP.NET Core управление ключами.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654262"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="94baf-103">Расширяемость управления ключами в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94baf-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="94baf-104">Ознакомьтесь с разделом [Управление ключами](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) перед чтением этого раздела, так как в нем объясняются основные понятия, лежащие в основе этих API-интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="94baf-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="94baf-105">Типы, реализующие любые из следующих интерфейсов должны быть потокобезопасными для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="94baf-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="94baf-106">Клавиши</span><span class="sxs-lookup"><span data-stu-id="94baf-106">Key</span></span>

<span data-ttu-id="94baf-107">Интерфейс `IKey` является базовым представлением ключа в криптосистемы.</span><span class="sxs-lookup"><span data-stu-id="94baf-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="94baf-108">Термин используется здесь в том смысле, абстрактный, не в том смысле, литерал «ключевого материала шифрования».</span><span class="sxs-lookup"><span data-stu-id="94baf-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="94baf-109">Ключ имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="94baf-109">A key has the following properties:</span></span>

* <span data-ttu-id="94baf-110">Даты активации, создания и истечения срока действия</span><span class="sxs-lookup"><span data-stu-id="94baf-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="94baf-111">Состояние отзыва.</span><span class="sxs-lookup"><span data-stu-id="94baf-111">Revocation status</span></span>

* <span data-ttu-id="94baf-112">Ключевой идентификатор (GUID)</span><span class="sxs-lookup"><span data-stu-id="94baf-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="94baf-113">Кроме того, `IKey` предоставляет метод `CreateEncryptor`, который можно использовать для создания экземпляра [иаусентикатеденкриптор](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) , привязанного к этому ключу.</span><span class="sxs-lookup"><span data-stu-id="94baf-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="94baf-114">Кроме того, `IKey` предоставляет метод `CreateEncryptorInstance`, который можно использовать для создания экземпляра [иаусентикатеденкриптор](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) , привязанного к этому ключу.</span><span class="sxs-lookup"><span data-stu-id="94baf-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="94baf-115">Нет API для получения необработанного криптографического материала из экземпляра `IKey`.</span><span class="sxs-lookup"><span data-stu-id="94baf-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="94baf-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="94baf-116">IKeyManager</span></span>

<span data-ttu-id="94baf-117">Интерфейс `IKeyManager` представляет объект, отвечающий за общее хранение, извлечение и обработку ключей.</span><span class="sxs-lookup"><span data-stu-id="94baf-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="94baf-118">Он предоставляет три высокоуровневых операций:</span><span class="sxs-lookup"><span data-stu-id="94baf-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="94baf-119">Создайте новый ключ и сохранить их в хранилище.</span><span class="sxs-lookup"><span data-stu-id="94baf-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="94baf-120">Получите все ключи из хранилища.</span><span class="sxs-lookup"><span data-stu-id="94baf-120">Get all keys from storage.</span></span>

* <span data-ttu-id="94baf-121">Сохранять сведения отзыва в хранилище и отозвать один или несколько ключей.</span><span class="sxs-lookup"><span data-stu-id="94baf-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="94baf-122">Написание `IKeyManager` является очень сложной задачей, и большинство разработчиков не должны предпринимать попытки.</span><span class="sxs-lookup"><span data-stu-id="94baf-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="94baf-123">Вместо этого большинству разработчиков следует воспользоваться возможностями, предлагаемыми классом [ксмлкэйманажер](#xmlkeymanager) .</span><span class="sxs-lookup"><span data-stu-id="94baf-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="94baf-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="94baf-124">XmlKeyManager</span></span>

<span data-ttu-id="94baf-125">Тип `XmlKeyManager` — это встроенная конкретная реализация `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="94baf-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="94baf-126">Он предоставляет несколько полезных набор возможностей, включая перенос ключа, а также шифрование неактивных ключей.</span><span class="sxs-lookup"><span data-stu-id="94baf-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="94baf-127">Ключи в этой системе представлены в виде XML-элементов (в частности, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="94baf-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="94baf-128">`XmlKeyManager` зависит от нескольких других компонентов в процессе выполнения задач.</span><span class="sxs-lookup"><span data-stu-id="94baf-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="94baf-129">`AlgorithmConfiguration`, определяющий алгоритмы, используемые новыми ключами.</span><span class="sxs-lookup"><span data-stu-id="94baf-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="94baf-130">`IXmlRepository`, определяющий, где хранятся ключи в хранилище.</span><span class="sxs-lookup"><span data-stu-id="94baf-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="94baf-131">`IXmlEncryptor` [необязательный], что позволяет шифровать ключи неактивных ключей.</span><span class="sxs-lookup"><span data-stu-id="94baf-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="94baf-132">`IKeyEscrowSink` [необязательно], который предоставляет службы депонирования ключей.</span><span class="sxs-lookup"><span data-stu-id="94baf-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="94baf-133">`IXmlRepository`, определяющий, где хранятся ключи в хранилище.</span><span class="sxs-lookup"><span data-stu-id="94baf-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="94baf-134">`IXmlEncryptor` [необязательный], что позволяет шифровать ключи неактивных ключей.</span><span class="sxs-lookup"><span data-stu-id="94baf-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="94baf-135">`IKeyEscrowSink` [необязательно], который предоставляет службы депонирования ключей.</span><span class="sxs-lookup"><span data-stu-id="94baf-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="94baf-136">Ниже приведены высокоуровневые схемы, которые показывают, как эти компоненты взаимодействуют в `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="94baf-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Создание ключа](key-management/_static/keycreation2.png)

<span data-ttu-id="94baf-138">*Создание ключа/Креатеневкэй*</span><span class="sxs-lookup"><span data-stu-id="94baf-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="94baf-139">В реализации `CreateNewKey`компонент `AlgorithmConfiguration` используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется как XML.</span><span class="sxs-lookup"><span data-stu-id="94baf-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="94baf-140">Если присутствует приемник доверительного хранения ключа, необработанный код XML (незашифрованного) предоставляется в приемник для долговременного хранения.</span><span class="sxs-lookup"><span data-stu-id="94baf-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="94baf-141">После этого незашифрованный XML выполняется с помощью `IXmlEncryptor` (при необходимости) для создания зашифрованного XML-документа.</span><span class="sxs-lookup"><span data-stu-id="94baf-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="94baf-142">Этот зашифрованный документ сохраняется в долгосрочном хранилище с помощью `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="94baf-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="94baf-143">(Если `IXmlEncryptor` не настроена, незашифрованный документ сохраняется в `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="94baf-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Получение ключа](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Создание ключа](key-management/_static/keycreation1.png)

<span data-ttu-id="94baf-146">*Создание ключа/Креатеневкэй*</span><span class="sxs-lookup"><span data-stu-id="94baf-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="94baf-147">В реализации `CreateNewKey`компонент `IAuthenticatedEncryptorConfiguration` используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется как XML.</span><span class="sxs-lookup"><span data-stu-id="94baf-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="94baf-148">Если присутствует приемник доверительного хранения ключа, необработанный код XML (незашифрованного) предоставляется в приемник для долговременного хранения.</span><span class="sxs-lookup"><span data-stu-id="94baf-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="94baf-149">После этого незашифрованный XML выполняется с помощью `IXmlEncryptor` (при необходимости) для создания зашифрованного XML-документа.</span><span class="sxs-lookup"><span data-stu-id="94baf-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="94baf-150">Этот зашифрованный документ сохраняется в долгосрочном хранилище с помощью `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="94baf-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="94baf-151">(Если `IXmlEncryptor` не настроена, незашифрованный документ сохраняется в `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="94baf-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Получение ключа](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="94baf-153">*Получение ключа или Жеталлкэйс*</span><span class="sxs-lookup"><span data-stu-id="94baf-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="94baf-154">В реализации `GetAllKeys`XML-документы, представляющие ключи и отзыва, считываются из базовых `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="94baf-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="94baf-155">Если эти документы шифруются, система автоматически расшифровывает их.</span><span class="sxs-lookup"><span data-stu-id="94baf-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="94baf-156">`XmlKeyManager` создает соответствующие экземпляры `IAuthenticatedEncryptorDescriptorDeserializer` для десериализации документов в `IAuthenticatedEncryptorDescriptor` экземпляры, которые затем упаковываются в отдельные экземпляры `IKey`.</span><span class="sxs-lookup"><span data-stu-id="94baf-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="94baf-157">Эта коллекция `IKey` экземпляров возвращается вызывающему объекту.</span><span class="sxs-lookup"><span data-stu-id="94baf-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="94baf-158">Дополнительные сведения об определенных элементах XML можно найти в [документе формат хранилища ключей](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="94baf-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="94baf-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="94baf-159">IXmlRepository</span></span>

<span data-ttu-id="94baf-160">Интерфейс `IXmlRepository` представляет тип, который может сохранять XML и получать XML из резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="94baf-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="94baf-161">Он предоставляет два API:</span><span class="sxs-lookup"><span data-stu-id="94baf-161">It exposes two APIs:</span></span>

* <span data-ttu-id="94baf-162">`GetAllElements`:`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="94baf-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="94baf-163">Реализациям `IXmlRepository` не нужно анализировать XML-код, передающий их.</span><span class="sxs-lookup"><span data-stu-id="94baf-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="94baf-164">Они должны обрабатывать XML-документов как непрозрачный и позволить более высоких уровнях беспокоиться о создании и анализе документов.</span><span class="sxs-lookup"><span data-stu-id="94baf-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="94baf-165">Существует четыре встроенных конкретных типа, которые реализуют `IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="94baf-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="94baf-166">филесистемксмлрепоситори</span><span class="sxs-lookup"><span data-stu-id="94baf-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="94baf-167">регистриксмлрепоситори</span><span class="sxs-lookup"><span data-stu-id="94baf-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="94baf-168">AzureStorage. Азуреблобксмлрепоситори</span><span class="sxs-lookup"><span data-stu-id="94baf-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="94baf-169">редисксмлрепоситори</span><span class="sxs-lookup"><span data-stu-id="94baf-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="94baf-170">филесистемксмлрепоситори</span><span class="sxs-lookup"><span data-stu-id="94baf-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="94baf-171">регистриксмлрепоситори</span><span class="sxs-lookup"><span data-stu-id="94baf-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="94baf-172">AzureStorage. Азуреблобксмлрепоситори</span><span class="sxs-lookup"><span data-stu-id="94baf-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="94baf-173">редисксмлрепоситори</span><span class="sxs-lookup"><span data-stu-id="94baf-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="94baf-174">Дополнительные сведения см. в [документе о поставщиках хранилища ключей](xref:security/data-protection/implementation/key-storage-providers) .</span><span class="sxs-lookup"><span data-stu-id="94baf-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="94baf-175">Регистрация настраиваемого `IXmlRepository` подходит при использовании другого резервного хранилища (например, табличного хранилища Azure).</span><span class="sxs-lookup"><span data-stu-id="94baf-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="94baf-176">Чтобы изменить приложение репозитория по умолчанию, зарегистрируйте пользовательский экземпляр `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="94baf-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

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

## <a name="ixmlencryptor"></a><span data-ttu-id="94baf-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="94baf-177">IXmlEncryptor</span></span>

<span data-ttu-id="94baf-178">Интерфейс `IXmlEncryptor` представляет тип, который может шифровать XML-элемент с открытым текстом.</span><span class="sxs-lookup"><span data-stu-id="94baf-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="94baf-179">Он предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="94baf-179">It exposes a single API:</span></span>

* <span data-ttu-id="94baf-180">Шифрование (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="94baf-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="94baf-181">Если сериализованный `IAuthenticatedEncryptorDescriptor` содержит элементы, помеченные как "требуется шифрование", `XmlKeyManager` запустит эти элементы через настроенный метод `Encrypt` `IXmlEncryptor`и сохранит элемент зашифрованные, а не элемент текста в `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="94baf-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="94baf-182">Выходным методом `Encrypt` является объект `EncryptedXmlInfo`.</span><span class="sxs-lookup"><span data-stu-id="94baf-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="94baf-183">Этот объект представляет собой оболочку, которая содержит результирующий зашифрованные `XElement` и тип, который представляет `IXmlDecryptor`, который можно использовать для расшифровки соответствующего элемента.</span><span class="sxs-lookup"><span data-stu-id="94baf-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="94baf-184">Существует четыре встроенных конкретных типа, которые реализуют `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="94baf-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="94baf-185">цертификатексмленкриптор</span><span class="sxs-lookup"><span data-stu-id="94baf-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="94baf-186">дпапингксмленкриптор</span><span class="sxs-lookup"><span data-stu-id="94baf-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="94baf-187">дпапиксмленкриптор</span><span class="sxs-lookup"><span data-stu-id="94baf-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="94baf-188">нуллксмленкриптор</span><span class="sxs-lookup"><span data-stu-id="94baf-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="94baf-189">Дополнительные сведения см. в статье о [шифровании ключа в неактивных документах](xref:security/data-protection/implementation/key-encryption-at-rest) .</span><span class="sxs-lookup"><span data-stu-id="94baf-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="94baf-190">Чтобы изменить значение по умолчанию для механизма шифрования по ключу "ключ-неактивных" на уровне приложения, зарегистрируйте пользовательский экземпляр `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="94baf-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

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

## <a name="ixmldecryptor"></a><span data-ttu-id="94baf-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="94baf-191">IXmlDecryptor</span></span>

<span data-ttu-id="94baf-192">Интерфейс `IXmlDecryptor` представляет тип, который знает, как расшифровать `XElement`, зашифрованные с помощью `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="94baf-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="94baf-193">Он предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="94baf-193">It exposes a single API:</span></span>

* <span data-ttu-id="94baf-194">Расшифровать (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="94baf-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="94baf-195">Метод `Decrypt` отменяет шифрование, выполняемое `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="94baf-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="94baf-196">Как правило, каждая конкретная реализация `IXmlEncryptor` будет иметь соответствующую конкретную реализацию `IXmlDecryptor`.</span><span class="sxs-lookup"><span data-stu-id="94baf-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="94baf-197">Типы, реализующие `IXmlDecryptor`, должны иметь один из следующих двух открытых конструкторов:</span><span class="sxs-lookup"><span data-stu-id="94baf-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="94baf-198">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="94baf-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="94baf-199">.ctor()</span><span class="sxs-lookup"><span data-stu-id="94baf-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="94baf-200">`IServiceProvider`, переданный конструктору, может иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="94baf-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="94baf-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="94baf-201">IKeyEscrowSink</span></span>

<span data-ttu-id="94baf-202">Интерфейс `IKeyEscrowSink` представляет тип, который может выполнять депонирование конфиденциальной информации.</span><span class="sxs-lookup"><span data-stu-id="94baf-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="94baf-203">Вспомним, что сериализованные дескрипторы могут содержать конфиденциальные сведения (такие как криптографические материалы), и это привело к вводу типа [иксмленкриптор](#ixmlencryptor) в первую очередь.</span><span class="sxs-lookup"><span data-stu-id="94baf-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="94baf-204">Тем не менее иногда происходят аварии и колец ключ можно удалить или поврежден.</span><span class="sxs-lookup"><span data-stu-id="94baf-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="94baf-205">Депонированный интерфейс предоставляет escape-последовательность аварийного переключения, предоставляя доступ к необработанному сериализованному XML, прежде чем он преобразуется любым настроенным [иксмленкриптор](#ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="94baf-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="94baf-206">Данный интерфейс предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="94baf-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="94baf-207">Store (идентификатор Guid ключа, элемент XElement)</span><span class="sxs-lookup"><span data-stu-id="94baf-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="94baf-208">Это `IKeyEscrowSink`ная реализация для обработки предоставленного элемента безопасным способом, согласованным с бизнес-политикой.</span><span class="sxs-lookup"><span data-stu-id="94baf-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="94baf-209">Одна из возможных реализаций может быть для того, чтобы депонированный приемник зашифрует XML-элемент с помощью известного корпоративного сертификата X. 509, в котором был указан закрытый ключ сертификата. Этот тип `CertificateXmlEncryptor` может помочь в этом.</span><span class="sxs-lookup"><span data-stu-id="94baf-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="94baf-210">Реализация `IKeyEscrowSink` также отвечает за сохранение указанного элемента соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="94baf-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="94baf-211">По умолчанию не включен механизм депонирования, хотя администраторы сервера могут [настроить глобально](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="94baf-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="94baf-212">Его также можно настроить программно с помощью метода `IDataProtectionBuilder.AddKeyEscrowSink`, как показано в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="94baf-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="94baf-213">`AddKeyEscrowSink` метод перегружает зеркальное отображение `IServiceCollection.AddSingleton` и `IServiceCollection.AddInstance` перегрузок, так как экземпляры `IKeyEscrowSink` должны быть одноэлементными.</span><span class="sxs-lookup"><span data-stu-id="94baf-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="94baf-214">Если зарегистрировано несколько `IKeyEscrowSink` экземпляров, каждый из них будет вызываться во время создания ключа, поэтому ключи могут быть переданы нескольким механизмам одновременно.</span><span class="sxs-lookup"><span data-stu-id="94baf-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="94baf-215">Нет API для чтения материала из экземпляра `IKeyEscrowSink`.</span><span class="sxs-lookup"><span data-stu-id="94baf-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="94baf-216">Это согласуется с теории проектирования механизма доверительного хранения: она предназначена предоставить доступ к доверенным центром сертификации материал ключа, и так, как приложение сам не является доверенным центром сертификации, он не должен иметь доступ к собственный депонированные материал.</span><span class="sxs-lookup"><span data-stu-id="94baf-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="94baf-217">В следующем примере кода демонстрируется создание и регистрация `IKeyEscrowSink`, в котором используются ключи, так что только члены группы "Администраторы Контосодомаин" могут их восстановить.</span><span class="sxs-lookup"><span data-stu-id="94baf-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="94baf-218">Чтобы запустить этот пример, необходимо быть на присоединенных к домену Windows 8 / компьютера Windows Server 2012 и контроллер домена должен быть Windows Server 2012 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="94baf-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
