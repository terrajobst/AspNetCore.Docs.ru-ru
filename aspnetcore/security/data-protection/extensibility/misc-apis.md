---
title: Прочие ASP.NET Core интерфейсы API защиты данных
author: rick-anderson
description: Дополнительные сведения об интерфейсе защиты данных ISecret для ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896621"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Прочие ASP.NET Core интерфейсы API защиты данных

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Типы, реализующие любые из следующих интерфейсов должны быть потокобезопасными для нескольких клиентов.

## <a name="isecret"></a>ISecret

`ISecret` Интерфейс представляет значение секрета, например материал криптографического ключа. Он содержит следующие поверхность API:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` Метод заполняет предоставленный буфер с необработанное значение секрета. Причина этого API принимает буфера в качестве параметра вместо возврата `byte[]` напрямую — это том, что это даёт вызывающий объект для закрепления объекта буфера, ограничивая секретный раскрытия в управляемых сборщика мусора.

`Secret` Тип — это конкретная реализация `ISecret` где секретное значение хранится в памяти в процессе. На платформах Windows, секретное значение шифруется с помощью [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
