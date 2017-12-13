---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: "Часть 1: Обзор и Создание проекта | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: d76efa2e95c95c91045c7f631040dfff3d4afd2c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="4221d-102">Часть 1: Обзор и Создание проекта</span><span class="sxs-lookup"><span data-stu-id="4221d-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="4221d-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4221d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4221d-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="4221d-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="4221d-105">Entity Framework является платформу объектно реляционного сопоставления.</span><span class="sxs-lookup"><span data-stu-id="4221d-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="4221d-106">Объекты домена в коде он преобразуется в сущности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="4221d-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="4221d-107">В большинстве случаев у вас беспокоиться о на уровне базы данных, так как он обеспечивает Entity Framework для вас.</span><span class="sxs-lookup"><span data-stu-id="4221d-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="4221d-108">Код выполняет действия над объектами, а изменения сохраняются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="4221d-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="4221d-109">О учебника</span><span class="sxs-lookup"><span data-stu-id="4221d-109">About the Tutorial</span></span>

<span data-ttu-id="4221d-110">В этом учебнике вы создадите приложение простой магазина.</span><span class="sxs-lookup"><span data-stu-id="4221d-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="4221d-111">Существует две основные части в приложение.</span><span class="sxs-lookup"><span data-stu-id="4221d-111">There are two main parts to the application.</span></span> <span data-ttu-id="4221d-112">Обычные пользователи могут просматривать продукты и создания заказов:</span><span class="sxs-lookup"><span data-stu-id="4221d-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="4221d-113">Администраторы могут создавать, удалять или изменять продуктов:</span><span class="sxs-lookup"><span data-stu-id="4221d-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="4221d-114">Навыки, которые вы узнаете</span><span class="sxs-lookup"><span data-stu-id="4221d-114">Skills You'll Learn</span></span>

<span data-ttu-id="4221d-115">Ниже приведен изучаемого материала.</span><span class="sxs-lookup"><span data-stu-id="4221d-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="4221d-116">Инструкции по использованию платформы Entity Framework с веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4221d-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="4221d-117">Инструкции по использованию knockout.js для создания динамического пользовательского интерфейса клиента.</span><span class="sxs-lookup"><span data-stu-id="4221d-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="4221d-118">Инструкции по использованию проверки подлинности форм с веб-API для проверки подлинности пользователей.</span><span class="sxs-lookup"><span data-stu-id="4221d-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="4221d-119">Несмотря на то, что этот учебник является автономной, может потребоваться сначала прочитать следующие учебники:</span><span class="sxs-lookup"><span data-stu-id="4221d-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="4221d-120">Ваш первый веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4221d-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="4221d-121">Создание веб-API, который поддерживает операции CRUD</span><span class="sxs-lookup"><span data-stu-id="4221d-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="4221d-122">Знание [ASP.NET MVC](../../../../mvc/index.md) это очень удобно.</span><span class="sxs-lookup"><span data-stu-id="4221d-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="4221d-123">Обзор</span><span class="sxs-lookup"><span data-stu-id="4221d-123">Overview</span></span>

<span data-ttu-id="4221d-124">На высоком уровне Вот архитектуры приложения.</span><span class="sxs-lookup"><span data-stu-id="4221d-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="4221d-125">ASP.NET MVC создает HTML-страниц для клиента.</span><span class="sxs-lookup"><span data-stu-id="4221d-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="4221d-126">Веб-API ASP.NET предоставляет операции CRUD с данными (products и orders).</span><span class="sxs-lookup"><span data-stu-id="4221d-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="4221d-127">Entity Framework преобразует моделей C#, используемых веб-API в сущности базы данных.</span><span class="sxs-lookup"><span data-stu-id="4221d-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="4221d-128">На следующей схеме показано, как объекты домена представлены на различных уровнях приложения: на уровне базы данных, объектную модель и наконец формата подключения, которая используется для передачи данных клиента по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="4221d-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="4221d-129">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4221d-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="4221d-130">Можно создать проект tutorial, с помощью Visual Web Developer, экспресс-выпуск или полную версию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4221d-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="4221d-131">Из **запустить** щелкните **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="4221d-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="4221d-132">В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла.</span><span class="sxs-lookup"><span data-stu-id="4221d-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="4221d-133">В разделе **Visual C#**выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="4221d-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="4221d-134">В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="4221d-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="4221d-135">Назовите проект «ProductStore» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="4221d-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="4221d-136">В **нового проекта ASP.NET MVC 4** диалогового окна выберите **веб-приложение** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="4221d-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="4221d-137">Шаблон «Интернет-приложения» создает приложения ASP.NET MVC, который поддерживает проверку подлинности форм.</span><span class="sxs-lookup"><span data-stu-id="4221d-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="4221d-138">Если запустить приложение, уже имеет некоторые функции:</span><span class="sxs-lookup"><span data-stu-id="4221d-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="4221d-139">Новые пользователи могут зарегистрировать, щелкнув ссылку «Регистрация» в правом верхнем углу.</span><span class="sxs-lookup"><span data-stu-id="4221d-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="4221d-140">Зарегистрированные пользователи выполняют вход, щелкнув ссылку «Вход».</span><span class="sxs-lookup"><span data-stu-id="4221d-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="4221d-141">Сведения о членстве сохраняется в базе данных, который создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="4221d-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="4221d-142">Дополнительные сведения о проверке подлинности форм в ASP.NET MVC см. в разделе [Пошаговое руководство: использование проверки подлинности форм в ASP.NET MVC](https://msdn.microsoft.com/en-us/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="4221d-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/en-us/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="4221d-143">Обновления в CSS-файл</span><span class="sxs-lookup"><span data-stu-id="4221d-143">Update the CSS File</span></span>

<span data-ttu-id="4221d-144">Этот шаг является формальной, но это сделает визуализации, как и на предыдущих снимках экрана страницы.</span><span class="sxs-lookup"><span data-stu-id="4221d-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="4221d-145">В обозревателе решений разверните папку содержимого и откройте файл с именем Site.css.</span><span class="sxs-lookup"><span data-stu-id="4221d-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="4221d-146">Добавьте следующие стили CSS.</span><span class="sxs-lookup"><span data-stu-id="4221d-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

>[!div class="step-by-step"]
[<span data-ttu-id="4221d-147">Вперед</span><span class="sxs-lookup"><span data-stu-id="4221d-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
