---
title: "Страниц Razor с основными EF - CRUD - 2, 8."
author: rick-anderson
description: "Показано, как создание, чтение, обновление, удаление с основными EF"
keywords: "ASP.NET Core, Entity Framework Core, CRUD, создание, чтение, обновление, удаление"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 163bc35afed0bf1d9236935d5ce60e6975356594
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="5c4e0-104">Создания, чтения, обновления и удаления - Core EF со страницами Razor (2, 8)</span><span class="sxs-lookup"><span data-stu-id="5c4e0-104">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="5c4e0-105">По [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5c4e0-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="5c4e0-106">В этом учебнике формирования шаблонов CRUD (Создание, чтение, обновление и удаление) просматривается и настроить код.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="5c4e0-107">Примечание: Чтобы уменьшить сложность и сохранить эти учебники, основное внимание уделено EF Core, EF основного кода используется в файлах кода страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-107">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages code-behind files.</span></span> <span data-ttu-id="5c4e0-108">Некоторые разработчики используют шаблон службы уровня или репозитория в создание уровень абстракции между пользовательского интерфейса (страниц Razor) и уровень доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="5c4e0-109">В этот учебник, создания, изменения, удаления и страниц Razor сведения в *студента* папки будут изменены.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="5c4e0-110">Создание, изменение и удаление страниц, формирования шаблонов кода строится по следующему шаблону:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="5c4e0-111">Получение и отображение запрошенные данные с помощью метода HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="5c4e0-112">Сохранить изменения в данных с помощью метода HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="5c4e0-113">На страницах индекса и сведения, получения и отображения запрошенные данные с помощью метода HTTP GET`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="5c4e0-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="5c4e0-114">Замените SingleOrDefaultAsync FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="5c4e0-114">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="5c4e0-115">Созданный код использует [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) для выборки Запрошенная сущность.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-115">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="5c4e0-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) более эффективен при выборке одной сущности:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="5c4e0-117">Если код должен проверить, не более одной сущности, возвращенных запросом.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-117">Unless the code needs to verify that there is not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="5c4e0-118">`SingleOrDefaultAsync`данные извлекаются и выполняет лишнюю работу.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="5c4e0-119">`SingleOrDefaultAsync`создает исключение, если имеется более одной сущности, которая соответствует часть фильтра.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-119">`SingleOrDefaultAsync` throws an exception if there is more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="5c4e0-120">`FirstOrDefaultAsync`не создавать при наличии более одной сущности, которая соответствует часть фильтра.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-120">`FirstOrDefaultAsync` doesn't throw if there is more than one entity that fits the filter part.</span></span>

<span data-ttu-id="5c4e0-121">Замените глобально `SingleOrDefaultAsync` с `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-121">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="5c4e0-122">`SingleOrDefaultAsync`используется в 5 местах:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-122">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="5c4e0-123">`OnGetAsync`на странице «сведения».</span><span class="sxs-lookup"><span data-stu-id="5c4e0-123">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="5c4e0-124">`OnGetAsync`и `OnPostAsync` на страницах, Edit и Delete.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-124">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="5c4e0-125">FindAsync</span><span class="sxs-lookup"><span data-stu-id="5c4e0-125">FindAsync</span></span>

<span data-ttu-id="5c4e0-126">В основную часть формирования шаблонов кода [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) может использоваться вместо `FirstOrDefaultAsync` или `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-126">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="5c4e0-127">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-127">`FindAsync`:</span></span>

* <span data-ttu-id="5c4e0-128">Находит сущности с первичным ключом (PK).</span><span class="sxs-lookup"><span data-stu-id="5c4e0-128">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="5c4e0-129">Если сущность с PK отслеживается контекстом, она возвращается без запроса к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-129">If an entity with the PK is being tracked by the context, it is returned without a request to the DB.</span></span>
* <span data-ttu-id="5c4e0-130">— Простой и быстрый.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-130">Is simple and concise.</span></span>
* <span data-ttu-id="5c4e0-131">Оптимизирован для поиска одной сущности.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-131">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="5c4e0-132">Можно иметь преимущества производительности в некоторых ситуациях, но они редко следует учитывать для обычных веб-скриптов.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-132">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="5c4e0-133">Неявно использует [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) вместо [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="5c4e0-133">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="5c4e0-134">Но если необходимо включить другие сущности, то поиск больше не подходит.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-134">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="5c4e0-135">Это означает, что может потребоваться отменить поиск и переместить запрос по мере продвижения вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-135">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="5c4e0-136">Настройка страницы сведений</span><span class="sxs-lookup"><span data-stu-id="5c4e0-136">Customize the Details page</span></span>

<span data-ttu-id="5c4e0-137">Перейдите к `Pages/Students` страницы.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-137">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="5c4e0-138">**Изменить**, **сведения**, и **удаление** ссылки, созданные [вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) в *страниц и студенты / Index.cshtml* файл.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-138">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="5c4e0-139">Выберите ссылку сведения.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-139">Select a Details link.</span></span> <span data-ttu-id="5c4e0-140">URL-адрес имеет вид `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-140">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="5c4e0-141">Идентификатор студента передается с помощью строки запроса (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="5c4e0-141">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="5c4e0-142">Обновить редактирования, подробные сведения и удаление страниц Razor для использования `"{id:int}"` шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-142">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="5c4e0-143">Измените директиву страницы для каждой из этих страниц c `@page` на `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-143">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="5c4e0-144">Запрос на странице с помощью шаблона маршрута «{ИД: int}», выполняющий **не** включают целое число со знаком маршрута значение возвращает HTTP 404 (не найдено) ошибку.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-144">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="5c4e0-145">Например `http://localhost:5000/Students/Details` возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-145">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="5c4e0-146">Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-146">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="5c4e0-147">Запустить приложение, щелкните ссылку сведений и проверьте URL-адрес передает идентификатор как данные маршрута (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="5c4e0-147">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="5c4e0-148">Не изменяйте глобально `@page` для `@page "{id:int}"`, это нарушит ссылки на корневую папку и создавать страницы.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-148">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="5c4e0-149">Добавление связанных данных</span><span class="sxs-lookup"><span data-stu-id="5c4e0-149">Add related data</span></span>

<span data-ttu-id="5c4e0-150">Отсутствует код, формирования шаблонов для страницы индекса учащихся `Enrollments` свойство.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-150">The scaffolded code for the Students Index page does not include the `Enrollments` property.</span></span> <span data-ttu-id="5c4e0-151">В этом разделе содержимого `Enrollments` коллекция отображается на странице «сведения».</span><span class="sxs-lookup"><span data-stu-id="5c4e0-151">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="5c4e0-152">`OnGetAsync` Метод *Pages/Students/Details.cshtml.cs* использует `FirstOrDefaultAsync` метод для извлечения одной `Student` сущности.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-152">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="5c4e0-153">Добавьте следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-153">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="5c4e0-154">`Include` И `ThenInclude` методы, которые вызывают контекст загрузки `Student.Enrollments` свойство навигации и в пределах каждой регистрации `Enrollment.Course` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-154">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="5c4e0-155">Эти методы являются examinied подробно в учебнике чтение связанных данных.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-155">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="5c4e0-156">`AsNoTracking` Метод позволяет повысить производительность в сценариях, если возвращаются сущности, не обновляются в текущем контексте.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-156">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="5c4e0-157">`AsNoTracking`рассматривается далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-157">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="5c4e0-158">Отображение связанных регистраций на этой странице</span><span class="sxs-lookup"><span data-stu-id="5c4e0-158">Display related enrollments on the Details page</span></span>

<span data-ttu-id="5c4e0-159">Откройте *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-159">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="5c4e0-160">Добавьте следующий выделенный код, чтобы отобразить список регистраций.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-160">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="5c4e0-161">Если коду отступ неправильный после вставки кода, нажмите клавиши CTRL-K-D, чтобы исправить его.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="5c4e0-162">Приведенный выше код выполняет цикл по сущности в `Enrollments` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="5c4e0-163">Для каждой регистрации отображает название курса и уровень.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="5c4e0-164">Название курса извлекается из курса сущность, которая хранится в `Course` свойства навигации сущности регистраций.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="5c4e0-165">Запустите приложение, выберите **учащихся** и нажмите кнопку **сведения** ссылку для учащихся.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="5c4e0-166">Отображается список курсов и оценок для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="5c4e0-167">Обновление страницы создания</span><span class="sxs-lookup"><span data-stu-id="5c4e0-167">Update the Create page</span></span>

<span data-ttu-id="5c4e0-168">Обновление `OnPostAsync` метод в *Pages/Students/Create.cshtml.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="5c4e0-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="5c4e0-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="5c4e0-170">Изучите [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) кода:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-170">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="5c4e0-171">В приведенном выше коде `TryUpdateModelAsync<Student>` пытается обновить `emptyStudent` объекта, используя значения из отправленной формы [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) свойство в [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="5c4e0-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="5c4e0-172">`TryUpdateModelAsync`обновляет только свойства, перечисленные (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="5c4e0-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="5c4e0-173">В предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-173">In the preceding sample:</span></span>

* <span data-ttu-id="5c4e0-174">Второй аргумент (` "student", // Prefix`) — это префикс использует для поиска значений.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="5c4e0-175">Не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-175">It's not case sensitive.</span></span>
* <span data-ttu-id="5c4e0-176">Значения из отправленной формы преобразуются в типы в `Student` модели с помощью [привязки модели](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="5c4e0-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="5c4e0-177">Оверпостинга</span><span class="sxs-lookup"><span data-stu-id="5c4e0-177">Overposting</span></span>

<span data-ttu-id="5c4e0-178">С помощью `TryUpdateModel` для обновления полей с переданные значения является рекомендации по безопасности, так как он предотвращает overposting.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="5c4e0-179">Предположим, например, сущность Student включает `Secret` свойство, которое этой веб-страницы не следует обновить или добавить:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-179">For example, suppose the Student entity includes a `Secret` property that this web page should not update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="5c4e0-180">Даже если приложение не имеет `Secret` поля для создания или обновления страниц Razor, злоумышленник может установить `Secret` значение оверпостинга.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="5c4e0-181">Злоумышленник может использовать такое средство, как Fiddler или написать сценарий JavaScript для учета `Secret` формирования значения.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="5c4e0-182">Исходный код не ограничивать поля, которые использует связыватель модели при создании экземпляра студента.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="5c4e0-183">Любое другое значение злоумышленнику, указанный для `Secret` поля формы обновляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="5c4e0-184">На следующем рисунке показано добавление средство Fiddler `Secret` поле (со значением «OverPost») для значения из отправленной формы.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![При добавлении секретный поля Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="5c4e0-186">Значение «OverPost» успешно добавлен `Secret` свойство вставленной строки.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="5c4e0-187">Конструктор приложения не предназначены `Secret` установить со страницы создания это свойство.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="5c4e0-188">Модель представления</span><span class="sxs-lookup"><span data-stu-id="5c4e0-188">View model</span></span>

<span data-ttu-id="5c4e0-189">Модель представления обычно содержит подмножество свойств, включенных в модель, используемые приложением.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="5c4e0-190">Модель приложения часто называют модели домена.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-190">The application model is often called the domain model.</span></span> <span data-ttu-id="5c4e0-191">Модель домена обычно содержит все свойства, необходимые для соответствующей сущности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="5c4e0-192">Модель представления содержит только свойства, необходимые для уровня пользовательского интерфейса (например, страницы создания).</span><span class="sxs-lookup"><span data-stu-id="5c4e0-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="5c4e0-193">В дополнение к модели представления некоторые приложения используют привязки модели или модели ввода для передачи данных между классом кода программной страниц Razor и браузер.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages code-behind class and the browser.</span></span> <span data-ttu-id="5c4e0-194">Рассмотрим следующий `Student` модель представления:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-194">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="5c4e0-195">Просмотр моделей предоставляют альтернативный способ предотвратить overposting.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="5c4e0-196">Модель представления содержит только свойства для просмотра (отображение) или обновления.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="5c4e0-197">В следующем коде используется `StudentVM` модель представления для создания нового студента:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="5c4e0-198">[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) метод задает значения этого объекта, считывая значения из другого [объекты propertyvalue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) объекта.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-198">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="5c4e0-199">`SetValues`использует сопоставление по имени свойства.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="5c4e0-200">Тип модели представления может не быть связанным с типом модели, необходимо иметь свойства, которые соответствуют только.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="5c4e0-201">С помощью `StudentVM` требует [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) обновить для использования `StudentVM` вместо `Student`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="5c4e0-202">На страницах Razor `PageModel` производный класс является модели представления.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="5c4e0-203">Обновление страницы правки</span><span class="sxs-lookup"><span data-stu-id="5c4e0-203">Update the Edit page</span></span>

<span data-ttu-id="5c4e0-204">Добавьте в файл кода страницы изменения:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-204">Update the Edit page code-behind file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="5c4e0-205">Изменения в коде похожи на странице создания за некоторыми исключениями.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-205">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="5c4e0-206">`OnPostAsync`имеется необязательный `id` параметра.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-206">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="5c4e0-207">Текущий студента извлекаются из базы данных, вместо создания пустой студента.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-207">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="5c4e0-208">`FirstOrDefaultAsync`был заменен [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="5c4e0-208">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="5c4e0-209">`FindAsync`лучше всего подойдет при выборе сущности из первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-209">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="5c4e0-210">В разделе [FindAsync](#FindAsync) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-210">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="5c4e0-211">Тестирование редактирования и создания страниц</span><span class="sxs-lookup"><span data-stu-id="5c4e0-211">Test the Edit and Create pages</span></span>

<span data-ttu-id="5c4e0-212">Создание и изменение сущностей несколько учащихся.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-212">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="5c4e0-213">Состояния сущностей</span><span class="sxs-lookup"><span data-stu-id="5c4e0-213">Entity States</span></span>

<span data-ttu-id="5c4e0-214">Сохраняет сведения о контекст базы данных, являются ли сущности в памяти в соответствии с их соответствующих строк в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-214">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="5c4e0-215">Сведения о синхронизации контекста базы данных определяет, что происходит при `SaveChanges` вызывается.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-215">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="5c4e0-216">Например, при передаче объекта в `Add` метод, который присваивается состояние сущности `Added`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-216">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="5c4e0-217">Когда `SaveChanges` вызывается DB контекста выдает команду SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-217">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="5c4e0-218">Сущность может иметь одно из следующих состояний:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-218">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="5c4e0-219">`Added`: Объект еще не существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-219">`Added`: The entity does not yet exist in the DB.</span></span> <span data-ttu-id="5c4e0-220">`SaveChanges` Метод выдает инструкции INSERT.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-220">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="5c4e0-221">`Unchanged`: Никакие изменения должны быть сохранены с этой сущностью.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-221">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="5c4e0-222">Сущность находится в этом состоянии при чтении данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-222">An entity has this status when it is read from the DB.</span></span>

* <span data-ttu-id="5c4e0-223">`Modified`: Были изменены некоторые или все значения свойств сущности.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-223">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="5c4e0-224">`SaveChanges` Метод выдает инструкции UPDATE.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-224">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="5c4e0-225">`Deleted`: Сущность была помечена для удаления.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-225">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="5c4e0-226">`SaveChanges` Метод выполняет инструкцию DELETE.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-226">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="5c4e0-227">`Detached`: Объект не отслеживается контекст базы данных.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-227">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="5c4e0-228">В приложении рабочего стола изменения состояния обычно устанавливаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-228">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="5c4e0-229">Сущность для чтения, изменения и состояние сущности автоматически меняется на `Modified`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-229">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="5c4e0-230">Вызов `SaveChanges` создает инструкцию SQL UPDATE, которая обновляет только измененных свойств.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-230">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="5c4e0-231">В веб-приложении `DbContext` , осуществляющий чтение сущности и отображает данные удаляется после отрисовки страницы.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-231">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="5c4e0-232">Если страницы `OnPostAsync` вызывается метод, новый веб-запроса и для нового экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-232">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="5c4e0-233">Повторное чтение сущности в этот новый контекст имитирует обработки рабочего стола.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-233">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="5c4e0-234">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="5c4e0-234">Update the Delete page</span></span>

<span data-ttu-id="5c4e0-235">В этом разделе добавляется код для реализации пользовательской ошибки сообщение при вызове `SaveChanges` завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-235">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="5c4e0-236">Добавьте строку к possile сообщений об ошибке:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-236">Add a string to contain possile error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="5c4e0-237">Замените метод `OnGetAsync` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-237">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="5c4e0-238">Предыдущий код содержит необязательный параметр `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-238">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="5c4e0-239">`saveChangesError`Указывает, является ли метод был вызван после сбоя, чтобы удалить объект student.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-239">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="5c4e0-240">Операция удаления может завершиться ошибкой из-за временных проблем с сетью.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-240">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="5c4e0-241">В облаке скорее временный сетевой ошибки.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-241">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="5c4e0-242">`saveChangesError`ложно, когда страница удаления `OnGetAsync` вызывается из пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-242">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="5c4e0-243">Когда `OnGetAsync` вызывается `OnPostAsync` (из-за сбоя операции удаления), `saveChangesError` параметр имеет значение true.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-243">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="5c4e0-244">Метод OnPostAsync удаления страницы</span><span class="sxs-lookup"><span data-stu-id="5c4e0-244">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="5c4e0-245">Замените `OnPostAsync` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-245">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="5c4e0-246">Приведенный выше код извлекает выбранную сущность, затем вызывает метод `Remove` метод, чтобы задать состояния сущности `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-246">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="5c4e0-247">При `SaveChanges` вызове SQL DELETE команда создается.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-247">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="5c4e0-248">Если `Remove` завершается ошибкой:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-248">If `Remove` fails:</span></span>

* <span data-ttu-id="5c4e0-249">Порождено исключение базы данных.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-249">The DB exception is caught.</span></span>
* <span data-ttu-id="5c4e0-250">Удаление страниц `OnGetAsync` метод вызывается с `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-250">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="5c4e0-251">Страница «обновление» Razor Delete</span><span class="sxs-lookup"><span data-stu-id="5c4e0-251">Update the Delete Razor Page</span></span>

<span data-ttu-id="5c4e0-252">Добавьте сообщение об ошибке выделенный страницу удалить Razor.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-252">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="5c4e0-253">Проверка удаления.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-253">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="5c4e0-254">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="5c4e0-254">Common errors</span></span>

<span data-ttu-id="5c4e0-255">Домашние студентов или другие ссылки не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-255">Student/Home or other links don't work:</span></span>

<span data-ttu-id="5c4e0-256">Проверьте страницу Razor содержит правильное `@page` директивы.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-256">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="5c4e0-257">Например, студентов или домашней странице Razor следует **не** содержать шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="5c4e0-257">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="5c4e0-258">Каждая страница Razor должна включать `@page` директивы.</span><span class="sxs-lookup"><span data-stu-id="5c4e0-258">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5c4e0-259">[Назад](xref:data/ef-rp/intro)
[Вперед](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="5c4e0-259">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
