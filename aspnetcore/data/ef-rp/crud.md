---
title: Razor Pages с EF Core в ASP.NET Core — CRUD — 2 из 8
author: rick-anderson
description: Описание операций создания, чтения, обновления и удаления в EF Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/crud
ms.openlocfilehash: adb281277599456356251c6ee30772a25f14ac58
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346058"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="8fb62-103">Razor Pages с EF Core в ASP.NET Core — CRUD — 2 из 8</span><span class="sxs-lookup"><span data-stu-id="8fb62-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8fb62-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="8fb62-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="8fb62-105">В этом учебнике описывается проверка и настройка шаблонного кода операций CRUD (создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="8fb62-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="8fb62-106">Чтобы максимально упростить этот код и сконцентрироваться на работе с EF Core, в моделях страниц в этих учебниках используется код EF Core.</span><span class="sxs-lookup"><span data-stu-id="8fb62-106">To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the page models.</span></span> <span data-ttu-id="8fb62-107">Некоторые разработчики используют уровень служб или шаблон репозитория для создания уровня абстракции между пользовательским интерфейсом (Razor Pages) и уровнем доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="8fb62-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="8fb62-108">В рамках этого руководства изучаются страницы Razor Pages в папке *Students*, предназначенные для создания, редактирования, удаления и просмотра сведений.</span><span class="sxs-lookup"><span data-stu-id="8fb62-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Students* folder are examined.</span></span>

<span data-ttu-id="8fb62-109">В шаблонном коде используется следующий шаблон для страниц создания, редактирования и удаления:</span><span class="sxs-lookup"><span data-stu-id="8fb62-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="8fb62-110">Получение и отображение запрашиваемых данных с помощью метода HTTP GET`OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="8fb62-111">Сохранение изменений в данных с помощью метода HTTP POST`OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="8fb62-112">Страницы указателя и сведений получают и отображают запрашиваемые данные с помощью метода HTTP GET`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="8fb62-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="singleordefaultasync-vs-firstordefaultasync"></a><span data-ttu-id="8fb62-113">SingleOrDefaultAsync и FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="8fb62-113">SingleOrDefaultAsync vs. FirstOrDefaultAsync</span></span>

<span data-ttu-id="8fb62-114">В созданном коде используется [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), что, как правило, предпочтительнее использования [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="8fb62-114">The generated code uses [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), which is generally preferred over [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

 <span data-ttu-id="8fb62-115">При выборке одной сущности `FirstOrDefaultAsync` демонстрирует более высокую эффективность `SingleOrDefaultAsync`:</span><span class="sxs-lookup"><span data-stu-id="8fb62-115">`FirstOrDefaultAsync` is more efficient than `SingleOrDefaultAsync` at fetching one entity:</span></span>

* <span data-ttu-id="8fb62-116">Если в коде не требуется проверять, что запрос возвращает не более одной сущности.</span><span class="sxs-lookup"><span data-stu-id="8fb62-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span>
* <span data-ttu-id="8fb62-117">`SingleOrDefaultAsync` выбирает больше данных и выполняет ненужные операции.</span><span class="sxs-lookup"><span data-stu-id="8fb62-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="8fb62-118">`SingleOrDefaultAsync` вызывает исключение при наличии нескольких сущностей, соответствующих части фильтра.</span><span class="sxs-lookup"><span data-stu-id="8fb62-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
* <span data-ttu-id="8fb62-119">`FirstOrDefaultAsync` на вызывает исключение при наличии нескольких сущностей, соответствующих части фильтра.</span><span class="sxs-lookup"><span data-stu-id="8fb62-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<a name="FindAsync"></a>

### <a name="findasync"></a><span data-ttu-id="8fb62-120">FindAsync</span><span class="sxs-lookup"><span data-stu-id="8fb62-120">FindAsync</span></span>

<span data-ttu-id="8fb62-121">Чаще всего в шаблонном коде [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) можно использовать вместо `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-121">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync`.</span></span>

<span data-ttu-id="8fb62-122">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="8fb62-122">`FindAsync`:</span></span>

* <span data-ttu-id="8fb62-123">Находит сущность с первичным ключом.</span><span class="sxs-lookup"><span data-stu-id="8fb62-123">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="8fb62-124">Если сущность с первичным ключом отслеживается контекстом, она возвращается без запроса к базе данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-124">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="8fb62-125">Является простым и быстрым.</span><span class="sxs-lookup"><span data-stu-id="8fb62-125">Is simple and concise.</span></span>
* <span data-ttu-id="8fb62-126">Оптимизирован для поиска одной сущности.</span><span class="sxs-lookup"><span data-stu-id="8fb62-126">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="8fb62-127">В некоторых ситуациях может давать преимущества в производительности, однако редко используется в обычных веб-приложениях.</span><span class="sxs-lookup"><span data-stu-id="8fb62-127">Can have perf benefits in some situations, but that rarely happens for typical web apps.</span></span>
* <span data-ttu-id="8fb62-128">Неявно использует [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) вместо [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="8fb62-128">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

<span data-ttu-id="8fb62-129">Однако если необходимо включить (`Include`) другие сущности, `FindAsync` более не будет подходить.</span><span class="sxs-lookup"><span data-stu-id="8fb62-129">But if you want to `Include` other entities, then `FindAsync` is no longer appropriate.</span></span> <span data-ttu-id="8fb62-130">Это значит, что по мере работы приложения может потребоваться отменить `FindAsync` и перейти к запросу.</span><span class="sxs-lookup"><span data-stu-id="8fb62-130">This means that you may need to abandon `FindAsync` and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="8fb62-131">Настройка страницы сведений</span><span class="sxs-lookup"><span data-stu-id="8fb62-131">Customize the Details page</span></span>

<span data-ttu-id="8fb62-132">Перейдите на страницу `Pages/Students`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-132">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="8fb62-133">Ссылки **Edit**, **Details** и **Delete** создаются [вспомогательной функцией тегов привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) в файле *Pages/Students/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8fb62-133">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

<span data-ttu-id="8fb62-134">Запустите приложение и щелкните ссылку **Details**.</span><span class="sxs-lookup"><span data-stu-id="8fb62-134">Run the app and select a **Details** link.</span></span> <span data-ttu-id="8fb62-135">URL-адрес имеет вид `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-135">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="8fb62-136">Идентификатор учащегося передается с помощью строки запроса (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="8fb62-136">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="8fb62-137">Обновите страницы Razor Pages Edit, Details и Delete так, чтобы использовался шаблон маршрута `"{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-137">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="8fb62-138">Измените директиву страницы для каждой из этих страниц c `@page` на `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-138">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="8fb62-139">Запрос к странице с шаблоном маршрута "{id:int}", который **не** включает в себя целочисленное значение маршрута, приводит к ошибке HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="8fb62-139">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="8fb62-140">Например, `http://localhost:5000/Students/Details` возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="8fb62-140">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="8fb62-141">Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:</span><span class="sxs-lookup"><span data-stu-id="8fb62-141">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="8fb62-142">Запустите приложение, щелкните ссылку Details и убедитесь, что в URL-адресе в виде данных маршрута передается идентификатор (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="8fb62-142">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="8fb62-143">Не выполняйте глобальную замену `@page` на `@page "{id:int}"`, поскольку это приведет к нарушению ссылок на страницы Home и Create.</span><span class="sxs-lookup"><span data-stu-id="8fb62-143">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="8fb62-144">Добавление связанных данных</span><span class="sxs-lookup"><span data-stu-id="8fb62-144">Add related data</span></span>

<span data-ttu-id="8fb62-145">Шаблонный код страницы указателя учащихся не включает свойство `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-145">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="8fb62-146">В этом разделе на странице Details отображается содержимое коллекции `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-146">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="8fb62-147">Метод `OnGetAsync` в файле *Pages/Students/Details.cshtml.cs* использует метод `FirstOrDefaultAsync` для извлечения одной сущности `Student`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-147">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="8fb62-148">Добавьте выделенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="8fb62-148">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="8fb62-149">Методы [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) и [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) инструктируют контекст для загрузки свойства навигации `Student.Enrollments`, а также свойства навигации `Enrollment.Course` в пределах каждой регистрации.</span><span class="sxs-lookup"><span data-stu-id="8fb62-149">The [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) and [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="8fb62-150">Эти методы более подробно рассматриваются в учебнике, посвященном чтению данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-150">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="8fb62-151">Метод [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) повышает производительность в тех сценариях, где возвращаемые сущности не обновляются в текущем контексте.</span><span class="sxs-lookup"><span data-stu-id="8fb62-151">The [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="8fb62-152">`AsNoTracking` рассматривается позднее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="8fb62-152">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="8fb62-153">Отображение связанных регистраций на странице Details</span><span class="sxs-lookup"><span data-stu-id="8fb62-153">Display related enrollments on the Details page</span></span>

<span data-ttu-id="8fb62-154">Откройте файл *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8fb62-154">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="8fb62-155">Добавьте выделенный ниже код, чтобы отобразить список регистраций:</span><span class="sxs-lookup"><span data-stu-id="8fb62-155">Add the following highlighted code to display a list of enrollments:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

<span data-ttu-id="8fb62-156">Если после вставки кода нарушаются отступы в нем, нажмите клавиши CTRL-K-D, чтобы исправить это.</span><span class="sxs-lookup"><span data-stu-id="8fb62-156">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="8fb62-157">Приведенный выше код циклически обрабатывает сущности в свойстве навигации `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-157">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="8fb62-158">Для каждой регистрации он отображает название курса и оценку.</span><span class="sxs-lookup"><span data-stu-id="8fb62-158">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="8fb62-159">Название курса извлекается из сущности Course, которая хранится в свойстве навигации `Course` сущности Enrollments.</span><span class="sxs-lookup"><span data-stu-id="8fb62-159">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="8fb62-160">Запустите приложение, выберите вкладку **Students** (Учащиеся) и щелкните ссылку **Details** (Сведения) для учащегося.</span><span class="sxs-lookup"><span data-stu-id="8fb62-160">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="8fb62-161">Отобразится список курсов и оценок для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="8fb62-161">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="8fb62-162">Обновление страницы Create</span><span class="sxs-lookup"><span data-stu-id="8fb62-162">Update the Create page</span></span>

<span data-ttu-id="8fb62-163">Измените метод `OnPostAsync` в файле *Pages/Students/Create.cshtml.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="8fb62-163">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a><span data-ttu-id="8fb62-164">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="8fb62-164">TryUpdateModelAsync</span></span>

<span data-ttu-id="8fb62-165">Проверьте код [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_):</span><span class="sxs-lookup"><span data-stu-id="8fb62-165">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="8fb62-166">В приведенном выше коде `TryUpdateModelAsync<Student>` пытается обновить объект `emptyStudent`, используя отправленные значения формы из свойства [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) в [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span><span class="sxs-lookup"><span data-stu-id="8fb62-166">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span></span> <span data-ttu-id="8fb62-167">`TryUpdateModelAsync` обновляет только перечисленные свойства (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="8fb62-167">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="8fb62-168">В предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="8fb62-168">In the preceding sample:</span></span>

* <span data-ttu-id="8fb62-169">Второй аргумент (`"student", // Prefix`) представляет собой префикс для поиска значений.</span><span class="sxs-lookup"><span data-stu-id="8fb62-169">The second argument (`"student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="8fb62-170">Задается без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="8fb62-170">It's not case sensitive.</span></span>
* <span data-ttu-id="8fb62-171">Отправленные значения формы преобразуются в типы в модели `Student` с использованием [привязки модели](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="8fb62-171">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>

### <a name="overposting"></a><span data-ttu-id="8fb62-172">Чрезмерная передача данных</span><span class="sxs-lookup"><span data-stu-id="8fb62-172">Overposting</span></span>

<span data-ttu-id="8fb62-173">В целях повышения безопасности рекомендуется использовать `TryUpdateModel` для обновления полей на основе отправленных значений, поскольку в этом случае исключается чрезмерная передача данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-173">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="8fb62-174">Например, сущность Student включает свойство `Secret`, которое веб-страница не должна обновлять или добавлять:</span><span class="sxs-lookup"><span data-stu-id="8fb62-174">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="8fb62-175">Даже если приложение не имеет поля `Secret` на странице создания или обновления Razor Pages, злоумышленник может установить значение `Secret` посредством чрезмерной передачи данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-175">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="8fb62-176">Злоумышленник может использовать такие средства, как Fiddler, или собственный код JavaScript для отправки значения формы `Secret`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-176">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="8fb62-177">В исходном коде не ограничиваются поля, которые используются при создании экземпляра Student связывателем модели.</span><span class="sxs-lookup"><span data-stu-id="8fb62-177">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="8fb62-178">Какое бы значение ни задал злоумышленник для поля формы `Secret`, оно будет обновлено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-178">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="8fb62-179">На следующем рисунке показано средство Fiddler, с помощью которого в отправленные значения формы добавляется поле `Secret` (со значением "OverPost").</span><span class="sxs-lookup"><span data-stu-id="8fb62-179">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Добавление поля Secret с помощью средства Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="8fb62-181">Значение "OverPost" успешно добавлено в свойство `Secret` вставленной строки.</span><span class="sxs-lookup"><span data-stu-id="8fb62-181">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="8fb62-182">Разработчик приложения не планировал, что свойство `Secret` будет устанавливаться на странице Create.</span><span class="sxs-lookup"><span data-stu-id="8fb62-182">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>

### <a name="view-model"></a><span data-ttu-id="8fb62-183">Модель представления</span><span class="sxs-lookup"><span data-stu-id="8fb62-183">View model</span></span>

<span data-ttu-id="8fb62-184">Модель представления обычно содержит подмножество свойств, которые включены в модель и используются приложением.</span><span class="sxs-lookup"><span data-stu-id="8fb62-184">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="8fb62-185">Модель приложения часто называют моделью домена.</span><span class="sxs-lookup"><span data-stu-id="8fb62-185">The application model is often called the domain model.</span></span> <span data-ttu-id="8fb62-186">Модель домена обычно содержит все свойства, необходимые для соответствующей сущности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-186">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="8fb62-187">Модель представления содержит только те свойства, которые необходимы уровню пользовательского интерфейса (например, на странице Create).</span><span class="sxs-lookup"><span data-stu-id="8fb62-187">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="8fb62-188">Помимо модели представления в некоторых приложениях используется модель привязки или модель ввода для передачи данных между классом модели страницы Razor Pages и браузером.</span><span class="sxs-lookup"><span data-stu-id="8fb62-188">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="8fb62-189">Рассмотрим следующую модель представления `Student`:</span><span class="sxs-lookup"><span data-stu-id="8fb62-189">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

<span data-ttu-id="8fb62-190">Модели представления реализуют альтернативный подход к защите от чрезмерной передачи данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-190">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="8fb62-191">Модель представления содержит только свойства для просмотра (отображения) или обновления.</span><span class="sxs-lookup"><span data-stu-id="8fb62-191">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="8fb62-192">В следующем коде модель представления используется `StudentVM` для создания нового учащегося:</span><span class="sxs-lookup"><span data-stu-id="8fb62-192">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="8fb62-193">Метод [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) устанавливает значения этого объекта, считывая значения из другого объекта [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues).</span><span class="sxs-lookup"><span data-stu-id="8fb62-193">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="8fb62-194">`SetValues` использует сопоставление имен свойств.</span><span class="sxs-lookup"><span data-stu-id="8fb62-194">`SetValues` uses property name matching.</span></span> <span data-ttu-id="8fb62-195">Тип модели представления может быть не связан с типом модели, однако они должны содержать совпадающие свойства.</span><span class="sxs-lookup"><span data-stu-id="8fb62-195">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="8fb62-196">При использовании `StudentVM` необходимо обновить [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml), чтобы использовать `StudentVM` вместо `Student`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-196">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="8fb62-197">В Razor Pages представление модели реализуется с помощью производного класса `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-197">In Razor Pages, the `PageModel` derived class is the view model.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="8fb62-198">Обновление страницы редактирования</span><span class="sxs-lookup"><span data-stu-id="8fb62-198">Update the Edit page</span></span>

<span data-ttu-id="8fb62-199">Обновите модель страницы Edit.</span><span class="sxs-lookup"><span data-stu-id="8fb62-199">Update the page model for the Edit page.</span></span> <span data-ttu-id="8fb62-200">Основные изменения выделены:</span><span class="sxs-lookup"><span data-stu-id="8fb62-200">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="8fb62-201">Изменения в коде аналогичны странице Create за некоторыми исключениями:</span><span class="sxs-lookup"><span data-stu-id="8fb62-201">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="8fb62-202">`OnPostAsync` имеет необязательный параметр `id`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-202">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="8fb62-203">Текущий учащийся извлекается из базы данных вместо того, чтобы создавать нового учащегося.</span><span class="sxs-lookup"><span data-stu-id="8fb62-203">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="8fb62-204">`FirstOrDefaultAsync` заменен на [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span><span class="sxs-lookup"><span data-stu-id="8fb62-204">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span></span> <span data-ttu-id="8fb62-205">`FindAsync` лучше использовать при выборе сущности из первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="8fb62-205">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="8fb62-206">Дополнительные сведения см. в разделе [FindAsync](#FindAsync).</span><span class="sxs-lookup"><span data-stu-id="8fb62-206">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="8fb62-207">Проверка страниц редактирования и создания</span><span class="sxs-lookup"><span data-stu-id="8fb62-207">Test the Edit and Create pages</span></span>

<span data-ttu-id="8fb62-208">Создайте и измените несколько сущностей учащихся.</span><span class="sxs-lookup"><span data-stu-id="8fb62-208">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="8fb62-209">Состояния сущностей</span><span class="sxs-lookup"><span data-stu-id="8fb62-209">Entity States</span></span>

<span data-ttu-id="8fb62-210">Контекст базы данных отслеживает синхронизацию сущностей в памяти с соответствующими им строками в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-210">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="8fb62-211">Сведения о синхронизации контекста базы данных определяют поведение при вызове метода [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="8fb62-211">The DB context sync information determines what happens when [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span> <span data-ttu-id="8fb62-212">Например, при передаче новой сущности в метод [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) ей присваивается состояние [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span><span class="sxs-lookup"><span data-stu-id="8fb62-212">For example, when a new entity is passed to the [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) method, that entity's state is set to [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span></span> <span data-ttu-id="8fb62-213">При вызове метода `SaveChangesAsync` контекст базы данных выполняет команду SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="8fb62-213">When `SaveChangesAsync` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="8fb62-214">Возможны следующие [состояния сущности](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span><span class="sxs-lookup"><span data-stu-id="8fb62-214">An entity may be in one of the [following states](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span></span>

* <span data-ttu-id="8fb62-215">`Added`: сущность еще не существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-215">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="8fb62-216">Метод `SaveChanges` выполняет инструкцию INSERT.</span><span class="sxs-lookup"><span data-stu-id="8fb62-216">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="8fb62-217">`Unchanged`: изменения сущности не сохраняются.</span><span class="sxs-lookup"><span data-stu-id="8fb62-217">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="8fb62-218">Сущность находится в этом состоянии при считывании из базы данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-218">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="8fb62-219">`Modified`: Были изменены значения некоторых или всех свойств сущности.</span><span class="sxs-lookup"><span data-stu-id="8fb62-219">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="8fb62-220">Метод `SaveChanges` выполняет инструкцию UPDATE.</span><span class="sxs-lookup"><span data-stu-id="8fb62-220">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="8fb62-221">`Deleted`: Сущность отмечена для удаления.</span><span class="sxs-lookup"><span data-stu-id="8fb62-221">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="8fb62-222">Метод `SaveChanges` выполняет инструкцию DELETE.</span><span class="sxs-lookup"><span data-stu-id="8fb62-222">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="8fb62-223">`Detached`: сущность не отслеживается контекстом базы данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-223">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="8fb62-224">В классическом приложении изменения состояния обычно осуществляются автоматически.</span><span class="sxs-lookup"><span data-stu-id="8fb62-224">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="8fb62-225">После считывания сущности и ее изменения ей автоматически присваивается состояние `Modified`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-225">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="8fb62-226">При вызове метода `SaveChanges` создается инструкция SQL UPDATE, которая обновляет только измененные свойства.</span><span class="sxs-lookup"><span data-stu-id="8fb62-226">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="8fb62-227">В веб-приложении объект `DbContext`, который считывает сущность и отображает ее данные, ликвидируется после отрисовки страницы.</span><span class="sxs-lookup"><span data-stu-id="8fb62-227">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="8fb62-228">При вызове метода страницы `OnPostAsync` выполняется новый веб-запрос с новым экземпляром `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-228">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="8fb62-229">Если повторно считать сущность в этот новый контекст, таким образом будет смоделирована обработка в классическом приложении.</span><span class="sxs-lookup"><span data-stu-id="8fb62-229">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="8fb62-230">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="8fb62-230">Update the Delete page</span></span>

<span data-ttu-id="8fb62-231">В этом разделе добавляется код, реализующий настраиваемое сообщение ошибки на случай сбоя при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-231">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="8fb62-232">Добавьте строку, содержащую возможные сообщения об ошибке:</span><span class="sxs-lookup"><span data-stu-id="8fb62-232">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="8fb62-233">Замените метод `OnGetAsync` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8fb62-233">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="8fb62-234">Приведенный выше код содержит необязательный параметр `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-234">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="8fb62-235">`saveChangesError` указывает, был ли метод вызван после того, как произошел сбой при удалении объекта учащегося.</span><span class="sxs-lookup"><span data-stu-id="8fb62-235">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="8fb62-236">Операция удаления может завершиться сбоем из-за временных проблем с сетью.</span><span class="sxs-lookup"><span data-stu-id="8fb62-236">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="8fb62-237">Вероятность возникновения временных проблем с сетью выше в облаке.</span><span class="sxs-lookup"><span data-stu-id="8fb62-237">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="8fb62-238">`saveChangesError` имеет значение false при вызове `OnGetAsync` страницы Delete из пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="8fb62-238">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="8fb62-239">Если `OnGetAsync` вызывается методом `OnPostAsync` (из-за сбоя операции удаления), параметру `saveChangesError` присваивается значение true.</span><span class="sxs-lookup"><span data-stu-id="8fb62-239">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="8fb62-240">Метод OnPostAsync страницы Delete</span><span class="sxs-lookup"><span data-stu-id="8fb62-240">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="8fb62-241">Замените `OnPostAsync` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8fb62-241">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="8fb62-242">Приведенный выше код извлекает выбранную сущность и вызывает метод [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_), чтобы присвоить ей состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-242">The preceding code retrieves the selected entity, then calls the [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="8fb62-243">При вызове метода `SaveChanges` создается инструкция SQL DELETE.</span><span class="sxs-lookup"><span data-stu-id="8fb62-243">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="8fb62-244">В случае сбоя `Remove`:</span><span class="sxs-lookup"><span data-stu-id="8fb62-244">If `Remove` fails:</span></span>

* <span data-ttu-id="8fb62-245">Вызывается исключение базы данных.</span><span class="sxs-lookup"><span data-stu-id="8fb62-245">The DB exception is caught.</span></span>
* <span data-ttu-id="8fb62-246">Вызывается метод `OnGetAsync` страницы Delete с параметром `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-246">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="8fb62-247">Обновление страницы удаления Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8fb62-247">Update the Delete Razor Page</span></span>

<span data-ttu-id="8fb62-248">Добавьте выделенное ниже сообщение об ошибке на страницу Delete Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8fb62-248">Add the following highlighted error message to the Delete Razor Page.</span></span>
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="8fb62-249">Проверьте удаление.</span><span class="sxs-lookup"><span data-stu-id="8fb62-249">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="8fb62-250">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="8fb62-250">Common errors</span></span>

<span data-ttu-id="8fb62-251">Не работает ссылка Students/Index или другие ссылки:</span><span class="sxs-lookup"><span data-stu-id="8fb62-251">Students/Index or other links don't work:</span></span>

<span data-ttu-id="8fb62-252">Убедитесь, что на странице Razor Pages содержится правильная директива `@page`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-252">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="8fb62-253">Например, страница Razor Pages Students/Index **не должна** содержать шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="8fb62-253">For example, The Students/Index Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="8fb62-254">Каждая страница Razor Pages должна содержать директиву `@page`.</span><span class="sxs-lookup"><span data-stu-id="8fb62-254">Each Razor Page must include the `@page` directive.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8fb62-255">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8fb62-255">Additional resources</span></span>

* [<span data-ttu-id="8fb62-256">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="8fb62-256">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&list=PLnVvOWNfuhGV45HnNgd0vuIIkoQ1UaVBf)

> [!div class="step-by-step"]
> <span data-ttu-id="8fb62-257">[Назад](xref:data/ef-rp/intro)
> [Вперед](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="8fb62-257">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
