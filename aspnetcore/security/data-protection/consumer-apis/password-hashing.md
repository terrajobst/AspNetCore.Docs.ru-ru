---
title: Хэширования паролей в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о хэширования паролей с помощью API защиты данных ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: f44e66789bf348ef6d99f6d862fb34c2d943a0b2
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740105"
---
# <a name="hash-passwords-in-aspnet-core"></a>Хэширования паролей в ASP.NET Core

База данных защиты кода включает пакет *Microsoft.AspNetCore.Cryptography.KeyDerivation* которого содержит функции шифрования ключа с наследованием. Данный пакет представляет собой изолированный компонент и не имеет зависимостей от остальной системой защиты данных. Его можно использовать совершенно независимо друг от друга. Наряду с кодом защиты данных, базовый для удобства существует источник.

Пакет сейчас предоставляет метод `KeyDerivation.Pbkdf2` разрешающее хэширования пароля с помощью [PBKDF2 алгоритм](https://tools.ietf.org/html/rfc2898#section-5.2). Этот API очень похожа на .NET Framework существующие [Rfc2898DeriveBytes тип](/dotnet/api/system.security.cryptography.rfc2898derivebytes), но имеются три важные различия:

1. `KeyDerivation.Pbkdf2` Метод поддерживает использование нескольких PRFs (в настоящее время `HMACSHA1`, `HMACSHA256`, и `HMACSHA512`), тогда как `Rfc2898DeriveBytes` поддерживает только тип `HMACSHA1`.

2. `KeyDerivation.Pbkdf2` Метод обнаруживает текущей операционной системы и пытается выбирать наиболее оптимизированная реализация процедуру, предоставляя гораздо более высокую производительность в определенных случаях. (В Windows 8 предлагает около 10 x пропускную способность `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Метод требует указать все параметры (хэшу, PRF и число итераций). `Rfc2898DeriveBytes` Тип предоставляет значения по умолчанию для этих.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

См. исходный код для ASP.NET Core удостоверения `PasswordHasher` вариант использования тип для реального мира.
