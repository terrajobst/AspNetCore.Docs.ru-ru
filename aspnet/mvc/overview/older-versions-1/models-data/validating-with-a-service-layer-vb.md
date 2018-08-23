---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Проверка с помощью уровня службы (Visual Basic) | Документация Майкрософт
author: StephenWalther
description: Узнайте, как переместить логику проверки из действий контроллера и поместить в отдельный слой. В этом руководстве объясняется, Стивен Вальтер как вы...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: ecce8e4f0a901ce8c185d2b085f4d706bd57fa1f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836617"
---
<a name="validating-with-a-service-layer-vb"></a><span data-ttu-id="c8006-104">Проверка с помощью уровня службы (VB)</span><span class="sxs-lookup"><span data-stu-id="c8006-104">Validating with a Service Layer (VB)</span></span>
====================
<span data-ttu-id="c8006-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="c8006-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="c8006-106">Узнайте, как переместить логику проверки из действий контроллера и поместить в отдельный слой.</span><span class="sxs-lookup"><span data-stu-id="c8006-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="c8006-107">В этом руководстве Стивен Вальтер объясняется, как можно поддерживать резкие Разделение областей ответственности, изолируя уровень службы из вашего уровня контроллера.</span><span class="sxs-lookup"><span data-stu-id="c8006-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="c8006-108">Целью данного учебника является описание один из способов выполнения проверки в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c8006-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="c8006-109">В этом руководстве вы узнаете, как переместить логику проверки из контроллеров и поместить в отдельный слой.</span><span class="sxs-lookup"><span data-stu-id="c8006-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="c8006-110">Разделение задач</span><span class="sxs-lookup"><span data-stu-id="c8006-110">Separating Concerns</span></span>

<span data-ttu-id="c8006-111">При создании приложения ASP.NET MVC, логику базы данных не следует помещать внутри действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="c8006-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="c8006-112">Смешивание логику базы данных и контроллера делает более сложным в обслуживании со временем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="c8006-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="c8006-113">Рекомендуется поместить все логику базы данных в отдельном репозитории слоя.</span><span class="sxs-lookup"><span data-stu-id="c8006-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="c8006-114">Например в листинге 1 содержит простой репозиторий с именем ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="c8006-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="c8006-115">Репозиторий продукта содержит все код доступа к данным для приложения.</span><span class="sxs-lookup"><span data-stu-id="c8006-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="c8006-116">Список также включает интерфейс IProductRepository, который реализует хранилище продукта.</span><span class="sxs-lookup"><span data-stu-id="c8006-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="c8006-117">**В листинге 1 - Models\ProductRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="c8006-117">**Listing 1 - Models\ProductRepository.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

<span data-ttu-id="c8006-118">Контроллер в листинге 2 использует уровня репозитория в Index() и Create() действия.</span><span class="sxs-lookup"><span data-stu-id="c8006-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="c8006-119">Обратите внимание на то, что этот контроллер не содержит логику базы данных.</span><span class="sxs-lookup"><span data-stu-id="c8006-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="c8006-120">Создание уровня хранилища позволяет поддерживать четкое разделение ответственности.</span><span class="sxs-lookup"><span data-stu-id="c8006-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="c8006-121">Контроллеры отвечают за логику управления потоком приложения и хранилище отвечает за логику доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="c8006-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="c8006-122">**В листинге 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="c8006-122">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="c8006-123">Создание уровня службы</span><span class="sxs-lookup"><span data-stu-id="c8006-123">Creating a Service Layer</span></span>

<span data-ttu-id="c8006-124">Таким образом, логику управления потоком приложения входят в контроллере, а логика доступа к данным входят в репозитории.</span><span class="sxs-lookup"><span data-stu-id="c8006-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="c8006-125">В этом случае, где разместить логику проверки?</span><span class="sxs-lookup"><span data-stu-id="c8006-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="c8006-126">Один из вариантов — разместить логику проверки в *слой служб*.</span><span class="sxs-lookup"><span data-stu-id="c8006-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="c8006-127">Слой служб представляет собой дополнительный уровень в приложении ASP.NET MVC, которое включается в процесс обмена данными между контроллером и уровня репозитория.</span><span class="sxs-lookup"><span data-stu-id="c8006-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="c8006-128">Слой служб содержит бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="c8006-128">The service layer contains business logic.</span></span> <span data-ttu-id="c8006-129">В частности он содержит логику проверки.</span><span class="sxs-lookup"><span data-stu-id="c8006-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="c8006-130">Например слой служб продукта в листинге 3 имеет метод CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="c8006-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="c8006-131">Метод CreateProduct() вызывает метод ValidateProduct() для проверки новый продукт перед их передачей продукта к репозиторию продукта.</span><span class="sxs-lookup"><span data-stu-id="c8006-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="c8006-132">**Листинг 3 - Models\ProductService.vb**</span><span class="sxs-lookup"><span data-stu-id="c8006-132">**Listing 3 - Models\ProductService.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

<span data-ttu-id="c8006-133">Контроллер продукта была обновлена в листинге 4, чтобы использовать уровень служб вместо уровня репозитория.</span><span class="sxs-lookup"><span data-stu-id="c8006-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="c8006-134">На уровне контроллер взаимодействует с уровня службы.</span><span class="sxs-lookup"><span data-stu-id="c8006-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="c8006-135">Слой служб взаимодействует с уровня репозитория.</span><span class="sxs-lookup"><span data-stu-id="c8006-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="c8006-136">Каждый слой имеет отдельный ответственности.</span><span class="sxs-lookup"><span data-stu-id="c8006-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="c8006-137">**Листинг 4 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="c8006-137">**Listing 4 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

<span data-ttu-id="c8006-138">Обратите внимание, что службы продукции создается в конструкторе контроллера продукта.</span><span class="sxs-lookup"><span data-stu-id="c8006-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="c8006-139">При создании службы продукции в словарь состояния модели передается в службу.</span><span class="sxs-lookup"><span data-stu-id="c8006-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="c8006-140">Служба продукта использует состояние модели для передачи сообщения об ошибках проверки обратно к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="c8006-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="c8006-141">Отделение уровня службы</span><span class="sxs-lookup"><span data-stu-id="c8006-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="c8006-142">Мы не изолировать контроллера и слои обслуживания в одном —.</span><span class="sxs-lookup"><span data-stu-id="c8006-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="c8006-143">На контроллере и уровни службы взаимодействуют через состояние модели.</span><span class="sxs-lookup"><span data-stu-id="c8006-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="c8006-144">Другими словами уровень служб имеет зависимость от определенная функция платформы ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c8006-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="c8006-145">Мы хотим изолировать слой служб от наших максимально слоя контроллера.</span><span class="sxs-lookup"><span data-stu-id="c8006-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="c8006-146">В теории мы должны иметь возможность использовать уровень служб с любым типом приложения, а не только приложение ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c8006-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="c8006-147">Например в будущем, мы может потребоваться создание WPF, переднего плана для нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="c8006-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="c8006-148">Мы должны найти состояние модели способ удалить зависимость от ASP.NET MVC от наших уровня службы.</span><span class="sxs-lookup"><span data-stu-id="c8006-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="c8006-149">В листинге 5 слой служб был обновлен, чтобы он больше не использует состояние модели.</span><span class="sxs-lookup"><span data-stu-id="c8006-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="c8006-150">Вместо этого он использует любой класс, реализующий интерфейс IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="c8006-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="c8006-151">**В листинге 5 - Models\ProductService.vb (в несвязанном состоянии)**</span><span class="sxs-lookup"><span data-stu-id="c8006-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

<span data-ttu-id="c8006-152">Интерфейс IValidationDictionary определен в листинге 6.</span><span class="sxs-lookup"><span data-stu-id="c8006-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="c8006-153">Этот простой интерфейс содержит один метод и одно свойство.</span><span class="sxs-lookup"><span data-stu-id="c8006-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="c8006-154">**В листинге 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="c8006-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

<span data-ttu-id="c8006-155">В листинге 7, с именем класса ModelStateWrapper, реализует интерфейс IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="c8006-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="c8006-156">Можно создать экземпляр класса ModelStateWrapper, передав словарь состояния модели в конструктор.</span><span class="sxs-lookup"><span data-stu-id="c8006-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="c8006-157">**Листинг 7 - Models\ModelStateWrapper.vb**</span><span class="sxs-lookup"><span data-stu-id="c8006-157">**Listing 7 - Models\ModelStateWrapper.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

<span data-ttu-id="c8006-158">Наконец обновленного контроллера в листинг 8 использует ModelStateWrapper при создании слой служб в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="c8006-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="c8006-159">**Листинг 8 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="c8006-159">**Listing 8 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

<span data-ttu-id="c8006-160">С помощью IValidationDictionary интерфейс и класс ModelStateWrapper позволяет нам полностью изолировать наш слой служб от наш слой контроллера.</span><span class="sxs-lookup"><span data-stu-id="c8006-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="c8006-161">Слой служб больше не зависит от состояния модели.</span><span class="sxs-lookup"><span data-stu-id="c8006-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="c8006-162">Можно передать любой класс, реализующий интерфейс IValidationDictionary уровень службы.</span><span class="sxs-lookup"><span data-stu-id="c8006-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="c8006-163">Например приложения WPF может реализовывать интерфейс IValidationDictionary с простой класс коллекций.</span><span class="sxs-lookup"><span data-stu-id="c8006-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="c8006-164">Сводка</span><span class="sxs-lookup"><span data-stu-id="c8006-164">Summary</span></span>

<span data-ttu-id="c8006-165">Цель этого учебника было обсудить одним из подходов к проверке в ASP.NET MVC-приложениях.</span><span class="sxs-lookup"><span data-stu-id="c8006-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="c8006-166">В этом руководстве вы узнали, как перемещать все обнаруженные логику проверки из контроллеров и поместить в отдельный слой.</span><span class="sxs-lookup"><span data-stu-id="c8006-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="c8006-167">Вы также узнали, как выделять на уровне службы, от вашего уровня контроллера, создав класс ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="c8006-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c8006-168">[Назад](validating-with-the-idataerrorinfo-interface-vb.md)
> [Вперед](validation-with-the-data-annotation-validators-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c8006-168">[Previous](validating-with-the-idataerrorinfo-interface-vb.md)
[Next](validation-with-the-data-annotation-validators-vb.md)</span></span>
