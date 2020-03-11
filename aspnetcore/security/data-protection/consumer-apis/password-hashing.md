---
title: Хэширование паролей в ASP.NET Core
author: rick-anderson
description: Узнайте, как хэшировать пароли с помощью ASP.NET Core интерфейсов API защиты данных.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 52532f9f4d7f9037609c8273deb8b6964d81c714
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651436"
---
# <a name="hash-passwords-in-aspnet-core"></a>Хэширование паролей в ASP.NET Core

База кода защиты данных включает пакет *Microsoft. AspNetCore. Cryptography. KeyDerivation* , который содержит функции формирования ключа шифрования. Этот пакет является автономным компонентом и не зависит от остальной части системы защиты данных. Его можно использовать полностью независимо друг от друга. Для удобства источник существует рядом с базой кода защиты данных.

В настоящее время пакет содержит метод `KeyDerivation.Pbkdf2`, который позволяет хэшировать пароль с помощью [алгоритма PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Этот API очень похож на существующий [тип Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes).NET Framework, но есть три важных различия:

1. Метод `KeyDerivation.Pbkdf2` поддерживает использование нескольких Прфс (в настоящее время `HMACSHA1`, `HMACSHA256`и `HMACSHA512`), в то время как тип `Rfc2898DeriveBytes` поддерживает только `HMACSHA1`.

2. Метод `KeyDerivation.Pbkdf2` обнаруживает текущую операционную систему и пытается выбрать наиболее оптимизированную реализацию подпрограммы, предоставляя гораздо более высокую производительность в некоторых случаях. (В Windows 8 она предлагает 10 раз пропускную способность `Rfc2898DeriveBytes`.)

3. Метод `KeyDerivation.Pbkdf2` требует, чтобы вызывающий объект указал все параметры (соль, PRF и число итераций). Тип `Rfc2898DeriveBytes` предоставляет значения по умолчанию для этих параметров.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

См. [Исходный код](https://github.com/dotnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) для `PasswordHasher` типа удостоверения ASP.NET Core для реального случая использования.
