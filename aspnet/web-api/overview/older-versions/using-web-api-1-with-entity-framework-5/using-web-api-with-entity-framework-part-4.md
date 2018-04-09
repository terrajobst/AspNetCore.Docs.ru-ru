---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Часть 4: Добавление представления Admin | Документы Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="911cb-102">Часть 4: Добавление представления администрирования</span><span class="sxs-lookup"><span data-stu-id="911cb-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="911cb-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="911cb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="911cb-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="911cb-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="911cb-105">Добавление представления администрирования</span><span class="sxs-lookup"><span data-stu-id="911cb-105">Add an Admin View</span></span>

<span data-ttu-id="911cb-106">Теперь мы будем включения на стороне клиента и добавление страницы, можно использовать данные из контроллера администратора.</span><span class="sxs-lookup"><span data-stu-id="911cb-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="911cb-107">Страница позволит создать, изменить или удалить продукты, отправляя запросы AJAX к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="911cb-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="911cb-108">В обозревателе решений разверните папку Controllers и откройте файл с именем HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="911cb-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="911cb-109">Этот файл содержит контроллер MVC.</span><span class="sxs-lookup"><span data-stu-id="911cb-109">This file contains an MVC controller.</span></span> <span data-ttu-id="911cb-110">Добавьте метод с именем `Admin`:</span><span class="sxs-lookup"><span data-stu-id="911cb-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="911cb-111">**HttpRouteUrl** метод создает URI веб-API, и мы сохраните его в пакет представления для использования в дальнейшем.</span><span class="sxs-lookup"><span data-stu-id="911cb-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="911cb-112">Затем поместите курсор текста внутри `Admin` метод действия, а затем щелкните правой кнопкой мыши и выберите **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="911cb-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="911cb-113">Данная команда откроет **добавить представление** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="911cb-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="911cb-114">В **добавить представление** диалога, имя представления «Admin».</span><span class="sxs-lookup"><span data-stu-id="911cb-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="911cb-115">Установите флажок **создать строго типизированное представление**.</span><span class="sxs-lookup"><span data-stu-id="911cb-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="911cb-116">В разделе **класс модели**, выберите «Product (ProductStore.Models)».</span><span class="sxs-lookup"><span data-stu-id="911cb-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="911cb-117">Оставьте все параметры как значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="911cb-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="911cb-118">Щелкнув **добавить** добавляет в файл с именем Admin.cshtml в области представления/домашние.</span><span class="sxs-lookup"><span data-stu-id="911cb-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="911cb-119">Откройте этот файл и добавьте следующий код HTML.</span><span class="sxs-lookup"><span data-stu-id="911cb-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="911cb-120">Следующий код HTML определяет структуру страницы, но еще подключено не добавляет новых функций.</span><span class="sxs-lookup"><span data-stu-id="911cb-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="911cb-121">Создать ссылку на страницу администрирования</span><span class="sxs-lookup"><span data-stu-id="911cb-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="911cb-122">В обозревателе решений разверните папку Views, а затем разверните папку Shared.</span><span class="sxs-lookup"><span data-stu-id="911cb-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="911cb-123">Откройте файл с именем \_Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="911cb-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="911cb-124">Найдите **ul** элемент с id = «menu» и ссылку действия для представления администрирования:</span><span class="sxs-lookup"><span data-stu-id="911cb-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="911cb-125">В образце проекта внесены несколько других финальных изменений, например замены строки «Ваша эмблема здесь».</span><span class="sxs-lookup"><span data-stu-id="911cb-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="911cb-126">Они не влияют на функциональные возможности приложения.</span><span class="sxs-lookup"><span data-stu-id="911cb-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="911cb-127">Можно загрузить проект и сравните файлы.</span><span class="sxs-lookup"><span data-stu-id="911cb-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="911cb-128">Запустите приложение и щелкните ссылку «Admin», которая появляется в верхней части домашней страницы.</span><span class="sxs-lookup"><span data-stu-id="911cb-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="911cb-129">Страницы администрирования должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="911cb-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="911cb-130">Право, страницы не выполняет никаких действий.</span><span class="sxs-lookup"><span data-stu-id="911cb-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="911cb-131">В следующем разделе мы будем использовать Knockout.js для создания динамического пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="911cb-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="911cb-132">Добавьте авторизацию</span><span class="sxs-lookup"><span data-stu-id="911cb-132">Add Authorization</span></span>

<span data-ttu-id="911cb-133">Страницы администрирования в настоящее время доступен любому пользователю получить на узле.</span><span class="sxs-lookup"><span data-stu-id="911cb-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="911cb-134">Позволяет изменить этот параметр, чтобы ограничить разрешения для администраторов.</span><span class="sxs-lookup"><span data-stu-id="911cb-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="911cb-135">Начните с добавления роли «Администратор» и пользователя-администратора.</span><span class="sxs-lookup"><span data-stu-id="911cb-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="911cb-136">В обозревателе решений разверните папку «фильтры» и откройте файл с именем InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="911cb-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="911cb-137">Найдите `SimpleMembershipInitializer` конструктор.</span><span class="sxs-lookup"><span data-stu-id="911cb-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="911cb-138">После вызова **WebSecurity.InitializeDatabaseConnection**, добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="911cb-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="911cb-139">Это скорую руку способ добавления роли «Администратор» и создайте пользователя для роли.</span><span class="sxs-lookup"><span data-stu-id="911cb-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="911cb-140">В обозревателе решений разверните папку Controllers, а затем откройте файл HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="911cb-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="911cb-141">Добавить **авторизовать** атрибут `Admin` метод.</span><span class="sxs-lookup"><span data-stu-id="911cb-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="911cb-142">Откройте файл AdminController.cs и добавьте **авторизовать** атрибут ко всему `AdminController` класса.</span><span class="sxs-lookup"><span data-stu-id="911cb-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="911cb-143">MVC и веб-API, как определить **авторизовать** атрибуты в разных пространствах имен.</span><span class="sxs-lookup"><span data-stu-id="911cb-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="911cb-144">MVC использует **System.Web.Mvc.AuthorizeAttribute**, а веб-API использует **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="911cb-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="911cb-145">Теперь только администраторы могут просматривать страницы администрирования.</span><span class="sxs-lookup"><span data-stu-id="911cb-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="911cb-146">Кроме того при отправке запроса HTTP к контроллеру Admin, запрос должен содержать файл cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="911cb-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="911cb-147">В противном случае сервер отправляет ответ HTTP 401 (не санкционировано).</span><span class="sxs-lookup"><span data-stu-id="911cb-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="911cb-148">Вы увидите это на Fiddler, отправив запрос GET к `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="911cb-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="911cb-149">[Назад](using-web-api-with-entity-framework-part-3.md)
> [Вперед](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="911cb-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
