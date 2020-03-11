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
# <a name="key-management-extensibility-in-aspnet-core"></a>Расширяемость управления ключами в ASP.NET Core

> [!TIP]
> Ознакомьтесь с разделом [Управление ключами](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) перед чтением этого раздела, так как в нем объясняются основные понятия, лежащие в основе этих API-интерфейсов.

> [!WARNING]
> Типы, реализующие любые из следующих интерфейсов должны быть потокобезопасными для нескольких клиентов.

## <a name="key"></a>Клавиши

Интерфейс `IKey` является базовым представлением ключа в криптосистемы. Термин используется здесь в том смысле, абстрактный, не в том смысле, литерал «ключевого материала шифрования». Ключ имеет следующие свойства:

* Даты активации, создания и истечения срока действия

* Состояние отзыва.

* Ключевой идентификатор (GUID)

::: moniker range=">= aspnetcore-2.0"

Кроме того, `IKey` предоставляет метод `CreateEncryptor`, который можно использовать для создания экземпляра [иаусентикатеденкриптор](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) , привязанного к этому ключу.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Кроме того, `IKey` предоставляет метод `CreateEncryptorInstance`, который можно использовать для создания экземпляра [иаусентикатеденкриптор](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) , привязанного к этому ключу.

::: moniker-end

> [!NOTE]
> Нет API для получения необработанного криптографического материала из экземпляра `IKey`.

## <a name="ikeymanager"></a>IKeyManager

Интерфейс `IKeyManager` представляет объект, отвечающий за общее хранение, извлечение и обработку ключей. Он предоставляет три высокоуровневых операций:

* Создайте новый ключ и сохранить их в хранилище.

* Получите все ключи из хранилища.

* Сохранять сведения отзыва в хранилище и отозвать один или несколько ключей.

>[!WARNING]
> Написание `IKeyManager` является очень сложной задачей, и большинство разработчиков не должны предпринимать попытки. Вместо этого большинству разработчиков следует воспользоваться возможностями, предлагаемыми классом [ксмлкэйманажер](#xmlkeymanager) .

## <a name="xmlkeymanager"></a>XmlKeyManager

Тип `XmlKeyManager` — это встроенная конкретная реализация `IKeyManager`. Он предоставляет несколько полезных набор возможностей, включая перенос ключа, а также шифрование неактивных ключей. Ключи в этой системе представлены в виде XML-элементов (в частности, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` зависит от нескольких других компонентов в процессе выполнения задач.

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, определяющий алгоритмы, используемые новыми ключами.

* `IXmlRepository`, определяющий, где хранятся ключи в хранилище.

* `IXmlEncryptor` [необязательный], что позволяет шифровать ключи неактивных ключей.

* `IKeyEscrowSink` [необязательно], который предоставляет службы депонирования ключей.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, определяющий, где хранятся ключи в хранилище.

* `IXmlEncryptor` [необязательный], что позволяет шифровать ключи неактивных ключей.

* `IKeyEscrowSink` [необязательно], который предоставляет службы депонирования ключей.

::: moniker-end

Ниже приведены высокоуровневые схемы, которые показывают, как эти компоненты взаимодействуют в `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Создание ключа](key-management/_static/keycreation2.png)

*Создание ключа/Креатеневкэй*

В реализации `CreateNewKey`компонент `AlgorithmConfiguration` используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется как XML. Если присутствует приемник доверительного хранения ключа, необработанный код XML (незашифрованного) предоставляется в приемник для долговременного хранения. После этого незашифрованный XML выполняется с помощью `IXmlEncryptor` (при необходимости) для создания зашифрованного XML-документа. Этот зашифрованный документ сохраняется в долгосрочном хранилище с помощью `IXmlRepository`. (Если `IXmlEncryptor` не настроена, незашифрованный документ сохраняется в `IXmlRepository`.)

![Получение ключа](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Создание ключа](key-management/_static/keycreation1.png)

*Создание ключа/Креатеневкэй*

В реализации `CreateNewKey`компонент `IAuthenticatedEncryptorConfiguration` используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется как XML. Если присутствует приемник доверительного хранения ключа, необработанный код XML (незашифрованного) предоставляется в приемник для долговременного хранения. После этого незашифрованный XML выполняется с помощью `IXmlEncryptor` (при необходимости) для создания зашифрованного XML-документа. Этот зашифрованный документ сохраняется в долгосрочном хранилище с помощью `IXmlRepository`. (Если `IXmlEncryptor` не настроена, незашифрованный документ сохраняется в `IXmlRepository`.)

![Получение ключа](key-management/_static/keyretrieval1.png)

::: moniker-end

*Получение ключа или Жеталлкэйс*

В реализации `GetAllKeys`XML-документы, представляющие ключи и отзыва, считываются из базовых `IXmlRepository`. Если эти документы шифруются, система автоматически расшифровывает их. `XmlKeyManager` создает соответствующие экземпляры `IAuthenticatedEncryptorDescriptorDeserializer` для десериализации документов в `IAuthenticatedEncryptorDescriptor` экземпляры, которые затем упаковываются в отдельные экземпляры `IKey`. Эта коллекция `IKey` экземпляров возвращается вызывающему объекту.

Дополнительные сведения об определенных элементах XML можно найти в [документе формат хранилища ключей](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

Интерфейс `IXmlRepository` представляет тип, который может сохранять XML и получать XML из резервного хранилища. Он предоставляет два API:

* `GetAllElements`:`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Реализациям `IXmlRepository` не нужно анализировать XML-код, передающий их. Они должны обрабатывать XML-документов как непрозрачный и позволить более высоких уровнях беспокоиться о создании и анализе документов.

Существует четыре встроенных конкретных типа, которые реализуют `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [филесистемксмлрепоситори](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [регистриксмлрепоситори](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. Азуреблобксмлрепоситори](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [редисксмлрепоситори](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [филесистемксмлрепоситори](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [регистриксмлрепоситори](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. Азуреблобксмлрепоситори](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [редисксмлрепоситори](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Дополнительные сведения см. в [документе о поставщиках хранилища ключей](xref:security/data-protection/implementation/key-storage-providers) .

Регистрация настраиваемого `IXmlRepository` подходит при использовании другого резервного хранилища (например, табличного хранилища Azure).

Чтобы изменить приложение репозитория по умолчанию, зарегистрируйте пользовательский экземпляр `IXmlRepository`.

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

Интерфейс `IXmlEncryptor` представляет тип, который может шифровать XML-элемент с открытым текстом. Он предоставляет единый интерфейс API:

* Шифрование (XElement plaintextElement): EncryptedXmlInfo

Если сериализованный `IAuthenticatedEncryptorDescriptor` содержит элементы, помеченные как "требуется шифрование", `XmlKeyManager` запустит эти элементы через настроенный метод `Encrypt` `IXmlEncryptor`и сохранит элемент зашифрованные, а не элемент текста в `IXmlRepository`. Выходным методом `Encrypt` является объект `EncryptedXmlInfo`. Этот объект представляет собой оболочку, которая содержит результирующий зашифрованные `XElement` и тип, который представляет `IXmlDecryptor`, который можно использовать для расшифровки соответствующего элемента.

Существует четыре встроенных конкретных типа, которые реализуют `IXmlEncryptor`:

* [цертификатексмленкриптор](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [дпапингксмленкриптор](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [дпапиксмленкриптор](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [нуллксмленкриптор](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Дополнительные сведения см. в статье о [шифровании ключа в неактивных документах](xref:security/data-protection/implementation/key-encryption-at-rest) .

Чтобы изменить значение по умолчанию для механизма шифрования по ключу "ключ-неактивных" на уровне приложения, зарегистрируйте пользовательский экземпляр `IXmlEncryptor`:

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

Интерфейс `IXmlDecryptor` представляет тип, который знает, как расшифровать `XElement`, зашифрованные с помощью `IXmlEncryptor`. Он предоставляет единый интерфейс API:

* Расшифровать (XElement encryptedElement): XElement

Метод `Decrypt` отменяет шифрование, выполняемое `IXmlEncryptor.Encrypt`. Как правило, каждая конкретная реализация `IXmlEncryptor` будет иметь соответствующую конкретную реализацию `IXmlDecryptor`.

Типы, реализующие `IXmlDecryptor`, должны иметь один из следующих двух открытых конструкторов:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider`, переданный конструктору, может иметь значение null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

Интерфейс `IKeyEscrowSink` представляет тип, который может выполнять депонирование конфиденциальной информации. Вспомним, что сериализованные дескрипторы могут содержать конфиденциальные сведения (такие как криптографические материалы), и это привело к вводу типа [иксмленкриптор](#ixmlencryptor) в первую очередь. Тем не менее иногда происходят аварии и колец ключ можно удалить или поврежден.

Депонированный интерфейс предоставляет escape-последовательность аварийного переключения, предоставляя доступ к необработанному сериализованному XML, прежде чем он преобразуется любым настроенным [иксмленкриптор](#ixmlencryptor). Данный интерфейс предоставляет единый интерфейс API:

* Store (идентификатор Guid ключа, элемент XElement)

Это `IKeyEscrowSink`ная реализация для обработки предоставленного элемента безопасным способом, согласованным с бизнес-политикой. Одна из возможных реализаций может быть для того, чтобы депонированный приемник зашифрует XML-элемент с помощью известного корпоративного сертификата X. 509, в котором был указан закрытый ключ сертификата. Этот тип `CertificateXmlEncryptor` может помочь в этом. Реализация `IKeyEscrowSink` также отвечает за сохранение указанного элемента соответствующим образом.

По умолчанию не включен механизм депонирования, хотя администраторы сервера могут [настроить глобально](xref:security/data-protection/configuration/machine-wide-policy). Его также можно настроить программно с помощью метода `IDataProtectionBuilder.AddKeyEscrowSink`, как показано в примере ниже. `AddKeyEscrowSink` метод перегружает зеркальное отображение `IServiceCollection.AddSingleton` и `IServiceCollection.AddInstance` перегрузок, так как экземпляры `IKeyEscrowSink` должны быть одноэлементными. Если зарегистрировано несколько `IKeyEscrowSink` экземпляров, каждый из них будет вызываться во время создания ключа, поэтому ключи могут быть переданы нескольким механизмам одновременно.

Нет API для чтения материала из экземпляра `IKeyEscrowSink`. Это согласуется с теории проектирования механизма доверительного хранения: она предназначена предоставить доступ к доверенным центром сертификации материал ключа, и так, как приложение сам не является доверенным центром сертификации, он не должен иметь доступ к собственный депонированные материал.

В следующем примере кода демонстрируется создание и регистрация `IKeyEscrowSink`, в котором используются ключи, так что только члены группы "Администраторы Контосодомаин" могут их восстановить.

> [!NOTE]
> Чтобы запустить этот пример, необходимо быть на присоединенных к домену Windows 8 / компьютера Windows Server 2012 и контроллер домена должен быть Windows Server 2012 или более поздней версии.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
