---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Создание контроллера (VB) | Документация Майкрософт
author: StephenWalther
description: В этом руководстве Стивен Вальтер демонстрирует, как можно добавить контроллер в приложении ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c14ceff17d15b1a8460bad41429b82cb9504a24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829137"
---
<a name="creating-a-controller-vb"></a><span data-ttu-id="cc711-103">Создание контроллера (VB)</span><span class="sxs-lookup"><span data-stu-id="cc711-103">Creating a Controller (VB)</span></span>
====================
<span data-ttu-id="cc711-104">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="cc711-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="cc711-105">В этом руководстве Стивен Вальтер демонстрирует, как можно добавить контроллер в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cc711-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="cc711-106">Чтобы объяснить, как можно создать новый ASP.NET MVC контроллеры является целью данного учебника.</span><span class="sxs-lookup"><span data-stu-id="cc711-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="cc711-107">Вы научитесь создавать контроллеры, как с помощью команды меню добавления контроллера Visual Studio, так и вручную создав файл класса.</span><span class="sxs-lookup"><span data-stu-id="cc711-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="cc711-108">С помощью контроллера добавить команду меню</span><span class="sxs-lookup"><span data-stu-id="cc711-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="cc711-109">Самый простой способ создать новый контроллер — щелкните правой кнопкой мыши папку Controllers в окне обозревателя решений Visual Studio и выберите **Add, контроллера** пункт меню (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="cc711-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="cc711-110">Если выбрать этот пункт меню открывает **Добавление контроллера** диалоговое окно (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="cc711-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="cc711-111">[![В диалоговом окне нового проекта](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cc711-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="cc711-112">**Рис 01**: Добавление нового контроллера ([Просмотр полноразмерного изображения](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="cc711-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>


<span data-ttu-id="cc711-113">[![В диалоговом окне нового проекта](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="cc711-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="cc711-114">**Рис. 02**: диалоговое окно добавления контроллера ([Просмотр полноразмерного изображения](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="cc711-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>


<span data-ttu-id="cc711-115">Обратите внимание, что первая часть имени контроллера будет выделен в **Добавление контроллера** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="cc711-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="cc711-116">Имя каждого контроллера должно заканчиваться суффиксом *контроллера*.</span><span class="sxs-lookup"><span data-stu-id="cc711-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="cc711-117">Например, можно создать контроллер с именем *ProductController* , но не контроллер с именем *продукта*.</span><span class="sxs-lookup"><span data-stu-id="cc711-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="cc711-118">Если вы создаете контроллер, который отсутствует *контроллера* суффикса, то вы не сможете заставить контроллер.</span><span class="sxs-lookup"><span data-stu-id="cc711-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="cc711-119">Не сделать, — я потрачены впустую бесконечные часы мою жизнь после совершения этой ошибки.</span><span class="sxs-lookup"><span data-stu-id="cc711-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="cc711-120">**В листинге 1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="cc711-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="cc711-121">Всегда следует создавать контроллеры в папку "контроллеры".</span><span class="sxs-lookup"><span data-stu-id="cc711-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="cc711-122">В противном случае будет нарушения правил ASP.NET MVC и других разработчиков будет сложнее основные сведения о приложении.</span><span class="sxs-lookup"><span data-stu-id="cc711-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="cc711-123">Методы действия формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="cc711-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="cc711-124">При создании контроллера, у вас есть возможность создать методы действий создания, обновления и сведения автоматически (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="cc711-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="cc711-125">Если выбран этот параметр, создается класс контроллера в листинге 2.</span><span class="sxs-lookup"><span data-stu-id="cc711-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="cc711-126">[![Автоматическое создание методов действий](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="cc711-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="cc711-127">**Рис 03**: автоматическое создание методов действий ([Просмотр полноразмерного изображения](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="cc711-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>


<span data-ttu-id="cc711-128">**В листинге 2 - Controllers\CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="cc711-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="cc711-129">Эти созданные методы являются методы-заглушки.</span><span class="sxs-lookup"><span data-stu-id="cc711-129">These generated methods are stub methods.</span></span> <span data-ttu-id="cc711-130">Фактическая логика для создания, обновления и с подробными сведениями для клиента самостоятельно, необходимо добавить.</span><span class="sxs-lookup"><span data-stu-id="cc711-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="cc711-131">Но методы-заглушки дают хороший отправной точки.</span><span class="sxs-lookup"><span data-stu-id="cc711-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="cc711-132">Создание класса контроллера</span><span class="sxs-lookup"><span data-stu-id="cc711-132">Creating a Controller Class</span></span>

<span data-ttu-id="cc711-133">Контроллер ASP.NET MVC — это класс.</span><span class="sxs-lookup"><span data-stu-id="cc711-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="cc711-134">При желании можно игнорировать удобный формирования шаблонов контроллера Visual Studio и создать класс контроллера вручную.</span><span class="sxs-lookup"><span data-stu-id="cc711-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="cc711-135">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="cc711-135">Follow these steps:</span></span>

1. <span data-ttu-id="cc711-136">Щелкните правой кнопкой мыши папку Controllers и выберите пункт меню **добавить, новый элемент** и выберите **класс** шаблона (см. рис. 4).</span><span class="sxs-lookup"><span data-stu-id="cc711-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="cc711-137">Назовите новый класс PersonController.vb и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="cc711-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="cc711-138">Измените полученный файл, класс наследуется от базового класса System.Web.Mvc.Controller класса (см. Листинг 3).</span><span class="sxs-lookup"><span data-stu-id="cc711-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="cc711-139">[![Создание нового класса](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="cc711-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="cc711-140">**Рис. 04**: Создание нового класса ([Просмотр полноразмерного изображения](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="cc711-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>


<span data-ttu-id="cc711-141">**Листинг 3 - Controllers\PersonController.vb**</span><span class="sxs-lookup"><span data-stu-id="cc711-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="cc711-142">Контроллер в листинге 3 предоставляет одно действие с именем Index(), которое возвращает строку «Hello World!».</span><span class="sxs-lookup"><span data-stu-id="cc711-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="cc711-143">Вы можете вызвать это действие контроллера, для запуска приложения и запрос URL-адрес следующего вида:</span><span class="sxs-lookup"><span data-stu-id="cc711-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="cc711-144">ASP.NET Development Server использует случайный номер порта (например, 40071).</span><span class="sxs-lookup"><span data-stu-id="cc711-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="cc711-145">При вводе URL-адрес для вызова контроллера, необходимо указать номер порта справа.</span><span class="sxs-lookup"><span data-stu-id="cc711-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="cc711-146">Можно определить номер порта, наведя указатель мыши на значок для сервера разработки ASP.NET в области уведомлений Windows (правом верхнем углу экрана).</span><span class="sxs-lookup"><span data-stu-id="cc711-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="cc711-147">[Назад](adding-dynamic-content-to-a-cached-page-vb.md)
> [Вперед](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="cc711-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
