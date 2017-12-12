---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: "Приступая к работе с ASP.NET Web API 2 (C#)"
author: MikeWasson
description: "HTTP не только для предоставления веб-страниц. Это также мощную платформу для построения API, которые предоставляют службы и данных. Протокол HTTP является простой и гибкий и ubiq..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: fa1cd068a7466e0b6b6fe7716090c8a7afd2a4d5
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2017
---
<a name="getting-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="ac0e5-105">Приступая к работе с ASP.NET Web API 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="ac0e5-105">Getting Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="ac0e5-106">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ac0e5-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ac0e5-107">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="ac0e5-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="ac0e5-108">HTTP не только для предоставления веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="ac0e5-109">HTTP также представляет собой мощную платформу для построения API, которые предоставляют службы и данных.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="ac0e5-110">Протокол HTTP является простой, гибкий и повсеместно.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="ac0e5-111">Практически на любой платформе, можно представить имеет библиотеку HTTP, поэтому службы HTTP может достигать широкий спектр клиентов, включая браузеры мобильных устройств и Классические приложения.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>
 
<span data-ttu-id="ac0e5-112">Веб-API ASP.NET — это платформа для построения веб-API на основе .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="ac0e5-113">В этом учебнике используется ASP.NET Web API для создания веб-API, который возвращает список продуктов.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

 
 ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ac0e5-114">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="ac0e5-114">Software versions used in the tutorial</span></span>
  
 - [<span data-ttu-id="ac0e5-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ac0e5-115">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)
 - <span data-ttu-id="ac0e5-116">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="ac0e5-116">Web API 2</span></span>

<span data-ttu-id="ac0e5-117">В разделе [создать веб-API ASP.NET Core и Visual Studio для Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) для более новой версии этого учебника.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="ac0e5-118">Создайте проект веб-API</span><span class="sxs-lookup"><span data-stu-id="ac0e5-118">Create a Web API Project</span></span>

<span data-ttu-id="ac0e5-119">В этом учебнике используется ASP.NET Web API для создания веб-API, который возвращает список продуктов.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="ac0e5-120">Интерфейсный веб-страница использует jQuery для отображения результатов.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="ac0e5-121">Запустите Visual Studio и выберите **новый проект** из **запустить** страницы.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="ac0e5-122">Или из **файл** последовательно выберите пункты **New** и затем **проекта**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="ac0e5-123">В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ac0e5-124">В разделе **Visual C#**выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ac0e5-125">В списке шаблонов проектов выберите **веб-приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="ac0e5-126">Назовите проект «ProductsApp» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="ac0e5-127">В **новый проект ASP.NET** диалогового окна выберите **пустой** шаблона.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="ac0e5-128">В разделе &quot;добавить папки и основные ссылки для&quot;, проверьте **веб-API**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="ac0e5-129">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="ac0e5-130">Можно также создать проект веб-API с помощью &quot;веб-API&quot; шаблона.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="ac0e5-131">Шаблон веб-API ASP.NET MVC использует для предоставления API страницы справки.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="ac0e5-132">Я использую пустого шаблона в этом учебнике, так как требуется для отображения веб-API без MVC.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="ac0e5-133">В общем случае не нужно знать ASP.NET MVC для использования веб-API.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="ac0e5-134">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="ac0e5-134">Adding a Model</span></span>

<span data-ttu-id="ac0e5-135">*Модель* — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="ac0e5-136">Веб-API ASP.NET можно Автоматическая сериализация модели в JSON, XML или другой формат и создайте сериализованные данные в текст сообщения HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="ac0e5-137">При условии, что клиент может прочитать формат сериализации, может выполнить десериализацию объекта.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="ac0e5-138">Большинство клиентов может выполнить синтаксический анализ XML или JSON.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="ac0e5-139">Кроме того клиент может указать, какой формат ему, задав для заголовка Accept в сообщение HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="ac0e5-140">Давайте начнем с создания простой модели, представляет продукт.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="ac0e5-141">Если обозреватель решений не отображается, нажмите кнопку **представление** и выбрать пункт **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="ac0e5-142">В обозревателе решений щелкните правой кнопкой мыши папку модели.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="ac0e5-143">В контекстном меню выберите **добавить** выберите **класса**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="ac0e5-144">Имя класса &quot;продукта&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="ac0e5-145">Добавьте следующие свойства для `Product` класса.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="ac0e5-146">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="ac0e5-146">Adding a Controller</span></span>

<span data-ttu-id="ac0e5-147">В веб-API *контроллера* — это объект, который обрабатывает HTTP-запросы.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="ac0e5-148">Будет добавлен контроллер, который может возвращать список продуктов и один продукт с указанным идентификатором.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="ac0e5-149">При использовании ASP.NET MVC, вы уже знакомы с контроллерами.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="ac0e5-150">Контроллеры веб-API аналогичны контроллеров MVC, но наследуют **ApiController** вместо класса **контроллера** класса.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="ac0e5-151">В **обозревателе решений**, щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="ac0e5-152">Выберите **добавить** , а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="ac0e5-153">В **Добавление формирования шаблонов** диалогового окна выберите **контроллер Web API - пустой**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="ac0e5-154">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="ac0e5-155">В **добавления контроллера** диалога, имя контроллера &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ac0e5-156">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="ac0e5-157">Формирование шаблонов создает файл с именем ProductsController.cs в папке Controllers находится.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="ac0e5-158">Не нужно поместить в папку с именем контроллеров контроллеров.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="ac0e5-159">Имя папки является лишь удобным способом для организации исходные файлы.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="ac0e5-160">Если этот файл уже не открыт, дважды щелкните файл, чтобы открыть его.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="ac0e5-161">Замените код в этот файл следующее:</span><span class="sxs-lookup"><span data-stu-id="ac0e5-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="ac0e5-162">Для простоты, продукты, хранятся в массив фиксированной длины внутри класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="ac0e5-163">Конечно в реальном приложении будет запросов к базе данных или использование другого источника внешних данных.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="ac0e5-164">Контроллер определяет два метода, которые возвращают продуктов:</span><span class="sxs-lookup"><span data-stu-id="ac0e5-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="ac0e5-165">`GetAllProducts` Метод возвращает весь список продуктов, как **IEnumerable&lt;продукта&gt;**  типа.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="ac0e5-166">`GetProduct` Метод выполняет поиск одного продукта по его идентификатору.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="ac0e5-167">Ну вот!</span><span class="sxs-lookup"><span data-stu-id="ac0e5-167">That's it!</span></span> <span data-ttu-id="ac0e5-168">У вас есть рабочий веб-API.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-168">You have a working web API.</span></span> <span data-ttu-id="ac0e5-169">Каждый метод в контроллере соответствует один или несколько URI-адресам:</span><span class="sxs-lookup"><span data-stu-id="ac0e5-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="ac0e5-170">Метод контроллера</span><span class="sxs-lookup"><span data-stu-id="ac0e5-170">Controller Method</span></span> | <span data-ttu-id="ac0e5-171">URI</span><span class="sxs-lookup"><span data-stu-id="ac0e5-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="ac0e5-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="ac0e5-172">GetAllProducts</span></span> | <span data-ttu-id="ac0e5-173">/ api/продуктов</span><span class="sxs-lookup"><span data-stu-id="ac0e5-173">/api/products</span></span> |
| <span data-ttu-id="ac0e5-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="ac0e5-174">GetProduct</span></span> | <span data-ttu-id="ac0e5-175">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="ac0e5-175">/api/products/*id*</span></span> |

<span data-ttu-id="ac0e5-176">Для `GetProduct` метода *идентификатор* в URI — это.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="ac0e5-177">Например, чтобы получить продукта с Идентификатором 5, URI имеет следующий вид `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="ac0e5-178">Дополнительные сведения о том, как веб-API направляет HTTP-запросов в методы контроллера см. в разделе [маршрутизации в ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ac0e5-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="ac0e5-179">Вызов веб-API с помощью Javascript и jQuery</span><span class="sxs-lookup"><span data-stu-id="ac0e5-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="ac0e5-180">В этом разделе мы добавим HTML-страницы, использующего технологию AJAX для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="ac0e5-181">Мы будем использовать jQuery выполнять вызовы AJAX, так и для обновления страницы с результатами.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="ac0e5-182">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="ac0e5-183">В **Добавление нового элемента** диалогового окна выберите **Web** узле **Visual C#**и выберите **HTML-страницу** элемента.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="ac0e5-184">Назовите страницу &quot;index.html&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="ac0e5-185">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ac0e5-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="ac0e5-186">Существует несколько способов получить jQuery.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="ac0e5-187">В этом примере используется [сети Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="ac0e5-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="ac0e5-188">Можно также загрузить его из [http://jquery.com/](http://jquery.com/)и ASP.NET «Веб-API» шаблон проекта включает также jQuery.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="ac0e5-189">Получение списка продуктов</span><span class="sxs-lookup"><span data-stu-id="ac0e5-189">Getting a List of Products</span></span>

<span data-ttu-id="ac0e5-190">Чтобы получить список продуктов, отправить запрос HTTP GET для &quot;/api/продуктов&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="ac0e5-191">JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) функция посылает запрос AJAX.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="ac0e5-192">Ответ содержит массив объектов JSON.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="ac0e5-193">`done` Функция задает обратный вызов, который вызывается, если запрос выполнен успешно.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="ac0e5-194">В обратный вызов мы обновлении DOM с сведения о продукте.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="ac0e5-195">Получение продукта по Идентификатору</span><span class="sxs-lookup"><span data-stu-id="ac0e5-195">Getting a Product By ID</span></span>

<span data-ttu-id="ac0e5-196">Для получения продуктов по Идентификатору, отправьте запрос HTTP GET для &quot;/api/продукты/*идентификатор*&quot;, где *идентификатор* идентификатор продукта.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="ac0e5-197">По-прежнему вызвать `getJSON` для отправки запроса AJAX, но на этот раз мы поместили идентификатор URI запроса.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="ac0e5-198">Ответ на этот запрос является JSON-представление за единицу продукта.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="ac0e5-199">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="ac0e5-199">Running the Application</span></span>

<span data-ttu-id="ac0e5-200">Нажмите клавишу F5, чтобы запустить отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="ac0e5-201">Веб-страница должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ac0e5-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="ac0e5-202">Чтобы получить продукта по Идентификатору, введите код и нажмите кнопку поиска:</span><span class="sxs-lookup"><span data-stu-id="ac0e5-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="ac0e5-203">Если введен недопустимый идентификатор, сервер возвращает ошибку HTTP:</span><span class="sxs-lookup"><span data-stu-id="ac0e5-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="ac0e5-204">С помощью клавиши F12 для просмотра HTTP-запроса и ответа</span><span class="sxs-lookup"><span data-stu-id="ac0e5-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="ac0e5-205">При работе службы HTTP может быть очень удобны для сообщений запросов и разделе HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="ac0e5-206">Это можно сделать с помощью средств разработчика F12 в Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="ac0e5-207">Internet Explorer 9, нажмите **F12** чтобы открыть средства.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="ac0e5-208">Нажмите кнопку **сети** вкладку и нажмите клавишу **начать захват**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="ac0e5-209">Теперь вернитесь на веб-страницы и нажмите клавишу **F5** перезагрузить веб-странице.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="ac0e5-210">Internet Explorer захватывает HTTP-трафика между браузером и веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="ac0e5-211">Представление "Сводка" показывает весь сетевой трафик для страницы:</span><span class="sxs-lookup"><span data-stu-id="ac0e5-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="ac0e5-212">Найдите запись для относительного URI «api/продукты /».</span><span class="sxs-lookup"><span data-stu-id="ac0e5-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="ac0e5-213">Выберите эту запись и нажмите кнопку **перейдите в подробном представлении**.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="ac0e5-214">В представлении «Подробности» есть вкладки, чтобы просмотреть запрос и ответ заголовки и текст.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="ac0e5-215">Например, если щелкнуть **заголовки запроса** вкладку, можно увидеть, что клиент запросил &quot;приложение/json&quot; в заголовке Accept.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="ac0e5-216">Если щелкнуть вкладку текст ответа, вы увидите, как список продуктов был сериализован в JSON.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="ac0e5-217">Другие браузеры имеют одинаковые функции.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="ac0e5-218">Другим полезным средством является [Fiddler](http://www.fiddler2.com/fiddler2/), прокси-сервером отладки веб-узла.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="ac0e5-219">Можно использовать Fiddler для просмотра трафика HTTP, а также для создания запросов HTTP, которая предоставляет полный контроль над заголовков HTTP в запросе.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="ac0e5-220">Это приложение, работающее на платформе Azure см.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-220">See this App Running on Azure</span></span>

<span data-ttu-id="ac0e5-221">Вы действительно хотите см. по завершении узел, работающий как динамические веб-приложения?</span><span class="sxs-lookup"><span data-stu-id="ac0e5-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="ac0e5-222">Можно развернуть полную версию приложения для учетной записи Azure, просто нажав кнопку ниже.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="ac0e5-223">Требуется учетная запись Azure для развертывания этого решения в Azure.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="ac0e5-224">Если у вас еще нет учетной записи, у вас есть следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="ac0e5-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="ac0e5-225">[Открыть учетную запись Azure бесплатно](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать, чтобы испытать оплаты служб Azure и даже после того, они используются до можно хранить учетной записи, и используйте освободить служб Azure.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-225">[Open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="ac0e5-226">[Активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -подписка Your MSDN дает кредиты каждого месяца, можно использовать для оплаты служб Azure.</span><span class="sxs-lookup"><span data-stu-id="ac0e5-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac0e5-227">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="ac0e5-227">Next Steps</span></span>

- <span data-ttu-id="ac0e5-228">Более полный пример службы HTTP, который поддерживает такие действия, POST, PUT и DELETE и записывает в базе данных см. в разделе [с помощью веб-API 2 с Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="ac0e5-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="ac0e5-229">Дополнительные сведения о создании гибкость и отвечать на запросы веб-приложений на основе службы HTTP см. в разделе [одностраничного приложения ASP.NET](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="ac0e5-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="ac0e5-230">Сведения о развертывании веб-проекта Visual Studio для службы приложений Azure см. в разделе [создать веб-приложение ASP.NET в службе приложений Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="ac0e5-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span></span>
