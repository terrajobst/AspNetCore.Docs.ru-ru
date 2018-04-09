---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Проверка с помощью на уровне службы (C#) | Документы Microsoft
author: StephenWalther
description: Узнайте, как переместить логику проверки из действия контроллера и в слой отдельной службе. В этом учебнике объясняется Стивен Вальтер как вы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 06042ac197cc54da767a94a44c57eb09bb3db9fa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="b2d59-104">Проверка с помощью на уровне службы (C#)</span><span class="sxs-lookup"><span data-stu-id="b2d59-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="b2d59-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b2d59-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b2d59-106">Узнайте, как переместить логику проверки из действия контроллера и в слой отдельной службе.</span><span class="sxs-lookup"><span data-stu-id="b2d59-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="b2d59-107">В этом учебнике Стивен Вальтер объясняется, как обеспечить острым Разделение областей ответственности изоляция на уровне службы из вашего уровня контроллера.</span><span class="sxs-lookup"><span data-stu-id="b2d59-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="b2d59-108">Целью данного учебника является описание один из способов проверки в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b2d59-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="b2d59-109">В этом учебнике вы узнаете, как переместить логику проверки из контроллеров и отправляются на уровне отдельных службы.</span><span class="sxs-lookup"><span data-stu-id="b2d59-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="b2d59-110">Отделение проблемы</span><span class="sxs-lookup"><span data-stu-id="b2d59-110">Separating Concerns</span></span>

<span data-ttu-id="b2d59-111">При создании приложения ASP.NET MVC, логику базы данных не следует помещать внутри действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="b2d59-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="b2d59-112">Смешивание логику базы данных и контроллера усложняет приложение для поддержания со временем.</span><span class="sxs-lookup"><span data-stu-id="b2d59-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="b2d59-113">Рекомендуется поместить все логики базы данных на уровне отдельных репозитория.</span><span class="sxs-lookup"><span data-stu-id="b2d59-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="b2d59-114">Например список 1 содержит простой репозиторий с именем ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="b2d59-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="b2d59-115">Репозиторий продукта содержит все код доступа к данным для приложения.</span><span class="sxs-lookup"><span data-stu-id="b2d59-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="b2d59-116">Список также включает интерфейс IProductRepository, который реализует хранилище продукта.</span><span class="sxs-lookup"><span data-stu-id="b2d59-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="b2d59-117">**1 — Models\ProductRepository.cs со списком**</span><span class="sxs-lookup"><span data-stu-id="b2d59-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="b2d59-118">Контроллер в списке 2 использует уровня репозитория Index() и Create() действий.</span><span class="sxs-lookup"><span data-stu-id="b2d59-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="b2d59-119">Обратите внимание, что данный контроллер не содержит логику базы данных.</span><span class="sxs-lookup"><span data-stu-id="b2d59-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="b2d59-120">Создание уровня хранилища позволяет поддерживать четкое разделение областей ответственности.</span><span class="sxs-lookup"><span data-stu-id="b2d59-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="b2d59-121">Контроллеры отвечают за логику управления потоком приложения и репозитория отвечает за логики доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="b2d59-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="b2d59-122">**Листинг 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="b2d59-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="b2d59-123">Создание уровня службы</span><span class="sxs-lookup"><span data-stu-id="b2d59-123">Creating a Service Layer</span></span>

<span data-ttu-id="b2d59-124">Таким образом приложения потока управления располагается в контроллере и принадлежит логики доступа к данным в репозитории.</span><span class="sxs-lookup"><span data-stu-id="b2d59-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="b2d59-125">В этом случае, где размещен свою логику проверки?</span><span class="sxs-lookup"><span data-stu-id="b2d59-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="b2d59-126">Первый вариант — разместить логику проверки в *уровень службы*.</span><span class="sxs-lookup"><span data-stu-id="b2d59-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="b2d59-127">На уровне службы представляет собой дополнительный уровень в приложении ASP.NET MVC, который является посредником связи между контроллером и уровня репозитория.</span><span class="sxs-lookup"><span data-stu-id="b2d59-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="b2d59-128">Уровень службы содержит бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="b2d59-128">The service layer contains business logic.</span></span> <span data-ttu-id="b2d59-129">В частности он содержит логику проверки.</span><span class="sxs-lookup"><span data-stu-id="b2d59-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="b2d59-130">Например уровень службы продукта в списке 3 имеет метод CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="b2d59-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="b2d59-131">Метод CreateProduct() вызывает метод ValidateProduct(), чтобы проверить новый продукт перед передачей продукта в репозитории продукта.</span><span class="sxs-lookup"><span data-stu-id="b2d59-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="b2d59-132">**Листинг 3 - Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="b2d59-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="b2d59-133">Контроллер продукта был обновлен в листинге 4 для использования вместо уровня репозитория уровня службы.</span><span class="sxs-lookup"><span data-stu-id="b2d59-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="b2d59-134">Обсуждения слоя контроллера до уровня службы.</span><span class="sxs-lookup"><span data-stu-id="b2d59-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="b2d59-135">Уровень служб обращается к уровня репозитория.</span><span class="sxs-lookup"><span data-stu-id="b2d59-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="b2d59-136">Каждый уровень имеет отдельный ответственности.</span><span class="sxs-lookup"><span data-stu-id="b2d59-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="b2d59-137">**Листинг 4 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="b2d59-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="b2d59-138">Обратите внимание, что службы продукта создается в конструктор контроллера продукта.</span><span class="sxs-lookup"><span data-stu-id="b2d59-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="b2d59-139">При создании службы продукта в словарь состояния модели передается в службу.</span><span class="sxs-lookup"><span data-stu-id="b2d59-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="b2d59-140">Обновления продукта состояние модели используется для передачи сообщений об ошибках проверки на контроллер.</span><span class="sxs-lookup"><span data-stu-id="b2d59-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="b2d59-141">Отделение уровень службы</span><span class="sxs-lookup"><span data-stu-id="b2d59-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="b2d59-142">Изолировать контроллера и слои службы в одном — произошел сбой.</span><span class="sxs-lookup"><span data-stu-id="b2d59-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="b2d59-143">Контроллер и слои службы взаимодействуют через состояние модели.</span><span class="sxs-lookup"><span data-stu-id="b2d59-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="b2d59-144">Другими словами уровень службы имеет зависимость от определенной функции платформы ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b2d59-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="b2d59-145">Мы хотим изолировать уровень службы из наших максимально уровня контроллера.</span><span class="sxs-lookup"><span data-stu-id="b2d59-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="b2d59-146">В теории мы должны иметь возможность использовать уровень службы с любым типом приложения, а не только приложение ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b2d59-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="b2d59-147">Например в дальнейшем может потребоваться для построения переднего плана для нашего приложения WPF.</span><span class="sxs-lookup"><span data-stu-id="b2d59-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="b2d59-148">Мы должны найти состояние модели способ удалить зависимость от ASP.NET MVC из наших уровня службы.</span><span class="sxs-lookup"><span data-stu-id="b2d59-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="b2d59-149">В листинге 5 уровня службы, была обновлена, чтобы он больше не использует состояние модели.</span><span class="sxs-lookup"><span data-stu-id="b2d59-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="b2d59-150">Вместо этого он использует любой класс, реализующий интерфейс IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="b2d59-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="b2d59-151">**Список 5 - Models\ProductService.cs (связано)**</span><span class="sxs-lookup"><span data-stu-id="b2d59-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="b2d59-152">Интерфейс IValidationDictionary определен в 6 со списком.</span><span class="sxs-lookup"><span data-stu-id="b2d59-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="b2d59-153">Этот простой интерфейс имеет один метод и одного свойства.</span><span class="sxs-lookup"><span data-stu-id="b2d59-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="b2d59-154">**Перечисление 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="b2d59-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="b2d59-155">Класс в список 7, с именем класса ModelStateWrapper, реализующий интерфейс IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="b2d59-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="b2d59-156">Можно создать экземпляр класса ModelStateWrapper, передав конструктору словарь состояния модели.</span><span class="sxs-lookup"><span data-stu-id="b2d59-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="b2d59-157">**Листинг 7 - Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="b2d59-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="b2d59-158">Наконец обновленный контроллер в листинг 8 использует ModelStateWrapper при создании уровня службы в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="b2d59-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="b2d59-159">**8 - Controllers\ProductController.cs со списком**</span><span class="sxs-lookup"><span data-stu-id="b2d59-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="b2d59-160">С помощью IValidationDictionary интерфейс и класс ModelStateWrapper позволяет полностью изолировать уровень нашей службы из наших уровня контроллера.</span><span class="sxs-lookup"><span data-stu-id="b2d59-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="b2d59-161">Уровень службы больше не зависит от состояния модели.</span><span class="sxs-lookup"><span data-stu-id="b2d59-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="b2d59-162">Можно передать любой класс, реализующий интерфейс IValidationDictionary до уровня службы.</span><span class="sxs-lookup"><span data-stu-id="b2d59-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="b2d59-163">Например приложение WPF может реализовывать интерфейс IValidationDictionary с класс простой коллекции.</span><span class="sxs-lookup"><span data-stu-id="b2d59-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="b2d59-164">Сводка</span><span class="sxs-lookup"><span data-stu-id="b2d59-164">Summary</span></span>

<span data-ttu-id="b2d59-165">Целью данного учебника было обсудить один из подходов к проверке в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b2d59-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="b2d59-166">В этом учебнике вы узнали, как перемещение всех логики проверки из контроллеров и отправляются на уровне отдельных службы.</span><span class="sxs-lookup"><span data-stu-id="b2d59-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="b2d59-167">Вы также узнали, как изоляция на уровне службы из вашего уровня контроллера, создав класс ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="b2d59-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2d59-168">[Назад](validating-with-the-idataerrorinfo-interface-cs.md)
> [Вперед](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b2d59-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
