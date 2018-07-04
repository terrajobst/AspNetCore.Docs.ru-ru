---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Создание REST API с помощью маршрутизации в ASP.NET Web API 2 с помощью атрибутов | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 20538348504427c30d5d75705271a5c3c3c2c171
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362224"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="2595d-102">Создание REST API с помощью маршрутизации атрибутов в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="2595d-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="2595d-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2595d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2595d-104">Веб-API 2 поддерживает новый тип маршрутизации, вызывается *маршрутизации с помощью атрибутов*.</span><span class="sxs-lookup"><span data-stu-id="2595d-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="2595d-105">Общие сведения о маршрутизации с помощью атрибутов, см. в разделе [маршрутизации с помощью атрибутов в веб-API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="2595d-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="2595d-106">В этом руководстве используется маршрутизация с помощью атрибутов для создания API REST для коллекции книг.</span><span class="sxs-lookup"><span data-stu-id="2595d-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="2595d-107">API-Интерфейс поддерживает следующие действия:</span><span class="sxs-lookup"><span data-stu-id="2595d-107">The API will support the following actions:</span></span>

| <span data-ttu-id="2595d-108">Действие</span><span class="sxs-lookup"><span data-stu-id="2595d-108">Action</span></span> | <span data-ttu-id="2595d-109">Пример URI</span><span class="sxs-lookup"><span data-stu-id="2595d-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="2595d-110">Получение списка всех книг.</span><span class="sxs-lookup"><span data-stu-id="2595d-110">Get a list of all books.</span></span> | <span data-ttu-id="2595d-111">/ api/книг</span><span class="sxs-lookup"><span data-stu-id="2595d-111">/api/books</span></span> |
| <span data-ttu-id="2595d-112">Получите книгу по идентификатору.</span><span class="sxs-lookup"><span data-stu-id="2595d-112">Get a book by ID.</span></span> | <span data-ttu-id="2595d-113">/API/Books/1</span><span class="sxs-lookup"><span data-stu-id="2595d-113">/api/books/1</span></span> |
| <span data-ttu-id="2595d-114">Получение сведений о книге.</span><span class="sxs-lookup"><span data-stu-id="2595d-114">Get the details of a book.</span></span> | <span data-ttu-id="2595d-115">/API/Books/1/Details</span><span class="sxs-lookup"><span data-stu-id="2595d-115">/api/books/1/details</span></span> |
| <span data-ttu-id="2595d-116">Получение списка книг по жанру.</span><span class="sxs-lookup"><span data-stu-id="2595d-116">Get a list of books by genre.</span></span> | <span data-ttu-id="2595d-117">/API/Books/FANTASY</span><span class="sxs-lookup"><span data-stu-id="2595d-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="2595d-118">Получение списка книг по дате публикации.</span><span class="sxs-lookup"><span data-stu-id="2595d-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="2595d-119">/API/Books/Date/2013-02-16 /api/books/date/2013/02/16 (альтернативный способ)</span><span class="sxs-lookup"><span data-stu-id="2595d-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="2595d-120">Получение списка книг определенного автора.</span><span class="sxs-lookup"><span data-stu-id="2595d-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="2595d-121">/API/authors/1/Books</span><span class="sxs-lookup"><span data-stu-id="2595d-121">/api/authors/1/books</span></span> |

<span data-ttu-id="2595d-122">Все методы являются только для чтения (HTTP-запросы GET).</span><span class="sxs-lookup"><span data-stu-id="2595d-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="2595d-123">Уровень данных мы будем использовать Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2595d-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="2595d-124">Записи книги будет иметь следующие поля:</span><span class="sxs-lookup"><span data-stu-id="2595d-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="2595d-125">ID</span><span class="sxs-lookup"><span data-stu-id="2595d-125">ID</span></span>
- <span data-ttu-id="2595d-126">Заголовок</span><span class="sxs-lookup"><span data-stu-id="2595d-126">Title</span></span>
- <span data-ttu-id="2595d-127">Жанра</span><span class="sxs-lookup"><span data-stu-id="2595d-127">Genre</span></span>
- <span data-ttu-id="2595d-128">Дата публикации</span><span class="sxs-lookup"><span data-stu-id="2595d-128">Publication date</span></span>
- <span data-ttu-id="2595d-129">Цена</span><span class="sxs-lookup"><span data-stu-id="2595d-129">Price</span></span>
- <span data-ttu-id="2595d-130">Описание:</span><span class="sxs-lookup"><span data-stu-id="2595d-130">Description</span></span>
- <span data-ttu-id="2595d-131">AuthorID (внешний ключ к таблице Authors)</span><span class="sxs-lookup"><span data-stu-id="2595d-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="2595d-132">Большинство запросов тем не менее, API вернет подмножество этих данных ("Заголовок", "Автор" и "genre").</span><span class="sxs-lookup"><span data-stu-id="2595d-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="2595d-133">Чтобы получить полную запись клиента запросы `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="2595d-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2595d-134">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="2595d-134">Prerequisites</span></span>

<span data-ttu-id="2595d-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional или Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="2595d-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="2595d-136">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2595d-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="2595d-137">Запустить Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2595d-137">Start by running Visual Studio.</span></span> <span data-ttu-id="2595d-138">Из **файл** меню, выберите **New** , а затем выберите **проекта**.</span><span class="sxs-lookup"><span data-stu-id="2595d-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="2595d-139">В **шаблоны** области выберите **установленные шаблоны** и разверните **Visual C#** узла.</span><span class="sxs-lookup"><span data-stu-id="2595d-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2595d-140">В разделе **Visual C#** выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="2595d-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="2595d-141">В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="2595d-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="2595d-142">Назовите проект &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="2595d-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="2595d-143">В **новый проект ASP.NET** диалоговом окне выберите **пустой** шаблона.</span><span class="sxs-lookup"><span data-stu-id="2595d-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="2595d-144">В разделе «Добавление папок и основные ссылки для» выберите **веб-API** флажок.</span><span class="sxs-lookup"><span data-stu-id="2595d-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="2595d-145">Нажмите кнопку **Создание проекта**.</span><span class="sxs-lookup"><span data-stu-id="2595d-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="2595d-146">Это создает схему проекта, который настроен для функциональных возможностей веб-API.</span><span class="sxs-lookup"><span data-stu-id="2595d-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="2595d-147">Модели домена</span><span class="sxs-lookup"><span data-stu-id="2595d-147">Domain Models</span></span>

<span data-ttu-id="2595d-148">Добавьте классы для модели домена.</span><span class="sxs-lookup"><span data-stu-id="2595d-148">Next, add classes for domain models.</span></span> <span data-ttu-id="2595d-149">В обозревателе решений щелкните правой кнопкой мыши папку Models.</span><span class="sxs-lookup"><span data-stu-id="2595d-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="2595d-150">Выберите **добавить**, а затем выберите **класс**.</span><span class="sxs-lookup"><span data-stu-id="2595d-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="2595d-151">Присвойте классу имя `Author`.</span><span class="sxs-lookup"><span data-stu-id="2595d-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="2595d-152">Замените код в Author.cs следующее:</span><span class="sxs-lookup"><span data-stu-id="2595d-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="2595d-153">Теперь добавьте класс с именем `Book`.</span><span class="sxs-lookup"><span data-stu-id="2595d-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="2595d-154">Добавить контроллер веб-API</span><span class="sxs-lookup"><span data-stu-id="2595d-154">Add a Web API Controller</span></span>

<span data-ttu-id="2595d-155">На этом этапе мы добавим контроллер Web API, использующий Entity Framework в качестве уровня данных.</span><span class="sxs-lookup"><span data-stu-id="2595d-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="2595d-156">Для сборки проекта нажмите CTRL+SHIFT+B.</span><span class="sxs-lookup"><span data-stu-id="2595d-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="2595d-157">Платформа Entity Framework использует отражение для обнаружения свойства моделей, поэтому для него требуется скомпилированной сборки для создания схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="2595d-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="2595d-158">В обозревателе решений щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="2595d-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="2595d-159">Выберите **добавить**, а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="2595d-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="2595d-160">В **Добавление шаблона** диалоговое окно, выберите «веб-API 2 контроллер с действиями чтения и записи, использующий Entity Framework.»</span><span class="sxs-lookup"><span data-stu-id="2595d-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="2595d-161">В **Добавление контроллера** диалоговом окне для **имя контроллера**, введите &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="2595d-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="2595d-162">Выберите &quot;использовать асинхронные действия контроллера&quot; флажок.</span><span class="sxs-lookup"><span data-stu-id="2595d-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="2595d-163">Для **класс модели**выберите &quot;книги&quot;.</span><span class="sxs-lookup"><span data-stu-id="2595d-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="2595d-164">(Если вы не видите `Book` класс в списке в раскрывающемся списке, убедитесь, что при построении проекта.) Нажмите кнопку «+».</span><span class="sxs-lookup"><span data-stu-id="2595d-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="2595d-165">Нажмите кнопку **добавить** в **новый контекст данных** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="2595d-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="2595d-166">Нажмите кнопку **добавить** в **Добавление контроллера** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="2595d-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="2595d-167">Формирование шаблонов добавляет класс с именем `BooksController` , определяющий контроллер API.</span><span class="sxs-lookup"><span data-stu-id="2595d-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="2595d-168">Он также добавляет класс с именем `BooksAPIContext` в папку Models, который определяет контекста данных Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2595d-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="2595d-169">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="2595d-169">Seed the Database</span></span>

<span data-ttu-id="2595d-170">В меню "Сервис" выберите **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="2595d-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="2595d-171">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2595d-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="2595d-172">Эта команда создает папку Migrations и добавляет новый файл кода Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="2595d-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="2595d-173">Откройте этот файл и добавьте следующий код, чтобы `Configuration.Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="2595d-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="2595d-174">В окне консоли диспетчера пакетов введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="2595d-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="2595d-175">Эти команды создают локальную базу данных и вызвать метод начальное значение для заполнения базы данных.</span><span class="sxs-lookup"><span data-stu-id="2595d-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="2595d-176">Добавьте классы объектов передачи данных</span><span class="sxs-lookup"><span data-stu-id="2595d-176">Add DTO Classes</span></span>

<span data-ttu-id="2595d-177">Если запустить приложение сейчас и отправить запрос GET /api/books/1, ответ выглядит примерно следующим.</span><span class="sxs-lookup"><span data-stu-id="2595d-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="2595d-178">(Я добавил отступами для удобства чтения).</span><span class="sxs-lookup"><span data-stu-id="2595d-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="2595d-179">Вместо этого я хочу этот запрос для возврата подмножества полей.</span><span class="sxs-lookup"><span data-stu-id="2595d-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="2595d-180">Кроме того мне необходимо вернуть имя автора, а не идентификатор автора.</span><span class="sxs-lookup"><span data-stu-id="2595d-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="2595d-181">Чтобы добиться этого, мы изменим методов контроллера для получения *объект передачи данных* (DTO) вместо модели EF.</span><span class="sxs-lookup"><span data-stu-id="2595d-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="2595d-182">DTO — это объект, предназначенный только для переноса данных.</span><span class="sxs-lookup"><span data-stu-id="2595d-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="2595d-183">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** | **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="2595d-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="2595d-184">Назовите папку &quot;DTO&quot;.</span><span class="sxs-lookup"><span data-stu-id="2595d-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="2595d-185">Добавьте класс с именем `BookDto` в папку DTO, со следующим определением:</span><span class="sxs-lookup"><span data-stu-id="2595d-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="2595d-186">Добавьте еще один класс с именем `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="2595d-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="2595d-187">Затем обновите `BooksController` класса для возвращения `BookDto` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="2595d-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="2595d-188">Мы будем использовать [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) метод проект `Book` экземпляры `BookDto` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="2595d-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="2595d-189">Вот обновленный код для класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="2595d-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="2595d-190">Я удалил `PutBook`, `PostBook`, и `DeleteBook` методов, так как они не нужны в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="2595d-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="2595d-191">Теперь Если вы запустите приложение и запросить /api/books/1, текст ответа должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2595d-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="2595d-192">Добавление атрибутов маршрутов</span><span class="sxs-lookup"><span data-stu-id="2595d-192">Add Route Attributes</span></span>

<span data-ttu-id="2595d-193">Затем мы преобразуем контроллер для использования маршрутизации с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="2595d-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="2595d-194">Во-первых, добавьте **RoutePrefix** атрибута к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="2595d-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="2595d-195">Этот атрибут определяет начальное сегментов URI-адреса для всех методов в этом контроллере.</span><span class="sxs-lookup"><span data-stu-id="2595d-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="2595d-196">Затем добавьте **[Route]** атрибуты к действиям контроллера, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2595d-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="2595d-197">Шаблон маршрута для каждого метода контроллера — это префикс, а также строки, указанные в **маршрута** атрибута.</span><span class="sxs-lookup"><span data-stu-id="2595d-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="2595d-198">Для `GetBook` метод, шаблон маршрута включает параметризованной строки &quot;{id: int}&quot;, выделяющему, если сегмент URI содержит целочисленное значение.</span><span class="sxs-lookup"><span data-stu-id="2595d-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="2595d-199">Метод</span><span class="sxs-lookup"><span data-stu-id="2595d-199">Method</span></span> | <span data-ttu-id="2595d-200">Шаблон маршрута</span><span class="sxs-lookup"><span data-stu-id="2595d-200">Route Template</span></span> | <span data-ttu-id="2595d-201">Пример URI</span><span class="sxs-lookup"><span data-stu-id="2595d-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="2595d-202">«api и книги»</span><span class="sxs-lookup"><span data-stu-id="2595d-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="2595d-203">«api/books / {id: int}»</span><span class="sxs-lookup"><span data-stu-id="2595d-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="2595d-204">Получение сведений о книге</span><span class="sxs-lookup"><span data-stu-id="2595d-204">Get Book Details</span></span>

<span data-ttu-id="2595d-205">Чтобы получить сведения о книге, клиент отправит запрос GET к `/api/books/{id}/details`, где *{id}* идентификатор книги.</span><span class="sxs-lookup"><span data-stu-id="2595d-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="2595d-206">Добавьте следующий метод в класс `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="2595d-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="2595d-207">Если вы запрашиваете `/api/books/1/details`, ответ выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2595d-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="2595d-208">Получение книги по жанру</span><span class="sxs-lookup"><span data-stu-id="2595d-208">Get Books By Genre</span></span>

<span data-ttu-id="2595d-209">Для получения списка книг определенного жанра, клиент отправит запрос GET к `/api/books/genre`, где *жанр* имя жанр.</span><span class="sxs-lookup"><span data-stu-id="2595d-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="2595d-210">(Например, `/api/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="2595d-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="2595d-211">Добавьте следующий метод в `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="2595d-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="2595d-212">Здесь мы при определении маршрута, который содержит параметр {жанр} в шаблоне URI.</span><span class="sxs-lookup"><span data-stu-id="2595d-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="2595d-213">Обратите внимание на то, что веб-API способен различать эти два URI и перенаправлять их в различные методы:</span><span class="sxs-lookup"><span data-stu-id="2595d-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="2595d-214">Это потому, что `GetBook` метод включает ограничение, что сегмент «id» должен быть целым числом:</span><span class="sxs-lookup"><span data-stu-id="2595d-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="2595d-215">Если вы запрашивали /api/books/fantasy, ответ выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2595d-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="2595d-216">Получение книги по авторам</span><span class="sxs-lookup"><span data-stu-id="2595d-216">Get Books By Author</span></span>

<span data-ttu-id="2595d-217">Чтобы получить список документации для данного автора, клиент отправит запрос GET к `/api/authors/id/books`, где *идентификатор* идентификатор автора.</span><span class="sxs-lookup"><span data-stu-id="2595d-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="2595d-218">Добавьте следующий метод в `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="2595d-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="2595d-219">В этом примере интересен поскольку &quot;книги&quot; является рассматривать дочерний ресурс &quot;авторы&quot;.</span><span class="sxs-lookup"><span data-stu-id="2595d-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="2595d-220">Этот шаблон является довольно часто в API-интерфейсов RESTful.</span><span class="sxs-lookup"><span data-stu-id="2595d-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="2595d-221">Тильда (~) в шаблоне маршрута переопределяет префикс маршрута в **RoutePrefix** атрибута.</span><span class="sxs-lookup"><span data-stu-id="2595d-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="2595d-222">Получение книги по дате публикации</span><span class="sxs-lookup"><span data-stu-id="2595d-222">Get Books By Publication Date</span></span>

<span data-ttu-id="2595d-223">Чтобы получить список документации по дате публикации, клиент отправит запрос GET к `/api/books/date/yyyy-mm-dd`, где *гггг мм дд* дата.</span><span class="sxs-lookup"><span data-stu-id="2595d-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="2595d-224">Вот один из способов сделать это:</span><span class="sxs-lookup"><span data-stu-id="2595d-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="2595d-225">`{pubdate:datetime}` Параметр ограничен в соответствии с **DateTime** значение.</span><span class="sxs-lookup"><span data-stu-id="2595d-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="2595d-226">Это работает, но это фактически более разрешительным, чем хотелось бы.</span><span class="sxs-lookup"><span data-stu-id="2595d-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="2595d-227">Например эти URI также будет соответствовать маршрута:</span><span class="sxs-lookup"><span data-stu-id="2595d-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="2595d-228">Нет ничего плохого, позволяя эти URI.</span><span class="sxs-lookup"><span data-stu-id="2595d-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="2595d-229">Тем не менее можно ограничить маршрут для определенного формата, добавление ограничения регулярных выражений в шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="2595d-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="2595d-230">Теперь только даты в виде &quot;гггг мм дд&quot; будут совпадать.</span><span class="sxs-lookup"><span data-stu-id="2595d-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="2595d-231">Обратите внимание на то, что мы не используем регулярное выражение для проверки того, что у нас есть фактическая дата.</span><span class="sxs-lookup"><span data-stu-id="2595d-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="2595d-232">Который обрабатывается при попытке преобразовать сегмент URI в веб-API **DateTime** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="2595d-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="2595d-233">Недопустимая дата такие как "2012-47-99' не сможет преобразовать, и клиент получит сообщение об ошибке 404.</span><span class="sxs-lookup"><span data-stu-id="2595d-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="2595d-234">Может также поддерживать разделитель косой черты (`/api/books/date/yyyy/mm/dd`), добавляя еще одну **[Route]** атрибута с другое регулярное выражение.</span><span class="sxs-lookup"><span data-stu-id="2595d-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="2595d-235">Нет сведений тонкое, но очень здесь.</span><span class="sxs-lookup"><span data-stu-id="2595d-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="2595d-236">Второй шаблон маршрута присутствует подстановочный знак (\*) в начале параметра {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="2595d-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="2595d-237">Подсистема маршрутизации {pubdate} должен соответствовать остальной части URI.</span><span class="sxs-lookup"><span data-stu-id="2595d-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="2595d-238">По умолчанию параметр шаблона соответствует один сегмент URI.</span><span class="sxs-lookup"><span data-stu-id="2595d-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="2595d-239">В этом случае мы хотим {pubdate} охватывать несколько сегментов URI-адреса:</span><span class="sxs-lookup"><span data-stu-id="2595d-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="2595d-240">Код контроллера</span><span class="sxs-lookup"><span data-stu-id="2595d-240">Controller Code</span></span>

<span data-ttu-id="2595d-241">Ниже приведен полный код для класса BooksController.</span><span class="sxs-lookup"><span data-stu-id="2595d-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="2595d-242">Сводка</span><span class="sxs-lookup"><span data-stu-id="2595d-242">Summary</span></span>

<span data-ttu-id="2595d-243">Маршрутизация с помощью атрибутов дает большей управляемости и большую гибкость при разработке URI для вашего API.</span><span class="sxs-lookup"><span data-stu-id="2595d-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
