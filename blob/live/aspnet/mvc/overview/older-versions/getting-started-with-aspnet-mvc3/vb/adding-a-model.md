---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: "Добавление модели (VB) | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a><span data-ttu-id="af1ee-103">Добавление модели (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="af1ee-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="af1ee-104">По [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="af1ee-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="af1ee-105">Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af1ee-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="af1ee-106">Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже.</span><span class="sxs-lookup"><span data-stu-id="af1ee-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="af1ee-107">Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="af1ee-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="af1ee-108">Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:</span><span class="sxs-lookup"><span data-stu-id="af1ee-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="af1ee-109">Необходимые компоненты Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="af1ee-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="af1ee-110">Обновление средств ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="af1ee-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="af1ee-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)</span><span class="sxs-lookup"><span data-stu-id="af1ee-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="af1ee-112">Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="af1ee-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="af1ee-113">Проект Visual Web Developer с VB.NET исходный код доступен по следующему адресу.</span><span class="sxs-lookup"><span data-stu-id="af1ee-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="af1ee-114">[Загрузить версию VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="af1ee-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="af1ee-115">Если вы предпочитаете C#, переключитесь в [C# версии](../cs/adding-a-model.md) этого учебника.</span><span class="sxs-lookup"><span data-stu-id="af1ee-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="af1ee-116">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="af1ee-116">Adding a Model</span></span>

<span data-ttu-id="af1ee-117">В этом разделе вы добавите некоторые классы для управления фильмов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="af1ee-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="af1ee-118">Эти классы будет «модель» часть приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="af1ee-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="af1ee-119">Мы воспользуемся технология доступа к данным .NET Framework платформа Entity Framework для определения и работы с этими классами модели.</span><span class="sxs-lookup"><span data-stu-id="af1ee-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="af1ee-120">Поддерживает платформы Entity Framework (часто обозначается как EF), который называется парадигмы разработки *Code First*.</span><span class="sxs-lookup"><span data-stu-id="af1ee-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="af1ee-121">Код сначала позволяет создавать объекты модели путем написания простых классов.</span><span class="sxs-lookup"><span data-stu-id="af1ee-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="af1ee-122">(Они также известны как классов POCO, от «plain old CLR объектов».) Затем можно установить базу данных создан автоматически из классов, который позволяет очень простой и быстрой разработки рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="af1ee-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="af1ee-123">Добавление классов модели</span><span class="sxs-lookup"><span data-stu-id="af1ee-123">Adding Model Classes</span></span>

<span data-ttu-id="af1ee-124">В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* выберите **добавить**и выберите **класса**.</span><span class="sxs-lookup"><span data-stu-id="af1ee-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="af1ee-125">Имя класса «Фильма».</span><span class="sxs-lookup"><span data-stu-id="af1ee-125">Name the class "Movie".</span></span>

<span data-ttu-id="af1ee-126">Добавьте следующие пять свойства `Movie` класса:</span><span class="sxs-lookup"><span data-stu-id="af1ee-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="af1ee-127">Мы будем использовать `Movie` класс для представления фильмов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="af1ee-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="af1ee-128">Каждый экземпляр `Movie` объекта будет соответствовать строки в таблицу базы данных и каждое свойство `Movie` класса сопоставляются со столбцом в таблице.</span><span class="sxs-lookup"><span data-stu-id="af1ee-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="af1ee-129">В том же файле добавьте следующий `MovieDBContext` класса:</span><span class="sxs-lookup"><span data-stu-id="af1ee-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="af1ee-130">`MovieDBContext` Класс представляет контекст базы данных Entity Framework фильм, который обрабатывает выборка, хранения и обновления `Movie` класса экземпляров в базе данных.</span><span class="sxs-lookup"><span data-stu-id="af1ee-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="af1ee-131">`MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="af1ee-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="af1ee-132">Дополнительные сведения о `DbContext` и `DbSet`, в разделе [средств повышения производительности платформы Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="af1ee-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="af1ee-133">Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `imports` инструкции в верхней части файла:</span><span class="sxs-lookup"><span data-stu-id="af1ee-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="af1ee-134">Полный *Movie.vb* файла показан ниже.</span><span class="sxs-lookup"><span data-stu-id="af1ee-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="af1ee-135">Создание строки подключения и работе с SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="af1ee-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="af1ee-136">`MovieDBContext` Созданный класс обрабатывает задачу подключения к базе данных и сопоставления `Movie` объектов для записи базы данных.</span><span class="sxs-lookup"><span data-stu-id="af1ee-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="af1ee-137">Один вопрос, который может возникнуть вопрос, является, как указать базу данных, в которой будут подключаться.</span><span class="sxs-lookup"><span data-stu-id="af1ee-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="af1ee-138">Который предстоит выполнить путем добавления сведений о соединении в *Web.config* файл приложения.</span><span class="sxs-lookup"><span data-stu-id="af1ee-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="af1ee-139">Откройте корневой каталог приложения *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="af1ee-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="af1ee-140">(Не *Web.config* файла в *представления* папки.) На рисунке ниже Показать оба *Web.config* файлов; откройте *Web.config* файл обведен красным цветом.</span><span class="sxs-lookup"><span data-stu-id="af1ee-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="af1ee-141">Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="af1ee-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="af1ee-142">В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:</span><span class="sxs-lookup"><span data-stu-id="af1ee-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="af1ee-143">Этот небольшой объем кода и XML предоставляет все необходимое для записи для представления и хранить в базе данных фильма.</span><span class="sxs-lookup"><span data-stu-id="af1ee-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="af1ee-144">Далее мы создадим новый `MoviesController` класс, который можно использовать для отображения данных фильма и позволяют пользователям создавать новые вхождения фильма.</span><span class="sxs-lookup"><span data-stu-id="af1ee-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="af1ee-145">[Назад](adding-a-view.md)
[Вперед](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="af1ee-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
