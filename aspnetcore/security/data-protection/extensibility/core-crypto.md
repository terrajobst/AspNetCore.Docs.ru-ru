---
title: "Основные расширяемости шифрования"
author: rick-anderson
description: "Описание IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer и фабрику верхнего уровня."
ms.author: riande
manager: wpickett
ms.date: 8/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b82c30fe40c4badc74645dafa9f0d13f6ffae031
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="core-cryptography-extensibility"></a>Основные расширяемости шифрования

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor** интерфейс является основной строительный блок подсистема шифрования. Обычно есть один IAuthenticatedEncryptor каждого ключа, и экземпляр IAuthenticatedEncryptor переносит все криптографические материалы ключа и алгоритма сведения, необходимые для выполнения криптографических операций.

Как и предполагает его имя, тип отвечает за предоставление прошедшего проверку подлинности служб шифрования и расшифровки. Он предоставляет следующие два API.

* Расшифровать (ArraySegment<byte> зашифрованного текста ArraySegment<byte> additionalAuthenticatedData): byte]

* Шифрование (ArraySegment<byte> открытого текста, ArraySegment<byte> additionalAuthenticatedData): byte]

Метод Encrypt возвращает большой двоичный объект, содержащий enciphered открытого текста и тег проверки подлинности. Тег аутентификации должен охватывать дополнительных данных прошедшего проверку подлинности (AAD), хотя сам AAD не обязательно восстанавливаемых из окончательного полезных данных. Метод расшифровки проверяет тег проверки подлинности и возвращает deciphered полезных данных. Все ошибки (за исключением ArgumentNullException и аналогичных) следует homogenized для CryptographicException.

> [!NOTE]
> Сам экземпляр IAuthenticatedEncryptor не обязательно фактически содержит материал ключа. Например реализация может делегировать аппаратный модуль безопасности для всех операций.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Создание IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра. API-интерфейса выглядит следующим образом.

* CreateEncryptorInstance (IKey ключ): IAuthenticatedEncryptor

Для любого заданного экземпляра IKey любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance должны рассматриваться как эквивалент, как в приведенном ниже примере кода.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра. API-интерфейса выглядит следующим образом.

* CreateEncryptorInstance(): IAuthenticatedEncryptor

* ExportToXml(): XmlSerializedDescriptorInfo

Как IAuthenticatedEncryptor экземпляр IAuthenticatedEncryptorDescriptor предполагается программы-оболочки для одного определенного ключа. Это означает, что для любого заданного экземпляра IAuthenticatedEncryptorDescriptor любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance следует учитывать эквивалент, как в приведенном ниже примере кода.

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x только)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как экспортировать сама себя XML. API-интерфейса выглядит следующим образом.

* ExportToXml(): XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>XML-сериализация

Основное различие между IAuthenticatedEncryptor и IAuthenticatedEncryptorDescriptor является то, что дескриптор знает, как создать шифратор и передайте в него допустимые аргументы. Рассмотрите возможность IAuthenticatedEncryptor, реализация которого зависит от SymmetricAlgorithm и KeyedHashAlgorithm. Задание шифратор — использовать эти типы, но он не обязательно знать эти типы происхождения, поэтому его нельзя записывать действительно правильную описание как воссоздать сам при повторном запуске приложения. Дескриптор действует как более высокого уровня на основе этого. Так как дескриптор знает, как создать экземпляр шифратор (например, он знает, как создать необходимые алгоритмы), он может сериализовать эти знания в формате XML, чтобы экземпляр шифратора можно воссоздать после сброса приложения.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

Дескриптор быть сериализованы посредством его ExportToXml подпрограммы. Эта процедура возвращает XmlSerializedDescriptorInfo, который содержит два свойства: это представление XElement дескриптора и тип, представляющий [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) которого может быть используется, чтобы восстановить этот дескриптор получает соответствующий XElement.

Сериализованный дескриптора может содержать конфиденциальные сведения, например материалом ключа шифрования. Система защиты данных имеет встроенную поддержку для шифрования данных, прежде чем он сохранен в хранилище. Для использования этой возможности, дескриптор следует пометить элемент, который содержит конфиденциальную информацию с имени атрибута «requiresEncryption» (xmlns «http://schemas.asp.net/2015/03/dataProtection»), значение «true».

>[!TIP]
> Нет вспомогательного API для установки этого атрибута. Вызов метода расширения, XElement.MarkAsRequiresEncryption(), расположенный в пространстве имен Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.

Также возможны случаи, где сериализованный дескриптора не содержит конфиденциальные сведения. Снова рассмотрим случай криптографический ключ, хранящийся в HSM. Дескриптор не может записать материал ключа при сериализации сам, так как аппаратный модуль безопасности не будут предоставлены материалы в текстовом виде. Вместо этого дескриптора может запишите ключ в оболочку версию ключа (если аппаратный модуль безопасности позволяет выполнять экспорт таким образом) или аппаратный модуль безопасности собственный уникальный идентификатор для ключа.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer** интерфейс представляет тип, который знает, как выполнить десериализацию IAuthenticatedEncryptorDescriptor экземпляра из элемента XElement. Он предоставляет один метод:

* ImportFromXml (элемент XElement): IAuthenticatedEncryptorDescriptor

Метод ImportFromXml принимает XElement, которое было возвращено [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) и создает эквивалентный исходного IAuthenticatedEncryptorDescriptor.

Типы, реализующие IAuthenticatedEncryptorDescriptorDeserializer должны иметь одно из следующих двух открытые конструкторы:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> IServiceProvider, переданный в конструктор может иметь значение null.

## <a name="the-top-level-factory"></a>Фабрика верхнего уровня

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration** класс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров. Он предоставляет единый интерфейс API.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

Считаете AlgorithmConfiguration фабрику верхнего уровня. Конфигурация служит в качестве шаблона. Заключает в оболочку алгоритмической сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но она еще не связана с указанным ключом.

При вызове CreateNewDescriptor, исключительно для этого вызова метода создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor которого переносится этот материал ключа и алгоритма сведения, необходимые для использования материала. Материал ключа может создания в программном обеспечении (и в памяти), может создать и находятся в аппаратный модуль безопасности и т. д. Важно точка находится любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.

Тип AlgorithmConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического смену ключей](../implementation/key-management.md#key-expiration-and-rolling). Чтобы изменить реализацию для всех будущих ключей, установите свойство AuthenticatedEncryptorConfiguration в KeyManagementOptions.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров. Он предоставляет единый интерфейс API.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

Считаете IAuthenticatedEncryptorConfiguration фабрику верхнего уровня. Конфигурация служит в качестве шаблона. Заключает в оболочку алгоритмической сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но она еще не связана с указанным ключом.

При вызове CreateNewDescriptor, исключительно для этого вызова метода создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor которого переносится этот материал ключа и алгоритма сведения, необходимые для использования материала. Материал ключа может создания в программном обеспечении (и в памяти), может создать и находятся в аппаратный модуль безопасности и т. д. Важно точка находится любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.

Тип IAuthenticatedEncryptorConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического смену ключей](../implementation/key-management.md#key-expiration-and-rolling). Чтобы изменить реализацию для всех будущих ключей, зарегистрируйте одноэлементный IAuthenticatedEncryptorConfiguration в контейнере службы.

---
