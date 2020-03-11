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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Прочие ASP.NET Core API-интерфейсы защиты данных

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Типы, реализующие любые из следующих интерфейсов должны быть потокобезопасными для нескольких клиентов.

## <a name="isecret"></a>исекрет

Интерфейс `ISecret` представляет секретное значение, например материал криптографического ключа. Он содержит следующую поверхность API:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

Метод `WriteSecretIntoBuffer` заполняет указанный буфер необработанным значением секрета. Причина, по которой этот API принимает буфер в качестве параметра, вместо того чтобы возвращать `byte[]` напрямую, это дает вызывающей стороне возможность закрепить объект буфера, ограничивая секретный код управляемому сборщику мусора.

Тип `Secret` является конкретной реализацией `ISecret`, где секретное значение хранится в памяти внутри процесса. На платформах Windows значение секрета шифруется через [криптпротектмемори](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
