---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: "Создание контроллера (VB) | Документы Microsoft"
author: StephenWalther
description: "В этом учебнике Стивен Вальтер показано, как добавить контроллер в приложении ASP.NET MVC."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: d2caf7fe137b48c016ff3cd52db9e36e1e8001c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-controller-vb"></a><span data-ttu-id="ad801-103">Создание контроллера (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="ad801-103">Creating a Controller (VB)</span></span>
====================
<span data-ttu-id="ad801-104">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="ad801-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="ad801-105">В этом учебнике Стивен Вальтер показано, как добавить контроллер в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ad801-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="ad801-106">Целью данного учебника является объясняется, как можно создать новый ASP.NET MVC контроллеры.</span><span class="sxs-lookup"><span data-stu-id="ad801-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="ad801-107">Вы узнаете, как для создания контроллеров, как с помощью пункта меню добавления контроллера Visual Studio, так и вручную создав файл класса.</span><span class="sxs-lookup"><span data-stu-id="ad801-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="ad801-108">С помощью контроллера добавить команду меню</span><span class="sxs-lookup"><span data-stu-id="ad801-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="ad801-109">Самый простой способ создать новый контроллер — щелкните правой кнопкой мыши папку Controllers в окне обозревателя решений Visual Studio и выберите **Add, контроллер** меню (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="ad801-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="ad801-110">При выборе этого варианта меню **добавить контроллер** диалогового окна (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="ad801-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="ad801-111">[![Диалоговое окно нового проекта](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ad801-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="ad801-112">**На рисунке 01**: Добавление нового контроллера ([Просмотр полноразмерное изображение](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="ad801-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>


<span data-ttu-id="ad801-113">[![Диалоговое окно нового проекта](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ad801-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="ad801-114">**На рисунке 02**: диалоговое окно добавления контроллера ([Просмотр полноразмерное изображение](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="ad801-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>


<span data-ttu-id="ad801-115">Обратите внимание, что первая часть имени контроллера, выделяется в **добавить контроллер** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="ad801-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="ad801-116">Имя каждого контроллера должно заканчиваться суффиксом *контроллера*.</span><span class="sxs-lookup"><span data-stu-id="ad801-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="ad801-117">Например, можно создать контроллер с именем *ProductController* , но не контроллер с именем *продукта*.</span><span class="sxs-lookup"><span data-stu-id="ad801-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="ad801-118">При создании контроллера, который отсутствует *контроллера* суффикс, то его нельзя вызвать контроллера.</span><span class="sxs-lookup"><span data-stu-id="ad801-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="ad801-119">Не используйте этот — я занято бесчисленные часы своей жизни после установления этой ошибке.</span><span class="sxs-lookup"><span data-stu-id="ad801-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="ad801-120">**Листинг 1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="ad801-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="ad801-121">Рекомендуется всегда создавать контроллеров в папке Controllers находится.</span><span class="sxs-lookup"><span data-stu-id="ad801-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="ad801-122">В противном случае будет нарушения соглашения о ASP.NET MVC и другим разработчикам будет сложнее основные сведения о приложении.</span><span class="sxs-lookup"><span data-stu-id="ad801-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="ad801-123">Методы действия формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="ad801-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="ad801-124">При создании контроллера, у вас есть возможность автоматически создавать методы действий для создания, обновления и получения сведений (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="ad801-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="ad801-125">Если этот параметр выбран, создается класс контроллера в списке 2.</span><span class="sxs-lookup"><span data-stu-id="ad801-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="ad801-126">[![Автоматическое создание методов действий](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ad801-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="ad801-127">**На рисунке 03**: автоматическое создание методов действий ([Просмотр полноразмерное изображение](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ad801-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>


<span data-ttu-id="ad801-128">**Листинг 2 - Controllers\CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="ad801-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="ad801-129">Эти созданные методы являются методы-заглушки.</span><span class="sxs-lookup"><span data-stu-id="ad801-129">These generated methods are stub methods.</span></span> <span data-ttu-id="ad801-130">Необходимо добавить фактической логики для создания, обновления и подробными сведениями для клиента самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="ad801-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="ad801-131">Но методы-заглушки дает с низким приоритетом отправной точки.</span><span class="sxs-lookup"><span data-stu-id="ad801-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="ad801-132">Создание класса контроллера</span><span class="sxs-lookup"><span data-stu-id="ad801-132">Creating a Controller Class</span></span>

<span data-ttu-id="ad801-133">Контроллер ASP.NET MVC — это класс.</span><span class="sxs-lookup"><span data-stu-id="ad801-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="ad801-134">При желании можно игнорировать удобный контроллера формирование шаблонов Visual Studio и создать класс контроллера вручную.</span><span class="sxs-lookup"><span data-stu-id="ad801-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="ad801-135">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="ad801-135">Follow these steps:</span></span>

1. <span data-ttu-id="ad801-136">Щелкните правой кнопкой мыши папку Controllers и выбрать пункт меню **добавить, новый элемент** и выберите **класса** шаблона (см. рис. 4).</span><span class="sxs-lookup"><span data-stu-id="ad801-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="ad801-137">Имя для нового класса PersonController.vb и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ad801-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="ad801-138">Измените полученный файл так, чтобы класс наследует от базового класса System.Web.Mvc.Controller (см. список 3).</span><span class="sxs-lookup"><span data-stu-id="ad801-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="ad801-139">[![Создание нового класса](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ad801-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="ad801-140">**На рисунке 04**: Создание нового класса ([Просмотр полноразмерное изображение](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="ad801-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>


<span data-ttu-id="ad801-141">**Листинг 3 - Controllers\PersonController.vb**</span><span class="sxs-lookup"><span data-stu-id="ad801-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="ad801-142">Контроллер в списке 3 предоставляет одно действие с именем Index(), которое возвращает строку «Hello World!».</span><span class="sxs-lookup"><span data-stu-id="ad801-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="ad801-143">Можно вызвать это действие контроллера путем запуска приложения и запроса URL-адреса следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ad801-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE] 
> 
> <span data-ttu-id="ad801-144">ASP.NET Development Server использует случайный номер порта (например, 40071).</span><span class="sxs-lookup"><span data-stu-id="ad801-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="ad801-145">При вводе URL-адрес для вызова контроллер, необходимо будет указать номер порта вправо.</span><span class="sxs-lookup"><span data-stu-id="ad801-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="ad801-146">Можно определить номер порта, наведите указатель мыши на значок для ASP.NET Development Server в области уведомлений Windows (правой нижней части экрана).</span><span class="sxs-lookup"><span data-stu-id="ad801-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ad801-147">[Назад](adding-dynamic-content-to-a-cached-page-vb.md)
[Вперед](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ad801-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
