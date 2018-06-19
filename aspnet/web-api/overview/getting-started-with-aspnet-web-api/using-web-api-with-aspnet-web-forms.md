---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Использование веб-API с веб-форм ASP.NET | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2017
ms.locfileid: "26536083"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="c5e99-102">Использование веб-API с веб-форм ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c5e99-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="c5e99-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c5e99-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c5e99-104">Несмотря на то, что веб-API ASP.NET упаковывается вместе с ASP.NET MVC, его можно легко добавить веб-API в традиционных приложений для веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c5e99-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="c5e99-105">Этот учебник поможет выполнить действия.</span><span class="sxs-lookup"><span data-stu-id="c5e99-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="c5e99-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="c5e99-106">Overview</span></span>

<span data-ttu-id="c5e99-107">Чтобы использовать веб-API в приложении Web Forms, существует два основных этапа.</span><span class="sxs-lookup"><span data-stu-id="c5e99-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="c5e99-108">Добавить контроллер веб-API, который является производным от **ApiController** класса.</span><span class="sxs-lookup"><span data-stu-id="c5e99-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="c5e99-109">Добавление таблицы маршрутов для **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="c5e99-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="c5e99-110">Создайте проект веб-форм</span><span class="sxs-lookup"><span data-stu-id="c5e99-110">Create a Web Forms Project</span></span>

<span data-ttu-id="c5e99-111">Запустите Visual Studio и выберите **новый проект** из **запустить** страницы.</span><span class="sxs-lookup"><span data-stu-id="c5e99-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="c5e99-112">Или из **файл** последовательно выберите пункты **New** и затем **проекта**.</span><span class="sxs-lookup"><span data-stu-id="c5e99-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c5e99-113">В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла.</span><span class="sxs-lookup"><span data-stu-id="c5e99-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c5e99-114">В разделе **Visual C#** выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="c5e99-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c5e99-115">В списке шаблонов проектов выберите **приложениях ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="c5e99-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="c5e99-116">Введите имя для проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="c5e99-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="c5e99-117">Создание модели и контроллера</span><span class="sxs-lookup"><span data-stu-id="c5e99-117">Create the Model and Controller</span></span>

<span data-ttu-id="c5e99-118">В этом учебнике используются те же классы модели и контроллера, как [Приступая к работе](tutorial-your-first-web-api.md) учебника.</span><span class="sxs-lookup"><span data-stu-id="c5e99-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="c5e99-119">Сначала добавьте класс модели.</span><span class="sxs-lookup"><span data-stu-id="c5e99-119">First, add a model class.</span></span> <span data-ttu-id="c5e99-120">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **Добавление класса**.</span><span class="sxs-lookup"><span data-stu-id="c5e99-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="c5e99-121">Имя класса продуктов и добавьте реализацию следующего:</span><span class="sxs-lookup"><span data-stu-id="c5e99-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="c5e99-122">Добавьте контроллер веб-API в проект., А *контроллера* — это объект, который обрабатывает запросы HTTP для веб-API.</span><span class="sxs-lookup"><span data-stu-id="c5e99-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="c5e99-123">В **обозревателе решений**, щелкните правой кнопкой мыши проект.</span><span class="sxs-lookup"><span data-stu-id="c5e99-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="c5e99-124">Выберите **Добавление нового элемента**.</span><span class="sxs-lookup"><span data-stu-id="c5e99-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="c5e99-125">В разделе **установленные шаблоны**, разверните **Visual C#** и выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="c5e99-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="c5e99-126">В списке шаблонов выберите **класс контроллера Web API**.</span><span class="sxs-lookup"><span data-stu-id="c5e99-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="c5e99-127">Имя контроллера «ProductsController» и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="c5e99-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="c5e99-128">**Добавление нового элемента** мастер создаст файл с именем ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="c5e99-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="c5e99-129">Удалите методы, которые включены в мастере и добавьте следующие методы:</span><span class="sxs-lookup"><span data-stu-id="c5e99-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="c5e99-130">Дополнительные сведения о коде в этом контроллере см. в разделе [Приступая к работе](tutorial-your-first-web-api.md) учебника.</span><span class="sxs-lookup"><span data-stu-id="c5e99-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="c5e99-131">Добавить сведения о маршрутизации</span><span class="sxs-lookup"><span data-stu-id="c5e99-131">Add Routing Information</span></span>

<span data-ttu-id="c5e99-132">Далее мы добавим маршрута URI таким образом, идентификаторы URI формы &quot;/api/продукты/&quot; направляются к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="c5e99-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="c5e99-133">В **обозревателе решений**, дважды щелкните файл Global.asax для открытия файла кода Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="c5e99-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="c5e99-134">Добавьте следующие **с помощью** инструкции.</span><span class="sxs-lookup"><span data-stu-id="c5e99-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="c5e99-135">Затем добавьте следующий код в **приложения\_запустить** метод:</span><span class="sxs-lookup"><span data-stu-id="c5e99-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="c5e99-136">Дополнительные сведения о маршрутизации таблицах см. в разделе [маршрутизации в ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c5e99-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="c5e99-137">Добавить AJAX на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="c5e99-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="c5e99-138">Это все, что нужно для создания веб-API, в которой клиенты имеют доступ.</span><span class="sxs-lookup"><span data-stu-id="c5e99-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="c5e99-139">Теперь добавим HTML-страницу, которая используется для вызова API jQuery.</span><span class="sxs-lookup"><span data-stu-id="c5e99-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="c5e99-140">Убедитесь, что на главной странице (например, *Site.Master*) включает в себя `ContentPlaceHolder` с `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="c5e99-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="c5e99-141">Откройте файл Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="c5e99-141">Open the file Default.aspx.</span></span> <span data-ttu-id="c5e99-142">Замените стандартного текста, который находится в области основного содержимого, как показано.</span><span class="sxs-lookup"><span data-stu-id="c5e99-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="c5e99-143">Добавьте ссылку на исходный файл jQuery в `HeaderContent` раздела:</span><span class="sxs-lookup"><span data-stu-id="c5e99-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="c5e99-144">Примечание: Можно легко добавить ссылку на скрипт, перетащив его из **обозревателе решений** в окне редактора кода.</span><span class="sxs-lookup"><span data-stu-id="c5e99-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="c5e99-145">За тега скрипта jQuery добавьте следующий блок сценария:</span><span class="sxs-lookup"><span data-stu-id="c5e99-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="c5e99-146">При загрузке документа, этот сценарий делает запрос AJAX для &quot;api и продуктов&quot;.</span><span class="sxs-lookup"><span data-stu-id="c5e99-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="c5e99-147">Запрос возвращает список продуктов в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="c5e99-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="c5e99-148">Сценарий добавляет сведения о продукте в таблицу HTML.</span><span class="sxs-lookup"><span data-stu-id="c5e99-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="c5e99-149">При запуске приложения, он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c5e99-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
