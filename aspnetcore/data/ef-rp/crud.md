---
title: "Razor Pages с EF Core — операции CRUD — 2 из 8"
author: rick-anderson
description: "Описание операций создания, чтения, обновления и удаления в EF Core"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/crud
ms.openlocfilehash: 757aeb713b645cea0fe633b150784184d2d3571e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="1a134-103">Создание, чтение, обновление и удаление — EF Core с Razor Pages (2 из 8)</span><span class="sxs-lookup"><span data-stu-id="1a134-103">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="1a134-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="1a134-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="1a134-105">В этом учебнике описывается проверка и настройка шаблонного кода операций CRUD (создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="1a134-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="1a134-106">Примечание. Чтобы максимально упростить этот код и сконцентрироваться на работе с EF Core, в моделях страниц Razor Pages в этих руководствах используется код EF Core.</span><span class="sxs-lookup"><span data-stu-id="1a134-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages page models.</span></span> <span data-ttu-id="1a134-107">Некоторые разработчики используют уровень служб или шаблон репозитория для создания уровня абстракции между пользовательским интерфейсом (Razor Pages) и уровнем доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="1a134-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="1a134-108">В рамках этого учебника изменяются страницы Razor Pages в папке *Student*, предназначенные для создания, редактирования, удаления и просмотра сведений.</span><span class="sxs-lookup"><span data-stu-id="1a134-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="1a134-109">В шаблонном коде используется следующий шаблон для страниц создания, редактирования и удаления:</span><span class="sxs-lookup"><span data-stu-id="1a134-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="1a134-110">Получение и отображение запрашиваемых данных с помощью метода HTTP GET`OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="1a134-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="1a134-111">Сохранение изменений в данных с помощью метода HTTP POST`OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="1a134-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="1a134-112">Страницы указателя и сведений получают и отображают запрашиваемые данные с помощью метода HTTP GET`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="1a134-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="1a134-113">Замена SingleOrDefaultAsync на FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="1a134-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="1a134-114">В созданном коде для выборки запрашиваемой сущности используется [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="1a134-114">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="1a134-115">При выборке одной сущности более высокую эффективность демонстрирует [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_):</span><span class="sxs-lookup"><span data-stu-id="1a134-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="1a134-116">Если в коде не требуется проверять, что запрос возвращает не более одной сущности.</span><span class="sxs-lookup"><span data-stu-id="1a134-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="1a134-117">`SingleOrDefaultAsync` выбирает больше данных и выполняет ненужные операции.</span><span class="sxs-lookup"><span data-stu-id="1a134-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="1a134-118">`SingleOrDefaultAsync` вызывает исключение при наличии нескольких сущностей, соответствующих части фильтра.</span><span class="sxs-lookup"><span data-stu-id="1a134-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="1a134-119">`FirstOrDefaultAsync` на вызывает исключение при наличии нескольких сущностей, соответствующих части фильтра.</span><span class="sxs-lookup"><span data-stu-id="1a134-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<span data-ttu-id="1a134-120">Выполните глобальную замену `SingleOrDefaultAsync` на `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="1a134-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="1a134-121">`SingleOrDefaultAsync` используется в 5 случаях:</span><span class="sxs-lookup"><span data-stu-id="1a134-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="1a134-122">`OnGetAsync` на странице сведений.</span><span class="sxs-lookup"><span data-stu-id="1a134-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="1a134-123">`OnGetAsync` и `OnPostAsync` на страницах редактирования и удаления.</span><span class="sxs-lookup"><span data-stu-id="1a134-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="1a134-124">FindAsync</span><span class="sxs-lookup"><span data-stu-id="1a134-124">FindAsync</span></span>

<span data-ttu-id="1a134-125">Чаще всего в шаблонном коде [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) можно использовать вместо `FirstOrDefaultAsync` или `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="1a134-125">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="1a134-126">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="1a134-126">`FindAsync`:</span></span>

* <span data-ttu-id="1a134-127">Находит сущность с первичным ключом.</span><span class="sxs-lookup"><span data-stu-id="1a134-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="1a134-128">Если сущность с первичным ключом отслеживается контекстом, она возвращается без запроса к базе данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-128">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="1a134-129">Является простым и быстрым.</span><span class="sxs-lookup"><span data-stu-id="1a134-129">Is simple and concise.</span></span>
* <span data-ttu-id="1a134-130">Оптимизирован для поиска одной сущности.</span><span class="sxs-lookup"><span data-stu-id="1a134-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="1a134-131">В некоторых ситуациях может давать преимущества в производительности, однако редко используется в обычных веб-сценариях.</span><span class="sxs-lookup"><span data-stu-id="1a134-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="1a134-132">Неявно использует [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) вместо [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="1a134-132">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="1a134-133">Однако если необходимо включить другие сущности, результаты поиска более не будут подходить.</span><span class="sxs-lookup"><span data-stu-id="1a134-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="1a134-134">Это значит, что по мере работы приложения может потребоваться отменить результаты поиска и перейти к запросу.</span><span class="sxs-lookup"><span data-stu-id="1a134-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="1a134-135">Настройка страницы сведений</span><span class="sxs-lookup"><span data-stu-id="1a134-135">Customize the Details page</span></span>

<span data-ttu-id="1a134-136">Перейдите на страницу `Pages/Students`.</span><span class="sxs-lookup"><span data-stu-id="1a134-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="1a134-137">Ссылки **Edit**, **Details** и **Delete** создаются [вспомогательной функцией тегов привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) в файле *Pages/Students/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a134-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="1a134-138">Щелкните ссылку Details.</span><span class="sxs-lookup"><span data-stu-id="1a134-138">Select a Details link.</span></span> <span data-ttu-id="1a134-139">URL-адрес имеет вид `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="1a134-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="1a134-140">Идентификатор учащегося передается с помощью строки запроса (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="1a134-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="1a134-141">Обновите страницы Razor Pages Edit, Details и Delete так, чтобы использовался шаблон маршрута `"{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1a134-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="1a134-142">Измените директиву страницы для каждой из этих страниц c `@page` на `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1a134-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="1a134-143">Запрос к странице с шаблоном маршрута "{id:int}", который **не** включает в себя целочисленное значение маршрута, приводит к ошибке HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="1a134-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="1a134-144">Например, `http://localhost:5000/Students/Details` возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="1a134-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="1a134-145">Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:</span><span class="sxs-lookup"><span data-stu-id="1a134-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="1a134-146">Запустите приложение, щелкните ссылку Details и убедитесь, что в URL-адресе в виде данных маршрута передается идентификатор (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="1a134-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="1a134-147">Не выполняйте глобальную замену `@page` на `@page "{id:int}"`, поскольку это приведет к нарушению ссылок на страницы Home и Create.</span><span class="sxs-lookup"><span data-stu-id="1a134-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="1a134-148">Добавление связанных данных</span><span class="sxs-lookup"><span data-stu-id="1a134-148">Add related data</span></span>

<span data-ttu-id="1a134-149">Шаблонный код страницы указателя учащихся не включает свойство `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="1a134-149">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="1a134-150">В этом разделе на странице Details отображается содержимое коллекции `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="1a134-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="1a134-151">Метод `OnGetAsync` в файле *Pages/Students/Details.cshtml.cs* использует метод `FirstOrDefaultAsync` для извлечения одной сущности `Student`.</span><span class="sxs-lookup"><span data-stu-id="1a134-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="1a134-152">Добавьте выделенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="1a134-152">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="1a134-153">Методы `Include` и `ThenInclude` инструктируют контекст для загрузки свойства навигации `Student.Enrollments`, а также свойства навигации `Enrollment.Course` в пределах каждой регистрации.</span><span class="sxs-lookup"><span data-stu-id="1a134-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="1a134-154">Эти методы более подробно рассматриваются в учебнике, посвященном чтению данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-154">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="1a134-155">Метод `AsNoTracking` повышает производительность в тех сценариях, где возвращаемые сущности не обновляются в текущем контексте.</span><span class="sxs-lookup"><span data-stu-id="1a134-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="1a134-156">`AsNoTracking` рассматривается позднее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="1a134-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="1a134-157">Отображение связанных регистраций на странице Details</span><span class="sxs-lookup"><span data-stu-id="1a134-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="1a134-158">Откройте файл *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a134-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="1a134-159">Добавьте выделенный ниже код, чтобы отобразить список регистраций:</span><span class="sxs-lookup"><span data-stu-id="1a134-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="1a134-160">Если после вставки кода нарушаются отступы в нем, нажмите клавиши CTRL-K-D, чтобы исправить это.</span><span class="sxs-lookup"><span data-stu-id="1a134-160">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="1a134-161">Приведенный выше код циклически обрабатывает сущности в свойстве навигации `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="1a134-161">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="1a134-162">Для каждой регистрации он отображает название курса и оценку.</span><span class="sxs-lookup"><span data-stu-id="1a134-162">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="1a134-163">Название курса извлекается из сущности Course, которая хранится в свойстве навигации `Course` сущности Enrollments.</span><span class="sxs-lookup"><span data-stu-id="1a134-163">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="1a134-164">Запустите приложение, выберите вкладку **Students** (Учащиеся) и щелкните ссылку **Details** (Сведения) для учащегося.</span><span class="sxs-lookup"><span data-stu-id="1a134-164">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="1a134-165">Отобразится список курсов и оценок для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="1a134-165">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="1a134-166">Обновление страницы Create</span><span class="sxs-lookup"><span data-stu-id="1a134-166">Update the Create page</span></span>

<span data-ttu-id="1a134-167">Измените метод `OnPostAsync` в файле *Pages/Students/Create.cshtml.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="1a134-167">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="1a134-168">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="1a134-168">TryUpdateModelAsync</span></span>

<span data-ttu-id="1a134-169">Проверьте код [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_):</span><span class="sxs-lookup"><span data-stu-id="1a134-169">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="1a134-170">В приведенном выше коде `TryUpdateModelAsync<Student>` пытается обновить объект `emptyStudent`, используя отправленные значения формы из свойства [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) в [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="1a134-170">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="1a134-171">`TryUpdateModelAsync` обновляет только перечисленные свойства (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="1a134-171">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="1a134-172">В предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="1a134-172">In the preceding sample:</span></span>

* <span data-ttu-id="1a134-173">Второй аргумент (` "student", // Prefix`) представляет собой префикс для поиска значений.</span><span class="sxs-lookup"><span data-stu-id="1a134-173">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="1a134-174">Задается без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="1a134-174">It's not case sensitive.</span></span>
* <span data-ttu-id="1a134-175">Отправленные значения формы преобразуются в типы в модели `Student` с использованием [привязки модели](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="1a134-175">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="1a134-176">Чрезмерная передача данных</span><span class="sxs-lookup"><span data-stu-id="1a134-176">Overposting</span></span>

<span data-ttu-id="1a134-177">В целях повышения безопасности рекомендуется использовать `TryUpdateModel` для обновления полей на основе отправленных значений, поскольку в этом случае исключается чрезмерная передача данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-177">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="1a134-178">Например, сущность Student включает свойство `Secret`, которое веб-страница не должна обновлять или добавлять:</span><span class="sxs-lookup"><span data-stu-id="1a134-178">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="1a134-179">Даже если приложение не имеет поля `Secret` на странице создания или обновления Razor Pages, злоумышленник может установить значение `Secret` посредством чрезмерной передачи данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-179">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="1a134-180">Злоумышленник может использовать такие средства, как Fiddler, или собственный код JavaScript для отправки значения формы `Secret`.</span><span class="sxs-lookup"><span data-stu-id="1a134-180">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="1a134-181">В исходном коде не ограничиваются поля, которые используются при создании экземпляра Student связывателем модели.</span><span class="sxs-lookup"><span data-stu-id="1a134-181">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="1a134-182">Какое бы значение ни задал злоумышленник для поля формы `Secret`, оно будет обновлено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-182">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="1a134-183">На следующем рисунке показано средство Fiddler, с помощью которого в отправленные значения формы добавляется поле `Secret` (со значением "OverPost").</span><span class="sxs-lookup"><span data-stu-id="1a134-183">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Добавление поля Secret с помощью средства Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="1a134-185">Значение "OverPost" успешно добавлено в свойство `Secret` вставленной строки.</span><span class="sxs-lookup"><span data-stu-id="1a134-185">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="1a134-186">Разработчик приложения не планировал, что свойство `Secret` будет устанавливаться на странице Create.</span><span class="sxs-lookup"><span data-stu-id="1a134-186">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="1a134-187">Модель представления</span><span class="sxs-lookup"><span data-stu-id="1a134-187">View model</span></span>

<span data-ttu-id="1a134-188">Модель представления обычно содержит подмножество свойств, которые включены в модель и используются приложением.</span><span class="sxs-lookup"><span data-stu-id="1a134-188">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="1a134-189">Модель приложения часто называют моделью домена.</span><span class="sxs-lookup"><span data-stu-id="1a134-189">The application model is often called the domain model.</span></span> <span data-ttu-id="1a134-190">Модель домена обычно содержит все свойства, необходимые для соответствующей сущности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-190">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="1a134-191">Модель представления содержит только те свойства, которые необходимы уровню пользовательского интерфейса (например, на странице Create).</span><span class="sxs-lookup"><span data-stu-id="1a134-191">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="1a134-192">Помимо модели представления в некоторых приложениях используется модель привязки или модель ввода для передачи данных между классом модели страницы Razor Pages и браузером.</span><span class="sxs-lookup"><span data-stu-id="1a134-192">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="1a134-193">Рассмотрим следующую модель представления `Student`:</span><span class="sxs-lookup"><span data-stu-id="1a134-193">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="1a134-194">Модели представления реализуют альтернативный подход к защите от чрезмерной передачи данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-194">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="1a134-195">Модель представления содержит только свойства для просмотра (отображения) или обновления.</span><span class="sxs-lookup"><span data-stu-id="1a134-195">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="1a134-196">В следующем коде модель представления используется `StudentVM` для создания нового учащегося:</span><span class="sxs-lookup"><span data-stu-id="1a134-196">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="1a134-197">Метод [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) устанавливает значения этого объекта, считывая значения из другого объекта [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues).</span><span class="sxs-lookup"><span data-stu-id="1a134-197">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="1a134-198">`SetValues` использует сопоставление имен свойств.</span><span class="sxs-lookup"><span data-stu-id="1a134-198">`SetValues` uses property name matching.</span></span> <span data-ttu-id="1a134-199">Тип модели представления может быть не связан с типом модели, однако они должны содержать совпадающие свойства.</span><span class="sxs-lookup"><span data-stu-id="1a134-199">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="1a134-200">При использовании `StudentVM` необходимо обновить [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml), чтобы использовать `StudentVM` вместо `Student`.</span><span class="sxs-lookup"><span data-stu-id="1a134-200">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="1a134-201">В Razor Pages представление модели реализуется с помощью производного класса `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="1a134-201">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="1a134-202">Обновление страницы редактирования</span><span class="sxs-lookup"><span data-stu-id="1a134-202">Update the Edit page</span></span>

<span data-ttu-id="1a134-203">Обновите модель страницы для страницы Edit:</span><span class="sxs-lookup"><span data-stu-id="1a134-203">Update the page model for the Edit page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="1a134-204">Изменения в коде аналогичны странице Create за некоторыми исключениями:</span><span class="sxs-lookup"><span data-stu-id="1a134-204">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="1a134-205">`OnPostAsync` имеет необязательный параметр `id`.</span><span class="sxs-lookup"><span data-stu-id="1a134-205">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="1a134-206">Текущий учащийся извлекается из базы данных вместо того, чтобы создавать нового учащегося.</span><span class="sxs-lookup"><span data-stu-id="1a134-206">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="1a134-207">`FirstOrDefaultAsync` заменен на [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="1a134-207">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="1a134-208">`FindAsync` лучше использовать при выборе сущности из первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="1a134-208">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="1a134-209">Дополнительные сведения см. в разделе [FindAsync](#FindAsync).</span><span class="sxs-lookup"><span data-stu-id="1a134-209">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="1a134-210">Проверка страниц редактирования и создания</span><span class="sxs-lookup"><span data-stu-id="1a134-210">Test the Edit and Create pages</span></span>

<span data-ttu-id="1a134-211">Создайте и измените несколько сущностей учащихся.</span><span class="sxs-lookup"><span data-stu-id="1a134-211">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="1a134-212">Состояния сущностей</span><span class="sxs-lookup"><span data-stu-id="1a134-212">Entity States</span></span>

<span data-ttu-id="1a134-213">Контекст базы данных отслеживает синхронизацию сущностей в памяти с соответствующими им строками в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-213">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="1a134-214">Сведения о синхронизации контекста базы данных определяют поведение при вызове метода `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1a134-214">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="1a134-215">Например, при передаче новой сущности в метод `Add` ей присваивается состояние `Added`.</span><span class="sxs-lookup"><span data-stu-id="1a134-215">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="1a134-216">При вызове метода `SaveChanges` контекст базы данных выполняет команду SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="1a134-216">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="1a134-217">Возможны следующие состояния сущности:</span><span class="sxs-lookup"><span data-stu-id="1a134-217">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="1a134-218">`Added`: сущность еще не существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-218">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="1a134-219">Метод `SaveChanges` выполняет инструкцию INSERT.</span><span class="sxs-lookup"><span data-stu-id="1a134-219">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="1a134-220">`Unchanged`: никакие изменения сущности не сохраняются.</span><span class="sxs-lookup"><span data-stu-id="1a134-220">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="1a134-221">Сущность находится в этом состоянии при считывании из базы данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-221">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="1a134-222">`Modified`: были изменены значения некоторых или всех свойств сущности.</span><span class="sxs-lookup"><span data-stu-id="1a134-222">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="1a134-223">Метод `SaveChanges` выполняет инструкцию UPDATE.</span><span class="sxs-lookup"><span data-stu-id="1a134-223">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="1a134-224">`Deleted`: сущность отмечена для удаления.</span><span class="sxs-lookup"><span data-stu-id="1a134-224">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="1a134-225">Метод `SaveChanges` выполняет инструкцию DELETE.</span><span class="sxs-lookup"><span data-stu-id="1a134-225">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="1a134-226">`Detached`: сущность не отслеживается контекстом базы данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-226">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="1a134-227">В классическом приложении изменения состояния обычно осуществляются автоматически.</span><span class="sxs-lookup"><span data-stu-id="1a134-227">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="1a134-228">После считывания сущности и ее изменения ей автоматически присваивается состояние `Modified`.</span><span class="sxs-lookup"><span data-stu-id="1a134-228">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="1a134-229">При вызове метода `SaveChanges` создается инструкция SQL UPDATE, которая обновляет только измененные свойства.</span><span class="sxs-lookup"><span data-stu-id="1a134-229">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="1a134-230">В веб-приложении объект `DbContext`, который считывает сущность и отображает ее данные, ликвидируется после отрисовки страницы.</span><span class="sxs-lookup"><span data-stu-id="1a134-230">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="1a134-231">При вызове метода страниц `OnPostAsync` выполняется новый веб-запрос с новым экземпляром `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1a134-231">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="1a134-232">Если повторно считать сущность в этот новый контекст, таким образом будет смоделирована обработка в классическом приложении.</span><span class="sxs-lookup"><span data-stu-id="1a134-232">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="1a134-233">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="1a134-233">Update the Delete page</span></span>

<span data-ttu-id="1a134-234">В этом разделе добавляется код, реализующий настраиваемое сообщение ошибки на случай сбоя при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1a134-234">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="1a134-235">Добавьте строку, содержащую возможные сообщения об ошибке:</span><span class="sxs-lookup"><span data-stu-id="1a134-235">Add a string to contain possible error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="1a134-236">Замените метод `OnGetAsync` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="1a134-236">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="1a134-237">Приведенный выше код содержит необязательный параметр `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="1a134-237">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="1a134-238">`saveChangesError` указывает, был ли метод вызван после того, как произошел сбой при удалении объекта учащегося.</span><span class="sxs-lookup"><span data-stu-id="1a134-238">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="1a134-239">Операция удаления может завершиться сбоем из-за временных проблем с сетью.</span><span class="sxs-lookup"><span data-stu-id="1a134-239">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="1a134-240">Вероятность возникновения временных проблем с сетью выше в облаке.</span><span class="sxs-lookup"><span data-stu-id="1a134-240">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="1a134-241">`saveChangesError` имеет значение false при вызове `OnGetAsync` страницы Delete из пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="1a134-241">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="1a134-242">Если `OnGetAsync` вызывается методом `OnPostAsync` (из-за сбоя операции удаления), параметру `saveChangesError` присваивается значение true.</span><span class="sxs-lookup"><span data-stu-id="1a134-242">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="1a134-243">Метод OnPostAsync страницы Delete</span><span class="sxs-lookup"><span data-stu-id="1a134-243">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="1a134-244">Замените `OnPostAsync` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="1a134-244">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="1a134-245">Приведенный выше код извлекает выбранную сущность и вызывает метод `Remove`, чтобы присвоить ей состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="1a134-245">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="1a134-246">При вызове метода `SaveChanges` создается инструкция SQL DELETE.</span><span class="sxs-lookup"><span data-stu-id="1a134-246">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="1a134-247">В случае сбоя `Remove`:</span><span class="sxs-lookup"><span data-stu-id="1a134-247">If `Remove` fails:</span></span>

* <span data-ttu-id="1a134-248">Вызывается исключение базы данных.</span><span class="sxs-lookup"><span data-stu-id="1a134-248">The DB exception is caught.</span></span>
* <span data-ttu-id="1a134-249">Вызывается метод `OnGetAsync` страницы Delete с параметром `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="1a134-249">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="1a134-250">Обновление страницы удаления Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1a134-250">Update the Delete Razor Page</span></span>

<span data-ttu-id="1a134-251">Добавьте выделенное ниже сообщение об ошибке на страницу Delete Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1a134-251">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="1a134-252">Проверьте удаление.</span><span class="sxs-lookup"><span data-stu-id="1a134-252">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="1a134-253">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="1a134-253">Common errors</span></span>

<span data-ttu-id="1a134-254">Не работает ссылка Student/Home или другие ссылки:</span><span class="sxs-lookup"><span data-stu-id="1a134-254">Student/Home or other links don't work:</span></span>

<span data-ttu-id="1a134-255">Убедитесь, что на странице Razor Pages содержится правильная директива `@page`.</span><span class="sxs-lookup"><span data-stu-id="1a134-255">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="1a134-256">Например, страница Razor Pages Student/Home **не должна** содержать шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="1a134-256">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="1a134-257">Каждая страница Razor Pages должна содержать директиву `@page`.</span><span class="sxs-lookup"><span data-stu-id="1a134-257">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1a134-258">[Назад](xref:data/ef-rp/intro)
[Вперед](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="1a134-258">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
