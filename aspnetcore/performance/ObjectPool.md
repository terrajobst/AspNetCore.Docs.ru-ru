---
title: Повторное использование объекта с ObjectPool в ASP.NET Core
author: rick-anderson
description: Советы по улучшению производительности в приложениях ASP.NET Core с помощью ObjectPool.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815522"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>Повторное использование объекта с ObjectPool в ASP.NET Core

По [Гордон Стив](https://twitter.com/stevejgordon), [Райан Новак](https://github.com/rynowak), и [Рик Андерсон](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> является частью инфраструктуры ASP.NET Core, который поддерживает сохранение группы объектов в памяти для повторного использования, а не позволяя сборщику мусора объекты собираются.

Может потребоваться использовать пул объектов, если объекты, которые управляются:

- Затраты на выделение или инициализировать.
- Представляют некоторые ограниченным ресурсом.
- Использовать предсказуемо и часто.

Например, платформа ASP.NET Core использует пул объектов в некоторых местах для повторного использования <xref:System.Text.StringBuilder> экземпляров. `StringBuilder` Выделяет и управляет свой собственный буфером для хранения символьных данных. ASP.NET Core, регулярно использующей `StringBuilder` для реализации функции, и их повторного использования обеспечит выигрыш в производительности.

Использование пулов объектов не всегда улучшает производительность:

- Если затраты на инициализацию объекта высокого уровня, обычно занимает для получения объекта из пула.
- Объектов, управляемых пулом не отменить выделение, пока не отменяется пула.

Использование пулов объектов только после сбора данных о производительности с помощью реалистичных ситуациях для приложения или библиотеки.

**ПРЕДУПРЕЖДЕНИЕ: `ObjectPool` Не реализует `IDisposable`. Не рекомендуется использовать его с типами, которые должны реализации.**

**ПРИМЕЧАНИЕ. ObjectPool не налагает ограничения на число объектов, которые он будет выделять, он устанавливает ограничение на число объектов, которые он будет сохранять.**

## <a name="concepts"></a>Основные понятия

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -абстракции пула базовым объектом. Используется для получения и возврата объектов.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> — Это настройка, способ создания объекта и способ их реализации *Сброс* при возвращении в пул. Это может быть передан в пула объектов, можно создать напрямую... OR

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> выступает в качестве фабрики для создания пулов объектов.
<!-- REview, there is no ObjectPoolProvider<T> -->

ObjectPool может использоваться в приложении несколькими способами:

* Создание пула.
* Регистрация пула в [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI) в качестве экземпляра.
* Регистрация `ObjectPoolProvider<>` в DI и использовать его в качестве фабрики.

## <a name="how-to-use-objectpool"></a>Как использовать ObjectPool

Вызовите <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> для получения объекта и <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> для возврата объекта.  Нет необходимости возвращать каждый объект. Если не возвращают объект, он будет удален сборщиком мусора.

## <a name="objectpool-sample"></a>Пример ObjectPool

В приведенном ниже коде

* Добавляет `ObjectPoolProvider` для [внедрения зависимостей](xref:fundamentals/dependency-injection) контейнера (DI).
* Добавление и настройка `ObjectPool<StringBuilder>` в контейнер внедрения Зависимостей.
* Добавляет `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Следующий код реализует `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
