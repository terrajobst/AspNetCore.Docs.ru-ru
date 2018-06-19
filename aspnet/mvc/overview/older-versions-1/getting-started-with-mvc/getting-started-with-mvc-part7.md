---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Добавление проверки для модели | Документы Microsoft
author: shanselman
description: Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, чтение и запись из базы данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871952"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="a26bf-104">Добавление проверки для модели</span><span class="sxs-lookup"><span data-stu-id="a26bf-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="a26bf-105">по [Скотт Хансельман](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="a26bf-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="a26bf-106">Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a26bf-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="a26bf-107">Вы создадите простое веб-приложение выполняет чтение и запись из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a26bf-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="a26bf-108">Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.</span><span class="sxs-lookup"><span data-stu-id="a26bf-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="a26bf-109">В этом разделе мы собираемся реализовать функции, необходимые для включения проверки входных данных в приложении.</span><span class="sxs-lookup"><span data-stu-id="a26bf-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="a26bf-110">Мы будем убедитесь, что наши содержимое базы данных всегда правильно работает и предоставляют полезные сообщения об ошибках для конечных пользователей при их повторите ввод данных фильма, которое не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="a26bf-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="a26bf-111">Для начала рассмотрим путем добавления немного логику проверки в класс фильма.</span><span class="sxs-lookup"><span data-stu-id="a26bf-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="a26bf-112">Щелкните правой кнопкой мыши папку модели и выберите пункт Добавить класс.</span><span class="sxs-lookup"><span data-stu-id="a26bf-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="a26bf-113">Имя класса фильма.</span><span class="sxs-lookup"><span data-stu-id="a26bf-113">Name your class Movie.</span></span>

<span data-ttu-id="a26bf-114">Когда ранее созданную модель сущностей фильма фильма класс создан интегрированной среды разработки.</span><span class="sxs-lookup"><span data-stu-id="a26bf-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="a26bf-115">На самом деле часть класса фильма может быть в один файл и входит в другой.</span><span class="sxs-lookup"><span data-stu-id="a26bf-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="a26bf-116">Это называется разделяемый класс.</span><span class="sxs-lookup"><span data-stu-id="a26bf-116">This is called a Partial Class.</span></span> <span data-ttu-id="a26bf-117">Мы собираемся расширить класс фильма из другого файла.</span><span class="sxs-lookup"><span data-stu-id="a26bf-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="a26bf-118">Мы создадим фильма разделяемого класса, указывающего «класс-близнец» с некоторых атрибутов, которые будут давать подсказки проверки в системе.</span><span class="sxs-lookup"><span data-stu-id="a26bf-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="a26bf-119">Мы будет пометить названия и цены как обязательные, а также на том, что цена должно быть в пределах определенного диапазона.</span><span class="sxs-lookup"><span data-stu-id="a26bf-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="a26bf-120">Щелкните правой кнопкой мыши папку модели и выберите пункт Добавить класс.</span><span class="sxs-lookup"><span data-stu-id="a26bf-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="a26bf-121">Имя класса фильма и нажмите кнопку "ОК".</span><span class="sxs-lookup"><span data-stu-id="a26bf-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="a26bf-122">Вот, что наши частичного фильма класса выглядит.</span><span class="sxs-lookup"><span data-stu-id="a26bf-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="a26bf-123">Повторно запустите приложение и вводят фильма с ценой более чем 100.</span><span class="sxs-lookup"><span data-stu-id="a26bf-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="a26bf-124">После отправки формы, приведет к ошибке.</span><span class="sxs-lookup"><span data-stu-id="a26bf-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="a26bf-125">Ошибка перехватывается на стороне сервера и возникает после отправки формы.</span><span class="sxs-lookup"><span data-stu-id="a26bf-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="a26bf-126">Обратите внимание на то, как ASP.NET MVC встроенные вспомогательные методы HTML были умеет отображать сообщение об ошибке и оставьте значения нам внутри текстового поля элементов:</span><span class="sxs-lookup"><span data-stu-id="a26bf-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="a26bf-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a26bf-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="a26bf-128">Это отлично работает, но было бы неплохо, если мы может попросить пользователя на стороне клиента, немедленно, прежде чем сервер получает участвуют.</span><span class="sxs-lookup"><span data-stu-id="a26bf-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="a26bf-129">Давайте включить некоторые проверки на стороне клиента с использованием JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a26bf-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="a26bf-130">Добавление проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="a26bf-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="a26bf-131">Поскольку нашего фильма класса уже имеет некоторые атрибуты проверки, нам просто нужно будет добавить несколько файлов JavaScript в наших Create.aspx Просмотр шаблона и добавить строку кода для включения клиентской проверки вступили в силу.</span><span class="sxs-lookup"><span data-stu-id="a26bf-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="a26bf-132">Из VWD go наши представления/фильма папку и откройте Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="a26bf-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="a26bf-133">Откройте папку «скрипты» в обозревателе решений и перетащите следующих трех сценариев в &lt;head&gt; тег.</span><span class="sxs-lookup"><span data-stu-id="a26bf-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="a26bf-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="a26bf-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="a26bf-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="a26bf-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="a26bf-136">Требуется, чтобы эти файлы в указанном порядке.</span><span class="sxs-lookup"><span data-stu-id="a26bf-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="a26bf-137">Кроме того добавьте эта строка выше Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="a26bf-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="a26bf-138">Ниже приведен код, показанный в среде IDE.</span><span class="sxs-lookup"><span data-stu-id="a26bf-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="a26bf-139">[![Фильмы - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a26bf-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="a26bf-140">Запустите приложение и повторном посещении /Movies/Create и нажмите кнопку Создать, не вводя данные.</span><span class="sxs-lookup"><span data-stu-id="a26bf-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="a26bf-141">Сообщения об ошибках отображаются сразу же без странице флэш-памяти, связанные с отправкой данных все способ обратно на сервер.</span><span class="sxs-lookup"><span data-stu-id="a26bf-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="a26bf-142">Это ASP.NET MVC теперь проверку входных данных на обоих клиента (с использованием JavaScript) и на сервере.</span><span class="sxs-lookup"><span data-stu-id="a26bf-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="a26bf-143">[![Создание - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a26bf-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="a26bf-144">Требуется хорошо!</span><span class="sxs-lookup"><span data-stu-id="a26bf-144">This is looking good!</span></span> <span data-ttu-id="a26bf-145">Теперь добавим один дополнительный столбец в базу данных.</span><span class="sxs-lookup"><span data-stu-id="a26bf-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a26bf-146">[Назад](getting-started-with-mvc-part6.md)
> [Вперед](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="a26bf-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
