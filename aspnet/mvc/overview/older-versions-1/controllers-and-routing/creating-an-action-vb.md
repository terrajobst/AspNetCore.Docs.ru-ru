---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Создание действия (Visual Basic) | Документация Майкрософт
author: microsoft
description: Узнайте, как добавить новое действие контроллера ASP.NET MVC. Дополнительные сведения о требованиях для метода для действия.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 6ec69f5aa9f7a789cb533a8dc04dca952adf35d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824050"
---
<a name="creating-an-action-vb"></a><span data-ttu-id="c061e-104">Создание действия (VB)</span><span class="sxs-lookup"><span data-stu-id="c061e-104">Creating an Action (VB)</span></span>
====================
<span data-ttu-id="c061e-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c061e-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c061e-106">Узнайте, как добавить новое действие контроллера ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c061e-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="c061e-107">Дополнительные сведения о требованиях для метода для действия.</span><span class="sxs-lookup"><span data-stu-id="c061e-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="c061e-108">Чтобы объяснить, как можно создать новое действие контроллера является целью данного учебника.</span><span class="sxs-lookup"><span data-stu-id="c061e-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="c061e-109">Дополнительные сведения о требованиях к методу действия.</span><span class="sxs-lookup"><span data-stu-id="c061e-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="c061e-110">Вы также узнаете, как предотвратить раскрытие как действие метода.</span><span class="sxs-lookup"><span data-stu-id="c061e-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="c061e-111">Добавление действия к контроллеру</span><span class="sxs-lookup"><span data-stu-id="c061e-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="c061e-112">Добавьте новое действие к контроллеру, добавив новый метод к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="c061e-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="c061e-113">Например контроллер в листинге 1 содержит действия с именем Index() и действие с именем SayHello().</span><span class="sxs-lookup"><span data-stu-id="c061e-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="c061e-114">Оба метода доступны как действия.</span><span class="sxs-lookup"><span data-stu-id="c061e-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="c061e-115">**В листинге 1 - Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="c061e-115">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="c061e-116">Чтобы предоставить вселенной как действие, метод должны соответствовать определенным требованиям:</span><span class="sxs-lookup"><span data-stu-id="c061e-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="c061e-117">Метод должен быть открытым.</span><span class="sxs-lookup"><span data-stu-id="c061e-117">The method must be public.</span></span>
- <span data-ttu-id="c061e-118">Метод не может быть статический метод.</span><span class="sxs-lookup"><span data-stu-id="c061e-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="c061e-119">Метод не может быть методом расширения.</span><span class="sxs-lookup"><span data-stu-id="c061e-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="c061e-120">Метод не может быть конструктор, метод получения или задания.</span><span class="sxs-lookup"><span data-stu-id="c061e-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="c061e-121">Метод не может иметь открытые универсальные типы.</span><span class="sxs-lookup"><span data-stu-id="c061e-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="c061e-122">Метод не является методом базового класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="c061e-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="c061e-123">Этот метод не может содержать **ref** или **out** параметров.</span><span class="sxs-lookup"><span data-stu-id="c061e-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="c061e-124">Обратите внимание на то, что не существует ограничений на тип возвращаемого значения действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="c061e-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="c061e-125">Действие контроллера может вернуть строку, DateTime, экземпляр класса Random, или void.</span><span class="sxs-lookup"><span data-stu-id="c061e-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="c061e-126">Платформа ASP.NET MVC преобразует любой возвращаемый тип, который не является результат действия в строку и просмотру строки в браузере.</span><span class="sxs-lookup"><span data-stu-id="c061e-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="c061e-127">При добавлении любого метода, который не нарушает эти требования к контроллеру, метод указывается в виде действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="c061e-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="c061e-128">Следите за здесь.</span><span class="sxs-lookup"><span data-stu-id="c061e-128">Be careful here.</span></span> <span data-ttu-id="c061e-129">Действие контроллера может вызываться кем-либо подключение к Интернету.</span><span class="sxs-lookup"><span data-stu-id="c061e-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="c061e-130">Не так, например, создать действие контроллера DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="c061e-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="c061e-131">Предотвращение вызов открытого метода</span><span class="sxs-lookup"><span data-stu-id="c061e-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="c061e-132">Если вам нужно создать открытый метод в класс контроллера и вы не хотите предоставлять метод как действие контроллера, то метод можно предотвратить вызов с помощью &lt;NonAction&gt; атрибута.</span><span class="sxs-lookup"><span data-stu-id="c061e-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="c061e-133">Например, контроллер в листинге 2 содержит открытый метод с именем CompanySecrets(), отмеченный атрибутом &lt;NonAction&gt; атрибута.</span><span class="sxs-lookup"><span data-stu-id="c061e-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

<span data-ttu-id="c061e-134">**В листинге 2 - Controllers\WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="c061e-134">**Listing 2 - Controllers\WorkController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="c061e-135">При попытке вызвать действие контроллера CompanySecrets(), введя в адресной строке браузера /Work/CompanySecrets затем вы получите сообщение об ошибке на рис. 1.</span><span class="sxs-lookup"><span data-stu-id="c061e-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="c061e-136">[![Вызов метода NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c061e-136">[![Invoking a NonAction method](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span></span>

<span data-ttu-id="c061e-137">**Рис 01**: вызов метода NonAction ([Просмотр полноразмерного изображения](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c061e-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c061e-138">[Назад](creating-a-controller-vb.md)
> [Вперед](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c061e-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>
