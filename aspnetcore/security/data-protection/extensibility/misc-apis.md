---
title: Прочие ASP.NET Core интерфейсы API защиты данных
author: rick-anderson
description: Дополнительные сведения об интерфейсе ISecret защиты данных для ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 484c6a0979a10e7cf2b801873655caa99a42532c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072768"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="15b1e-103">Прочие ASP.NET Core интерфейсы API защиты данных</span><span class="sxs-lookup"><span data-stu-id="15b1e-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="15b1e-104">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="15b1e-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="15b1e-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="15b1e-105">ISecret</span></span>

<span data-ttu-id="15b1e-106">`ISecret` Интерфейс представляет секретное значение, например материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="15b1e-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="15b1e-107">Он содержит следующие области API:</span><span class="sxs-lookup"><span data-stu-id="15b1e-107">It contains the following API surface:</span></span>

* <span data-ttu-id="15b1e-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="15b1e-108">`Length`: `int`</span></span>

* <span data-ttu-id="15b1e-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="15b1e-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="15b1e-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="15b1e-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="15b1e-111">`WriteSecretIntoBuffer` Метод заполняет предоставленный буфер с необработанное значение секрета.</span><span class="sxs-lookup"><span data-stu-id="15b1e-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="15b1e-112">Причина буфера в качестве параметра принимает этот API не возвращается `byte[]` непосредственно является, это дает вызывающий возможность закрепления объекта буфера, ограничение возможности раскрытия секрета управляемого сборщика мусора.</span><span class="sxs-lookup"><span data-stu-id="15b1e-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="15b1e-113">`Secret` Тип — это конкретная реализация `ISecret` где секретное значение хранится в памяти в процессе.</span><span class="sxs-lookup"><span data-stu-id="15b1e-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="15b1e-114">На платформах Windows секретное значение шифруется через [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="15b1e-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
