---
title: "Различные интерфейсы API"
author: rick-anderson
description: "Этот документ посвящен интерфейс ISecret защиты данных ASP.NET Core."
keywords: "ASP.NET Core, защиты данных, ISecret"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="67bfc-104">Различные интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="67bfc-104">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="67bfc-105">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="67bfc-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="67bfc-106">ISecret</span><span class="sxs-lookup"><span data-stu-id="67bfc-106">ISecret</span></span>

<span data-ttu-id="67bfc-107">`ISecret` Интерфейс представляет секретное значение, например материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="67bfc-107">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="67bfc-108">Он содержит следующие области API:</span><span class="sxs-lookup"><span data-stu-id="67bfc-108">It contains the following API surface:</span></span>

* <span data-ttu-id="67bfc-109">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="67bfc-109">`Length`: `int`</span></span>

* <span data-ttu-id="67bfc-110">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="67bfc-110">`Dispose()`: `void`</span></span>

* <span data-ttu-id="67bfc-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="67bfc-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="67bfc-112">`WriteSecretIntoBuffer` Метод заполняет предоставленный буфер с необработанное значение секрета.</span><span class="sxs-lookup"><span data-stu-id="67bfc-112">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="67bfc-113">Причина буфера в качестве параметра принимает этот API не возвращается `byte[]` непосредственно является, это дает вызывающий возможность закрепления объекта буфера, ограничение возможности раскрытия секрета управляемого сборщика мусора.</span><span class="sxs-lookup"><span data-stu-id="67bfc-113">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="67bfc-114">`Secret` Тип — это конкретная реализация `ISecret` где секретное значение хранится в памяти в процессе.</span><span class="sxs-lookup"><span data-stu-id="67bfc-114">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="67bfc-115">На платформах Windows секретное значение шифруется через [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="67bfc-115">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
