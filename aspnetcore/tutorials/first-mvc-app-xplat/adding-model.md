---
title: Добавление модели в приложение MVC ASP.NET Core
author: rick-anderson
description: Добавление модели в простое приложение ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 77750ba0df7775d6a0e4744811848bfe9782d995
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2018
ms.locfileid: "30896542"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="1a025-103">Добавление модели в приложение MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a025-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="1a025-104">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="1a025-104">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="1a025-105">Добавьте в файл *ToUpperPlugin.cs* следующий код:</span><span class="sxs-lookup"><span data-stu-id="1a025-105">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="1a025-106">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="1a025-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="1a025-107">Соберите приложение, чтобы убедиться, что оно не содержит ошибок. Теперь вы добавили **M**одель в свое приложение **M**VC.</span><span class="sxs-lookup"><span data-stu-id="1a025-107">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="1a025-108">Подготовка проекта для формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="1a025-108">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="1a025-109">Добавьте в файл *MvcMovie.csproj* следующие выделенные пакеты NuGet:</span><span class="sxs-lookup"><span data-stu-id="1a025-109">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="1a025-110">Сохраните файл и нажмите кнопку **Восстановить** в **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости).</span><span class="sxs-lookup"><span data-stu-id="1a025-110">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="1a025-111">Создайте файл *Models/MvcMovieContext.cs* и добавьте следующий класс `MvcMovieContext`:</span><span class="sxs-lookup"><span data-stu-id="1a025-111">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="1a025-112">Откройте файл *Startup.cs* и добавьте два использования:</span><span class="sxs-lookup"><span data-stu-id="1a025-112">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="1a025-113">Добавьте контекст базы данных в файл *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1a025-113">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="1a025-114">За счет этого Entity Framework будет знать, какие классы модели входят в модель данных.</span><span class="sxs-lookup"><span data-stu-id="1a025-114">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="1a025-115">Вы определяете один *набор сущностей* объектов Movie, который будут представлен в базу данных как таблица Movie.</span><span class="sxs-lookup"><span data-stu-id="1a025-115">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="1a025-116">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="1a025-116">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="1a025-117">Формирование шаблонов для MovieController</span><span class="sxs-lookup"><span data-stu-id="1a025-117">Scaffold the MovieController</span></span>

<span data-ttu-id="1a025-118">Откройте окно терминала в папке проекта и выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="1a025-118">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="1a025-119">Ядро формирование шаблонов создает следующее.</span><span class="sxs-lookup"><span data-stu-id="1a025-119">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="1a025-120">Контроллер фильмов (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="1a025-120">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="1a025-121">Файлы представления Razor для страниц Create, Delete, Edit и Index (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="1a025-121">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="1a025-122">Автоматическое создание методов и представлений действия [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="1a025-122">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="1a025-123">Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="1a025-123">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="1a025-124">Теперь у вас есть база данных и страницы для отображения, редактирования, обновления и удаления данных.</span><span class="sxs-lookup"><span data-stu-id="1a025-124">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="1a025-125">В следующем руководстве мы будем работать с базой данных.</span><span class="sxs-lookup"><span data-stu-id="1a025-125">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="1a025-126">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1a025-126">Additional resources</span></span>

* [<span data-ttu-id="1a025-127">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="1a025-127">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="1a025-128">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="1a025-128">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="1a025-129">[Назад: Добавление представления](adding-view.md)
> [Далее: Работа с SQLite](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="1a025-129">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
