---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First с ASP.NET MVC: Настройка представления | Документация Майкрософт'
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: bfbcfd39dd1cf0abe89a00d2958ca010f0e5e109
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376502"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="55225-104">EF Database First с ASP.NET MVC: Настройка представления</span><span class="sxs-lookup"><span data-stu-id="55225-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="55225-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="55225-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="55225-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="55225-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="55225-107">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="55225-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="55225-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="55225-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="55225-109">Части этой серии посвящена изменении представлений автоматически создается, чтобы улучшить представление.</span><span class="sxs-lookup"><span data-stu-id="55225-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="55225-110">Добавление зарегистрированных курсов сведения об учащихся</span><span class="sxs-lookup"><span data-stu-id="55225-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="55225-111">Сформированный код предусматривает хорошей отправной точкой для вашего приложения, но он не обязательно предоставляет все функциональные возможности, необходимые в приложении.</span><span class="sxs-lookup"><span data-stu-id="55225-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="55225-112">Вы можете настроить код в соответствии с требованиями приложения.</span><span class="sxs-lookup"><span data-stu-id="55225-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="55225-113">В настоящее время приложение не отображает зарегистрированные курсов для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="55225-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="55225-114">В этом разделе вы добавите зарегистрированных курсов для каждого учащегося, чтобы **сведения** представление для учащегося.</span><span class="sxs-lookup"><span data-stu-id="55225-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="55225-115">Откройте **Students/Details.cshtml**и ниже последнего &lt;/dl&gt; вкладки, но перед закрывающей скобкой &lt;/div&gt; , добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="55225-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="55225-116">Этот код создает таблицу, которая отображается по одной строке для каждой записи в таблицу Enrollment для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="55225-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="55225-117">**Отображения** метод выводит HTML для объекта (modelItem), который представляет выражение.</span><span class="sxs-lookup"><span data-stu-id="55225-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="55225-118">Используйте метод отображения (а не просто внедрение значение свойства в коде) чтобы убедиться в том, значение форматируется правильно на основе его типа и шаблон для этого типа.</span><span class="sxs-lookup"><span data-stu-id="55225-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="55225-119">В этом примере каждое выражение возвращает одно свойство из текущей записи в цикле, а значения являются типами-примитивами, которые подготавливаются к просмотру как текст.</span><span class="sxs-lookup"><span data-stu-id="55225-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="55225-120">Перейдите в представление указателя учащихся и еще раз и выберите **сведения** для одного из учащихся.</span><span class="sxs-lookup"><span data-stu-id="55225-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="55225-121">Вы увидите, что Зарегистрированные курсы были включены в представлении.</span><span class="sxs-lookup"><span data-stu-id="55225-121">You will see the enrolled courses have been included in the view.</span></span>

![учащихся с регистрацией](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="55225-123">[Назад](changing-the-database.md)
> [Вперед](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="55225-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
