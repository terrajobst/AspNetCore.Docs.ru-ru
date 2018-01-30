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
# <a name="miscellaneous-apis"></a>Различные интерфейсы API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Типы, реализующие любой из следующих интерфейсов должны быть потокобезопасным для нескольких клиентов.

## <a name="isecret"></a>ISecret

`ISecret` Интерфейс представляет секретное значение, например материалом ключа шифрования. Он содержит следующие области API:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` Метод заполняет предоставленный буфер с необработанное значение секрета. Причина буфера в качестве параметра принимает этот API не возвращается `byte[]` непосредственно является, это дает вызывающий возможность закрепления объекта буфера, ограничение возможности раскрытия секрета управляемого сборщика мусора.

`Secret` Тип — это конкретная реализация `ISecret` где секретное значение хранится в памяти в процессе. На платформах Windows секретное значение шифруется через [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
