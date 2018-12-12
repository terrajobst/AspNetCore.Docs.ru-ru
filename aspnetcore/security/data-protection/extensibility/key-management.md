---
title: Расширяемость управления ключами в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о расширяемости защиты данных в ASP.NET Core управление ключами.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284608"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Расширяемость управления ключами в ASP.NET Core

> [!TIP]
> Чтение [управление ключами](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) раздел перед прочтением данного раздела, в том случае, как он объясняет, что некоторые основные понятия за эти API-интерфейсы.

> [!WARNING]
> Типы, реализующие любые из следующих интерфейсов должны быть потокобезопасными для нескольких клиентов.

## <a name="key"></a>Ключ

`IKey` Интерфейс — это основные представление ключа в криптосистеме. Термин используется здесь в том смысле, абстрактный, не в том смысле, литерал «ключевого материала шифрования». Ключ имеет следующие свойства:

* Даты активации, создания и истечения срока действия

* Состояние отзыва

* Ключевой идентификатор (GUID)

::: moniker range=">= aspnetcore-2.0"

Кроме того `IKey` предоставляет `CreateEncryptor` метод, который может использоваться для создания [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к этому ключу.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Кроме того `IKey` предоставляет `CreateEncryptorInstance` метод, который может использоваться для создания [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к этому ключу.

::: moniker-end

> [!NOTE]
> Не существует API для извлечения необработанных криптографических из `IKey` экземпляра.

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager` Интерфейс представляет объект, ответственный за общие хранилища ключей, извлечения и обработки. Он предоставляет три высокоуровневых операций:

* Создайте новый ключ и сохранить их в хранилище.

* Получите все ключи из хранилища.

* Сохранять сведения отзыва в хранилище и отозвать один или несколько ключей.

>[!WARNING]
> Написание `IKeyManager` — очень сложная задача, и большинство разработчиков не следует пытаться выполнить ее. Вместо этого, большинство разработчиков следует воспользоваться функциональными возможностями [XmlKeyManager](#xmlkeymanager) класса.

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager` Измеряется в поле конкретная реализация `IKeyManager`. Он предоставляет несколько полезных набор возможностей, включая перенос ключа, а также шифрование неактивных ключей. Ключи в этой системе представлены в виде XML-элементы (в частности, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` зависит от нескольких компонентов во время выполнении своих задач.

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, который определяет алгоритмы, используемые новые ключи.

* `IXmlRepository`, какие элементы управления, где ключи сохраняются в хранилище.

* `IXmlEncryptor` [необязательно], который позволяет шифровать неактивных ключей.

* `IKeyEscrowSink` [необязательно], которая предоставляет службы доверительного хранения ключа.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, какие элементы управления, где ключи сохраняются в хранилище.

* `IXmlEncryptor` [необязательно], который позволяет шифровать неактивных ключей.

* `IKeyEscrowSink` [необязательно], которая предоставляет службы доверительного хранения ключа.

::: moniker-end

Ниже перечислены высокоуровневые диаграммы, которые указывают, как эти компоненты при объединении в `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Создание ключа](key-management/_static/keycreation2.png)

*Создание ключа / CreateNewKey*

В реализации `CreateNewKey`, `AlgorithmConfiguration` компонент используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется как XML. Если присутствует приемник доверительного хранения ключа, необработанный код XML (незашифрованного) предоставляется в приемник для долговременного хранения. Незашифрованные XML этого выполняется `IXmlEncryptor` (при необходимости) для создания зашифрованного XML-документа. Этот зашифрованный документ сохраняется в долговременном хранилище с помощью `IXmlRepository`. (Если не `IXmlEncryptor` — настройки незашифрованные документ сохраняется в `IXmlRepository`.)

![Получение ключа](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Создание ключа](key-management/_static/keycreation1.png)

*Создание ключа / CreateNewKey*

В реализации `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` компонент используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется как XML. Если присутствует приемник доверительного хранения ключа, необработанный код XML (незашифрованного) предоставляется в приемник для долговременного хранения. Незашифрованные XML этого выполняется `IXmlEncryptor` (при необходимости) для создания зашифрованного XML-документа. Этот зашифрованный документ сохраняется в долговременном хранилище с помощью `IXmlRepository`. (Если не `IXmlEncryptor` — настройки незашифрованные документ сохраняется в `IXmlRepository`.)

![Получение ключа](key-management/_static/keyretrieval1.png)

::: moniker-end

*Получение ключа / GetAllKeys*

В реализации `GetAllKeys`, представляющих ключи документов XML и выдачу считываются из основного `IXmlRepository`. Если эти документы шифруются, система автоматически расшифровывает их. `XmlKeyManager` создает соответствующий `IAuthenticatedEncryptorDescriptorDeserializer` экземпляры десериализовать документы обратно в `IAuthenticatedEncryptorDescriptor` экземпляров, которые затем упаковываются в отдельных `IKey` экземпляров. Эта коллекция `IKey` экземпляров возвращается вызывающему объекту.

Дополнительные сведения об отдельных элементов XML можно найти в [хранилища ключей форматировать документ](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository` Интерфейс представляет тип, который можно XML для сохранения и извлечения XML из резервного хранилища. Он предоставляет два API:

* `GetAllElements` :`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Реализации `IXmlRepository` не требуется синтаксический анализ XML, проходящие через них. Они должны обрабатывать XML-документов как непрозрачный и позволить более высоких уровнях беспокоиться о создании и анализе документов.

Существует четыре встроенных конкретные типы, реализующие `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

См. в разделе [документа поставщиков хранилища ключей](xref:security/data-protection/implementation/key-storage-providers) Дополнительные сведения.

Регистрация пользовательского `IXmlRepository` подходит при использовании резервных хранилищ (например, хранилище таблиц Azure).

Чтобы изменить репозитория по умолчанию уровня приложения, зарегистрируйте пользовательский `IXmlRepository` экземпляр:

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

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor` Интерфейс представляет тип, который можно зашифровать XML-элемент открытого текста. Он предоставляет единый интерфейс API:

* Encrypt(XElement plaintextElement): EncryptedXmlInfo

Если сериализованный `IAuthenticatedEncryptorDescriptor` содержит все элементы с пометкой «требует шифрования», затем `XmlKeyManager` запустит эти элементы настроенного `IXmlEncryptor`в `Encrypt` метод и он будет сохранять зашифрованные элемент, а не открытый текст элемента `IXmlRepository`. Выходные данные `Encrypt` метод `EncryptedXmlInfo` объекта. Этот объект является оболочкой, содержащий оба результирующих зашифрованные `XElement` и тип, представляющий `IXmlDecryptor` которого может использоваться для расшифровки соответствующий элемент.

Существует четыре встроенных конкретные типы, реализующие `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

См. в разделе [шифрование ключей при документа rest](xref:security/data-protection/implementation/key-encryption-at-rest) Дополнительные сведения.

Чтобы изменить ключ шифрования при хранении механизма по умолчанию уровня приложения, зарегистрируйте пользовательский `IXmlEncryptor` экземпляр:

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

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor` Интерфейс представляет тип, который знает, как для расшифровки `XElement` , был зашифрованные с помощью `IXmlEncryptor`. Он предоставляет единый интерфейс API:

* Расшифровка (XElement encryptedElement): XElement

`Decrypt` Метод отменяет шифрования, выполняемые `IXmlEncryptor.Encrypt`. Как правило, каждый конкретный `IXmlEncryptor` реализация будет иметь соответствующий устойчивый `IXmlDecryptor` реализации.

Типы, которые реализуют интерфейс `IXmlDecryptor` должны иметь одно из следующих двух открытые конструкторы:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider` Передается конструктору может иметь значение null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink` Интерфейс представляет тип, который можно выполнить перенос конфиденциальных данных. Отозвать сериализованный дескрипторы могут содержать конфиденциальные сведения (например, криптографическим материалом), что это, что привело к появлением [IXmlEncryptor](#ixmlencryptor) введите в первую очередь. Тем не менее иногда происходят аварии и колец ключ можно удалить или поврежден.

Интерфейс доверительного хранения обеспечивает аварийного escape-штрих, предоставляя доступ к необработанные сериализованный XML, перед его преобразованием каким-либо настроить [IXmlEncryptor](#ixmlencryptor). Данный интерфейс предоставляет единый интерфейс API:

* Store (идентификатор Guid ключа, элемент XElement)

О `IKeyEscrowSink` реализации для обработки указанного элемента в безопасной форме, совместимой с бизнес-политике. Может быть одна возможная реализация для приемника доверительного хранения для шифрования элемента XML при помощи известных корпоративный сертификат X.509, где депонирован закрытый ключ сертификата; `CertificateXmlEncryptor` типа может помочь с данным. `IKeyEscrowSink` Реализации также отвечает за сохранение указанного элемента соответствующим образом.

По умолчанию включен механизм доверительного хранения, то, что администраторы сервера могут [глобально эту настройку](xref:security/data-protection/configuration/machine-wide-policy). Его также можно настроить программно с помощью `IDataProtectionBuilder.AddKeyEscrowSink` метод, как показано в приведенном ниже примере. `AddKeyEscrowSink` Зеркальный перегрузки метода `IServiceCollection.AddSingleton` и `IServiceCollection.AddInstance` перегрузок, как `IKeyEscrowSink` экземпляры должны быть одноэлементных экземпляров. При наличии нескольких `IKeyEscrowSink` зарегистрированных экземпляров, каждый из них будет вызываться во время создания ключа, поэтому ключи может быть передан в несколько механизмов, одновременно.

Не существует API на чтение материала из `IKeyEscrowSink` экземпляра. Это согласуется с теории проектирования механизма доверительного хранения: она предназначена предоставить доступ к доверенным центром сертификации материал ключа, и так, как приложение сам не является доверенным центром сертификации, он не должен иметь доступ к собственный депонированные материал.

В следующем примере кода показано создание и регистрация `IKeyEscrowSink` где ключи являются передан, таким образом, что их можно восстановить только члены «CONTOSODomain Admins».

> [!NOTE]
> Чтобы запустить этот пример, необходимо быть на присоединенных к домену Windows 8 / компьютера Windows Server 2012 и контроллер домена должен быть Windows Server 2012 или более поздней версии.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
