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
# <a name="miscellaneous-apis"></a>Различные интерфейсы API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.

## <a name="isecret"></a>ISecret

Интерфейс ISecret представляет секретное значение, например материалом ключа шифрования. Он содержит следующие области API.

* Длина: int

* Метод Dispose(): void

* WriteSecretIntoBuffer (ArraySegment<byte> буфера): void

Метод WriteSecretIntoBuffer заполняет предоставленный буфер с необработанное значение секрета. Причина, по которой этот API принимает буфера в качестве параметра, вместо того, чтобы возвращать byte [] является напрямую, это дает вызывающий возможность закрепления объекта буфера ограничение возможности раскрытия секрета управляемого сборщика мусора.

Секретный тип — это конкретная реализация ISecret, где секретное значение хранится в памяти в процессе. На платформах Windows секретное значение шифруется через [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
