---
title: "Управление ключами расширяемости"
author: rick-anderson
description: "Этот документ посвящен расширения управления ключами для защиты данных ASP.NET Core."
keywords: "Управление ключами ASP.NET Core, защиты данных"
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 0702e13163c0208e9d2863e711b02ffb257f6260
ms.sourcegitcommit: e641c5794525f983485621860926d8ab4e7360c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="51e1f-104">Управление ключами расширяемости</span><span class="sxs-lookup"><span data-stu-id="51e1f-104">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="51e1f-105">Чтение [управление ключами](../implementation/key-management.md#data-protection-implementation-key-management) раздел перед считыванием в этом разделе, как он описаны некоторые основные принципы эти API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="51e1f-105">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="51e1f-106">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="51e1f-106">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="51e1f-107">Ключ</span><span class="sxs-lookup"><span data-stu-id="51e1f-107">Key</span></span>

<span data-ttu-id="51e1f-108">`IKey` Интерфейса — это основные представление ключа в криптосистеме.</span><span class="sxs-lookup"><span data-stu-id="51e1f-108">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="51e1f-109">Термин используется здесь в том смысле, абстрактным, не в литерал смысле «материалом ключа шифрования».</span><span class="sxs-lookup"><span data-stu-id="51e1f-109">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="51e1f-110">Ключ имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="51e1f-110">A key has the following properties:</span></span>

* <span data-ttu-id="51e1f-111">Даты истечения срока действия, создания и активации</span><span class="sxs-lookup"><span data-stu-id="51e1f-111">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="51e1f-112">Состояние отзыва</span><span class="sxs-lookup"><span data-stu-id="51e1f-112">Revocation status</span></span>

* <span data-ttu-id="51e1f-113">Ключевой идентификатор (GUID)</span><span class="sxs-lookup"><span data-stu-id="51e1f-113">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="51e1f-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="51e1f-115">Кроме того `IKey` предоставляет `CreateEncryptor` метод, который может использоваться для создания [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к данному ключу.</span><span class="sxs-lookup"><span data-stu-id="51e1f-115">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="51e1f-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="51e1f-117">Кроме того `IKey` предоставляет `CreateEncryptorInstance` метод, который может использоваться для создания [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к данному ключу.</span><span class="sxs-lookup"><span data-stu-id="51e1f-117">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="51e1f-118">Нет API-интерфейс для извлечения необработанных криптографические материалы из `IKey` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="51e1f-118">There is no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="51e1f-119">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="51e1f-119">IKeyManager</span></span>

<span data-ttu-id="51e1f-120">`IKeyManager` Интерфейс представляет собой объект, ответственный за общие хранилища ключей, получения и обработки.</span><span class="sxs-lookup"><span data-stu-id="51e1f-120">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="51e1f-121">Он представляет три операции высокого уровня:</span><span class="sxs-lookup"><span data-stu-id="51e1f-121">It exposes three high-level operations:</span></span>

* <span data-ttu-id="51e1f-122">Создание нового ключа и сохранения его в хранилище.</span><span class="sxs-lookup"><span data-stu-id="51e1f-122">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="51e1f-123">Получите все ключи из хранилища.</span><span class="sxs-lookup"><span data-stu-id="51e1f-123">Get all keys from storage.</span></span>

* <span data-ttu-id="51e1f-124">Отозвать один или несколько ключей и сохраняет сведения об отзыве в хранилище.</span><span class="sxs-lookup"><span data-stu-id="51e1f-124">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="51e1f-125">Написание `IKeyManager` является очень сложных задач, и большинство разработчиков не должны предпринимать попытки его.</span><span class="sxs-lookup"><span data-stu-id="51e1f-125">Writing an `IKeyManager` is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="51e1f-126">Вместо этого, большинство разработчиков следует воспользоваться функциональными возможностями [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) класса.</span><span class="sxs-lookup"><span data-stu-id="51e1f-126">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="51e1f-127">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="51e1f-127">XmlKeyManager</span></span>

<span data-ttu-id="51e1f-128">`XmlKeyManager` Тип — входящие в конкретную реализацию `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="51e1f-128">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="51e1f-129">Она предоставляет несколько полезных средств, включая переноса ключей и шифрования ключей при хранении.</span><span class="sxs-lookup"><span data-stu-id="51e1f-129">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="51e1f-130">Ключи в этой системе представляются в виде XML-элементы (в частности, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="51e1f-130">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="51e1f-131">`XmlKeyManager`зависит от других компонентов во время выполнения своих задач.</span><span class="sxs-lookup"><span data-stu-id="51e1f-131">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="51e1f-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="51e1f-133">`AlgorithmConfiguration`, которые указывают, алгоритмы, используемые с новыми ключами.</span><span class="sxs-lookup"><span data-stu-id="51e1f-133">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="51e1f-134">`IXmlRepository`, какие элементы управления, где ключи сохраняются в хранилище.</span><span class="sxs-lookup"><span data-stu-id="51e1f-134">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="51e1f-135">`IXmlEncryptor`[необязательно], который позволяет шифровать ключи хранятся.</span><span class="sxs-lookup"><span data-stu-id="51e1f-135">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="51e1f-136">`IKeyEscrowSink`[необязательно], который предоставляет перенос ключа службы.</span><span class="sxs-lookup"><span data-stu-id="51e1f-136">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="51e1f-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="51e1f-138">`IXmlRepository`, какие элементы управления, где ключи сохраняются в хранилище.</span><span class="sxs-lookup"><span data-stu-id="51e1f-138">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="51e1f-139">`IXmlEncryptor`[необязательно], который позволяет шифровать ключи хранятся.</span><span class="sxs-lookup"><span data-stu-id="51e1f-139">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="51e1f-140">`IKeyEscrowSink`[необязательно], который предоставляет перенос ключа службы.</span><span class="sxs-lookup"><span data-stu-id="51e1f-140">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="51e1f-141">Ниже перечислены высокоуровневые диаграммы, которые указывают, как эти компоненты соединены друг с другом в пределах `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="51e1f-141">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="51e1f-142">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-142">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Создание ключа](key-management/_static/keycreation2.png)

   <span data-ttu-id="51e1f-144">*Создание ключа / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="51e1f-144">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="51e1f-145">В реализации `CreateNewKey`, `AlgorithmConfiguration` компонент используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется в формат XML.</span><span class="sxs-lookup"><span data-stu-id="51e1f-145">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="51e1f-146">Если присутствует приемник перенос ключа, необработанные XML-данные (незашифрованные) предоставляется в приемник для долговременного хранения.</span><span class="sxs-lookup"><span data-stu-id="51e1f-146">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="51e1f-147">Незашифрованные XML этого выполняется `IXmlEncryptor` (при необходимости) для формирования зашифрованного XML-документа.</span><span class="sxs-lookup"><span data-stu-id="51e1f-147">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="51e1f-148">Этот зашифрованный документ сохраняется в долговременном хранилище через `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="51e1f-148">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="51e1f-149">(Если не `IXmlEncryptor` будет настроена, незашифрованные документ сохраняется в `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="51e1f-149">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="51e1f-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Создание ключа](key-management/_static/keycreation1.png)

   <span data-ttu-id="51e1f-152">*Создание ключа / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="51e1f-152">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="51e1f-153">В реализации `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` компонент используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется в формат XML.</span><span class="sxs-lookup"><span data-stu-id="51e1f-153">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="51e1f-154">Если присутствует приемник перенос ключа, необработанные XML-данные (незашифрованные) предоставляется в приемник для долговременного хранения.</span><span class="sxs-lookup"><span data-stu-id="51e1f-154">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="51e1f-155">Незашифрованные XML этого выполняется `IXmlEncryptor` (при необходимости) для формирования зашифрованного XML-документа.</span><span class="sxs-lookup"><span data-stu-id="51e1f-155">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="51e1f-156">Этот зашифрованный документ сохраняется в долговременном хранилище через `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="51e1f-156">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="51e1f-157">(Если не `IXmlEncryptor` будет настроена, незашифрованные документ сохраняется в `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="51e1f-157">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="51e1f-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Получение ключа](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="51e1f-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Получение ключа](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="51e1f-162">*Получение ключа / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="51e1f-162">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="51e1f-163">В реализации `GetAllKeys`, представляющих ключи документы XML и выдачу считываются из основного `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="51e1f-163">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="51e1f-164">Если эти документы шифруются, система автоматически расшифровать их.</span><span class="sxs-lookup"><span data-stu-id="51e1f-164">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="51e1f-165">`XmlKeyManager`создает соответствующий `IAuthenticatedEncryptorDescriptorDeserializer` экземпляров для десериализации документов обратно в `IAuthenticatedEncryptorDescriptor` экземпляров, которые затем помещается в отдельных `IKey` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="51e1f-165">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="51e1f-166">Эта коллекция `IKey` экземпляров возвращается вызывающему объекту.</span><span class="sxs-lookup"><span data-stu-id="51e1f-166">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="51e1f-167">Дополнительные сведения об отдельных элементов XML, которые можно найти в [хранилища ключей форматировать документ](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="51e1f-167">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="51e1f-168">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="51e1f-168">IXmlRepository</span></span>

<span data-ttu-id="51e1f-169">`IXmlRepository` Интерфейс представляет тип, который может хранить XML-Документы и получить XML из резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="51e1f-169">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="51e1f-170">Он предоставляет два API:</span><span class="sxs-lookup"><span data-stu-id="51e1f-170">It exposes two APIs:</span></span>

* <span data-ttu-id="51e1f-171">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="51e1f-171">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="51e1f-172">StoreElement (элемент XElement, friendlyName строка)</span><span class="sxs-lookup"><span data-stu-id="51e1f-172">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="51e1f-173">Реализации `IXmlRepository` не требуется синтаксический анализ XML, проходящие через них.</span><span class="sxs-lookup"><span data-stu-id="51e1f-173">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="51e1f-174">Они должны обрабатывать XML-документов как непрозрачный и позволить волноваться по поводу создания и разбора документы более высокого уровня.</span><span class="sxs-lookup"><span data-stu-id="51e1f-174">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="51e1f-175">Существуют две встроенные конкретные типы, реализующие `IXmlRepository`: `FileSystemXmlRepository` и `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="51e1f-175">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="51e1f-176">В разделе [документа поставщиков хранилища ключей](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="51e1f-176">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="51e1f-177">Регистрация пользовательского `IXmlRepository` будет подходящим способом резервных хранилищ, например, использование хранилища больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="51e1f-177">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="51e1f-178">Чтобы изменить репозиторий по умолчанию уровне приложения, зарегистрируйте пользовательский `IXmlRepository` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="51e1f-178">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="51e1f-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="51e1f-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="51e1f-181">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="51e1f-181">IXmlEncryptor</span></span>

<span data-ttu-id="51e1f-182">`IXmlEncryptor` Интерфейс представляет тип, который можно зашифровать XML-элементе обычного текста.</span><span class="sxs-lookup"><span data-stu-id="51e1f-182">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="51e1f-183">Он предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="51e1f-183">It exposes a single API:</span></span>

* <span data-ttu-id="51e1f-184">Шифрование (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="51e1f-184">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="51e1f-185">Если сериализованный `IAuthenticatedEncryptorDescriptor` содержит все элементы, помеченные как «требует шифрования», затем `XmlKeyManager` будет выполняться этих элементов посредством настроенного `IXmlEncryptor` `Encrypt` метод, который будет сохраняться enciphered элемент, а не обычный текст элемента `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="51e1f-185">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="51e1f-186">Выходные данные `Encrypt` метод `EncryptedXmlInfo` объекта.</span><span class="sxs-lookup"><span data-stu-id="51e1f-186">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="51e1f-187">Этот объект является программа-оболочка которого содержит оба итоговые enciphered `XElement` и тип, представляющий `IXmlDecryptor` которого можно расшифровать соответствующий элемент.</span><span class="sxs-lookup"><span data-stu-id="51e1f-187">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="51e1f-188">Существует четыре встроенных конкретные типы, реализующие `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="51e1f-188">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="51e1f-189">В разделе [ключа шифрования в документе rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="51e1f-189">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="51e1f-190">Чтобы изменить ключ шифрования на rest механизма по умолчанию уровне приложения, зарегистрируйте пользовательский `IXmlEncryptor` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="51e1f-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="51e1f-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="51e1f-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="51e1f-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="51e1f-193">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="51e1f-193">IXmlDecryptor</span></span>

<span data-ttu-id="51e1f-194">`IXmlDecryptor` Интерфейс представляет тип, который знает, как расшифровать `XElement` , был enciphered через `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="51e1f-194">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="51e1f-195">Он предоставляет единый интерфейс API:</span><span class="sxs-lookup"><span data-stu-id="51e1f-195">It exposes a single API:</span></span>

* <span data-ttu-id="51e1f-196">Расшифровать (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="51e1f-196">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="51e1f-197">`Decrypt` Метод отменяет шифрование, выполненных `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="51e1f-197">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="51e1f-198">Как правило, каждый конкретный `IXmlEncryptor` реализация будет иметь соответствующий конкретный `IXmlDecryptor` реализации.</span><span class="sxs-lookup"><span data-stu-id="51e1f-198">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="51e1f-199">Типы, которые реализуют `IXmlDecryptor` должны иметь одно из следующих двух открытые конструкторы:</span><span class="sxs-lookup"><span data-stu-id="51e1f-199">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="51e1f-200">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="51e1f-200">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="51e1f-201">.ctor()</span><span class="sxs-lookup"><span data-stu-id="51e1f-201">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="51e1f-202">`IServiceProvider` , Передаваемый конструктору может иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="51e1f-202">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="51e1f-203">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="51e1f-203">IKeyEscrowSink</span></span>

<span data-ttu-id="51e1f-204">`IKeyEscrowSink` Интерфейс представляет тип, который можно выполнить перенос конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="51e1f-204">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="51e1f-205">Вспомним, что сериализованные дескрипторы могут содержать конфиденциальные сведения (например, криптографические материалы), это то, что привело к появлением [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) введите в первую очередь.</span><span class="sxs-lookup"><span data-stu-id="51e1f-205">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="51e1f-206">Однако аварий и кольца ключ может быть удален или поврежден.</span><span class="sxs-lookup"><span data-stu-id="51e1f-206">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="51e1f-207">Интерфейс условно предоставляет штриховки аварийный escape, доступ к необработанные сериализованный XML перед его преобразованием каким-либо настроить [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="51e1f-207">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="51e1f-208">Интерфейс предоставляет единый интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="51e1f-208">The interface exposes a single API:</span></span>

* <span data-ttu-id="51e1f-209">Хранилище (идентификатор Guid ключа, элемент XElement)</span><span class="sxs-lookup"><span data-stu-id="51e1f-209">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="51e1f-210">О `IKeyEscrowSink` реализации для обработки предоставленного элемента безопасным образом согласуется с бизнес-политика.</span><span class="sxs-lookup"><span data-stu-id="51e1f-210">It is up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="51e1f-211">Может быть одна возможная реализация для приемника депонированной сумки для шифрования XML-элемента с помощью известных корпоративный сертификат X.509, где депонирован закрытый ключ сертификата; `CertificateXmlEncryptor` типа может помочь с данным.</span><span class="sxs-lookup"><span data-stu-id="51e1f-211">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="51e1f-212">`IKeyEscrowSink` Реализации также отвечает за сохранение указанного элемента соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="51e1f-212">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="51e1f-213">По умолчанию включен механизм переноса, хотя администраторы сервера могут [глобально эту настройку](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="51e1f-213">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="51e1f-214">Его можно также настроить программно через `IDataProtectionBuilder.AddKeyEscrowSink` метода, как показано в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="51e1f-214">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="51e1f-215">`AddKeyEscrowSink` Зеркальный перегрузок метода `IServiceCollection.AddSingleton` и `IServiceCollection.AddInstance` перегрузок, как `IKeyEscrowSink` экземпляры должны быть единственные экземпляры.</span><span class="sxs-lookup"><span data-stu-id="51e1f-215">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="51e1f-216">При наличии нескольких `IKeyEscrowSink` зарегистрированных экземпляров, каждый из них будет вызываться во время создания ключа, поэтому ключи могут быть перенесены на несколько механизмов одновременно.</span><span class="sxs-lookup"><span data-stu-id="51e1f-216">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="51e1f-217">Никакой интерфейс API для чтения материалы из `IKeyEscrowSink` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="51e1f-217">There is no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="51e1f-218">Это согласуется с теории проектирования механизма переноса: он предназначен для материала ключа доступа к доверенным центром сертификации, и так как приложение сам не является доверенным центром сертификации, он не должен иметь доступ к собственный депонированные материал.</span><span class="sxs-lookup"><span data-stu-id="51e1f-218">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="51e1f-219">В следующем примере кода показано создание и регистрация `IKeyEscrowSink` где ключи перенесены таким образом, чтобы их можно восстановить только члены группы «Администраторы CONTOSODomain».</span><span class="sxs-lookup"><span data-stu-id="51e1f-219">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="51e1f-220">Чтобы запустить этот образец, должен быть на Windows 8, присоединенном к домену / компьютера Windows Server 2012 и контроллер домена должен быть Windows Server 2012 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="51e1f-220">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
