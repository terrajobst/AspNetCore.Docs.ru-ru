---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Начало работы с ASP.NET Web API 2 (C#)
author: MikeWasson
description: HTTP не только для предоставления веб-страниц. Это также мощную платформу для создания API, которые предоставляют службы и данные. Протокол HTTP является простая и гибкая и ubiq...
ms.author: riande
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 62e99a41ba935470c39476c9aea8ee4193543425
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795297"
---
<a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="b7b5a-105">Начало работы с ASP.NET Web API 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="b7b5a-105">Get Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="b7b5a-106">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b7b5a-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b7b5a-107">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="b7b5a-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="b7b5a-108">HTTP не только для предоставления веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="b7b5a-109">Протокол HTTP также является мощной платформой для создания API, которые предоставляют службы и данные.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="b7b5a-110">HTTP — простую, гибкую и повсеместно.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="b7b5a-111">Практически любой платформе, можно представить себе имеет библиотеки HTTP, поэтому службы HTTP могут стать идеальным широкого круга клиентов, включая браузеры мобильных устройств и Классические приложения.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>

<span data-ttu-id="b7b5a-112">Веб-API ASP.NET — это платформа для создания веб-API-интерфейсы на основе .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="b7b5a-113">В этом руководстве используется веб-API ASP.NET для создания веб-API, который возвращает список продуктов.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b7b5a-114">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="b7b5a-114">Software versions used in the tutorial</span></span>

- [<span data-ttu-id="b7b5a-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b7b5a-115">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- <span data-ttu-id="b7b5a-116">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="b7b5a-116">Web API 2</span></span>

<span data-ttu-id="b7b5a-117">См. в разделе [Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) для более новой версии этого руководства.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="b7b5a-118">Создать проект веб-API</span><span class="sxs-lookup"><span data-stu-id="b7b5a-118">Create a Web API Project</span></span>

<span data-ttu-id="b7b5a-119">В этом руководстве используется веб-API ASP.NET для создания веб-API, который возвращает список продуктов.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="b7b5a-120">Интерфейсный веб-страницы использует jQuery для отображения результатов.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="b7b5a-121">Запустите Visual Studio и выберите **новый проект** из **запустить** страницы.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="b7b5a-122">Или с **файл** меню, выберите **New** и затем **проекта**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="b7b5a-123">В **шаблоны** области выберите **установленные шаблоны** и разверните **Visual C#** узла.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="b7b5a-124">В разделе **Visual C#** выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="b7b5a-125">В списке шаблонов проектов выберите **веб-приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="b7b5a-126">Присвойте проекту имя «ProductsApp» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="b7b5a-127">В **новый проект ASP.NET** диалоговом окне выберите **пустой** шаблона.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="b7b5a-128">В разделе &quot;добавить папки и основные ссылки для&quot;, проверьте **веб-API**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="b7b5a-129">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="b7b5a-130">Можно также создать проект веб-API с помощью &quot;веб-API&quot; шаблона.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="b7b5a-131">Шаблон веб-API ASP.NET MVC использует для предоставления страницы справки по API.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="b7b5a-132">Я использую пустой шаблон в этом руководстве, поскольку я хочу показать веб-API без MVC.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="b7b5a-133">В общем случае не нужно знать ASP.NET MVC позволяет использовать веб-API.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="b7b5a-134">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="b7b5a-134">Adding a Model</span></span>

<span data-ttu-id="b7b5a-135">*Модель* — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="b7b5a-136">Веб-API ASP.NET может автоматически сериализовать модель в JSON, XML или другом формате, а затем записывает сериализованные данные в тексте сообщения HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="b7b5a-137">До тех пор, пока клиент может прочитать формат сериализации, он может выполнить десериализацию объекта.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="b7b5a-138">Большинство клиентов может выполнить синтаксический анализ XML или JSON.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="b7b5a-139">Кроме того клиент может указать, какой формат, ему, задав заголовок Accept в сообщении запроса HTTP.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="b7b5a-140">Давайте сначала создадим простую модель, которая представляет продукт.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="b7b5a-141">Если обозреватель решений не отображается, нажмите кнопку **представление** меню и выберите **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="b7b5a-142">В обозревателе решений щелкните правой кнопкой мыши папку Models.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="b7b5a-143">В контекстном меню выберите **добавить** выберите **класс**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="b7b5a-144">Назовите класс &quot;продукта&quot;.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="b7b5a-145">Добавьте следующие свойства `Product` класса.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="b7b5a-146">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="b7b5a-146">Adding a Controller</span></span>

<span data-ttu-id="b7b5a-147">В веб-API *контроллера* — это объект, который обрабатывает HTTP-запросы.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="b7b5a-148">Мы добавим контроллер, который может возвращать список продуктов и один продукт, указываемого идентификатором.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="b7b5a-149">Если вы использовали ASP.NET MVC, вы уже знакомы с контроллерами.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="b7b5a-150">Контроллеры веб-API похожи на контроллеры MVC, но наследуется **ApiController** вместо класса **контроллера** класса.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="b7b5a-151">В **обозревателе решений**, щелкните правой кнопкой мыши папку Controllers.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="b7b5a-152">Выберите **добавить** , а затем выберите **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="b7b5a-153">В **Добавление шаблона** диалоговом окне выберите **контроллер Web API — пустой**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="b7b5a-154">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="b7b5a-155">В **Добавление контроллера** диалоговое окно, назовите контроллер &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="b7b5a-156">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="b7b5a-157">Формирование шаблонов создает файл с именем ProductsController.cs в папку "контроллеры".</span><span class="sxs-lookup"><span data-stu-id="b7b5a-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="b7b5a-158">Не нужно поместить в папку с именем контроллеры контроллеров.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="b7b5a-159">Имя папки — это просто удобный способ организации исходные файлы.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="b7b5a-160">Если этот файл еще не открыт, дважды щелкните файл, чтобы открыть его.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="b7b5a-161">Замените код в этот файл следующее:</span><span class="sxs-lookup"><span data-stu-id="b7b5a-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="b7b5a-162">Для простоты в примере, продукты, хранятся в массив фиксированной длины внутри класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="b7b5a-163">Разумеется в реальном приложении бы запрос к базе данных или использовать другого источника внешних данных.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="b7b5a-164">Контроллер определяет два метода, которые возвращают продуктов:</span><span class="sxs-lookup"><span data-stu-id="b7b5a-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="b7b5a-165">`GetAllProducts` Метод возвращает весь список продуктов, как **IEnumerable&lt;продукта&gt;**  типа.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="b7b5a-166">`GetProduct` Метод ищет один продукт по его идентификатору.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="b7b5a-167">Вот и все!</span><span class="sxs-lookup"><span data-stu-id="b7b5a-167">That's it!</span></span> <span data-ttu-id="b7b5a-168">У вас есть рабочий веб-API.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-168">You have a working web API.</span></span> <span data-ttu-id="b7b5a-169">Каждый метод в контроллере соответствует на один или несколько URI:</span><span class="sxs-lookup"><span data-stu-id="b7b5a-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="b7b5a-170">Метод контроллера</span><span class="sxs-lookup"><span data-stu-id="b7b5a-170">Controller Method</span></span> | <span data-ttu-id="b7b5a-171">URI</span><span class="sxs-lookup"><span data-stu-id="b7b5a-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="b7b5a-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="b7b5a-172">GetAllProducts</span></span> | <span data-ttu-id="b7b5a-173">/ api/продуктов</span><span class="sxs-lookup"><span data-stu-id="b7b5a-173">/api/products</span></span> |
| <span data-ttu-id="b7b5a-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="b7b5a-174">GetProduct</span></span> | <span data-ttu-id="b7b5a-175">/API/продукты/*идентификатор*</span><span class="sxs-lookup"><span data-stu-id="b7b5a-175">/api/products/*id*</span></span> |

<span data-ttu-id="b7b5a-176">Для `GetProduct` метод, *идентификатор* в URI — это.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="b7b5a-177">Например, чтобы получить продукт с Идентификатором 5, URI имеет `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="b7b5a-178">Дополнительные сведения о том, как веб-API направляет HTTP-запросов в методы контроллера, см. в разделе [маршрутизации в ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b7b5a-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="b7b5a-179">Вызов веб-API с помощью Javascript и jQuery</span><span class="sxs-lookup"><span data-stu-id="b7b5a-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="b7b5a-180">В этом разделе мы добавим страницу HTML, которое использует AJAX для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="b7b5a-181">Мы будем использовать jQuery для выполнения вызовов AJAX, так и для обновления страницы с результатами.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="b7b5a-182">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="b7b5a-183">В **Добавление нового элемента** диалоговое окно, выберите **Web** в узле **Visual C#**, а затем выберите **HTML-страницу** элемента.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="b7b5a-184">Присвойте странице имя &quot;index.html&quot;.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="b7b5a-185">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="b7b5a-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="b7b5a-186">Для получения jQuery можно использовать следующие способы.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="b7b5a-187">В этом примере я использовал [сети доставки Содержимого Microsoft Ajax](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7b5a-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="b7b5a-188">Вы также можете скачать его из [ http://jquery.com/ ](http://jquery.com/)и ASP.NET «Веб-API» шаблон проекта включает также jQuery.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="b7b5a-189">Получение списка продуктов</span><span class="sxs-lookup"><span data-stu-id="b7b5a-189">Getting a List of Products</span></span>

<span data-ttu-id="b7b5a-190">Для получения списка продуктов, отправьте запрос HTTP GET к &quot;/api/products&quot;.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="b7b5a-191">JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) функция отправляет запрос AJAX.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="b7b5a-192">Ответ содержит массив объектов JSON.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="b7b5a-193">`done` Функция задает обратный вызов, который вызывается, если запрос выполнен успешно.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="b7b5a-194">В обратный вызов Мы обновляем DOM с помощью информации о продукте.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="b7b5a-195">Получение продукта по Идентификатору</span><span class="sxs-lookup"><span data-stu-id="b7b5a-195">Getting a Product By ID</span></span>

<span data-ttu-id="b7b5a-196">Для получения продуктов по Идентификатору, отправьте запрос HTTP GET к &quot;/API/продукты/*идентификатор*&quot;, где *идентификатор* идентификатор продукта.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="b7b5a-197">Мы по-прежнему вызвать `getJSON` для отправки запроса AJAX, но на этот раз мы поместили идентификатор в URI запроса.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="b7b5a-198">Ответ от этого запроса — это представление JSON за единицу продукта.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="b7b5a-199">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="b7b5a-199">Running the Application</span></span>

<span data-ttu-id="b7b5a-200">Нажмите клавишу F5, чтобы начать отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="b7b5a-201">Веб-страницы должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b7b5a-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="b7b5a-202">Чтобы получить продукт по Идентификатору, введите код и нажмите кнопку поиска:</span><span class="sxs-lookup"><span data-stu-id="b7b5a-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="b7b5a-203">Если введен недопустимый идентификатор, сервер возвращает ошибку HTTP:</span><span class="sxs-lookup"><span data-stu-id="b7b5a-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="b7b5a-204">С помощью F12 для просмотра HTTP-запроса и ответа</span><span class="sxs-lookup"><span data-stu-id="b7b5a-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="b7b5a-205">При работе с HTTP-службу, может быть полезно для сообщений запросов и см. в разделе HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="b7b5a-206">Это можно сделать с помощью средств разработчика F12 в Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="b7b5a-207">Internet Explorer 9, нажмите **F12** чтобы открыть средства.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="b7b5a-208">Нажмите кнопку **сети** вкладку и нажмите клавишу **начать захват**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="b7b5a-209">Теперь вернитесь к веб-страницы и нажмите клавишу **F5** перезагрузить веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="b7b5a-210">Internet Explorer захватывает HTTP-трафика между браузером и веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="b7b5a-211">Представление "Сводка" показывает весь сетевой трафик для страницы:</span><span class="sxs-lookup"><span data-stu-id="b7b5a-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="b7b5a-212">Найдите запись для относительного URI «api/products /».</span><span class="sxs-lookup"><span data-stu-id="b7b5a-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="b7b5a-213">Выберите эту запись и нажмите кнопку **перейдите к представлению подробные**.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="b7b5a-214">В представлении «Подробности» есть вкладки для просмотра заголовков запроса и ответа и текст.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="b7b5a-215">Например, если щелкнуть **заголовки запроса** вкладке, можно увидеть, что клиент запросил &quot;application/json&quot; в заголовке Accept.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="b7b5a-216">Если щелкнуть вкладку тело ответа, вы увидите сообщение о том, как список продуктов был сериализован в формат JSON.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="b7b5a-217">Другие браузеры имеют схожие функции.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="b7b5a-218">Другой полезный инструмент — [Fiddler](http://www.fiddler2.com/fiddler2/), веб-прокси для отладки.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="b7b5a-219">Можно использовать Fiddler для просмотра трафик HTTP, а также для составления HTTP-запросов, который дает полный контроль над заголовков HTTP в запросе.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="b7b5a-220">Это приложение работает в Azure см. в разделе</span><span class="sxs-lookup"><span data-stu-id="b7b5a-220">See this App Running on Azure</span></span>

<span data-ttu-id="b7b5a-221">Вы действительно хотите см. по завершении сайт, запущенный в качестве активного веб-приложения?</span><span class="sxs-lookup"><span data-stu-id="b7b5a-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="b7b5a-222">Полную версию приложения можно развернуть учетную запись Azure, просто нажав кнопку ниже.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="b7b5a-223">Требуется учетная запись Azure для развертывания этого решения в Azure.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="b7b5a-224">Если у вас еще нет учетной записи, у вас есть следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="b7b5a-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="b7b5a-225">[Открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать для опробования платных служб Azure и даже в том случае, если они используются, вы сохраняете учетную запись и использовать бесплатные службы Azure.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-225">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="b7b5a-226">[Активировать преимущества подписчика MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ваша подписка MSDN предоставляет вам кредиты каждый месяц, который можно использовать для оплаты служб Azure.</span><span class="sxs-lookup"><span data-stu-id="b7b5a-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7b5a-227">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="b7b5a-227">Next Steps</span></span>

- <span data-ttu-id="b7b5a-228">Более полный пример службы HTTP, поддерживающий действия POST, PUT и DELETE и запись в базе данных, см. в разделе [с помощью веб-API 2 с Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="b7b5a-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="b7b5a-229">Дополнительные сведения о создании динамична и веб-приложений на основе HTTP-службу, см. в разделе [одностраничного приложения ASP.NET](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="b7b5a-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="b7b5a-230">Сведения о том, как развернуть веб-проекта Visual Studio в службе приложений Azure, см. в разделе [создать веб-приложение ASP.NET в службе приложений Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="b7b5a-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
