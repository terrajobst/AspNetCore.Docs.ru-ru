---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: Учебник. Настройка представления для EF Database First с помощью приложения ASP.NET MVC
description: Это руководство посвящено изменении представлений автоматически создается, чтобы улучшить представление.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236501"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="3f382-103">Учебник. Настройка представления для EF Database First с помощью приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3f382-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="3f382-104">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="3f382-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="3f382-105">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="3f382-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="3f382-106">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="3f382-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="3f382-107">Это руководство посвящено изменении представлений автоматически создается, чтобы улучшить представление.</span><span class="sxs-lookup"><span data-stu-id="3f382-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="3f382-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="3f382-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3f382-109">Добавление курсов на страницу сведений о учащегося</span><span class="sxs-lookup"><span data-stu-id="3f382-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="3f382-110">Убедитесь, что курсы добавляются на страницу</span><span class="sxs-lookup"><span data-stu-id="3f382-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f382-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3f382-111">Prerequisites</span></span>

* [<span data-ttu-id="3f382-112">Изменение базы данных</span><span class="sxs-lookup"><span data-stu-id="3f382-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="3f382-113">Добавьте сведения об учащемся курсов</span><span class="sxs-lookup"><span data-stu-id="3f382-113">Add courses to student detail</span></span>

<span data-ttu-id="3f382-114">Сформированный код предусматривает хорошей отправной точкой для вашего приложения, но он не обязательно предоставляет все функциональные возможности, необходимые в приложении.</span><span class="sxs-lookup"><span data-stu-id="3f382-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="3f382-115">Вы можете настроить код в соответствии с требованиями приложения.</span><span class="sxs-lookup"><span data-stu-id="3f382-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="3f382-116">В настоящее время приложение не отображает зарегистрированные курсов для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="3f382-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="3f382-117">В этом разделе вы добавите зарегистрированных курсов для каждого учащегося, чтобы **сведения** представление для учащегося.</span><span class="sxs-lookup"><span data-stu-id="3f382-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="3f382-118">Откройте **представления** > **учащихся** > *Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3f382-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="3f382-119">Ниже последнего &lt;/dl&gt; тег, но перед закрывающей скобкой &lt;/div&gt; , добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="3f382-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="3f382-120">Этот код создает таблицу, которая отображается по одной строке для каждой записи в таблицу Enrollment для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="3f382-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="3f382-121">**Отображения** метод выводит HTML для объекта (modelItem), который представляет выражение.</span><span class="sxs-lookup"><span data-stu-id="3f382-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="3f382-122">Используйте метод отображения (а не просто внедрение значение свойства в коде) чтобы убедиться в том, значение форматируется правильно на основе его типа и шаблон для этого типа.</span><span class="sxs-lookup"><span data-stu-id="3f382-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="3f382-123">В этом примере каждое выражение возвращает одно свойство из текущей записи в цикле, а значения являются типами-примитивами, которые подготавливаются к просмотру как текст.</span><span class="sxs-lookup"><span data-stu-id="3f382-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="3f382-124">Убедитесь, что курсы добавляются</span><span class="sxs-lookup"><span data-stu-id="3f382-124">Confirm courses are added</span></span>

<span data-ttu-id="3f382-125">Запустите решение.</span><span class="sxs-lookup"><span data-stu-id="3f382-125">Run the solution.</span></span> <span data-ttu-id="3f382-126">Нажмите кнопку **список учащихся** и выберите **сведения** для одного из учащихся.</span><span class="sxs-lookup"><span data-stu-id="3f382-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="3f382-127">Вы увидите, что Зарегистрированные курсы были включены в представлении.</span><span class="sxs-lookup"><span data-stu-id="3f382-127">You will see the enrolled courses have been included in the view.</span></span>

![учащихся с регистрацией](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="3f382-129">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="3f382-129">Next steps</span></span>
<span data-ttu-id="3f382-130">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="3f382-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3f382-131">Добавлена курсов на страницу сведений о учащегося</span><span class="sxs-lookup"><span data-stu-id="3f382-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="3f382-132">Подтверждает, что курсы добавляются на страницу</span><span class="sxs-lookup"><span data-stu-id="3f382-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="3f382-133">Перейдите к следующему руководству, чтобы научиться добавлять заметки к данным для указания требований к проверке и формат отображения.</span><span class="sxs-lookup"><span data-stu-id="3f382-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="3f382-134">Улучшения проверки данных</span><span class="sxs-lookup"><span data-stu-id="3f382-134">Enhance data validation</span></span>](enhancing-data-validation.md)
