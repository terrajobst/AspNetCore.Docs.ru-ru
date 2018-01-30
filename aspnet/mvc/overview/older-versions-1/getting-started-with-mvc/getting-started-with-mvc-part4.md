---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: "Создание базы данных | Документы Microsoft"
author: shanselman
description: "Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, чтение и запись из базы данных."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 22f1042555d7cdd0ca8d75f1cbde65fbb65d81cc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
<a name="creating-a-database"></a><span data-ttu-id="9fcd3-104">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="9fcd3-104">Creating a Database</span></span>
====================
<span data-ttu-id="9fcd3-105">по [Скотт Хансельман](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="9fcd3-106">Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="9fcd3-107">Вы создадите простое веб-приложение выполняет чтение и запись из базы данных.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="9fcd3-108">Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="9fcd3-109">В этом разделе мы собираемся создать новую БД SQL Express, мы будем использовать для хранения и извлечения данных фильма.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="9fcd3-110">В СРЕДЕ Visual Web Developer выберите представление | Обозреватель серверов.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="9fcd3-111">Щелкните правой кнопкой мыши подключения данных и выберите команду Добавить подключение...</span><span class="sxs-lookup"><span data-stu-id="9fcd3-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="9fcd3-113">В диалоговом окне Выбор источника данных выберите Microsoft SQL Server и нажмите кнопку Продолжить.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="9fcd3-114">В диалоговом окне Добавление соединения, введите «. \SQLEXPRESS» для имени сервера и введите «Фильмы» в качестве имени новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="9fcd3-115">[![Добавить диалоговое окно соединения](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="9fcd3-116">Нажмите "ОК" и запрашивается Если вы хотите создать эту базу данных.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="9fcd3-117">Выберите "Да".</span><span class="sxs-lookup"><span data-stu-id="9fcd3-117">Select yes.</span></span>

<span data-ttu-id="9fcd3-118">[![Создать фильмы?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="9fcd3-119">Итак, пустую базу данных в обозревателе серверов.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-119">Now you've got an empty database in Server Explorer.</span></span>

![Добавить новую таблицу](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="9fcd3-121">Щелкните правой кнопкой мыши на таблицы и нажмите кнопку Добавить таблицу.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="9fcd3-122">Появится конструктор таблиц.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-122">The Table Designer will appear.</span></span> <span data-ttu-id="9fcd3-123">Добавьте столбцы для идентификатора, заголовок, ReleaseDate, Жанр и Price.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="9fcd3-124">Щелкните правой кнопкой мыши столбец ID и нажмите кнопку Задать первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="9fcd3-125">Вот какие my области конструктора, имеет следующий вид.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="9fcd3-126">[![Редактор таблиц базы данных](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="9fcd3-127">Кроме того выберите столбец, идентификатор и в разделе ниже свойства столбца измените «Спецификация идентификации» на «Да».</span><span class="sxs-lookup"><span data-stu-id="9fcd3-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="9fcd3-128">[![IsIdentity - свойства столбца](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="9fcd3-129">Если у вас есть задачи, щелкните значок «сохранить» на панели инструментов или выберите файл | Сохранить в меню, а имя таблицы «**фильма**» (единственного числа).</span><span class="sxs-lookup"><span data-stu-id="9fcd3-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="9fcd3-130">У нас есть базы данных и таблицы.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-130">We've got a database and a table!</span></span>

<span data-ttu-id="9fcd3-131">[![Выберите имя](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="9fcd3-132">Вернитесь в обозревателе серверов щелкните правой кнопкой мыши таблицу фильма, затем выберите «Показать таблицу данных».</span><span class="sxs-lookup"><span data-stu-id="9fcd3-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="9fcd3-133">Введите несколько фильмы, базе данных имеет некоторые данные.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="9fcd3-134">[![Редактирование таблиц базы данных](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="9fcd3-135">Создание модели</span><span class="sxs-lookup"><span data-stu-id="9fcd3-135">Creating a Model</span></span>

<span data-ttu-id="9fcd3-136">Теперь вернитесь в обозревателе решений в правой части интегрированной среды разработки и правой кнопкой мыши папку модели и выберите пункт Добавить | Новый элемент.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="9fcd3-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="9fcd3-138">Мы собираемся создать модель EDM из нашей новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="9fcd3-139">Набор классов, добавляются в свой проект, который позволяет с легкостью нам для запросов и управления данными в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="9fcd3-140">Выберите узел данных в левой части диалогового окна, а затем выберите шаблона элемента модели EDM ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="9fcd3-141">Присвойте ему имя Movies.edmx.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="9fcd3-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="9fcd3-143">Нажмите кнопку «Добавить».</span><span class="sxs-lookup"><span data-stu-id="9fcd3-143">Click the "Add" button.</span></span> <span data-ttu-id="9fcd3-144">Затем откроется «мастер моделей EDM».</span><span class="sxs-lookup"><span data-stu-id="9fcd3-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="9fcd3-145">В появившемся окне Новый выберите Создать из базы данных.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="9fcd3-146">Поскольку мы только что внесли базы данных, мы только необходимо сообщить платформе Entity Framework о нашей новой базы данных и соответствующей таблицей.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="9fcd3-147">Нажмите рядом с полем, сохранить соединение базы данных в конфигурации наш веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="9fcd3-148">Теперь проверьте таблицы и фильма флажок и нажмите кнопку Готово.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="9fcd3-149">[![Мастер моделей EDM](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="9fcd3-150">Теперь мы нашей новая таблица фильма в конструкторе Entity Framework и доступ к нему из кода.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="9fcd3-151">[![Фильмы - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="9fcd3-152">В области конструктора появится класса «Фильма».</span><span class="sxs-lookup"><span data-stu-id="9fcd3-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="9fcd3-153">Этот класс сопоставляется с таблицей «Фильма» в базе данных, а каждое свойство внутри он сопоставляется со столбцом в таблице.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="9fcd3-154">Каждый экземпляр класса «Фильма» будут соответствовать строки в таблице «Фильма».</span><span class="sxs-lookup"><span data-stu-id="9fcd3-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="9fcd3-155">Если вас не устраивает именования по умолчанию и сопоставления соглашения, используемые платформой Entity Framework, можно использовать конструктор Entity Framework, изменить или настроить их.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="9fcd3-156">Для этого приложения будет использовать значения по умолчанию и сохраните файл как-является.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="9fcd3-157">Теперь давайте работать с некоторыми реальными данными.</span><span class="sxs-lookup"><span data-stu-id="9fcd3-157">Now, let's work with some real data!</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9fcd3-158">[Назад](getting-started-with-mvc-part3.md)
[Вперед](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="9fcd3-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
