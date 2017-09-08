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
ms.openlocfilehash: 541dd721a00495632f0d633577b55933c9be03fa
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="d09cd-103">Различные интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="d09cd-103">Miscellaneous APIs</span></span>

<a name=data-protection-extensibility-mics-apis></a>

>[!WARNING]
> <span data-ttu-id="d09cd-104">Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="d09cd-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="d09cd-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="d09cd-105">ISecret</span></span>

<span data-ttu-id="d09cd-106">Интерфейс ISecret представляет секретное значение, например материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="d09cd-106">The ISecret interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="d09cd-107">Он содержит следующие области API.</span><span class="sxs-lookup"><span data-stu-id="d09cd-107">It contains the following API surface.</span></span>

* <span data-ttu-id="d09cd-108">Длина: int</span><span class="sxs-lookup"><span data-stu-id="d09cd-108">Length : int</span></span>

* <span data-ttu-id="d09cd-109">Метод Dispose(): void</span><span class="sxs-lookup"><span data-stu-id="d09cd-109">Dispose() : void</span></span>

* <span data-ttu-id="d09cd-110">WriteSecretIntoBuffer (ArraySegment<byte> буфера): void</span><span class="sxs-lookup"><span data-stu-id="d09cd-110">WriteSecretIntoBuffer(ArraySegment<byte> buffer) : void</span></span>

<span data-ttu-id="d09cd-111">Метод WriteSecretIntoBuffer заполняет предоставленный буфер с необработанное значение секрета.</span><span class="sxs-lookup"><span data-stu-id="d09cd-111">The WriteSecretIntoBuffer method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="d09cd-112">Причина, по которой этот API принимает буфера в качестве параметра, вместо того, чтобы возвращать byte [] является напрямую, это дает вызывающий возможность закрепления объекта буфера ограничение возможности раскрытия секрета управляемого сборщика мусора.</span><span class="sxs-lookup"><span data-stu-id="d09cd-112">The reason this API takes the buffer as a parameter rather than returning a byte[] directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="d09cd-113">Секретный тип — это конкретная реализация ISecret, где секретное значение хранится в памяти в процессе.</span><span class="sxs-lookup"><span data-stu-id="d09cd-113">The Secret type is a concrete implementation of ISecret where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="d09cd-114">На платформах Windows секретное значение шифруется через [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="d09cd-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
