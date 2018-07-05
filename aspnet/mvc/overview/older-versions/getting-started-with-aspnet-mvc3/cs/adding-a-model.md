---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Добавление модели (C#) | Документация Майкрософт
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасное и гораздо проще выполнить и демонстрационных версий...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 797a4b26960881c9df60a47389a0f979b00e45cf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371604"
---
<a name="adding-a-model-c"></a><span data-ttu-id="49333-104">Добавление модели (C#)</span><span class="sxs-lookup"><span data-stu-id="49333-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="49333-105">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="49333-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="49333-106">Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49333-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="49333-107">Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже.</span><span class="sxs-lookup"><span data-stu-id="49333-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="49333-108">Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="49333-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="49333-109">Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:</span><span class="sxs-lookup"><span data-stu-id="49333-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="49333-110">Необходимые компоненты для Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="49333-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="49333-111">Обновление средств ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="49333-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="49333-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)</span><span class="sxs-lookup"><span data-stu-id="49333-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="49333-113">Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="49333-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="49333-114">Проект Visual Web Developer с исходным кодом C# — прилагаются в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="49333-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="49333-115">[Загрузить версию C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="49333-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="49333-116">Если вы предпочитаете Visual Basic, переключитесь в [версии Visual Basic](../vb/adding-a-model.md) работы с этим руководством.</span><span class="sxs-lookup"><span data-stu-id="49333-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="49333-117">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="49333-117">Adding a Model</span></span>

<span data-ttu-id="49333-118">В этом разделе вы добавите некоторые классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="49333-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="49333-119">Эти классы будут «модель» часть приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="49333-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="49333-120">Вы используете технологии доступа к данным .NET Framework, известный как Entity Framework для определения и работы с этими классами модели.</span><span class="sxs-lookup"><span data-stu-id="49333-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="49333-121">Поддерживает Entity Framework (часто обозначается как EF), называется парадигмы разработки *Code First*.</span><span class="sxs-lookup"><span data-stu-id="49333-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="49333-122">Во-первых, код позволяет создавать объекты модели путем написания простых классов.</span><span class="sxs-lookup"><span data-stu-id="49333-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="49333-123">(Они также называются классы POCO, от «plain old CLR объекты».) Затем можно создавать базы данных, созданной в режиме реального времени из классов, который позволяет очень простой и быстрой разработки рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="49333-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="49333-124">Добавление классов модели</span><span class="sxs-lookup"><span data-stu-id="49333-124">Adding Model Classes</span></span>

<span data-ttu-id="49333-125">В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* папку, выберите **добавить**, а затем выберите **класс**.</span><span class="sxs-lookup"><span data-stu-id="49333-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="49333-126">Имя *класс* «Фильма».</span><span class="sxs-lookup"><span data-stu-id="49333-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="49333-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="49333-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="49333-128">Добавьте следующие пять свойства `Movie` класса:</span><span class="sxs-lookup"><span data-stu-id="49333-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="49333-129">Мы будем использовать `Movie` класс для представления фильмов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="49333-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="49333-130">Каждый экземпляр `Movie` объекта будет соответствовать ряду в таблице базы данных, а каждое свойство `Movie` класс сопоставляется со столбцом в таблице.</span><span class="sxs-lookup"><span data-stu-id="49333-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="49333-131">В этом же файле добавьте следующий `MovieDBContext` класса:</span><span class="sxs-lookup"><span data-stu-id="49333-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="49333-132">`MovieDBContext` Класс представляет контекст базы данных movie Entity Framework, который обрабатывает получение, хранения и обновления `Movie` экземпляров в базе данных класса.</span><span class="sxs-lookup"><span data-stu-id="49333-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="49333-133">`MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="49333-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="49333-134">Дополнительные сведения о `DbContext` и `DbSet`, см. в разделе [средств повышения производительности платформы Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="49333-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="49333-135">Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `using` инструкция в верхней части файла:</span><span class="sxs-lookup"><span data-stu-id="49333-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="49333-136">Полный *Movie.cs* файла приведен ниже.</span><span class="sxs-lookup"><span data-stu-id="49333-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="49333-137">Создание строки подключения и работы с SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="49333-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="49333-138">`MovieDBContext` Созданный класс обрабатывает задачи подключения к базе данных и сопоставления `Movie` объектов для записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="49333-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="49333-139">Один вопрос, на который вы можете спросить, однако, как указать базу данных, которая подключается к.</span><span class="sxs-lookup"><span data-stu-id="49333-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="49333-140">Это можно сделать путем добавления сведений о соединении в *Web.config* файл приложения.</span><span class="sxs-lookup"><span data-stu-id="49333-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="49333-141">Откройте корневой каталог приложения *Web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="49333-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="49333-142">(Не *Web.config* файл *представления* папки.) На следующем рисунке показано, как *Web.config* файлы; открыть *Web.config* файл помеченные красными кружками.</span><span class="sxs-lookup"><span data-stu-id="49333-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="49333-143">Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="49333-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="49333-144">В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:</span><span class="sxs-lookup"><span data-stu-id="49333-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="49333-145">Этот небольшой объем кода и XML предоставляет все необходимое для записи для представления и сохранения данных фильмов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="49333-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="49333-146">Далее предстоит создать новый `MoviesController` класс, который можно использовать для отображения данных фильма и разрешить пользователям создавать новые вхождения фильма.</span><span class="sxs-lookup"><span data-stu-id="49333-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="49333-147">[Назад](adding-a-view.md)
> [Вперед](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="49333-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
