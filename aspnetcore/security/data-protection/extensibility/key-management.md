---
title: "Управление ключами расширяемости"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: ce23931e72404347ebc17c69ae90e70cd15328bc
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="90a89-103">Управление ключами расширяемости</span><span class="sxs-lookup"><span data-stu-id="90a89-103">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="90a89-104">Чтение [управление ключами](../implementation/key-management.md#data-protection-implementation-key-management) раздел перед считыванием в этом разделе, как он описаны некоторые основные принципы эти API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="90a89-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="90a89-105">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="90a89-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="90a89-106">Ключ</span><span class="sxs-lookup"><span data-stu-id="90a89-106">Key</span></span>

<span data-ttu-id="90a89-107">Интерфейс IKey является представлением основные ключа в криптосистеме.</span><span class="sxs-lookup"><span data-stu-id="90a89-107">The IKey interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="90a89-108">Термин используется здесь в том смысле, абстрактным, не в литерал смысле «материалом ключа шифрования».</span><span class="sxs-lookup"><span data-stu-id="90a89-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="90a89-109">Ключ имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="90a89-109">A key has the following properties:</span></span>

* <span data-ttu-id="90a89-110">Даты истечения срока действия, создания и активации</span><span class="sxs-lookup"><span data-stu-id="90a89-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="90a89-111">Состояние отзыва</span><span class="sxs-lookup"><span data-stu-id="90a89-111">Revocation status</span></span>

* <span data-ttu-id="90a89-112">Ключевой идентификатор (GUID)</span><span class="sxs-lookup"><span data-stu-id="90a89-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90a89-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90a89-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="90a89-114">Кроме того, IKey предоставляет метод CreateEncryptor, который может использоваться для создания [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к данному ключу.</span><span class="sxs-lookup"><span data-stu-id="90a89-114">Additionally, IKey exposes a CreateEncryptor method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90a89-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90a89-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="90a89-116">Кроме того, IKey предоставляет метод CreateEncryptorInstance, который может использоваться для создания [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к данному ключу.</span><span class="sxs-lookup"><span data-stu-id="90a89-116">Additionally, IKey exposes a CreateEncryptorInstance method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="90a89-117">Нет никакой интерфейс API для извлечения необработанных криптографические материалы из экземпляра IKey.</span><span class="sxs-lookup"><span data-stu-id="90a89-117">There is no API to retrieve the raw cryptographic material from an IKey instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="90a89-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="90a89-118">IKeyManager</span></span>

<span data-ttu-id="90a89-119">Интерфейс IKeyManager представляет собой объект, ответственный за общие хранилища ключей, получения и обработки.</span><span class="sxs-lookup"><span data-stu-id="90a89-119">The IKeyManager interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="90a89-120">Он представляет три операции высокого уровня:</span><span class="sxs-lookup"><span data-stu-id="90a89-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="90a89-121">Создание нового ключа и сохранения его в хранилище.</span><span class="sxs-lookup"><span data-stu-id="90a89-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="90a89-122">Получите все ключи из хранилища.</span><span class="sxs-lookup"><span data-stu-id="90a89-122">Get all keys from storage.</span></span>

* <span data-ttu-id="90a89-123">Отозвать один или несколько ключей и сохраняет сведения об отзыве в хранилище.</span><span class="sxs-lookup"><span data-stu-id="90a89-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="90a89-124">Записи IKeyManager является очень сложных задач, а большинство разработчиков не должны предпринимать попытки его.</span><span class="sxs-lookup"><span data-stu-id="90a89-124">Writing an IKeyManager is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="90a89-125">Вместо этого, большинство разработчиков следует воспользоваться функциональными возможностями [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) класса.</span><span class="sxs-lookup"><span data-stu-id="90a89-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="90a89-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="90a89-126">XmlKeyManager</span></span>

<span data-ttu-id="90a89-127">Тип XmlKeyManager — входящие в конкретную реализацию IKeyManager.</span><span class="sxs-lookup"><span data-stu-id="90a89-127">The XmlKeyManager type is the in-box concrete implementation of IKeyManager.</span></span> <span data-ttu-id="90a89-128">Она предоставляет несколько полезных средств, включая переноса ключей и шифрования ключей при хранении.</span><span class="sxs-lookup"><span data-stu-id="90a89-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="90a89-129">Ключи в этой системе представляются в виде XML-элементы (в частности, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).</span><span class="sxs-lookup"><span data-stu-id="90a89-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).</span></span>

<span data-ttu-id="90a89-130">XmlKeyManager зависит от других компонентов во время выполнения своих задач.</span><span class="sxs-lookup"><span data-stu-id="90a89-130">XmlKeyManager depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90a89-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90a89-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="90a89-132">AlgorithmConfiguration определяет алгоритмы, используемые с новыми ключами.</span><span class="sxs-lookup"><span data-stu-id="90a89-132">AlgorithmConfiguration, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="90a89-133">IXmlRepository, какие элементы управления, где ключи сохраняются в хранилище.</span><span class="sxs-lookup"><span data-stu-id="90a89-133">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="90a89-134">IXmlEncryptor [необязательно], который позволяет шифровать ключи неактивные.</span><span class="sxs-lookup"><span data-stu-id="90a89-134">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="90a89-135">IKeyEscrowSink [необязательно], который предоставляет службы переноса ключей.</span><span class="sxs-lookup"><span data-stu-id="90a89-135">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90a89-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90a89-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="90a89-137">IXmlRepository, какие элементы управления, где ключи сохраняются в хранилище.</span><span class="sxs-lookup"><span data-stu-id="90a89-137">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="90a89-138">IXmlEncryptor [необязательно], который позволяет шифровать ключи неактивные.</span><span class="sxs-lookup"><span data-stu-id="90a89-138">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="90a89-139">IKeyEscrowSink [необязательно], который предоставляет службы переноса ключей.</span><span class="sxs-lookup"><span data-stu-id="90a89-139">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="90a89-140">Ниже перечислены высокоуровневые диаграммы, которые указывают, как эти компоненты соединены друг с другом в пределах XmlKeyManager.</span><span class="sxs-lookup"><span data-stu-id="90a89-140">Below are high-level diagrams which indicate how these components are wired together within XmlKeyManager.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90a89-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90a89-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Создание ключа](key-management/_static/keycreation2.png)

   <span data-ttu-id="90a89-143">*Создание ключа / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="90a89-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="90a89-144">В реализации CreateNewKey AlgorithmConfiguration компонент используется для создания уникальных IAuthenticatedEncryptorDescriptor, который затем сериализуется в формат XML.</span><span class="sxs-lookup"><span data-stu-id="90a89-144">In the implementation of CreateNewKey, the AlgorithmConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="90a89-145">Если присутствует приемник перенос ключа, необработанные XML-данные (незашифрованные) предоставляется в приемник для долговременного хранения.</span><span class="sxs-lookup"><span data-stu-id="90a89-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="90a89-146">Незашифрованные XML затем выполняется через IXmlEncryptor (при необходимости) для формирования зашифрованного XML-документа.</span><span class="sxs-lookup"><span data-stu-id="90a89-146">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="90a89-147">Этот зашифрованный документ сохраняется в долговременном хранилище через IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="90a89-147">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="90a89-148">(Если не IXmlEncryptor не настроена, незашифрованные документа сохраняются в IXmlRepository.)</span><span class="sxs-lookup"><span data-stu-id="90a89-148">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90a89-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90a89-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Создание ключа](key-management/_static/keycreation1.png)

   <span data-ttu-id="90a89-151">*Создание ключа / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="90a89-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="90a89-152">В реализации CreateNewKey IAuthenticatedEncryptorConfiguration компонент используется для создания уникальных IAuthenticatedEncryptorDescriptor, который затем сериализуется в формат XML.</span><span class="sxs-lookup"><span data-stu-id="90a89-152">In the implementation of CreateNewKey, the IAuthenticatedEncryptorConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="90a89-153">Если присутствует приемник перенос ключа, необработанные XML-данные (незашифрованные) предоставляется в приемник для долговременного хранения.</span><span class="sxs-lookup"><span data-stu-id="90a89-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="90a89-154">Незашифрованные XML затем выполняется через IXmlEncryptor (при необходимости) для формирования зашифрованного XML-документа.</span><span class="sxs-lookup"><span data-stu-id="90a89-154">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="90a89-155">Этот зашифрованный документ сохраняется в долговременном хранилище через IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="90a89-155">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="90a89-156">(Если не IXmlEncryptor не настроена, незашифрованные документа сохраняются в IXmlRepository.)</span><span class="sxs-lookup"><span data-stu-id="90a89-156">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90a89-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90a89-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Получение ключа](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90a89-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90a89-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Получение ключа](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="90a89-161">*Получение ключа / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="90a89-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="90a89-162">В реализации GetAllKeys XML-документов, представляющий ключи и выдачу считываются из базового IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="90a89-162">In the implementation of GetAllKeys, the XML documents representing keys and revocations are read from the underlying IXmlRepository.</span></span> <span data-ttu-id="90a89-163">Если эти документы шифруются, система автоматически расшифровать их.</span><span class="sxs-lookup"><span data-stu-id="90a89-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="90a89-164">XmlKeyManager создает соответствующие экземпляры IAuthenticatedEncryptorDescriptorDeserializer десериализовать обратно в экземпляры IAuthenticatedEncryptorDescriptor, которые затем помещаются в отдельные экземпляры IKey документов.</span><span class="sxs-lookup"><span data-stu-id="90a89-164">XmlKeyManager creates the appropriate IAuthenticatedEncryptorDescriptorDeserializer instances to deserialize the documents back into IAuthenticatedEncryptorDescriptor instances, which are then wrapped in individual IKey instances.</span></span> <span data-ttu-id="90a89-165">Эта коллекция экземпляров IKey возвращается вызывающему объекту.</span><span class="sxs-lookup"><span data-stu-id="90a89-165">This collection of IKey instances is returned to the caller.</span></span>

<span data-ttu-id="90a89-166">Дополнительные сведения об отдельных элементов XML, которые можно найти в [хранилища ключей форматировать документ](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="90a89-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="90a89-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="90a89-167">IXmlRepository</span></span>

<span data-ttu-id="90a89-168">Интерфейс IXmlRepository представляет тип, который может хранить XML-Документы и получить XML из резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="90a89-168">The IXmlRepository interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="90a89-169">Он предоставляет два API:</span><span class="sxs-lookup"><span data-stu-id="90a89-169">It exposes two APIs:</span></span>

* <span data-ttu-id="90a89-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="90a89-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="90a89-171">StoreElement (элемент XElement, friendlyName строка)</span><span class="sxs-lookup"><span data-stu-id="90a89-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="90a89-172">Нет необходимости реализации IXmlRepository синтаксический анализ XML, проходящие через них.</span><span class="sxs-lookup"><span data-stu-id="90a89-172">Implementations of IXmlRepository don't need to parse the XML passing through them.</span></span> <span data-ttu-id="90a89-173">Они должны обрабатывать XML-документов как непрозрачный и позволить волноваться по поводу создания и разбора документы более высокого уровня.</span><span class="sxs-lookup"><span data-stu-id="90a89-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="90a89-174">Существуют две встроенные конкретные типы, реализующие IXmlRepository: FileSystemXmlRepository и RegistryXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="90a89-174">There are two built-in concrete types which implement IXmlRepository: FileSystemXmlRepository and RegistryXmlRepository.</span></span> <span data-ttu-id="90a89-175">В разделе [документа поставщиков хранилища ключей](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="90a89-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="90a89-176">Регистрация пользовательского IXmlRepository бы быть подходящим способом, чтобы использовать другой резервное хранилище, например, хранилище больших двоичных объектов Azure.</span><span class="sxs-lookup"><span data-stu-id="90a89-176">Registering a custom IXmlRepository would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span> <span data-ttu-id="90a89-177">Чтобы изменить приложение всего репозитория по умолчанию, зарегистрируйте пользовательские одноэлементный IXmlRepository в поставщик услуг.</span><span class="sxs-lookup"><span data-stu-id="90a89-177">To change the default repository application-wide, register a custom singleton IXmlRepository in the service provider.</span></span>

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="90a89-178">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="90a89-178">IXmlEncryptor</span></span>

<span data-ttu-id="90a89-179">Интерфейс IXmlEncryptor представляет тип, который можно зашифровать XML-элементе обычного текста.</span><span class="sxs-lookup"><span data-stu-id="90a89-179">The IXmlEncryptor interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="90a89-180">Он предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="90a89-180">It exposes a single API:</span></span>

* <span data-ttu-id="90a89-181">Шифрование (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="90a89-181">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="90a89-182">Если сериализованный IAuthenticatedEncryptorDescriptor содержит все элементы, помеченные как «требует шифрования», XmlKeyManager запускается этих элементов с помощью метода шифрования настроенных IXmlEncryptor и вместо этого будет сохраняться enciphered элемент чем элемент IXmlRepository открытого текста.</span><span class="sxs-lookup"><span data-stu-id="90a89-182">If a serialized IAuthenticatedEncryptorDescriptor contains any elements marked as "requires encryption", then XmlKeyManager will run those elements through the configured IXmlEncryptor's Encrypt method, and it will persist the enciphered element rather than the plaintext element to the IXmlRepository.</span></span> <span data-ttu-id="90a89-183">Выходные данные метода шифрования является объектом EncryptedXmlInfo.</span><span class="sxs-lookup"><span data-stu-id="90a89-183">The output of the Encrypt method is an EncryptedXmlInfo object.</span></span> <span data-ttu-id="90a89-184">Этот объект — это оболочка, содержащий результирующие enciphered XElement и тип, представляющий IXmlDecryptor, который может использоваться для расшифровки соответствующий элемент.</span><span class="sxs-lookup"><span data-stu-id="90a89-184">This object is a wrapper which contains both the resultant enciphered XElement and the Type which represents an IXmlDecryptor which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="90a89-185">Существует четыре встроенных конкретные типы, реализующие IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor и NullXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="90a89-185">There are four built-in concrete types which implement IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor, and NullXmlEncryptor.</span></span> <span data-ttu-id="90a89-186">В разделе [ключа шифрования в документе rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="90a89-186">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span> <span data-ttu-id="90a89-187">Чтобы изменить ключ шифрования на rest механизма по умолчанию уровне приложения, зарегистрируйте пользовательские одноэлементный IXmlEncryptor в поставщик услуг.</span><span class="sxs-lookup"><span data-stu-id="90a89-187">To change the default key-encryption-at-rest mechanism application-wide, register a custom singleton IXmlEncryptor in the service provider.</span></span>

## <a name="ixmldecryptor"></a><span data-ttu-id="90a89-188">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="90a89-188">IXmlDecryptor</span></span>

<span data-ttu-id="90a89-189">Интерфейс IXmlDecryptor представляет тип, который знает, как для расшифровки элемента XElement, который был enciphered через IXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="90a89-189">The IXmlDecryptor interface represents a type that knows how to decrypt an XElement that was enciphered via an IXmlEncryptor.</span></span> <span data-ttu-id="90a89-190">Он предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="90a89-190">It exposes a single API:</span></span>

* <span data-ttu-id="90a89-191">Расшифровать (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="90a89-191">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="90a89-192">Метод расшифровки отменяет шифрование, выполненных IXmlEncryptor.Encrypt.</span><span class="sxs-lookup"><span data-stu-id="90a89-192">The Decrypt method undoes the encryption performed by IXmlEncryptor.Encrypt.</span></span> <span data-ttu-id="90a89-193">Обычно каждая конкретная реализация IXmlEncryptor есть соответствующий конкретная реализация IXmlDecryptor.</span><span class="sxs-lookup"><span data-stu-id="90a89-193">Generally each concrete IXmlEncryptor implementation will have a corresponding concrete IXmlDecryptor implementation.</span></span>

<span data-ttu-id="90a89-194">Типы, реализующие IXmlDecryptor должны иметь одно из следующих двух открытые конструкторы:</span><span class="sxs-lookup"><span data-stu-id="90a89-194">Types which implement IXmlDecryptor should have one of the following two public constructors:</span></span>

* <span data-ttu-id="90a89-195">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="90a89-195">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="90a89-196">.ctor()</span><span class="sxs-lookup"><span data-stu-id="90a89-196">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="90a89-197">IServiceProvider, переданный в конструктор может иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="90a89-197">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="90a89-198">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="90a89-198">IKeyEscrowSink</span></span>

<span data-ttu-id="90a89-199">Интерфейс IKeyEscrowSink представляет тип, который можно выполнить перенос конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="90a89-199">The IKeyEscrowSink interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="90a89-200">Вспомним, что сериализованные дескрипторы могут содержать конфиденциальные сведения (например, криптографические материалы), это то, что привело к появлением [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) введите в первую очередь.</span><span class="sxs-lookup"><span data-stu-id="90a89-200">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="90a89-201">Однако аварий и keyrings может быть удален или поврежден.</span><span class="sxs-lookup"><span data-stu-id="90a89-201">However, accidents happen, and keyrings can be deleted or become corrupted.</span></span>

<span data-ttu-id="90a89-202">Интерфейс условно предоставляет штриховки аварийный escape, доступ к необработанные сериализованный XML перед его преобразованием каким-либо настроить [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="90a89-202">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="90a89-203">Интерфейс предоставляет единый интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="90a89-203">The interface exposes a single API:</span></span>

* <span data-ttu-id="90a89-204">Хранилище (идентификатор Guid ключа, элемент XElement)</span><span class="sxs-lookup"><span data-stu-id="90a89-204">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="90a89-205">Это зависит от реализации IKeyEscrowSink для обработки предоставленного элемента безопасным образом согласуется с бизнес-политика.</span><span class="sxs-lookup"><span data-stu-id="90a89-205">It is up to the IKeyEscrowSink implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="90a89-206">Может быть одна возможная реализация для приемника депонированной сумки для шифрования XML-элемента с помощью известных корпоративный сертификат X.509, где депонирован закрытый ключ сертификата; Тип CertificateXmlEncryptor может помочь с данным.</span><span class="sxs-lookup"><span data-stu-id="90a89-206">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the CertificateXmlEncryptor type can assist with this.</span></span> <span data-ttu-id="90a89-207">Реализация IKeyEscrowSink также отвечает за сохранение указанного элемента соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="90a89-207">The IKeyEscrowSink implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="90a89-208">По умолчанию включен механизм переноса, хотя администраторы сервера могут [глобально эту настройку](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span><span class="sxs-lookup"><span data-stu-id="90a89-208">By default no escrow mechanism is enabled, though server administrators can [configure this globally](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span></span> <span data-ttu-id="90a89-209">Его можно также настроить программно через *IDataProtectionBuilder.AddKeyEscrowSink* метода, как показано в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="90a89-209">It can also be configured programmatically via the *IDataProtectionBuilder.AddKeyEscrowSink* method as shown in the sample below.</span></span> <span data-ttu-id="90a89-210">*AddKeyEscrowSink* зеркальный перегрузок метода *IServiceCollection.AddSingleton* и *IServiceCollection.AddInstance* перегрузок, как IKeyEscrowSink экземпляры должны быть единственные экземпляры.</span><span class="sxs-lookup"><span data-stu-id="90a89-210">The *AddKeyEscrowSink* method overloads mirror the *IServiceCollection.AddSingleton* and *IServiceCollection.AddInstance* overloads, as IKeyEscrowSink instances are intended to be singletons.</span></span> <span data-ttu-id="90a89-211">Если зарегистрировано несколько экземпляров IKeyEscrowSink, каждый из них будет вызываться во время создания ключа, ключи могут быть перенесены на несколько механизмов одновременно.</span><span class="sxs-lookup"><span data-stu-id="90a89-211">If multiple IKeyEscrowSink instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="90a89-212">Нет не API для чтения из экземпляра IKeyEscrowSink материала.</span><span class="sxs-lookup"><span data-stu-id="90a89-212">There is no API to read material from an IKeyEscrowSink instance.</span></span> <span data-ttu-id="90a89-213">Это согласуется с теории проектирования механизма переноса: он предназначен для материала ключа доступа к доверенным центром сертификации, и так как приложение сам не является доверенным центром сертификации, он не должен иметь доступ к собственный депонированные материал.</span><span class="sxs-lookup"><span data-stu-id="90a89-213">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="90a89-214">В следующем примере кода показано создание и регистрация IKeyEscrowSink, где ключи перенесены таким образом, чтобы их можно восстановить только члены группы «Администраторы CONTOSODomain».</span><span class="sxs-lookup"><span data-stu-id="90a89-214">The following sample code demonstrates creating and registering an IKeyEscrowSink where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="90a89-215">Чтобы запустить этот образец, должен быть на Windows 8, присоединенном к домену / компьютера Windows Server 2012 и контроллер домена должен быть Windows Server 2012 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="90a89-215">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-none[Main](key-management/samples/key-management-extensibility.cs)]
