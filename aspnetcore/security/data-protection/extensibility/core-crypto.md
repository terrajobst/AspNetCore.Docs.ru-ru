---
title: Расширяемость базового шифрования в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer и фабрику верхнего уровня.
ms.author: riande
ms.date: 8/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: cf4a142992efe5b00a75285ef9ad9735fe7be411
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896681"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a>Расширяемость базового шифрования в ASP.NET Core

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Типы, реализующие любые из следующих интерфейсов должны быть потокобезопасными для нескольких клиентов.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor** интерфейс является основной строительный блок подсистема шифрования. Обычно имеется один IAuthenticatedEncryptor на ключ, и экземпляр IAuthenticatedEncryptor переносит все материал криптографического ключа и алгоритма сведения, необходимые для выполнения криптографических операций.

Как предполагает имя, тип отвечает за предоставление прошедшим проверку подлинности служб шифрования и расшифровки. Он предоставляет следующие два API.

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

Метод Encrypt возвращает большой двоичный объект, содержащий зашифрованные открытым текстом и с тегом проверки подлинности. Тег проверки подлинности должен охватывать дополнительные данные прошедшего проверку подлинности (AAD), хотя сам AAD не требуется восстановить из окончательных полезных данных. Метод расшифровки проверяет тег проверки подлинности и возвращает deciphered полезных данных. Все сбои (за исключением ArgumentNullException и аналогичных) должен быть homogenized для CryptographicException.

> [!NOTE]
> Сам экземпляр IAuthenticatedEncryptor фактически не должен содержать материал ключа. Например реализация может делегировать аппаратный модуль безопасности для всех операций.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Как создать IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра. Ее API выглядит следующим образом.

* CreateEncryptorInstance (ключ IKey): IAuthenticatedEncryptor

Для любого заданного экземпляра IKey любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance следует учитывать эквивалент, как в следующем примере кода.

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

**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) экземпляра. Ее API выглядит следующим образом.

* CreateEncryptorInstance() : IAuthenticatedEncryptor

* ExportToXml() : XmlSerializedDescriptorInfo

Как и IAuthenticatedEncryptor экземпляр IAuthenticatedEncryptorDescriptor считается программы-оболочки для одного конкретного ключа. Это означает, что для любого заданного экземпляра IAuthenticatedEncryptorDescriptor любого прошедшего проверку подлинности шифраторы, созданные методом его CreateEncryptorInstance следует учитывать эквивалент, как в следующем примере кода.

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

**IAuthenticatedEncryptorDescriptor** интерфейс представляет тип, который знает, как экспортировать сама себя в XML. Ее API выглядит следующим образом.

* ExportToXml() : XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>XML-сериализация

Основное различие между IAuthenticatedEncryptor и IAuthenticatedEncryptorDescriptor является то, что дескриптор знает, как создать шифратор и задать в нем допустимые аргументы. Рассмотрите возможность IAuthenticatedEncryptor, реализация которой зависит от того, SymmetricAlgorithm и KeyedHashAlgorithm. Задание шифратора, – это использование этих типов, но он не обязательно знать эти типы происхождения, поэтому его нельзя записывать действительно правильное описание как воссоздать сам при повторном запуске приложения. Дескриптор действует как более высокий уровень поверх этого. Так как дескриптор знает, как создать экземпляр шифратор (например, он знает, как создать необходимые алгоритмы), он может сериализовать эти знания в формате XML, таким образом, чтобы экземпляр шифратора можно воссоздать после сброса приложения.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

Дескриптор может быть сериализован с помощью его ExportToXml подпрограммы. Эта процедура возвращает XmlSerializedDescriptorInfo, который содержит два свойства: это представление XElement дескриптор и тип, представляющий [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) которого может быть используется, чтобы восстановить этот дескриптор, учитывая соответствующие XElement.

Сериализованный дескриптора могут содержать конфиденциальные сведения, такие как материал криптографического ключа. Система защиты данных имеется встроенная поддержка для шифрования данных, прежде чем он сохранен в хранилище. Чтобы воспользоваться преимуществами, дескриптор необходимо пометить элемент, который содержит конфиденциальные данные с именем атрибута «requiresEncryption» (xmlns "<http://schemas.asp.net/2015/03/dataProtection>«), значение «true».

>[!TIP]
> Нет API модуля поддержки для установки этого атрибута. Вызовите метод расширения XElement.MarkAsRequiresEncryption(), расположенный в пространстве имен Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.

Также можно случаев, где дескриптор сериализованный не содержит конфиденциальные сведения. Давайте снова рассмотрим случай криптографический ключ, хранящийся в HSM. Дескриптор нельзя записывать материал ключа, при сериализации самой, так как аппаратный модуль безопасности не будет предоставлять материал в виде открытого текста. Вместо этого дескриптора может записать ключ в программе-оболочке версии ключ (если аппаратный модуль безопасности позволяет выполнять экспорт таким образом) или аппаратным модулем безопасности собственный уникальный идентификатор для ключа.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer** интерфейс представляет тип, который знает, как для десериализации экземпляра IAuthenticatedEncryptorDescriptor из элемента XElement. Она предоставляет единственный метод:

* ImportFromXml (элемент XElement): IAuthenticatedEncryptorDescriptor

Метод ImportFromXml принимает XElement, который был возвращен [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) и создает эквивалент исходного IAuthenticatedEncryptorDescriptor.

Типы, реализующие IAuthenticatedEncryptorDescriptorDeserializer должны иметь одно из следующих двух открытые конструкторы:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> IServiceProvider, переданный в конструктор может иметь значение null.

## <a name="the-top-level-factory"></a>Фабрика верхнего уровня

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration** класс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров. Он предоставляет единый интерфейс API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Считать AlgorithmConfiguration фабрики верхнего уровня. Конфигурация служит в качестве шаблона. Он обертывает алгоритмического сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но еще не связан с указанным ключом.

При вызове CreateNewDescriptor, исключительно для этого вызова создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor оболочку этот материал ключа и алгоритма сведения, необходимые для использования материала. Материал ключа можно создавать в программном обеспечении (и хранятся в памяти), можно создавать и хранящиеся в аппаратный модуль безопасности и т. д. Ключевым моментом является то, что любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.

Тип AlgorithmConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического ключа обновлением](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Чтобы изменить реализацию для всех будущих ключей, задайте свойство AuthenticatedEncryptorConfiguration в KeyManagementOptions.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration** интерфейс представляет тип, который знает, как создать [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) экземпляров. Он предоставляет единый интерфейс API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Считать IAuthenticatedEncryptorConfiguration фабрики верхнего уровня. Конфигурация служит в качестве шаблона. Он обертывает алгоритмического сведения (например, эта конфигурация создает дескрипторы с главный ключ AES-128-GCM), но еще не связан с указанным ключом.

При вызове CreateNewDescriptor, исключительно для этого вызова создается новый материал ключа и создается новый IAuthenticatedEncryptorDescriptor оболочку этот материал ключа и алгоритма сведения, необходимые для использования материала. Материал ключа можно создавать в программном обеспечении (и хранятся в памяти), можно создавать и хранящиеся в аппаратный модуль безопасности и т. д. Ключевым моментом является то, что любые два вызова CreateNewDescriptor никогда не следует создавать эквивалентные IAuthenticatedEncryptorDescriptor экземпляров.

Тип IAuthenticatedEncryptorConfiguration служит в качестве точки входа для создания ключа подпрограммы например [автоматического ключа обновлением](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Чтобы изменить реализацию для всех будущих ключей, зарегистрируйте одноэлементный IAuthenticatedEncryptorConfiguration в контейнере службы.

---
