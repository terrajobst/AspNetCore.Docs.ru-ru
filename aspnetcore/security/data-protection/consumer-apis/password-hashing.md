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
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="3ff65-103">Хэширования паролей в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ff65-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="3ff65-104">Базовый код защиты данных включает в себя пакет *Microsoft.AspNetCore.Cryptography.KeyDerivation* которого содержит функции шифрования производного ключа.</span><span class="sxs-lookup"><span data-stu-id="3ff65-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="3ff65-105">Этот пакет отдельного компонента и не имеет зависимостей от остальной части системы защиты данных.</span><span class="sxs-lookup"><span data-stu-id="3ff65-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="3ff65-106">Его можно использовать полностью независимо друг от друга.</span><span class="sxs-lookup"><span data-stu-id="3ff65-106">It can be used completely independently.</span></span> <span data-ttu-id="3ff65-107">Источник существует вместе с кодом защиты данных, базовый для удобства.</span><span class="sxs-lookup"><span data-stu-id="3ff65-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="3ff65-108">Пакет в настоящее время предлагает свой метод `KeyDerivation.Pbkdf2` разрешающее хэширования пароля с помощью [алгоритм PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="3ff65-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="3ff65-109">Этот API очень похож на существующие платформы .NET Framework [Rfc2898DeriveBytes тип](/dotnet/api/system.security.cryptography.rfc2898derivebytes), но имеются три важные различия:</span><span class="sxs-lookup"><span data-stu-id="3ff65-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="3ff65-110">`KeyDerivation.Pbkdf2` Метод поддерживает использование нескольких PRFs (в настоящее время `HMACSHA1`, `HMACSHA256`, и `HMACSHA512`), тогда как `Rfc2898DeriveBytes` типа поддерживает только `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="3ff65-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="3ff65-111">`KeyDerivation.Pbkdf2` Метод обнаруживает текущей операционной системы и пытается выбирать наиболее оптимизированную реализацию процедуры, обеспечивая более высокую производительность в определенных случаях.</span><span class="sxs-lookup"><span data-stu-id="3ff65-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="3ff65-112">(В Windows 8, он предлагает около 10 x пропускную способность `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="3ff65-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="3ff65-113">`KeyDerivation.Pbkdf2` Метод требует вызывающему объекту указать все параметры (случайных данных, PRF и число итераций).</span><span class="sxs-lookup"><span data-stu-id="3ff65-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="3ff65-114">`Rfc2898DeriveBytes` Тип предоставляет значения по умолчанию для них.</span><span class="sxs-lookup"><span data-stu-id="3ff65-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="3ff65-115">См. в разделе [исходный код](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) для ASP.NET Core Identity `PasswordHasher` вариант использования тип из реальной жизни.</span><span class="sxs-lookup"><span data-stu-id="3ff65-115">See the [source code](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
