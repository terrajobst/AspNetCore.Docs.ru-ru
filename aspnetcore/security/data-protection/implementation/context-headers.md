---
title: Заголовки контекста в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о реализации защиты данных в ASP.NET Core контекста заголовков.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 518423f5df93924d3df144994e4beb1755cd0bfc
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814016"
---
# <a name="context-headers-in-aspnet-core"></a><span data-ttu-id="e4999-103">Заголовки контекста в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4999-103">Context headers in ASP.NET Core</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="e4999-104">Общие сведения и теории</span><span class="sxs-lookup"><span data-stu-id="e4999-104">Background and theory</span></span>

<span data-ttu-id="e4999-105">В системе защиты данных «ключа» означает, что объект, который может предоставить службы шифрования прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="e4999-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="e4999-106">Каждый ключ определяется с помощью уникального идентификатора (GUID), поэтому он несет с собой алгоритмического сведения и entropic материал.</span><span class="sxs-lookup"><span data-stu-id="e4999-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="e4999-107">Она предназначена, каждый ключ выполнения уникальный энтропии, но системы не поддерживают применение, и мы также должны учитывать для разработчиков, которые могут изменить набора ключей вручную, изменив алгоритмического сведения существующего ключа в связку ключей.</span><span class="sxs-lookup"><span data-stu-id="e4999-107">It's intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="e4999-108">Для достижения наших требований безопасности, учитывая этих случаях система защиты данных существует понятие [криптографическая гибкость](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), что позволяет безопасно используют одно значение entropic через несколько алгоритмов шифрования.</span><span class="sxs-lookup"><span data-stu-id="e4999-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="e4999-109">Большинство систем, которые поддерживают криптографическая гибкость сделать это, включая некоторые идентификационные сведения об алгоритме внутри полезных данных.</span><span class="sxs-lookup"><span data-stu-id="e4999-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="e4999-110">OID алгоритма и обычно является хорошим кандидатом для этого.</span><span class="sxs-lookup"><span data-stu-id="e4999-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="e4999-111">Одна из проблем, мы столкнулись то, что существует несколько способов для указания того же алгоритма: «AES» (CNG) и управляемых Aes, aesmanaged совместим со, AesCryptoServiceProvider, AesCng и (при наличии определенных параметров) RijndaelManaged классы являются фактически все то же самое, и мы должны будут хранить сопоставление все действия, чтобы правильный идентификатор Объекта.</span><span class="sxs-lookup"><span data-stu-id="e4999-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="e4999-112">Если разработчик хотел предоставить пользовательский алгоритм (или даже другую реализацию AES!), они бы сообщить нам его OID.</span><span class="sxs-lookup"><span data-stu-id="e4999-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="e4999-113">Дополнительный регистрации на этом шаге создается конфигурация системы затраты особенно велики.</span><span class="sxs-lookup"><span data-stu-id="e4999-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="e4999-114">Переход назад, мы решили, что мы достиг проблему с неправильно.</span><span class="sxs-lookup"><span data-stu-id="e4999-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="e4999-115">OID указывает, что такое алгоритм, но нас не волнуют фактически это.</span><span class="sxs-lookup"><span data-stu-id="e4999-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="e4999-116">Если нам нужно безопасно использовать одно значение entropic в два разных алгоритма, не необходимые нам знать, что фактически, алгоритмы.</span><span class="sxs-lookup"><span data-stu-id="e4999-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="e4999-117">Что мы действительно заботитесь, — это поведение.</span><span class="sxs-lookup"><span data-stu-id="e4999-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="e4999-118">Любой алгоритм неплохую симметричного блочного шифра также является строгим перестановка псевдослучайных чисел (PRP): исправить входные данные (ключ, цепочки режим, IV, открытым текстом) и выходные данные зашифрованного текста с забрасывая вероятности будут отделяться от других симметричный блочный шифр алгоритм, учитывая одинаковыми входными данными.</span><span class="sxs-lookup"><span data-stu-id="e4999-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="e4999-119">Аналогичным образом любая функция, неплохую хеша также является строгим псевдослучайные функции (PRF), и на основе фиксированного набора входных его выходные данные преимущественно будет отличается от любой другой с ключами хэш-функции.</span><span class="sxs-lookup"><span data-stu-id="e4999-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="e4999-120">Мы используем эту концепцию строгой PRP и PRFs для создания заголовка контекста.</span><span class="sxs-lookup"><span data-stu-id="e4999-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="e4999-121">Этот заголовок контекста по существу выступает в качестве стабильной отпечаток через алгоритмы, используемые для любой конкретной операции, и предоставляет криптографическая гибкость, необходимые системе защиты данных.</span><span class="sxs-lookup"><span data-stu-id="e4999-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="e4999-122">Этот заголовок можно воспроизвести и используется позже как часть [подраздел процесс наследования](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="e4999-122">This header is reproducible and is used later as part of the [subkey derivation process](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="e4999-123">Существует два разных способа для создания заголовка контекста, в зависимости от режима работы базовых алгоритмов.</span><span class="sxs-lookup"><span data-stu-id="e4999-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="e4999-124">Шифрование в режиме CBC + проверка подлинности HMAC</span><span class="sxs-lookup"><span data-stu-id="e4999-124">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="e4999-125">Заголовок контекста состоит из следующих компонентов:</span><span class="sxs-lookup"><span data-stu-id="e4999-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="e4999-126">[16 бит] Значение 00 00, что — это метка это означает «шифрование CBC + HMAC проверки подлинности».</span><span class="sxs-lookup"><span data-stu-id="e4999-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="e4999-127">[32 бита] Длина ключа (в байтах, с прямым порядком байтов) алгоритм симметричного блочного шифрования.</span><span class="sxs-lookup"><span data-stu-id="e4999-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="e4999-128">[32 бита] Размер блока (в байтах, с прямым порядком байтов) алгоритма симметричного блочного шифра.</span><span class="sxs-lookup"><span data-stu-id="e4999-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="e4999-129">[32 бита] Длина ключа (в байтах, с прямым порядком байтов) алгоритм HMAC.</span><span class="sxs-lookup"><span data-stu-id="e4999-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="e4999-130">(В настоящее время размер ключа всегда соответствует размер хэш-кода.)</span><span class="sxs-lookup"><span data-stu-id="e4999-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="e4999-131">[32 бита] Размер хэш-кода (в байтах, с прямым порядком байтов) алгоритма HMAC.</span><span class="sxs-lookup"><span data-stu-id="e4999-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="e4999-132">EncCBC (K_E, IV, ""), который является результатом работы алгоритма симметричного блочного шифра, заданным входным пустую строку, и где IV — вектор все ноль.</span><span class="sxs-lookup"><span data-stu-id="e4999-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="e4999-133">Конструирование K_E описан ниже.</span><span class="sxs-lookup"><span data-stu-id="e4999-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="e4999-134">MAC (K_H, ""), который является результатом работы алгоритма HMAC, заданным входным пустая строка.</span><span class="sxs-lookup"><span data-stu-id="e4999-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="e4999-135">Конструирование K_H описан ниже.</span><span class="sxs-lookup"><span data-stu-id="e4999-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="e4999-136">В идеальном случае мы передаем векторы нулевой all, K_E и K_H.</span><span class="sxs-lookup"><span data-stu-id="e4999-136">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="e4999-137">Тем не менее мы хотим избежать ситуации, где базовый алгоритм проверяет, наличие слабых ключи перед выполнением любых операций (в частности DES и 3DES), которая не позволяет с помощью такой шаблон простой или повторяющихся операций, как вектор все ноль.</span><span class="sxs-lookup"><span data-stu-id="e4999-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="e4999-138">Вместо этого мы используем NIST SP800-108 Проблемы в режиме счетчика (см. в разделе [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), 5.1 в секунду) с ключом нулевой длины, метки и контекст и HMACSHA512 как базового PRF.</span><span class="sxs-lookup"><span data-stu-id="e4999-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="e4999-139">Мы получаем | K_E | + | K_H | байт из выходных данных, затем разложения результат K_E и K_H сами.</span><span class="sxs-lookup"><span data-stu-id="e4999-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="e4999-140">Математически она представлена следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e4999-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="e4999-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span><span class="sxs-lookup"><span data-stu-id="e4999-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="e4999-142">Пример AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="e4999-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="e4999-143">Например рассмотрим случай, где алгоритм симметричного блочного шифрования — AES-192-CBC, а алгоритм проверки — HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="e4999-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="e4999-144">Система будет создавать заголовок контекста, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="e4999-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="e4999-145">Во-первых, позвольте (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", метка = "", контекст = ""), где | K_E | = 192 бита и | K_H | = 256 бит в указанной алгоритмов.</span><span class="sxs-lookup"><span data-stu-id="e4999-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="e4999-146">Это приводит к K_E = 5BB6... 21DD и K_H = A04A... 00A9 в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e4999-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="e4999-147">Затем вычислить Enc_CBC (K_E, IV, «») для AES-192-CBC учитывая IV = 0 \* и K_E, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e4999-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="e4999-148">result := F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="e4999-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="e4999-149">Затем вычислений MAC (K_H, "") для HMACSHA256, учитывая K_H, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e4999-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="e4999-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="e4999-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="e4999-151">В результате получается полный заголовок ниже:</span><span class="sxs-lookup"><span data-stu-id="e4999-151">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="e4999-152">Этот заголовок контекста — отпечаток пары алгоритм шифрование с проверкой подлинности (шифрование AES-192-CBC + HMACSHA256 проверки).</span><span class="sxs-lookup"><span data-stu-id="e4999-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="e4999-153">Компоненты, как описано [выше](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) являются:</span><span class="sxs-lookup"><span data-stu-id="e4999-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="e4999-154">маркер (00 00)</span><span class="sxs-lookup"><span data-stu-id="e4999-154">the marker (00 00)</span></span>

* <span data-ttu-id="e4999-155">Длина ключа для шифрования блока (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="e4999-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="e4999-156">размер блока шифра блока (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="e4999-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="e4999-157">Длина ключа HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="e4999-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="e4999-158">размер хэш-кода HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="e4999-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="e4999-159">блочный шифр выходной политики репликации Паролей (F4 74 - DB 6F) и</span><span class="sxs-lookup"><span data-stu-id="e4999-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="e4999-160">выходные данные HMAC PRF (D4 79 - end).</span><span class="sxs-lookup"><span data-stu-id="e4999-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="e4999-161">Шифрование в режиме CBC + HMAC заголовок проверки подлинности контекста создается так же, независимо от того, предоставляются ли реализаций алгоритмы Windows CNG или управляемых типов SymmetricAlgorithm и KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="e4999-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="e4999-162">Это позволяет приложениям, выполняющимся в различных операционных системах, надежно производить один и тот же заголовок контекста, несмотря на то, что реализации алгоритмов отличаются в разных операционных системах.</span><span class="sxs-lookup"><span data-stu-id="e4999-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="e4999-163">(На практике KeyedHashAlgorithm не обязательно должен быть правильной HMAC.</span><span class="sxs-lookup"><span data-stu-id="e4999-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="e4999-164">Он может быть любого типа алгоритма хэширования с ключом.)</span><span class="sxs-lookup"><span data-stu-id="e4999-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="e4999-165">Пример 3DES-192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="e4999-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="e4999-166">Во-первых, позвольте (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", метка = "", контекст = ""), где | K_E | = 192 бита и | K_H | = 160 бит в указанной алгоритмов.</span><span class="sxs-lookup"><span data-stu-id="e4999-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="e4999-167">Это приводит к K_E = A219... E2BB и K_H = DC4A... B464 в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e4999-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="e4999-168">Затем вычислить Enc_CBC (K_E, IV, «») для 3DES-192-CBC учитывая IV = 0 \* и K_E, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e4999-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="e4999-169">result := ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="e4999-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="e4999-170">Затем вычислений MAC (K_H, "") для HMAC-SHA1, учитывая K_H, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e4999-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="e4999-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="e4999-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="e4999-172">В результате получается полный заголовок, который является отпечатком прошедшего проверку подлинности пары алгоритм шифрования (шифрование 3DES-192-CBC + проверки HMAC-SHA1), показано ниже:</span><span class="sxs-lookup"><span data-stu-id="e4999-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="e4999-173">Компоненты разбить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e4999-173">The components break down as follows:</span></span>

* <span data-ttu-id="e4999-174">маркер (00 00)</span><span class="sxs-lookup"><span data-stu-id="e4999-174">the marker (00 00)</span></span>

* <span data-ttu-id="e4999-175">Длина ключа для шифрования блока (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="e4999-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="e4999-176">размер блока шифра блока (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="e4999-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="e4999-177">Длина ключа HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="e4999-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="e4999-178">размер хэш-кода HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="e4999-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="e4999-179">блочный шифр выходной политики репликации Паролей (B1 AB - E1 0E) и</span><span class="sxs-lookup"><span data-stu-id="e4999-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="e4999-180">выходные данные HMAC PRF (76 EB - end).</span><span class="sxs-lookup"><span data-stu-id="e4999-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="e4999-181">Режим Galois/счетчика шифрования + проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="e4999-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="e4999-182">Заголовок контекста состоит из следующих компонентов:</span><span class="sxs-lookup"><span data-stu-id="e4999-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="e4999-183">[16 бит] Значение 00 01, который является маркером это означает «GCM шифрования и проверки подлинности».</span><span class="sxs-lookup"><span data-stu-id="e4999-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="e4999-184">[32 бита] Длина ключа (в байтах, с прямым порядком байтов) алгоритм симметричного блочного шифрования.</span><span class="sxs-lookup"><span data-stu-id="e4999-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="e4999-185">[32 бита] Nonce размер (в байтах, с прямым порядком байтов), используемый во время операций шифрование с проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="e4999-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="e4999-186">(Для нашей системы, эта проблема устранена в размере nonce = 96 битов.)</span><span class="sxs-lookup"><span data-stu-id="e4999-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="e4999-187">[32 бита] Размер блока (в байтах, с прямым порядком байтов) алгоритма симметричного блочного шифра.</span><span class="sxs-lookup"><span data-stu-id="e4999-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="e4999-188">(Для GCM, это имеет фиксированный размер блока 128 бит.)</span><span class="sxs-lookup"><span data-stu-id="e4999-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="e4999-189">[32 бита] Проверка подлинности тега размер (в байтах, с прямым порядком байтов) создаются с помощью функции шифрование с проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="e4999-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="e4999-190">(Для нашей системы, это имеет фиксированный размер метки = 128 бит.)</span><span class="sxs-lookup"><span data-stu-id="e4999-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="e4999-191">[128 бит] Тег Enc_GCM (K_E, nonce, «»), который является результатом работы алгоритма симметричного блочного шифра, заданным входным пустую строку и где nonce — вектор все ноль 96-разрядного.</span><span class="sxs-lookup"><span data-stu-id="e4999-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="e4999-192">K_E формируется с помощью того же механизма, как шифрование CBC + сценарии проверки подлинности HMAC.</span><span class="sxs-lookup"><span data-stu-id="e4999-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="e4999-193">Тем не менее, так как здесь нет K_H, у нас по сути есть | K_H | = 0, и алгоритм сворачивается до под формой.</span><span class="sxs-lookup"><span data-stu-id="e4999-193">However, since there's no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="e4999-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span><span class="sxs-lookup"><span data-stu-id="e4999-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="e4999-195">Пример AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="e4999-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="e4999-196">Во-первых, позвольте K_E = SP800_108_CTR (prf = HMACSHA512, key = "", метка = "", контекст = ""), где | K_E | = 256 бит.</span><span class="sxs-lookup"><span data-stu-id="e4999-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="e4999-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="e4999-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="e4999-198">Затем вычислений в тег проверки подлинности Enc_GCM (K_E, nonce, «») для алгоритма AES-256-GCM учитывая nonce = 096 и K_E как описано выше.</span><span class="sxs-lookup"><span data-stu-id="e4999-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="e4999-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="e4999-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="e4999-200">В результате получается полный заголовок ниже:</span><span class="sxs-lookup"><span data-stu-id="e4999-200">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="e4999-201">Компоненты разбить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e4999-201">The components break down as follows:</span></span>

* <span data-ttu-id="e4999-202">маркер (00 01)</span><span class="sxs-lookup"><span data-stu-id="e4999-202">the marker (00 01)</span></span>

* <span data-ttu-id="e4999-203">Длина ключа для шифрования блока (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="e4999-203">the block cipher key length (00 00 00 20)</span></span>

* <span data-ttu-id="e4999-204">nonce размер (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="e4999-204">the nonce size (00 00 00 0C)</span></span>

* <span data-ttu-id="e4999-205">размер блока шифра блока (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="e4999-205">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="e4999-206">размер тега (00 00 00 10) для проверки подлинности и</span><span class="sxs-lookup"><span data-stu-id="e4999-206">the authentication tag size (00 00 00 10) and</span></span>

* <span data-ttu-id="e4999-207">тег проверки подлинности запуск блочный шифр (E7 DC - end).</span><span class="sxs-lookup"><span data-stu-id="e4999-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
