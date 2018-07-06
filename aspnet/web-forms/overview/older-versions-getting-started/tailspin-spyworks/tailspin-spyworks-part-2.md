---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Часть 2: Уровень доступа к данным | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. В части 2 рассматривается добавление уровня доступа к данным.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: c1d61681952cbf1f0587f0e42da21f8436d79ebb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812036"
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="12ea8-104">Часть 2: Уровень доступа к данным</span><span class="sxs-lookup"><span data-stu-id="12ea8-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="12ea8-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="12ea8-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="12ea8-106">Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="12ea8-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="12ea8-107">Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="12ea8-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="12ea8-108">В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="12ea8-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="12ea8-109">В части 2 рассматривается добавление уровня доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="12ea8-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="12ea8-110">Добавление уровня доступа к данным</span><span class="sxs-lookup"><span data-stu-id="12ea8-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="12ea8-111">Наше приложение электронной коммерции будет зависеть от двух баз данных.</span><span class="sxs-lookup"><span data-stu-id="12ea8-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="12ea8-112">Сведения о клиенте мы будем использовать стандартной базы данных членства ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="12ea8-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="12ea8-113">Для наших корзину для покупок и продукта каталог реализуется базу данных SQL Express следующим образом.</span><span class="sxs-lookup"><span data-stu-id="12ea8-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="12ea8-114">После создания базы данных (Commerce.mdf) в приложении приложения\_можно переходить к слою доступа данных, использующий .NET Entity Framework создать папку данных.</span><span class="sxs-lookup"><span data-stu-id="12ea8-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="12ea8-115">Мы создадим папку с именем «данные\_доступ» и их правой кнопкой мыши на эту папку и выберите «Добавить новый элемент».</span><span class="sxs-lookup"><span data-stu-id="12ea8-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="12ea8-116">В «Установленные шаблоны» элемент и затем выберите «ADO.NET Entity Data Model» введите EDM\_Commerce.edmx как имя и нажмите кнопку «Добавить».</span><span class="sxs-lookup"><span data-stu-id="12ea8-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="12ea8-117">Выберите «Создать из базы данных».</span><span class="sxs-lookup"><span data-stu-id="12ea8-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="12ea8-118">Сохраните и выполните сборку.</span><span class="sxs-lookup"><span data-stu-id="12ea8-118">Save and build.</span></span>

<span data-ttu-id="12ea8-119">Теперь мы готовы добавить наш первый компонент — меню категории продукта.</span><span class="sxs-lookup"><span data-stu-id="12ea8-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="12ea8-120">[Назад](tailspin-spyworks-part-1.md)
> [Вперед](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="12ea8-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
