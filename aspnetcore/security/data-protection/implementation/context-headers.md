---
title: "Заголовков контекста"
author: rick-anderson
description: "В этом документе перечислены сведения о реализации заголовков контекста защиты данных ASP.NET Core."
keywords: "ASP.NET Core, защиты данных, заголовков контекста"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: eb8e4c9ad67d3046648aea1b45f4a675b41b3ec0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="context-headers"></a><span data-ttu-id="e3aaf-104">Заголовков контекста</span><span class="sxs-lookup"><span data-stu-id="e3aaf-104">Context headers</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="e3aaf-105">Общие сведения и теории</span><span class="sxs-lookup"><span data-stu-id="e3aaf-105">Background and theory</span></span>

<span data-ttu-id="e3aaf-106">В системе защиты данных «ключ» означает, что объект, способный предоставлять службы шифрования с проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-106">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="e3aaf-107">Каждый ключ определяется с помощью уникального идентификатора (GUID) и несет с собой сведения, алгоритма и entropic материал.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-107">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="e3aaf-108">Он предназначен каждый ключ содержат уникальные энтропии, но системе не удается обеспечить, и мы также должны быть учтены для разработчиков, которые могут изменить кольцо ключ вручную, изменив алгоритмической сведения существующего ключа в ключ обмена.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-108">It is intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="e3aaf-109">Для достижения нашей требования безопасности, заданным в таких случаях системы защиты данных имеет смысл [криптографическая гибкость](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), что позволяет безопасно используют одно значение entropic через несколько алгоритмов шифрования.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-109">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="e3aaf-110">Большинство систем, которые поддерживают криптографическая гибкость сделать, включая некоторые идентифицирующие сведения об алгоритме внутри полезных данных.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-110">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="e3aaf-111">Алгоритм OID обычно является хорошим кандидатом для этого.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-111">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="e3aaf-112">Одна из проблем возникла то, что существует несколько способов для указания того же алгоритма: «AES» (CNG) и управляемые Aes, AesManaged, AesCryptoServiceProvider, AesCng и (при наличии конкретные параметры) RijndaelManaged классы одинаковы все очередь и мы бы нужно хранить сопоставление всех их на правильный идентификатор Объекта.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-112">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="e3aaf-113">Если разработчик хотел предоставить пользовательский алгоритм (или даже другую реализацию AES), они бы сообщить нам его OID.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-113">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="e3aaf-114">Лишние регистрации в результате этого действия система конфигурации особенно велики.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-114">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="e3aaf-115">Пошаговое выполнение обратно, мы решили, достигает проблему в обратную сторону.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-115">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="e3aaf-116">OID указывает, является алгоритм, но мы не интересует фактически это.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-116">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="e3aaf-117">Если требуется использовать значение single entropic безопасно в два разных алгоритма, не необходимые нам знать, что фактически, алгоритмы.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-117">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="e3aaf-118">Что мы волнует это поведение.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-118">What we actually care about is how they behave.</span></span> <span data-ttu-id="e3aaf-119">Любой алгоритм довольно симметричный блочный шифр также является строгим псевдослучайное перестановки (политики репликации Паролей): исправьте входные данные (ключ, цепочки режим IV, открытым текстом) и выходной зашифрованного текста с перегрузке вероятности будут отделяться от симметричный блочный шифр алгоритм учетом же входных данных.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-119">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="e3aaf-120">Аналогичным образом любая функция довольно ключевого хэширования также является функцией строгого псевдослучайное (PRF), и на основе фиксированного набора входных свои выходные данные чрезвычайно будут distinct из любой другой функции хэширования с ключом.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-120">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="e3aaf-121">Мы используем эту концепцию строгого PRPs и PRFs для создания заголовка контекста.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-121">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="e3aaf-122">Этот заголовок контекста по существу выступает в качестве стабильного отпечаток через алгоритмы используются для любой заданной операции и предоставляет криптографическая гибкость, системы защиты данных.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-122">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="e3aaf-123">Этот заголовок не возникнут и используется позже как часть [подраздел производного процесса](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="e3aaf-123">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="e3aaf-124">Существует два разных способа для создания заголовка контекста, в зависимости от режима работы базовых алгоритмов.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-124">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="e3aaf-125">Шифрование в режиме CBC + HMAC проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="e3aaf-125">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="e3aaf-126">Заголовок контекста состоит из следующих компонентов:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-126">The context header consists of the following components:</span></span>

* <span data-ttu-id="e3aaf-127">[16 бит] Значение 00 00, что является маркером означает «шифрование CBC + HMAC проверки подлинности».</span><span class="sxs-lookup"><span data-stu-id="e3aaf-127">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="e3aaf-128">[32 бита] Длина ключа (в байтах, с обратным порядком байтов) алгоритм шифрования симметричных блока.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-128">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="e3aaf-129">[32 бита] Размер блока (в байтах, с обратным порядком байтов) алгоритм шифрования симметричных блока.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-129">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="e3aaf-130">[32 бита] Длина ключа (в байтах, с обратным порядком байтов) алгоритм HMAC.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-130">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="e3aaf-131">(В настоящее время размер ключа всегда совпадает с размером хэш-кода.)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-131">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="e3aaf-132">[32 бита] Размер хэш-кода (в байтах, с обратным порядком байтов) алгоритма HMAC.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-132">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="e3aaf-133">EncCBC (K_E, IV, ""), который является результатом работы алгоритма симметричный блочный шифр, пустую строку входных данных и где IV — вектор нулевые.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-133">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="e3aaf-134">Конструирование K_E описан ниже.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-134">The construction of K_E is described below.</span></span>

* <span data-ttu-id="e3aaf-135">MAC (K_H, ""), который является результатом работы алгоритма HMAC, пустая строка входных данных.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-135">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="e3aaf-136">Конструирование K_H описан ниже.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-136">The construction of K_H is described below.</span></span>

<span data-ttu-id="e3aaf-137">В идеальном случае мы передаем нулевые векторов для K_E и K_H.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-137">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="e3aaf-138">Тем не менее мы хотим избежать ситуации, где основной алгоритм проверяет наличие слабых ключей перед выполнением любой операции (в частности DES и 3DES), которая исключает использование шаблона как вектор нулевые простое или repeatable.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-138">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="e3aaf-139">Вместо этого мы используем Формирования NIST SP800-108 в режиме счетчика (см. [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), секунду 5.1) с ключом нулевой длины, метку и контекст и HMACSHA512 как базовый PRF.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-139">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="e3aaf-140">Мы производные | K_E | + | K_H | байт выходных данных, затем разложения результат K_E и K_H сами.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-140">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="e3aaf-141">Математически она представлена следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-141">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="e3aaf-142">(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, ключ = "», метки ="», контекст = "")</span><span class="sxs-lookup"><span data-stu-id="e3aaf-142">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="e3aaf-143">Пример: AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="e3aaf-143">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="e3aaf-144">Например рассмотрим случай, где блок симметричный алгоритм шифрования AES-192-CBC она алгоритм проверки HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-144">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="e3aaf-145">Система создает заголовок контекста, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-145">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="e3aaf-146">Во-первых, (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, ключ = "», метки ="», контекст = ""), где | K_E | = 192 бита и | K_H | = 256 бит в указанной алгоритмов.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-146">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="e3aaf-147">Это приводит к K_E = 5BB6... 21DD и K_H = A04A... 00A9 в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-147">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="e3aaf-148">Далее вычислений Enc_CBC (K_E, IV, «») для AES-192-CBC заданный вектор Инициализации = 0 * и K_E, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-148">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="e3aaf-149">результат: = F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="e3aaf-149">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="e3aaf-150">Далее вычислений MAC (K_H, "") для HMACSHA256, учитывая K_H, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-150">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="e3aaf-151">результат: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="e3aaf-151">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="e3aaf-152">В результате получается полный заголовок ниже:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-152">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="e3aaf-153">Этот заголовок контекста является отпечаток пары алгоритм шифрования, прошедшего проверку подлинности (шифрование AES-192-CBC + HMACSHA256 проверки).</span><span class="sxs-lookup"><span data-stu-id="e3aaf-153">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="e3aaf-154">Компоненты, как описано [выше](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) являются:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-154">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="e3aaf-155">маркер (00 00)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-155">the marker (00 00)</span></span>

* <span data-ttu-id="e3aaf-156">Длина ключа для шифрования блока (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-156">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="e3aaf-157">размер блока блоков шифра (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-157">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="e3aaf-158">Длина ключа HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-158">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="e3aaf-159">размер хэш-кода HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-159">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="e3aaf-160">блочный шифр выходной политики репликации Паролей (F4 74 - DB 6F) и</span><span class="sxs-lookup"><span data-stu-id="e3aaf-160">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="e3aaf-161">выходные данные HMAC PRF (D4 79 - end).</span><span class="sxs-lookup"><span data-stu-id="e3aaf-161">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="e3aaf-162">Шифрование в режиме CBC + HMAC заголовок проверки подлинности контекста создается так же, независимо от того, предоставляются ли реализации алгоритмы Windows CNG или управляемых типов SymmetricAlgorithm и KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-162">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="e3aaf-163">Это позволяет приложениям, выполняющимся в различных операционных системах, надежно создавать один и тот же заголовок контекста, несмотря на то, что реализации алгоритмов различаться в разных операционных систем.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-163">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="e3aaf-164">(На практике KeyedHashAlgorithm не должен быть правильный HMAC.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-164">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="e3aaf-165">Он может быть любой тип алгоритма хэширования с ключом.)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-165">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="e3aaf-166">Например: 3DES-192-CBC + HMAC-SHA1</span><span class="sxs-lookup"><span data-stu-id="e3aaf-166">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="e3aaf-167">Во-первых, (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, ключ = "», метки ="», контекст = ""), где | K_E | = 192 бита и | K_H | = 160 бит в указанной алгоритмов.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-167">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="e3aaf-168">Это приводит к K_E = A219... E2BB и K_H = DC4A... B464 в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-168">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="e3aaf-169">Далее вычислений Enc_CBC (K_E, IV, «») для 3DES-192-CBC заданный вектор Инициализации = 0 * и K_E, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-169">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="e3aaf-170">результат: = ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="e3aaf-170">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="e3aaf-171">Далее вычислений MAC (K_H, "") для HMAC-SHA1, учитывая K_H, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-171">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="e3aaf-172">результат: = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="e3aaf-172">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="e3aaf-173">В результате получается полный заголовок, который является отпечаток прошедшего проверку подлинности пары алгоритм шифрования (шифрование 3DES-192-CBC + проверки HMAC-SHA1), показано ниже:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-173">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="e3aaf-174">Компоненты разбить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-174">The components break down as follows:</span></span>

* <span data-ttu-id="e3aaf-175">маркер (00 00)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-175">the marker (00 00)</span></span>

* <span data-ttu-id="e3aaf-176">Длина ключа для шифрования блока (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-176">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="e3aaf-177">размер блока блоков шифра (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-177">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="e3aaf-178">Длина ключа HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-178">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="e3aaf-179">размер хэш-кода HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-179">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="e3aaf-180">блочный шифр выходной политики репликации Паролей (B1 AB - E1 0E) и</span><span class="sxs-lookup"><span data-stu-id="e3aaf-180">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="e3aaf-181">выходные данные HMAC PRF (76 EB - end).</span><span class="sxs-lookup"><span data-stu-id="e3aaf-181">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="e3aaf-182">Режим Galois/счетчика шифрования + проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="e3aaf-182">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="e3aaf-183">Заголовок контекста состоит из следующих компонентов:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-183">The context header consists of the following components:</span></span>

* <span data-ttu-id="e3aaf-184">[16 бит] Значение 00 01, которая является маркером означает «шифрование GCM + проверки подлинности».</span><span class="sxs-lookup"><span data-stu-id="e3aaf-184">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="e3aaf-185">[32 бита] Длина ключа (в байтах, с обратным порядком байтов) алгоритм шифрования симметричных блока.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-185">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="e3aaf-186">[32 бита] Nonce размер (в байтах, с обратным порядком байтов), используемый во время операции шифрования с проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-186">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="e3aaf-187">(Для нашей системы, это имеет фиксированный размер nonce = 96 битов.)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-187">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="e3aaf-188">[32 бита] Размер блока (в байтах, с обратным порядком байтов) алгоритм шифрования симметричных блока.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-188">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="e3aaf-189">(Для GCM, это имеет фиксированный размер блока = 128 бит.)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-189">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="e3aaf-190">[32 бита] Проверка подлинности тег размер (в байтах, с обратным порядком байтов) созданном функцией шифрования прошедшего проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-190">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="e3aaf-191">(Для нашей системы, это имеет фиксированный размер тег = 128 бит.)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-191">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="e3aaf-192">[128 бит] Тег Enc_GCM (K_E, nonce, «»), который является результатом работы алгоритма симметричный блочный шифр, пустую строку входных данных и размещения nonce нулевые 96-разрядного вектора.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-192">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="e3aaf-193">K_E формируется с помощью того же механизма, как и шифрования CBC + HMAC сценарии проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-193">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="e3aaf-194">Тем не менее, поскольку здесь не K_H, мы фактически имеют | K_H | = 0, и алгоритм сворачивает форму ниже.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-194">However, since there is no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="e3aaf-195">K_E = SP800_108_CTR (prf = HMACSHA512, ключ = "», метки ="», контекст = "")</span><span class="sxs-lookup"><span data-stu-id="e3aaf-195">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="e3aaf-196">Пример: AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="e3aaf-196">Example: AES-256-GCM</span></span>

<span data-ttu-id="e3aaf-197">Сначала разрешить K_E = SP800_108_CTR (prf = HMACSHA512, ключ = "», метки ="», контекст = ""), где | K_E | = 256 бит.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-197">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="e3aaf-198">K_E: = 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="e3aaf-198">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="e3aaf-199">Далее вычислений тега Enc_GCM проверки подлинности (K_E, nonce, «») для AES-256-GCM заданному nonce = 096 и K_E как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e3aaf-199">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="e3aaf-200">результат: = E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="e3aaf-200">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="e3aaf-201">В результате получается полный заголовок ниже:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-201">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="e3aaf-202">Компоненты разбить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e3aaf-202">The components break down as follows:</span></span>

   * <span data-ttu-id="e3aaf-203">маркер (00 01)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-203">the marker (00 01)</span></span>

   * <span data-ttu-id="e3aaf-204">Длина ключа для шифрования блока (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-204">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="e3aaf-205">размер nonce (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-205">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="e3aaf-206">размер блока блоков шифра (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="e3aaf-206">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="e3aaf-207">размер тега (00 00 00 10) для проверки подлинности и</span><span class="sxs-lookup"><span data-stu-id="e3aaf-207">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="e3aaf-208">тег аутентификации из работающих блочного шифра (DC E7 — end).</span><span class="sxs-lookup"><span data-stu-id="e3aaf-208">the authentication tag from running the block cipher (E7 DC - end).</span></span>
