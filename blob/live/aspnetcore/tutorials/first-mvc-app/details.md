---
title: "Изучение методов Details и Delete"
author: rick-anderson
description: "Метод и представление контроллера Details в базовом приложении ASP.NET Core MVC."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 43394106c9074f9487e1065a37a88eb017833bae
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="ba376-104">Изучение методов Details и Delete</span><span class="sxs-lookup"><span data-stu-id="ba376-104">Examining the Details and Delete methods</span></span>

<span data-ttu-id="ba376-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ba376-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ba376-106">Откройте контроллер Movie и изучите метод `Details`:</span><span class="sxs-lookup"><span data-stu-id="ba376-106">Open the Movie controller and examine the `Details` method:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="ba376-107">Подсистема формирования шаблонов MVC, созданная этим методом действия, добавляет комментарий, показывающий HTTP-запрос, который вызывает метод.</span><span class="sxs-lookup"><span data-stu-id="ba376-107">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="ba376-108">Здесь это запрос GET, состоящий из трех сегментов URL-адреса, контроллера `Movies`, метода `Details` и значения `id`.</span><span class="sxs-lookup"><span data-stu-id="ba376-108">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="ba376-109">Эти сегменты определены в *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ba376-109">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="ba376-110">EF упрощает поиск данных с помощью метода `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="ba376-110">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="ba376-111">Важной функцией обеспечения безопасности, встроенной в метод, является то, что код проверяет, что метод поиска обнаружил фильм до выполнения с ним каких-либо действий.</span><span class="sxs-lookup"><span data-stu-id="ba376-111">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="ba376-112">Например, злоумышленник может внести ошибки на сайт путем изменения созданного ссылками URL-адреса с `http://localhost:xxxx/Movies/Details/1` на что-то вроде `http://localhost:xxxx/Movies/Details/12345` (или любое другое значение, которое не представляет фактический фильм).</span><span class="sxs-lookup"><span data-stu-id="ba376-112">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="ba376-113">Если вы не проверили наличие фильма со значением NULL, приложение вызовет исключение.</span><span class="sxs-lookup"><span data-stu-id="ba376-113">If you did not check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="ba376-114">Просмотрите методы `Delete` и `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="ba376-114">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

<span data-ttu-id="ba376-115">Обратите внимание, что метод `HTTP GET Delete` не удаляет указанный фильм, он возвращает представление фильма, где можно выполнить (HttpPost) удаление.</span><span class="sxs-lookup"><span data-stu-id="ba376-115">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="ba376-116">Выполнение операции удаления в ответ на запрос GET (или выполнение операции редактирования, создания или любой другой операции, изменяющей данные) открывает брешь в системе безопасности.</span><span class="sxs-lookup"><span data-stu-id="ba376-116">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="ba376-117">Метод `[HttpPost]`, который удаляет данные, называется `DeleteConfirmed`, поэтому метод HTTP POST обладает уникальной сигнатурой или именем.</span><span class="sxs-lookup"><span data-stu-id="ba376-117">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="ba376-118">Ниже приведены сигнатуры двух методов:</span><span class="sxs-lookup"><span data-stu-id="ba376-118">The two method signatures are shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="ba376-119">Требуется, чтобы в среде CLR перегруженные методы имели уникальную сигнатуру параметров (то же имя метода, но другой список параметров).</span><span class="sxs-lookup"><span data-stu-id="ba376-119">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="ba376-120">Однако здесь необходимы два метода `Delete` — один для GET и один для POST — с одинаковой сигнатурой параметров.</span><span class="sxs-lookup"><span data-stu-id="ba376-120">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="ba376-121">(Они оба должны принимать целочисленное значение в качестве параметра.)</span><span class="sxs-lookup"><span data-stu-id="ba376-121">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="ba376-122">Существует два подхода к решению этой проблемы, один из которых заключается в указании разных имен для методов.</span><span class="sxs-lookup"><span data-stu-id="ba376-122">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="ba376-123">Именно это было представлено в предыдущем примере механизма формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="ba376-123">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="ba376-124">Однако в этом случае возникает небольшая проблема: ASP.NET сопоставляет сегменты URL-адреса с методами действий по имени, а при переименовании метода, как правило, маршрутизация не сможет найти этот метод.</span><span class="sxs-lookup"><span data-stu-id="ba376-124">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="ba376-125">Решение показано в примере, а именно: в метод `DeleteConfirmed` следует добавить атрибут `ActionName("Delete")`.</span><span class="sxs-lookup"><span data-stu-id="ba376-125">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="ba376-126">Этот атрибут выполняет сопоставление для системы маршрутизации, чтобы URL-адрес, включающий /Delete/ для запроса POST, смог найти метод `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="ba376-126">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="ba376-127">Другим распространенным решением проблемы для методов с одинаковыми именами и сигнатурами является искусственное изменение сигнатуры метода POST для включения дополнительного (неиспользуемого) параметра.</span><span class="sxs-lookup"><span data-stu-id="ba376-127">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="ba376-128">Именно это было сделано ранее, когда был добавлен параметр `notUsed`.</span><span class="sxs-lookup"><span data-stu-id="ba376-128">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="ba376-129">То же самое можно сделать для метода `[HttpPost] Delete`:</span><span class="sxs-lookup"><span data-stu-id="ba376-129">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="ba376-130">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="ba376-130">Publish to Azure</span></span>

<span data-ttu-id="ba376-131">Дополнительные сведения о публикации этого приложения в Azure с помощью Visual Studio см. в руководстве по [публикации веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="ba376-131">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="ba376-132">Приложение можно также опубликовать из [командной строки](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="ba376-132">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="ba376-133">Благодарим вас за изучение общих сведений по ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ba376-133">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="ba376-134">Мы будем рады любым комментариям.</span><span class="sxs-lookup"><span data-stu-id="ba376-134">We appreciate any comments you leave.</span></span> <span data-ttu-id="ba376-135">Отличным дополнением к этому учебнику является статья [Начало работы с EF и MVC](xref:data/ef-mvc/intro).</span><span class="sxs-lookup"><span data-stu-id="ba376-135">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ba376-136">Назад</span><span class="sxs-lookup"><span data-stu-id="ba376-136">Previous</span></span>](validation.md)
