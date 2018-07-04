---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Добавление проверки в модель | Документация Майкрософт
author: shanselman
description: Это руководство для начинающих, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, которое считывает и записывает в базу данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 1a8c186d5a6b00aaf1061bb4025f4f062203a7df
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402985"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="43ebc-104">Добавление проверки в модель</span><span class="sxs-lookup"><span data-stu-id="43ebc-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="43ebc-105">по [(Scott hanselman)](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="43ebc-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="43ebc-106">Это руководство для начинающих, в котором представлены основные сведения по ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="43ebc-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="43ebc-107">Вы создадите простое веб-приложение, которое считывает и записывает в базу данных.</span><span class="sxs-lookup"><span data-stu-id="43ebc-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="43ebc-108">Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.</span><span class="sxs-lookup"><span data-stu-id="43ebc-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="43ebc-109">В этом разделе мы собираемся реализовать поддержку, необходимые для включения проверки входных данных в наше приложение.</span><span class="sxs-lookup"><span data-stu-id="43ebc-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="43ebc-110">Мы будем убедитесь, что содержимое базы данных будет всегда правильным и предоставляют полезные сообщения об ошибках для конечных пользователей, когда они пытаются, а введите данные фильма, которое не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="43ebc-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="43ebc-111">Начнем с добавления немного логики проверки в класс Movie.</span><span class="sxs-lookup"><span data-stu-id="43ebc-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="43ebc-112">Щелкните правой кнопкой папку модели и выберите Добавить класс.</span><span class="sxs-lookup"><span data-stu-id="43ebc-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="43ebc-113">Назовите класс Movie.</span><span class="sxs-lookup"><span data-stu-id="43ebc-113">Name your class Movie.</span></span>

<span data-ttu-id="43ebc-114">Мы ранее при создании сущности модели фильма, интегрированной среды разработки создал класс Movie.</span><span class="sxs-lookup"><span data-stu-id="43ebc-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="43ebc-115">На самом деле часть класса фильма может быть в один файл и в другом.</span><span class="sxs-lookup"><span data-stu-id="43ebc-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="43ebc-116">Это называется разделяемый класс.</span><span class="sxs-lookup"><span data-stu-id="43ebc-116">This is called a Partial Class.</span></span> <span data-ttu-id="43ebc-117">Мы собираемся расширить класс Movie из другого файла.</span><span class="sxs-lookup"><span data-stu-id="43ebc-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="43ebc-118">Мы создадим класс частичного фильма, указывающего «класс-близнец» некоторые атрибуты, которые будут давать подсказки проверки системы.</span><span class="sxs-lookup"><span data-stu-id="43ebc-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="43ebc-119">Мы будем отметить названия и цены по необходимости и также на том, что цена быть в определенный диапазон.</span><span class="sxs-lookup"><span data-stu-id="43ebc-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="43ebc-120">Щелкните папку Models правой кнопкой мыши и выберите Добавить класс.</span><span class="sxs-lookup"><span data-stu-id="43ebc-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="43ebc-121">Имя класса фильма и нажмите кнопку "ОК".</span><span class="sxs-lookup"><span data-stu-id="43ebc-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="43ebc-122">Вот, что наши частичного фильма класс похож.</span><span class="sxs-lookup"><span data-stu-id="43ebc-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="43ebc-123">Снова запустите приложение и попробуйте ввести фильм с ценой более чем 100.</span><span class="sxs-lookup"><span data-stu-id="43ebc-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="43ebc-124">Вы получите ошибку, после отправки формы.</span><span class="sxs-lookup"><span data-stu-id="43ebc-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="43ebc-125">Ошибка перехвачено на стороне сервера и возникает после отправки формы.</span><span class="sxs-lookup"><span data-stu-id="43ebc-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="43ebc-126">Обратите внимание на то, как ASP.NET MVC, встроенные вспомогательные функции HTML были достаточно интеллектуальным отобразить сообщение об ошибке и оставьте значения для нас внутри элементов textbox:</span><span class="sxs-lookup"><span data-stu-id="43ebc-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="43ebc-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="43ebc-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="43ebc-128">Это прекрасно работает, но было бы неплохо, если удалось пользователю сообщается о на стороне клиента, немедленно, прежде чем сервер получает участвует.</span><span class="sxs-lookup"><span data-stu-id="43ebc-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="43ebc-129">Давайте включить некоторые проверки на стороне клиента с помощью JavaScript.</span><span class="sxs-lookup"><span data-stu-id="43ebc-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="43ebc-130">Добавление проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="43ebc-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="43ebc-131">Поскольку наш класс Movie уже имеет некоторые атрибуты проверки, просто необходимо добавить несколько файлов JavaScript к шаблону представления Create.aspx и добавьте строку кода для включения клиентской проверки вступили в силу.</span><span class="sxs-lookup"><span data-stu-id="43ebc-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="43ebc-132">Изнутри VWD перейдите наших папку представления/фильма и открывают Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="43ebc-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="43ebc-133">Откройте папку «скрипты» в обозревателе решений и перетащите следующие три скрипты, чтобы в &lt;head&gt; тега.</span><span class="sxs-lookup"><span data-stu-id="43ebc-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="43ebc-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="43ebc-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="43ebc-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="43ebc-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="43ebc-136">Требуется, чтобы эти файлы сценариев, чтобы указывать в следующем порядке.</span><span class="sxs-lookup"><span data-stu-id="43ebc-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="43ebc-137">Кроме того добавьте одна строчка кода выше Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="43ebc-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="43ebc-138">Ниже приведен код, показанный в интегрированной среде разработки.</span><span class="sxs-lookup"><span data-stu-id="43ebc-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="43ebc-139">[![Фильмы - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="43ebc-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="43ebc-140">Запустите приложение и снова посетите /Movies/Create и нажмите кнопку Создать, не вводя никаких данных.</span><span class="sxs-lookup"><span data-stu-id="43ebc-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="43ebc-141">Сообщения об ошибках отображаются сразу же без странице флэш-памяти, что мы связываем с отправкой данных выполнения всего пути к серверу.</span><span class="sxs-lookup"><span data-stu-id="43ebc-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="43ebc-142">Это так, как ASP.NET MVC теперь проверяет входные данные на обоих клиента (с использованием JavaScript) и на сервере.</span><span class="sxs-lookup"><span data-stu-id="43ebc-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="43ebc-143">[![Создание — Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="43ebc-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="43ebc-144">Это выглядит хорошо!</span><span class="sxs-lookup"><span data-stu-id="43ebc-144">This is looking good!</span></span> <span data-ttu-id="43ebc-145">Теперь добавим один дополнительный столбец в базу данных.</span><span class="sxs-lookup"><span data-stu-id="43ebc-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="43ebc-146">[Назад](getting-started-with-mvc-part6.md)
> [Вперед](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="43ebc-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
