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
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a><span data-ttu-id="468cd-103">Повторное использование объекта с ObjectPool в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="468cd-103">Object reuse with ObjectPool in ASP.NET Core</span></span>

<span data-ttu-id="468cd-104">По [Гордон Стив](https://twitter.com/stevejgordon), [Райан Новак](https://github.com/rynowak), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="468cd-104">By [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="468cd-105"><xref:Microsoft.Extensions.ObjectPool> является частью инфраструктуры ASP.NET Core, который поддерживает сохранение группы объектов в памяти для повторного использования, а не позволяя сборщику мусора объекты собираются.</span><span class="sxs-lookup"><span data-stu-id="468cd-105"><xref:Microsoft.Extensions.ObjectPool> is part of the ASP.NET Core infrastructure that supports keeping a group of objects in memory for reuse rather than allowing the objects to be garbage collected.</span></span>

<span data-ttu-id="468cd-106">Может потребоваться использовать пул объектов, если объекты, которые управляются:</span><span class="sxs-lookup"><span data-stu-id="468cd-106">You might want to use the object pool if the objects that are being managed are:</span></span>

- <span data-ttu-id="468cd-107">Затраты на выделение или инициализировать.</span><span class="sxs-lookup"><span data-stu-id="468cd-107">Expensive to allocate/initialize.</span></span>
- <span data-ttu-id="468cd-108">Представляют некоторые ограниченным ресурсом.</span><span class="sxs-lookup"><span data-stu-id="468cd-108">Represent some limited resource.</span></span>
- <span data-ttu-id="468cd-109">Использовать предсказуемо и часто.</span><span class="sxs-lookup"><span data-stu-id="468cd-109">Used predictably and frequently.</span></span>

<span data-ttu-id="468cd-110">Например, платформа ASP.NET Core использует пул объектов в некоторых местах для повторного использования <xref:System.Text.StringBuilder> экземпляров.</span><span class="sxs-lookup"><span data-stu-id="468cd-110">For example, the ASP.NET Core framework uses the object pool in some places to reuse <xref:System.Text.StringBuilder> instances.</span></span> <span data-ttu-id="468cd-111">`StringBuilder` Выделяет и управляет свой собственный буфером для хранения символьных данных.</span><span class="sxs-lookup"><span data-stu-id="468cd-111">`StringBuilder` allocates and manages its own buffers to hold character data.</span></span> <span data-ttu-id="468cd-112">ASP.NET Core, регулярно использующей `StringBuilder` для реализации функции, и их повторного использования обеспечит выигрыш в производительности.</span><span class="sxs-lookup"><span data-stu-id="468cd-112">ASP.NET Core regularly uses `StringBuilder` to implement features, and reusing them provides a performance benefit.</span></span>

<span data-ttu-id="468cd-113">Использование пулов объектов не всегда улучшает производительность:</span><span class="sxs-lookup"><span data-stu-id="468cd-113">Object pooling doesn't always improve performance:</span></span>

- <span data-ttu-id="468cd-114">Если затраты на инициализацию объекта высокого уровня, обычно занимает для получения объекта из пула.</span><span class="sxs-lookup"><span data-stu-id="468cd-114">Unless the initialization cost of an object is high, it's usually slower to get the object from the pool.</span></span>
- <span data-ttu-id="468cd-115">Объектов, управляемых пулом не отменить выделение, пока не отменяется пула.</span><span class="sxs-lookup"><span data-stu-id="468cd-115">Objects managed by the pool aren't de-allocated until the pool is de-allocated.</span></span>

<span data-ttu-id="468cd-116">Использование пулов объектов только после сбора данных о производительности с помощью реалистичных ситуациях для приложения или библиотеки.</span><span class="sxs-lookup"><span data-stu-id="468cd-116">Use object pooling only after collecting performance data using realistic scenarios for your app or library.</span></span>

<span data-ttu-id="468cd-117">**ПРЕДУПРЕЖДЕНИЕ: `ObjectPool` Не реализует `IDisposable`. Не рекомендуется использовать его с типами, которые должны реализации.**</span><span class="sxs-lookup"><span data-stu-id="468cd-117">**WARNING: The `ObjectPool` doesn't implement `IDisposable`. We don't recommend using it with types that need disposal.**</span></span>

<span data-ttu-id="468cd-118">**ПРИМЕЧАНИЕ. ObjectPool не налагает ограничения на число объектов, которые он будет выделять, он устанавливает ограничение на число объектов, которые он будет сохранять.**</span><span class="sxs-lookup"><span data-stu-id="468cd-118">**NOTE: The ObjectPool doesn't place a limit on the number of objects that it will allocate, it places a limit on the number of objects it will retain.**</span></span>

## <a name="concepts"></a><span data-ttu-id="468cd-119">Основные понятия</span><span class="sxs-lookup"><span data-stu-id="468cd-119">Concepts</span></span>

<span data-ttu-id="468cd-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -абстракции пула базовым объектом.</span><span class="sxs-lookup"><span data-stu-id="468cd-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> - the basic object pool abstraction.</span></span> <span data-ttu-id="468cd-121">Используется для получения и возврата объектов.</span><span class="sxs-lookup"><span data-stu-id="468cd-121">Used to get and return objects.</span></span>

<span data-ttu-id="468cd-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> — Это настройка, способ создания объекта и способ их реализации *Сброс* при возвращении в пул.</span><span class="sxs-lookup"><span data-stu-id="468cd-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> - implement this to customize how an object is created and how it is *reset* when returned to the pool.</span></span> <span data-ttu-id="468cd-123">Это может быть передан в пула объектов, можно создать напрямую... OR</span><span class="sxs-lookup"><span data-stu-id="468cd-123">This can be passed into an object pool that you construct directly.... OR</span></span>

<span data-ttu-id="468cd-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> выступает в качестве фабрики для создания пулов объектов.</span><span class="sxs-lookup"><span data-stu-id="468cd-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> acts as a factory for creating object pools.</span></span>
<!-- REview, there is no ObjectPoolProvider<T> -->

<span data-ttu-id="468cd-125">ObjectPool может использоваться в приложении несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="468cd-125">The ObjectPool can be used in an app in multiple ways:</span></span>

* <span data-ttu-id="468cd-126">Создание пула.</span><span class="sxs-lookup"><span data-stu-id="468cd-126">Instantiating a pool.</span></span>
* <span data-ttu-id="468cd-127">Регистрация пула в [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI) в качестве экземпляра.</span><span class="sxs-lookup"><span data-stu-id="468cd-127">Registering a pool in [Dependency injection](xref:fundamentals/dependency-injection) (DI) as an instance.</span></span>
* <span data-ttu-id="468cd-128">Регистрация `ObjectPoolProvider<>` в DI и использовать его в качестве фабрики.</span><span class="sxs-lookup"><span data-stu-id="468cd-128">Registering the `ObjectPoolProvider<>` in DI and using it as a factory.</span></span>

## <a name="how-to-use-objectpool"></a><span data-ttu-id="468cd-129">Как использовать ObjectPool</span><span class="sxs-lookup"><span data-stu-id="468cd-129">How to use ObjectPool</span></span>

<span data-ttu-id="468cd-130">Вызовите <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> для получения объекта и <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> для возврата объекта.</span><span class="sxs-lookup"><span data-stu-id="468cd-130">Call <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> to get an object and <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> to return the object.</span></span>  <span data-ttu-id="468cd-131">Нет необходимости возвращать каждый объект.</span><span class="sxs-lookup"><span data-stu-id="468cd-131">There's no requirement that you return every object.</span></span> <span data-ttu-id="468cd-132">Если не возвращают объект, он будет удален сборщиком мусора.</span><span class="sxs-lookup"><span data-stu-id="468cd-132">If you don't return an object, it will be garbage collected.</span></span>

## <a name="objectpool-sample"></a><span data-ttu-id="468cd-133">Пример ObjectPool</span><span class="sxs-lookup"><span data-stu-id="468cd-133">ObjectPool sample</span></span>

<span data-ttu-id="468cd-134">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="468cd-134">The following code:</span></span>

* <span data-ttu-id="468cd-135">Добавляет `ObjectPoolProvider` для [внедрения зависимостей](xref:fundamentals/dependency-injection) контейнера (DI).</span><span class="sxs-lookup"><span data-stu-id="468cd-135">Adds `ObjectPoolProvider` to the [Dependency injection](xref:fundamentals/dependency-injection) (DI) container.</span></span>
* <span data-ttu-id="468cd-136">Добавление и настройка `ObjectPool<StringBuilder>` в контейнер внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="468cd-136">Adds and configures `ObjectPool<StringBuilder>` to the DI container.</span></span>
* <span data-ttu-id="468cd-137">Добавляет `BirthdayMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="468cd-137">Adds the `BirthdayMiddleware`.</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

<span data-ttu-id="468cd-138">Следующий код реализует `BirthdayMiddleware`</span><span class="sxs-lookup"><span data-stu-id="468cd-138">The following code implements `BirthdayMiddleware`</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
