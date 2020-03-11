---
title: Повторное использование объектов с Обжектпул в ASP.NET Core
author: rick-anderson
description: Советы по повышению производительности ASP.NET Core приложений с помощью Обжектпул.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654382"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>Повторное использование объектов с Обжектпул в ASP.NET Core

[Стив Гордон](https://twitter.com/stevejgordon), [Райан Nowak)](https://github.com/rynowak)и [Рик Андерсон (](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> является частью инфраструктуры ASP.NET Core, которая поддерживает повторное использование группы объектов в памяти, а не позволяет собирать объекты в мусор.

Пул объектов может потребоваться, если управляемые объекты являются:

- Затратно на выделение и инициализацию.
- Представляет некоторый ограниченный ресурс.
- Используется в качестве прогнозируемых и часто используемых.

Например, ASP.NET Core Framework использует пул объектов в некоторых местах для повторного использования экземпляров <xref:System.Text.StringBuilder>. `StringBuilder` выделяет собственные буферы для хранения символьных данных и управляет ими. ASP.NET Core регулярно используют `StringBuilder` для реализации функций, и их повторное использование обеспечивает выигрыш в производительности.

Пул объектов не всегда повышает производительность:

- Если затраты на инициализацию объекта не высоки, обычно бывает медленнее получить объект из пула.
- Объекты, управляемые пулом, не выделяются, пока пул не будет освобожден.

Используйте пул объектов только после сбора данных о производительности с помощью реалистичных сценариев для приложения или библиотеки.

**Предупреждение: `ObjectPool` не реализует `IDisposable`. Мы не рекомендуем использовать его с типами, которые требуют реализации.**

**Примечание. Обжектпул не устанавливает ограничение на количество объектов, которые он будет выделять, он ограничивает количество объектов, которое он будет хранить.**

## <a name="concepts"></a>Основные понятия

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> — базовый уровень абстракции пула объектов. Используется для получения и возврата объектов.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> — реализуйте его, чтобы настроить способ создания объекта и его *сброса* при возврате в пул. Это можно передать в пул объектов, который создается напрямую... НИ

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> выступает в качестве фабрики для создания пулов объектов.
<!-- REview, there is no ObjectPoolProvider<T> -->

Обжектпул можно использовать в приложении несколькими способами:

* Создание экземпляра пула.
* Регистрация пула в [внедрении зависимостей](xref:fundamentals/dependency-injection) (DI) в качестве экземпляра.
* Регистрация `ObjectPoolProvider<>` в процессе внедрения и использование в качестве фабрики.

## <a name="how-to-use-objectpool"></a>Как использовать Обжектпул

Вызовите <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1>, чтобы получить объект и <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> для возврата объекта.  Нет необходимости возвращать каждый объект. Если не вернуть объект, он будет удален сборщиком мусора.

## <a name="objectpool-sample"></a>Пример Обжектпул

Следующий код:

* Добавляет `ObjectPoolProvider` в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI).
* Добавляет и настраивает `ObjectPool<StringBuilder>` в контейнер DI.
* Добавляет `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Следующий код реализует `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
