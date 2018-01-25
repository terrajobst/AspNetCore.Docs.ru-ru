---
title: "Управление ключами расширяемости"
author: rick-anderson
description: "Этот документ посвящен расширения управления ключами для защиты данных ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: d748ee9d3edf9eed4285fab447d5b379dfcd937c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="key-management-extensibility"></a>Управление ключами расширяемости

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> Чтение [управление ключами](../implementation/key-management.md#data-protection-implementation-key-management) раздел перед считыванием в этом разделе, как он описаны некоторые основные принципы эти API-интерфейсы.

>[!WARNING]
> Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.

## <a name="key"></a>Ключ

`IKey` Интерфейса — это основные представление ключа в криптосистеме. Термин используется здесь в том смысле, абстрактным, не в литерал смысле «материалом ключа шифрования». Ключ имеет следующие свойства:

* Даты истечения срока действия, создания и активации

* Состояние отзыва

* Ключевой идентификатор (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Кроме того `IKey` предоставляет `CreateEncryptor` метод, который может использоваться для создания [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к данному ключу.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Кроме того `IKey` предоставляет `CreateEncryptorInstance` метод, который может использоваться для создания [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра привязан к данному ключу.

---

> [!NOTE]
> Нет API-интерфейс для извлечения необработанных криптографические материалы из `IKey` экземпляра.

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager` Интерфейс представляет собой объект, ответственный за общие хранилища ключей, получения и обработки. Он представляет три операции высокого уровня:

* Создание нового ключа и сохранения его в хранилище.

* Получите все ключи из хранилища.

* Отозвать один или несколько ключей и сохраняет сведения об отзыве в хранилище.

>[!WARNING]
> Написание `IKeyManager` является очень сложных задач, и большинство разработчиков не должен пытаться выполнить ее. Вместо этого, большинство разработчиков следует воспользоваться функциональными возможностями [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) класса.

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager` Тип — входящие в конкретную реализацию `IKeyManager`. Она предоставляет несколько полезных средств, включая переноса ключей и шифрования ключей при хранении. Ключи в этой системе представляются в виде XML-элементы (в частности, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager`зависит от других компонентов во время выполнения своих задач.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`, которые указывают, алгоритмы, используемые с новыми ключами.

* `IXmlRepository`, какие элементы управления, где ключи сохраняются в хранилище.

* `IXmlEncryptor`[необязательно], который позволяет шифровать ключи хранятся.

* `IKeyEscrowSink`[необязательно], который предоставляет перенос ключа службы.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `IXmlRepository`, какие элементы управления, где ключи сохраняются в хранилище.

* `IXmlEncryptor`[необязательно], который позволяет шифровать ключи хранятся.

* `IKeyEscrowSink`[необязательно], который предоставляет перенос ключа службы.

---

Ниже перечислены высокоуровневые диаграммы, которые указывают, как эти компоненты соединены друг с другом в пределах `XmlKeyManager`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Создание ключа](key-management/_static/keycreation2.png)

   *Создание ключа / CreateNewKey*

В реализации `CreateNewKey`, `AlgorithmConfiguration` компонент используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется в формат XML. Если присутствует приемник перенос ключа, необработанные XML-данные (незашифрованные) предоставляется в приемник для долговременного хранения. Незашифрованные XML этого выполняется `IXmlEncryptor` (при необходимости) для формирования зашифрованного XML-документа. Этот зашифрованный документ сохраняется в долговременном хранилище через `IXmlRepository`. (Если не `IXmlEncryptor` будет настроена, незашифрованные документ сохраняется в `IXmlRepository`.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Создание ключа](key-management/_static/keycreation1.png)

   *Создание ключа / CreateNewKey*

В реализации `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` компонент используется для создания уникального `IAuthenticatedEncryptorDescriptor`, который затем сериализуется в формат XML. Если присутствует приемник перенос ключа, необработанные XML-данные (незашифрованные) предоставляется в приемник для долговременного хранения. Незашифрованные XML этого выполняется `IXmlEncryptor` (при необходимости) для формирования зашифрованного XML-документа. Этот зашифрованный документ сохраняется в долговременном хранилище через `IXmlRepository`. (Если не `IXmlEncryptor` будет настроена, незашифрованные документ сохраняется в `IXmlRepository`.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Получение ключа](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Получение ключа](key-management/_static/keyretrieval1.png)

---

   *Получение ключа / GetAllKeys*

В реализации `GetAllKeys`, представляющих ключи документы XML и выдачу считываются из основного `IXmlRepository`. Если эти документы шифруются, система автоматически расшифровать их. `XmlKeyManager`создает соответствующий `IAuthenticatedEncryptorDescriptorDeserializer` экземпляров для десериализации документов обратно в `IAuthenticatedEncryptorDescriptor` экземпляров, которые затем помещается в отдельных `IKey` экземпляров. Эта коллекция `IKey` экземпляров возвращается вызывающему объекту.

Дополнительные сведения об отдельных элементов XML, которые можно найти в [хранилища ключей форматировать документ](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository` Интерфейс представляет тип, который может хранить XML-Документы и получить XML из резервного хранилища. Он предоставляет два API:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement (элемент XElement, friendlyName строка)

Реализации `IXmlRepository` не требуется синтаксический анализ XML, проходящие через них. Они должны обрабатывать XML-документов как непрозрачный и позволить волноваться по поводу создания и разбора документы более высокого уровня.

Существуют две встроенные конкретные типы, реализующие `IXmlRepository`: `FileSystemXmlRepository` и `RegistryXmlRepository`. В разделе [документа поставщиков хранилища ключей](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) для получения дополнительной информации. Регистрация пользовательского `IXmlRepository` будет подходящим способом резервных хранилищ, например, использование хранилища больших двоичных объектов.

Чтобы изменить репозиторий по умолчанию уровне приложения, зарегистрируйте пользовательский `IXmlRepository` экземпляр:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor` Интерфейс представляет тип, который можно зашифровать XML-элементе обычного текста. Он предоставляет единый интерфейс API:

* Шифрование (XElement plaintextElement): EncryptedXmlInfo

Если сериализованный `IAuthenticatedEncryptorDescriptor` содержит все элементы, помеченные как «требует шифрования», затем `XmlKeyManager` будет выполняться этих элементов посредством настроенного `IXmlEncryptor` `Encrypt` метод, который будет сохраняться enciphered элемент, а не обычный текст элемента `IXmlRepository`. Выходные данные `Encrypt` метод `EncryptedXmlInfo` объекта. Этот объект является программа-оболочка которого содержит оба итоговые enciphered `XElement` и тип, представляющий `IXmlDecryptor` которого можно расшифровать соответствующий элемент.

Существует четыре встроенных конкретные типы, реализующие `IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

В разделе [ключа шифрования в документе rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) для получения дополнительной информации.

Чтобы изменить ключ шифрования на rest механизма по умолчанию уровне приложения, зарегистрируйте пользовательский `IXmlEncryptor` экземпляр:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor` Интерфейс представляет тип, который знает, как расшифровать `XElement` , был enciphered через `IXmlEncryptor`. Он предоставляет единый интерфейс API:

* Расшифровать (XElement encryptedElement): XElement

`Decrypt` Метод отменяет шифрование, выполненных `IXmlEncryptor.Encrypt`. Как правило, каждый конкретный `IXmlEncryptor` реализация будет иметь соответствующий конкретный `IXmlDecryptor` реализации.

Типы, которые реализуют `IXmlDecryptor` должны иметь одно из следующих двух открытые конструкторы:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider` , Передаваемый конструктору может иметь значение null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink` Интерфейс представляет тип, который можно выполнить перенос конфиденциальных данных. Вспомним, что сериализованные дескрипторы могут содержать конфиденциальные сведения (например, криптографические материалы), это то, что привело к появлением [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) введите в первую очередь. Однако аварий и кольца ключ может быть удален или поврежден.

Интерфейс условно предоставляет штриховки аварийный escape, доступ к необработанные сериализованный XML перед его преобразованием каким-либо настроить [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). Интерфейс предоставляет единый интерфейс API.

* Хранилище (идентификатор Guid ключа, элемент XElement)

О `IKeyEscrowSink` реализации для обработки предоставленного элемента безопасным образом согласуется с бизнес-политика. Может быть одна возможная реализация для приемника депонированной сумки для шифрования XML-элемента с помощью известных корпоративный сертификат X.509, где депонирован закрытый ключ сертификата; `CertificateXmlEncryptor` типа может помочь с данным. `IKeyEscrowSink` Реализации также отвечает за сохранение указанного элемента соответствующим образом.

По умолчанию включен механизм переноса, хотя администраторы сервера могут [глобально эту настройку](xref:security/data-protection/configuration/machine-wide-policy). Его можно также настроить программно через `IDataProtectionBuilder.AddKeyEscrowSink` метода, как показано в примере ниже. `AddKeyEscrowSink` Зеркальный перегрузок метода `IServiceCollection.AddSingleton` и `IServiceCollection.AddInstance` перегрузок, как `IKeyEscrowSink` экземпляры должны быть единственные экземпляры. При наличии нескольких `IKeyEscrowSink` зарегистрированных экземпляров, каждый из них будет вызываться во время создания ключа, поэтому ключи могут быть перенесены на несколько механизмов одновременно.

Никакой интерфейс API для чтения материалы из `IKeyEscrowSink` экземпляра. Это согласуется с теории проектирования механизма переноса: он предназначен для материала ключа доступа к доверенным центром сертификации, и так как приложение сам не является доверенным центром сертификации, он не должен иметь доступ к собственный депонированные материал.

В следующем примере кода показано создание и регистрация `IKeyEscrowSink` где ключи перенесены таким образом, чтобы их можно восстановить только члены группы «Администраторы CONTOSODomain».

> [!NOTE]
> Чтобы запустить этот образец, должен быть на Windows 8, присоединенном к домену / компьютера Windows Server 2012 и контроллер домена должен быть Windows Server 2012 или более поздней версии.

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
