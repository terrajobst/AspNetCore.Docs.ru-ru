---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Часть 1: Обзор и Создание проекта | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f0616383fce2e92f7d1a0b63bf840208f7327bf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394065"
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="0f971-102">Часть 1: Обзор и Создание проекта</span><span class="sxs-lookup"><span data-stu-id="0f971-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="0f971-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0f971-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0f971-104">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="0f971-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="0f971-105">Платформа Entity Framework — это с объектно реляционного сопоставления.</span><span class="sxs-lookup"><span data-stu-id="0f971-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="0f971-106">Оно сопоставляет объекты домена в коде с сущностями в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="0f971-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="0f971-107">По большей части у вас нет беспокоиться о на уровне базы данных, так как Entity Framework берет на себя его для вас.</span><span class="sxs-lookup"><span data-stu-id="0f971-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="0f971-108">Код обрабатывает объекты, а изменения сохраняются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0f971-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="0f971-109">О руководстве</span><span class="sxs-lookup"><span data-stu-id="0f971-109">About the Tutorial</span></span>

<span data-ttu-id="0f971-110">В этом руководстве вы создадите приложение простого хранилища.</span><span class="sxs-lookup"><span data-stu-id="0f971-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="0f971-111">Существует две основные части к приложению.</span><span class="sxs-lookup"><span data-stu-id="0f971-111">There are two main parts to the application.</span></span> <span data-ttu-id="0f971-112">Обычные пользователи могут просматривать продукты и размещают заказы:</span><span class="sxs-lookup"><span data-stu-id="0f971-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="0f971-113">Администраторы можно создать, удалить или изменить продуктов:</span><span class="sxs-lookup"><span data-stu-id="0f971-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="0f971-114">Навыки, которые вы узнаете</span><span class="sxs-lookup"><span data-stu-id="0f971-114">Skills You'll Learn</span></span>

<span data-ttu-id="0f971-115">Вот, вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="0f971-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="0f971-116">Сведения об использовании Entity Framework с помощью веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0f971-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="0f971-117">Как создать с помощью knockout.js динамического пользовательского интерфейса клиента.</span><span class="sxs-lookup"><span data-stu-id="0f971-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="0f971-118">Как использовать проверку подлинности форм с веб-API для проверки подлинности пользователей.</span><span class="sxs-lookup"><span data-stu-id="0f971-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="0f971-119">Несмотря на то, что это руководство представляет собой автономное, может потребоваться сначала прочитайте следующие руководства:</span><span class="sxs-lookup"><span data-stu-id="0f971-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="0f971-120">Ваш первый веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0f971-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="0f971-121">Создание веб-API, который поддерживает операции CRUD</span><span class="sxs-lookup"><span data-stu-id="0f971-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="0f971-122">Знание [ASP.NET MVC](../../../../mvc/index.md) это очень удобно.</span><span class="sxs-lookup"><span data-stu-id="0f971-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="0f971-123">Обзор</span><span class="sxs-lookup"><span data-stu-id="0f971-123">Overview</span></span>

<span data-ttu-id="0f971-124">На высоком уровне здесь — это архитектура приложения:</span><span class="sxs-lookup"><span data-stu-id="0f971-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="0f971-125">ASP.NET MVC создает HTML-страницы для клиента.</span><span class="sxs-lookup"><span data-stu-id="0f971-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="0f971-126">Веб-API ASP.NET предоставляет операции CRUD с данными (products и orders).</span><span class="sxs-lookup"><span data-stu-id="0f971-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="0f971-127">Платформа Entity Framework преобразует моделей C#, используемых веб-API в сущности базы данных.</span><span class="sxs-lookup"><span data-stu-id="0f971-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="0f971-128">На следующей схеме показано, как объекты домена представлены на различных уровнях приложения: на уровне базы данных, объектной модели и, наконец, формат подключения, которая используется для передачи данных клиента по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="0f971-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="0f971-129">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0f971-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="0f971-130">Можно создать проект tutorial, с помощью Visual Web Developer Express или полную версию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0f971-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="0f971-131">Из **запустить** щелкните **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="0f971-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="0f971-132">В **шаблоны** области выберите **установленные шаблоны** и разверните **Visual C#** узла.</span><span class="sxs-lookup"><span data-stu-id="0f971-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="0f971-133">В разделе **Visual C#** выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="0f971-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="0f971-134">В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="0f971-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="0f971-135">Присвойте проекту имя «ProductStore» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="0f971-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="0f971-136">В **создания проекта ASP.NET MVC 4** диалоговом окне выберите **веб-приложение** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="0f971-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="0f971-137">Шаблон «Веб-приложение» создает приложение ASP.NET MVC, который поддерживает проверку подлинности форм.</span><span class="sxs-lookup"><span data-stu-id="0f971-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="0f971-138">Если запустить приложение сейчас, он уже имеет некоторые функции:</span><span class="sxs-lookup"><span data-stu-id="0f971-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="0f971-139">Новые пользователи могут зарегистрировать, щелкнув ссылку «Зарегистрировать» в правом верхнем углу.</span><span class="sxs-lookup"><span data-stu-id="0f971-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="0f971-140">Зарегистрированные пользователи могут входить в, щелкнув ссылку «Вход».</span><span class="sxs-lookup"><span data-stu-id="0f971-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="0f971-141">Сведения о членстве сохраняется в базе данных, который создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="0f971-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="0f971-142">Дополнительные сведения о проверке подлинности форм в ASP.NET MVC см. в разделе [Пошаговое руководство: использование проверки подлинности форм в ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="0f971-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="0f971-143">Обновите файл CSS</span><span class="sxs-lookup"><span data-stu-id="0f971-143">Update the CSS File</span></span>

<span data-ttu-id="0f971-144">Этот шаг является декоративным, но это сделает визуализации как на предыдущих снимках экрана страницы.</span><span class="sxs-lookup"><span data-stu-id="0f971-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="0f971-145">В обозревателе решений разверните папку и откройте файл с именем Site.css.</span><span class="sxs-lookup"><span data-stu-id="0f971-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="0f971-146">Добавьте следующие стили CSS:</span><span class="sxs-lookup"><span data-stu-id="0f971-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="0f971-147">Вперед</span><span class="sxs-lookup"><span data-stu-id="0f971-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
