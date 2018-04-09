---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF базы данных сначала с ASP.NET MVC: Настройка представления | Документы Microsoft'
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложения, который предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="4f0a2-104">EF базы данных сначала с ASP.NET MVC: Настройка представления</span><span class="sxs-lookup"><span data-stu-id="4f0a2-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="4f0a2-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4f0a2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4f0a2-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложения, который предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="4f0a2-107">Этот ряд учебника показано, как автоматически создают код, который дает пользователям возможность отображения, изменения, создания и удаления данных, которые хранятся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="4f0a2-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="4f0a2-109">Эта часть ряда фокусируется на изменение представлений автоматически создается, чтобы улучшить представление.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="4f0a2-110">Добавьте сведения об учащихся зарегистрированных курсов</span><span class="sxs-lookup"><span data-stu-id="4f0a2-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="4f0a2-111">Сформированный код предусматривает хорошей отправной точкой для вашего приложения, но он не обязательно предоставляет все функциональные возможности, необходимые в приложении.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="4f0a2-112">Можно изменить код в соответствии с требованиями приложения.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="4f0a2-113">В настоящее время приложение не отображает зарегистрированных курсов для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="4f0a2-114">В этом разделе вы добавите курсов, зарегистрированных для каждого студента для **сведения** представление для учащихся.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="4f0a2-115">Откройте **Students/Details.cshtml**и ниже последней &lt;/dl&gt; вкладки, но перед закрывающим тегом &lt;/div&gt; , добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="4f0a2-116">Этот код создает таблицу, отображающую строку для каждой записи в таблицу Enrollment для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="4f0a2-117">**Отображения** метод отображает HTML для объекта (modelItem), который представляет выражение.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="4f0a2-118">Используйте метод отображения (а не просто внедрение значение свойства в коде) чтобы убедиться, что оно будет отформатировано неправильно на основе ее типа и шаблон для этого типа.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="4f0a2-119">В этом примере каждое выражение возвращает одно свойство из текущей записи в цикле и значения, простые типы, которые отображаются в виде текста.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="4f0a2-120">Перейдите к представлению студентов или индекса еще раз и выберите **сведения** для одного из слушателей.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="4f0a2-121">Вы увидите, что Зарегистрированные курсы включены в представление.</span><span class="sxs-lookup"><span data-stu-id="4f0a2-121">You will see the enrolled courses have been included in the view.</span></span>

![студента с регистрации](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="4f0a2-123">[Назад](changing-the-database.md)
> [Вперед](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="4f0a2-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
