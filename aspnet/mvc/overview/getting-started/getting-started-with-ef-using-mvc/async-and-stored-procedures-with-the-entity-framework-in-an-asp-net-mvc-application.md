---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Учебник. Использование ключевых слов async и хранимые процедуры с EF в приложении MVC ASP.NET
description: В этом учебнике вы научитесь реализовать асинхронную модель программирования и узнайте, как использовать хранимые процедуры.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 0896664174bc2fee65b73ecf256d994f2abacc0a
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444367"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="9a5d4-103">Учебник. Использование ключевых слов async и хранимые процедуры с EF в приложении MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9a5d4-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="9a5d4-104">В предыдущих учебниках вы узнали, как для чтения и обновления данных с помощью синхронной модели программирования.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="9a5d4-105">В данном учебном курсе реализации асинхронной модели программирования.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="9a5d4-106">Асинхронный код может помочь приложения работают лучше, так как он повышает эффективность использования ресурсов сервера.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="9a5d4-107">В этом руководстве вы также см. в разделе способы использования хранимых процедур для вставки, обновления и удаления сущности.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="9a5d4-108">Наконец повторное развертывание приложения в Azure, а также все изменения базы данных, реализованные с момента при первом развертывании.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="9a5d4-109">На следующих рисунках изображены некоторые из страниц, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Страница отделов](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Создание подразделения](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="9a5d4-112">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9a5d4-113">Дополнительные сведения о асинхронного кода</span><span class="sxs-lookup"><span data-stu-id="9a5d4-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="9a5d4-114">Создание контроллера Department</span><span class="sxs-lookup"><span data-stu-id="9a5d4-114">Create a Department controller</span></span>
> * <span data-ttu-id="9a5d4-115">Использование хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="9a5d4-115">Use stored procedures</span></span>
> * <span data-ttu-id="9a5d4-116">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="9a5d4-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a5d4-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="9a5d4-117">Prerequisites</span></span>

* [<span data-ttu-id="9a5d4-118">Обновление связанных данных</span><span class="sxs-lookup"><span data-stu-id="9a5d4-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="9a5d4-119">Зачем использовать асинхронный код</span><span class="sxs-lookup"><span data-stu-id="9a5d4-119">Why use asynchronous code</span></span>

<span data-ttu-id="9a5d4-120">Веб-сервер имеет ограниченное число потоков, поэтому при высокой загрузке могут использоваться все доступные потоки.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="9a5d4-121">В таких случаях сервер не может обрабатывать новые запросы до тех пор, пока не будут высвобождены потоки.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="9a5d4-122">В синхронном коде многие потоки могут быть заняты, не выполняя при этом какие-либо операции и ожидая завершения ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="9a5d4-123">В асинхронном коде в то время, когда процесс ожидает завершения ввода-вывода, его поток высвобождается и может использоваться сервером для обработки других запросов.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="9a5d4-124">Таким образом асинхронный код позволяет ресурсы сервера для более эффективного использования, а на сервере включено обрабатывать больше трафика без задержек.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="9a5d4-125">В более ранних версиях .NET, написании и тестировании асинхронного кода была сложной задачей, ошибкам, поэтому сложно отлаживать.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="9a5d4-126">В .NET 4.5 гораздо проще, что следует обычно писать асинхронный код при отсутствии причины не написания, тестирования и отладки асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="9a5d4-127">Асинхронный код вводят немного больше служебных ресурсов, но ситуациях низким трафиком, снижение производительности пренебречь большого объема трафика, является существенным потенциальное улучшение производительности.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="9a5d4-128">Дополнительные сведения об асинхронном программировании см. в разделе [Поддержка асинхронного использования .NET 4.5 во избежание блокирующих вызовов](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="9a5d4-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="9a5d4-129">Создание контроллера Department</span><span class="sxs-lookup"><span data-stu-id="9a5d4-129">Create Department controller</span></span>

<span data-ttu-id="9a5d4-130">Создание контроллера Department, так же, как более ранних контроллеров, но в этом случае выберите **использовать асинхронные действия контроллера** "флажок".</span><span class="sxs-lookup"><span data-stu-id="9a5d4-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="9a5d4-131">Ниже кратко описан Показать, что был добавлен на синхронный код `Index` для того, асинхронные:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="9a5d4-132">Четыре изменения были применены для включения асинхронного запроса к базе данных Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="9a5d4-133">Метод помечен атрибутом `async` ключевое слово, которое указывает компилятору создавать обратные вызовы для частей тела метода и автоматически создавать `Task<ActionResult>` возвращаемый объект.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="9a5d4-134">Возвращаемый тип был изменен с `ActionResult` для `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="9a5d4-135">`Task<T>` Тип представляет текущую операцию с помощью результата типа `T`.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="9a5d4-136">`await` Ключевое слово была применена для вызова веб-службы.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="9a5d4-137">Когда компилятор обнаруживает ключевое слово, за кулисами он разделяет метод на две части.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="9a5d4-138">Первая часть завершается операцией, который запускается в асинхронном режиме.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="9a5d4-139">Вторая часть помещается в метод обратного вызова, вызываемый при завершении операции.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="9a5d4-140">Асинхронная версия `ToList` был вызван метод расширения.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="9a5d4-141">Почему является `departments.ToList` инструкции изменения, но не `departments = db.Departments` инструкции?</span><span class="sxs-lookup"><span data-stu-id="9a5d4-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="9a5d4-142">Причина в том, что только инструкции, вызывающие запросы или команды, отправляемые в базу данных будут выполняться асинхронно.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="9a5d4-143">`departments = db.Departments` Устанавливает инструкцию запроса, но запрос не выполняется до `ToList` вызывается метод.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="9a5d4-144">Таким образом только `ToList` метод выполняется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="9a5d4-145">В `Details` метод и `HttpGet` `Edit` и `Delete` методы, `Find` метод является тот, который завершает запрос, отправляемые в базу данных, это и есть метод, который выполняется асинхронно:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="9a5d4-146">В `Create`, `HttpPost Edit`, и `DeleteConfirmed` это методы, `SaveChanges` вызов метода, который вызывает команду для выполнения, не операторы, например `db.Departments.Add(department)` чему сущности только в памяти, чтобы изменить.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="9a5d4-147">Откройте *Views\Department\Index.cshtml*и замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="9a5d4-148">Этот код изменяет заголовок из индекса в отделах, перемещает имя администратора справа и предоставляет полное имя администратора.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="9a5d4-149">В создания, удаления, просмотра сведений и изменение представлений, изменить заголовок для `InstructorID` поле «Администратор» так же, как вы изменили поле имя отдела «Отдел» в представлениях курса.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="9a5d4-150">В Create и Edit представления используйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="9a5d4-151">В представлениях Delete и Details используйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="9a5d4-152">Запустите приложение и нажмите кнопку **отделов** вкладки.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="9a5d4-153">Все работает так же, как и в других контроллеров, но в этом контроллере все SQL-запросы выполняются асинхронно.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="9a5d4-154">Некоторые моменты, которые следует учитывать при использовании асинхронного программирования с Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="9a5d4-155">Асинхронный код не является потокобезопасным.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-155">The async code is not thread safe.</span></span> <span data-ttu-id="9a5d4-156">Другими словами другими словами, не следует пытаться выполнять несколько операций параллельно, используя один и тот же экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="9a5d4-157">Если вы хотите использовать преимущества в производительности, которые обеспечивает асинхронный код, убедитесь, что все используемые пакеты библиотек (например, для разбиения на страницы) также используют асинхронный код при вызове любых методов Entity Framework, выполняющих запросы к базе данных.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="9a5d4-158">Использование хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="9a5d4-158">Use stored procedures</span></span>

<span data-ttu-id="9a5d4-159">Некоторые разработчики и Администраторы баз данных предпочитает использовать хранимые процедуры для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="9a5d4-160">В более ранних версиях Entity Framework можно извлекать данные с помощью хранимой процедуры с [выполнение необработанный SQL-запрос](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), но нельзя указать EF использование хранимых процедур для операций обновления.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="9a5d4-161">В EF 6 можно легко настроить Code First с помощью хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="9a5d4-162">В *DAL\SchoolContext.cs*, добавьте выделенный код, который `OnModelCreating` метод.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="9a5d4-163">Этот код указывает, что платформа Entity Framework для использования хранимых процедур для вставки, обновления и удаления операций на `Department` сущности.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="9a5d4-164">В консоли управления пакета введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="9a5d4-165">Откройте *миграций\&lt; метка времени&gt;\_DepartmentSP.cs* код в `Up` метод, который создает вставки, обновления и удаления хранимых процедур:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-165">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="9a5d4-166">В консоли управления пакета введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9a5d4-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="9a5d4-167">Запустите приложение в режиме отладки, нажмите кнопку **отделов** , а затем щелкните **Create New**.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="9a5d4-168">Введите данные для нового отдела, а затем нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="9a5d4-169">В Visual Studio, просмотрите журналы в **вывода** окно, чтобы увидеть, что хранимая процедура использовалась для вставки новой строки отдела.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![Отдел Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="9a5d4-171">Код сначала создает имена по умолчанию хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="9a5d4-172">Если вы используете существующую базу данных, может потребоваться настроить имен хранимых процедур, чтобы использовать хранимые процедуры, уже определенные в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="9a5d4-173">Сведения о том, как это сделать, см. в разделе [Entity Framework код первого Insert/Update/Delete хранимые процедуры](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="9a5d4-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="9a5d4-174">Если вы хотите настроить, что созданные хранимых процедур, можно изменить сформированный код для миграций `Up` метод, который создает хранимую процедуру.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="9a5d4-175">Таким образом изменения будут отражены каждый раз, когда что миграции запускается и будет применяться в производственной базе данных, при миграции автоматически выполняется в рабочей среде после развертывания.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="9a5d4-176">Если вы хотите изменить существующую хранимую процедуру, которая была создана в предыдущей миграции, используйте команду Add-Migration, чтобы создать пустой миграции и вручную написать код, вызывающий [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) метод .</span><span class="sxs-lookup"><span data-stu-id="9a5d4-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="9a5d4-177">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="9a5d4-177">Deploy to Azure</span></span>

<span data-ttu-id="9a5d4-178">В этом разделе, необходимо завершить необязательный **развертыванию приложения в Azure** статьи [миграции и развертывания](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) руководств этой серии.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="9a5d4-179">Если у вас есть ошибки миграции, которые можно разрешить путем удаления базы данных в локальном проекте, пропустите этот раздел.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="9a5d4-180">В Visual Studio щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="9a5d4-181">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-181">Click **Publish**.</span></span>

    <span data-ttu-id="9a5d4-182">Visual Studio развертывает приложение в Azure, и приложение откроется в браузере по умолчанию, работающих в Azure.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="9a5d4-183">Протестируйте приложение, чтобы проверить его работы.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="9a5d4-184">При первом запуске страницы, который обращается к базе данных, платформа Entity Framework выполняет все миграции `Up` методы, необходимые для приведения базы данных в актуальном состоянии с текущей моделью данных.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="9a5d4-185">Теперь можно использовать все веб-страниц, добавленных с момента последнего развертывания, включая страницы подразделения, добавленные в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="9a5d4-186">Получение кода</span><span class="sxs-lookup"><span data-stu-id="9a5d4-186">Get the code</span></span>

[<span data-ttu-id="9a5d4-187">Скачивание готового проекта</span><span class="sxs-lookup"><span data-stu-id="9a5d4-187">Download the Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a><span data-ttu-id="9a5d4-188">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9a5d4-188">Additional resources</span></span>

<span data-ttu-id="9a5d4-189">Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="9a5d4-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a5d4-190">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="9a5d4-190">Next steps</span></span>

<span data-ttu-id="9a5d4-191">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9a5d4-192">Узнали о асинхронного кода</span><span class="sxs-lookup"><span data-stu-id="9a5d4-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="9a5d4-193">Создании контроллера Department</span><span class="sxs-lookup"><span data-stu-id="9a5d4-193">Created a Department controller</span></span>
> * <span data-ttu-id="9a5d4-194">Использовать хранимые процедуры</span><span class="sxs-lookup"><span data-stu-id="9a5d4-194">Used stored procedures</span></span>
> * <span data-ttu-id="9a5d4-195">Развертывания в Azure</span><span class="sxs-lookup"><span data-stu-id="9a5d4-195">Deployed to Azure</span></span>

<span data-ttu-id="9a5d4-196">Перейдите к следующей статье, чтобы узнать, как обрабатывать конфликты, когда несколько пользователей изменяют одну сущность, в то же время.</span><span class="sxs-lookup"><span data-stu-id="9a5d4-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="9a5d4-197">Обработка параллелизма</span><span class="sxs-lookup"><span data-stu-id="9a5d4-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
