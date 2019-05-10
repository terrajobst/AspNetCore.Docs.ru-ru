---
title: Хэширования паролей в ASP.NET Core
author: rick-anderson
description: Узнайте, как для хэширования паролей с помощью API защиты данных ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 70301ffffbaaf3c5ff0642b19b80e40be83aa438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896651"
---
# <a name="hash-passwords-in-aspnet-core"></a>Хэширования паролей в ASP.NET Core

Базовый код защиты данных включает в себя пакет *Microsoft.AspNetCore.Cryptography.KeyDerivation* которого содержит функции шифрования производного ключа. Этот пакет отдельного компонента и не имеет зависимостей от остальной части системы защиты данных. Его можно использовать полностью независимо друг от друга. Источник существует вместе с кодом защиты данных, базовый для удобства.

Пакет в настоящее время предлагает свой метод `KeyDerivation.Pbkdf2` разрешающее хэширования пароля с помощью [алгоритм PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Этот API очень похож на существующие платформы .NET Framework [Rfc2898DeriveBytes тип](/dotnet/api/system.security.cryptography.rfc2898derivebytes), но имеются три важные различия:

1. `KeyDerivation.Pbkdf2` Метод поддерживает использование нескольких PRFs (в настоящее время `HMACSHA1`, `HMACSHA256`, и `HMACSHA512`), тогда как `Rfc2898DeriveBytes` типа поддерживает только `HMACSHA1`.

2. `KeyDerivation.Pbkdf2` Метод обнаруживает текущей операционной системы и пытается выбирать наиболее оптимизированную реализацию процедуры, обеспечивая более высокую производительность в определенных случаях. (В Windows 8, он предлагает около 10 x пропускную способность `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Метод требует вызывающему объекту указать все параметры (случайных данных, PRF и число итераций). `Rfc2898DeriveBytes` Тип предоставляет значения по умолчанию для них.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

См. в разделе [исходный код](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) для ASP.NET Core Identity `PasswordHasher` вариант использования тип из реальной жизни.
