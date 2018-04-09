---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Часть 2: Уровень доступа к данным | Документы Microsoft'
author: JoeStagner
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Вторая часть посвящена Добавление доступа к данным.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="04eb3-104">Часть 2: Уровень доступа к данным</span><span class="sxs-lookup"><span data-stu-id="04eb3-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="04eb3-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="04eb3-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="04eb3-106">Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="04eb3-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="04eb3-107">Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="04eb3-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="04eb3-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="04eb3-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="04eb3-109">Вторая часть посвящена Добавление доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="04eb3-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="04eb3-110">Добавление доступа к данным</span><span class="sxs-lookup"><span data-stu-id="04eb3-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="04eb3-111">Наше приложение электронной коммерции будет зависеть от двух баз данных.</span><span class="sxs-lookup"><span data-stu-id="04eb3-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="04eb3-112">Сведения о заказчике мы используем стандартной базы данных членства ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="04eb3-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="04eb3-113">Для наших корзину покупок и продукта каталога мы будет реализован базы данных SQL Express следующим образом.</span><span class="sxs-lookup"><span data-stu-id="04eb3-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="04eb3-114">После создания базы данных (Commerce.mdf) в приложении приложения\_папку данных, мы можно перейти к созданию нашей уровня доступа к данным платформы Entity Framework .NET.</span><span class="sxs-lookup"><span data-stu-id="04eb3-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="04eb3-115">Мы создадим папку с именем «данных\_доступ» и их правой кнопкой мыши на эту папку и выберите «Добавить новый элемент».</span><span class="sxs-lookup"><span data-stu-id="04eb3-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="04eb3-116">В разделе «Установленные шаблоны» элемент и затем выберите пункт меню «модель EDM ADO.NET» введите EDM\_Commerce.edmx как имя и нажмите кнопку «Добавить».</span><span class="sxs-lookup"><span data-stu-id="04eb3-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="04eb3-117">Выберите «Создать из базы данных».</span><span class="sxs-lookup"><span data-stu-id="04eb3-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="04eb3-118">Сохраните и постройте.</span><span class="sxs-lookup"><span data-stu-id="04eb3-118">Save and build.</span></span>

<span data-ttu-id="04eb3-119">Теперь мы готовы к добавьте наш первый компонент — меню категории продукта.</span><span class="sxs-lookup"><span data-stu-id="04eb3-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="04eb3-120">[Назад](tailspin-spyworks-part-1.md)
> [Вперед](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="04eb3-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
