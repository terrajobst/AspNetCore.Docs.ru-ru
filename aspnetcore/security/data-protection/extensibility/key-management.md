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
ms.openlocfilehash: ed84b6dc257d5fd9e4c1cf6106df3c8bd6e14f64
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="key-management-extensibility"></a>Управление ключами расширяемости

<a name=data-protection-extensibility-key-management></a>

>[!TIP]
> Чтение [управление ключами](../implementation/key-management.md#data-protection-implementation-key-management) раздел перед считыванием в этом разделе, как он описаны некоторые основные принципы эти API-интерфейсы.

>[!WARNING]
> Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.

## <a name="key"></a>Ключ

Интерфейс IKey является представлением основные ключа в криптосистеме. Термин используется здесь в том смысле, абстрактным, не в литерал смысле «материалом ключа шифрования». Ключ имеет следующие свойства:

* Даты истечения срока действия, создания и активации

* Состояние отзыва

* Ключевой идентификатор (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Кроме того, IKey предоставляет метод CreateEncryptor, который может использоваться для создания [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к данному ключу.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Кроме того, IKey предоставляет метод CreateEncryptorInstance, который может использоваться для создания [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к данному ключу.

---

> [!NOTE]
> Нет никакой интерфейс API для извлечения необработанных криптографические материалы из экземпляра IKey.

## <a name="ikeymanager"></a>IKeyManager

Интерфейс IKeyManager представляет собой объект, ответственный за общие хранилища ключей, получения и обработки. Он представляет три операции высокого уровня:

* Создание нового ключа и сохранения его в хранилище.

* Получите все ключи из хранилища.

* Отозвать один или несколько ключей и сохраняет сведения об отзыве в хранилище.

>[!WARNING]
> Записи IKeyManager является очень сложных задач, а большинство разработчиков не должны предпринимать попытки его. Вместо этого, большинство разработчиков следует воспользоваться функциональными возможностями [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) класса.

<a name=data-protection-extensibility-key-management-xmlkeymanager></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

Тип XmlKeyManager — входящие в конкретную реализацию IKeyManager. Она предоставляет несколько полезных средств, включая переноса ключей и шифрования ключей при хранении. Ключи в этой системе представляются в виде XML-элементы (в частности, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).

XmlKeyManager зависит от других компонентов во время выполнения своих задач.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* AlgorithmConfiguration определяет алгоритмы, используемые с новыми ключами.

* IXmlRepository, какие элементы управления, где ключи сохраняются в хранилище.

* IXmlEncryptor [необязательно], который позволяет шифровать ключи неактивные.

* IKeyEscrowSink [необязательно], который предоставляет службы переноса ключей.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IXmlRepository, какие элементы управления, где ключи сохраняются в хранилище.

* IXmlEncryptor [необязательно], который позволяет шифровать ключи неактивные.

* IKeyEscrowSink [необязательно], который предоставляет службы переноса ключей.

---

Ниже перечислены высокоуровневые диаграммы, которые указывают, как эти компоненты соединены друг с другом в пределах XmlKeyManager.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Создание ключа](key-management/_static/keycreation2.png)

   *Создание ключа / CreateNewKey*

В реализации CreateNewKey AlgorithmConfiguration компонент используется для создания уникальных IAuthenticatedEncryptorDescriptor, который затем сериализуется в формат XML. Если присутствует приемник перенос ключа, необработанные XML-данные (незашифрованные) предоставляется в приемник для долговременного хранения. Незашифрованные XML затем выполняется через IXmlEncryptor (при необходимости) для формирования зашифрованного XML-документа. Этот зашифрованный документ сохраняется в долговременном хранилище через IXmlRepository. (Если не IXmlEncryptor не настроена, незашифрованные документа сохраняются в IXmlRepository.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Создание ключа](key-management/_static/keycreation1.png)

   *Создание ключа / CreateNewKey*

В реализации CreateNewKey IAuthenticatedEncryptorConfiguration компонент используется для создания уникальных IAuthenticatedEncryptorDescriptor, который затем сериализуется в формат XML. Если присутствует приемник перенос ключа, необработанные XML-данные (незашифрованные) предоставляется в приемник для долговременного хранения. Незашифрованные XML затем выполняется через IXmlEncryptor (при необходимости) для формирования зашифрованного XML-документа. Этот зашифрованный документ сохраняется в долговременном хранилище через IXmlRepository. (Если не IXmlEncryptor не настроена, незашифрованные документа сохраняются в IXmlRepository.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Получение ключа](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Получение ключа](key-management/_static/keyretrieval1.png)

---

   *Получение ключа / GetAllKeys*

В реализации GetAllKeys XML-документов, представляющий ключи и выдачу считываются из базового IXmlRepository. Если эти документы шифруются, система автоматически расшифровать их. XmlKeyManager создает соответствующие экземпляры IAuthenticatedEncryptorDescriptorDeserializer десериализовать обратно в экземпляры IAuthenticatedEncryptorDescriptor, которые затем помещаются в отдельные экземпляры IKey документов. Эта коллекция экземпляров IKey возвращается вызывающему объекту.

Дополнительные сведения об отдельных элементов XML, которые можно найти в [хранилища ключей форматировать документ](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

Интерфейс IXmlRepository представляет тип, который может хранить XML-Документы и получить XML из резервного хранилища. Он предоставляет два API:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement (элемент XElement, friendlyName строка)

Нет необходимости реализации IXmlRepository синтаксический анализ XML, проходящие через них. Они должны обрабатывать XML-документов как непрозрачный и позволить волноваться по поводу создания и разбора документы более высокого уровня.

Существуют две встроенные конкретные типы, реализующие IXmlRepository: FileSystemXmlRepository и RegistryXmlRepository. В разделе [документа поставщиков хранилища ключей](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) для получения дополнительной информации. Регистрация пользовательского IXmlRepository бы быть подходящим способом, чтобы использовать другой резервное хранилище, например, хранилище больших двоичных объектов Azure. Чтобы изменить приложение всего репозитория по умолчанию, зарегистрируйте пользовательские одноэлементный IXmlRepository в поставщик услуг.

<a name=data-protection-extensibility-key-management-ixmlencryptor></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

Интерфейс IXmlEncryptor представляет тип, который можно зашифровать XML-элементе обычного текста. Он предоставляет единый интерфейс API:

* Шифрование (XElement plaintextElement): EncryptedXmlInfo

Если сериализованный IAuthenticatedEncryptorDescriptor содержит все элементы, помеченные как «требует шифрования», XmlKeyManager запускается этих элементов с помощью метода шифрования настроенных IXmlEncryptor и вместо этого будет сохраняться enciphered элемент чем элемент IXmlRepository открытого текста. Выходные данные метода шифрования является объектом EncryptedXmlInfo. Этот объект — это оболочка, содержащий результирующие enciphered XElement и тип, представляющий IXmlDecryptor, который может использоваться для расшифровки соответствующий элемент.

Существует четыре встроенных конкретные типы, реализующие IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor и NullXmlEncryptor. В разделе [ключа шифрования в документе rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) для получения дополнительной информации. Чтобы изменить ключ шифрования на rest механизма по умолчанию уровне приложения, зарегистрируйте пользовательские одноэлементный IXmlEncryptor в поставщик услуг.

## <a name="ixmldecryptor"></a>IXmlDecryptor

Интерфейс IXmlDecryptor представляет тип, который знает, как для расшифровки элемента XElement, который был enciphered через IXmlEncryptor. Он предоставляет единый интерфейс API:

* Расшифровать (XElement encryptedElement): XElement

Метод расшифровки отменяет шифрование, выполненных IXmlEncryptor.Encrypt. Обычно каждая конкретная реализация IXmlEncryptor есть соответствующий конкретная реализация IXmlDecryptor.

Типы, реализующие IXmlDecryptor должны иметь одно из следующих двух открытые конструкторы:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> IServiceProvider, переданный в конструктор может иметь значение null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

Интерфейс IKeyEscrowSink представляет тип, который можно выполнить перенос конфиденциальных данных. Вспомним, что сериализованные дескрипторы могут содержать конфиденциальные сведения (например, криптографические материалы), это то, что привело к появлением [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) введите в первую очередь. Однако аварий и keyrings может быть удален или поврежден.

Интерфейс условно предоставляет штриховки аварийный escape, доступ к необработанные сериализованный XML перед его преобразованием каким-либо настроить [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). Интерфейс предоставляет единый интерфейс API.

* Хранилище (идентификатор Guid ключа, элемент XElement)

Это зависит от реализации IKeyEscrowSink для обработки предоставленного элемента безопасным образом согласуется с бизнес-политика. Может быть одна возможная реализация для приемника депонированной сумки для шифрования XML-элемента с помощью известных корпоративный сертификат X.509, где депонирован закрытый ключ сертификата; Тип CertificateXmlEncryptor может помочь с данным. Реализация IKeyEscrowSink также отвечает за сохранение указанного элемента соответствующим образом.

По умолчанию включен механизм переноса, хотя администраторы сервера могут [глобально эту настройку](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy). Его можно также настроить программно через *IDataProtectionBuilder.AddKeyEscrowSink* метода, как показано в примере ниже. *AddKeyEscrowSink* зеркальный перегрузок метода *IServiceCollection.AddSingleton* и *IServiceCollection.AddInstance* перегрузок, как IKeyEscrowSink экземпляры должны быть единственные экземпляры. Если зарегистрировано несколько экземпляров IKeyEscrowSink, каждый из них будет вызываться во время создания ключа, ключи могут быть перенесены на несколько механизмов одновременно.

Нет не API для чтения из экземпляра IKeyEscrowSink материала. Это согласуется с теории проектирования механизма переноса: он предназначен для материала ключа доступа к доверенным центром сертификации, и так как приложение сам не является доверенным центром сертификации, он не должен иметь доступ к собственный депонированные материал.

В следующем примере кода показано создание и регистрация IKeyEscrowSink, где ключи перенесены таким образом, чтобы их можно восстановить только члены группы «Администраторы CONTOSODomain».

> [!NOTE]
> Чтобы запустить этот образец, должен быть на Windows 8, присоединенном к домену / компьютера Windows Server 2012 и контроллер домена должен быть Windows Server 2012 или более поздней версии.

[!code-none[Main](key-management/samples/key-management-extensibility.cs)]
