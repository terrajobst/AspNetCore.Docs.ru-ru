---
title: "Различные интерфейсы API"
author: rick-anderson
description: "Этот документ посвящен интерфейс ISecret защиты данных ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="607fc-103">Различные интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="607fc-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="607fc-104">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="607fc-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="607fc-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="607fc-105">ISecret</span></span>

<span data-ttu-id="607fc-106">`ISecret` Интерфейс представляет секретное значение, например материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="607fc-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="607fc-107">Он содержит следующие области API:</span><span class="sxs-lookup"><span data-stu-id="607fc-107">It contains the following API surface:</span></span>

* <span data-ttu-id="607fc-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="607fc-108">`Length`: `int`</span></span>

* <span data-ttu-id="607fc-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="607fc-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="607fc-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="607fc-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="607fc-111">`WriteSecretIntoBuffer` Метод заполняет предоставленный буфер с необработанное значение секрета.</span><span class="sxs-lookup"><span data-stu-id="607fc-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="607fc-112">Причина буфера в качестве параметра принимает этот API не возвращается `byte[]` непосредственно является, это дает вызывающий возможность закрепления объекта буфера, ограничение возможности раскрытия секрета управляемого сборщика мусора.</span><span class="sxs-lookup"><span data-stu-id="607fc-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="607fc-113">`Secret` Тип — это конкретная реализация `ISecret` где секретное значение хранится в памяти в процессе.</span><span class="sxs-lookup"><span data-stu-id="607fc-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="607fc-114">На платформах Windows секретное значение шифруется через [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="607fc-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
