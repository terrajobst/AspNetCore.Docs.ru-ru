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
