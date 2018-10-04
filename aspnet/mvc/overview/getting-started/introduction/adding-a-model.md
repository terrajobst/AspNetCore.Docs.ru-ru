---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Добавление модели | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 3b9d84ae757eac6cc2a1ae8f7857244adff50d7c
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578280"
---
<a name="adding-a-model"></a><span data-ttu-id="0634c-102">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="0634c-102">Adding a Model</span></span>
====================
<span data-ttu-id="0634c-103">по [Рик Андерсон]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="0634c-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="0634c-104">В этом разделе вы добавите некоторые классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0634c-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="0634c-105">Эти классы будут представлять &quot;модели&quot; входит приложение ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0634c-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="0634c-106">Вы используете .NET Framework технологий доступа к данным, известный как [Entity Framework](https://docs.microsoft.com/ef/) определить и работать с этими классами модели.</span><span class="sxs-lookup"><span data-stu-id="0634c-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="0634c-107">Поддерживает Entity Framework (часто обозначается как EF), называется парадигмы разработки *Code First*.</span><span class="sxs-lookup"><span data-stu-id="0634c-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="0634c-108">Во-первых, код позволяет создавать объекты модели путем написания простых классов.</span><span class="sxs-lookup"><span data-stu-id="0634c-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="0634c-109">(Они также называются классами POCO из &quot;обычные старые объекты CLR.&quot;) Затем можно создавать базы данных, созданной в режиме реального времени из классов, который позволяет очень простой и быстрой разработки рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="0634c-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="0634c-110">Вы должны сначала создать базу данных, можно по-прежнему выполнить для этого учебника, чтобы узнать о разработке приложений MVC и EF.</span><span class="sxs-lookup"><span data-stu-id="0634c-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="0634c-111">После этого можно отследить Tom Fizmakens [формирование шаблонов ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) руководство, охватывающее первый подход базы данных.</span><span class="sxs-lookup"><span data-stu-id="0634c-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="0634c-112">Добавление классов модели</span><span class="sxs-lookup"><span data-stu-id="0634c-112">Adding Model Classes</span></span>

<span data-ttu-id="0634c-113">В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* папку, выберите **добавить**, а затем выберите **класс**.</span><span class="sxs-lookup"><span data-stu-id="0634c-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="0634c-114">Введите *класс* имя &quot;фильма&quot;.</span><span class="sxs-lookup"><span data-stu-id="0634c-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="0634c-115">Добавьте следующие пять свойства `Movie` класса:</span><span class="sxs-lookup"><span data-stu-id="0634c-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="0634c-116">Мы будем использовать `Movie` класс для представления фильмов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0634c-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="0634c-117">Каждый экземпляр `Movie` объекта будет соответствовать ряду в таблице базы данных, а каждое свойство `Movie` класс сопоставляется со столбцом в таблице.</span><span class="sxs-lookup"><span data-stu-id="0634c-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="0634c-118">Примечание: Чтобы использовать System.Data.Entity и связанный класс, необходимо установить [пакет NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="0634c-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="0634c-119">Перейдите по ссылке для получения дальнейших инструкций.</span><span class="sxs-lookup"><span data-stu-id="0634c-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="0634c-120">В этом же файле добавьте следующий `MovieDBContext` класса:</span><span class="sxs-lookup"><span data-stu-id="0634c-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="0634c-121">`MovieDBContext` Класс представляет контекст базы данных movie Entity Framework, который обрабатывает получение, хранения и обновления `Movie` экземпляров в базе данных класса.</span><span class="sxs-lookup"><span data-stu-id="0634c-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="0634c-122">`MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0634c-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="0634c-123">Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `using` инструкция в верхней части файла:</span><span class="sxs-lookup"><span data-stu-id="0634c-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="0634c-124">Это можно сделать вручную, добавив в с помощью инструкции, или вы можете наведите указатель мыши красной волнистой линией, нажав кнопку `Show potential fixes` и нажмите кнопку `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="0634c-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="0634c-125">Примечание: Некоторые неиспользуемые `using` инструкций будут удалены.</span><span class="sxs-lookup"><span data-stu-id="0634c-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="0634c-126">Visual Studio будет отображать неиспользуемые зависимости серым.</span><span class="sxs-lookup"><span data-stu-id="0634c-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="0634c-127">Вы можете удалить неиспользуемые зависимости навести указатель мыши на серую зависимости, нажав кнопку `Show potential fixes` и нажмите кнопку **удалить неиспользуемые директивы using.**</span><span class="sxs-lookup"><span data-stu-id="0634c-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="0634c-128">Наконец, мы добавили модели (M в MVC).</span><span class="sxs-lookup"><span data-stu-id="0634c-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="0634c-129">В следующем разделе вы будете работать со строкой подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="0634c-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0634c-130">[Назад](adding-a-view.md)
> [Вперед](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="0634c-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
