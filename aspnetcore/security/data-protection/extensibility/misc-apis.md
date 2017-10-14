---
title: "Различные интерфейсы API"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 72274a0da94a14bcd5af11d245ba626214fa2607
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="dfb31-103">Различные интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="dfb31-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="dfb31-104">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="dfb31-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="dfb31-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="dfb31-105">ISecret</span></span>

<span data-ttu-id="dfb31-106">Интерфейс ISecret представляет секретное значение, например материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="dfb31-106">The ISecret interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="dfb31-107">Он содержит следующие области API.</span><span class="sxs-lookup"><span data-stu-id="dfb31-107">It contains the following API surface.</span></span>

* <span data-ttu-id="dfb31-108">Длина: int</span><span class="sxs-lookup"><span data-stu-id="dfb31-108">Length : int</span></span>

* <span data-ttu-id="dfb31-109">Метод Dispose(): void</span><span class="sxs-lookup"><span data-stu-id="dfb31-109">Dispose() : void</span></span>

* <span data-ttu-id="dfb31-110">WriteSecretIntoBuffer (ArraySegment<byte> буфера): void</span><span class="sxs-lookup"><span data-stu-id="dfb31-110">WriteSecretIntoBuffer(ArraySegment<byte> buffer) : void</span></span>

<span data-ttu-id="dfb31-111">Метод WriteSecretIntoBuffer заполняет предоставленный буфер с необработанное значение секрета.</span><span class="sxs-lookup"><span data-stu-id="dfb31-111">The WriteSecretIntoBuffer method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="dfb31-112">Причина, по которой этот API принимает буфера в качестве параметра, вместо того, чтобы возвращать byte [] является напрямую, это дает вызывающий возможность закрепления объекта буфера ограничение возможности раскрытия секрета управляемого сборщика мусора.</span><span class="sxs-lookup"><span data-stu-id="dfb31-112">The reason this API takes the buffer as a parameter rather than returning a byte[] directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="dfb31-113">Секретный тип — это конкретная реализация ISecret, где секретное значение хранится в памяти в процессе.</span><span class="sxs-lookup"><span data-stu-id="dfb31-113">The Secret type is a concrete implementation of ISecret where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="dfb31-114">На платформах Windows секретное значение шифруется через [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="dfb31-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
