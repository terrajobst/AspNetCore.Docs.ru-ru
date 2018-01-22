---
title: "Различные интерфейсы API"
author: rick-anderson
description: "Этот документ посвящен интерфейс ISecret защиты данных ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 88a08a25abf4c4e1ba0746087b05b1cc8fa13024
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="ffdcb-103">Различные интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="ffdcb-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="ffdcb-104">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="ffdcb-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="ffdcb-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="ffdcb-105">ISecret</span></span>

<span data-ttu-id="ffdcb-106">`ISecret` Интерфейс представляет секретное значение, например материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="ffdcb-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="ffdcb-107">Он содержит следующие области API:</span><span class="sxs-lookup"><span data-stu-id="ffdcb-107">It contains the following API surface:</span></span>

* <span data-ttu-id="ffdcb-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="ffdcb-108">`Length`: `int`</span></span>

* <span data-ttu-id="ffdcb-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="ffdcb-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="ffdcb-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="ffdcb-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="ffdcb-111">`WriteSecretIntoBuffer` Метод заполняет предоставленный буфер с необработанное значение секрета.</span><span class="sxs-lookup"><span data-stu-id="ffdcb-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="ffdcb-112">Причина буфера в качестве параметра принимает этот API не возвращается `byte[]` непосредственно является, это дает вызывающий возможность закрепления объекта буфера, ограничение возможности раскрытия секрета управляемого сборщика мусора.</span><span class="sxs-lookup"><span data-stu-id="ffdcb-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="ffdcb-113">`Secret` Тип — это конкретная реализация `ISecret` где секретное значение хранится в памяти в процессе.</span><span class="sxs-lookup"><span data-stu-id="ffdcb-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="ffdcb-114">На платформах Windows секретное значение шифруется через [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="ffdcb-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
