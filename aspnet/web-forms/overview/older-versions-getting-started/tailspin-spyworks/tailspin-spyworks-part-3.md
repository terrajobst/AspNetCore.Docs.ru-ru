---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Часть 3: Макет и меню категории | Документы Microsoft'
author: JoeStagner
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 3 описывает добавление макета и меню категории.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27a493173b03f813ee3dcbbfafd8bc52fb0b9771
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="5f7d4-104">Часть 3: Макет и меню категории</span><span class="sxs-lookup"><span data-stu-id="5f7d4-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="5f7d4-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="5f7d4-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="5f7d4-106">Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="5f7d4-107">Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="5f7d4-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="5f7d4-109">Часть 3 описывает добавление макета и меню категории.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="5f7d4-110">Добавление некоторых макета и меню категории</span><span class="sxs-lookup"><span data-stu-id="5f7d4-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="5f7d4-111">В нашем главной страницы сайта мы добавим div для левой части столбца, который будет содержать нашей меню категории продукта.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="5f7d4-112">Обратите внимание, что нужное выравнивание и другие параметры форматирования будут предоставляться класс CSS, который мы добавили в наш файл Style.css.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="5f7d4-113">Меню для категории продукта динамически создаются во время выполнения с помощью запроса к базе данных Commerce для существующей категории продуктов, а также создание пунктов меню и соответствующие ссылки.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="5f7d4-114">Для этого мы будем использовать два ASP. NET элементы управления данными с широкими возможностями.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="5f7d4-115">Элемент управления «Entity Data Source» и элемента управления «ListView».</span><span class="sxs-lookup"><span data-stu-id="5f7d4-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="5f7d4-116">Давайте переключитесь в «Конструктор» и используйте вспомогательные методы, чтобы настроить элемент управления.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="5f7d4-117">Давайте свойству ID элемента управления EntityDataSource ТАБЛИЦ\_категории\_меню и нажмите кнопку «Настройка источника данных».</span><span class="sxs-lookup"><span data-stu-id="5f7d4-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="5f7d4-118">Выберите CommerceEntities соединения, в котором был создан для нас, создавая исходной модели EDM для торговли США в базе данных и нажмите кнопку «Далее».</span><span class="sxs-lookup"><span data-stu-id="5f7d4-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="5f7d4-119">Выберите имя набора сущностей «Категории» и оставьте другие параметры по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="5f7d4-120">Нажмите кнопку «Готово».</span><span class="sxs-lookup"><span data-stu-id="5f7d4-120">Click "Finish".</span></span>

<span data-ttu-id="5f7d4-121">Теперь давайте задайте свойство идентификатора экземпляра элемента управления ListView, который мы разместили на нашей странице к ListView\_ProductsMenu и активировать его поддержки.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="5f7d4-122">Хотя можно использовать параметры управления для форматирования элемента данных и форматирование, нашей меню создания только потребует простой разметки, мы будет вводить этот код в представлении источника.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="5f7d4-123">Имейте в виду оператор строку «Eval»: &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="5f7d4-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="5f7d4-124">Синтаксис ASP.NET &lt;% # %&gt; — сокращение соглашение, которое указывает, что среда выполнения независимо от содержащихся в и выводить результаты «в строке».</span><span class="sxs-lookup"><span data-stu-id="5f7d4-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="5f7d4-125">Эта инструкция Eval("CategoryName") указывает, для текущей записи в привязанной коллекции элементов данных выборки значения имен элемента модели сущности «CatagoryName».</span><span class="sxs-lookup"><span data-stu-id="5f7d4-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="5f7d4-126">Это сокращенный синтаксис для очень мощная функция.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="5f7d4-127">Позволяет запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="5f7d4-128">Обратите внимание, теперь отображается меню категории наших продуктов и при наведении курсора мыши над одним из пунктов меню категории, можно увидеть меню элемента ссылка указывает на страницу, у нас есть еще для реализации с именем ProductsList.aspx и что мы создали динамического запроса строковый аргумент, содержащий  Идентификатор категории.</span><span class="sxs-lookup"><span data-stu-id="5f7d4-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5f7d4-129">[Назад](tailspin-spyworks-part-2.md)
> [Вперед](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="5f7d4-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
