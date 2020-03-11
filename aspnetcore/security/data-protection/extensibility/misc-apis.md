---
title: Прочие ASP.NET Core API-интерфейсы защиты данных
author: rick-anderson
description: Сведения об интерфейсе Исекрет защиты данных ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654358"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="4767f-103">Прочие ASP.NET Core API-интерфейсы защиты данных</span><span class="sxs-lookup"><span data-stu-id="4767f-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="4767f-104">Типы, реализующие любые из следующих интерфейсов должны быть потокобезопасными для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="4767f-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="4767f-105">исекрет</span><span class="sxs-lookup"><span data-stu-id="4767f-105">ISecret</span></span>

<span data-ttu-id="4767f-106">Интерфейс `ISecret` представляет секретное значение, например материал криптографического ключа.</span><span class="sxs-lookup"><span data-stu-id="4767f-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="4767f-107">Он содержит следующую поверхность API:</span><span class="sxs-lookup"><span data-stu-id="4767f-107">It contains the following API surface:</span></span>

* <span data-ttu-id="4767f-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="4767f-108">`Length`: `int`</span></span>

* <span data-ttu-id="4767f-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="4767f-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="4767f-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="4767f-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="4767f-111">Метод `WriteSecretIntoBuffer` заполняет указанный буфер необработанным значением секрета.</span><span class="sxs-lookup"><span data-stu-id="4767f-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="4767f-112">Причина, по которой этот API принимает буфер в качестве параметра, вместо того чтобы возвращать `byte[]` напрямую, это дает вызывающей стороне возможность закрепить объект буфера, ограничивая секретный код управляемому сборщику мусора.</span><span class="sxs-lookup"><span data-stu-id="4767f-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="4767f-113">Тип `Secret` является конкретной реализацией `ISecret`, где секретное значение хранится в памяти внутри процесса.</span><span class="sxs-lookup"><span data-stu-id="4767f-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="4767f-114">На платформах Windows значение секрета шифруется через [криптпротектмемори](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="4767f-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
