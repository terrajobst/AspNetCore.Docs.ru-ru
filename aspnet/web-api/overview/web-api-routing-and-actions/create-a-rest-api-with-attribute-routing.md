---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Создание REST API с помощью маршрутизации в ASP.NET Web API 2 с помощью атрибутов | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 18a44c280e6df1603837938d24d7d639d8c87cc2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021421"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="84a84-102">Создание REST API с помощью маршрутизации атрибутов в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="84a84-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="84a84-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="84a84-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="84a84-104">Веб-API 2 поддерживает новый тип маршрутизации, вызывается *маршрутизации с помощью атрибутов*.</span><span class="sxs-lookup"><span data-stu-id="84a84-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="84a84-105">Общие сведения о маршрутизации с помощью атрибутов, см. в разделе [маршрутизации с помощью атрибутов в веб-API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="84a84-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="84a84-106">В этом руководстве используется маршрутизация с помощью атрибутов для создания API REST для коллекции книг.</span><span class="sxs-lookup"><span data-stu-id="84a84-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="84a84-107">API-Интерфейс поддерживает следующие действия:</span><span class="sxs-lookup"><span data-stu-id="84a84-107">The API will support the following actions:</span></span>

| <span data-ttu-id="84a84-108">Действие</span><span class="sxs-lookup"><span data-stu-id="84a84-108">Action</span></span> | <span data-ttu-id="84a84-109">Пример URI</span><span class="sxs-lookup"><span data-stu-id="84a84-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="84a84-110">Получение списка всех книг.</span><span class="sxs-lookup"><span data-stu-id="84a84-110">Get a list of all books.</span></span> | <span data-ttu-id="84a84-111">/ api/книг</span><span class="sxs-lookup"><span data-stu-id="84a84-111">/api/books</span></span> |
| <span data-ttu-id="84a84-112">Получите книгу по идентификатору.</span><span class="sxs-lookup"><span data-stu-id="84a84-112">Get a book by ID.</span></span> | <span data-ttu-id="84a84-113">/API/Books/1</span><span class="sxs-lookup"><span data-stu-id="84a84-113">/api/books/1</span></span> |
| <span data-ttu-id="84a84-114">Получение сведений о книге.</span><span class="sxs-lookup"><span data-stu-id="84a84-114">Get the details of a book.</span></span> | <span data-ttu-id="84a84-115">/API/Books/1/Details</span><span class="sxs-lookup"><span data-stu-id="84a84-115">/api/books/1/details</span></span> |
| <span data-ttu-id="84a84-116">Получение списка книг по жанру.</span><span class="sxs-lookup"><span data-stu-id="84a84-116">Get a list of books by genre.</span></span> | <span data-ttu-id="84a84-117">/API/Books/FANTASY</span><span class="sxs-lookup"><span data-stu-id="84a84-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="84a84-118">Получение списка книг по дате публикации.</span><span class="sxs-lookup"><span data-stu-id="84a84-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="84a84-119">/API/Books/Date/2013-02-16 /api/books/date/2013/02/16 (альтернативный способ)</span><span class="sxs-lookup"><span data-stu-id="84a84-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="84a84-120">Получение списка книг определенного автора.</span><span class="sxs-lookup"><span data-stu-id="84a84-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="84a84-121">/API/authors/1/Books</span><span class="sxs-lookup"><span data-stu-id="84a84-121">/api/authors/1/books</span></span> |

<span data-ttu-id="84a84-122">Все методы являются только для чтения (HTTP-запросы GET).</span><span class="sxs-lookup"><span data-stu-id="84a84-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="84a84-123">Уровень данных мы будем использовать Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="84a84-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="84a84-124">Записи книги будет иметь следующие поля:</span><span class="sxs-lookup"><span data-stu-id="84a84-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="84a84-125">ID</span><span class="sxs-lookup"><span data-stu-id="84a84-125">ID</span></span>
- <span data-ttu-id="84a84-126">Заголовок</span><span class="sxs-lookup"><span data-stu-id="84a84-126">Title</span></span>
- <span data-ttu-id="84a84-127">Жанра</span><span class="sxs-lookup"><span data-stu-id="84a84-127">Genre</span></span>
- <span data-ttu-id="84a84-128">Дата публикации</span><span class="sxs-lookup"><span data-stu-id="84a84-128">Publication date</span></span>
- <span data-ttu-id="84a84-129">Цена</span><span class="sxs-lookup"><span data-stu-id="84a84-129">Price</span></span>
- <span data-ttu-id="84a84-130">Описание</span><span class="sxs-lookup"><span data-stu-id="84a84-130">Description</span></span>
- <span data-ttu-id="84a84-131">AuthorID (внешний ключ к таблице Authors)</span><span class="sxs-lookup"><span data-stu-id="84a84-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="84a84-132">Большинство запросов тем не менее, API вернет подмножество этих данных ("Заголовок", "Автор" и "genre").</span><span class="sxs-lookup"><span data-stu-id="84a84-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="84a84-133">Чтобы получить полную запись клиента запросы `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="84a84-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84a84-134">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="84a84-134">Prerequisites</span></span>

<span data-ttu-id="84a84-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional или Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="84a84-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="84a84-136">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84a84-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="84a84-137">Запустить Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84a84-137">Start by running Visual Studio.</span></span> <span data-ttu-id="84a84-138">Из **файл** меню, выберите **New** , а затем выберите **проекта**.</span><span class="sxs-lookup"><span data-stu-id="84a84-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="84a84-139">Разверните **установленные** > **Visual C#** категории.</span><span class="sxs-lookup"><span data-stu-id="84a84-139">Expand the **Installed** > **Visual C#** category.</span></span> <span data-ttu-id="84a84-140">В разделе **Visual C#** выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="84a84-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="84a84-141">В списке шаблонов проектов выберите **веб-приложение ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="84a84-141">In the list of project templates, select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="84a84-142">Назовите проект &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="84a84-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="84a84-143">В **новое веб-приложение ASP.NET** диалоговом окне выберите **пустой** шаблона.</span><span class="sxs-lookup"><span data-stu-id="84a84-143">In the **New ASP.NET Web Application** dialog, select the **Empty** template.</span></span> <span data-ttu-id="84a84-144">В разделе «Добавление папок и основные ссылки для» выберите **веб-API** флажок.</span><span class="sxs-lookup"><span data-stu-id="84a84-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="84a84-145">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="84a84-145">Click **OK**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="84a84-146">Это создает схему проекта, который настроен для функциональных возможностей веб-API.</span><span class="sxs-lookup"><span data-stu-id="84a84-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="84a84-147">Модели домена</span><span class="sxs-lookup"><span data-stu-id="84a84-147">Domain Models</span></span>

<span data-ttu-id="84a84-148">Добавьте классы для модели домена.</span><span class="sxs-lookup"><span data-stu-id="84a84-148">Next, add classes for domain models.</span></span> <span data-ttu-id="84a84-149">В обозревателе решений щелкните правой кнопкой мыши папку Models.</span><span class="sxs-lookup"><span data-stu-id="84a84-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="84a84-150">Выберите **добавить**, а затем выберите **класс**.</span><span class="sxs-lookup"><span data-stu-id="84a84-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="84a84-151">Присвойте классу имя `Author`.</span><span class="sxs-lookup"><span data-stu-id="84a84-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="84a84-152">Замените код в Author.cs следующее:</span><span class="sxs-lookup"><span data-stu-id="84a84-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="84a84-153">Теперь добавьте класс с именем `Book`.</span><span class="sxs-lookup"><span data-stu-id="84a84-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="84a84-154">Добавить контроллер веб-API</span><span class="sxs-lookup"><span data-stu-id="84a84-154">Add a Web API Controller</span></span>

<span data-ttu-id="84a84-155">На этом этапе мы добавим контроллер Web API, использующий Entity Framework в качестве уровня данных.</span><span class="sxs-lookup"><span data-stu-id="84a84-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="84a84-156">Для сборки проекта нажмите CTRL+SHIFT+B.</span><span class="sxs-lookup"><span data-stu-id="84a84-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="84a84-157">Платформа Entity Framework использует отражение для обнаружения свойства моделей, поэтому для него требуется скомпилированной сборки для создания схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="84a84-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="84a84-158">В обозревателе решений щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="84a84-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="84a84-159">Выберите **добавить**, а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="84a84-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="84a84-160">В **Добавление шаблона** диалоговом окне выберите **контроллер Web API 2 с действиями, использующий Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="84a84-160">In the **Add Scaffold** dialog, select **Web API 2 Controller with actions, using Entity Framework**.</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="84a84-161">В **Добавление контроллера** диалоговом окне для **имя контроллера**, введите &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="84a84-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="84a84-162">Выберите &quot;использовать асинхронные действия контроллера&quot; флажок.</span><span class="sxs-lookup"><span data-stu-id="84a84-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="84a84-163">Для **класс модели**выберите &quot;книги&quot;.</span><span class="sxs-lookup"><span data-stu-id="84a84-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="84a84-164">(Если вы не видите `Book` класс в списке в раскрывающемся списке, убедитесь, что при построении проекта.) Нажмите кнопку «+».</span><span class="sxs-lookup"><span data-stu-id="84a84-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="84a84-165">Нажмите кнопку **добавить** в **новый контекст данных** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="84a84-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="84a84-166">Нажмите кнопку **добавить** в **Добавление контроллера** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="84a84-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="84a84-167">Формирование шаблонов добавляет класс с именем `BooksController` , определяющий контроллер API.</span><span class="sxs-lookup"><span data-stu-id="84a84-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="84a84-168">Он также добавляет класс с именем `BooksAPIContext` в папку Models, который определяет контекста данных Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="84a84-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="84a84-169">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="84a84-169">Seed the Database</span></span>

<span data-ttu-id="84a84-170">В меню "Сервис" выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="84a84-170">From the Tools menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="84a84-171">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="84a84-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="84a84-172">Эта команда создает папку Migrations и добавляет новый файл кода Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="84a84-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="84a84-173">Откройте этот файл и добавьте следующий код, чтобы `Configuration.Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="84a84-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="84a84-174">В окне консоли диспетчера пакетов введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="84a84-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="84a84-175">Эти команды создают локальную базу данных и вызвать метод начальное значение для заполнения базы данных.</span><span class="sxs-lookup"><span data-stu-id="84a84-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="84a84-176">Добавьте классы объектов передачи данных</span><span class="sxs-lookup"><span data-stu-id="84a84-176">Add DTO Classes</span></span>

<span data-ttu-id="84a84-177">Если запустить приложение сейчас и отправить запрос GET /api/books/1, ответ выглядит примерно следующим.</span><span class="sxs-lookup"><span data-stu-id="84a84-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="84a84-178">(Я добавил отступами для удобства чтения).</span><span class="sxs-lookup"><span data-stu-id="84a84-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="84a84-179">Вместо этого я хочу этот запрос для возврата подмножества полей.</span><span class="sxs-lookup"><span data-stu-id="84a84-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="84a84-180">Кроме того мне необходимо вернуть имя автора, а не идентификатор автора.</span><span class="sxs-lookup"><span data-stu-id="84a84-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="84a84-181">Чтобы добиться этого, мы изменим методов контроллера для получения *объект передачи данных* (DTO) вместо модели EF.</span><span class="sxs-lookup"><span data-stu-id="84a84-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="84a84-182">DTO — это объект, предназначенный только для переноса данных.</span><span class="sxs-lookup"><span data-stu-id="84a84-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="84a84-183">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** | **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="84a84-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="84a84-184">Назовите папку &quot;DTO&quot;.</span><span class="sxs-lookup"><span data-stu-id="84a84-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="84a84-185">Добавьте класс с именем `BookDto` в папку DTO, со следующим определением:</span><span class="sxs-lookup"><span data-stu-id="84a84-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="84a84-186">Добавьте еще один класс с именем `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="84a84-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="84a84-187">Затем обновите `BooksController` класса для возвращения `BookDto` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="84a84-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="84a84-188">Мы будем использовать [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) метод проект `Book` экземпляры `BookDto` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="84a84-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="84a84-189">Вот обновленный код для класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="84a84-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="84a84-190">Я удалил `PutBook`, `PostBook`, и `DeleteBook` методов, так как они не нужны в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="84a84-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="84a84-191">Теперь Если вы запустите приложение и запросить /api/books/1, текст ответа должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="84a84-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="84a84-192">Добавление атрибутов маршрутов</span><span class="sxs-lookup"><span data-stu-id="84a84-192">Add Route Attributes</span></span>

<span data-ttu-id="84a84-193">Затем мы преобразуем контроллер для использования маршрутизации с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="84a84-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="84a84-194">Во-первых, добавьте **RoutePrefix** атрибута к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="84a84-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="84a84-195">Этот атрибут определяет начальное сегментов URI-адреса для всех методов в этом контроллере.</span><span class="sxs-lookup"><span data-stu-id="84a84-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="84a84-196">Затем добавьте **[Route]** атрибуты к действиям контроллера, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="84a84-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="84a84-197">Шаблон маршрута для каждого метода контроллера — это префикс, а также строки, указанные в **маршрута** атрибута.</span><span class="sxs-lookup"><span data-stu-id="84a84-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="84a84-198">Для `GetBook` метод, шаблон маршрута включает параметризованной строки &quot;{id: int}&quot;, выделяющему, если сегмент URI содержит целочисленное значение.</span><span class="sxs-lookup"><span data-stu-id="84a84-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="84a84-199">Метод</span><span class="sxs-lookup"><span data-stu-id="84a84-199">Method</span></span> | <span data-ttu-id="84a84-200">Шаблон маршрута</span><span class="sxs-lookup"><span data-stu-id="84a84-200">Route Template</span></span> | <span data-ttu-id="84a84-201">Пример URI</span><span class="sxs-lookup"><span data-stu-id="84a84-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="84a84-202">«api и книги»</span><span class="sxs-lookup"><span data-stu-id="84a84-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="84a84-203">«api/books / {id: int}»</span><span class="sxs-lookup"><span data-stu-id="84a84-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="84a84-204">Получение сведений о книге</span><span class="sxs-lookup"><span data-stu-id="84a84-204">Get Book Details</span></span>

<span data-ttu-id="84a84-205">Чтобы получить сведения о книге, клиент отправит запрос GET к `/api/books/{id}/details`, где *{id}* идентификатор книги.</span><span class="sxs-lookup"><span data-stu-id="84a84-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="84a84-206">Добавьте следующий метод в класс `BooksController` .</span><span class="sxs-lookup"><span data-stu-id="84a84-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="84a84-207">Если вы запрашиваете `/api/books/1/details`, ответ выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="84a84-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="84a84-208">Получение книги по жанру</span><span class="sxs-lookup"><span data-stu-id="84a84-208">Get Books By Genre</span></span>

<span data-ttu-id="84a84-209">Для получения списка книг определенного жанра, клиент отправит запрос GET к `/api/books/genre`, где *жанр* имя жанр.</span><span class="sxs-lookup"><span data-stu-id="84a84-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="84a84-210">(Например, `/api/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="84a84-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="84a84-211">Добавьте следующий метод в `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="84a84-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="84a84-212">Здесь мы при определении маршрута, который содержит параметр {жанр} в шаблоне URI.</span><span class="sxs-lookup"><span data-stu-id="84a84-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="84a84-213">Обратите внимание на то, что веб-API способен различать эти два URI и перенаправлять их в различные методы:</span><span class="sxs-lookup"><span data-stu-id="84a84-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="84a84-214">Это потому, что `GetBook` метод включает ограничение, что сегмент «id» должен быть целым числом:</span><span class="sxs-lookup"><span data-stu-id="84a84-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="84a84-215">Если вы запрашивали /api/books/fantasy, ответ выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="84a84-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="84a84-216">Получение книги по авторам</span><span class="sxs-lookup"><span data-stu-id="84a84-216">Get Books By Author</span></span>

<span data-ttu-id="84a84-217">Чтобы получить список документации для данного автора, клиент отправит запрос GET к `/api/authors/id/books`, где *идентификатор* идентификатор автора.</span><span class="sxs-lookup"><span data-stu-id="84a84-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="84a84-218">Добавьте следующий метод в `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="84a84-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="84a84-219">В этом примере интересен поскольку &quot;книги&quot; является рассматривать дочерний ресурс &quot;авторы&quot;.</span><span class="sxs-lookup"><span data-stu-id="84a84-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="84a84-220">Этот шаблон является довольно часто в API-интерфейсов RESTful.</span><span class="sxs-lookup"><span data-stu-id="84a84-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="84a84-221">Тильда (~) в шаблоне маршрута переопределяет префикс маршрута в **RoutePrefix** атрибута.</span><span class="sxs-lookup"><span data-stu-id="84a84-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="84a84-222">Получение книги по дате публикации</span><span class="sxs-lookup"><span data-stu-id="84a84-222">Get Books By Publication Date</span></span>

<span data-ttu-id="84a84-223">Чтобы получить список документации по дате публикации, клиент отправит запрос GET к `/api/books/date/yyyy-mm-dd`, где *гггг мм дд* дата.</span><span class="sxs-lookup"><span data-stu-id="84a84-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="84a84-224">Вот один из способов сделать это:</span><span class="sxs-lookup"><span data-stu-id="84a84-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="84a84-225">`{pubdate:datetime}` Параметр ограничен в соответствии с **DateTime** значение.</span><span class="sxs-lookup"><span data-stu-id="84a84-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="84a84-226">Это работает, но это фактически более разрешительным, чем хотелось бы.</span><span class="sxs-lookup"><span data-stu-id="84a84-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="84a84-227">Например эти URI также будет соответствовать маршрута:</span><span class="sxs-lookup"><span data-stu-id="84a84-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="84a84-228">Нет ничего плохого, позволяя эти URI.</span><span class="sxs-lookup"><span data-stu-id="84a84-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="84a84-229">Тем не менее можно ограничить маршрут для определенного формата, добавление ограничения регулярных выражений в шаблон маршрута:</span><span class="sxs-lookup"><span data-stu-id="84a84-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="84a84-230">Теперь только даты в виде &quot;гггг мм дд&quot; будут совпадать.</span><span class="sxs-lookup"><span data-stu-id="84a84-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="84a84-231">Обратите внимание на то, что мы не используем регулярное выражение для проверки того, что у нас есть фактическая дата.</span><span class="sxs-lookup"><span data-stu-id="84a84-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="84a84-232">Который обрабатывается при попытке преобразовать сегмент URI в веб-API **DateTime** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="84a84-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="84a84-233">Недопустимая дата такие как "2012-47-99' не сможет преобразовать, и клиент получит сообщение об ошибке 404.</span><span class="sxs-lookup"><span data-stu-id="84a84-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="84a84-234">Может также поддерживать разделитель косой черты (`/api/books/date/yyyy/mm/dd`), добавляя еще одну **[Route]** атрибута с другое регулярное выражение.</span><span class="sxs-lookup"><span data-stu-id="84a84-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="84a84-235">Нет сведений тонкое, но очень здесь.</span><span class="sxs-lookup"><span data-stu-id="84a84-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="84a84-236">Второй шаблон маршрута присутствует подстановочный знак (\*) в начале параметра {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="84a84-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="84a84-237">Подсистема маршрутизации {pubdate} должен соответствовать остальной части URI.</span><span class="sxs-lookup"><span data-stu-id="84a84-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="84a84-238">По умолчанию параметр шаблона соответствует один сегмент URI.</span><span class="sxs-lookup"><span data-stu-id="84a84-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="84a84-239">В этом случае мы хотим {pubdate} охватывать несколько сегментов URI-адреса:</span><span class="sxs-lookup"><span data-stu-id="84a84-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="84a84-240">Код контроллера</span><span class="sxs-lookup"><span data-stu-id="84a84-240">Controller Code</span></span>

<span data-ttu-id="84a84-241">Ниже приведен полный код для класса BooksController.</span><span class="sxs-lookup"><span data-stu-id="84a84-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="84a84-242">Сводка</span><span class="sxs-lookup"><span data-stu-id="84a84-242">Summary</span></span>

<span data-ttu-id="84a84-243">Маршрутизация с помощью атрибутов дает большей управляемости и большую гибкость при разработке URI для вашего API.</span><span class="sxs-lookup"><span data-stu-id="84a84-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
