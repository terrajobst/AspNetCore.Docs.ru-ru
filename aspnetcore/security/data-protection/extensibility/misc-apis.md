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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Прочие ASP.NET Core интерфейсы API защиты данных

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
