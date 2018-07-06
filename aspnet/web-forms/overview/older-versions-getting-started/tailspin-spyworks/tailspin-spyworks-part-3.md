---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Часть 3: Макет и меню категории | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. В части 3 описывается добавление макет и меню категории.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3047c53e21a418aef8617bd772a247bc46edb98f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817529"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="142b8-104">Часть 3: Макет и меню категории</span><span class="sxs-lookup"><span data-stu-id="142b8-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="142b8-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="142b8-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="142b8-106">Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="142b8-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="142b8-107">Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="142b8-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="142b8-108">В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="142b8-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="142b8-109">В части 3 описывается добавление макет и меню категории.</span><span class="sxs-lookup"><span data-stu-id="142b8-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="142b8-110">Добавление некоторых макет и меню категории</span><span class="sxs-lookup"><span data-stu-id="142b8-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="142b8-111">В нашей главной страницы узла мы добавим элемент div для левой части столбца, который будет содержать нашему меню категории продукта.</span><span class="sxs-lookup"><span data-stu-id="142b8-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="142b8-112">Обратите внимание на то, что нужное выравнивание и других видов форматирования, предоставляются классом CSS, который мы добавили в наш файл Style.css.</span><span class="sxs-lookup"><span data-stu-id="142b8-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="142b8-113">Меню для категории продукта динамически создаются во время выполнения, запросив Commerce базы данных для существующей категории продуктов, а также создание элементов меню и соответствующие ссылки.</span><span class="sxs-lookup"><span data-stu-id="142b8-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="142b8-114">Для выполнения этой задачи мы будем использовать два из ASP. NET-элементы управления данными с широкими возможностями.</span><span class="sxs-lookup"><span data-stu-id="142b8-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="142b8-115">Элемент управления «Entity Data Source» и элемента управления «ListView».</span><span class="sxs-lookup"><span data-stu-id="142b8-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="142b8-116">Давайте перейдите в «Конструктор» и используйте вспомогательные методы для настройки четыре элемента управления.</span><span class="sxs-lookup"><span data-stu-id="142b8-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="142b8-117">Установим свойство EntityDataSource ID для EDS\_категории\_меню и щелкните «Настройка источника данных».</span><span class="sxs-lookup"><span data-stu-id="142b8-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="142b8-118">Выберите CommerceEntities соединения, в котором был создан для нас, когда мы создавали исходной модели EDM для нашей базы данных Commerce и нажмите кнопку «Далее».</span><span class="sxs-lookup"><span data-stu-id="142b8-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="142b8-119">Выберите имя набора сущностей «Категории» и оставьте остальные параметры по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="142b8-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="142b8-120">Нажмите кнопку «Готово».</span><span class="sxs-lookup"><span data-stu-id="142b8-120">Click "Finish".</span></span>

<span data-ttu-id="142b8-121">Теперь установим свойство идентификатора экземпляра элемента управления ListView, который мы разместили на нашей странице к ListView\_ProductsMenu и активировать его вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="142b8-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="142b8-122">Хотя мы могли бы использовать параметры управления для форматирования отображения элемента данных и форматирование, мы создавали меню будет требовать только простой разметки, мы будет ввести код в представлении источника.</span><span class="sxs-lookup"><span data-stu-id="142b8-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="142b8-123">Обратите внимание, строку «Eval» инструкция: &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="142b8-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="142b8-124">Синтаксис ASP.NET &lt;% # %&gt; является сокращением условным обозначением, указывает среде выполнения все, что содержится в и выводить результаты «в строке».</span><span class="sxs-lookup"><span data-stu-id="142b8-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="142b8-125">Эта инструкция Eval("CategoryName") указывает, для текущей записи в коллекции связанных элементов данных, извлечения значения из имен элементов сущности модели «CatagoryName».</span><span class="sxs-lookup"><span data-stu-id="142b8-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="142b8-126">Это сокращенный синтаксис для это очень мощная функция.</span><span class="sxs-lookup"><span data-stu-id="142b8-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="142b8-127">Можно запустить приложение сейчас.</span><span class="sxs-lookup"><span data-stu-id="142b8-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="142b8-128">Учтите, что теперь отображается меню категории наших продуктов и при наведении курсора мыши над одним из пунктов меню категории, мы видим, меню элемента ссылка указывает на страницу, у нас есть еще для реализации с именем ProductsList.aspx и что мы разработали строкового аргумента динамический запрос, содержащий  Идентификатор категории.</span><span class="sxs-lookup"><span data-stu-id="142b8-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="142b8-129">[Назад](tailspin-spyworks-part-2.md)
> [Вперед](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="142b8-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
