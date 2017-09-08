---
title: "Хэширование пароля"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 982a1eb2-1e6f-4909-896f-82784364472d
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 262eed1f310ff3216c893fdc8a738f2ba6ec6729
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="password-hashing"></a>Хэширование пароля

База данных защиты кода включает пакет *Microsoft.AspNetCore.Cryptography.KeyDerivation* которого содержит функции шифрования ключа с наследованием. Данный пакет представляет собой изолированный компонент и не имеет зависимостей от остальной системой защиты данных. Его можно использовать совершенно независимо друг от друга. Наряду с кодом защиты данных, базовый для удобства существует источник.

Пакет сейчас предоставляет метод `KeyDerivation.Pbkdf2` разрешающее хэширования пароля с помощью [PBKDF2 алгоритм](https://tools.ietf.org/html/rfc2898#section-5.2). Этот API очень похожа на .NET Framework существующие [Rfc2898DeriveBytes тип](https://msdn.microsoft.com/library/System.Security.Cryptography.Rfc2898DeriveBytes(v=vs.110).aspx), но имеются три важные различия:

1. `KeyDerivation.Pbkdf2` Метод поддерживает использование нескольких PRFs (в настоящее время `HMACSHA1`, `HMACSHA256`, и `HMACSHA512`), тогда как `Rfc2898DeriveBytes` поддерживает только тип `HMACSHA1`.

2. `KeyDerivation.Pbkdf2` Метод обнаруживает текущей операционной системы и пытается выбирать наиболее оптимизированная реализация процедуру, предоставляя гораздо более высокую производительность в определенных случаях. (В Windows 8 предлагает около 10 x пропускную способность `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Метод требует указать все параметры (хэшу, PRF и число итераций). `Rfc2898DeriveBytes` Тип предоставляет значения по умолчанию для этих.

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

См. исходный код для ASP.NET Core удостоверения `PasswordHasher` вариант использования тип для реального мира.
