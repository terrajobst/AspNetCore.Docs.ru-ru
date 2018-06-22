---
title: Прочие ASP.NET Core интерфейсы API защиты данных
author: rick-anderson
description: Дополнительные сведения об интерфейсе ISecret защиты данных для ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279159"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="ab3e3-103">Прочие ASP.NET Core интерфейсы API защиты данных</span><span class="sxs-lookup"><span data-stu-id="ab3e3-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="ab3e3-104">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="ab3e3-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="ab3e3-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="ab3e3-105">ISecret</span></span>

<span data-ttu-id="ab3e3-106">`ISecret` Интерфейс представляет секретное значение, например материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="ab3e3-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="ab3e3-107">Он содержит следующие области API:</span><span class="sxs-lookup"><span data-stu-id="ab3e3-107">It contains the following API surface:</span></span>

* <span data-ttu-id="ab3e3-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="ab3e3-108">`Length`: `int`</span></span>

* <span data-ttu-id="ab3e3-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="ab3e3-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="ab3e3-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="ab3e3-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="ab3e3-111">`WriteSecretIntoBuffer` Метод заполняет предоставленный буфер с необработанное значение секрета.</span><span class="sxs-lookup"><span data-stu-id="ab3e3-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="ab3e3-112">Причина буфера в качестве параметра принимает этот API не возвращается `byte[]` непосредственно является, это дает вызывающий возможность закрепления объекта буфера, ограничение возможности раскрытия секрета управляемого сборщика мусора.</span><span class="sxs-lookup"><span data-stu-id="ab3e3-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="ab3e3-113">`Secret` Тип — это конкретная реализация `ISecret` где секретное значение хранится в памяти в процессе.</span><span class="sxs-lookup"><span data-stu-id="ab3e3-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="ab3e3-114">На платформах Windows секретное значение шифруется через [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="ab3e3-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
